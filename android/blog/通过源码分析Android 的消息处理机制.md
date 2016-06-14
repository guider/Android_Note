#ͨ��Դ�����Android ����Ϣ�������

����֪����AndroidӦ����ͨ����Ϣ�������ģ�ÿһ�����̱�fork֮�󣬶����ڸý��̵�UI�̣߳����̣߳�������һ����Ϣ���У����̻߳Ὺ��һ����ѭ������ѵ������У������������Ϣ��

ͨ��Android���̵���� ActivityThread#main �������Կ�������߼���

```java

    public static void main(String[] args) {

        Looper.prepareMainLooper();

        ActivityThread thread = new ActivityThread();
        thread.attach(false);

        Looper.loop();
	
	//ֻҪ����������������ִ�е���δ���
        throw new RuntimeException("Main thread loop unexpectedly exited");
    }

```

���⣬������ƽʱ�����У�����������߳��и���UI�������ֶ�����һ��Looper���󣬲��ҵ�������loop������

```java
class TestThread extends Thread {
       public Handler mHandler;
	
        public void run() {

	    // Ϊ��ǰ�̴߳���Looper����ͬʱ��Ϣ����MessageQueueҲ׼������
            Looper.prepare();
  
            mHandler = new Handler() {
                public void handleMessage(Message msg) {
                    
		    //������Ը���UI��

                }
            }; 
  
	    // ��ʼ������Ϣ�����е���Ϣ
            Looper.loop();
        }
}
```

�������������У���������Looper����Ӱ����ΪLooper������Ϣ���еĹ����ߣ����ǵ�������prepareMainLooper��prepare�������ͻ�Ϊ��ǰ�̴߳�������Ϣ���У�����looper�����ͻῪʼ��Ϣ���е���ѯ������̡�

Ҫ�����Ϣ������ƣ�����Looper�����ǻ�Ҫ��������ࣺ

> * MessageQueue����Ϣ����
> * Message����Ϣ
> * Handler����Ϣ�Ĵ�����

## UML��ͼ
![�˴�����ͼƬ������][1]

## ��������

### Looper

�ȴ�Looper��ʼ�������ᵽ��������̬���� prepareMainLooper()��prepare()�����þ���Ϊ��ǰ�̴߳�����Ϣ���У�loop()���������Ϣ���С�

����Looper#prepareMainLooper��

```java
 public static void prepareMainLooper() {
        prepare(false);
        synchronized (Looper.class) {
	    
            if (sMainLooper != null) {
                throw new IllegalStateException("The main Looper has already been prepared.");
            }
            sMainLooper = myLooper();
        }
    }
```
prepareMainLooper��ͨ������prepare()����ʵ�֣�����ͨ��myLooper()�����õ��˵�ǰ�̵߳�Looper�����̵߳�Looper������sMainLooper��

���ſ�prepare������

```java
/**
*  @param uitAllowed �Ƿ�������Ϣѭ���˳�����ActivityThread#main �У����Ǵ����������̵߳���Ϣѭ�����϶��������˳��������ط���������˳�
*/
private static void prepare(boolean quitAllowed) {
        if (sThreadLocal.get() != null) {
            throw new RuntimeException("Only one Looper may be created per thread");
        }
        sThreadLocal.set(new Looper(quitAllowed));
    }
```
���������Ϊÿһ���߳�new��һ��Looper���󣬲������õ�sThreadLocal�С�sThreadLocal��ThreadLocal���͵ı������ֲ߳̾�������������֤��ÿһ���߳���һ���Լ����е�Looper����

myLooper()��
```java
/**
* ��sThreadLocal�л�ȡ��ǰ�̵߳�Looper����
*/
public static Looper myLooper() {
        return sThreadLocal.get();
}
```
�����ܼ򵥣����Ƿ����˵�ǰ�߳������е��Ǹ�Looper����

Looer���󴴽����ˣ�ͬʱ��Ϣ����Ҳ�������ˣ�������������������Ϣ�����������������Ҫͨ��Looper#loopʵ�֣�
```java
    public static void loop() {

	//�õ���ǰ�̵߳�Looper
        final Looper me = myLooper();

	//û��Looper����ֱ�����쳣
        if (me == null) {
            throw new RuntimeException("No Looper; Looper.prepare() wasn't called on this thread.");
        }

	//�õ���ǰLooper��Ӧ����Ϣ����
        final MessageQueue queue = me.mQueue;
	
	//һ����ѭ������ͣ�Ĵ�����Ϣ�����е���Ϣ����Ϣ�Ļ�ȡ��ͨ��MessageQueue��next()����ʵ��
        for (;;) {
            Message msg = queue.next(); // might block
            if (msg == null) {
                // No message indicates that the message queue is quitting.
                return;
            }
		
	    //����Message��target������Ҳ����Handler�ˣ���dispatchMessage������������Ϣ
            msg.target.dispatchMessage(msg);

            msg.recycleUnchecked();
        }
    }
```
���ȣ��õ���ǰ�̵߳�Looper����ͨ��Looper�õ���ǰ�̵߳���Ϣѭ����Ȼ����ѯ�����Ϣ���У�����ÿһ����Ϣ�������Ҿ�������Ϣ����MessageQueue�Լ���ϢMessage��

### MessageQueue
MessageQueue����Message���У���Ϣ������һ���ݽṹ��ͨ��һ��Message��ʵ�ֵģ�Message������һ��next�ֶ�ָ��������һ��㡣

```java
public final class MessageQueue {
	Message mMessages;

	//��Ϣ���еĳ�ʼ�������٣���ѯ���̣����������ѣ�����ͨ�����ط���ʵ�ֵ�
	private native static long nativeInit();
	private native static void nativeDestroy(long ptr);
	private native static void nativePollOnce(long ptr, int timeoutMillis);
	private native static void nativeWake(long ptr);
	private native static boolean nativeIsIdling(long ptr);


	/**
	*  ����Ϣ���뵽��Ϣѭ����
        *  @param msg 
	*  @param when ��ôʱ��ִ�У�����Ϣ�����лᰴ��ʱ������
 	*/
	boolean enqueueMessage(Message msg, long when) {}

	/**
	*  ����Ϣ���뵽��Ϣѭ����
        *  @param msg 
	*  @param when ʲôʱ��ִ��
 	*/
	boolean enqueueMessage(Message msg, long when) {}
	
        /**�Ƴ���Ϣ*/
	void removeMessages(Handler h, int what, Object object) {
}
```

Message����������ͨ��enqueueMessage()����ʵ�֣���Ϣ�����λ�ȡ��ͨ��next()����ʵ�֣����е���ȡ�����ɴ���Message����ʱ���÷��������ȴ�״̬��

### Message

Message������Ϣ�ķ�װ������ʵ����Parcelable�ӿڣ���˿��Կ���̴��䡣

```java
public final class Message implements Parcelable {
	
	//��Ϣ�ı�ʶ��
	public int what;

	//����int�ε���չ����
	public int arg1; 
	public int arg2;

	//һ��Object��չ����
	public Object obj;

	//��Ϣ�����ߵ�UID
	public int sendingUid = -1;
	
	//��Ϣ����data
	Bundle data;

	//target����Ҳ���Ǵ���ǰ��Ϣ��Handler
	Handler target;

	//ָ����Ϣ��������һ����Ϣ
	Message next;
	
	//��Ϣͬ����
	private static final Object sPoolSync = new Object();

	//��Ϣ��
	private static Message sPool;

	//��Ϣ�ش�С
	private static int sPoolSize = 0;

	//��Ϣ�������Ϣ����
	private static final int MAX_POOL_SIZE = 50;
}

```
����Ϣ�Ĺ�����ͨ��һ����Ϣ����ʵ�ֵģ���Ϊ��Ϣ�صĴ��ڣ���������Ҫ����һ��Message����ʱ�����ʹ�ÿɸ�����Ϣ����ķ�������������

��ȷ��ʹ�÷����ǣ�

```java
Message msg = mHandler.obtainMessage(WHAT);
msg.sendToTarget();
```
obtainMessage������ʵ���ϻ����Message#obtain����������Ϣ����ȡһ������

```java
public static Message obtain() {
        synchronized (sPoolSync) {
            if (sPool != null) {
                Message m = sPool;
                sPool = m.next;
                m.next = null;
                m.flags = 0; // clear in-use flag
                sPoolSize--;
                return m;
            }
        }
        return new Message();
    }
```

�����ʹ�÷�����

```java
Message msg = new Message();
msg.what = WHAT;
mHandler.sendMessage(msg);
```
### Handler

Handler ������Ϣѭ����MessageQueue������Ϣ��Message�������������ߣ�ͨ��Looper#loop���Կ�����

```java
public static void loop() {

        for (;;) {
            Message msg = queue.next(); // might block

            msg.target.dispatchMessage(msg);
        }
    }
```
Message��target���Ծ���һ��Handler���󣬶���Ϣ�Ĵ���ʵ������ͨ��Handler#dispatchMessage����������ģ���dispatchMessag���������Handler#handleMessage���������ǿ�����дhandleMessage����
�������Ĵ�����Ϣ�Ļص���

  [1]: http://7xr1sh.dl1.z0.glb.clouddn.com/304B.tmp.png