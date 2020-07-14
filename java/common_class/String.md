# String
字符串,是一个final类,不可变字符序列,字符内容存储在一个字符数组value[]中
* 不可变性
* char[]存储

## 实例化方式
* 字面量
* 构造器new

## 常用方法
...略过

# StringBuffer
* 可变
* 线程安全,效率低
* char[]存储
* 默认capacity为16,不够时会继续扩容,默认扩容为capacity<<1 + 2,并将原有元素复制到新数组中

## 常用方法
...略过

# StringBuilder
* 可变
* 线程不安全,效率高
* jdk5.0新增
* char[]存储

## 常用方法
...略过

> 建议:
> 使用 StringBuffer(int capacity) 或 StringBuilder(int capacity)

# 常见算法
* 将一个字符串进行反转。将字符串中指定部分进行反转。比如“abcdefg”反转为”abfedcg”

```java
public class StringDemo {
    //方式一：转换为char[]
    public String reverse(String str, int startIndex, int endIndex) {

        if (str != null) {
            char[] arr = str.toCharArray();
            for (int x = startIndex, y = endIndex; x < y; x++, y--) {
                char temp = arr[x];
                arr[x] = arr[y];
                arr[y] = temp;
            }

            return new String(arr);
        }
        return null;
    }

    //方式二：使用String的拼接
    public String reverse1(String str, int startIndex, int endIndex) {
        if (str != null) {
            //第1部分
            String reverseStr = str.substring(0, startIndex);
            //第2部分
            for (int i = endIndex; i >= startIndex; i--) {
                reverseStr += str.charAt(i);
            }
            //第3部分
            reverseStr += str.substring(endIndex + 1);

            return reverseStr;

        }
        return null;
    }

    //方式三：使用StringBuffer/StringBuilder替换String
    public String reverse2(String str, int startIndex, int endIndex) {
        if (str != null) {
            StringBuilder builder = new StringBuilder(str.length());

            //第1部分
            builder.append(str.substring(0, startIndex));
            //第2部分
            for (int i = endIndex; i >= startIndex; i--) {

                builder.append(str.charAt(i));
            }
            //第3部分
            builder.append(str.substring(endIndex + 1));

            return builder.toString();
        }
        return null;

    }

    @Test
    public void testReverse() {
        String str = "abcdefg";
        String reverse = reverse1(str, 2, 5);
        System.out.println(reverse);
    }

}
```

* 获取一个字符串在另一个字符串中出现的次数。比如：获取“ab”在 “abkkcadkabkebfkaabkskab” 中出现的次数

```java
public class StringDemo {
    /**
     * 获取subStr在mainStr中出现的次数
     */
    public int getCount(String mainStr, String subStr) {
        int mainLength = mainStr.length();
        int subLength = subStr.length();
        int count = 0;
        int index = 0;
        if (mainLength >= subLength) {
            //方式一：
//            while((index = mainStr.indexOf(subStr)) != -1){
//                count++;
//                mainStr = mainStr.substring(index + subStr.length());
//            }
            //方式二：对方式一的改进
            while ((index = mainStr.indexOf(subStr, index)) != -1) {
                count++;
                index += subLength;
            }

            return count;
        } else {
            return 0;
        }
    }

    @Test
    public void testGetCount() {
        String mainStr = "abkkcadkabkebfkaabkskab";
        String subStr = "ab";
        int count = getCount(mainStr, subStr);
        System.out.println(count);
    }
}
```

* 获取两个字符串中最大相同子串。比如：str1 = "abcwerthelloyuiodefabcdef";str2 = "cvhellobnm" 提示：将短的那个串进行长度依次递减的子串与较长的串比较。

```java
public class StringDemo2 {
    //前提：两个字符串中只有一个最大相同子串
    public String getMaxSameString(String str1, String str2) {
        if (str1 != null && str2 != null) {
            String maxStr = (str1.length() >= str2.length()) ? str1 : str2;
            String minStr = (str1.length() < str2.length()) ? str1 : str2;
            int length = minStr.length();

            for (int i = 0; i < length; i++) {
                for (int x = 0, y = length - i; y <= length; x++, y++) {
                    String subStr = minStr.substring(x, y);
                    if (maxStr.contains(subStr)) {
                        return subStr;
                    }

                }
            }

        }
        return null;
    }

    // 如果存在多个长度相同的最大相同子串
    // 此时先返回String[]，后面可以用集合中的ArrayList替换，较方便
    public String[] getMaxSameString1(String str1, String str2) {
        if (str1 != null && str2 != null) {
            StringBuffer sBuffer = new StringBuffer();
            String maxString = (str1.length() > str2.length()) ? str1 : str2;
            String minString = (str1.length() > str2.length()) ? str2 : str1;

            int len = minString.length();
            for (int i = 0; i < len; i++) {
                for (int x = 0, y = len - i; y <= len; x++, y++) {
                    String subString = minString.substring(x, y);
                    if (maxString.contains(subString)) {
                        sBuffer.append(subString + ",");
                    }
                }
//                System.out.println(sBuffer);
                if (sBuffer.length() != 0) {
                    break;
                }
            }
            String[] split = sBuffer.toString().replaceAll(",$", "").split("\\,");
            return split;
        }

        return null;
    }

    @Test
    public void testGetMaxSameString() {
        String str1 = "abcwerthello1yuiodefabcdef";
        String str2 = "cvhello1bnmabcdef";
        String[] maxSameStrings = getMaxSameString1(str1, str2);
        System.out.println(Arrays.toString(maxSameStrings));

    }

}
```