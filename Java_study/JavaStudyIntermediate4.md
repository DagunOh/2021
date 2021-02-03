JavaStudyIntermediate4

## Java IO

- **자바 IO 패키지**
- **I : Input, O : output**
  - 입력과 출력에 대한 부분에 대한 인터페이스와 클래스 
  - 출력 예시 : 파일, 네트워크, 콘솔 등
- **Byte VS Char**
  - Byte : 추상클래스 InputStream, OutputStream
  - Char: 추상클래스 Reader, Outer
- 어디로부터 입력을 받을 것인지, 쓸 것인지 : 

![스크린샷 2021-01-23 오후 2.33.08](/Users/dagunoh/Library/Application Support/typora-user-images/스크린샷 2021-01-23 오후 2.33.08.png)



장식 : java IO = **데코레이터 패턴**으로 만들어짐. 하나의 클래스를 장식하는 것 처럼 생성자에서 감싸서 새로운 기능 계속 추가할 수 있도록 클래스를 만드는 방식. 

- 샤워기- 연수기 - 필터  등등...



## Byte 단위 입출력

파일로 부터 **`1byte씩 읽어들여`** 파일에 1byte씩 저장하는 프로그램을 작성

- 파일로 부터 읽어오기 위한 객체 - FileInputStream
- 파일에 쓸수 있게 해주는 객체 - FileOutputStream .

**read()**메소드가

- **byte를 리턴한다면 끝을 나타내는 값을 표현할 수가 없기 때문에**, **byte가 아닌 int를 리턴**한다.
- **음수의 경우 맨 좌측 비트가 1**이 된다. **읽어들일 것이 있다면 항상 양수를 리턴한다고볼 수 있다.** .
  - 즉, 더이상 읽어들일 것이 없을 때는 음수 -1을 리턴한다. 

```java

    public class ByteIOExam1 {
        public static void main(String[] args){     
            FileInputStream fis = null; 
            FileOutputStream fos = null;        
            try {
                fis = new FileInputStream("src/javaIO/exam/ByteExam1.java"); //이 파일에서 1byte 씩 읽어서
                fos = new FileOutputStream("byte.txt"); //여기에 써주기 

                int readData = -1; 
              
              //반복문 통해 입력
                while((readData = fis.read())!= -1){
                    fos.write(readData);
                }           
            } catch (Exception e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }finally{
              //열었으면 무조건 닫아야 함.
                try {
                    fos.close();
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                try {
                    fis.close();
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            }
        }
    }

```





이번엔, **더 큰 Byte 단위 입력과 출력** => `배열` 이용. 

- byte[] buffer = new byte[512];
- 

```java
    public class ByteIOExam1 {
        public static void main(String[] args){     
            //메소드가 시작된 시간을 구하기 위함
            long startTime = System.currentTimeMillis();        
            FileInputStream fis = null; 
            FileOutputStream fos = null;        
            try {
                fis = new FileInputStream("src/javaIO/exam/ByteExam1.java");
                fos = new FileOutputStream("byte.txt");

                int readCount = -1; 
                byte[] buffer = new byte[512];
                while((readCount = fis.read(buffer))!= -1){
                    fos.write(buffer,0,readCount);
                  //buffer의 0부터 readCount
                  //1byte 씩 읽어들이는 것보다 훨씬 빠름.
                }
            } catch (Exception e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }finally{
                try {
                    fos.close();
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
                try {
                    fis.close();
                } catch (IOException e) {
                    // TODO Auto-generated catch block
                    e.printStackTrace();
                }
            }
        //메소드가 끝났을때 시간을 구하기 위함. 
        long endTime = System.currentTimeMillis();
        //메소드를 수행하는데 걸린 시간을 구할 수 있음. 
        System.out.println(endTime-startTime); 
        }
    }

```



## 다양한 타입의 출력



- **java io객체는 인스턴스를 만들고, 모두 사용하면 close()메소드를 호출해야 한다.**

  - ####  **`try with resources`**

```java
	  try(
                //io객체 선언
        ){
                //io객체 사용
        }catch(Exception ex){
            ex.printStackTrace();
        }
```



다양한 타입으로 저장 할 수 있는 **DataOutputStream**

- wirteInt() - 정수값으로 저장
- wirteBoolean() - boolean값으로 저장
- writeDouble() - double 값으로 저장



```java

    import java.io.DataOutputStream;
    import java.io.FileOutputStream;    
    public class ByteExam3 {    
        public static void main(String[] args) {
            try(
                    DataOutputStream out = new DataOutputStream(new FileOutputStream("data.txt")); 
              //file의 경로 써줌. 안쓰면 자동으로 프로젝트 파일아래에 생성
            ){
                out.writeInt(100); //다양한 type / 4byte
                out.writeBoolean(true); //1 byte
                out.writeDouble(50.5); //8 byte
            }catch (Exception e) { //try블럭 열었으니까 catch 블럭.
                e.printStackTrace();
            }
        }   
    }
```

근데 결과가 막 이상하게 나옴. 이러한 결과를 읽어낼 수 있게 하는 것은....



- file 속성 확인하면 13byte로.
  
  - int(4) + boolean(1) + double(8) = 13
- data.dat로부터 값을 읽어들여 화면에 출력하는 클래스

- 다양한 타입의 데이터를 읽어낼 수 있는 DataInputStream
  - readInt() -정수를 읽어들이는 메소드
  - readBoolean() - boolean 값을 읽어들이는 메소드
  - readDouble() - douboe 값을 읽어들이는 메소드
  
  

```java
    import java.io.DataInputStream;
    import java.io.FileInputStream;

    public class ByteIOExam4 {

        public static void main(String[] args) {
            try(
                    DataInputStream out = new DataInputStream(new FileInputStream("data.txt"));
            ){
              
                int i = out.readInt();          
                boolean b = out.readBoolean();          
                double d = out.readDouble();

                System.out.println(i);
                System.out.println(b);
                System.out.println(d);
            }catch(Exception ex){
                ex.printStackTrace();
            }
        }
    }

```



## Char 단위 입출력 

- char단위 입출력 클래스는 클래스 이름이 Reader나 Writer로 끝이 남.
- char단위 입출력 클래스를 이용해서 키보드로 부터 한줄 입력 받아서 콘솔에 출력
  - System.in - 키보드를 의미 (InputStream )
  - BufferedReader - 한줄씩 입력 받기위한 클래스 : reader 만 받을 수 있음. 
  - BufferedReader 클래스의 생성자는 InputStream을 입력받는 생성자가 없다.
  - System.in은 InputStream 타입이므로 BufferedReader의 생성자에 바로 들어갈 수 없으므로 InputStreamReader 클래스를 이용해야함.



* Decorator 패턴 : 객체의 추가적인 요건(기능)을 동적으로 첨가하는 방식. 
  * 서브클래스를 만드는 것을 통해 유연하게 확장 가능. 

```java
import java.io.BufferedReader;
import java.io.FileWriter;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter; 

public class CharIOExam01 {
        public static void main(String[] args) {
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            //키보드로 입력받은 문자열을 저장하기 위해 line변수를 선언
            //실제로...
            //System.In 받을 수 있는 InputStreamReader 객체에다가 키보드를 의미하는 System.in을 넣어주면 됨. 
          	//Decorator Pattern : 
            
          String line = null;     
            try {
                line = br.readLine()
            } catch (IOException e) {
                e.printStackTrace();
            }
          // Exception 해결
          
            //콘솔에 출력 
            System.out.println(line);
        }
    }


hello!!!!
  
>>> hello!!!
```





## Char 단위 입출력

### try -> catch -> finally / Exception

- 파일에서 한 줄씩 입력 받아서 파일에 출력
  - **파일에서 읽기위해서 FileReader 클래스** 이용
  - **한 줄 읽어 들이기 위해서 BufferedReader 클래스** 이용
    - BufferedReader 클래스가 가지고 있는 readLine() 메소드가 한줄씩 읽게 해준다.
    - readLine()메소드는 읽어낼 때 **더 이상 읽어 들일 내용이 없을 때 null을 리턴**한다.
  - 파일에 쓰게하기 위해서 FileWriter 클래스 이용
  - 편리하게 출력하기 위해서 PrintWriter 클래스 이용

```java

    import java.io.BufferedReader;
    import java.io.FileReader;
    import java.io.FileWriter;
    import java.io.IOException;
    import java.io.PrintWriter; 


    public class CharIOExam02 {
        public static void main(String[] args) {
            BufferedReader br = null; 
            PrintWriter pw = null;
            try{        
                br = new BufferedReader(new FileReader("src/javaIO/exam/CharIOExam02.java"));
                pw = new PrintWriter(new FileWriter("test.txt")); //어디에 있는 파일에 저장?
              
                String line = null;
                while((line = br.readLine())!= null){
                    pw.println(line);
                }
              
            }catch(Exception e){
                e.printStackTrace();
            
              //반드시 처리해야할 로직을 finally 에... -> 닫아야함. 
            }finally {
                pw.close();
                try {
                    br.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    
```



```java
import java.io.*;

public class CharIOExam{
    public static void main(String[]args){
        PrintWriter pw = null;
        try{
            pw = new PrintWriter(new FileWriter("data.txt"));
            pw.println("안녕하세요. PrintWriter입니다.");

        }catch(Exception e){
            e.printStackTrace();
        }finally{
            pw.close();
        }
    }
```

**`Exception 처리 안해주면 틀린다!`**

