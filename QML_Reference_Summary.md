QML (Qt Modeling Language) 레퍼런스 요약

### QML 개요

* 개발 템플릿
  - Qt Quick: QML과 C++을 이용하여 프로그램 개발 템플릿
  - Qt Widget: QML을 배제한 프로그램 개발 템플릿

* 자료형은 크게 값(Value) 타입, 객체(Object) 타입으로 나눠져 있다. (C++ 언어의 Primitive 자료형과 class 자료형으로 비유할 수 있음)
  - 값 타입: int, bool, double, string...
  - 객체 타입에서 생성한 인스턴스를 객체(Object)라고 함
  - 객체 안에 들어 있는 속성 값들을 애트리뷰트(Attribute)라고 한다.

* 애트리뷰트에는 id, 프로퍼티, 시그널, 시그널 핸들러, 메서드 등이 있다.
  - 프로퍼티(Property): 객체 안에 들어 있는 타입(값 타입, 객체 타입)들을 의미한다.
  - 시그널(Signal): 
  - 시그널(Signal Handler): 

* 컴포넌트(Component)는 .qml 파일로 만든 객체 타입을 의미한다.
  - 컴포넌트는 중첩된 구조를 만들 수 있다.




. 루트
+-- 가지1
| +--
| | +--
| +--
+--
