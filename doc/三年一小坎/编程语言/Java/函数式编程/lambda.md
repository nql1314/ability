## 函数式编程 Lambda 表达式
对象在流操作（函数操作里面）里面,不应该产生任何修改（副作用side-effects）,修改对象的时候应该返回新对象.
### 将函数作为参数
Java 中 Lambda 表达式一共有五种基本形式,具体如下:
Runnable noArguments = () -> System.out.println("Hello World");
ActionListener oneArgument = event -> System.out.println("button clicked");
Runnable multiStatement = () -> {
    System.out.print("Hello");
    System.out.println(" World");
};
BinaryOperator<Long> add = (x, y) -> x + y;
BinaryOperator<Long> addExplicit = (Long x, Long y) -> x + y;

lambda表达式中参数是final的

### JDK 8 中提供了一组常用的核心函数接口:

接口	参数	返回类型	描述
Predicate<T>	T	boolean	用于判别一个对象.比如求一个人是否为男性
Consumer<T>	T	void	用于接收一个对象进行处理但没有返回,比如接收一个人并打印他的名字
Function<T, R>	T	R	转换一个对象为不同类型的对象
Supplier<T>	None	T	提供一个对象
UnaryOperator<T>	T	T	接收对象并返回同类型的对象
BinaryOperator<T>	(T, T)	T	接收两个同类型的对象,并返回一个原类型对象

BiFunction 提供了三个参数的方法

