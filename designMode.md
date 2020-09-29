# 单例模式

## 饿汉式

```
//饿汉式
public class Hungrey {

    private final static Hungrey hungrey = new Hungrey();

    private Hungrey(){

    }

    public static Hungrey getInstance(){
        return hungrey;
    }

}
```

假如类有很多占内存的变量会造成内存浪费。

## 懒汉式(DCL)

```
// 懒汉式
public class LazyMan {
	// 保障其他线程可见，避免极端情况
    private volatile static LazyMan lazyMan;

    private LazyMan(){}

    public static LazyMan getInstance(){
        if(lazyMan == null){
            synchronized (LazyMan.class){
                if(lazyMan == null){
                    /**
                     * new 一个对象有三个步骤
                     * 1、分配内存空间
                     * 2、执行构造方法，初始化对象
                     * 3、把对象指向空间
                     * 指令重排序导致的顺序可能有：123，132
                     */
                    lazyMan = new LazyMan();
                }
            }
        }
        return lazyMan;
    }
}
```

线程A执行132的顺序时，执行到13时，线程B执行第一个lazyMan==null，会直接return，这时的lazyMan是null。

## 枚举式

```
public enum EnumSingle {

    INSTANCE;

    public EnumSingle getInstance(){
        return INSTANCE;
    }
}
```

使用jad.exe反编译EnumSingle.class的java文件，可以看到这个类的构造方法是需要参数的。

![image-20200825161718762](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20200825161718762.png)