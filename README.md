# Android-
### Activity的生命周期与fragment的生命周期 ###
![](http://img.my.csdn.net/uploads/201211/29/1354170682_3824.png) ![](http://img.my.csdn.net/uploads/201211/29/1354170699_6619.png)
### ListView布局之View复用原理 ###
ListView的父类AbsList中有一个变量：
    `/** 
    * The data set used to store unused views that should be reused during the next layout 
    * to avoid creating new ones 
    */  
   final RecycleBin mRecycler = new RecycleBin();`
注释的意思上用一个数据集来存储应当在下一个布局重用的View，避免重新创建新的布局。这个对象应该就是对我们缓存管理的核心类了。

注释的意思上用一个数据集来存储应当在下一个布局重用的View，避免重新创建新的布局。这个对象应该就是对我们缓存管理的核心类了。

如果数据发生变化则把当前的ItemView放入ScrapViews中，否则把当前显示的ItemView放入ActiveViews中。

Android提供了一个叫做Recycler(反复循环)的构件，就是当ListView的Item从滚出屏幕视角之外，对应Item的View会被缓存到Recycler中，相应的会从生成一个Item，而此时调用的getView中的convertView参数就是滚出屏幕的缓存Item的View，所以说如果能重用这个convertView，就会大大改善性能。

我们都知道在getView()方法中的操作是这样的：先从xml中创建view对象（inflate操作，我们采用了重用convertView方法优化），然后在这个view去findViewById，找到每一个item的子View的控件对象，如：ImageView、TextView等。这里的findViewById操作是一个树查找过程，也是一个耗时的操作，所以这里也需要优化，就是使用ViewHolder，把每一个item的子View控件对象都放在Holder中，当第一次创建convertView对象时，便把这些item的子View控件对象findViewById实例化出来并保存到ViewHolder对象中。然后用convertView的setTag将viewHolder对象设置到Tag中， 当以后加载ListView的item时便可以直接从Tag中取出复用ViewHolder对象中的，不需要再findViewById找item的子控件对象了。这样便大大提高了性能。
### Android中，px、sp、dip、dp的区别与联系 ###

     dip: device independent pixels(设备独立像素). 不同设备有不同的显示效果,这个和设备硬件有关，一般我们为了支持WVGA、HVGA和QVGA 推荐使用这个，不依赖像素。 

     dp: dip是一样的

     px: pixels(像素). 不同设备显示效果相同，一般我们HVGA代表320x480像素，这个用的比较多。

     pt: point，是一个标准的长度单位，1pt＝1/72英寸，用于印刷业，非常简单易用；
     sp: scaled pixels(放大像素). 主要用于字体显示best for textsize。

    in（英寸）：长度单位。 
    mm（毫米）：长度单位。
### Broadcast、Content Provider和AIDL的区别和联系 ###


1. Broadcast：发送和接收广播，可实现消息的传递



1. Aidl：全称Android Interface definition language顾名思义，就是不同进程间的通信接口

1. ContentProvider：暴露app的数据访问接口，让其他应该访问app数据

1. Messager：本质是Aidl，对Aidl进行了封装，不用写.aidl文件





----------
 各自的优缺点




1. Broadcast：只要注册了广播，都能收到，有点范围广，缺点速度慢必须在一定时间完成处理操作（onRecevice执行必须在10秒完成，否则系统认为该程序无响应造成ANR，所以不能做耗时的操作）



1. Aidl：进程间的通信，速度快(系统底层直接是共享内存)，性能稳，效率高，一般进程间通信就用它。



1. Messager：效率应该是和Aidl是一样的，与Aidl的区别在于Messager是线程安全的，而Aidl是非线程安全的，所以Aidl在使用的时候应该注意这个问题



1. ContentProvider：一般是成熟的App暴露自己的数据，其他app可以获取到数据，数据本身不是实时的，而前面三种是实时的数据

### Android中最常用的设计模式
 