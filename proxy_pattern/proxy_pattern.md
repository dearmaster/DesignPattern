---
# 标题:设计模式之代理模式
##简介
**什么是代理模式?**
<br/>
代理模式的定义:为其他对象提供一种代理以控制对这个对象的访问。在某些情况下，一个对象不适合或者不能直接引用另一个对象，而代理对象可以在客户端和目标对象之间起到中介的作用。
<br/>
---
**有哪些场景我们能用到代理模式呢？**
生活中很多地方都能有代理模式的体验，举个例子。小张要去买一个联想笔记本电脑，此时他想到的必然是到联想授权的店里面去购买，而不是直接去找生产笔记本的厂商购买。此处，小张购买电脑这个过程便是与代理商进行沟通，买到它心仪的电脑，而不用与直接与厂商进行打交道。除了买电脑之外，像我们购买某品牌的家具，某品牌的电器也是直接和代理商打交道，来实现顾客的购买目的。我们在使用计算机的过程中，也有很多场景都用到了代理模式，比如通过代理连上数据库，浏览器翻墙等等。

##java如何实现代理模式
**java的代理可以分为静态代理和动态代理2种，此处主要介绍java的动态代理实现**
###静态代理 
---
继承实现静态代理、聚合实现静态代理。

1、使用继承来实现静态代理
>类图：![](/1.png)

代码:<br/>
`

public interface Moveable {
	void move();
}


import java.util.Random;
public class Car implements Moveable {

	@Override
	public void move() {
		//实现开车
		try {
			Thread.sleep(new Random().nextInt(1000));
			System.out.println("汽车行驶中....");
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}

}


public class Car2 extends Car {

	@Override
	public void move() {
		long starttime = System.currentTimeMillis();
		System.out.println("汽车开始行驶....");
		super.move();
		long endtime = System.currentTimeMillis();
		System.out.println("汽车结束行驶....  汽车行驶时间：" 
				+ (endtime - starttime) + "毫秒！");
	}

	
}

public class Client {

	/**
	 * 测试类
	 */
	public static void main(String[] args) {
		Car car = new Car();
		car.move();
		//使用集成方式
		Moveable m = new Car2();
		m.move();
		//使用聚合方式实现
		Car car = new Car();
		Moveable m = new Car3(car);
		m.move();
	}

}

`
<br/>
2、使用聚合来实现静态代理
>类图：![](/2.png)

代码:<br/>
`
public interface Moveable {
	void move();
}

import java.util.Random;

public class Car implements Moveable {

	@Override
	public void move() {
		//实现开车
		try {
			Thread.sleep(new Random().nextInt(1000));
			System.out.println("汽车行驶中....");
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}

}


public class CarLogProxy implements Moveable {

	public CarLogProxy(Moveable m) {
		super();
		this.m = m;
	}

	private Moveable m;
	
	@Override
	public void move() {
		System.out.println("日志开始....");
		m.move();
		System.out.println("日志结束....");
	}

}

public class CarTimeProxy implements Moveable {

	public CarTimeProxy(Moveable m) {
		super();
		this.m = m;
	}

	private Moveable m;
	
	@Override
	public void move() {
		long starttime = System.currentTimeMillis();
		System.out.println("汽车开始行驶....");
		m.move();
		long endtime = System.currentTimeMillis();
		System.out.println("汽车结束行驶....  汽车行驶时间：" 
				+ (endtime - starttime) + "毫秒！");
	}

}

public class Client {

	/**
	 * 测试类
	 */
	public static void main(String[] args) {
		Car car = new Car();
		CarLogProxy clp = new CarLogProxy(car);
		CarTimeProxy ctp = new CarTimeProxy(clp);
		ctp.move();
	}

}
`

关于继承和聚合：继承不利于扩展，一般情况下我们推荐使用聚合来代替继承。


###动态代理
---
jdk实现动态代理、cglib实现动态代理。

1、jdk动态代理：
实现InvocationHandler接口，重写invoke方法。
>类图：![](/3.png)


代码:<br/>
`
public interface Moveable {
	void move();
}

import java.util.Random;

public class Car implements Moveable {

	@Override
	public void move() {
		//实现开车
		try {
			Thread.sleep(new Random().nextInt(1000));
			System.out.println("汽车行驶中....");
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}

}

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class TimeHandler implements InvocationHandler {

	public TimeHandler(Object target) {
		super();
		this.target = target;
	}

	private Object target;	
	
	/*
	 * 参数：
	 * proxy  被代理对象
	 * method  被代理对象的方法
	 * args 方法的参数
	 * 
	 * 返回值：
	 * Object  方法的返回值
	 * */
	@Override
	public Object invoke(Object proxy, Method method, Object[] args)
			throws Throwable {
		long starttime = System.currentTimeMillis();
		System.out.println("汽车开始行驶....");
		method.invoke(target);
		long endtime = System.currentTimeMillis();
		System.out.println("汽车结束行驶....  汽车行驶时间：" 
				+ (endtime - starttime) + "毫秒！");
		return null;
	}

}

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Proxy;

import Car;
import Moveable;

public class Test {

	/**
	 * JDK动态代理测试类
	 */
	public static void main(String[] args) {
		Car car = new Car();
		InvocationHandler h = new TimeHandler(car);
		Class<?> cls = car.getClass();
		/**
		 * loader  类加载器
		 * interfaces  实现接口
		 * h InvocationHandler
		 */
		Moveable m = (Moveable)Proxy.newProxyInstance(cls.getClassLoader(),
												cls.getInterfaces(), h);
		m.move();
	}

}

`


2、cglib动态代理：通过实现方法拦截来实现动态代理，此处必须要导入cglib的jar包。
>类图：![](/4.png)


代码:<br/>

`
public class Train {

	public void move(){
		System.out.println("火车行驶中...");
	}
}


import java.lang.reflect.Method;

import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

public class CglibProxy implements MethodInterceptor {

	private Enhancer enhancer = new Enhancer();
	
	public Object getProxy(Class clazz){
		//设置创建子类的类
		enhancer.setSuperclass(clazz);
		enhancer.setCallback(this);
		
		return enhancer.create();
	}
	
	/**
	 * 拦截所有目标类方法的调用
	 * obj  目标类的实例
	 * m   目标方法的反射对象
	 * args  方法的参数
	 * proxy代理类的实例
	 */
	@Override
	public Object intercept(Object obj, Method m, Object[] args,
			MethodProxy proxy) throws Throwable {
		System.out.println("日志开始...");
		//代理类调用父类的方法
		proxy.invokeSuper(obj, args);
		System.out.println("日志结束...");
		return null;
	}

}


public class Client {

	/**
	 * @param args
	 */
	public static void main(String[] args) {

		CglibProxy proxy = new CglibProxy();
		Train t = (Train)proxy.getProxy(Train.class);
		t.move();
	}

}
`

总结:JDK动态代理是面向接口的代理，通过实现InvocationHandler接口来完成代理。而cglib不需要声明接口，只用实现方法拦截器就能实现代理


**未完，还要剖析如何生成代理的字节码**

## 参考文档
1.慕课网:模式的秘密---代理模式



