#模板方法模式

#####定义
模板方法模式的定义：定义一个操作中的算法框架，而将一些步骤延迟到子类中。使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。  

#####使用场景
1.多个子类有公有的方法，并且逻辑基本相同。  
2.把核心的算法设计为模板方法，而周边的相关细节功能放到子类实现。    
3.可以经常使用模板方法重构代码，把相同的代码抽到父类中，并通过使用钩子函数来约束控制其行为。  

#####类图
![1.png](./template_pattern/1.png "")  
templateMethod: 模板方法，封装了一些公有的核心算法。  
hookMethod：钩子方法，子类实现具体细节，用来约束模板方法。  
abstractMethod：基本的实现方法，这些方法由子类来实现。  

#####示例代码

```

public abstract class AbstractTemplate {

	
	protected abstract void step1();
	
	protected abstract void step2();
	
	protected abstract void step3();
	
	protected void hookMethod() {
		System.out.println("默认钩子方法");
	};
	
	public void templateMethod() {
		step1();
		step2();
		step3();
		hookMethod();
	}
}

public class ConcreteTemplate1 extends AbstractTemplate {

	@Override
	protected void step1() {
		System.out.println("实现类1：step1");
	}

	@Override
	protected void step2() {
		System.out.println("实现类1：step2");
		
	}

	@Override
	protected void step3() {
		System.out.println("实现类1：step3");
	}
	
	@Override
	protected void hookMethod() {

		System.out.println("实现类1：钩子方法");
	}

}

public class ConcreteTemplate2 extends AbstractTemplate {

	@Override
	protected void step1() {
		System.out.println("实现类2：step1");
	}

	@Override
	protected void step2() {
		System.out.println("实现类2：step2");
		
	}

	@Override
	protected void step3() {
		System.out.println("实现类2：step3");
	}
	
	@Override
	protected void hookMethod() {

		System.out.println("实现类2：钩子方法");
	}

}

public class ConcreteTemplate3 extends AbstractTemplate {
	
	@Override
	protected void step1() {
		System.out.println("实现类3：step1");
	}

	@Override
	protected void step2() {
		System.out.println("实现类3：step2");
		
	}

	@Override
	protected void step3() {
		System.out.println("实现类3：step3");
	}
	
}

public class Client {

	public static void main(String[] args) {

		AbstractTemplate m1 = new ConcreteTemplate1();
		AbstractTemplate m2 = new ConcreteTemplate2();
		AbstractTemplate m3 = new ConcreteTemplate3();
		
		m1.templateMethod();
		System.out.println("--------------------");
		m2.templateMethod();
		System.out.println("--------------------");
		m3.templateMethod();
	}

}

运行结果：

实现类1：step1
实现类1：step2
实现类1：step3
实现类1：钩子方法
--------------------
实现类2：step1
实现类2：step2
实现类2：step3
实现类2：钩子方法
--------------------
实现类3：step1
实现类3：step2
实现类3：step3
默认钩子方法


```