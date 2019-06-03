静态代理
	通用的接口是代理模式实现的基础

	代理类与真正业务实现的类都同时实现同一个接口
	
	代理类可以让你在不更改被代理类对象的基础上，进行一些功能的增加和增强

动态代理
	public static Object newProxyInstance(ClassLoader loader, Class<?>[] interfaces, InvocationHandler h)

	loader：类加载器
	interfaces：要用来代理的接口
	h：一个InvocationHandler对象
	
	每个代理对象内部都有一个InvocationHandler实现类，当被代理的方法执行时，代理会通知InvocationHandler，所以InvocationHandler是实际执行者

	InvocationHandler只有一个方法

		public interface InvocationHandler {

		    public Object invoke(Object proxy, Method method, Object[] args)
		        throws Throwable;
		}

	 invoke(Object proxy, Method method, Object[] args)
		proxy 被代理的对象
		method 代理对象调用的方法
		args  方法需要的参数

代理作用
	在不修改被代理类的基础上，进行功能的增强(怎么感觉有点类似装饰设计模式？？？？？)

装饰设计模式与代理模式的区别
	装饰设计模式在于客户知道自己就是想把被调用者的能力给提升，通过构造方法持有被装饰者的对象实例
	代理模式在于客户端并不关心代理对象是怎么实现的功能，只要你能实现功能即可
