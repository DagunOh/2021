JavaStudyIntermediate7

## 람다식

- 람다식은 다른말로 익명 메소드라고도 한다.

- 인터페이스 중에서 메소드를 하나만 가지고 있는 인터페이스를 함수형 인터페이스라고 한다.
  - 쓰레드를 만들때 사용하는 Runnable 인터페이스의 경우 run()메소드를 하나만 가지고 있다.
- Runnable을 이용하여 쓰레드를 만드는 방법



##### 메소드만 전달할 수 있다면, 좀더 편리하게 프로그래밍할 수 있음. -> 내말이

##### 자바는 메소드만 전달할 수 있는 방법은 없었기 기때문에 매번 객체를 생성해서 매개변수로 전달해야 했다. 

##### 이런 부분을 해결한 것이 람다표현식.



<Runnable 이용하여 쓰레드 만드는 방법>

```java
 public class LambdaExam1 {

        public static void main(String[] args) {
            new Thread(new Runnable(){
              
              public void run(){
                for(int i = 0; i < 10; i++){
                    System.out.println("hello");
                }
                
            }}).start();
        }   
    }
```



<람다 표현식>

```java

    public class LambdaExam1 {  
        public static void main(String[] args) {
          
          //객체 생성할 필요 없음.
          //구현할 부분만 남겨두면 됨.
            new Thread(()->{
                for(int i = 0; i < 10; i++){
                    System.out.println("hello");
                }
            }).start();
        }   
    }
```

- ()->{ ..... } 부분이 람다식, 다른말로 익명 메소드
- JVM은 Thread생성자를 보고 ()->{} 이 무엇인지 대상을 추론한다.
- Thread생성자 api를 보면 Runnable인터페이스를 받아들이는 것을 알 수 있다.
- JVM은 Thread생성자가 Runnable인터페이스를 구현한 것이 와야 하는 것을 알게 되고 람다식을 Runnable을 구현하는 객체로 자동으로 만들어서 매개변수로 넣어준다.



----



## 람다식 기본 문법

### **`(매개변수목록)->{실행문}`**

2개의 값을 비교하여 어떤 값이 더 큰지 구하는 compareTo()라는 메소드를 가지고 있는 Compare 인터페이스

- 2개의 값을 받아들인 후, 정수를 반환하는 메소드를 선언



```java
public interface Compare{
  	public int compareTo(int value1, int value2);
 
}


public class CompareExam{
  	public static void exec(Compare compare)
      int k = 10;
  		int m = 20;
  		int value = compare.compareTo(k,m);
  		System.out.println(value);
}

publiv static void main(String[] args){
  	exec((i,j))->{
      return i-j;
      //양수면 i가 크고, 음수면 j가 크고.
      //Compare 을 구현하는 익명 객체 만들어서 exec에 전달. 
    }); }
}

```

