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
