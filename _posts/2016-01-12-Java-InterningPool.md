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
분명 "Same"이 출력될 것을 기대하였으나 콘솔은 아무것도 출력하지 않는다. Java가 미워지기 시작한다...


----

## ==연산자, 객체, 참조변수에 대한 이해

결론부터 얘기하면 Java에서 String객체의 비교는 .equals() 메서드를 이용해야한다. 직전의 코드를 '두 개의 String객체 값을 비교'하는 의도대로 작성한 올바른 코드는 아래와 같다.
{% highlight java %}
// Java
if(str1.equals(str2)) // true
  System.out.println("Same"); // print "Same"
{% endhighlight %}

C++에서는 string 객체에 대해 비교연산자(==)를 사용하면 string class 내부적으로 정의된 operator overloading의 동작을 수행하게 된다.
{% highlight cpp %}
// C++
class string{
  ...
  
  bool operator == (const String & str){
    // 길이가 다르면 return false
    // 길이가 같으면 character단위로 문자열 비교
  }
};
{% endhighlight %}

그럼, Java에서 비교연산자(==)는 어떻게 동작하는 것일까?

분명히 오류없이 실행은 되었기에 문법상의 문제가 아니라 '두 객체의 값을 비교'한다는 **의도**와는 다른 동작을 수행했음을 짐작할 수 있다.

연산자의 동작에 대해 알아보기전에 우선적으로 다들 잘 알고있는 **객체**와 **참조변수**에 관한 아래의 Java 기본 내용을 되짚어본다.

{% highlight java %}
Object obj = new Object();
{% endhighlight %}

할당연산자(=)를 기준으로 우측의 new Object()는 실제로 Object타입의 **객체**를 힙(heap)메모리에 생성하고, 좌측의 Object obj는 방금 생성한 객체를 코드상에서 참조하여 다루기위해 생성된 객체의 참조값(주소값?)을 저장하는 **참조변수**이다.

따라서, 앞에서 보았던 코드 **str1.equals(str2)**를 조금 더 깊게 해석하면,

'String변수 str1과 str2를 비교한다'보다는

'참조변수 str1이 저장하고있는 참조값에 해당하는 String객체와 str2가 저장하고있는 참조값에 해당하는 String객체를 비교한다'라고 생각할 수 있다.

이제 포스트의 초기에 언급했던 코드 **str1 == str2**는 String객체가 저장하고 있는 값을 비교하는것이 아닌, 참조변수 str1과 str2가 가진 **참조값을 비교**한다는 것을 알 수 있다.

좀 전에 보았던 아래의 코드를 다시 해석하자면,

{% highlight java %}
// Java
String str1 = new String("abc");
String str2 = new String("abc");

if(str1 == str2) // false
  System.out.println("Same"); // skip
{% endhighlight %}
비록 같은 값("abc")을 가지긴 했지만 new 연산자를 통해 String객체를 각각 별도로 생성했으므로 str1과 str2는 서로 다른 참조값을 가지게 되므로, 참조변수를 비교하는 코드 **str1 == str2**는 false를 반환하게 된다.



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


