#������ģʽ

#####ǰ�ԣ�
��������������У�ͨ����Ҫ����һЩ������ȷ�����������Щ���󣬿����߻����ȶ����������������Щ���������һ��������������ҳ�һ�����ʺϵĴ����������������������Ҿ�һ�������������⡣  
С�����Ϻ�����һ��ʱ���ϣ���ܰ����Ϻ���ס֤�� ���������һ���鷳�£���Ҫ��ǰ׼�����ٵĲ����Լ��Ƿ�߱�������ʸ�������Ҫ׼��������6���µ��籣֤�����Լ���С����ס��֤���ȣ�Ȼ��Я����ȫ������ȥ����������һ���롣  
���ǰ�С�ŵ����뿴����һ�����󣬰Ѹ������ز��ſ����ǽ����������Ĵ�����󡣲����кü�����������С������ҵ����Ҫ���㿪С����ס֤����Ȼ���������������ģ�������ľ�ס����Ѿ��ʸ���ˡ����ս����ͨ��������һ�����������Ž��к�ʵ�������Ϻ���ס֤��  
������ӽ����ľ���������ģʽ��һ�����󣬶�Ӧ�����ͬ�Ĵ������ÿ�����������Ҫ�жϵ�ǰ����ļ������ܾ����Ƿ��������ƽ�����һ�����������������ŵ��ǿͻ�ֻҪ׼�����������ϣ������뽻����һ�����ţ�Ȼ���Ż����θ����㵱ǰ������׶ν��д��������û����������Ҳ�������  
�������������������дһ���������롣  

#####��������
```
/**
 * ��װ�������
 * @author kexun
 *
 */
public class ResidencePermitRequest {

	public final static int LEVEL_WUYE = 1;
	public final static int LEVEL_SHEQU = 2;
	public final static int LEVEL_GONGAN = 3;
	
	private int level;
	
	public ResidencePermitRequest() {
	}
	
	public int getLevel() {
		return level;
	}
	
	public void setLevel(int level) {
		this.level = level;
	}
}

/**
 * ��װ���ض���
 * @author kexun
 *
 */
public class ResidencePermitResponse {

	private String message;
	
	public ResidencePermitResponse(String msg) {
		this.message = msg;
	}

	public String getMessage() {
		return message;
	}

}

/**
 * �����������
 * @author kexun
 *
 */
public abstract class ResidencePermitHandler {

	
	private ResidencePermitHandler nextHandler;
	
	/**
	 * ģ�巽�������ݵ�ǰ���󼶱𣬴������󣬻��߽�����ַ�����һ���������
	 * @param request
	 * @return
	 */
	public final ResidencePermitResponse hadleRequest(ResidencePermitRequest request) {
		
		if (request.getLevel() == this.getLevel()) {
			return echo();
		} else {
			if (nextHandler != null) {
				return nextHandler.hadleRequest(request);
			} else {
			    // �����û�ҵ�����Ĭ�ϴ���ʧ��
				return new ResidencePermitResponse("����ʧ��");
			}
		}
		
	}
	
	protected abstract int getLevel();
	
	protected abstract ResidencePermitResponse echo();
	
	public void setNextHandler(ResidencePermitHandler nextHandler) {
		this.nextHandler = nextHandler;
	}
	
}

/**
 * С����ҵ
 * @author kexun
 *
 */
public class WuYe extends ResidencePermitHandler {

	
	@Override
	protected int getLevel() {
		return ResidencePermitRequest.LEVEL_WUYE;
	}

	@Override
	protected ResidencePermitResponse echo() {
		return new ResidencePermitResponse("��ҵ����ͨ��");
	}
}

/**
 * ������������
 * @author kexun
 *
 */
public class SheQu extends ResidencePermitHandler {

	@Override
	protected int getLevel() {
		return ResidencePermitRequest.LEVEL_SHEQU;
	}

	@Override
	protected ResidencePermitResponse echo() {
		return new ResidencePermitResponse("����������������ͨ��");
	}
}

/**
 * ��������
 * @author kexun
 *
 */
public class GongAn extends ResidencePermitHandler {

	@Override
	protected int getLevel() {
		return ResidencePermitRequest.LEVEL_GONGAN;
	}

	@Override
	protected ResidencePermitResponse echo() {
		return new ResidencePermitResponse("��������ͨ��");
	}
}

public class RequestHandler {

	private ResidencePermitHandler shequ;
	private ResidencePermitHandler wuye;
	private ResidencePermitHandler gongan;
	
    // ����һ��������������������������������
	public RequestHandler() {
		
		wuye = new WuYe();
		shequ = new SheQu();
		gongan = new GongAn();
		wuye.setNextHandler(shequ);
		shequ.setNextHandler(gongan);
	}
	
    //��������
	public ResidencePermitResponse handle(ResidencePermitRequest request) {
		return wuye.hadleRequest(request);
	}
}

public class Zhangsan {

	public static void main(String[] args) {

		ResidencePermitRequest request = new ResidencePermitRequest();
		RequestHandler handler = new RequestHandler();
		for (int i=1; i<=6; i++) {
			request.setLevel(i);
			ResidencePermitResponse response = handler.handle(request);
			System.out.println(response.getMessage());
		}
	}
}


//���н��
��ҵ����ͨ��
����������������ͨ��
��������ͨ��
����ʧ��
����ʧ��
����ʧ��
```

#####��ͼ
![1.jpg](./1.jpg "")  

����ͼ�п��Կ�����ÿ������ͳһ����hander�ӿڣ��Ӵ������е�ͷ����ʼ���ο�ʼ�������ݵ�ǰ���󼶱�������󣬻����ƽ�����һ���������γ�һ����������

#####����
������ģʽ��ʹ��������л��ᴦ�����󣬴Ӷ�����������ķ����ߺͽ�����֮�����Ϲ�ϵ������Щ��������һ���������������������ݸ�����ֱ����������Ϊֹ��