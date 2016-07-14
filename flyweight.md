#享元模式

#####简介：
享元模式的核心理念既是共享对象。何为共享对象呢? 就是让同一个对象可以共享给多个用户使用。从而避免了对象的过多创建，节约了宝贵的资源。既然要实现共享，就必须满足几个条件。  
第一：要求细粒度对象，对象本身不能太复杂，而且正因为对象的粒度细，所以会导致出现大量性质相似的对象，这时就可以使用共享对象了。  
第二：共享状态，何为共享状态？ 就是说一个对象创建了以后就不能被改变。试想我创建了一个对象，被多个用户同时引用，如果这个时候其中一个用户改变了对象的状态，那么势必会影响其他用户。  
在下文中我将分两部分介绍享元模式，前半部分通过设计一个享元模式的demo程序介绍其来龙去脉，第二部分着重介绍享元模式在Integer类中的应用。

#####应用场景
首先我为了举例而举例，捏造一个假想的场景。在一个用户管理系统中，会有一个用户注册模块。我简化了用户的注册信息，假使只需要年龄即可。首先我创建一个Age类，其中包含age属性，map中。代码如下：  
```
public class Age {

	private final int age;
	public Age(int age) {
		this.age = age;
	}

	public int getAge() {
		return age;
	}

}

public class Client {

	public static void main(String[] args) {
		
        // 存放被注册的用户
		Map<String, Age> map = new HashMap<String, Age>();
			// 创建10000个用户
		for (int i = 0; i < 10000; i++) {
        	// 随机创建用户年龄
			int age = (int)(1+Math.random()*(100 - 1));
			map.put("name"+i, new Age(age));
		}
	}

}
```

这段代码的缺点显而易见，每注册一个用户，就需要创建一个Age对象，一旦用户量增大，创建如此多的Age对象，将对内存会是一个非常大的挑战。   
通过对业务的分析可以得出，用户年龄在1-100之间。而相同年龄的Age具有极度的相似性，完全可以重用该对象。这也引出了享元模式的一个重要概念，共享对象。Age对象本身就具有细粒度特性以及共享性。Age只包涵年龄一个属性，并且一旦创建成功，就不能被修改。用享元模式来设计这个功能再合适不过。  

享元模式的具体思路是，共享对象。首先创建一个对象容器，通过容器获取对象，如果该对象不存在，就新创建一个对象，并保存到容器中，下次获取相同性质的对象时，就直接从容器中获取。在该应用场景中，容器中最多只需要包含1-100年龄段的100个Age对象即可。代码如下：  

```
public class FlyWeightFactory {

	private static HashMap<Integer, Age> pool = new HashMap<Integer, Age>();
	
	public static Age getAge(Integer key) {
		
		if (pool.containsKey(key)) {
			return pool.get(key);
		} else {
			Age age = new Age(key);
			pool.put(key, age);
			return age;
		}
	}
}

public class Client {

	public static void main(String[] args) {
    
		Map<String, Age> map = new HashMap<String, Age>();
		for (int i = 0; i < 10000; i++) {
			int age = (int)(1+Math.random()*(100 - 1));
			map.put("name"+i, FlyWeightFactory.getAge(age));
		}
	}
}
```

#####享元模式在Integer中的应用

Integer类是int的包装类，内部封装了int基本类型。Integer非常常用，在进行整型数值计算的过程中都会使用到Integer。因此系统在某些情况下有可能会出现过多的Integer对象。  
那么我们先来分享Integer类，Integer是int基本类型的包装类，对int的扩展，他符合细粒度特性。其次Integer封装的int数值是不可变的。这两个特性正好符合享元模式的特性。毫无疑问我们可以用享元模式来设计Integer类。  

那么问题来了，int数值范围在-2^31 到 2^31-1 之间，既然要共享对象，那是不是要共享所有的数值呢？显然不合理，如果共享所有数值，那将创建2^32个Integer对象。这显然是对内存的一个巨大浪费，得不偿失。那该怎么解决呢？其实在普通的系统开发中，-128 到 127之间的数值出现的频率最高，而过大或过小的数值出现频率较低，对于这些低频数值，只需要在出现时创建即可。  

这种有选择的共享对象方式非常合理，他综合考虑了对象出现的频率以及共享对象的数量，共享过多的不常出现的对象无疑是一中浪费。这也是对享元模式的灵活应用，而不是生搬硬套。  

```
public static Integer valueOf(int i) {
    if (i >= IntegerCache.low && i <= IntegerCache.high)
    	// 如果创建的数值再-128和127直接，就使用共享对象
        return IntegerCache.cache[i + (-IntegerCache.low)];
    return new Integer(i);
}

// 创建-128到127之间的Integer对象，用于共享
private static class IntegerCache {
    static final int low = -128;
    static final int high;
    static final Integer cache[];

    static {
        // high value may be configured by property
        int h = 127;
        String integerCacheHighPropValue =
            sun.misc.VM.getSavedProperty("java.lang.Integer.IntegerCache.high");
        if (integerCacheHighPropValue != null) {
            try {
                int i = parseInt(integerCacheHighPropValue);
                i = Math.max(i, 127);
                // Maximum array size is Integer.MAX_VALUE
                h = Math.min(i, Integer.MAX_VALUE - (-low) -1);
            } catch( NumberFormatException nfe) {
                // If the property cannot be parsed into an int, ignore it.
            }
        }
        high = h;

        cache = new Integer[(high - low) + 1];
        int j = low;
        for(int k = 0; k < cache.length; k++)
            cache[k] = new Integer(j++);

        // range [-128, 127] must be interned (JLS7 5.1.7)
        assert IntegerCache.high >= 127;
    }

    private IntegerCache() {}
}

```
















