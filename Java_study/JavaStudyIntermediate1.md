JavaStudyIntermediate1



## Object와 오버라이딩

object 클래스는 모든 클래스의 최상위 클래스



equals, toString, hashCode 등을 씀 => 반드시 오버라이딩 해서 사용해야한다.

**equals**; 객체가 가진 값을 비교할 때 사용. 

**toString** ; 객체가 가지고 있는 값을 문자열로 반환.

**hashCode**; 객체의 해시코드 값 반환.

해시코드_ 객체별로 다 다른 값. 알게모르게 생각보다 자주 사용.



~~~java
//오버라이딩 안했을 때
public class Student {
	String name;	//field (속성 ) 선언 
	String number;
	int birthYear;
  
  
	public static void main(String[] args) {
		Student s1 = new Student();
		s1.name = "yoda";
		s1.number = "1234";
		s1.birthYear = 2000;
		
		Student s2 = new Student();
		s2.name = "yoda";
		s2.number = "1234";
		s2.birthYear = 2000;
		
		//s1 = s2 field 모두 같음.
		
		if(s1.equals(s2))
			System.out.println("s1==s2");
	    else
	    	System.out.println("s1 != s2");//얘로 출력됨.
		
		System.out.println(s1.hashCode());
		System.out.println(s2.hashCode()); //다른 hashcode
  }
}
~~~

꼭 오버라이딩 해야함. Source -> Generate ...methods... 누르면 자동으로 코드 생성. 

~~~java

public class Student {
	String name;	//field (속성 ) 선언 
	String number;
	int birthYear;

	
	@Override
	public int hashCode() {
		final int prime = 31;
		int result = 1;
		result = prime * result + birthYear;
		result = prime * result + ((name == null) ? 0 : name.hashCode());
		result = prime * result + ((number == null) ? 0 : number.hashCode());
		return result;
	}


	@Override
	public boolean equals(Object obj) {
		if (this == obj)
			return true;
		if (obj == null)
			return false;
		if (getClass() != obj.getClass())
			return false;
		Student other = (Student) obj;
		if (birthYear != other.birthYear)
			return false;
		if (name == null) {
			if (other.name != null)
				return false;
		} else if (!name.equals(other.name))
			return false;
		if (number == null) {
			if (other.number != null)
				return false;
		} else if (!number.equals(other.number))
			return false;
		return true;
	}


	@Override
	public String toString() {
		return "Student [name=" + name + ", number=" + number + ", birthYear=" + birthYear + "]";
	}


	public static void main(String[] args) {
		Student s1 = new Student();
		s1.name = "yoda";
		s1.number = "1234";
		s1.birthYear = 2000;
		
		Student s2 = new Student();
		s2.name = "yoda";
		s2.number = "1234";
		s2.birthYear = 2000;
		
		//s1 = s2 field 모두 같음.
		
		if(s1.equals(s2))
			System.out.println("s1==s2");
	    else
	    	System.out.println("s1 != s2");
		
		System.out.println(s1.hashCode());
		System.out.println(s2.hashCode()); //다른 hashcode
		
		System.out.println(s1);
		System.out.println(s1.toString()); //위랑 같음.
	}
	
}

~~~



## java.lang 패키지/오토박싱

자바는 기본적으로 *다양한 패키지*를 지원, 그 중에서도 가장 중요한 패키지!

- 모든 클래스의 최상위 클래스인 Object도 java.lang패키지
- 문자열과 관련된 String, StringBuffer, StringBuilder도 모두 java.lang패키지
- 화면에 값을 출력할때 사용했던 System클래스도 java.lang패키지
- 수학과 관련된 Math클래스도 java.lang패키지
- Thread와 관련된 중요 클래스들이 java.lang패키지
- 이외에도 다양한 클래스와 인터페이스가 java.lang패키지에 속해 있다.
- 

**wrapper** : 8개의 기본데이터 타입을 **객체**로 변환시킬 때 사용.

- Boolean, Byte, Short, Integer, Long, Float, Double 클래스

  

**오토박싱** : 기본 타입 데이터를 개체 타입의 데이터로 자동 형변환 시켜주는 기능

**오토언박싱**: 오토박싱과 반대로 객체 타입의 데이터를 기본형 타입 데이터로 자동 형변환

~~~java
public class WrapperExam {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int i =  5; //얜 아직 객체 아님, 메서드 없음.
		Integer i2 =  new Integer(5);//원래 이래야 함 
		Integer i3 = 5;//오토박싱. 
		//실제로 컴파일 할 때는 new Integer(5)로 실행됨.
		
		int i4 = i3.intValue(); //객체임
		int i5 = i3;
		//intValue 호출하지 않아도 괜찮음.
		//오토 언박싱
	}

}
~~~



## 스트링버퍼

- String : 불변클래스인 것 기억해야함.

- 하지만 스트링버퍼 클래스는 -> 자기 자신이 변함.

- ### **`메소드 체이닝`** : 자기 자신을 리턴하여 계속해서 자신의 메소드를 호출하는 방식

  ~~~java
  public class StringBufferExam {
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		StringBuffer sb = new StringBuffer();
  		sb.append("hello");
  		sb.append(" ");
  		sb.append("World");
  		
  		String str = sb.toString(); //String - StringBuffer
  		System.out.println(str); //sb에 추가했던 문자들이 차례로 출력. 
  		//대체로 자기 자신을 반환 
  		
  		StringBuffer sb2 = new StringBuffer();
  		StringBuffer sb3 = sb2.append("hello");
  		//append메소드 -> 자기 자신 this 반환 
  		if(sb2 == sb3)
  			System.out.println("sb2==sb3");
  
  ~~~

  string buffer 는 그 자체로 객체이기 때문에, 메소드가 존재. 연속해서 메소드 사용 가능.

  **메소드.메소드.메소드.toString();**

  ~~~java
  String str2 = new StringBuffer().append("hello").append(" ").append("world").toString();
  System.out.println(str2);
  ~~~

  

  ## String class의 문제점

  string class는 불변클래스이다.

  ~~~java
  public class StringExam2 {
  
  	public static void main(String[] args) {
  		// TODO Auto-generated method stub
  		String str1 = "hello world";
  		String str2 = str1.substring(5); //5번째부터 출력 
  		System.out.println(str1);
  		System.out.println(str2);
      
      //2.
  		String str3 = str1 + str2; //StringBuffer 사용하는 것과 같음. 
  		System.out.println(str3);
  		
      //2와 같음. 
  		String str4 = new StringBuffer().append(str1).append(str2).toString();
  		System.out.println(str4);
    }
  }
  ~~~

  만약 for 문 사용한다면... 아래것으로 사용해야함.

  ~~~java
  //비효율적 방법
  String str5 = "";
  	for (int i =0; i<100; i++) {
  		str5 = str5 + "*"; //성능에 문제생김.
    }
  System.out.println(str5);
  }
  ~~~

  ~~~java
  //효율적 방법 StringBuffer() 사용.
  StringBuffer sb=  new StringBuffer();
  for (int i =0;i<100; i++) {
  		sb.append("*");
  	}
  	String str6 = sb.toString();
  	System.out.println(str6);
  }
  ~~~

  

## Math 클래스

- 수학 계산을 위한 클래스
- 코사인, 사인, 탄젠트, 절댓값, 랜덤값 구할 수 있는 클래스.
- `private but static`
- 객체 생성하지 않고도 사용할 수 있음.



`파이썬`이랑 똑같음.

~~~java
public class MathExam {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		int value1 = Math.max(5, 30);//두가지 값 중 더 큰 
		System.out.println(value1);
		
		int value2 = Math.min(5, 30); //두 가지 값 중 더 작은 
		System.out.println(value2);
		
		int value3 = Math.abs(-19);
		System.out.println(value3);
		
		//Random
		System.out.println(Math.random());
		
		//Sqrt
		System.out.println(Math.sqrt(25));
	
    //Power
    System.out.println(Math.pow(16,0.5));
	}

}

~~~





​	