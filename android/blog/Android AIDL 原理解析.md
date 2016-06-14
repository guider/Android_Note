# Android AIDL ԭ�����

���ȥ�Ķ�Android��Դ���룬�ͻᷢ����������õ���Binder��AIDL���֪ʶ�����統����ȥʹ��```AMS```��```PMS```��```WMS```��Щ���ķ�����Ϊ���Ƕ������� ```system_server``` ���̣���ͨӦ������������ṩ�ķ������磺```startActivity()```������Ҫ```AMS```��ʵ�֣����ͱ���Ҫ����̵��ã���ˣ��������Ķ�����֮ǰ��������ȥ�������Binder��AIDL���֪ʶ��

ΪʲôҪʹ��AIDL�أ�

ͨ��AIDL�������ñ��ص���Զ�̷������Ľӿھ�����ñ��ؽӿ���ô�򵥣����û������ע�ڲ�ϸ�ڣ�ֻ��Ҫʵ���Լ���ҵ���߼��ӿڣ��ڲ����ӵĲ������л����͡����ա��ͻ��˵��÷���˵��߼����㶼����Ҫȥ�����ˡ�

## һ ��һ�����ӿ�ʼ

����ͨ��һ���򵥵Ŀ���̵��õ����������AIDL��

���һ���򵥵ĳ�����

> ������һ��```CoreService```������```"com.cundong.practice:core"```�����У��ṩ�ļ����ط������ǻ���```"com.cundong.practice"```������ȥbind���Service��������Ϊ�����ṩ�ķ���

ʵ�������ܼ򵥣�ֻ��Ҫ���¼������裺

 - ����һ��AIDL�ļ�
```java
 interface IDownloadService {
 
    /**
     * ����
     * @param url
     */
     void download(String url);

     /**
     * ɾ����������
     * @param url
     */
     void delete(String url);

     /**
     * ֹͣ��������
     * @param url
     */
     void stop(String url);

     /**
     * ��ȡ���ض��д�С
     * @return
     */
     int getQueueSize();
}
```

- ͨ��AIDL�Ĵ���������������ʵ��Proxy��Stub�����߼�
��Android StudioΪ�����Զ����ɵ�IDownloadService.javaλ��\app\build\generated\source\aidl\debugĿ¼�£����ķ����������ע�͡�

```java
public interface IDownloadService extends android.os.IInterface {
    /**
     * ����
     *
     * @param url
     */
    public void download(java.lang.String url) throws android.os.RemoteException;

    /**
     * ɾ����������
     *
     * @param url
     */
    public void delete(java.lang.String url) throws android.os.RemoteException;

    /**
     * ֹͣ��������
     *
     * @param url
     */
    public void stop(java.lang.String url) throws android.os.RemoteException;

    /**
     * ��ȡ���ض��д�С
     *
     * @return
     */
    public int getQueueSize() throws android.os.RemoteException;

    /**
     * Local-side IPC implementation stub class.
     */
    public static abstract class Stub extends android.os.Binder implements com.cundong.touch.IDownloadService {
    
        // ע�⣬ͨ�����´������Ƿ��֣�������.aidl�ж���ķ���������ʵûʲô���壬�������˸���û��ʹ         // ����Щ���֣������Զ�Ϊ��Щ���������˵�����ID�����ݷ�����˳�������������һ��һ������
        static final int TRANSACTION_download = (android.os.IBinder.FIRST_CALL_TRANSACTION + 0);
        static final int TRANSACTION_delete = (android.os.IBinder.FIRST_CALL_TRANSACTION + 1);
        static final int TRANSACTION_stop = (android.os.IBinder.FIRST_CALL_TRANSACTION + 2);
        static final int TRANSACTION_getQueueSize = (android.os.IBinder.FIRST_CALL_TRANSACTION + 3);
        private static final java.lang.String DESCRIPTOR = "com.cundong.touch.IDownloadService";

        /**
         * Construct the stub at attach it to the interface.
         */
        public Stub() {
            this.attachInterface(this, DESCRIPTOR);
        }

        /**
         * Cast an IBinder object into an com.cundong.touch.IDownloadService interface,
         * generating a proxy if needed.
         * 
         * ע��
         * ������������и�����ͨ��queryLocalInterface��ѯ���������˺Ϳͻ��˶�����ͬһ�����̣���          * ô�Ͳ���Ҫ������ˣ�ֱ�ӽ�IDownloadService������ͨ�Ķ�����ʹ�ü��ɣ����򷵻�Զ�̶���
         * �Ĵ���
         */
        public static com.cundong.touch.IDownloadService asInterface(android.os.IBinder obj) {
            if ((obj == null)) {
                return null;
            }
            android.os.IInterface iin = obj.queryLocalInterface(DESCRIPTOR);
            if (((iin != null) && (iin instanceof com.cundong.touch.IDownloadService))) {
                return ((com.cundong.touch.IDownloadService) iin);
            }
            return new com.cundong.touch.IDownloadService.Stub.Proxy(obj);
        }
        
        /**
        * ����Binderʵ��
        */
        @Override
        public android.os.IBinder asBinder() {
            return this;
        }

        /**
        * ����code��������������������������ҵ��ʵ����
        */
        @Override
        public boolean onTransact(int code, android.os.Parcel data, android.os.Parcel reply, int flags) throws android.os.RemoteException {
            switch (code) {
                case INTERFACE_TRANSACTION: {
                    reply.writeString(DESCRIPTOR);
                    return true;
                }
                case TRANSACTION_download: {
                    data.enforceInterface(DESCRIPTOR);
                    java.lang.String _arg0;
                    _arg0 = data.readString();
                    this.download(_arg0);
                    reply.writeNoException();
                    return true;
                }
                case TRANSACTION_delete: {
                    data.enforceInterface(DESCRIPTOR);
                    java.lang.String _arg0;
                    _arg0 = data.readString();
                    this.delete(_arg0);
                    reply.writeNoException();
                    return true;
                }
                case TRANSACTION_stop: {
                    data.enforceInterface(DESCRIPTOR);
                    java.lang.String _arg0;
                    _arg0 = data.readString();
                    this.stop(_arg0);
                    reply.writeNoException();
                    return true;
                }
                case TRANSACTION_getQueueSize: {
                    data.enforceInterface(DESCRIPTOR);
                    int _result = this.getQueueSize();
                    reply.writeNoException();
                    reply.writeInt(_result);
                    return true;
                }
            }
            return super.onTransact(code, data, reply, flags);
        }

        private static class Proxy implements com.cundong.touch.IDownloadService {
            private android.os.IBinder mRemote;
            
            Proxy(android.os.IBinder remote) {
                mRemote = remote;
            }
            
            //Proxy��asBinder()����λ�ڱ��ؽӿڵ�Զ�̴���
            @Override
            public android.os.IBinder asBinder() {
                return mRemote;
            }
            
            public java.lang.String getInterfaceDescriptor() {
                return DESCRIPTOR;
            }

            /**
             * ����
             *
             * @param url
             */
            @Override
            public void download(java.lang.String url) throws android.os.RemoteException {
                android.os.Parcel _data = android.os.Parcel.obtain();
                android.os.Parcel _reply = android.os.Parcel.obtain();
                try {
                    _data.writeInterfaceToken(DESCRIPTOR);
                    _data.writeString(url);
                    
                    //Proxy�У� ҵ��ӿ�����ʵû�����κ�ҵ���߼������������ռ����ز��������л�
                    //��ͨ��IBinder��transact�����������������ˣ�����ͨ��_reply�����շ�������
                    //�Ĵ�����
                    mRemote.transact(Stub.TRANSACTION_download, _data, _reply, 0);
                    _reply.readException();
                } finally {
                    _reply.recycle();
                    _data.recycle();
                }
            }

            /**
             * ɾ����������
             *
             * @param url
             */
            @Override
            public void delete(java.lang.String url) throws android.os.RemoteException {
                android.os.Parcel _data = android.os.Parcel.obtain();
                android.os.Parcel _reply = android.os.Parcel.obtain();
                try {
                    _data.writeInterfaceToken(DESCRIPTOR);
                    _data.writeString(url);
                    mRemote.transact(Stub.TRANSACTION_delete, _data, _reply, 0);
                    _reply.readException();
                } finally {
                    _reply.recycle();
                    _data.recycle();
                }
            }

            /**
             * ֹͣ��������
             *
             * @param url
             */
            @Override
            public void stop(java.lang.String url) throws android.os.RemoteException {
                android.os.Parcel _data = android.os.Parcel.obtain();
                android.os.Parcel _reply = android.os.Parcel.obtain();
                try {
                    _data.writeInterfaceToken(DESCRIPTOR);
                    _data.writeString(url);
                    mRemote.transact(Stub.TRANSACTION_stop, _data, _reply, 0);
                    _reply.readException();
                } finally {
                    _reply.recycle();
                    _data.recycle();
                }
            }

            /**
             * ��ȡ���ض��д�С
             *
             * @return
             */
            @Override
            public int getQueueSize() throws android.os.RemoteException {
                android.os.Parcel _data = android.os.Parcel.obtain();
                android.os.Parcel _reply = android.os.Parcel.obtain();
                int _result;
                try {
                    _data.writeInterfaceToken(DESCRIPTOR);
                    mRemote.transact(Stub.TRANSACTION_getQueueSize, _data, _reply, 0);
                    _reply.readException();
                    _result = _reply.readInt();
                } finally {
                    _reply.recycle();
                    _data.recycle();
                }
                return _result;
            }
        }
    }
}

```
- �̳�```IDownloadService.Stub```��ʵ�������ķ������˽ӿ�

```java
public class IDowanloadServiceImpl extends IDownloadService.Stub {
    private final Random mGenerator = new Random();

    @Override
    public void download(String url) throws RemoteException {
        //TODO ������ҵ���߼�
    }

    @Override
    public void delete(String url) throws RemoteException {
        //TODO ������ҵ���߼�
    }

    @Override
    public void stop(String url) throws RemoteException {
        //TODO ������ҵ���߼�
    }

    @Override
    public int getQueueSize() throws RemoteException {
        //TODO ������ҵ���߼�
        return mGenerator.nextInt(100);
    }
}
```
- Զ��Service��onBind()�ӿڷ���Stub��IBinder
```java
public class CoreService extends Service {

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return new IDowanloadServiceImpl();
    }
}
```

- �ͻ��˷���Զ�̵���
```java
private ServiceConnection mConnection = new ServiceConnection() {

        @Override
        public void onServiceConnected(ComponentName className, IBinder service) {

            sIDownloadService = IDownloadService.Stub.asInterface(service);
            mBound = true;
        }

        @Override
        public void onServiceDisconnected(ComponentName arg0) {
            mBound = false;
        }
    };
 ```
 
 ```java
 //����
 if (mBound) {
         int size = 0;
         try {
            size = sIDownloadService.getQueueSize();
         } catch (RemoteException e) {
             e.printStackTrace();
         }
  }
 ```
 
## �� ԭ�����

ͨ���Ķ��Զ����ɵ�IDownloadService.java�����Կ���������һ������Ĵ���ģʽ�ܹ���AIDL�Ĵ������������Ѿ�����.aidl�ļ��Զ�����������Proxy��Stub�������ࣩ�����࣬���Ұѿͻ��˴���mRemote��transact()�����Լ�
�������˵�onTtransact()����Ĭ��ʵ�ֺ��ˣ�����ֻ��Ҫ�ڷ������˼̳�Stub��ʵ���Լ���ҵ���ࣨ��onTtransact()�л���ã���

### 1. UMLͼ
![IDownloadService.java UMLͼ][4]

### 2. �������

������Ҫ��ΪProxy��Stub�����֡�

#### 1) Proxy

Proxy�����ڿͻ��ˣ���ʵ����IDownloadService�ӿڣ����ҳ���һ��Զ�̴���IBinder mRemote��mRemote�����κ�ҵ���߼���������ͨ��IBinder�ӿڵ�transact()�������ѿͻ��˵ĵ��ò������л���transact��Զ�̷�������

ʾ����
```java
@Override
public void download(java.lang.String url) throws android.os.RemoteException {
        android.os.Parcel _data = android.os.Parcel.obtain();
        android.os.Parcel _reply = android.os.Parcel.obtain();
            try {
                _data.writeInterfaceToken(DESCRIPTOR);
                _data.writeString(url);
                mRemote.transact(Stub.TRANSACTION_download, _data, _reply, 0);
                _reply.readException();
            } finally {
                _reply.recycle();
                _data.recycle();
            }
}         
```
_data�����ýӿڴ���Ĳ�����_replyΪ���÷����õ��ķ���ֵ��```mRemote.transact(Stub.TRANSACTION_download, _data, _reply, 0);```Ϊ���ù��̡�

#### 2) Stub

Stub�����ڷ������ˣ��̳���Binder��ͬ��Ҳʵ����IDownloadService�ӿڣ����ĺ����߼���onTransact�����У�

```java
@Override
public boolean onTransact(int code, Parcel data, Parcel reply, int flags) throws RemoteException {
    switch (code) {
        case TRANSACTION_download: {
                data.enforceInterface(DESCRIPTOR);
                java.lang.String _arg0;
                _arg0 = data.readString();
                this.download(_arg0);
                reply.writeNoException();
                return true;
        }
        case TRANSACTION_delete: {
            ...
        }
        case TRANSACTION_stop: {
            ...
        }
        case TRANSACTION_getQueueSize: {
            ...
        }
    }
    return super.onTransact(code, data, reply, flags);
}
```
Զ�̷�������ͨ��IBinder�ӿڵ�onTtransact()�������������ݡ��������ݣ����ҵ�����ʵ��ҵ���߼����루�������download�ӿڣ���ͨ��reply��ͻ��˴��ݷ���ֵ��

���⣬Stub������һ���Ƚ���Ҫ�Ľӿھ���asInterface()�ӿڣ������ڿͻ�������ʹ�õ�ʱ��ͨ��������ʹ������

```java
IDownloadService sIDownloadService = IDownloadService.Stub.asInterface(IBinder service);
```
ͨ���������֣����Ǵ��¿��Բ³��������ʵ�ֵĹ��ܣ����ǽ�һ��IBinder����ת��Ϊ�ӿڶ���IDownloadService��ͨ��������Կ��������߼���

```java
public static com.cundong.touch.IDownloadService asInterface(android.os.IBinder obj) {
            if ((obj == null)) {
                return null;
            }
            android.os.IInterface iin = obj.queryLocalInterface(DESCRIPTOR);
            if (((iin != null) && (iin instanceof com.cundong.touch.IDownloadService))) {
                return ((com.cundong.touch.IDownloadService) iin);
            }
            return new com.cundong.touch.IDownloadService.Stub.Proxy(obj);
        }
```
 ��ͨ��queryLocalInterface��ѯ���������˺Ϳͻ��˶�����ͬһ�����̣���ô�Ͳ���Ҫ������ˣ�ֱ�ӽ�IDownloadService������ͨ�Ķ�����ʹ�ã�����᷵��Զ�̶���Ĵ������


���ˣ����Ǿ�ͨ��Դ������˽���AIDL����ι����ˡ�

�ο���

[1][http://www.jianshu.com/p/cfb1d2a109a2]

[2][http://blog.csdn.net/xude1985/article/details/9232049]

[3][http://developer.android.com/intl/zh-cn/guide/components/aidl.html]


  [1]: http://www.jianshu.com/p/cfb1d2a109a2
  [2]: http://blog.csdn.net/xude1985/article/details/9232049
  [3]: http://developer.android.com/intl/zh-cn/guide/components/aidl.html
  [4]: http://7xr1sh.dl1.z0.glb.clouddn.com/469751777359982261.jpg