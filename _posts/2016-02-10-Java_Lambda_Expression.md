---
layout: post
title: Java - Lambda Exression
comments: true
---

<br><br>

## 개요

----

컴퓨터과학 및 수리논리학에서 함수 정의, 함수 적용, 귀납적 함수를 추상화한 형식 체계이다. 1930년대 알론조 처치가 수학기초론을 연구하는 과정에서 람다 대수의 형식을 제안하였다. 

알론조 처치가 제안한 람다는 프로그래밍 언어가 아니라 수학적 추상화였지만, 이후에 함수형 프로그래밍의 근간을 이루었다.

함수형 프로그래밍이란, 한 번쯤은 들어봤을 '절차지향' 또는 '객체지향'과 같이 프로그래밍 패러다임중의 하나이다. 객체지향 프로그래밍이 **명령형 프로그래밍(Imperative Programming)**에 속해있는 반면 함수형 프로그래밍은 함수간의 속성과 관계를 선언하여 최종 결과를 계산하는 방법을 기술하는 형식으로써 **선언형 프로그래밍(Declarative Programming)**에 속한다.
  
아래 표는 명령형 방법과 함수형 방법의 차이를 간략하게 설명해준다.

![imperative_functional]({{ site.url }}/img/imperative_functional.JPG)

<br>

## 맛보기 (경험담)

----

예전에 우연히 C++에서 람다식을 활용한 성능향상에 관련된 문서를 읽고 C++에서 간단한 람다식을 구현해본 적이 있는데, 아주 잠깐 람다식에 대해 맛보고 느꼈던 경험에 대해 얘기해보고자 한다.

C++에서 선형 자료구조(배열, vector, ..)에 저장된 데이터를 정렬하기 위해 `STL::sort()`를 사용하는데, int와 같이 원소들간 비교 기준이 정해진 primitive타입이 아닌 사용자정의 데이터 타입에 대해서는 컴파일러에게 '이 데이터는 이걸 기준으로 비교를 해줘'라는 코드를 작성해주어야 한다.

가령, 아래와 같이 학생 정보를 저장하는 데이터 타입을 정의하고 시험점수(score)를 기준으로 데이터를 정렬하기 위해 `operator <`를 통해 비교 기준을 정의할 수 있다.

```cpp
class Student {
public:
	string name;
	int score;

	bool operator < (const Student & student) const {
		return score < student.score;
	}
};
```

위에서 정의한 비교기준에 의해 아래와 같이 `STL::sort()`를 사용할 수 있다.

```cpp
int main() {
	vector <Student> students = {
		{ "Jack", 80 },
		{ "John", 100 },
		{ "Mike", 75 }
	};
	sort(students.begin(), students.end());
}
```

여기서 람다식을 적용하면 위와 같이 클래스 내부에 `operator <`를 통한 비교기준을 정의하지 않더라도 아래와 같이 sort()함수를 이용할 수 있다. 원래 sort()함수의 세 번째 매개변수는 정렬하려는 데이터의 비교기준을 명확히 정의한 bool타입을 반환형으로 하는 함수의 이름(함수가 정의된 주소영역)을 넘겨줘야 하는데, 람다식을 이용한 익명함수를 통해 인자를 전달하는 영역에 곧장 비교기준을 정의한 것이다.

```cpp
int main() {
	vector <Student> students = {
		{ "Jack", 80 },
		{ "John", 100 },
		{ "Mike", 75 }
	};
	
	sort(students.begin(), students.end(),
		[](Student & a, Student & b) {
			return a.score < b.score;
		}
	);
}
```

처음 썼을땐 **[](){}**를 연달아 쓰는 모양이 이상하기도 하고, 함수호출의 인자로 전달하는 부분에 익명함수를 정의하는 중괄호 {..} 영역을 바로 쓰는것이 이상하게 보이기도 하고 매우 거북스러웠다.

그런데, 정말로 성능이 향상되는지에 대한 의문에 단순히 (x, y)좌표를 표현하는 class를 선언하고 x좌표를 우선기준으로 정렬하는 프로그램을 간단히 짜보았는데, 100만개/1000만개 데이터에서 람다식을 활용한 경우가 확실히 수행시간이 더 빠르게 나와서(증명 자료는 없다..) 람다식을 제대로 써먹을 줄은 모르지만, 위와같이 단순히 정렬 기준을 정의한다거나 하는 간단한 익명함수를 경우에는 종종 람다식을 사용해왔다.

그러다 어느날 같은 주제에 대해 친구와 개별적으로 코드를 짜던중에 친구의 코드를 보면서 느낀점이 있었는데, 불과 200줄 남짓한 코드였지만 main함수부터 시작해서 실행흐름을 살펴보면서 특정 함수를 호출하는 부분을 만나면 해당 함수가 어떻게 수행되는지 보기위해 해당 함수가 정의된 영역으로 스크롤을 이동해보고.. 다시 돌아오고.. 하던 작업을 반복하다보니 문득 람다식 두어개가 들어간 자신의 코드를 보는데는 그런 수고가 필요 없음을 느끼고 람다식에 대해 작은 경의로움(?)을 느꼈던 적이 있다.

<br>

## Java8 - Lambda 예제

----

```java
class Person {
    private String name;
    private String eMail;
    private String phone;
    private String address;
    private String gender;
    private int age;
}

public class Main {
    public static void main(String[] args) {
        List<Person> personList = Person.createShortList();
    }
}
```

아래는 위에서 정의한 Person타입에 해당하는 데이터들이 담긴 personList를 정렬하는 작업을 표현한 코드이다.

```java
// 평소방식
Collections.sort(personList, new Comparator<Person>(){
    public int compare(Person p1, Person p2){
        return p1.getName().compareTo(p2.getName());
    }
});

// Lambda
Collections.sort(personList,
        (Person p1, Person p2) -> p1.getName().compareTo(p2.getName()));
```

첫 번째 방식은 `Collections.sort()`함수의 두 번째 인자로 전달해야하는 데이터의 비교기준을 표현하는데 Comparator 클래스의 인스턴스를 생성하여 추상메소드인 `compare()`를 곧장 명시해주는 경우이다.

두 번째 방식은 람다식을 적용하여 정렬하는 코드를 한 줄로 우아하게 표현한 경우이다. 짧으면서도 읽는데 무리없이 직관적으로 쉽게 이해할 수 있다. 만약, 첫 번째 방식에서 Comparator를 상속받아 비교기준을 정의한 클래스를 외부에 정의하고 `Collections.sort()`함수의 두 번째 인자로 전달했다면 정렬하는 작업을 표현한 코드가 여러곳에 분산되어 코드를 읽는데 더 어려웠을 것이다.

<br>

## Lambda (작성중.. 언젠가..)

----

<br>

## 왜 쓰는가 ?

----

그렇다면 람다식을 사용해서 좋은점이 무엇일까 ?

앞에서 언급한 **가독성**에 관한것이 전부... 일리가 없다. 다양한 장점들 중에 병렬 프로그래밍과 관련한 내용이 있었는데, 다행히 대략적인 전개정도는 쉽게 파악할 수 있게 잘 정리해준 [블로그 포스트](http://tmondev.blog.me/220288794560)를 발견하여 이해하는데 많은 도움이 되었다.

요약하자면, 무어의 법칙(Moore's law)대로 반도체 직접회로의 성능이 18개월마다 2배씩 증가하고 있었는데 어느 순간부터 반도체 산업의 모든 경쟁 업체들이 최대 clock speed 4GHz를 넘지 못하고 있었다. 그래서 '하나의 성능을 더 못 올리면, 여러개를 넣자!'라는 생각으로 이때부터 CPU칩 내부에 다수의 코어를 장착한 **멀티코어** CPU가 등장하게 되었다. 멀티코어의 등장은 프로그래머들에게 새로운 고민거리를 안겨주었는데, 예전에는 프로그램을 짜두면 하드웨어의 발전에 따라 프로그램의 성능도 향상된 반면에 앞으로는 CPU내부에 있는 코어들에게 프로그램의 작업량을 적절히 분배해서 다중 코어들을 잘 활용하는 **병렬 프로그래밍**이 필수적인 요소가 된 것이다.

그런데 이 병렬 프로그래밍이 생각보다 쉽지가 않다. 작업량을 그냥 무식하게 N등분해서 분배하면 될 일이 아니라, 각 작업의 단위들이 공유하는 자원이 있다거나.. 서로 연관관계가 있는 경우 race condition이나 deadlock을 유발할 가능성이 있다.

병렬 프로그래밍과 관련한 내용으로 앞에서 언급한 블로그에서 게시한 [포스트](http://blog.naver.com/PostView.nhn?blogId=tmondev&logNo=220412722908&parentCategoryNo=&categoryNo=6&viewDate=&isShowPopularPosts=false&from=postView)의 내용중 일부를 적어보자면..

아래는 학생이라는 데이터 타입에 대해 다수의 학생들 중 최고 점수를 얻은 학생을 찾는 프로그램 코드이다.

```java
class Student 
{ String name; int gradYear; double score; } 
 
List students = ...; 
 
double max = Double.MIN_VALUE; 
for (Student s : students) { 
   if (s.gradYear == 2011) max = Math.max(max, s.score); 
} 
return max; 
```

만약 List에 꽤 많은 원소들이 저장되어 있어, 이를 병렬 프로그래밍으로 해결하고자 코드를 작성하면..

```java
public class Task extends java.util.concurrent.RecursiveAction { 
    List students; 
    Task(List as) { students = ss; } 
    Task leftHalf() { 
         int n = students.size(); 
        return new Task (students.subList(0, n/2)); 
     } 
    Task rightHalf() { 
          int n = students.size(); 
           return new Task (students.subList(n/2, n)); 
    } 
 
   double computeSequentially() { 
        double max = Double.MIN_VALUE; 
        for (Student s : students) { 
           if (s.gradYear == 2011) max = Math.max(max, s.score); 
       } 
       return max; 
    } 
 
    static final int SEQUENTIAL_THRESHOLD = 1 << 14; 
    double result; 
 
    protected void compute() {
      if (students.size() < SEQUENTIAL_THRESHOLD) {
           result = computeSequentially(); 
      } else { 
         Task left = leftHalf(); 
         Task right = rightHalf(); 
          invokeAll(left, right); // INVOKE-IN-PARALLEL (병행 처리 시작) 
          result = Math.max(left.result, right.result);
         } 
     } 
 
     static double compute(List as) { 
          ForkJoinPool pool = new ForkJoinPool(); 
         Task t = new Task(ss); 
         pool.invoke(t); 
         return t.result;
         } 
  } 
```

간단한 프로그램조차 병렬 프로그래밍으로 바꾸는게 쉽지 않은 작업임을 확실하게 느낄 수 있다.

아래의 코드는 자바8 스트림API와 람다를 적용하여 위의 코드를 개선한 것이다.

```java
double max = students.parallel()
                           .filter(s -> s.gradYear == 2011) 
                           .map(s -> s.score) 
                           .reduce(0.0, Math#max);
```

* 원본출처 : [http://www.oracle.com/kr/corporate/magazines/winter-tech2-1429486-ko.pdf](http://www.oracle.com/kr/corporate/magazines/winter-tech2-1429486-ko.pdf)

스트림 API 내용을 몰라서.. 코드 한 줄 한 줄 의미를 파악하기에는 조금 무리가 있지만, 직전의 코드와 단순하게만 비교해보아도 월등하게 차이가 난다는것을 알 수 있다.

<br>

## 참고문서

----

[https://ko.wikipedia.org/wiki/함수형_프로그래밍](https://ko.wikipedia.org/wiki/%ED%95%A8%EC%88%98%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)
[http://tmondev.blog.me/220412722908?Redirect=Log&from=postView](http://tmondev.blog.me/220412722908?Redirect=Log&from=postView)
[http://tmondev.blog.me/220288794560](http://tmondev.blog.me/220288794560)
[http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/Lambda-QuickStart/index.html#section2](http://www.oracle.com/webfolder/technetwork/tutorials/obe/java/Lambda-QuickStart/index.html#section2)
[http://www.oracle.com/kr/corporate/magazines/winter-tech2-1429486-ko.pdf](http://www.oracle.com/kr/corporate/magazines/winter-tech2-1429486-ko.pdf)


