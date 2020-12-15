##反射
* 反射(Reflection)是Java 程序开发语言的特征之一,它允许运行中的 Java 程序获取自身的信息,并且可以操作类或对象的内部属性.
* 反射的核心是JVM在运行时才动态加载类或调用方法/访问属性,它不需要事先（写代码的时候或编译期）知道运行对象是谁.
    1. 在运行时判断任意一个对象所属的类;
    2. 在运行时构造任意一个类的对象;
    3. 在运行时判断任意一个类所具有的成员变量和方法（通过反射甚至可以调用private方法）;
    4. 在运行时调用任意一个对象的方法
* 获取Class对象
    1. Class.forName(driver);
    2. Class<?> klass = int.class;
    3. Class<?> classInt = Integer.TYPE;
    4. Class<?> klass = str.getClass();
* 判断是否为某个类的实例
    1. public native boolean isInstance(Object obj);
* 创建实例
    1. 使用Class对象的newInstance()方法来创建Class对象对应类的实例;
    2. 先通过Class对象获取指定的Constructor对象,再调用Constructor对象的newInstance()方法来创建实例.这种方法可以用指定的构造器构造类的实例.

