#适配器模式

#####前言：
在我们的日常生活中，时常会用到适配器这个概念。假如我买了一个港版的iphone，想要在大陆使用，那么商家给我配的充电器必定会带上一个适配器。
这个笨重的东西介于充电器与插座之间，充当一个适配，中转的角色。原因是香港用的是英式的插座，与大陆有所不同，所以就出现了适配器这个东西。
难么这个适配器的有点是什么呢？假如不使用适配器，那我们该用什么办法解决这个问题呢？  
我首先想到了，把家里的插座改成英式插座，于是乎我花了九牛二虎之力改装完毕，就这样我可以愉快的充电了。但是问题来了，我其他电器无法使用
这个插座了，这可如何是好。而且当我出门以后，在任何一个地方都找不到合适的充电插座，所以这个方案失败！  
接下来我想了一个办法就是改变我的iphone充电器，但是问题来了，这个充电器是厂家制造的，并不是我个人所能完成的。于是我只能通知厂家，改变
这个插头，但是厂家不理我，他的理由是这批货生产了好多，如果全都改掉成本太大，而且这个产品本来就是面向香港的。第二个方案也失败了。  
经历了一系列的失败以后，我才恍然大悟，那个笨重的适配器存在的意义。在不改变双方的前提下，我依旧能使双方互相配合协作，那么我只能在两者之间再增加一层进行撮合，这就被成为适配器。  
还是那句话，软件即生活的抽象，我们在项目开发过程中，通常会对系统进行扩展，就需要将原本两个不同的接口配合使用，但是由于这两个接口在各自的模块中早已被频繁使用，不容易修改，这也不符合开闭原则。于是就想到了在两个接口中加一层适配器，这样做既可以不改变双方接口，也可以是两个接口可以互相愉快的调用，一举两得的事情。  

#####类图：
![1.jpg](/adapter_pattern/1.jpg "")

#####通用代码：

1.首先定义一个target接口    
```
public interface Target {

	public void request();
}
```

2.target的实现类  
```
public class ConcreteTarget implements Target {

	@Override
	public void request() {

		System.out.println("it is Target");
	}
}
```

3.定义被适配的类  
```
public class Adaptee {

	public void doSomething(String str) {
		System.out.println(str);
	}
}
```

4.适配器  
```
public class Adapter extends Adaptee implements Target {

	@Override
	public void request() {
		doSomething("adapter call the adaptee");
	}
}
```

5.客户端代码  
```
public class Client {

	public static void main(String[] args) {

		Target t = new ConcreteTarget();
		t.request();
		
		Target t2 = new Adapter();
		t2.request();
	}

}
```

6.输出  
```
it is Target
adapter call adaptee
```







