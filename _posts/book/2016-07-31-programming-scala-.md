---
layout: post
title:  "Programming Scala"
date:   2016-07-31
type: book
category: reading book
tag: programming book
---

# Programming Scala
![Programming Scala]({{ site.url }}/assets/img/postImg/programmingScala.jpg)

## Chapter1

### 정리

1. scala 설치

2. About Scala

* 정적타입지정
	* 타입추론
* 객체지향 프로그래밍
	* 혼합합성을 사용하여 타입을 깔끔하게 구현하는 트레이트로 자바 객체 모델을 보완한다.
	* 스칼라에서는 모든 것이 객체.
* 함수형 프로그래밍
	* 불변값(immutable value),1급함수(first class), 부수효과가 없는 함수, 고차함수, 함수 컬렉션.
* 규모 확장성
	* 트레이트를 사용한 혼합합성
	* 추상타입맴버(abstract type member)
	* 제레릭스
	* 내포 클래스
	* 명시적 자기타입 지정
* 패턴매칭
* for 내장
* val : 불변값 지정
* 타입정보를 나타내기 위해서는 대상 이름 위에 콜론(:)을 붙인 다음 타입표기를 추가한다.

```
	// src/main/scala/progscala2/introscala/upper1.sc

	class Upper {
		def upper(strings: String*): Seq[String] = {
			strings.map((s:String) => s.toUpperCase())
		}
	}

	val up = new Upper
	println(up.upper("Hello", "World!"))
```

* class
	* 스칼라에서 클래스는 class 키워드로 시작한다.
	* 클래스의 본문이 그 클래스의 주 생성자다.
	* 이 생성자가 매개변수를 받아야 한다면 클래스 이름 뒤에 매개변수 목록을 추가할 수 있다.
* 메서드
	* 정의는 def 키워드로 시작하며 그 다음에 이름이 온다.
	* 이름 다음에는 선택적으로 매개변수목록이 온다.
	* 그 다음 선택적으로 반환 타입이 올 수 있다(대부분 컴파일러가 추론함). 반환 타입은 콜론뒤에 타입을 붙여서 표시한다.
	* 등호(=)가 매서드 시그니처와 본문 사이에 온다.
	* 본문이 단일 식으로 이뤄진 경우 중괄호 생략가능.
* Seq
	* 일정한 순서로 반복 가능한 추상적 컬렉션.

```
	// src/main/scala/progscala2/introscala/upper2.sc

    object Upper {	//싱글턴 객체로 선언.
      def upper(strings: String*) = strings.map(_.toUpperCase())
    }

    println(Upper.upper("Hello", "World!"))

```

* 자바의 static과 같이 클래스 수준의 맴버를 활용해야 할 경우 스칼라에서는 object를 사용한다.
	* 객체의 매서드를 직접 호출할 수 있다.
* 스칼라의 타입추론 알고리즘은 지역타입추론.
* main
	* 스칼라의 main은 object에 정의한 메서드이어야 한다.
* 패키지와 실제 소스파일의 위치를 다르게할 수 있다.

```
	// src/main/scala/progscala2/introscala/shapes/shapes.scala
    package progscala2.introscala.shapes

    case class Point(x: Double = 0.0, y: Double = 0.0)                   // <1>

    abstract class Shape() {                                             // <2>
      /**
       * Draw takes a function argument. Each shape will pass a stringized
       * version of itself to this function, which does the "drawing".
       */
      def draw(f: String => Unit): Unit = f(s"draw: ${this.toString}")   // <3>
    }

    case class Circle(center: Point, radius: Double) extends Shape       // <4>

    case class Rectangle(lowerLeft: Point, height: Double, width: Double) // <5>
          extends Shape

    case class Triangle(point1: Point, point2: Point, point3: Point)     // <6>
          extends Shape

```

* case
	* 클래스 선언앞에 넣으면 각각의 생성자 매개변수가 자동으로 클래스 인스턴스의 읽기 전용 필드로 바뀐다.
	* 인자에 기본값을 지정할 수 있다.
	* 컴파일러가 자동으로 몇 가지 메서드를 만들어준다.(toString, equals, hashCode)
	* 컴파일러가 모든 케이스 클래스에 대해 각 클래스와 이름이 같은 싱글턴 객체인 동반객체(companion object) 를 자동으로 만들어준다.
* 동반객체
	* 직접생성 : object와 class의 이름이 같고, 그 둘이 모두 같은 파일 안에 정의된 경우, 이 둘은 각각 상대방의 동반객체, 동반 클래스이다.
	* 메서드를 추가할 수 있다.
	* 자동으로 추가되는 메서드 apply : 동반 클래스의 생성자와 같은 인자를 받는다.
* 고차함수(high-order-function)
	* 어떤 함수가 다른 함수를 값으로 변환하거나 인자로 받는 경우.

```
    // src/main/scala/progscala2/introscala/shapes/ShapesDrawingActor.scala
    package progscala2.introscala.shapes

    object Messages {                                                    // <1>
      object Exit                                                        // <2>
      object Finished
      case class Response(message: String)                               // <3>
    }

    import akka.actor.Actor                                              // <4>

    class ShapesDrawingActor extends Actor {                             // <5>
      import Messages._                                                  // <6>

      def receive = {                                                    // <7>
        case s: Shape =>
          s.draw(str => println(s"ShapesDrawingActor: $str"))
          sender ! Response(s"ShapesDrawingActor: $s drawn")
        case Exit =>
          println(s"ShapesDrawingActor: exiting...")
          sender ! Finished
        case unexpected =>  // default. Equivalent to "unexpected: Any"
          val response = Response(s"ERROR: Unknown message: $unexpected")
          println(s"ShapesDrawingActor: $response")
          sender ! response
      }
    }

```

* import
	* 블록안에 내포시킬 수 있다. 임포트한 값의 영역을 그 값을 사용할 장소로 한정시킬 수 있다.
* Any
	* 스칼라 타입 계층의 최상위 클래스.
* !
	* 비동기(asynchronous)메시지를 보내는 메서드.



### 의문사항

* 정적타입지정(static typing)
* 타입추론(type inference)
* 혼합합성(mixin composition)
* 트레이트(trait)
* 제네릭스(generics)
* 추상타입멤버(abstract type member)
* 내포 클래스
* 명시적인 자기 타입
* 패턴매칭
* for 내장
* 리터럴(literal)
* 함수 리터널(function literal) : 익명함수
* 위치지정자(placeholder) _
* 동반객체(companion object)
* Unit
* 고차함수(high-order-function)
* 문자열 인터폴레이션(interpolation)
* 중위 표기법(infix notation)
