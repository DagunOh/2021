

JavaStudyIntermediate5

## ANNOTATION

- 소스코드에 메타코드 (추가 정보 )주는 것
  - 로직에는 영향을 주지 않지만, 해당 타겟의 연결 방법이나 소스코드의 구조를 변경 가능.
- 클래스나 메소드 위에 붙여 사용 @Override

* `정의 -> 사용 -> 실행`
* ![img](https://www.nextree.co.kr/content/images/2021/01/eykim-20140205-annotation-01.jpg)
* 패키지 익스플로러에서 [new - Annotation]을 이용하여 Count100이라는 어노테이션 생성

  - Count100어노테이션을 JVM실행시에 감지할 수 있도록 하려면 @Retention(RetentionPolicy.RUNTIME)를 붙여줘야 합니다.
- Retention은 해당 어노테이션이 언제까지 유효한지를 정해주는 옵션
* `interface` 앞에 `@`

```java
    import java.lang.annotation.Retention;
    import java.lang.annotation.RetentionPolicy;

    @Retention(RetentionPolicy.RUNTIME)
	public @interface Count100 {

    }


```

  ```java
 public class MyHello {
        @Count100
        public void hello(){
            System.out.println("hello");
        }
    }

  ```

- MyHello의 hello메소드가 @Count100어노테이션이 설정되어 있을 경우, hello()메소드를 100번 호출하도록 합니다.

My hello 클래스를 이용하는 MyHelloExam 클래스 작성

```java
    import java.lang.reflect.Method;

    public class MyHelloExam {
        public static void main(String[] args) {
            MyHello hello = new MyHello();

            try{
                Method method = hello.getClass().getDeclaredMethod("hello");
              //만약 어노테이션이 존재한다면, 
            if(method.isAnnotationPresent(Count100.class)){
                    for(int i = 0; i < 100; i++){
                        hello.hello();
                    }
                }else{
                    hello.hello();
                }
            }catch(Exception ex){
                ex.printStackTrace();
            }       
        }
    }
```

