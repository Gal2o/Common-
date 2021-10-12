# 객체지향 프로그래밍 5대 원칙

  - ### SRP : 단일 책임 원칙 (Single Responsibilty Principle) <br><br> <img src="https://user-images.githubusercontent.com/35948339/136965399-4c47ea14-94d9-460f-90ce-7db9f73304e5.png" width=600>
    - #### 클래스는 오직 한 가지 책임만 져야한다 (🔵 함수, 메서드 등에도 적용이 가능)
    - #### 🟥 만약 클래스가 여러 가지 책임을 진다면, 기능을 추가 하다 보면, 나도 모르게 버그가 생길 수 있다.
    ``` java
      // 나쁜 예시
      public class Job {
        String position;
        
        public String Backend() {
          // 백엔드 개발
          this.position = "Backend";
          return this.position;
        }
        
        public String Frontend() {
          // 프론트엔드 개발
          this.position = "Frontend";
          return this.position;
        }
        
        public String DevObs {
          // dev-obs 개발
          this.position = "Dev-Obs"
          return this.position;
        }
      }
      
      Job job = new Job();
      
      job.Backend();
      job.Frontend();
      job.DevObs();
    ```
    - #### 🟢 역할 별로 클래스를 분리하여 사용합니다
    ``` java
      // 좋은 예시
       public class Job {
        String position;
        
        // 자기 할 일만 한다
        public doMyJob() {
            if (this.position != null)
              return this.position;
            else
              return "I don't have any Job";
        }
       }
       
       public class Backend() extends Job {
          constructor() {
            super();
            this.position = "Backend";
          }
       }
       
       public class Frontend() extends Job {
          constructor() {
            super();
            this.position = "Frontend";
          }
       }
       
       public class DevObs extends Job {
          constructor() {
            super();
            this.position = "DevObs";
          }
        }
        
        Backend backend = new Backend();
        backend.doMyJob();
        
        Frontend frontend = new Frontend();
        frontend.doMyJob();
        
        DevObs devObs = new DevObs();
        devObs.doMyJob();
    ```
  --------
  - ### OCP : 개방-폐쇄 원칙 (Open-Closed Principle) <br><br> <img src="https://user-images.githubusercontent.com/35948339/136973491-fcb85e8b-5b01-4017-bcfb-e41a492ca9e6.png" width=600>
    - #### 클래스는 확장에는 열려 있어야 하고, 변경에는 폐쇄적이어야 한다.
    - #### 클래스의 현재 코드를 변경하는 것은 해당 클래스를 사용하고 있는 모든 시스템에 영향을 주게 된다.
    - #### 만약, 클래스에 요구사항이 생기게 된다면 새로운 함수를 추가하는 것이 가장 이상적이다
    ``` java
      // 정상적인 TV 이름을 검증하는 클래스
      public class Validation {
          public void validateTV(Production production) throws IllegalArgumentException {
              if (production.getNameLength() < 3) {
                  throw new IllegalAccessException("TV 이름은 3글자 이상이어야합니다.");
              }
          }
      }
    ```
    - #### 만약, 위 클래스에 에어컨 또는 냉장고를 검증하는 메소드가 추가 된다면?
    ``` java
      // 나쁜 예제
      public class Validation {
          public void validateProduction(Production production) throws IllegalArgumentException {
              if (production.getOption() == "TV" && production.getNameLength() < 3) {
              
                  throw new IllegalAccessException("TV 이름은 3글자 이상이어야합니다.");
  
              } else if(production.getOption() == "Air" && production.getNameLength() < 7) {
              
                  throw new IllegalAccessException("에어컨 이름은 7글자 이상이어야합니다.");
                  
              } else if(production.getOption() == "refri" && production.getNameLength() < 5) {
              
                  throw new IllegalAccessException("냉장고 이름은 5글자 이상이어야합니다.");
                  
              }
          }
      }
    ```
    - #### ❌ 이런식으로 분기가 계속 늘어날 것이다.
    --------
    - #### OCP를 적용하게 된다면,
    ``` java
      // 클래스가 아닌 인터페이스 사용 !!
      public interface Validation {
          
          boolean selection(Production production);
          
          void validate(Production production) throws IllegalArgumentException;
          
      }
    ```
    ``` java
      public class tvValidator implements Validation {
      
          @Override
          public boolean selection(Production production) {
              return production.getOption() == "TV"
          }
          
          @Override
          public void validate(Production production) throws IllegalArgumentException {
              if (production.getNameLength() < 3) {
                  throw new IllegalAccessException("TV 이름은 3글자 이상이어야합니다.");
              }
          }
      }
      
      public class airValidator implements Validation {
      
          @Override
          public boolean selection(Production production) {
              return production.getOption() == "Air"
          }
          
          @Override
          public void validate(Production production) throws IllegalArgumentException {
              if (production.getNameLength() < 7) {
                  throw new IllegalAccessException("에어컨 이름은 7글자 이상이어야합니다.");
              }
          }
      }
      
      public class refriValidator implements Validation {
      
          @Override
          public boolean selection(Production production) {
              return production.getOption() == "refri"
          }
          
          @Override
          public void validate(Production production) throws IllegalArgumentException {
              if (production.getNameLength() < 5) {
                  throw new IllegalAccessException("냉장고 이름은 5글자 이상이어야합니다.");
              }
          }
      }
    ```
    - #### 이렇게 변경 없이 확장 가능한 구조를 만들 수 있다.

