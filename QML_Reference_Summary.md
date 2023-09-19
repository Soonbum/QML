# QML (Qt Modeling Language) 레퍼런스 요약

## QML 개요

* 개발 템플릿
  - Qt Quick: QML과 C++을 이용하여 프로그램 개발 템플릿
  - Qt Widget: QML을 배제한 프로그램 개발 템플릿

* 자료형은 크게 값(Value) 타입, 객체(Object) 타입으로 나눠져 있다. (C++ 언어의 Primitive 자료형과 class 자료형으로 비유할 수 있음)
  - 값 타입: int, bool, double, string, ...
  - 객체 타입: Rectangle, Gradient, Text, ...
  - 객체 타입에서 생성한 인스턴스를 객체(Object)라고 함

* 객체 안에 들어 있는 속성 값들을 애트리뷰트(Attribute)라고 한다.
  - 애트리뷰트 예시: width, height, color, ...
  - 그 외에도 애트리뷰트에는 id, 프로퍼티(property), 시그널(signal), 시그널 핸들러, 메서드(function), 열거형(enum) 등이 있다.
  - 프로퍼티(Property): C++의 typedef 키워드와 비슷함 (property string title, property color nextColor)
  - 프로퍼티에는 정적인 값과 동적인 계산식 모두 입력 가능함
  - 시그널(Signal): 객체에서 발생하는 이벤트 (signal이라는 키워드가 앞에 붙음, 이름이 동사 과거형 형태로 되어 있음) [능동적]
  - 시그널(Signal Handler): 특정 이벤트가 발생했을 때 트리거 되는 함수 (주로 on 접두사가 붙어 있음) [수동적]
  - 객체 안에는 객체가 들어 있을 수 있다. (중첩된 구조 가능)

* 컴포넌트(Component)는 .qml 파일로 만든 객체 타입을 의미한다.
  - 커스텀 객체 타입을 사용자가 만들 수 있음

* 기본 문법
  - CSS와 JavaScript 문법과 유사함

```qml
import QtQuick 2.0    // import를 통해 JavaScript 코드도 가져올 수 있음

Rectangle {
    width: 100     // 애트리뷰트와 정수 값
    height: 100    // 애트리뷰트와 정수 값

    /*
    애트리뷰트에 객체 타입이 할당됨
    객체 타입 안에는 또 다른 객체 타입이 들어 있고, 그 안에는 애트리뷰트들이 들어 있음
    */
    gradient: Gradient {
        GradientStop { position: 0.0; color: "yellow" }
        GradientStop { position: 1.0; color: "green" }
    }
}
```

* 자료형 종류
  - 값 타입: bool, int, real, double, list, string, enumeration, date, url, var, variant, void, ...
  - 객체 타입: color, font, point, size, rect, quaternion, vector2d, vector3d, vector4d, matrix4x4, ...

* 자료 링크
  - [All Qt C++ Classes](https://doc.qt.io/qt-6/classes.html)
  - [All QML Types](https://doc.qt.io/qt-6/qmltypes.html)
  - [All Qt Modules](https://doc.qt.io/qt-6/qtmodules.html)
  - [All Qt Reference Pages](https://doc.qt.io/qt-6/reference-overview.html)
  - [Examples and Tutorials](https://doc.qt.io/qt-6/qtexamplesandtutorials.html)
