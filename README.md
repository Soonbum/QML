# QML (Qt Modeling Language)

### 목차

* [QML의 예시](#qml의-예시)
* [QML 용어](#qml-용어)
* [QML 레퍼런스](#qml-레퍼런스)
  - [QML 구문 기초](#qml-구문-기초)
  - [QML Object Attributes](#qml-object-attributes)
  - [프로퍼티 바인딩](#프로퍼티-바인딩)
  - [시그널과 핸들러 이벤트 시스템](#시그널과-핸들러-이벤트-시스템)
  - [QML과 JavaScript 통합하기](#qml과-javascript-통합하기)
  - [QML 타입 시스템](#qml-타입-시스템)
  - [QML 모듈](#qml-모듈)
  - [QML 도큐먼트](#qml-도큐먼트)


### QML의 예시

```qml
import QtQuick
import QtQuick.Controls

ApplicationWindow {
    width: 400
    height: 400
    visible: true

    Button {
        id: button
        text: "A Special Button"
        background: Rectangle {
            implicitWidth: 100
            implicitHeight: 40
            color: button.down ? "#d6d6d6" : "#f6f6f6"
            border.color: "#26282a"
            border.width: 1
            radius: 4
        }
    }
}
```

### [QML 용어](https://doc.qt.io/qt-6/qml-glossary.html)

| 용어 | 정의 |
| --- | --- |
| QML | QML 앱에서 작성할 때 사용하는 언어입니다. 언어 구조 및 엔진은 Qt QML 모듈에 의해 구현됩니다. |
| Qt Quick | QML 언어에 대한 타입들에 대한 표준 라이브러리와 기능은 Qt Quick 모듈이 제공합니다. "import [QtQuick](https://doc.qt.io/qt-6/qtquick-module.html)"로 접근할 수 있습니다. |
| Type | QML에는 [Value Type](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html) 또는 [QML Object Type](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)이라는 타입이 있습니다. QML 언어는 많은 내장 [value 타입](https://doc.qt.io/qt-6/qtqml-typesystem-valuetypes.html)을 제공합니다. 그리고 Qt Quick 모듈은 QML 앱을 만들기 위한 다양한 [Qt Quick 타입](https://doc.qt.io/qt-6/qtquick-qmlmodule.html)을 제공합니다. 또, 서드파티 개발자들이 [모듈](https://doc.qt.io/qt-6/qtqml-modules-topic.html)을 통해, 혹은 앱 개발자가 [QML 문서](https://doc.qt.io/qt-6/qtqml-documents-definetypes.html)를 통해 앱에서 자체적으로 타입을 제공할 수도 있습니다. 자세한 것은 [QML Type System](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)을 보십시오. |
| Value Type | [Value Type](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)은 int, string, bool과 같은 단순한 타입입니다. [Object type](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)과 달리 Object는 Value Type으로부터 인스턴스를 만들 수 없습니다. 예를 들어, 프로퍼티, 메서드, 시그널 등을 이용하여 int Object를 만들 수 없습니다. Object type 뿐만 아니라 Value type도 [QML 모듈](https://doc.qt.io/qt-6/qtqml-modules-topic.html)에 속해 있습니다. 이것들을 사용하려면 모듈을 import해야 합니다. 일부 타입들은 언어에 내장되어 있습니다. 예를 들면 int, bool, double, string과 [QtObject](https://doc.qt.io/qt-6/qml-qtqml-qtobject.html)와 Component가 있습니다. 자세한 것은 [QML Type System](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)을 보십시오. |
| Object Type | [QML Object Type](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)은 QML 엔진에 의해 인스턴스화 할 수 있는 타입을 의미합니다. QML Type은 대문자로 시작하는 .qml 파일 내 도큐먼트, 혹은 [QObject](https://doc.qt.io/qt-6/qobject.html)-기반 C++ 클래스에 의해 정의될 수 있습니다. 자세한 것은 [QML Type System](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)을 보십시오. |
| Object | QML Object는 [QML Object Type](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)의 인스턴스입니다. 이 객체들은 [객체 선언](https://doc.qt.io/qt-6/qtqml-syntax-basics.html)을 처리할 때 엔진에 의해 생성됩니다. 객체 선언에는 생성될 객체를 정의하거나 객 객체에 대하여 정의되는 속성이 있습니다. 게다가 Object는 Component.createObject()와 Qt.createQmlObject() 함수를 통해 런타임 시에 동적으로 생성될 수 있습니다. [Lazy Instantiation](https://doc.qt.io/qt-6/qml-glossary.html#lazy-instantiation)도 보십시오. |
| Component | Component는 QML Object 또는 Object tree가 생성되는 템플릿입니다. QML 엔진에 의해 도큐먼트가 로드될 때 Component가 생성됩니다. 일단 한 번 로드되면 그것을 표현하는 Object 또는 Object tree를 인스턴스화 하는 데 사용될 수 있습니다. 게다가 [Component](https://doc.qt.io/qt-6/qml-qtqml-component.html) type은 도큐먼트 안에서 인라인으로 선언하는 데 사용될 수 있는 특수 타입입니다. Component object들은 동적으로 QML object들을 생성하는 Qt.createComponent()를 통해서도 동적으로 생성될 수 있습니다. |
| Document | [QML Document](https://doc.qt.io/qt-6/qtqml-documents-topic.html)는 1개 이상의 import 구문으로 시작하고 단일 최상위 레벨 object 선언을 포함하는 자체 포함 QML 소스 코드 조각입니다. Document는 .qml 파일 또는 텍스트 문자열 안에 있을 수 있습니다. 만약 도큐먼트가 대문자로 시작하는 이름을 가진 .qml 파일 안에 있다면, 엔진은 이 파일이 QML 타입의 정의라고 인식합니다. 최상위 레벨 Object 선언은 타입에 의해 인스턴스화될 Object tree를 캡슐화합니다. |
| Property | 이름을 가지고 있으며 연관된 값을 가진 Object type의 속성입니다. 이 값은 외부에서 읽혀질 수 있습니다. (그리고 대부분의 경우 쓰여질 수도 있습니다.) Object는 여러 개의 Property를 가질 수 있습니다. 여타 Property들은 해당 Type에 대한 데이터인 반면(예. [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 타입의 "text" Property), 몇 가지 Property들은 canvas와 연관되어 있습니다(예. x, y, width, height, opacity). 자세한 것은 [QML Object Attributes](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html)를 보십시오. |
| Binding | Binding은 Property와 "결합"되어 있는 JavaScript 표현식을 의미합니다. Property의 값은 해당 표현식을 평가하여 리턴되는 값입니다. 자세한 것은 [Property Binding](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html)을 보십시오. |
| Signal | Signal은 QML Object로부터의 알림을 의미합니다. 어떤 Object가 Signal을 방출(emit)하면, 다른 Object들은 이것을 듣고 [Signal Handler](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-attributes)를 통해 Signal을 처리할 수 있습니다. QML Object의 대부분의 Property들은 Change Signal을 가지고 있습니다. 그리고 기능을 구현하기 위해 클라이언트가 정의한 연관된 Change Signal Handler도 가지고 있습니다. 예를 들어 앱에서 정의된 MouseArea type 인스턴스의 "onClicked()" Handler는 사운드 재생을 발생시킬 수 있습니다. 자세한 것은 [Signal and Handler Event System](https://doc.qt.io/qt-6/qtqml-syntax-signals.html)을 보십시오. |
| Signal Handler | Signal Handler는 Signal에 의해 트리거되는 표현식(또는 함수)입니다. C++에서는 이것을 "slot"이라고도 합니다. 자세한 것은 [Signal and Handler Event System](https://doc.qt.io/qt-6/qtqml-syntax-signals.html)을 보십시오. |
| Lazy Instantiation | 필요할 때까지 불필요한 작업을 하지 않기 위해 Object Instance는 런타임 시 "늦게" 인스턴스화 될 수 있습니다. Qt Quick은 편하게 Lazy Instantiation을 구현하기 위해 [Loader](https://doc.qt.io/qt-6/qml-qtquick-loader.html) Type을 제공합니다. |

### [QML 레퍼런스](https://doc.qt.io/qt-6/qmlreference.html)

QML은 고도의 동적 앱을 만들기 위한 멀티 패러다임 언어입니다. QML을 이용하면 UI 컴포넌트 같은 앱 빌딩 블럭을 선언할 수 있고 앱 동작을 정의할 수 있는 다양한 프로퍼티 셋이 있습니다. 앱 동작은 JavaScript를 통해서도 스크립팅이 가능합니다. 게다가 QML은 Qt를 많이 사용하므로 QML 앱에서 타입 및 기타 Qt 기능에 직접 접근할 수 있습니다.

이 레퍼런스 가이드는 QML 언어의 기능에 대해 설명합니다. 이 가이드에 있는 많은 QML 타입들은 [Qt QML](https://doc.qt.io/qt-6/qtqml-index.html) 또는 [Qt Quick](https://doc.qt.io/qt-6/qtquick-index.html) 모듈에 기원을 두고 있습니다.

#### QML 구문 기초

QML은 object가 attribute 및 다른 object의 변경사항과의 관계와 응답하는 방식으로 정의될 수 있게 해주는 다중 패러다임 언어입니다. attribute와 동작의 변경사항이 단계적으로 처리되는 일련의 명령문들을 통해 표현되는 순수한 명령형 코드와 달리 QML의 선언적 구문은 attribute와 동작 변경사항을 개별 object의 정의에 직접 통합합니다. 이러한 attribute 정의에는 복잡한 커스텀 앱 동작이 필요한 경우 명령형 코드가 포함될 수 있습니다.

QML 소스 코드는 일반적으로 QML 코드의 독립형 도큐먼트인 QML 도큐먼트를 통해 엔진에 의해 로드됩니다. 이를 사용하여 앱 전체에서 재사용할 수 있는 QML object type을 정의할 수 있습니다. QML 파일에서 QML object type으로 선언하려면 타입 이름은 반드시 대문자로 시작해야 합니다.

* import 구문

QML 도큐먼트는 파일의 최상단에 1개 이상의 import 구문을 갖고 있습니다. import는 다음 중 하나가 될 수 있습니다:

- 타입이 등록된 버전이 부여된 네임스페이스 (예. 플러그인에 의해)
- QML 도큐먼트 형태의 타입-정의를 포함하는 상대 경로 디렉토리
- JavaScript 파일

import할 때 JavaScript 파일 가져오기는 반드시 적합해야 합니다. 그래야 제공되는 프로퍼티와 메서드에 접근할 수 있습니다.

다양한 import의 일반적인 형태는 다음과 같습니다:

- import Namespace VersionMajor.VersionMinor
- import Namespace VersionMajor.VersionMinor as SingletonTypeIdentifier
- import "directory"
- import "file.js" as ScriptIdentifier

예제:

- import QtQuick 2.0
- import QtQuick.LocalStorage 2.0 as Database
- import "../privateComponents"
- import "somefile.js" as Script

QML 가져오기에 대한 자세한 정보를 보려면 QML 구문 - Import 구문 문서를 보십시오.

* Object 선언

구문적으로 QML 코드 블럭은 생성될 QML object 트리를 정의합니다. object는 object에 주어진 attribute뿐만 아니라 생성될 object의 타입을 설명하는 object 선언을 사용하여 정의됩니다. 각 object는 nested object 선언을 사용하여 child object를 선언할 수도 있습니다.

object 선언은 해당 object 타입의 이름과 {} 세트로 구성되어 있습니다. 모든 attribute와 child object는 이 {} 안에서 정의됩니다.

다음은 간단한 object 선언입니다:

```qml
Rectangle {
    width: 100
    height: 100
    color: "red"
}
```

이 선언은 타입 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)의 object입니다. 바로 뒤에는 object에 대해 정의된 attribute를 감싸는 {}가 나옵니다. [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 타입은 QtQuick 모듈을 통해 만들 수 있으며 이 경우 정의된 attribute는 직사각형의 너비(width), 높이(height), 컬러(color) property의 값입니다.

위의 object는 [QML document](https://doc.qt.io/qt-6/qtqml-documents-topic.html)의 일부인 경우 엔진에 의해 로드될 수 있습니다. 즉, ([Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 타입을 이용할 수 있도록) 소스 코드에 QtQuick 모듈을 가져오는 import 구문을 추가하면 아래와 같습니다:

```qml
import QtQuick 2.0

Rectangle {
    width: 100
    height: 100
    color: "red"
}
```

어떤 .qml 파일에 배치되고 QML 엔진에 의해 로드되면, 위의 코드는 QtQuick 모듈에 의해 지원되는 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) type을 이용하여 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) object를 생성합니다.

만약 object 정의가 적은 수의 property를 갖고 있다면 다음과 같이 property들을 세미콜론으로 분리하여 한 줄로 작성할 수 있습니다.

```qml
Rectangle { width: 100; height: 100; color: "red" }
```

분명히 예제에서 선언된 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) object는 실제로 매우 간단합니다. 이는 적은 수의 property 값들만 정의하기 때문입니다. 좀 더 유용한 object들을 생성하려면 object 선언이 여러 가지 attribute 타입들을 정의해야 합니다: 이것은 [QML Object Attributes](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html) 문서에서 논의할 것입니다. 게다가 object 선언은 아래에서 논의한 대로 child object들을 정의할 수도 있습니다.

* Child Objects

어떤 object 선언이라도 nested object 선언을 통해 child object들을 정의할 수 있습니다. 이런 식으로 **object 선언은 여러 개의 child object를 포함하는 object 트리를 선언할 수 있습니다**.

예를 들어, 아래의 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) object 선언은 하나의 [Gradient](https://doc.qt.io/qt-6/qml-qtquick-gradient.html) object 선언을 포함하고 있으며 또 이것은 2개의 [GradientStop](https://doc.qt.io/qt-6/qml-qtquick-gradientstop.html) 선언을 포함하고 있습니다:

```qml
import QtQuick 2.0

Rectangle {
    width: 100
    height: 100

    gradient: Gradient {
        GradientStop { position: 0.0; color: "yellow" }
        GradientStop { position: 1.0; color: "green" }
    }
}
```

엔진이 이 코드를 로드하면, 이것은 루트가 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) object인 object 트리를 생성합니다; 이 object는 [Gradient](https://doc.qt.io/qt-6/qml-qtquick-gradient.html) child object를 가지고 있습니다. 또 이것은 2개의 [GradientStop](https://doc.qt.io/qt-6/qml-qtquick-gradientstop.html) child를 갖고 있습니다.

Note, however, that this is a parent-child relationship in the context of the QML object tree, not in the context of the visual scene. The concept of a parent-child relationship in a visual scene is provided by the [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) type from the QtQuick module, which is the base type for most QML types, as most QML objects are intended to be visually rendered. For example, [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) and [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) are both [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html)-based types, and below, a [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object has been declared as a visual child of a [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) object:

```qml
import QtQuick 2.0

Rectangle {
    width: 200
    height: 200
    color: "red"

    Text {
        anchors.centerIn: parent
        text: "Hello, QML!"
    }
}
```

When the [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object refers to its [parent](https://doc.qt.io/qt-6/qml-qtquick-item.html#parent-prop) value in the above code, it is referring to its visual parent, not the parent in the object tree. In this case, they are one and the same: the [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) object is the parent of the [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object in both the context of the QML object tree as well as the context of the visual scene. However, while the [parent](https://doc.qt.io/qt-6/qml-qtquick-item.html#parent-prop) property can be modified to change the visual parent, the parent of an object in the context of the object tree cannot be changed from QML.

(Additionally, notice that the [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object has been declared without assigning it to a property of the [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html), unlike the earlier example which assigned a [Gradient](https://doc.qt.io/qt-6/qml-qtquick-gradient.html) object to the rectangle's gradient property. This is because the [children](https://doc.qt.io/qt-6/qml-qtquick-item.html#children-prop) property of [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html#children-prop) has been set as the type's [default property](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#default-properties) to enable this more convenient syntax.)

See the [visual parent](https://doc.qt.io/qt-6/qtquick-visualcanvas-visualparent.html) documentation for more information on the concept of visual parenting with the [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) type.

* 주석




#### QML Object Attributes

#### 프로퍼티 바인딩

#### 시그널과 핸들러 이벤트 시스템

#### QML과 JavaScript 통합하기

#### QML 타입 시스템

#### QML 모듈

#### QML 도큐먼트
