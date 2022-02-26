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

> add메서드가 println메서드보다 먼저 출력되는 이유?  
> println 메서드가 결과를 출력하려면 add메서드의 결과가 먼저 계산되어야하기 때문.  

---  

## 가변인자(varargs)와 오버로딩  
기존에는 메서드의 매개변수 개수가 고정적이었으나 jdk1.5부터 동적으로 지정해 줄 수 있게 되었으며,  
이 기능을 '가변인자'라고 한다. 가변인자는 '타입... 변수명'과 같은 형식으로 선언하며, PrintStream클래스의  
printf()가 대표적인 예이다. 
	> public PrintStream printf(String format, Object... args) {...}. 
	
위와 같이 가변인자 외에도 매개변수가 더 있다면, 가변인자를 매개변수 중에서 제일 마지막에 선언해야 한다.  
그렇지 않으면, 컴파일 에러가 발생한다. 가변인자인지 아닌지를 구별할 방법이 없기 때문에 허용하지 않는 것.  







