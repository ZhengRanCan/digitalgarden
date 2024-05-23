---
{"tags":["Project/AndroidStudio/Kotlin"],"dg-publish":true,"permalink":"/Project/AndroidStudio/Field 反射/","dgPassFrontmatter":true}
---

# 介绍
`Field` 是JAVA反射API的一部分，表示类的属性（字段），通过 `Field` 类，可以在运行时检查和操作类的字段。
## 获取字段信息
通过反射获取类的信息，常用的方法有以下几种
1. `getFields()`: 获取所有公共属性，包括继承的公共属性
2. `getDeclaredFidleds ()`: 获取所有属性，不包括继承的字段
```java
Field[] fields = this.getClass().getDeclaredFields();
```
## 获取属性
1. 获取属性的名称和类型
	1.  `String getName()`：获取字段的名称。
	2. `Class<?> getType()`：获取字段的类型。
2. **访问和修改字段的值**：
	1. `Object get(Object obj)`：返回指定对象上此 `Field` 表示的字段的值。
	2. `void set(Object obj, Object value)`：将指定对象上此 `Field` 对象表示的字段设置为指定的新值。
3. **修改字段的可访问性**：
	1. - `void setAccessible(boolean flag)`：启动或禁止字段的访问权限检查。即使字段是私有的，也可以通过这种方式访问和修改其值。

## 示例
```java
import java.lang.reflect.Field;

public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public static void main(String[] args) {
        Person person = new Person("John", 30);

        // 获取所有声明的字段
        Field[] fields = person.getClass().getDeclaredFields();

        for (Field field : fields) {
            // 输出字段的名称和类型
            System.out.println("Field name: " + field.getName());
            System.out.println("Field type: " + field.getType().getName());

            // 设置字段的可访问性（即使是私有字段）
            field.setAccessible(true);

            try {
                // 获取字段的值
                Object value = field.get(person);
                System.out.println("Original value: " + value);

                // 修改字段的值
                if (field.getType() == String.class) {
                    field.set(person, "Doe");
                } else if (field.getType() == int.class) {
                    field.set(person, 25);
                }

                // 获取修改后的值
                Object newValue = field.get(person);
                System.out.println("New value: " + newValue);

            } catch (IllegalAccessException e) {
                e.printStackTrace();
            }
        }
    }
}

```
### 为什么需要一个对象实例
 当你通过反射获取某个类的字段时，`Field`对象本身代表类中的一个字段，但它并不包含该字段的具体值。具体的值存储在某个对象实例中。因此，`field.get`和`field.set`方法需要一个对象实例作为参数，从而获取或修改该对象实例中的字段值。
# REF:
1. [[Project/ue/UE5U++学习/反射\|反射]]，应该是跟C++概念类似