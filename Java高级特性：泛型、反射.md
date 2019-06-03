泛型:
	任何的泛型变量都派生自Object，所以不能使用基本数据类型填充泛型类

	泛型类型绑定：extends
		有时候你想要泛型只能是某一类型，这时候就需要使用类型绑定。让泛型只能是某一部分类型
		extends不同于继承的哪个extends ，他所能绑定的接口与类都可以

通配符
	无边界通配符  他是一个未知符号，可以代表任意类  未知类型
	通配符只能用于填充泛型变量T，不能用于定义变量
	
	通配符也可以使用extends进行类型绑定
	
	Point<? extends Number>  
	
	point = new Point<Integer>(3,3)  //此时point的内部填充类型依然是继承自Number的通配符，不是Integer的类型

	？ extends xxx  派生自xxx的子类   
	？ super  XXX  填充为xxx的任意父类
	
	如果想从一个数据类型里获取数据  使用？ extends xxx  能取不能存
	如果想把数据存入一个数据类型里  使用？ super xxx  能存不能取

	构造泛型实例时，如果没有填充类型，则默认为无边界通配符(?)

反射(相当重要......Activity与Application的创建基本都是使用反射  View加载也是)
	众所周知，java代码需要编译和运行
		编译阶段将java代码编译成字节码文件

		运行阶段：
			分成三个阶段：

			1、加载
				使用类加载器加载class文件的二进制文件，将此文件加载到JVM的方法区，并且在堆区创建描述这个类的对象。用来封装数据。
				一个类只能被加载一次
					
			2、链接
				把二进制数据组装成可以运行的状态。分为校验、准备、解析3各阶段。校验意思是查看此二进制文件是否适用于当前的JVM，准备是给静态成员分配内存空间，并设置默认值。解析指的是将常量池中的代码作为直接引用的过程，直到所有的符号引用都可以被运行程序使用

			3、初始化
				就是对类的变量进行初始化，初始化之后，类就完成了初始化，初始化之后类的对象就能正常使用了，直到一个对象不再使用，将被垃圾回收。释放资源

获取类类型】
	以下三种方式获取类类型  (由于类只能被装载一次，所以他们是相等的)  需要了解一下三种类类型的变量得泛型填充类型
	 	Class<Person> personClass = Person.class;
    
        Person person = new Person();
        Class<? extends Person> personClass1 = person.getClass();

        Class<?> personClass2 = Class.forName("com.reflex.test.Person"); //需要处理异常  将类装载、链接、初始化
	第四种方式获取
		context.getClassLoader().loadClass(String ClassName);//只是将类装载 并没有链接和初始化

	获取超类型
		getSuperClass  获取的直接继承的父类
		getInterfaces() 获取所有直接实现的接口数组  直接实现

	获取类的访问修饰符
		int modifiers = getModifier()   Modifier.toString(modifiers);//直接打印出访问修饰符

获取泛型超类以及接口的相关信息
	Type type = getGenericSuperClass();
	如果该type是一个泛型类 直接转换 ParameterizedType parameterizedType = (ParameterizedType) type;
	 parameterizedType 
		Type type = parameterizedType.getRawType();//获取该泛型类的真实类型
		Type[] actualTypeArguments = parameterizedType.getActualTypeArguments();//获取该泛型类的所有泛型填充类型
		
		actualTypeArguments的填充类型若能确定类型则直接转为Class对象  若为不能确定的泛型类型 则为 TypeVariable
		若为通配符，则为WildcardType  若为数组 则为GenericArrayType类型 
		
		如果对应的是ArrayList、List这样的类型，则是ParameterizedType

		TypeVariable 操作：  <>内部为未知的泛型变量
			String  getName();//获取泛型变量的名称		
			Type[]  getBounds();//获取当前泛型变量的上边界的类型的数组  如果没有则为Object

		GenericArrayType 操作：  <>内部为数组类型
			Type getGenericComponentType();获取的数组类型所对应的type

		WildcardType  操作：  <>内部为通配符类型
			Type[] getUpperBounds();//获取上边界对象列表
			Type[] getLowerBounds();//获取下边界对象列表
			
			例子：
				<? extends Integer>:这个通配符的上边界就是Integer.Class，下边界就是null
				<? super String>:这个通配符的下边界是String,上边界就是Object；

反射(3) 获取类内部信息

  构造方法
	getDeclaredConstructors()//获取所有的构造方法
	Constructor<?> constructor = Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes)//根据传入的类型获取执行的构造方法 类型
	
	Constructor 常用方法
		public T newInstance(Object... args);//创建对象
		Class<?>[] getParameterTypes();//获取一般的参数类型
		Type[] getGenericParameterTypes();//解析具有泛型类型参数的构造函数
		getModifiers();//获取访问修饰符
		Class<T> getDeclaringClass(); //得到声明Constructor类的class对象

  成员变量
	
	Field[] getDeclaredFields();//可以获取全部的成员变量
	Field getDeclaredField(String name);//获取指定变量名的Field类型

	常用方法
		field.getName()：//用于得到当前成员变量的名称，比如Person类中的age,name
    	field.getType()：//用于得到当前成员变量的类型。

		void set(Object object, Object value)//为指定对象设置变量值
    	Object get(Object object)//获取指定对象的变量值
		
		Class<T> getDeclaringClass(); //得到声明Field类的class对象

  方法
	Method[] getDeclaredMethods()//获取一个类中所有的方法
	Method getDeclaredMethod(String name, Class<?>... parameterTypes);//根据指定的方法名和方法参数类型获取Method类型对象
	
	Method常用方法：
		Object invoke(Object receiver, Object... args);;/执行指定对象的方法  receiver 指定对象 ， args[]  方法参数值 ，返回值  实际方法的返回值

		Class<?>[] getParameterTypes()//获取指定Method的参数类型数组
    	Type[] getGenericParameterTypes()//获取含有泛型变量类型的数组

		Class<?> getReturnType()  //获取普通返回值类型
    	Type getGenericReturnType() //获取泛型类型的返回值类型
	 
	
	