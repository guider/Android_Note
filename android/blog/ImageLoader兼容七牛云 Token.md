##使用imageloader加载图片时 修改缓存的使用的key


由于某些时候使用imageloader加载图片的时候整个图片链接不是固定的，会加上某些生成的一串编码，
这样图片只能在某些特定的时间段内访问，过了这个时间就换了另一串编码，这样就算是同一张图片，也是不同的链接，imageloader也会重新从网络加载。

为了避免不必要的加载，可以再存储图片的时候将存储hash的key存为不加上编码的图片链接，作为唯一的key。



初始化imageloader的时候更改discCacheFileNameGenerator，这是生成保存图片的名字。

这是默认的使用md5将链接生成的名字的配置方法：


 .discCacheFileNameGenerator(new Md5FileNameGenerator())


我更改了生成规则，使用图片名字作为存储的hash的key：





.discCacheFileNameGenerator(new Md5FileNameGenerator() {
                    @Override
                    public String generate(String imageUri) {
                        String imageName = imageUri;
                        try {
                            imageName = imageUri.split("[?]e=")[0];
                        } catch (Exception e) {
                            // e.printStackTrace();
                        }
                        return super.generate(imageName);
                    }
                })


我把原来的图片链接后面自动生成的一段东西去掉了，只取前面的一部分。

由于在存储图片和读取图片的时候，都会重新生成名字，再用这个名字去找图片，所以就可以避免同一张图片，不同链接的加载情况了。



我的完整imageloader初始化代码如下：


    DisplayImageOptions defaultOptions = new DisplayImageOptions.Builder().cacheInMemory(true).cacheOnDisc(true).build();
        ImageLoaderConfiguration config = new ImageLoaderConfiguration.Builder(context)//
                .defaultDisplayImageOptions(defaultOptions)//
                .threadPriority(Thread.NORM_PRIORITY - 2)//
                .denyCacheImageMultipleSizesInMemory()//
                .discCacheFileNameGenerator(new Md5FileNameGenerator() {
                    @Override
                    public String generate(String imageUri) {
                        String imageName = imageUri;
                        try {
                            imageName = imageUri.split("[?]e=")[0];
                        } catch (Exception e) {
                            // e.printStackTrace();
                        }
                        return super.generate(imageName);
                    }
                })//
                .tasksProcessingOrder(QueueProcessingType.LIFO)//
                .build();
        ImageLoader.getInstance().init(config);