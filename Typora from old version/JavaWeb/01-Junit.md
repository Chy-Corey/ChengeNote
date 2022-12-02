### Junit

---

#### 1. 测试分类

- 黑盒测试：不需要写代码，给定不同参数，看程序是否能输出期望值，不需要写代码。
- 白盒测试：需要关注程序的具体流程，需要写一些代码。

#### 2. Junit 使用

`Junit` 属于白盒测试。

最原始的测试，是创建一个新的测试类，然后建立主函数，调用被测试的类，运行函数查看是否结果正确。

```java
// 原始测试代码
package com.chy.junit;

public class CalculatorTest {
	public static void main(String[] args) {
		Calculator container = new Calculator();
		int addResult = container.add(2, 3);
		int subResult = container.sub(2, 3);
		System.out.println(addResult);
		System.out.println(subResult);
	}
}
// 被测试的类代码
package com.chy.junit;

public class Calculator {
	/**
	 * 加法
	 * @param a
	 * @param b
	 * @return
	 */
	public int add(int a, int b) {
		return a + b;
	}
	
	/**
	 * 减法
	 * @param a
	 * @param b
	 * @return
	 */
	public int sub(int a, int b) {
		return a - b;
	}
}
```

##### 使用步骤

1. 定义一个测试类

   测试类名：被测试的类名+Test

2. 定义测试方法，且可以独立运行

   - 方法名：test+被测试的方法名

   - 返回值：void

   - 参数列表：空参

3. 给方法添加标签`@Test`

4. 导入Test依赖包

判定结果：红色失败，绿色正确。一般用断言来处理结果。

```java
// 测试代码
package com.chy.junit;

import org.junit.Assert;
import org.junit.Test;

public class CalculatorTest {
	@Test
	public void testAdd() {
		Calculator c = new Calculator();
		int resultAdd = c.add(2, 1);
		//断言结果为3
		Assert.assertEquals(3, resultAdd);
	}
	
	@Test
	public void testSub() {
		Calculator c = new Calculator();
		int resultSub = c.sub(2, 0);
		Assert.assertEquals(1, resultSub);
	}
}
```

#### 3. @Before & @ After

这两个标签用于资源申请和资源释放。在所有测试函数执行的前后都会执行这两个标签。如果一个测试类里有两个测试函数，那么Before和After就会各执行两次。也就是每执行一个测试函数，执行一次Before和After。



#### 4. 一些遇到的问题

导入Test标签依赖以后，程序一直报错，此时删去**module - info.java**即可。
