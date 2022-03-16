# ✏오버라이딩  
조상 클래스로부터 상속받은 메서드의 내용을 변경하는 것을 오버라이딩이라고 한다. 상속받은 메서드를 그대로 사용하기도  
하지만, 자손 클래스 자신에 맞게 변경해야하는 경우가 많다. 이럴 때 조상의 메서드를 오버라이딩한다.  
  
참고- override의 사전적 의미는 '~위에 덮어쓰다'이다.  

### 오버라이딩의 조건  
오버라이딩은 메서드의 내용만을 새로 작성하는 것이므로 메서드의 선언부는 조상의 것과 완전히 일치해야한다.  
그래서 오버라이딩이 성립하기 위해서는 다음과 같은 조건을 만족해야한다.  

> 자손클래스에서 오버라이딩하는 메서드는 조상 클래스의 메서드와  
> -이름이 같아야 한다.  
> -매개변수가 같아야한다.  
> -반환타입이 같아야한다.  

**다만 접근제어자와 예외는 제한된 조건 하에서만 다르게 변경할 수 있다.  

#### 1.접근 제어자는 조상 클래스의 메서드보다 좋은 범위로 변경할 수 없다.  
만일 조상 클래스에 정의된 메서드의 접근 제어자가 protected라면, 이를 오버라이딩하는 자손 클래스의 메서드는  
접근 제어자가 protected나 public 이어야 함. 대부분 같은 범위의 접근 제어자 사용.  접근 제어자의 접근 범위를  
넓은 것에서 좋은 것 순으로 나열 public->protected,(default),private이다.  

#### 2.조상 클래스의 메서드보다 많은 수의 예외를 선언할 수 없다.  
throws Exception의 수가 조상이 더 많아야함.  
단*개수의 문제가 아닌 Exception이라는 예외는 최고의 조상이므로 많은 개수의 예외를 던질 수 있도록 선언한 것이기 때문에  
주의해야함 

> 조상 클래스의 메서드를 자손 클래스에서 오버라이딩할 때  
> 1.접근 제어자를 조상 클래스의 메서드보다 좁은 범위로 변경 할 수 없다.(public,private등)    
> 2.예외는 조상 클래스의 메서드보다 많이 선언할 수 없다.  
> 3.인스턴스메서드를 static메서드로 또는 그 반대로 변경 할 수 없다.  

여기서 질문? 조상 클래스에 정의된 static메서드 같은 경우는 자손 클래스에서 똑같은 이름의 static메서드로 정의가능한가?  
답변: 가능하지만 이것은 각 클래스에 별개의 static메서드를 정의한 것일 뿐 오버라이딩이 아님, static멤버들은 자신들이  
정의된 클래스에 묶여있다고 생각해라.  

--- 

## 오버로딩 vs 오버라이딩  
오버로딩과 오버라이딩은 서로 혼동하기 쉽지만 사실 그차이는 명백하다. 오버로딩은 기존에 없는 새로운  
메서드를 추가하는 것이고, 오버라이딩은 조상으로부터 상속받은 메서드의 내용을 변경하는 것이다.  

> 오버로딩(overloading) 기존에 없는 새로운 메서드를 정의하는 것(new).  
> 오버라이딩(overriding) 상속받은 메서드의 내용을 변경하는 것(change, modify). 


--- 

## super  
super는 자손 클래스에서 조상 클래스로부터 상속받은 멤버를 참조하는데 사용되는 참조변수이다.  멤버변수와 지역변수의  
이름이 같을 때 this를 붙여서 구별했듯이 상속받은 멤버와 자신의 멤버와 이름이 같을 때는 super를 붙여서 구별할 수 있다.  
조상 클래스로부터 상속받은 멤버도 자손 클래스 자신의 멤버이므로 super대신 this를 사용할 수 있다.그래도 조상 클래스의  
멤버와 자손클래스의 멤버가 중복 정의되어 서로 구별해야하는 경우에만 super를 사용하는 것이 좋다.  
*조상의 멤버와 자신의 멤버를 구별하는데 사용된다는 점을 제외하고는 super와 this는 근본적으로 같다.   

static메서드는 인스턴스와 관련이 없다.그래서 this와 마찬가지로 super역시 static메서드에서는 사용할 수 없고 인스턴스 메서드에서만  
사용할 수 있다.  


```java
class ProductTest {
	public static void main(String args[]) {
		Child c = new Child();
		c.method();
	}
}

class Parent {
	int x=10;
}

class Child extends Parent {
	void method() {
		System.out.println("x=" + x);
		System.out.println("this.x=" + this.x);
		System.out.println("super.x="+ super.x);
    //이경우 x,this.x,super.x 모두 같은 변수를 의미하므로 모두 같은 값이 출력되었다.  
	}
}

```
실행결과:  
x=10  
this.x=10  
super.x=10  

```java
class ProductTest {
	public static void main(String args[]) {
		Child c = new Child();
		c.method();
	}
}
class Parent {
	int x=10;	//같은이름의 조상 멤버변수  
}

class Child extends Parent {
	int x=20;   //같은이름의 자손 멤버변수  

	void method() {
		System.out.println("x=" + x);
		System.out.println("this.x=" + this.x);
		System.out.println("super.x="+ super.x);	//조상클래스의 멤버변수의 값을 출력  
	}   //이처럼 조상 클래스에 선언된 멤버변수와 같은 이름의 멤버변수를 자손 클래스에서 중복해서 정의하는 것이 가능하며  
      //참조변수 super를 이용해서 서로 구별할 수 있다.   
}
```
실행결과:  
x=20  
this.x=20  
super.x=10  

--- 

### super() - 조상 클래스의 생성자  
this()와 마찬가지로 super() 역시 생성자이다. this()는 같은 클래스의 다른 생성자를 호출하는데 사용되지만,  
super()는 조상 클래스의 생성자를 호출하는데 사용된다.  
*생성자의 첫줄에서 조상클래스의 생성자를 호출해야하는 이유는 자손 클래스의 멤버가 조상 클래스의 멤버를 사용할 수도 있으므로  
조상의 멤버들이 먼저 초기화되어 있어야 하기 때문이다.  
Object클래스를 제외한 모든 클래스의 생성자는 첫 줄에 반드시 자신의 다른 생성자 또는 조상의 생성자를 호출해야한다.  
그렇지 않으면 컴파일러는 생성자의 첫 줄에 'super();'를 자동적으로 추가할 것이다.  

> Object 클래스를 제외한 모든 클래스의 생성자 첫 줄에 생성자,this() 또는 super(),를 호출해야 한다.  
> 그렇지 않으면 컴파일러가 자동적으로 'super();를 생성자의 첫줄에 삽입한다. 
>     
인스턴스를 생성할 때는 클래스를 선택하는 것만큼 생성자를 선택하는 것도 중요하다.  

> 1.클래스 - 어떤 클래스의 인스턴스를 생성할 것인가?  
> 2.생성자 - 선택한 클래스의 어떤 생성자를 이용해서 인스턴스를 생성할 것인가?  


```java
class PointTest {
	public static void main(String args[]) {
		Point3D p3 = new Point3D(1,2,3);
	}
}

class Point {
	int x;	
	int y;

	Point(int x, int y) {
		this.x = x;
		this.y = y;
	}

	String getLocation() {
		return "x :" + x + ", y :"+ y;
	}
}

class Point3D extends Point {
	int z;

	Point3D(int x, int y, int z) {
			// <- 생성자 첫 줄에서 다른 생성자를 호출하지 않기 때문에 컴파일러가 'super();'를 여기에 삽입한다.  
			//super()는 Point3D의 조상인 Point클래스의 기본 생성자인 Point()를 의미한다.   
		this.x = x;
		this.y = y;
		this.z = z;
	}

	String getLocation() {	// 오버라이딩
		return "x :" + x + ", y :"+ y + ", z :" + z;
	}	
}
```
실행결과:  
에러  
이유:Point3D클래스의 생성자에서 조상 클래스의 생성자인 Point()를 찾을 수 없다는 내용이다.  

해설:Point3D클래스의 생성자의 첫 줄이 생성자(조상의것이든 자신의 것이든)를 호출하는 문장이 아니기 때문에 
컴파일러는 자동적으로 'super();'를 Point3D클래스의 생성자의 첫줄에 넣어준다.  

수정:이 에러를 수정하려면, Point클래스에 생성자 Point()를 추가해주던가, 생성자 Point3D(int x, int y , int z)  
의 첫줄에서 Point(int x , int y)를 호출하도록 변경하면 된다.  

super(x,y);// 조상클래스의 생성자 Point(int x, int y)를 호출한다.  *이걸 자손 클래스에 넣어주면 문제없이 컴파일이 된다.  

*생성자가 정의되어 있는 클래스에는 컴파일러가 기본 생성자를 자동적으로 추가하지 않는다.  

```java
class ProductTest {
	public static void main(String argsp[]) {
		Point3D p3 = new Point3D();
		System.out.println("p3.x=" + p3.x);
		System.out.println("p3.y=" + p3.y);
		System.out.println("p3.z=" + p3.z);
	}
}

class Point {
	int x=10;	
	int y=20;

	Point(int x, int y) {
			//<- 생성자 첫줄에서 다른 생성자를 호출하지 않기 때문에 컴파일러가 'super();'를 여기에 삽입한다. 
		        //super()는 Point의 조상인 Object클래스의 기본 생성자인 obejct()를 의미한다.  
		this.x = x;
		this.y = y;
	}
}

class Point3D extends Point {
	int z=30;

	Point3D() {
		this(100, 200, 300);	// Point3D(int x, int y, int z)를 호출한다.
	}

	Point3D(int x, int y, int z) {
		super(x, y);			// Point(int x, int y)를 호출한다.
		this.z = z;
	}
}
```
컴파일결과:  
p3.x=100  
p3.y=200  
p3.z=300  

설명:  
Point3D클래스의 인스턴스를 생성할 때 어떤 순서로 인스턴스의 초기화가 진행되는 지 보여주기 위한 예제이다.  
Point클래스의 생성자 Point(int x, int y)는 어떠한 생성자도 호출하고 있지 않기 때문에 컴파일 한 후에 다음과  
같은 코드로 변경된다. 그래서 'Point3D p3 = new Point3D();와 같이 인스턴스를 생성하면, 아래와 같은 순서로  
생성자가 호출된다  

Point3D() -> Point3D(int x, int y,int z) -> Point(int x, int y) -> Object()(main).   

어떤 클래스의 인스턴스를 생성하면, 클래스 상속관계의 최고조상인 Object클래스까지 거슬러 올라가면서  
모든 조상 클래스의 생성자가 순서대로 호출된다는 것을 알수 있다.  

---

## Package와 import. 
패키지란, 클래스의 묶음이다. 패키지에는 클래스 또는 인터페이스를 포함시킬 수 있으며, 서로 관련된 클래스들끼리 그룹 단위로 묶어 놓음으로써  
클래스를 효율적으로 관리할 수 있다. 지금까지 단순히 클래스 이름으로만 클래스를 구분 했지만, 사실 클래스의 실제 이름은 패키지명을 포함한 것이다.  
예를들면 String클래스의 실제 이름은 java.lang.String이다. java.lang패키지에 속한 String클래스라는 의미이다.  
*클래스가 물리적으로 하나의 클래스파일(.class)인 것과 같이 패키지는 물리적으로 하나의 디렉토리다.  

> -하나의 소스파일에는 첫 번째 문장으로 단 한 번의 패키지 선언만을 허용한다.  
> -모든 클래스는 반드시 하나의 패키지에 속해야 한다.  
> -패키지는 점(.)을 구분자로 하여 계층구조로 구성할 수 있다.  
> -패키지는 물리적으로 클래스 파일(.class)을 포함하는 하나의 디렉토리이다.  

### 패키지의 선언  
패키지를 선언하는 것은 아주 간단핟.  클래스나 인터페이스의 소스파일(.java)의 맨 위에 다음과 같이 한줄만 적어주면 된다.  
> package 패키지명;  

```java
package com.javachobo.book;

class PackageTest {
	public static void main(String[] args) {
		System.out.println("Hello World!");
	}
}
``` 
실행결과:  
경로:\>javac -d . PackageTest.java  
-d 옵션은 소스파일에 지정된 경로를 통해 패키지의 위치를 찾아서 클래스파일을 생성한다.  
지정된 패키지가 없으면 자동적으로 생성된다  

---

## import문  
소스코드를 작성할 때 다른 패키지의 클래스를 사용하려면 패키지명이 포함된 클래스 이름을 사용해야한다.  
-참고 import문은 프로그램의 성능에 전혀 영향을 미치지않는다. import문을 많이 사용하면 컴파일 시간이 아주 조금 더  
걸릴 뿐이다.  

### import문의 선언  
모든 소스파일(.java)에서 importansdms package문 다음에, 그리고 클래스 선언문 이전에 위치 해야한다.  
import문은 package문과 달리 한 소스 파일에 여러 번 선언 할수 있다.  

> 일반적인 소스파일(*.java)의 구성은 다음의 순서로 되어 있다.  
> 1.package문  
> 2.import문  
> 3.클래스 선언  

import문 선언 방법  
> import 패키지명.클래스명;  
> 또는  
> import 패키지명.*;  //*는 전체중 필요한것을 찾아줌  

참고- import문에서 클래스의 이름 대신 '*'을 사용하는 것이 하위 패키지의 클래스까지 포함하는 것은 아니라는 것이다.  
> import java.util.*;  
> import java.text.*;  
위의 두 문장 대신 다음과 같이 할수는 없다.  

> import java.*;  

```java
package javatest;
import java.text.SimpleDateFormat;
import java.util.Date;

class ProductTest 
{
	public static void main(String[] args) 
	{
		 Date today = new Date();
		 
		 SimpleDateFormat date = new SimpleDateFormat("yyyy/MM/dd");
		 SimpleDateFormat time = new SimpleDateFormat("hh:mm:ss a");

		 System.out.println("오늘의 날짜는" + date.format(today));
		 System.out.println("현재 시간은" + time.format(today));
	}
}
``` 
컴파일결과:  
오늘의 날짜는2022/03/17  
현재 시간은01:20:29 오전  

설명: SimpleDateFormat과 Date클래스는 다른 패키지에 속한 클래스이므로 import문으로 어느 패키지에 속하는  
클래스인지 명시해주었다. 그래서 소스에서 클래스이름 앞에 패키지명을 생략할 수 있었다. 만일 import문을 지정하지 
않았다면 클래스이름에 패키지명도 적어줘야 했을 것이다 .  

---

### static import문  
import문을 사용하면 클래스의 패키지명을 생략할 수 있는 것과 같이 static import문을 사용하면 static 멤버를  
호출할 때 클래스 이름을 생략할 수 있다.특정 클래스의 static멤버를 자주 사용할 때 편리하다. 그리고 코드도 간결해짐  

```java 
import static java.lang.Integer.*; 	//Integer클래스의 모든 static메서드  
import static java.lang.Math.random;    //Math.random()만. 괄호 안붙임.  
import static java.lang.Sustem.out;   	//Systme.out을 out만으로 참조가능  
``` 
만일 위의 같이 static import문을 선언하였다면, 아래의 왼쪽 코드를 오른쪽 코드와 같이 간략히 할수 있다.  
```java 
	System.out.println(Math.random()); --->  out.prinln(random());  
```









