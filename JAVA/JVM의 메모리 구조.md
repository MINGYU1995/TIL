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





