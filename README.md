# Android-知识整理
### Activity的生命周期与fragment的生命周期 ###
在Fragment 中onCreateView是创建的时候调用，onViewCreated是在onCreateView后被触发的事件，前后关系。
![](http://img.my.csdn.net/uploads/201211/29/1354170682_3824.png) ![](http://img.my.csdn.net/uploads/201211/29/1354170699_6619.png)
### ListView布局之View复用原理 ###
ListView的父类AbsList中有一个变量：

    /** 
    * The data set used to store unused views that should be reused during the next layout 
    * to avoid creating new ones 
    */  
	final RecycleBin mRecycler = new RecycleBin(); 

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
- 单例模式：确保一个类只有一个实例，并且自行实例化并向整个系统提供整个实例	
 	- 对于那些耗内存的类，只实例化一次，大大提高性能，尤其是移动开发中程序运行中，始终保持只有一个实例在内存中
 	
			public class ActivityManager {  
			  
			    private static volatile ActivityManager instance;  
			    private Stack<Activity> mActivityStack = new Stack<Activity>();  
			      
			    private ActivityManager(){  
			          
			    }  
			      
			    public static ActivityManager getInstance(){  
			        if (instance == null) {  
			        synchronized (ActivityManager.class) {  
			            if (instance == null) {  
			                instance = new ActivityManager();  
			            }  
			        }  
			        return instance;  
			    }  
			      
			    public void addActicity(Activity act){  
			        mActivityStack.push(act);  
			    }  
			      
			    public void removeActivity(Activity act){  
			        mActivityStack.remove(act);  
			    }  
			      
			    public void killMyProcess(){  
			        int nCount = mActivityStack.size();  
			        for (int i = nCount - 1; i >= 0; i--) {  
			            Activity activity = mActivityStack.get(i);  
			            activity.finish();  
			        }  
			          
			        mActivityStack.clear();  
			        android.os.Process.killProcess(android.os.Process.myPid());  
			    }  
			}
- Builder 模式:将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示。
 
	-- 定义一个静态内部类Builder，内部成员变量跟外部一样

	-- Builder通过一系列方法给成员变量赋值，并返回当前对象（this）

	-- Builder类内部提供一个build方法方法或者create方法用于创建对应的外部类，该方法内部调用了外部类的一个私有化构造方法，该构造方法的参数就是内部类Builder

	-- 外部类提供一个私有化的构造方法供内部类调用，在该构造函数中完成成员变量的赋值

			public class Person {  
		    private String name;  
		    private int age;  
		    private double height;  
		    private double weight;  
		  
		    privatePerson(Builder builder) {  
		        this.name=builder.name;  
		        this.age=builder.age;  
		        this.height=builder.height;  
		        this.weight=builder.weight;  
		    }  
		    public String getName() {  
		        return name;  
		    }  
		  
		    public void setName(String name) {  
		        this.name = name;  
		    }  
		  
		    public int getAge() {  
		        return age;  
		    }  
		  
		    public void setAge(int age) {  
		        this.age = age;  
		    }  
		  
		    public double getHeight() {  
		        return height;  
		    }  
		  
		    public void setHeight(double height) {  
		        this.height = height;  
		    }  
		  
		    public double getWeight() {  
		        return weight;  
		    }  
		  
		    public void setWeight(double weight) {  
		        this.weight = weight;  
		    }  
		  
		    static class Builder{  
		        private String name;  
		        private int age;  
		        private double height;  
		        private double weight;  
		        public Builder name(String name){  
		            this.name=name;  
		            return this;  
		        }  
		        public Builder age(int age){  
		            this.age=age;  
		            return this;  
		        }  
		        public Builder height(double height){  
		            this.height=height;  
		            return this;  
		        }  
		  
		        public Builder weight(double weight){  
		            this.weight=weight;  
		            return this;  
		        }  
		  
		        public Person build(){  
		            return new Person(this);  
		        }  
		    }  
		}  
	从上边代码我们可以看到我们在Builder类中定义了一份跟Person类一样的属性，通过一系列的成员函数进行赋值，但是返回的都是this,最后提供了一个build函数来创建person对象，对应的在Person的构造函数中，传入了Builder对象，然后依次对自己的成员变量进行赋值。此外，Builder的成员函数返回的都是this的另一个作用就是让他支持链式调用，使代码可读性大大增强。
	于是我们就可以这样创建Person对象：

		Person.Builder builder=new Person.Builder();  
		Person person=builder  
        .name("张三")  
        .age(18)  
        .height(178.5)  
        .weight(67.4)  
        .build();  
	
- 观察者模式：定义对象间的一种一对多的依赖关系，当一个对象的状态发送改变时，所有依赖于它的对象都能得到通知并被自动更新

	被观察者：

			public class Observable<T> {  
			    List<Observer<T>> mObservers = new ArrayList<Observer<T>>();  
			  
			    public void register(Observer<T> observer) {  
			        if (observer == null) {  
			            throw new NullPointerException("observer == null");  
			        }  
			        synchronized (this) {  
			            if (!mObservers.contains(observer))  
			                mObservers.add(observer);  
			        }  
			    }  
			  
			    public synchronized void unregister(Observer<T> observer) {  
			        mObservers.remove(observer);  
			    }  
			  
			    public void notifyObservers(T data) {  
			        for (Observer<T> observer : mObservers) {  
			            observer.onUpdate(this, data);  
			        }  
			    }  
			  
			}  



- 策略模式：策略模式定义了一系列算法，并将每一个算法封装起来，而且使他们可以相互替换，策略模式让算法独立于使用的客户而独立改变
- 原型模式：用原型实例指定创建对象的种类，并通过拷贝这些原型创建新的对象。


### Android 中的动画种类 
Android动画主要包含补间动画（Tween）View Animation、帧动画（Frame）Drawable Animation、以及属性动画Property Animation。

- Tween动画，通过对View的内容进行一系列的图形变换 (包括平移、缩放、旋转、改变透明度)来实现动画效果。动画效果的定义可以采用XML来做也可以采用编码来做。

		| 动画类型              | 编码定义动画使用的类     | 
		| -------------------  |:----------------------:|
		| 渐变透明度动画效果     | AlphaAnimation         |
		| 渐变尺寸缩放动画效果   | ScaleAnimation         |  
		| 画面位置移动动画效果   | TranslateAnimation     |   
		| 画面旋转动画效果      | RotateAnimation       |

- Frame动画，即顺序播放事先做好的图像，跟放胶片电影类似。

- 属性动画 动画的执行类来设置动画操作的对象的属性、持续时间，开始和结束的属性值，时间差值等，然后系统会根据设置的参数动态的变化对象的属性。
