javaStudyIntermediate2



## java.util 패키지

1. **java.util package**

- 유용한 클래스들을 많이 가지고 있음. 
- 프로그래밍 하다 보면 자주 사용하는 클래스.
- `date`  클래스 등..
  - deprecated : 문구 있는 메서드 : 더 이상 지원하지 않는 기능이므로 사용을 자제.
- 자료구조에 관련된 인터페이스 구현하는 다양한 클래스. 



----



2. #### **컬렉션 프레임워크** 

- 자료구조 : 자료를 저장할 수 있는 구조

- 책을 보관하기 위해서 책장을 이용한다든지 :  Collection Framework

- #### `Collection` 

  - 저장된 순서 없음. -> iterator 이용.
  - add() : 자료 추가 
  - Size() : 자료의 개수 

  #### `iterator` : 자료 하나씩 꺼내기 위해...

  - hasNext() : 꺼낼것이 있는지 없는지 확인
  - next() : 하나씩 자료를 꺼낼때.

  #### `set`

  -  Collections 상속
  - add () : 같은 자료 있으면 True, 아니면 False

  

  #### `List`

  - 순서가 있는 자료구조
  - Get(int) : Object

  

  #### `Map`

  - key 와 value 가지는 자료구조  (파이썬의 Dict)
  - Put(Object, Object) : void 
    - 키와 밸류 동시에 저장
  - Get(Object) : Object 
    - 키 get
  - keySet() : Set
    - 중복 제거, key 가져오기...



-----



3. ### Generic

Box class

~~~java
 public class Box {
        private Object obj;
        public void setObj(Object obj){
        this.obj = obj;
        }

        public Object getObj(){
        return obj;
        }
    }
~~~

BoxExam class

~~~java
public class BoxExam {
    public static void main(String[] args) {
            Box box = new Box(); //객체 만들기
            box.setObj(new Object());
            Object obj = box.getObj();

            box.setObj("hello");
            String str = (String)box.getObj();
            System.out.println(str);

            box.setObj(1);
            int value = (int)box.getObj();
            System.out.println(value);
        }
    }
~~~

Generic 사용

- **가상의 클래스 지정해놓은 뒤, `구체적인 다양한 클래스`사용할 수 있게 함.**

- 클래스 이름 뒤에 `<E>`가 제네릭을 적용한 것이다. Box는 가상의 클래스 E를 사용한다는 의미.

- 으로 클래스 이름 뒤에 <E>가 제네릭을 적용한 것이다. Box는 가상의 클래스 E를 사용한다는 의미.
- **Object를 받아들이고, 리턴하던 부분이 E로 변경. E는 실제로 존재하는 클래스는 아니다.**

~~~java
public class Box<E> {
        private E obj;
        public void setObj(E obj){
            this.obj = obj;
        }

        public E getObj(){
            return obj;
        }
    }
~~~

수정한 BoxExam Class

~~~java
 public class BoxExam {
    public static void main(String[] args) {
            Box<Object> box = new Box<>();
            box.setObj(new Object());
            Object obj = box.getObj();

            Box<String> box2 = new Box<>(); //string 받아들이는 box
            box2.setObj("hello"); //string 받아들임
            String str = box2.getObj(); //값 string으로 변호
            System.out.println(str);

            Box<Integer> box3 = new Box<>();
            box3.setObj(1);
            int value = (int)box3.getObj();
            System.out.println(value);
        }
    }
~~~



----

4. ### Set

- 중복이 없고 , 순서도 없는 자료구조. 
- Hashset, TreeSet
- set는 Interface 이기 때문에,  HashSet 이용..
- 값 다 꺼내기 위해선 Iterator 필요



~~~java
import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class setExam {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//Set은 인터페이스이기 때문에 New 사용해서 객체 생성 불가 
		Set<String> set1 = new HashSet<>();
		//set1.add("kang"); //저장할 때마다 불린 값. 이미 같은값 있으면 False.
		boolean flag1 = set1.add("kang");
		boolean flag2 = set1.add("kim");
		boolean flag3 = set1.add("kang");
		
		//set의 크기
		System.out.println(set1.size());
		//2가 출력. 같은것 제외되기 때문에 
		
		System.out.println(flag1);
		System.out.println(flag2);
		System.out.println(flag3);
		
		//값 다 꺼낼때 필요한 iterator
		
		Iterator <String> iter = set1낼 것이 있다면 true 리턴.  
			String str = iter.next(); //값을 꺼내서 받아냄.
			System.out.println(str);
		}
		
	}

}
~~~



----



5. ### List

- 저장공간이 필요에 따라 자유 + 자료구조
- 예전에 배웠던 배열은 자료구조이면서, 한 번 생성하면 크기 변경 불가 
- index 사용가능



~~~java
import java.util.ArrayList;
import java.util.List;

public class ListExam {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		List <String> list = new ArrayList<>();
		list.add("kim");
		list.add("kang");
		list.add("kim");
		
		System.out.println(list.size());
		//사이즈 출력 
		//리스트의 부모 인터페이스인 collections가 가지고 있는 메소드
		
		for (int i = 0 ; i <list.size(); i++) {
			String str = list.get(i);
			System.out.println(str);			
		}
	}
}
~~~

예시 문제 : 내가 헤맸던  것. 

```Java
import java.util.*;

public class ListExam{
    public List<String> addArray(String[]arr1, String[]arr2){
        List<String> list = new ArrayList<String>();

        for(String str : arr1){
            list.add(str);     
        }

        for(String str : arr2){
            list.add(str);
        }

        return list;
    }

    public static void main(String[] args){
        ListExam LE = new ListExam();
        String[] arr1 = {"임","의"};
            String[] arr2 = {"의","리","스","트"};

        List<String> list = LE.addArray(arr1,arr2);
        System.out.println("List의 원소의 수는 " + list.size() + " 개 입니다.");
        for(int i = 0; i < list.size(); i++ ){
    System.out.println("List의 " + (i+1) + "번째 원소는 '" + list.get(i) + "' 입니다.");
         }
    }
}
```



-----



6. ### Map

- Key, value 쌍으로 저장하는 자료 구조.
- key 중복 불가능. 값은 중복 가능. 



```java
import java.util.HashMap;
import java.util.Iterator;
import java.util.Map;
import java.util.Set;

import javax.swing.JComboBox.KeySelectionManager;
public class MapExam {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Map <String, String>  map = new HashMap();
		map.put("001", "kim");
		map.put("002", "lee");
		map.put("003", "choi");
		
		//같은 key로 두 번 담으면 어차피 사이즈는 똑같음. 
		map.put("001", "kang");
		//데이터는 마지막 것.
		
		System.out.println(map.get("001"));
		System.out.println(map.get("002"));
		
		//get all keys
		Set <String> keys = map.keySet();
		Iterator<String> iter = keys.iterator();
		while(iter.hasNext()) {
			String key = iter.next();
			String value = map.get(key);
			System.out.println(key +  ":"+ value);
		}
		
	}
```

