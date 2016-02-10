---
layout: post
title: Java - Lambda Exression
comments: true
---

<br><br>

----

## 개요

컴퓨터과학 및 수리논리학에서 함수 정의, 함수 적용, 귀납적 함수를 추상화한 형식 체계이다. 1930년대 알론조 처치가 수학기초론을 연구하는 과정에서 람다 대수의 형식을 제안하였다. 

알론조 처치가 제안한 람다는 프로그래밍 언어가 아니라 수학적 추상화였지만, 이후에 함수형 프로그래밍의 근간을 이루었다.

함수형 프로그래밍이란, 한 번쯤은 들어봤을 '절차지향' 또는 '객체지향'과 같이 프로그래밍 패러다임중의 하나이다. 객체지향 프로그래밍이 **명령형 프로그래밍(Imperative Programming)**에 속해있는 반면 함수형 프로그래밍은 함수간의 속성과 관계를 선언하여 최종 결과를 계산하는 방법을 기술하는 형식으로써 **선언형 프로그래밍(Declarative Programming)**에 속한다.
  
아래 표는 명령형 방법과 함수형 방법의 차이를 간략하게 설명해준다.

![imperative_functional]({{ site.url }}/img/imperative_functional.JPG)

<br>

----

## 맛보기 (경험담)

예전에 우연히 C++에서 람다식을 활용한 성능향상에 관련된 문서를 읽고 C++에서 간단한 람다식을 구현해본 적이 있는데, 아주 잠깐 람다식에 대해 맛보고 느꼈던 경험에 대해 얘기해보고자 한다.

C++에서 선형 자료구조(배열, vector, ..)에 저장된 데이터를 정렬하기 위해 `STL::sort()`를 사용하는데, int와 같이 원소들간 비교 기준이 정해진 primitive타입이 아닌 사용자정의 데이터 타입에 대해서는 컴파일러에게 '이 데이터는 이걸 기준으로 비교를 해줘'라는 코드를 작성해주어야 한다.

가령, 아래와 같이 학생 정보를 저장하는 데이터 타입을 정의하고 시험점수(score)를 기준으로 데이터를 정렬하기 위해 `operator <`를 통해 비교 기준을 정의할 수 있다.

{% highlight cpp %}
class Student {
public:
	string name;
	int score;

	bool operator < (const Student & student) const {
		return score < student.score;
	}
};
{% endhighlight %}

위에서 정의한 비교기준에 의해 아래와 같이 `STL::sort()`를 사용할 수 있다.

{% highlight cpp %}
int main() {

	vector <Student> students = {
		{ "Jack", 80 },
		{ "John", 100 },
		{ "Mike", 75 }
	};
	
	sort(students.begin(), students.end());
}
{% endhighlight %}

여기서 람다식을 적용하면 데이터 타입을 정의하는 클래스 내부에 비교기준을 정의하지 않더라도 아래와 같이 sort()함수를 이용할 수 있다.

{% highlight cpp %}
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
{% endhighlight %}

처음 썼을땐 **[](){}**를 연달아 쓰는 모양이 이상하기도 하고, 함수호출의 인자로 전달하는 부분에 익명함수를 정의하는 중괄호 {..} 영역을 바로 쓰는것이 이상하게 보이기도 하고 매우 거북스러웠다.

그런데, 정말로 성능이 향상되는지에 대한 의문에 단순히 (x, y)좌표를 표현하는 class를 선언하고 x좌표를 우선기준으로 정렬하는 프로그램을 간단히 짜보았는데, 100만개/1000만개 데이터에서 람다식을 활용한 경우가 확실히 수행시간이 더 빠르게 나와서(증명 자료는 없다..) 람다식을 제대로 써먹을 줄은 모르지만, 위와같이 단순히 정렬 기준을 정의한다거나 하는 간단한 익명함수를 경우에는 종종 람다식을 사용해왔다.

그러다 어느날 같은 주제에 대해 친구와 개별적으로 코드를 짜던중에 친구의 코드를 보면서 느낀점이 있었는데, 불과 200줄 남짓한 코드였지만 main함수부터 시작해서 실행흐름을 살펴보면서 특정 함수를 호출하는 부분을 만나면 해당 함수가 어떻게 수행되는지 보기위해 해당 함수가 정의된 영역으로 스크롤을 이동해보고.. 다시 돌아오고.. 하던 작업을 반복하다보니 문득 람다식 두어개가 들어간 자신의 코드를 보는데는 그런 수고가 필요 없음을 느끼고 람다식에 대해 작은 경의로움(?)을 느꼈던 적이 있다.

<br>

----

## 람다식 실전




<br>

----

## 왜 쓰는가 ?

가독성이 전부일리가..

4Ghz 장벽.. 멀티코어... 멀티코어를 잘 활용하는 프로그래밍..(DX11 DX12 속도 비교 사례)

병렬 프로그래밍 코드를 짜는데 고려해야 할 사항들.. 각 실행흐름들 간에 공유하는 자원들로 인해 발생하는 race condition..  복잡..

근데 java lambda쓰면 알아서 다 해준다..

좀 어렵..




<br>

----

## 부제




<br>

----

## 부제




<br>

----

## 참고문서

https://ko.wikipedia.org/wiki/%ED%95%A8%EC%88%98%ED%98%95_%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D

http://tmondev.blog.me/220412722908?Redirect=Log&from=postView

http://docs.oracle.com/javase/tutorial/java/javaOO/lambdaexpressions.html

http://www.oracle.com/kr/corporate/magazines/winter-tech2-1429486-ko.pdf









