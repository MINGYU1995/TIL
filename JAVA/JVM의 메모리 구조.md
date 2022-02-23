# ✏JVM의 메모리 구조  

응용프로그래임이 실행되면, jvm은 시스템으로부터 프로그램을 수행하는데 필요한  
메모리를 할당받고 jvm은 이 메모리를 용도에 따라 여러 영역으로 나누어 관리한다.  

### 1.메서드 영역(method area)   
> 프로그램 실행 중 어떤 클래스가 사용되면, jvm은 해당 클래스의 클래스파일(별.class)을 읽어서 분석하여 
> 클래스에 대한 정보(클래스 데이터)를 이곳에 저장한다. 이 때, 그 클래스의 클래스 변수도 이영역에 함께 생성된다.
 
### 2.힙(heap)  
> 인스턴스가 생성되는 공간. 프로그램 실행 중 생성되는 인스턴스는 모두 이곳에 생성된다. 즉, 인스턴스 변수들이 
> 생성되는 공간이다.  

### 3.호출스택(call stack 또는 execution stack)  
> 호출스택은 메서드의 작업에 필요한 메모리 공간을 제공한다. 메서드가 호출되면, 호출스택에 호출된 메서드를 위한
> 메모리가 할당되며, 이 메모리는 메서드 작업을 수행하는 동안 지역변수(매개변수 포함) 들과 연산의 중간결과 등
> 을 저장하는데 사용된다. 그리고 메서드가 작업을 마치면 할당되었던 메모리공간은 반한되어 비워진다.  

### 호출스택의 특징  
1.메서드가 호출되면 수행에 필요한 만큼의 메모리를 스택에 할당받는다.  
2.메서드가 수행을 마치고나면 사용했던 메모리를 반환하고 스택에서 제거된다.  
3.호출스택의 제일 위에 있는 메서드가 현재 실행 중인 메서드이다.  
4.아래에 있는 메서드가 바로 위의 메서드를 호출한 메서드이다.  
 
```java
class CallStackTest2 {
	public static void main(String[] args) {
		System.out.println("main(String[] args)이 시작되었음.");
		firstMethod();
		System.out.println("main(String[] args)이 끝났음.");
	}

	static void firstMethod() {
		System.out.println("firstMethod()이 시작되었음.");
		secondMethod();
		System.out.println("firstMethod()이 끝났음.");		
	}

	static void secondMethod() {
		System.out.println("secondMethod()이 시작되었음.");
		System.out.println("secondMethod()이 끝났음.");		
	}
}
```
컴파일결과:  
main(String[] args)이 시작되었음.  
firstMethod()이 시작되었음.  
secondMethod()이 시작되었음.  
secondMethod()이 끝났음.  
firstMethod()이 끝났음.  
main(String[] args)이 끝났음.  

---
## ✏기본형 매개변수와 참조형 매개변수  

> 기본형 매개변수 : 변수의 값을 읽기만 할 수 있다.(read only)  
> 참조형 매개변수 : 변수의 값을 읽고 변경할 수 있다.(read & write)

```java
class Data { int x; }

class MyMathTest3 {
	public static void main(String[] args) {
	Data d = new Data();
	d.x = 10;
	System.out.println("main() : x = " + d.x);

	change(d.x);
	System.out.println("After change(d.x)");
	System.out.println("main() : x = " + d.x);
}

static void change(int x) {  // 기본형 매개변수 
	x = 1000;
	System.out.println("change() : x = " + x);
	
	}
}
```
컴파일결과:  
main() : x = 10  
change() : x = 1000  
After change(d.x)  
main() : x = 10  
 
 ---
 메서드로 배열을 다루는 여러가지 방법이 존재한다.
 
```java
class MyMathTest3 {
	public static void main(String[] args) {
		int[] arr = new int[] {3,2,1,6,5,4};

		printArr(arr);  // 배열의 모든 요소를 출력
		sortArr(arr);   // 배열을 정렬
		printArr(arr);  // 정렬후 결과를 출력
		System.out.println("sum="+sumArr(arr)); // 배열의 총합을 출력
	}

	static void printArr(int[] arr) {  // 배열의 모든 요소를 출력
		System.out.print("[");

		for(int i : arr)  // 향상된 for문
			System.out.print(i+",");
		System.out.println("]");
	}

	static int sumArr(int[] arr) {  // 배열의 모든 요소의 합을 반환
		int sum = 0;

		for(int i=0;i<arr.length;i++)
			sum += arr[i];
		return sum;
	}

	static void sortArr(int[] arr) {  // 배열을 오름차순으로 정렬
		for(int i=0;i<arr.length-1;i++)
			for(int j=0;j<arr.length-1-i;j++)
				if(arr[j] > arr[j+1]) {
					int tmp = arr[j];
					arr[j] = arr[j+1];
					arr[j+1] = tmp;
				}
	} // sortArr(int[] arr)
}
```
컴파일결과:  
[3,2,1,6,5,4,]  
[1,2,3,4,5,6,]  
sum=21  
  
---
## ✏참조형 반환타입


```java   
class Data { int x; }

class ReferenceReturnEx {
	public static void main(String[] args) 
	{
		Data d = new Data();
		d.x = 10;

		Data d2 = copy(d); 
		System.out.println("d.x ="+d.x);
		System.out.println("d2.x="+d2.x);
	}

	static Data copy(Data d) {
		Data tmp = new Data();	//새로운 객체 tmp를 생성한다. 
		tmp.x = d.x;		//d.x의 값을 tmp.x에 복사한다.

		return tmp;		// 복사한 객체의 주소를 반환한다.
		//copy의 메서드가 종료되어도 반환된 tmp의 값은 참조변수 d2에 저장된다. tmp가 사라졌지만 d2로 새로운 객체를 다룰 수 있다.
		
	}
}
```
컴파일결과:  
d.x =10  
d2.x=10  
  
copy메서드는 새로운 객체를 생성한 다음에, 매개변수로 넘겨받은 객체에 저장된 값을 복사해서 반환한다.  
반환하는 값이 Data객체의 주소이므로 반환 타입이 'Data'인 것이다.  

> copy 메서드의 반환타입이 'Data'이므로, 호출결과를 저장하는 변수의 타입 역시 'Data'타입의 참조 변수이어야 한다.  
  
  ---
## ✏재귀호출(recursive call)  

```java
class FactorialTest {
	public static void main(String args[]) {
		System.out.println(factorial(4)); // FactorialTest.factorial(4)
	}

	static long factorial(int n) {
		long result=0;

		if (n == 1) return 1;		

		return n * factorial(n-1); // ¸ 다시 메서드 자신을 호출한다.
	}
}
}
```
컴파일결과:  
24  

  ---
  
## ✏클래스 메서드(static메서드)와 인스턴스 메서드  
  
클래스를 정의할때, 어느 경우에 static을 사용해서 클래스 메서드로 정의 해야하는 걸까??  
인스턴스 메서드는 인스턴스 변수와 관련된 작업을 하는, 즉 메서드의 작업을 수행하는데 인스턴스 변수를 필요로 하는 메서드이다.  
그런데 인스턴스 변수는 인스턴스(객체)를 생성해야만 만들어지므로 인스턴스 메서드 역시 인스턴스를 생성해야마나 호출할 수 있는 것이다.  
반면에 메서드 중에서 인스턴스와 관계없느(인스턴스 변수나 인스턴스 메서드를 사용하지 않는)메서드를 클래스 메서드(static)로 정의한다.

|참고|: 클래스 영역에 선언된 변수를 멤버변수라고 함, 멤버변수 중에 static이 붙은 것은 클래스(static변수), static이 붙지 않은 것은  
인스턴스 변수라 함, 멤버변수는 인스턴스 변수와 static변수를 모두 통칭하는 말이다.  


### 1.클래스를 설계할 때, 멤버 변수 중 모든 인스턴스에 공통으로 사용하는 것에 static을 붙인다.  
### 2.클래스 변수(static변수)는 인스턴스를 생성하지 않아도 사용할 수 있다.(클래스변수는 클래스가 메모리에 올라갈떄 이미 자동으로 생성되기 때문)  
### 3.클래스 메서드(static메서드)는 인스턴스 변수를 사용 할수 없다.  
3.1. 클래스 메서드에서 인스턴스 변수의 사용을 금지한다.반면에 인스턴스 변수나 인스턴스 메서드에서는 static이 붙은 멤버들을 사용하는 것이 언제나 가능하다.  
### 4.메서드 내에서 인스턴스 변수를 사용하지 않는다면,static을 붙이는 것을 고려한다.  
4.1. 인스턴스 변수를 필요로 하지 않는다면 static을 붙이자. 메서드 호출시간이 짧아지므로 성능이 향상된다. static을 안붙인 메서드는 실행 시 호출되어야할 메서드를 찾는 과정이 추가적으로 필요하기 때문이다.  


> -클래스의 멤버변수 중 모든 인스턴스에 공통된 값을 유지해야하는 것이 있는지 살펴보고 있으면 static을 붙여준다.  
> -작성한 메서드 중에서 인스턴스 변수나 인스턴스 메서드를 사용하지 않는 메서드에 static을 붙일 것 을 고려한다.  

```java 
  class MyMath2 {
	long a, b;
	
	// 인스턴스변수 a, b만을 이용해서 작업하므로 매개변수가 필요없다.
	long add() 	    { return a + b; }  // a, b는 인스턴스변수
	long subtract() { return a - b; }
	long multiply() { return a * b; }
	double divide() { return a / b; }

	// 인스턴스변수와 관계없이 매개변수만으로 작업이 가능하다.
	static long   add(long a, long b) 	   	 { return a + b; } // a, b는 지역변수
	static long   subtract(long a, long b)   { return a - b; }
	static long   multiply(long a, long b)	 { return a * b; }
	static double divide(double a, double b) { return a / b; }
}

class MyMathTest2 {
	public static void main(String args[]) {
		// 클래스메서드 호출. 인스턴스 생성없이 호출가능
		System.out.println(MyMath2.add(200L, 100L));
		System.out.println(MyMath2.subtract(200L, 100L));
		System.out.println(MyMath2.multiply(200L, 100L));
		System.out.println(MyMath2.divide(200.0, 100.0));

		MyMath2 mm = new MyMath2(); // 인스턴스를 생성
		mm.a = 200L;
		mm.b = 100L;
		// 인스턴스메서드는 객체생성 후에만 호출이 가능함.
		System.out.println(mm.add());
		System.out.println(mm.subtract());
		System.out.println(mm.multiply());
		System.out.println(mm.divide());
	}
}
```    
---  
## ✏클래스 멤버와 인스턴스 멤버간의 참조와 호출  

같은 클래스에 속한 멤버들 간에는 별도의 인스턴스를 생성하지 않고도 서로 참조 또는 호출이 가능하다.단, 클래스 멤버가 인스턴스 멤버를 참조 또는 호출하고자  
하는 경우에는 인스턴스를 생성해야 한다.*그이유는 인스턴스 멤버가 존재하는 시점에 클래스 멤버는 항상 존재하지만, 클래스멤버가 존재하는 시점에 인스턴스 멤버  
가 존재하지 않을 수 있기 때문이다.   

```java 
lass MemberCall {
	int iv = 10;
	static int cv = 20;

	int iv2 = cv;
//	static int cv2 = iv;		// 에러. 클래스변수는 인스턴스 변수를 사용할 수 없음.
	static int cv2 = new MemberCall().iv;	 // 이처럼 객체를 생성해야 사용가능.

	static void staticMethod1() {
		System.out.println(cv);
//		System.out.println(iv); // 에러. 클래스메서드에서 인스턴스변수를 사용불가.
		MemberCall c = new MemberCall();	
		System.out.println(c.iv);   // 객체를 생성한 후에야 인스턴스변수의 참조가능.
}

	void instanceMethod1() {
		System.out.println(cv);		
		System.out.println(iv); // 인스턴스메서드에서는 인스턴스변수를 바로 사용가능.
}

	static void staticMethod2() {
		staticMethod1();
//		instanceMethod1(); // 에러. 클래스메서드에서는 인스턴스메서드를 호출할 수 없음.
		MemberCall c = new MemberCall();
		c.instanceMethod1(); // 인스턴스를 생성한 후에야 호출할 수 있음.
 	}
	
	void instanceMethod2() {	// 인스턴스메서드에서는 인스턴스메서드와 클래스메서드
		staticMethod1();		//  모두 인스턴스 생성없이 바로 호출이 가능하다.
		instanceMethod1();
	}
}
``` 
> 인스턴스멤버(인스턴스 변수와 인스턴스메서드)는 반드시 객체를 생성한 후에만 참조 또는 호출이 가능하기 때문에 클래스멤버가 인스턴스멤버를 참조, 호출하기 위해서  
> 객체를 생성해야만 한다. 하지만, *인스턴스 멤버간의 호출에는 아무런 문제가 없다. 하나의 인스턴스멤버가 존재한다는 것은 인스턴스가 이미 생성되어있다는 것을  
> 의미하며, 즉 다른 인스턴스멤버들도 모두 존재하기 때문이다. 

```java
memberCall c = new MemberCall();
int result = c.instanceMethod1();
//위의 두 줄을 다음과 같이 한 줄로 할 수 있다.
int = result = new MemberCall().instanceMethod1();
//대신 참조변수(c)를 선언하지 않았기 때문에 생성된 MemberCall 인스턴스는 더 이상 사용할 수 없다.
``` 

# 끝

