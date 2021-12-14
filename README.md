# java8-basic
JDK 8 中提供了一组常用的核心函数接口：

  接口	               参数      	      返回类型	            			描述
Predicate<T>	      	 T		  boolean	   		用于判别一个对象。比如求一个人是否为男性
Consumer<T>	         T		  void				用于接收一个对象进行处理但没有返回，比如接收一个人并打印他的名字
Function<T, R>	         T		   R			        转换一个对象为不同类型的对象
Supplier<T>	        None		   T				提供一个对象
UnaryOperator<T>	 T	           T				接收对象并返回同类型的对象
BinaryOperator<T>      (T, T)	      	   T				接收两个同类型的对象，并返回一个原类型对象
其中 Cosumer 与 Supplier 对应，一个是消费者，一个是提供者。
Predicate 用于判断对象是否符合某个条件，经常被用来过滤对象。
Function 是将一个对象转换为另一个对象，比如说要装箱或者拆箱某个对象。
UnaryOperator 接收和返回同类型对象，一般用于对对象修改属性。BinaryOperator 则可以理解为合并对象。

lambda表达式学习
    1、使用的依据
能够使用Lambda的依据是必须有相应的函数接口（函数接口，是指内部只有一个抽象方法的接口）。这一点跟Java是强类型语言吻合，也就是说你并不能在代码的任何地方任性的写Lambda表达式。实际上Lambda的类型就是对应函数接口的类型。Lambda表达式另一个依据是类型推断机制，在上下文信息足够的情况下，编译器可以推断出参数表的类型，而不需要显式指名。Lambda表达更多合法的书写形式如下：
代码中，1展示了无参函数的简写；2处展示了有参函数的简写，以及类型推断机制；3是代码块的写法；4和5再次展示了类型推断机制。
// Lambda表达式的书写形式
Runnable run = () -> System.out.println("Hello World");// 1
ActionListener listener = event -> System.out.println("button clicked");// 2
Runnable multiLine = () -> {// 3 代码块
    System.out.print("Hello");
    System.out.println(" Hoolee");
};
BinaryOperator<Long> add = (Long x, Long y) -> x + y;// 4
BinaryOperator<Long> addImplicit = (x, y) -> x + y;// 5 类型推断

    2、自定义函数接口
自定义函数接口很容易，只需要编写一个只有一个抽象方法的接口即可。

// 自定义函数接口
@FunctionalInterface
public interface ConsumerInterface<T>{
	void accept(T t);
}
上面代码中的@FunctionalInterface是可选的，但加上该标注编译器会帮你检查接口是否符合函数接口规范。就像加入@Override标注会检查是否重载了函数一样。
有了上述接口定义，就可以写出类似如下的代码：

ConsumerInterface<String> consumer = str -> System.out.println(str);
进一步的，还可以这样使用：

class MyStream<T>{
	private List<T> list;
    ...
	public void myForEach(ConsumerInterface<T> consumer){// 1
		for(T t : list){
			consumer.accept(t);
		}
	}
}
MyStream<String> stream = new MyStream<String>();
stream.myForEach(str -> System.out.println(str));// 使用自定义函数接口书写Lambda表达式   
    
    3、Lambda表达式和匿名内部类在JVM层面的区别
