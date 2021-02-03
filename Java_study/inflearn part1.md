## inflearn Part1

### 클래스의 정의

클래스 : 설계도

```java

//클래스 정의
class Npc{
  //필드 - 데이터
  String name;
  int hp;
  //메서드 - 동작
  void say(){
    System.out.println("안녕하세요.");
  }
}

//Npc 클래스 실행시켜주는 MyNpc 클래스 만들기
//객체 생성

public class MyNpc{
  public static void main(String[] args){
    Npc saram1 = new Npc();	//Npc 타입의 변수명
    
    //. 이용해 할당
    saram1.name = "경비";
    saram1.hp = 100; 
    System.out.pritnln(saram1.name + ":" + saram1.hp);
    
    saram1.say();
  }
}
```

```java
//클래스에 메소드만 있는 경우
class Calc{
  int add(int a, int b){
    return a+b;
  } 
}

public Calculation{
  public static void main(String[] args){
    //객체 생성
    Calc calc = new Calc();
    
    //메서드 호출
    int nReturn = calc.add(3,9);
    System.out.println("3 + 9 = ", nReturn );
    
  }
}

//클래스에 필드만 있는 경우
class Book{
  String title;
  String author;
  int price;
}

public class myBook{
  public static void main(String[] args{
    //객체 생성
    Book book = new Book();
    //필드 접근
    book.title = "클래스의 기초";
    book.author = "홍길동";
    book.price = 1000;
    
    System.out.println(book.title + ":" + book.author + ":"
                      + book.price);
  }
}

                 
```

---



### 패키지 선언

* 같은 클래스 사용해버리면... 동일 공간에서의 충돌, 접근 방법에서의 충돌... 

* 서로 다른 패키지의 두 클래스는 인스턴스 생성시 사용하는 이름이 다르다.

* 서로 다른 패키지의 두 클래스 파일은 저장되는 위치가 다름 -> 디렉토리이기 때문에. 

* 인터넷 도메인 이름의 역순으로 이름 구성, `소문자`

  

```java
package team1;

public class Circle{
  final double PI = 3.14;
  double rad;
  
  public void stRad(double r){
    rad = r;
  }
  //원의 넓이 반환  
public double getArea(){
   return (rad*rad) * PI;
  }
}

```

```java
package team2;

public class Circle{
  final double PI = 3.14;
  double rad;
  
  public void setRad(double r){
    rad = r;
  }
  
  //원의 둘레를 반환
  public double getPerimeter(){
    return (rad*2) * PI;
  }
}

//패키지 명 구분해주었기 때문에 같은 클래스 이름을 만들어도 에러 나지 않음.
  

```

```java
public class A_CircleUsing{
  public static void main(String[] args){
    //패키지.클래스
    team1.Circle c1 = new team1.Circle();
    c1.setRad(3.5);
    System.out.println("반지름 3.5의 원 넓이: "+ c1.getArea());
    
    team2.Circle c2 = new team2.Circle();
    c2.setRad(3.5);
        System.out.println("반지름 3.5의 원 둘레: "+ c1.getPerimeter());

  }
}

//OR

import team1.Circle;

public class B_CircleUsing{
  public static void main(String[] args){
    Circle c1 =  new Circle(); //객체 생성 성공
  }
}


```



----

### 오버로딩

* 하나의 클래스 내에 인수의 개수나 형이 다른 동일한 이름의 메서드를 여러 개 정의.

* 시그니처 = 함수의 이름 + 파라미터 
  * 오버로드 : 함수의 이름은 같으나, 시그니처가 다름. 매개변수만 다른 것. 



```java
//클래스에 같은 이름의 메서드가 여러개 존재. 
class Cal{
  int add(int a, int b){
    return a+ b;
  }
  int add(int a){
    return a +1;
  }
  double add(double a, double b){
    return a+b;
  }
}

//오버로딩된 메소드 사용
public class Calculation{  
  public class void main(String[] args){
   	Cal calc = new Calc();
    int nReturn1 = calc.add(3,9);
    int nReturn2 = calc.add(3);
    double nReturn3 = calc.add(3.0,9.0);
    
    System.out.println("Return1 : " + nReturn1);
    System.out.println("Return2 : " + nReturn2);
    System.out.println("Return3 : " + nReturn3);
    
  }
}
//모두 정상적으로 작용. 
```



---

### 생성자

* 오브젝트 생성과 함께 자동적으로 호출되는 특수한 메서드. 
* 개발자가 생성자를 기술하지 않는 경우, 인수가 없는 생성자가 자동으로 만들어짐. 



`생성자는 가장 흔한 오버로딩의 대상`

```java
class Book{
  String title;
  int price;
  
  Book(){		// 매개변수가 없는 디폴트 생성자
 		title = "자바 클래스 기초";
    price = 10000;
  }
  
  Book(String title, int price){ //매개변수가 두 개인 생성자
    this.title = title;
    this.price = price;
  }
  //복제생성자
	Book(Book copy){
    title = copy.title;
    price = copy.price;
    
  }
	void print(){
    System.out.println("제목 : " +title);
    System.out.println("가격 : " + price);
  }   	
}

public Class MyBook{
  public class void main(String[] args){
    Book book = new Book("자바 디자인 패턴", 20000);
    book.print();
    
    Book book2 = new Book(book1); //복제 생성자
    book2.title = "자바 디자인 패턴";
    book2.print();
  }
}

>>> 제목 : 자바 디자인 패턴
>>> 가격 : 20000
```

매개변수가 있는 생성자를 만든 경우, `디폴트 생성자`를 호출하면 에러가 발생. 

```java
Book book = new Book(); // 에러 난다는 것. 
Book book = new Book("자바 디자인 패턴", 20000); //으로 해주어야 함.  
```

---



### 메모리 모델

자바 가상머신이 메모리 모델 : 세 개의 영역으로 구분

* `메서드 영역`

  * 메서드의 byte 코드, static 변수

* `스택 영역`

  * 지역 변수, 매개변수

* `힙 영역 `

  * 인스턴스(객체)

  

1) Byte code

* 고급 언어로 작성된 소스 코드를 가상머신이 이해할 수 있는 중간 코드로 컴파일 한 것. 

2) 스택의 흐름

* 지역 변수는  스택에 할당. 

* 스택에 할당된 지역변수는 해당 메서드를 빠져나가면 소멸



---

### 접근제한자

`public` & `default`

public : 어디서든 인스턴스 생성이 가능.

Default : 동일  패키지로 묶인 클래스 내에서만 인스턴스 생성 허용.

![스크린샷 2021-01-30 오후 11.33.01](/Users/dagunoh/Library/Application Support/typora-user-images/스크린샷 2021-01-30 오후 11.33.01.png)

