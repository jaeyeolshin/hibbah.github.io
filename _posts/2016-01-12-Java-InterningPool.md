---
layout: post
title: Java - Interning Pool
comments: true
---

----

## 1. 서론

C++문법에 익숙한 사람이 Java의 세계에 입문하면서 부딪히게 되는 몇 가지 오류들에 대해 다룬다.

이 포스트에서는 Java의 String객체를 다루는데 있어서 평소 C++에서 사용했던 비교연산(**==**)을 그대로 사용함으로써 마주하게 되는 혼란과, 더 나아가 **Interning Pool**, **Immutable Object**에 대해 중점적으로 이야기한다.

----

## 2. 의문의 시작

```cpp
// C++
string str1 = "abc";
string str2 = "abc";

if(str1 == str2) // true
  cout<<"Same"<<endl; // print "Same"
```
위의 C++코드는 의도한대로 "Same"을 출력한다.



```java
// Java
String str1 = "abc";
String str2 = "abc";

if(str1 == str2) // true
  System.out.println("Same"); // print "Same"
```
Java또한 마찬가지로 **의도**한대로 "Same"을 출력한다.

(사실 "Same"이 출력되는것이 **의도**와는 전혀 다른 이유지만, 결과적으로 "Same"이 출력되어 버리기 때문에 아래 코드의 결과에서 혼란을 마주하게 된다.)

'아! Java에서는 객체 생성을 new연산자를 통해 할당하지!'라는 생각으로 객체 생성법만 달리할 뿐, 위와 동일한 코드를 작성한다.



```java
// Java
String str1 = new String("abc");
String str2 = new String("abc");

if(str1 == str2) // false
  System.out.println("Same"); // skip
```
분명 "Same"이 출력될 것을 기대하였으나 콘솔은 아무것도 출력하지 않는다. Java가 미워지기 시작한다...


----

## 3. ==연산자, 객체, 참조변수에 대한 이해

결론부터 얘기하면 Java에서 String객체의 비교는 .equals() 메서드를 이용해야한다. 직전의 코드를 '두 개의 String객체 값을 비교'하는 의도대로 작성한 올바른 코드는 아래와 같다.
 
```java
// Java
if(str1.equals(str2)) // true
  System.out.println("Same"); // print "Same"
```

C++에서는 string 객체에 대해 비교연산자(==)를 사용하면 string class 내부적으로 정의된 operator overloading의 동작을 수행하게 된다.
  
```cpp
// C++
class string{
  ...
  
  bool operator == (const String & str){
    // 길이가 다르면 return false
    // 길이가 같으면 character단위로 문자열 비교
  }
};
```

그럼, Java에서 비교연산자(==)는 어떻게 동작하는 것일까?

분명히 오류없이 실행은 되었기에 문법상의 문제가 아니라 '두 객체의 값을 비교'한다는 **의도**와는 다른 동작을 수행했음을 짐작할 수 있다.

연산자의 동작에 대해 알아보기전에 우선적으로 다들 잘 알고있는 **객체**와 **참조변수**에 관한 아래의 Java 기본 내용을 되짚어본다.

```java
Object obj = new Object();
```

할당연산자(=)를 기준으로 우측의 new Object()는 실제로 Object타입의 **객체**를 힙(heap)메모리에 생성한 뒤 해당 객체의 참조값을 반환하고, 좌측의 Object obj는 방금 생성한 객체를 코드상에서 참조하여 다루기위해 객체의 참조값(주소값?)을 저장하는 **참조변수**이다.

따라서, 앞에서 보았던 코드 **str1.equals(str2)**를 조금 더 길게 해석하면,

'String변수 str1과 str2를 비교한다'보다는

'참조변수 str1이 저장하고있는 참조값에 해당하는 String객체와 str2가 저장하고있는 참조값에 해당하는 String객체를 비교한다'라고 생각할 수 있다.

이제 포스트의 초기에 언급했던 코드 **str1 == str2**는 String객체가 저장하고 있는 값을 비교하는것이 아닌, 참조변수 str1과 str2가 가진 **참조값을 비교**한다는 것을 알 수 있다.

좀 전에 보았던 아래의 코드를 다시 해석하자면,

```java
// Java
String str1 = new String("abc");
String str2 = new String("abc");

if(str1 == str2) // false
  System.out.println("Same"); // skip
```

비록 같은 값("abc")을 가지긴 했지만 new 연산자를 통해 String객체를 각각 별도로 생성했으므로 str1과 str2는 서로 다른 참조값을 가지게 되므로, 참조변수를 비교하는 코드 **str1 == str2**는 false를 반환하게 된다. 위의 코드를 메모리 구조 관점에서 간단하게 살펴본 그림은 아래와 같다.

![memory1]({{ site.url }}/img/java_String1.JPG)

String(string)타입에 대한 ==연산자를 C++에서는 **operator ==**로 해석하는 반면, Java에서는 참조변수가 가진 참조값 자체에 대한 비교로 해석한다는 것을 알아두자.

----

## 4. Interning Pool

이제 뭔가 알 것도 같은데 도입부에서 언급했던 아래의 코드는 여전히 혼란으로 남아있다.

```java
// Java
String str1 = "abc";
String str2 = "abc";

if(str1 == str2) // true
  System.out.println("Same"); // print "Same"
```
String객체는 독특하게 new연산자 뿐만 아니라 할당연산자를 통해서도 객체 생성이 가능한데, 왜 new연산자를 통한 객체 생성에서는 비교연산을 false로 해석하고 리터럴값의 할당을 통해 객체를 생성한 위의 코드는 true로 해석하는 것일까?

결론부터 얘기하면, 리터럴값의 할당을 통한 객체의 생성은 new 연산자를 사용하는것과 객체 생성 과정이 조금 다르다.

리터럴값의 할당을 통한 객체 생성은 **Interning Pool**이라는 메모리 공간을 참조하게 되는데, 먼저 Interning이 왜 등장했는지를 이해하기 위해 아래의 코드를 살펴본다.

```java
// Java
String[] str = new String[X];

for(int i=0; i<X; ++i)
  str[i] = "abc";
```

X개의 String객체를 생성하는데 모두 동일한 값("abc")를 갖도록 한다. 여기서 만약 X가 굉장히 크다면 동일한 값을 가지는 객체 X개를 모두 별도로 생성하게 되므로 메모리 낭비가 커진다.

위와 같은 메모리 낭비를 줄이고자 등장한 기법(?)이 Interning이며, 리터럴값의 할당을 통해 객체를 생성할때는 무작정 객체를 생성하는 것이 아니라 **Interning Pool**이라는 메모리 영역을 참조하여 동일한 값의 객체가 이미 메모리에 존재하는지 확인하는 과정을 거친다.

먼저, 첫 번째 객체를 생성하는 **String str[0] = "abc"**에서 Interning Pool을 참조하여 "abc"라는 값을 가진 객체가 존재하지 않음을 확인하고 힙메모리에 "abc"값을 갖는 String객체를 생성한다. 이후의 모든 라인은 Interning Pool을 참조하여 "abc"라는 값의 String객체가 이미 메모리 영역에 존재함을 알고, 해당 객체의 참조값을 반환하게된다. 즉, str[0]... str[X-1]까지 X개의 참조변수는 모두 동일한 하나의 객체를 참조하게 된다.

![memory2]({{ site.url }}/img/java_String2.JPG)

new연산자를 이용한 방식은 앞에서도 언급했듯이 new연산자를 통해 각각 별도의 객체를 생성함을 **명시적**으로 나타낸 것이므로 Interning Pool을 참조하지 않고 모두 별개의 객체로 생성한다.

따라서, 리터럴값 할당으로 같은 값의 String객체를 생성한 경우에는 str1 == str2가 true를 반환하는 이유를 이해할 수 있다.



----

## 5. Immutable Object (작성중)

Interning과 관련하여 서로다른 참조변수가 같은 객체를 참조하고 있을 때, 해당 객체의 데이터를 변경하려는 의도로 아래와 같은 코드 작성.

C++로 생각하면, 동일 객체를 참조하고 있었으므로 str1과 str2가 가리키는 객체는 동일 객체 "abcdef"라고 생각.

```java
String str1 = "abc";
String str2 = "abc";

str1 += "def";

if(str1 == str2) // false
  System.out.println("Same");
```

str1 += "~~", String::concat() 는 C++과 달리, 기존에 존재하던 객체는 그대로 두고 전혀 다른 새로운 String객체를 생성하여 반환.

str1 += "~~"과 같이 새로 생성된 객체를 참조변수 str1에 할당하면, 기존에 str1이 참조하고 있던 String객체는 garbege collector에 의해 제거.




## Q

1. Interning Pool에서 있나/없나를 확인하는 성능 ? (hash 구조?)
2. Interning Pool은 어떤 메모리 영역에? (heap추측)
3. String객체 외에 Interning이 적용되는 객체는 ? (immutable 속성을 갖는 모든 객체?) 
