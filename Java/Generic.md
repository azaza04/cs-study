# Generic


# 자바의 제네릭(Generic in Java)

> 💡클래스 내부에서 사용할 데이터 타입을 외부에서 지정하는 기법

**제네릭(Generic)** 은 자바에서 **컴파일 타임에 타입을 미리 지정할 수 있는 기능**으로, **다양한 데이터 타입을 처리할 수 있는 클래스나 메서드를 정의**하는 데 사용한다. 이를 통해 **타입 안정성**을 확보하고, **재사용 가능한 코드**를 작성할 수 있다. 자바의 제네릭은 5.0 버전에서 도입되었다.

## 제네릭의 주요 개념

1. **타입 매개변수(Type Parameter)**:
   - 제네릭 클래스나 메서드에서 사용하는 **타입 변수**를 의미한다.
   - 일반적으로 `T`, `E`, `K`, `V` 등의 문자를 사용하여 선언하며, 특정 타입이 아니라 타입을 대표하는 매개변수로 취급된다.
   
   ```java
   public class Box<T> {
       private T item;

       public void set(T item) {
           this.item = item;
       }

       public T get() {
           return item;
       }
   }
   ```

2. **타입 안전성(Type Safety)**:
   - 제네릭을 사용하면 **컴파일 시점에 타입을 체크**할 수 있어, **런타임 타입 오류를 방지**할 수 있다.
   - 타입을 명시하지 않는 경우와 달리, 제네릭을 사용하면 잘못된 타입을 할당하는 것을 방지할 수 있다.

3. **재사용성(Reusability)**:
   - 제네릭을 사용하면 **동일한 클래스나 메서드를 여러 타입에 대해 재사용**할 수 있다. 예를 들어, `Box<Integer>`와 `Box<String>` 모두 동일한 `Box` 클래스를 사용하지만 서로 다른 타입을 처리할 수 있다.

4. **제네릭 클래스(Generic Class)**:
   - 타입 매개변수를 사용하는 클래스이다. 클래스 선언부에서 타입 매개변수를 명시하고, 이를 클래스 내부에서 사용한다.

   ```java
   public class Box<T> {
       private T item;

       public void set(T item) {
           this.item = item;
       }

       public T get() {
           return item;
       }
   }
   ```

   사용 예시:

   ```java
   Box<String> stringBox = new Box<>();
   stringBox.set("Hello");

   Box<Integer> integerBox = new Box<>();
   integerBox.set(123);
   ```

5. **제네릭 메서드(Generic Method)**:
   - 메서드에서만 타입 매개변수를 사용할 수 있으며, **메서드의 선언부**에 `<T>`와 같은 타입 매개변수를 명시한다.
   
   ```java
   public <T> void printArray(T[] array) {
       for (T element : array) {
           System.out.println(element);
       }
   }
   ```

   사용 예시:

   ```java
   Integer[] intArray = {1, 2, 3};
   String[] strArray = {"A", "B", "C"};

   printArray(intArray);
   printArray(strArray);
   ```

6. **제네릭 인터페이스(Generic Interface)**:
   - 인터페이스에서도 제네릭을 사용할 수 있다. 제네릭 인터페이스는 타입에 의존하지 않는 메서드들을 선언할 수 있다.

   ```java
   public interface Container<T> {
       void add(T item);
       T get();
   }
   ```

7. **바운디드 타입 매개변수(Bounded Type Parameter)**:
   - 특정 타입만 허용하도록 **제한(제약)을 거는 제네릭**이다. 클래스나 인터페이스를 상속받은 타입만 허용할 수 있다.
   
   ```java
   public <T extends Number> void printNumbers(T[] numbers) {
       for (T number : numbers) {
           System.out.println(number);
       }
   }
   ```

   여기서 `T`는 `Number` 클래스 또는 `Number`를 상속받은 타입만 허용된다 (`Integer`, `Double`, `Float` 등).

8. **와일드카드(Wildcard)**:
   - `?`를 사용하여 **제네릭 타입에 대한 유연한 매개변수**를 지정할 수 있다. 와일드카드는 **타입 매개변수가 무엇인지 모를 때** 사용된다.
   
   ```java
   public void printList(List<?> list) {
       for (Object element : list) {
           System.out.println(element);
       }
   }
   ```

   **상한 경계 와일드카드(Bounded Wildcards)**:
   - `? extends T`는 **T와 그 하위 타입**만 허용한다.
   
   ```java
   public void printNumbers(List<? extends Number> list) {
       for (Number num : list) {
           System.out.println(num);
       }
   }
   ```

   **하한 경계 와일드카드(Lower Bounded Wildcards)**:
   - `? super T`는 **T와 그 상위 타입**만 허용한다.
   
   ```java
   public void addNumbers(List<? super Integer> list) {
       list.add(1);
       list.add(2);
   }
   ```

## 제네릭의 제한사항

1. **프리미티브 타입 사용 불가**:
   - 제네릭은 **프리미티브 타입**(`int`, `char`, `double` 등)을 사용할 수 없다. 대신 **래퍼 클래스**(`Integer`, `Character`, `Double` 등)를 사용해야 한다.
   
   ```java
   // 오류: List<int>는 허용되지 않음
   List<Integer> list = new ArrayList<>();
   ```

2. **런타임 타입 소거(Type Erasure)**:
   - 자바의 제네릭은 **런타임에 타입 정보를 제거**하는 방식으로 동작한다. 즉, **컴파일 이후에는 제네릭 타입 정보가 소거**되어 실제로는 **Object 타입**으로 변환된다. 이는 **호환성**을 위한 것으로, 제네릭 도입 이전의 코드와의 호환성을 유지하기 위함이다.

   ```java
   List<String> stringList = new ArrayList<>();
   List<Integer> intList = new ArrayList<>();
   
   // 런타임에는 둘 다 List로 간주됨
   if (stringList.getClass() == intList.getClass()) {
       System.out.println("Same class");
   }
   ```

3. **정적 멤버에 제네릭 사용 불가**:
   - 제네릭 타입 매개변수는 **인스턴스 레벨에서만 사용**되므로, **정적(static) 멤버**에서는 사용할 수 없다.

   ```java
   public class MyClass<T> {
       // 오류: 제네릭 타입 T는 정적 변수에서 사용할 수 없음
       // private static T value;

       public static void print(T item) { // 오류 발생
           System.out.println(item);
       }
   }
   ```

## 제네릭의 장점

1. **타입 안전성**: 잘못된 타입이 할당되는 것을 방지하고, 컴파일 시점에 오류를 검출할 수 있다.
2. **재사용성**: 다양한 타입에 대해 하나의 클래스나 메서드를 재사용할 수 있다.
3. **가독성 및 유지보수성**: 제네릭을 통해 코드의 의도를 명확하게 전달할 수 있으며, 코드를 쉽게 유지보수할 수 있다.

## 제네릭의 사용 예시

```java
import java.util.ArrayList;
import java.util.List;

public class GenericExample {
    public static void main(String[] args) {
        // 제네릭을 사용한 List
        List<String> stringList = new ArrayList<>();
        stringList.add("Hello");
        stringList.add("World");

        for (String str : stringList) {
            System.out.println(str);
        }

        // 제네릭 메서드 호출
        Integer[] intArray = {1, 2, 3};
        printArray(intArray);
    }

    // 제네릭 메서드
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.println(element);
        }
    }
}
```