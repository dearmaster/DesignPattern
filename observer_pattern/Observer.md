#观察者模式

#####前言：
观察者模式又名（发布者/订阅者模式），顾名思义，我可以观察你，监视你，你的一举一动我了如指掌。你也可以发布消息，一条广播出来，所有收听你节目的观众都能收到你的消息。程序就是现实生活的抽象，通常一个系统中会涉及到多个不同模块之间会有相互关联，比如当一个模块A完成某种操作以后，其他模块B,C,D就要做出相应的响应。这种方式就可以使用观察者模式来实现。  

#####应用场景
1.tomcat的生命周期管理： tomcat整个系统有非常多的组件构成。当tomcat启动的时候，就需要利用这种设计，通知其他所有注册的组件一并启动。  
2.消息队列的实现：生产者将消息加入队列，通知特定主体的消费者从队列中获取消息。  
3.文件系统的设计：当删除或增加一个文件的时候，文件目录，资源管理器等都会做出相应的改变。  

#####类图

![](/12.bmp)

#####代码实现：

1.首先定义一个首先主体类：  

```
public abstract class Subject {

	
	private ArrayList<Observer> list = new ArrayList<Observer>(); 
	
	public void registerObserver(Observer observer) {
		
		if (observer != null) {
			list.add(observer);
		}
	}
	
	public void removeObserver(Observer observer) {
		list.remove(observer);
	}
	
	public void nodify(Object obj) {
		for (Observer observer : list) {
			observer.update(obj);
		}
	}
}
```

2.然后定义抽象主题类的实现类，这个类就是具体被观察的对象，他的一举一动都可以调用nodify通知观察者  

```
package com.demo.observer;

public class ContreteSubject extends Subject{

	public void todo() {
		System.out.println("我是被观察者！");
		nodify("通知：明天放假");
	}
}

```

3.接下来定义抽象观察者，这个类只提供一个统一的update接口，所有观察者都需要实现这个方法，并执行具体的业务。  

```
public interface Observer {

	public void update(Object obj);
}

```
4.最后实现具体的观察者类  

```
public class WatcherA implements Observer {
	
	@Override
	public void update(Object obj) {
		System.out.println("我是观察者A：  "+obj.toString());
	}
	
}

public class WatcherB implements Observer {

	@Override
	public void update(Object obj) {
		System.out.println("我是观察者B：  "+obj.toString());
	}

}

public class WatcherC implements Observer {

	@Override
	public void update(Object obj) {
		System.out.println("我是观察者C：  "+obj.toString());
	}

}
```

5.测试  

```
public class Client {

	public static void main(String[] args) {
		
		ContreteSubject sub = new ContreteSubject();
		WatcherA a = new WatcherA();
		sub.registerObserver(a);
		sub.registerObserver(new WatcherA());
		sub.registerObserver(new WatcherB());
		sub.registerObserver(new WatcherC());

		sub.todo();
	}

}
```

```
我是被观察者！
我是观察者A：  通知：明天放假
我是观察者A：  通知：明天放假
我是观察者B：  通知：明天放假
我是观察者C：  通知：明天放假

```





