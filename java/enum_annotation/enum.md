# Enum
* 私有化构造器
* private final实例变量
* 实例以','分隔, 以';'结尾
* 必肱在第1行声明枚举类对象

## 例

```java
public enum SeasonEnum {
    SPRING("春天", "春风又绿江南岸"),
    SUMMER("夏天", "映日荷花别样红"),
    AUTUMN("秋天", "秋水共长天一色"),
    WINTER("冬天", "窗含西岭千秋雪");

    private final String seasonName;
    private final String seasonDesc;

    private SeasonEnum(String seasonName, String seasonDesc) {
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }
}
```

## 常用方法
* values() 方法 ：返回枚举类型的对象数组。该方法可以很方便地遍历所有的枚举值。
* valueOf (String str ))：可以把一个字符串转为对应的枚举类对象。要求字符串必须是枚举类对象的“名字”。如不是，会有 运行 时异常：IllegalArgumentException 。
* toString()toString()：返回当前枚举类对象常量的名称

```java
public class EnumTest {
    @Test
    public void t3() {
        SeasonEnum spring = SeasonEnum.valueOf("SPRING");
        System.out.println(spring);
    }

    @Test
    public void t2() {
        SeasonEnum[] values = SeasonEnum.values();
        Arrays.asList(values).forEach(System.out::println);

        System.out.println("------------------------------------");
        //线程状态
        Thread.State[] values1 = Thread.State.values();
        Arrays.asList(values1).forEach(System.out::println);
    }

    @Test
    public void t1() {
        SeasonEnum s1 = SeasonEnum.SPRING;
        System.out.println(s1.getSeasonName());
        System.out.println(s1.getSeasonDesc());
        System.out.println(s1);
        System.out.println(s1.toString());
        System.out.println(SeasonEnum.class.getSuperclass());
    }
}
```

## 实现接口
* 和普通 Java 类一样，枚举类可以实现一个或多个接口
* 若每个枚举值在调用实现的接口方法呈现相同的行为方式，则只要统一实现该方法即可。
* 若需要每个枚举值在调用实现的接口方法呈现出不同的行为方式,则可以让每个枚举值分别来实现该方法

```java
public interface EnumShow {
    public void show();
}

public enum SeasonEnum implements EnumShow {
    SPRING("春天", "春风又绿江南岸"){
        @Override
        public void show() {
            //do something
        }
    },
    SUMMER("夏天", "映日荷花别样红") {
        @Override
        public void show() {
            //do something
        }
    },
    AUTUMN("秋天", "秋水共长天一色") {
        @Override
        public void show() {
            //do something
        }
    },
    WINTER("冬天", "窗含西岭千秋雪") {
        @Override
        public void show() {
            //do something
        }
    };

    private final String seasonName;
    private final String seasonDesc;

    private SeasonEnum(String seasonName, String seasonDesc) {
        this.seasonName = seasonName;
        this.seasonDesc = seasonDesc;
    }

    public String getSeasonName() {
        return seasonName;
    }

    public String getSeasonDesc() {
        return seasonDesc;
    }

//    @Override
//    public void show() {
//        System.out.println("四季");
//    }
}
```