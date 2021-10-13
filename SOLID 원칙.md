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
    - #### 🟢 역할 별로 클래스를 분리하여 독립적으로 모듈화가 가능해진다
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
      // 나쁜 예시
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
      // 좋은 예시
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
  --------
  - ### LSP : 리스코프 치환 원칙 (Liskov Substitution Principle) <br><br> <img src="https://user-images.githubusercontent.com/35948339/136980261-3b794bc7-9bd2-4769-abb2-1852ae37ad27.png" width=600>
    - #### `상위 타입의 객체`를 `하위 타입의 객체로 치환해도` 상위 타입을 사용하는 메소드는 정상적으로 작동해야 한다
    - #### 대표적인 예제인 직사각형/정사각형 예제
    ``` java
      // 나쁜 예시
      // 부모인 직사각형
      class Rectangle {
        protected int width = -1;
        protected int height = -1;

        public get width() {
          return this.width;
        }
        public set width(int w) {
          this.width = w;
        }

        public get height() {
          return this.height;
        }
        public set height(int h) {
          this.height = h;
        }

        public get area() {
          return this.width * this.height;
        }
      }

      // 자식인 정사각형
      class Square extends Rectangle {
        // 정사각형은 가로, 세로가 같으므로 재정의
        public set width(int w) {
          this.width = w;
          this.height = w;
        }

        public set height(int h) {
          this.width = h;
          this.height = h;
        }
      }
    ```
    ``` java
      Rectangle rectangle = new Rectangle();
      rectangle.width = 3;
      rectangle.height = 4;
      
      System.out.println(rectangle.area() == 12) => true
      
      Rectangle square = new Square();
      square.width = 3;
      square.height = 4;
      
      System.out.println(square.area() == 12) => false !!
    ```
    - #### 위 결과처럼, 논리상으로 `정사각형은 직사각형`이지만, `직사각형은 정사각형`이 아니다 <br><br> 를 코드로 상속 관계를 반영할 수 있지만 결과가 다르므로, `상속 관계로 존재할 수 없다`를 알 수 있다.
    --------
    ``` java
      // 좋은 예시
      // 더 포괄적인 Shape를 상속 받도록 리팩토링
      interface Shape {
         int area;
      }

      class Rectangle implements Shape {
        constructor(int number, int height) { }

        public get area() {
          return this.width * this.height;
        }
      }

      class Square implements Shape {
        constructor(int width) { }

        public get area() {
          return this.width * this.width;
        }
      }
    ```
    ``` java
      Shape rectangle = new Rectangle(3, 4);
      System.out.println(rectangle.area()) ==> 12 
      
      Shape square = new Square(4);
      System.out.println(square.area()) ==> 16 
    ```
    - #### ⭐상위 타입의 객체를 하위 타입에서도 그대로 지킬 수 있을 때, 상속을 해야 한다 <br><br> ‼ LSP가 지켜지지 않으면 개방 폐쇄 원칙도 위반하게 되므로, 상속을 잘 정의해야 한다
  -------
  - ### ISP : 인터페이스 분리 원칙 (Interface Segregation Principle) <br><br> <img src="https://user-images.githubusercontent.com/35948339/137154749-538e51a4-13db-47a5-9c30-1c854d7a293a.png" width=600>
    - #### 클라이언트는 `사용하지 않는 메서드`에 의존적이지 않아야 한다. <br><br> 즉, `일부 기능만을 사용`하는 클라이언트는 `사용하지 않는 기능은 알 필요가 없다`.
    - #### 대표적인 예인, 복합기를 예를 들면 다음과 같다
    ``` java
      // 나쁜 예시
      // 복합기 인터페이스
      public interface AllInOneDevice {
          void print();

          void copy();

          void fax();
      }
    ```
    ``` java
      // 복합기의 모든 기능을 가진 MFP
      public class MultiFunctionPrinter implements AllInOneDevice {
          @Override
          public void print() {
              System.out.println("print");
          }

          @Override
          public void copy() {
              System.out.println("copy");
          }

          @Override
          public void fax() {
              System.out.println("fax");
          }
      }
    ```
    - #### 하지만, 인쇄 기능만 필요한 프린터를 위 인터페이스를 이용하여 구현한다면❓
    ``` java
      public class Printer implements AllInOneDevice {
          @Override
          public void print() {
              System.out.println("print");
          }

          @Override
          public void copy() {
              throw new UnsupportedOperationException();
          }

          @Override
          public void fax() {
              throw new UnsupportedOperationException();
          }
      }
    ```
    - #### 인쇄 기능은 적절하게 오버라이딩이 되었지만, 나머지 기능은 예외를 발생 시킨다 <br><br> ‼ 이 경우, 클라이언트는 복사 or 팩스 기능이 구현 되어 있는지 모르기 때문에 예상치 못한 버그가 발생 할 수 있다 <br><br> ❌ 위와 같은 경우는 SRP도 위반하는 경우가 될 수도 있다
    ---------
    - 🟢 해결책은 여러개의 인터페이스로 나누는 것이다
    ``` java
      // 좋은 예시
      public interface PrinterDevice {
          void print();
      }

      public interface CopyDevice {
          void copy();
      }

      public interface FaxDevice {
          void fax();
      }
    ```
    - #### 그 후, 필요한 기능을 implements 받으면 해결 할 수 있다.
    ``` java
      public class MultiFunctionPrinter implements PrinterDevice, CopyDevice, FaxDevice {
      }
      
      public class Printer implements PrinterDevice {
      }
    ```
  --------
  - ### DIP : 의존관계 역전 법칙 (Dependency Inversion Principle) <br><br> <img src="https://user-images.githubusercontent.com/35948339/137167155-36673fef-46af-4022-a9fc-5032e01acd6d.png" width=600>
    - #### `추상화에 의존해야지`, `구체화에 의존하면 안된다.`
    - #### 🟥 DIP가 지켜지지 않은 예시를 보면,
    ``` java
      // 나쁜 예시
      public class Validator {
          public void validate(Production production) {
              //validate
          }
      }
      
      public class ProductionService {

          private final Validator validator;
          
          public ProductionService(Validator validator) {
              this.validator = validator;
          }

          public void validate(Production production) {
              validator.validate(production);
          }
      }
    ```
    - #### 이 코드에 새로운 Validator가 추가 된다면 ❓
    ``` java
       // **새로운 Validator**
       public class NewValidator {
          public void validate(Production production) {
              //validate
          }
      }
      
      // 서비스가 복잡해지기 시작한다
      public class ProductionService {

          private final Validator validator;
          private final NewValidator newValidator;
          
          public ProductionService(Validator validator, NewValidator newValidator) {
              this.validator = validator;
              this.newValidator = newValidator;
          }

          public void validate(Production production) {
              if ( ... ) {
                  validator.validate(production);
              } else if ( ... ) {
                  newValidator.validate(production);
              }
          }

      }
    ```
    - #### <img src="https://user-images.githubusercontent.com/35948339/137172123-2ab6d47c-41ef-4a41-bc9a-6365f581d5ca.png" width=500> <br> 🔼 위 그림처럼, 서비스가 기능에 의존적인 상황이 발생하게 된다
    --------
    - #### OCP의 예제 처럼, if..else if 가 반복되는 구조를 만들고 싶지 않다면 <br><br> 🟢 인터페이스를 통해 의존성을 가지는 것을 줄일 수 있다.
    ``` java
      // 좋은 예시
      // 통합 인터페이스 생성
      public interface Validator {
          void validate(Production production);
      }

      public class DefaultValidator implements Validator {
          @Override
          public void validate(Production production) {
              //validate
          }
      }

      public class NewValidator implements Validator {
          @Override
          public void validate(Production production) {
              //validate
          }
      }

      public class ProductionService {

          private final Validator validator;

          public ProductionService(Validator validator) {
              this.validator = validator;
          }

          public void validate(Production production) {
              validator.validate(production);
          }
      }
      
      // 의존성을 반대로 주입시키기
      Valid valid = new DefaultValidator();
      Valid valid2 = new NewValidator();
    ```
    - #### <img src="https://user-images.githubusercontent.com/35948339/137173088-fdd9b24c-f1dd-438e-9ee4-8f68cd5ad015.png" width=500> <br> 🔼 위 그림처럼, 의존성의 방향을 `역전` 시키게 만들어서 OCP의 이점 (확장에는 자유, 변경에 닫힌)을 얻을 수 있다
    - #### 🟢 한 클래스에 의존성이 지나치게 많다면, DIP를 준수하여 지나친 의존성을 분산 시킬 수 있다.
