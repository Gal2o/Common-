``` java
  class Dog {
    private String name;
    
    public Dog(String name) {
      this.name = name;
    }
    
    public String getName() {
      return name;
    }
    
    public void setName(String name) {
      this.name = name;
    }
  }
  
  public class Test { 
    public static void main(String[] args) { 
      int x = 10; 
      int[] y = {2, 3, 4}; 
      
      Dog dog1 = new Dog("강아지1");    
      Dog dog2 = new Dog("강아지2"); 
      
      // 함수 실행       
      foo(x, array, dog1, dog2); 
      
      System.out.println("x = " + x);
      System.out.println("y = " + y[0]);
      System.out.println("dog1.name = " + dog1.getName());
      System.out.println("dog2.name = " + dog2.getName()); 
      
    } 
    
    // foo 함수
    public static void foo(int x, int[] y, Dog dog1, Dog dog2) { 
      x++; 
      y[0]++; 
      dog1 = new Dog("이름 바뀐 강아지1"); 
      dog2.setName("이름 바뀐 강아지2"); 
    } 
  }
```
- #### 어떠한 결과가 나올까 ❓
```
  // 결과
  x = 10
  y = 3
  강아지1
  이름 바뀐 강아지2
```
- #### 자바는 Pass By Value 방식을 사용하여 위와 같은 결과가 나온다.
---------
- #### 자바에서는 Primitive Type의 메모리는 `스택`에 존재한다
``` java
  // Primitive 변수 할당
  public void test() {
    int x = 3;
    double y = 10.32322f;
    boolean flag = truel
  }
```
- #### <img src="https://user-images.githubusercontent.com/35948339/137704508-9a14208b-83a6-4ac4-91f7-524c65cff344.png" width=600> <br><br> 변수에 메모리를 할당하면 이렇게 할당이 된다.
- #### 여기에 참조타입인 String을 선언을 한다면 ❓
``` java
  // Primitive 변수 할당
  public void test() {
    int x = 3;
    double y = 10.32322f;
    boolean flag = truel
    
    // Object or Reference Type
    String name = "Mac"
  }
```
- #### <img src="https://user-images.githubusercontent.com/35948339/137705376-d022d7c7-715b-4e67-8834-d79906be3942.png" width=600> <br><br> Object의 메모리 할당은 스택에 주소 값을 저장, 그 주소는 힙에 있는 어느 메모리를 가르키게 된다.
- #### 여기에 추가로 배열을 선언 한다면 ❓
``` java
  // Primitive 변수 할당
  public void test() {
    int x = 3;
    double y = 10.32322f;
    boolean flag = truel
    
    // Object or Reference Type
    String name = "Mac"
  
    // array or Referecne Type
    Stirng[] names = new String[3];
    names[0] = "i";
    names[1] = "am";
    names[2] = "Mac";
  }
```
- #### <img src="https://user-images.githubusercontent.com/35948339/137705929-c6c0fd89-2ec9-4d64-bac3-de9fb08f0310.png" width=600> <br><br> 배열도 마찬가지로 스택에는 주소 값이 저장, 그 주소를 따라가면 배열의 각 주소는 힙을 가르키게 된다.
--------
- ### 문제 풀이
  - #### 1️⃣ 처음 변수 선언 및 메모리 할당을 하면,
  ``` java
      int x = 10; 
      int[] y = {2, 3, 4}; 
      
      Dog dog1 = new Dog("강아지1");    
      Dog dog2 = new Dog("강아지2"); 
  ```
  - <img src="https://user-images.githubusercontent.com/35948339/137706741-6335ad21-c1b8-444f-ab64-c829c4764037.png" width=600> <br><br> 스택과 힙의 상황은 이런식으로 그려진다.
  - #### 2️⃣ foo 호출 및 실행
    - #### foo 함수 안에서 int x의 값이 1 증가하게 되고, 이 증가된 x는 foo 함수 내에 있는 새로운 int x의 증가가 된다.


