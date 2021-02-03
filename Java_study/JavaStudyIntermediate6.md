JavaIntermediateStudy6

## Thread

1. **상속받거나/runnable 인터페이스 구현하는 방법**



-  thread 가  가지고 있는 `run 메소드 오버라이딩`

```java
    public class MyThread1 extends Thread {
        String str; //필드 생성
        public MyThread1(String str){ //생성자 지정
            this.str = str; //객체가 생성될 때 str에 넣어줌
        }

        public void run(){ //실제 수행 흐름에서 하고 싶은 것. 
            for(int i = 0; i < 10; i ++){
                System.out.print(str);
                try {
                    //컴퓨터가 너무 빠르기 때문에 수행결과를 잘 확인 할 수 없어서 Thread.sleep() 메서드를 이용해서 조금씩 
                    //쉬었다가 출력할 수 있게한다. 
                    Thread.sleep((int)(Math.random() * 1000)); 
                  //자동으로 Exception 처리
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            } 
        } 
    }
```

- Thread 클래스를 상속받은 MyThread1을 사용하는 클래스 
  - Thread를 상속 받았으므로 MyThread1은 Thread 이다.
  - 쓰레드를 생성하고, Thread 클래스가 가지고 있는 `start() 메소드`를 호출 한다.

```java
    public class ThreadExam1 {
        public static void main(String[] args) {
            // MyThread인스턴스를 2개 만듭니다. 
            MyThread1 t1 = new MyThread1("*");
            MyThread1 t2 = new MyThread1("-");

            t1.start(); //실행 메소드 준비 
            t2.start(); //두번째 스레드 준비시켜줘!
            System.out.print("main end!!!!!");  //메인 스레드가 다 실행이 되면 출력해줘!
        }   
    }
```

- 모든 스레드의 흐름이 종료되어야 전체 프로그램 종료



## Thread 만들기

* Runnable인터페이스를 구현해서 쓰레드를 만드는 방법
  * Runable 인터페이스가 가지고 있는 run()메소드를 구현한다.
    * `이미 상속을 받은 상태라면 Thread 사용할 수 없음`
    * Runnable 인터페이스 구현해야함

```java
    public class MyThread2 implements Runnable {
        String str;
        public MyThread2(String str){
            this.str = str;
        }

        public void run(){
            for(int i = 0; i < 10; i ++){
                System.out.print(str);
                try {
                    Thread.sleep((int)(Math.random() * 1000));
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            } 
        } 
    }
```

Runable 인터페이스를 구현한 MyThread2 사용하는 방법

- MyThread2는 Thread를 상속받지 않았기 때문에 `Thread가 아니다.` => `start` 으로 호출하지 않아도 됨
- Thread를 생성하고, 해당 생성자에 MyThread2를 넣어서 Thread를 생성한다.
- Thread 클래스가 가진 start()메소드를 호출한다.

```java
    public class ThreadExam2 {  
        public static void main(String[] args) {
            MyThread2 r1 = new MyThread2("*");
            MyThread2 r2 = new MyThread2("-");

            Thread t1 = new Thread(r1);
          //runable 상속받은 애 넣어주기
          //Runnable target 넣어주기
            Thread t2 = new Thread(r2);
          

            t1.start();
            t2.start();
            System.out.print("!!!!!");  
        }   
    }
```

`상속과 인터페이스 구현은 동시에 가능`합니다. Bus클래스에서 상속을 명시하는 extends Car뒤에 implements Runnable이라고 적으면 됩니다.



```java
public class Bus extends Car implements Runnable
```



### 공유객체

----

- 하나의 객체를 여러개의 thread가 사용한다.

- Music Box 사용하는 Music Player 3명 만들어보자.

```java
    public class MusicBox { 
        //신나는 음악!!! 이란 메시지가 1초이하로 쉬면서 10번 반복출력
        
      	public void playMusicA(){
            for(int i = 0; i < 10; i ++){
                System.out.println("신나는 음악!!!");
                try {
                    Thread.sleep((int)(Math.random() * 1000));
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            } // for        
        } //playMusicA

        //슬픈 음악!!!이란 메시지가 1초이하로 쉬면서 10번 반복출력
        public void playMusicB(){
            for(int i = 0; i < 10; i ++){
                System.out.println("슬픈 음악!!!");
                try {
                    Thread.sleep((int)(Math.random() * 1000));
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            } // for        
        } //playMusicB
      
        //카페 음악!!! 이란 메시지가 1초이하로 쉬면서 10번 반복출력
        public void playMusicC(){
            for(int i = 0; i < 10; i ++){
                System.out.println("카페 음악!!!");
                try {
                    Thread.sleep((int)(Math.random() * 1000));
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            } // for        
        } //playMusicC  
    }


//그네를 사용하는  사용자 
MusicBox를 가지는 Thread객체 MusicPlayer
    public class MusicPlayer extends Thread{
        int type;
        MusicBox musicBox;  
        // 생성자로 부터 musicBox와 정수를 하나 받아들여서 필드를 초기화
        public MusicPlayer(int type, MusicBox musicBox){
            this.type = type;
            this.musicBox = musicBox;
        }
      
        // type이 무엇이냐에 따라서 musicBox가 가지고 있는 메소드가 다르게 호출 (Switch)
        public void run(){
            switch(type){
                case 1 : musicBox.playMusicA(); break;
                case 2 : musicBox.playMusicB(); break;
                case 3 : musicBox.playMusicC(); break;
            }
        }       
    }

```



- MusicBox와 MusicPlayer를 이용하는 MusicBoxExam1 클래스



```java
    public class MusicBoxExam1 {

        public static void main(String[] args) {
            // MusicBox 인스턴스 : box
            MusicBox box = new MusicBox();
						
          	// Music Player 한명씩 지정
            MusicPlayer kim = new MusicPlayer(1, box);
            MusicPlayer lee = new MusicPlayer(2, box);
            MusicPlayer kang = new MusicPlayer(3, box);

            // MusicPlayer쓰레드를 실행합니다. 
          	// by Start
            kim.start();
            lee.start();
            kang.start();           
        }   
    }
```



### 동기화 메소드와 동기화 블록

- 공유객체가 가진 메소드를 동시에 호출 되지 않도록 하는 방법 : `synchronized`
  - 사용권을 얻게 됨 (모니터링 락)
  - 메서드 실행이 차례차례....
  - 메소드 앞에 synchronized 를 붙혀서 실행해 보면, 메소드 하나가 모두 실행된 후에 다음 메소드가 실행된다.

```java
    public synchronized void playMusicA(){
        for(int i = 0; i < 10; i ++){
            System.out.println("신나는 음악!!!");
            try {
                Thread.sleep((int)(Math.random() * 1000));
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        } // for        
    } //playMusicA
```



- Synchronized 블럭
  - **synchronized를 붙히지 않은 메소드**는 다른 쓰레드들이 synchronized메소드를 실행하면서 모니터링 락을 획득했다 하더라도, 그것과 상관없이 실행된다. -> 중간중간에 나타남.
  - **synchronized를 메소드에 붙혀서 사용 할 경우**, 메소드의 코드가 길어지면, 마지막에 대기하는 쓰레드가 너무 오래 기다리는것을 막기위해서 메소드에 synchronized를 붙이지 않고, **문제가 있을것 같은 부분만 synchronized블록을 사용**한다.

```java
synchronized(this){
                System.out.println("슬픈 음악!!!");
            }

```



```java
  public void playMusicB(){
        for(int i = 0; i < 10; i ++){
            synchronized(this){
                System.out.println("슬픈 음악!!!");
            }
            try {
                Thread.sleep((int)(Math.random() * 1000));
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        } // for        
    } //playMusicB
```



---

#### 쓰레드와 상태제어

쓰레드가 3개가 있다면 JVM은 시간을 잘게 쪼갠 후에 한번은 쓰레드1을, 한번은 쓰레드 2를, 한번은 쓰레드 3을 실행합니다. 이것에 빠르게 일어나다 보니 쓰레드가 모두 동작하는 것처럼 보이는 것이다.

- 멈췄다가 반복하는 것처럼 보임

- 그렇지만 멈추는건 아니고, 실행 대기 상태.

  

  - 쓰레드는 `실행가능상태인 Runnable`과 `실행상태인 Running상태`로 나뉜다.
  - 실행되는 쓰레드 안에서 Thread.sleep()이나 Object가 가지고 있는 wait()메소드가 호출이 되면 쓰레드는 블록상태가 된다.
  - Thread.sleep()은 특정시간이 지나면 자신 스스로 블록상태에서 빠져나와 Runnable이나 Running상태가 된다.
  - Object가 가지고 있는 wait()메소드는 다른 쓰레드가 `notify()나 notifyAll()메소드를 호출하기 전에는 블록상태에서 해제되지 않는다.`
  - wait()메소드는 호출이 되면 모니터링 락을 놓게 된다. 그래서 대기중인 다른 메소드가 실행한다.
  - 쓰레드의 run메소드가 종료되면, 쓰레드는 종료된다. 즉 Dead상태가 된다.
  - Thread의 yeild메소드가 호출되면 해당 쓰레드는 다른 쓰레드에게 자원을 양보하게 된다.
  - Thread가 가지고 있는 join메소드를 호출하게 되면 해당 쓰레드가 종료될 때까지 대기하게 된다.



---



## Join 

- Join() 메소드는 쓰레드가 멈출때까지 기다리게 함.



<0.5초씩 쉬면서 숫자 출력하는 MyThread5>

```java
    public class MyThread5 extends Thread{
        public void run(){
            for(int i = 0; i < 5; i++){
                System.out.println("MyThread5 : "+ i);
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        } // run
    }
```

```java
    public class JoinExam { 
        public static void main(String[] args) {
            MyThread5 thread = new MyThread5();
            // Thread 시작 
            thread.start(); 
            System.out.println("Thread가 종료될때까지 기다립니다.");
            System.out.println("Thread가 종료되었습니다."); 
        }   
    }

'''
 >> 시작
  종료
  1
  2
  3
  4
  5
  
  //다른 방법으로 코드 작성해야함. 
```

```java
    public class JoinExam { 
        public static void main(String[] args) {
            MyThread5 thread = new MyThread5();
            // Thread 시작 
            thread.start(); 
            System.out.println("Thread가 종료될때까지 기다립니다.");
          
            try {
                // 해당 쓰레드가 멈출때까지 멈춤
                thread.join();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
          
            System.out.println("Thread가 종료되었습니다."); 
        }   
    }
```

----

## Thread : Wait, Notify

- wait와 notify는 동기화된 블록안에서 사용해야 한다. 

- wait를 만나게 되면 해당 쓰레드는 해당 객체의 모니터링 락에 대한 권한을 가지고 있다면 모니터링 락의 권한을 놓고 대기한다.

   

```java
    public class ThreadB extends Thread{
       // 해당 쓰레드가 실행되면 자기 자신의 모니터링 락을 획득
       // 5번 반복하면서 0.5초씩 쉬면서 total에 값을 누적
       // 그후에 notify()메소드를 호출하여 wiat하고 있는 쓰레드를 깨움 
        int total;
      
        @Override
        public void run(){
            synchronized(this){
                for(int i=0; i<5 ; i++){
                    System.out.println(i + "를 더합니다.");
                    total += i;
                  
                    try {
                        Thread.sleep(500); //0.5초 쉼 
                      //exception 처리
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                notify();
            }
        }
    }

```



<Thread B 사용하여 wait 하는 클래스 작성> 

```java
    public class ThreadA {
        public static void main(String[] args){
            // 앞에서 만든 쓰레드 B를 만든 후 start 
            // 해당 쓰레드가 실행되면, 해당 쓰레드는 run메소드 안에서 자신의 모니터링 락을 획득
           
          	ThreadB b = new ThreadB();
            b.start();

            // b에 대하여 동기화 블럭을 설정
            // 만약 main쓰레드가 아래의 블록을 위의 Thread보다 먼저 실행되었다면 wait를 하게 되면서 모니터링 락을 놓고 대기       
            synchronized(b){ //b라는 쓰레드가 완료될때까지 기다릴 수 있도록 wait.
                try{
                    // b.wait()메소드를 호출.
                    // 메인쓰레드는 정지
                    // ThreadB가 5번 값을 더한 후 notify를 호출하게 되면 wait에서 깨어남
                    System.out.println("b가 완료될때까지 기다립니다.");
                    b.wait();
                }catch(InterruptedException e){
                    e.printStackTrace();
                }

                //깨어난 후 결과를 출력
                System.out.println("Total is: " + b.total);
            }
        }
```



----

## 데몬스레드

- 데몬(Daemon)이란 보통 리눅스와 같은 유닉스계열의 운영체제에서 백그라운드로 동작하는 프로그램을 말한다.
- 윈도우 기반에서는 서비스.



`데몬쓰레드를 만드는 방법`

* 쓰레드에 데몬 설정을 하면 된다.

- 이런 쓰레드는 자바프로그램을 만들 때 백그라운드에서 특별한 작업을 처리하게 하는 용도로 만든다.
- 데몬쓰레드는 일반 쓰레드(main 등)가 모두 종료되면 강제적으로 종료되는 특징을 가지고 있다.

```java
    
		// Runnable을 구현하는 DaemonThread클래스를 작성
    public class DaemonThread implements Runnable {

        // 무한루프안에서 0.5초씩 쉬면서 데몬쓰레드가 실행중입니다를 출력하도록 run()메소드를 작성
        @Override
        public void run() {
            while (true) {
                System.out.println("데몬 쓰레드가 실행중입니다.");

                try {
                    Thread.sleep(500); // 1초가 1000milisecond니까

                } catch (InterruptedException e) {
                    e.printStackTrace();
                    break; //Exception발생시 while 문 빠찌도록 
                }
            }
        }

      //작동 가능하게 하는 메소드 
        public static void main(String[] args) {
            
          	// Runnable을 구현하는 DaemonThread를 실행하기 위하여 Thread 생성
            Thread th = new Thread(new DaemonThread());
            
          	// 데몬쓰레드로 설정
            th.setDaemon(true);
            // 쓰레드를 실행
            th.start();

            // 메인 쓰레드가 1초뒤에 종료되도록 설정. 
            // 데몬쓰레드는 다른 쓰레드가 모두 종료되면 자동종료.
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }   
            System.out.println("메인 쓰레드가 종료됩니다. ");    
        }   
    }
```

