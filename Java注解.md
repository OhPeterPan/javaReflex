定义
	@interface 关键字进行定义

元注解
	注解到注解的注解 @Retention、@Documented、@Target、@Inherited、@Repeatable 

@Retention
	值有三个：
		RetentionPolicy.SOURCE  该注解只存在于源码中，编译期将被丢弃
		RetentionPolicy.CLASS   注解只保留到编译进行的时候，并不会被加载到JVM中
		RetentionPolicy.RUNTIME 注解被保留到程序运行的时候，被加载到JVM中，程序运行时可以获取它们

@Documented  文档
	表示将注解的元素包含到Javadoc中去

@Target  目标  指定了注解运用的地方
	ElementType.ANNOTATION_TYPE 可以给一个注解进行注解
	ElementType.CONSTRUCTOR 可以给构造方法进行注解
	ElementType.FIELD 可以给属性进行注解
	ElementType.LOCAL_VARIABLE 可以给局部变量进行注解
	ElementType.METHOD 可以给方法进行注解
	ElementType.PACKAGE 可以给一个包进行注解
	ElementType.PARAMETER 可以给一个方法内的参数进行注解
	ElementType.TYPE 可以给一个类型进行注解，比如类、接口、枚举

@Inherited  继承 表示的是当一个超类的注解被这个注解注解的话，如果有个子类此时继承超类，则超类的这个注解也会被子类继承

@Repeatable  可重复的  (这个没有怎么理解)
	示例：
		@interface Persons { 

				Person[] value(); 

				}
						
		 @Repeatable(Persons.class)  Persons相当于一个容器注解  用来存放其它注解  可以理解为Persons是一个总的标签，上面贴满							 了Person这种同类型但是内容不太一样的标签  把Persons给一个SuperMe贴上，相当于给他贴上很							 多标签
		 @interface Person{ 

				String role default ""; 
				
				}
		
		 @Person(role="artist")
		 @Person(role="coder")
		 @Person(role="PM") 
		public class SuperMan{ 
		
				}

注解的属性
	也叫 成员变量  只有成员变量，没有方法  成员变量被定义成"无形参的方法"形式来声明，无方法体，方法名是变量名字，返回值定义了变量的类型

	属性类型：8种基本数据类型  类、接口、注解以及他们的数组	

	示例：
		@Target(ElementType.TYPE)
		@Retention(RetentionPolicy.RUNTIME)
		public @interface TestAnnotation {
			
			int id() default 0;//带默认值的成员变量
			
			String msg();
		
		}

		@TestAnnotation(id=3,msg="hello annotation")
		public class Test {
		
		}

	如果一个注解只有一个变量，在使用时可以直接赋值，不用写 变量名=xxx

使用反射获取注解
	Class类型有几个方法可以获取注解
		public boolean isAnnotationPresent(Class<? extends Annotation> annotationClass) {} //是否应用了某个注解
		
		public <A extends Annotation> A getAnnotation(Class<A> annotationClass) {}//根据参数获取指定注解
		
		public Annotation[] getAnnotations() {} //获取所有的注解

