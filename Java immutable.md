- ## Mutable vs Immutable
  - #### Mutable : 변할 수 있는, 잘 변하는
  - #### Immutable : `변경할 수 없는`, 불변의
-------

- ## Java는 mutable일까 immutable일까?
  - #### 정답은 `immutable`이다.
  - #### 자바 같은 경우, 레퍼런스 타입의 객체는 immutable이다.
  - #### heap 영역에 객체의 메모리가 할당이 되고, 만약 사용되지 않으면 gc의 대상이 된다.
------
  - ### immutable 객체의 특징
    - ### 장점
      - #### 1️⃣ 객체에 대한 신뢰도가 높다. (객체가 생성되고 그 값이 변하지 않기 때문에)
      - #### 2️⃣ 생성자, 객체에 접근하는 메소드에 대해 `방어적 복사`가 필요없다.
        - #### 방어적 복사란 ?
        ``` java
          public class Names {
             private final List<Name> names;

             public Names(List<Name> names) {
                 this.names = names;
             }
          }
          
          void main(String[] args) {
              List<Name> originalNames = new ArrayList<>();
              originalNames.add(new Name("Person1"));
              originalNames.add(new Name("Person2"));

              Names NewNames = new Names(originalNames); // NewNames : Person1, Person2
              originalNames.add(new Name("Person3")); // NewNames : Person1, Person2, Person3
          }
        ```
        - #### originNames와 NewNames가 주소를 공유하므로, 변경이 모두에게 적용된다.
        -------
        ``` java
          public class Names {
              private final List<Name> names;
              
              public Names(List<Name> names) {
                  this.names = new ArrayList<>(names);
              }
          }
        ```
        - #### 생성자의 매개변수에 대한 새로운 주소의 복사본을 만들어 다른 주소를 갖게 하여 방어적 복사를 한다면 해결된다.
        -------
      - #### 3️⃣ 데이터 동기화가 필요 없으므로 멀티스레드 환경에서 객체가 공유 가능하다. (Thread-safe)
      - #### 4️⃣ 캐싱을 가능하게 하여 중복 값일 경우 재사용이 가능하다. 만약 중복이 적고 변경이 많은 경우 StringBuilder(mutable)을 사용하여 캐시 없이 관리할 수 있다.
        - #### String은 값을 StringPool에 넣어두고 같은 값을 할당 받을 경우, StringPool에 있는 값을 참조하게 만들어서 캐싱한다.
    ------
    - ### 단점
      - #### 객체가 가지는 값마다 새로운 객체가 필요하므로, 메모리 누수로 인한 성능 저하가 발생할 가능성이 있다.
--------
- #### 대표적인 Immutable인 String
  - #### Stirng에서 제공하는 `concat`메소드는 새로운 객체를 만들어 그 객체에 합친다.
  ``` java
    String sample = "Hello";
    String result = sample.concat("World");
    
    // 결과
    HelloWorld
  ```
  |주소|값|
  |:--:|:--:|
  |1000|Hello|
  |2000|HelloWorld|
  -----
  - #### 여기서 중요한 점은 concat은 null 값을 가진 객체에 사용하면 NPE가 발생한다.
  ``` java
    String sample = null;
    String result = sample.concat("World");
    
    // 결과
    java.lang.NullPointerException
  ```
  -----
- #### 대표적인 Mutable인 StringBuffer
  - #### StringBuilder는 객체를 변경할 수 있는 mutable 객체이다.
  ``` java
    StringBuilder sample = null;
    StringBuilder sample2 = "Hello";
    
    sample.append("World");
    sample2.append("World");
    
    // 결과
    nullWorld
    HelloWorld
  ```
  |주소|값|
  |:--:|:--:|
  |1000|nullWorld|
  |2000|HelloWorld|
  --------
  - #### `concat`과는 달리 StringBuilder는 null을 초기화해도 `appendNull()`이라는 메소드가 NPE를 방지해준다.
  ``` java
    public AbstractStringBuilder append(String str) {
      if (str == null)
          return appendNull();
      ...
    }

    private AbstractStringBuilder appendNull() {
        int c = count;
        ensureCapacityInternal(c + 4);
        final char[] value = this.value;
        value[c++] = 'n';
        value[c++] = 'u';
        value[c++] = 'l';
        value[c++] = 'l';
        count = c;
        return this;
    }
  ```
  ------
  - #### immutable class 만들어보기
    - #### 1️⃣ 멤버 변수를 final로 선언
    - #### 2️⃣ 접근 메서드를 구현하지 않기 (Setter)
    ``` java
      public class ImmutableString {
      
          private final String name;

          ImmutableString(String name){
              this.name = name;
          }

          @Override
          public String toString(){
              return this.name;
          }
      }
      
      String name = "Hello";
      ImmutableString immutableString = new ImmutableString(name);
      name.concat("World");
      System.out.println(immutableString);
      System.out.println(name);
      
      // 결과
      Hello (immutable 객체)
      HelloWolrd (처음 name과는 새로운 주소의 객체에 담겨짐)
    ```
  -------
  - #### JDK 1.5 이전 버전은 immutable 객체의 많은 잘못된 연산으로 인해 성능 이슈가 많아져, <br><br> JDK 1.5 이상 버전 부터는 String 객체를 선언하여도 StringBuilder로 치환시켜 컴파일 하도록 변경이 되었다.
