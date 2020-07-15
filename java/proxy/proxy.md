# 动态代理
### 代理设计模式原理  
使用一个代理将对象包装起来, 然后用该代理对象取代原始对象。任何对原始对象的调用都要通过代理。代理对象决定是否以及何时将方法调用转到原始对象上

### java动态代理api
Proxy ：专门完成代理的操作类，是所有动态代理类的父类。通过此类为一个或多个接口动态地生成实现类。

### 静态代理示例
```java
/**
 * 静态代理
 * 特点: 代理类和被代理类在编译期间就确定
 */
public class StaticProxyTest {
    public static void main(String[] args) {
        //创建被代理类对象
        AppleFactory apple = new AppleFactory();
        //创建代理类对象
        ProxyFoodFactory proxyFoodFactory = new ProxyFoodFactory(apple);

        proxyFoodFactory.getFood();
    }
}

//代理类
class ProxyFoodFactory implements FoodFactory {
    private FoodFactory factory;//用被代理类对象进行实例化

    public ProxyFoodFactory(FoodFactory factory) {
        this.factory = factory;
    }

    @Override
    public void getFood() {
        System.out.println("代理工厂初始化");
        factory.getFood();
        System.out.println("代理工厂结束");
    }
}
//被代理类
class AppleFactory implements FoodFactory{

    @Override
    public void getFood() {
        System.out.println("买一箱苹果");
    }
}

interface FoodFactory {
    void getFood();
}
```

### 动态代理示例
```java
/**
 * 动态代理
 */
public class ProxyTest {
    public static void main(String[] args) {
        Male male = new Male();
        //代理类对象
        People proxyInstance = (People) ProxyFactory.getProxyInstance(male);

        //当通过代理类对象调用方法时,会自动调用被代理类中同名的方法
        String type = proxyInstance.getType();
        System.out.println(type);
        proxyInstance.like("女人");
    }

}

class Util{
    public static void method1(){
        System.out.println("-----------通用方法1------------");
    }

    public static void method2(){
        System.out.println("-----------通用方法2------------");
    }
}

class ProxyFactory{
    //通过调用此方法,返回一个代理类对象
    public static Object getProxyInstance(Object obj){//obj为被代理类对象
        MyInvocationHandler myInvocationHandler =  new MyInvocationHandler();
        myInvocationHandler.bind(obj);
        return  Proxy.newProxyInstance(obj.getClass().getClassLoader(), obj.getClass().getInterfaces(),myInvocationHandler);
    }
}

class MyInvocationHandler implements InvocationHandler{
    private  Object obj;//需要使用被代理类的对象赋值

    public void bind(Object obj){
        this.obj=obj;
    }

    //当我们通过代理类对象调用方法a时, 就会自动调用此方法: invoke()
    //将被代理类要执行的方法a的功能就声明在invoke()中
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Util.method1();//切面1
        //method为代理类对象调用的方法,此方法也就作为了被代理类对象要调用的方法
        //obj:被代理类对象
        Object invoke = method.invoke(obj, args);

        Util.method2();//切面2
        //上述方法的返回值作为invoke()的返回值
        return invoke;
    }
}

//被代理类
class Male implements People {
    @Override
    public String getType() {
        return "男性";
    }

    @Override
    public void like(String thing) {
        System.out.println("男性喜欢" + thing);
    }
}

//被代理类
class Female implements People {
    @Override
    public String getType() {
        return "女性";
    }

    @Override
    public void like(String thing) {
        System.out.println("女性喜欢" + thing);
    }
}

interface People {
    String getType();
    void like(String thing);
}
```
