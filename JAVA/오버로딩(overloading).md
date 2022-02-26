# ✏오버로딩이란(과적하다)?
메서드도 변수와 마찬가지로 같은 클래스 내에서 서로 구별될 수 있어야 하기 때문에  
각기 다른 이름을 가져야한다. 그러나 자바에서는 한 클래스 내에 이미 사용하려는  
이름과 같은 이름을 가진 메서드가 있더라도 매개변수의 개수 또는 타입이 다르면,  
같은 이름을 사용해서 메서드를 정의 할수 있다.  

## 오버로딩의 조건  
> 1.메서드 이름이 같아야 한다.  
> 2.매개변수의 개수 또는 타입이 달라야 한다.  

*오버로딩 된 메서드들은 매개변수에 의해서만 구별될 수 있으므로 반환 타입은 오버로딩을 구현하는데  
아무런 영향을 주지 못한다는 것에 주의!  
  
  
### 오버로딩의 예  
예로 가장 대표적인 것은 바로 println메서드!!  
> void println().  
> void println(boolean x)   
> void println(char x).  
> void println(int x).  등   
  
  
  
보기1.  
```java   
//오버로딩x  매개변수 이름만 다를뿐 
int add(int a, int b) {return a+b;}
int add(int x, int y) {return x+y;}. 
  
 //오버로딩x return 타입만 다를경우
int add(int a, int b) { return a+b;}
long add(int a, int b) { return (long)(a + b);}  
  
 //오버로딩o 각 변수의 타입이 정의되어 있지만 서로 순서가 다르기 때문.  
long add(int a, long b) {return a+b;}
long add(long a, int b) {return a+b;}
```
--- 

```java
public class OverloadingTest {
	public static void main(String[] args) {
		MyMath3 mm = new MyMath3();
		System.out.println("mm.add(3, 3)결과:" + mm.add(3,3));
		System.out.println("mm.add(3, 3L)결과:" + mm.add(3L,3));
		System.out.println("mm.add(3L, 3)결과:" + mm.add(3L,3));
		System.out.println("mm.add(3L, 3L)결과:" + mm.add(3L,3L));
		 
		int[] a = {100,200,300};
		System.out.println("mm.add(a) 결과:" + mm.add(a));
	}
}
class MyMath3 {
	int add(int a , int b) {
		System.out.print("int add(int a, int b)  - ");
		return a+b;
	}
	long add(int a, long b) {
		System.out.print("long add(int a, long b) - ");
		return a+b;
	}
	long add(long a, int b) {
		System.out.print("long add(long a, int b)  - ");
		return a+b;
	}
	long add(long a, long b) {
		System.out.print("long add(long a, long b) -  ");
		return a+b;
	}
	int add(int[] a) {			//배열의 모든 요소의 합을 결과로 돌려줌. 
		System.out.print("int add(int[] a) - ");
		int result = 0;
		for(int i=0; i < a.length; i++) {
			result += a[i];
		}
		return result;
	}
}
```
실행결과:  
int add(int a, int b)  - mm.add(3, 3)결과:6  
long add(long a, int b)  - mm.add(3, 3L)결과:6  
long add(long a, int b)  - mm.add(3L, 3)결과:6  
long add(long a, long b) -  mm.add(3L, 3L)결과:6  
int add(int[] a) - mm.add(a) 결과:600  
  
add메서드가 println메서드보다 먼저 출력되는 이유?  
println 메서드가 결과를 출력하려면 add메서드의 결과가 먼저 계산되어야하기 때문이다.    

-----

## 가변인자(varargs)와 오버로딩  
기존에는 메서드의 매개변수 개수가 고정적이었으나 jdk1.5부터 동적으로 지정해 줄 수 있게 되었으며,  
이 기능을 '가변인자'라고 한다. 가변인자는 '타입... 변수명'과 같은 형식으로 선언하며, PrintStream클래스의  
printf()가 대표적인 예이다.  

	> public PrintStream printf(String format, Object... args) {...}. 
	
위와 같이 가변인자 외에도 매개변수가 더 있다면, 가변인자를 매개변수 중에서 제일 마지막에 선언해야 한다.  
그렇지 않으면, 컴파일 에러가 발생한다. 가변인자인지 아닌지를 구별할 방법이 없기 때문에 허용하지 않는 것.  
  
*만일 여러 문자열을 하나로 결합하여 반환하는 concatenate메서드를 작성한다면, 아래와 같이 매개변수의 개수를  
다르게 해서 여러 개의 메서드를 작성해야 할 것이다.  

> String concatenate(String s1, String s2) {...}. 
> String concatenate(String s1, String s2, String s3) {...}. 
> String concatenate(String s1, String s2, String s3, String s4) {...}. 

이럴 때, 가변인자를 사용하면 메서드 하나로 간단히 대체할 수 있다.  

> String concatenate(String... str) {...}  

이 메서드를 호출할 때는 아래와 같이 인자의 개수를 가변적으로 할 수 있다. 심지어는 인자가 아예 없어도 되고 배열도 인자가 될 수 있다.  

> System.out.println(concatenate());	//인자가 없음.  
> System.out.println(concatenate("a"));		//인자가 하나.  
> System.out.println(concatenate("a","b"));	//인자가 둘.  
> System.out.println(concatenate(new String[]{"A","B"}));	//배열도 가능  

가변인자는 내부적으로 배열을 이용하는 것이다. 그래서 가변인자가 선언된 메서드를 호출할 때마다 배열이 새로 생성된다.  
가변인자가 편리하지만, 이런 비효율이 숨어있으므로 꼭 필요한 경우에만 가변인자를 사용하자.  
  
그러면 가변인자는 아래와 같이 매개변수의 타입을 배열로 하는 것과 어떤 차이가 있는가?  

> String concatenate(string[] str){ ...}. 
> String result = concatenate(new Stgring[0]); 	//인자를 배열로 지정  
> String result = concatenate(null);		//인자를 null로 지정  
> String result = concatenate();		//에러. 인자가 필요함  
 
매개변수의 타입을 배열로 하면, 반드시 인자를 지정해 줘야하기 때문에, 위의 코드에서 처럼 인자를 생략 할 수 있다.  
그래서 null이나 길이가 0인 배열을 인자로 지정해줘야 하는 불편함이 있다.  
  
```java
class VarArgsEx {
	public static void main(String[] args) {
		String[] strArr = { "100", "200", "300" };
		
		System.out.println(concatenate("", "100", "200", "300"));
		System.out.println(concatenate("-", strArr));
		System.out.println(concatenate(",", new String[]{"1", "2", "3"}));
		System.out.println("["+concatenate(",", new String[0])+"]");
		System.out.println("["+concatenate(",")+"]");
	}

	static String concatenate(String delim, String... args) {
		String result = "";

		for(String str : args) {
			result += str + delim;
		}
		
		return result;
	}
/*
	static String concatenate(String... args) {
		return concatenate("",args);
	}
*/ 이주석을 풀고 컴파일시 에러가 뜬다. 오버로딩 된 메서드가 구분되지 않아서 발생하는 것 
} // class
```
실행결과:  
100200300  
100-200-300-   
1,2,3,  
[]  
[]  

concatenate메서드는 매개변수로 입려된 문자열에 구분자를 사이에 포함시켜 결합해서 반환한다. 가변인자로 매개변수를 선언했기 때문에  
문자열을 개수의 제약없이 매개변수로 지정할 수 있다.  

> String[] strArr = new String[]{"100","200","300"};  
> System.out.println(concatenate("-", strArr));  

위의 두 문장을 하나로 합치면 아래로 같이 사용가능함.  

> System.out.println(concatenate("-",new String[]{"100","200","300"}));  
  
> System.out.println(concatenate("-",{"100","200","300"})); 이건 사용 불가!  



## 메서드를 호출했을 때 이와 같이 구별되지 못하는 경우가 발생하기 쉽기 때문에 가능하면 가변인자를 사용한 메서드는 오버로딩하지 않는 것이 좋다.  
  
# 끝




