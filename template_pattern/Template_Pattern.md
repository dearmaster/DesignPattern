#ģ�巽��ģʽ

#####����
ģ�巽��ģʽ�Ķ��壺����һ�������е��㷨��ܣ�����һЩ�����ӳٵ������С�ʹ��������Բ��ı�һ���㷨�Ľṹ�����ض�����㷨��ĳЩ�ض����衣  

#####ʹ�ó���
1.��������й��еķ����������߼�������ͬ��  
2.�Ѻ��ĵ��㷨���Ϊģ�巽�������ܱߵ����ϸ�ڹ��ܷŵ�����ʵ�֡�    
3.���Ծ���ʹ��ģ�巽���ع����룬����ͬ�Ĵ���鵽�����У���ͨ��ʹ�ù��Ӻ�����Լ����������Ϊ��  

#####��ͼ
![1.png](./template_pattern/1.png "")  
templateMethod: ģ�巽������װ��һЩ���еĺ����㷨��  
hookMethod�����ӷ���������ʵ�־���ϸ�ڣ�����Լ��ģ�巽����  
abstractMethod��������ʵ�ַ�������Щ������������ʵ�֡�  

#####ʾ������

```

public abstract class AbstractTemplate {

	
	protected abstract void step1();
	
	protected abstract void step2();
	
	protected abstract void step3();
	
	protected void hookMethod() {
		System.out.println("Ĭ�Ϲ��ӷ���");
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
		System.out.println("ʵ����1��step1");
	}

	@Override
	protected void step2() {
		System.out.println("ʵ����1��step2");
		
	}

	@Override
	protected void step3() {
		System.out.println("ʵ����1��step3");
	}
	
	@Override
	protected void hookMethod() {

		System.out.println("ʵ����1�����ӷ���");
	}

}

public class ConcreteTemplate2 extends AbstractTemplate {

	@Override
	protected void step1() {
		System.out.println("ʵ����2��step1");
	}

	@Override
	protected void step2() {
		System.out.println("ʵ����2��step2");
		
	}

	@Override
	protected void step3() {
		System.out.println("ʵ����2��step3");
	}
	
	@Override
	protected void hookMethod() {

		System.out.println("ʵ����2�����ӷ���");
	}

}

public class ConcreteTemplate3 extends AbstractTemplate {
	
	@Override
	protected void step1() {
		System.out.println("ʵ����3��step1");
	}

	@Override
	protected void step2() {
		System.out.println("ʵ����3��step2");
		
	}

	@Override
	protected void step3() {
		System.out.println("ʵ����3��step3");
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

���н����

ʵ����1��step1
ʵ����1��step2
ʵ����1��step3
ʵ����1�����ӷ���
--------------------
ʵ����2��step1
ʵ����2��step2
ʵ����2��step3
ʵ����2�����ӷ���
--------------------
ʵ����3��step1
ʵ����3��step2
ʵ����3��step3
Ĭ�Ϲ��ӷ���


```