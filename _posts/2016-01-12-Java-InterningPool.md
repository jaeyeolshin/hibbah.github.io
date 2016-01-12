---
layout: post
title: Java - Interning Pool
comments: true
---

----

## 요약

C++문법에 익숙한 사람이 Java의 세계에 입문하면서 부딪히게 되는 몇 가지 오류들에 대해 다룬다.

이 포스트에서는 Java의 String객체를 다루는데 있어서 평소 C++에서 사용했던 비교연산(**==**)을 그대로 사용함으로써 마주하게 되는 혼란과, 더 나아가 **Interning Pool**, **Immutable Object**에 대해 중점적으로 이야기한다.

----

## 의문의 시작 (고통의 시작)

{% highlight cpp %}
// C++
string str1 = "abc";
string str2 = "abc";

if(str1 == str2) // true
  cout<<"Same"<<endl; // print "Same"
{% endhighlight %}
위의 C++코드는 의도한대로 "Same"을 출력한다.



{% highlight java %}
// Java
String str1 = "abc";
String str2 = "abc";

if(str1 == str2) // true
  System.out.println("Same"); // print "Same"
{% endhighlight %}
Java또한 마찬가지로 **의도**한대로 "Same"을 출력한다.

(사실 "Same"이 출력되는 이유가 **의도**와는 전혀 다른 이유지만, 결과적으로 "Same"이 출력되어 버리기 때문에 아래 코드의 결과에서 혼란을 마주하게 된다.)

'아! Java에서는 객체 생성을 new연산자를 통해 할당하지!'라는 생각으로 객체 생성법만 달리할 뿐, 위와 동일한 코드를 작성한다.



{% highlight java %}
// Java
String str1 = new String("abc");
String str2 = new String("abc");

if(str1 == str2) // false
  System.out.println("Same"); // skip
{% endhighlight %}
분명 "Same"이 출력될 것을 기대하였으나, 아무런 메시지를 출력하지 않는 콘솔창을 보고 몇 번이나 재실행 해보지만 콘솔은 묵묵하게 정적을 유지한다.

----

## Interning Pool

결론부터 얘기하면 Java에서 String객체의 비교는 .equals() 메서드를 이용해야 한다. 직전의 코드를 '두 String 객체의 값을 비교'하는 의도대로 작성한 올바른 코드는 아래와 같다.
{% highlight java %}
// Java
if(str1.equals(str2)) // true
  System.out.println("Same"); // print "Same"
{% endhighlight %}

C++에서 string 객체에 대해 비교연산자(==)를 사용하면 string class 내부적으로 정의된 operator overloading의 동작을 수행하게 된다.
{% highlight cpp %}
// C++
class string{
  ...
  
  bool operator == (const String & str){
    // 길이가 다르면 return false
    // 길이가 같으면 1byte단위로 문자열 비교
  }
};
{% endhighlight %}


스트링 데이터를 다룬다.
으억.. 비교연산이 의도한대로 동작하지 않는다. Java가 밉다.
Equals()함수를 이용해야한다.

----

## Interning Pool


----

## Immutable Object

{% highlight java %}
String str1 = "abc";
String str2 = "abc";

str1 += "def";

if(str1 == str2) // false
  System.out.println("Same");
{% endhighlight %}


![testimg2]({{ site.url }}/img/spongebob.png)


