### Reflect

框架设计的灵魂

---

#### 1. Concept

反射：将类的各个组成部分封装为其他对象，这就是反射机制

反射可以在程序的运行过程中操作这些对象；反射可以对程序进行解耦，提高程序可扩展性。

#### 2. 获取Class对象的方式

1. Class.forname("全类名"):   将字节码文件加载进内存，返回class对象
   - 多用于配置文件，将类名定义在配置文件中。读取文件，加载类。
2. 类名.class:    通过类名的属性class获取
   - 多用于参数传递
3. 对象.getClass():    getClass()方法在Object类中定义
   - 多用于对象的获取字节码的方式

同一个字节码文件，在一次程序运行过程中，只会被加载一次，无论通过哪种方式获取的Class对象，都是同一个。

#### 3. Class对象功能

大部分都是get功能

- 获取成员变量
  - Field[] getFields()	获取所有public修饰的变量
  - Field[] getField(String name)    获取public修饰的变量
  - Field[] getDeclaredFields()
  - field getDeclaredField(String name)
- 获取构造方法
  - Constructor<?>[] getConstructors()
  - Constructor<T> getConstructor(类<?>... parameterTypes)
  - Constructor<T> getDeclaredConstructor(类<?>... parameterTypes)
  - Constructor<?>[] getDeclaredConstructors()
- 获取成员方法
  - Method[] getMethods()
  - Method getMethod(String name, 类<?>... parameterTypes)
  - Method[] getDeclaredMethodsI()
  - Method getDeclaredMethod(String name, 类<?>... parameterTypes)
- 获取类名 
  - String getName()































