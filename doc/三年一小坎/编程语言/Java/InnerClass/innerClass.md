# 内部类
* 在其他地方创建内部类:OutClass.InnerClass;
* 内部类自动拥有对其外部类所有成员的访问权;
* 在内部类中生成对外部类的引用,使用OutClass.this;  
* 创建内部类 OutClass.InnerClass = out.new InnerClass(),必须使用外部类的对象来创建.
* 局部内部类:定义在方法中的内部类,不能有访问说明符,在代码块中创建,
  可以访问当前代码块的常量以及外围类的所有成员
* 匿名内部类:创建一个继承自Contents的匿名类的对象;如果使用一个外部的对象,则需要将引用设定为final.
    public class OutClass{
        public Cont ents getContents(){
        return new Contents(){
            private int i = 11;
            public int value(){
            return i;
            }
        };
        }
    }
* 嵌套内部类 static的内部类
* 闭包是一个可调用的对象,内部类是面向对象的闭包
* 内部类继承时必须初始化其外围类引用
