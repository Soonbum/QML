# QML (Qt Modeling Language)

### 목차

* [QML의 예시](#qml의-예시)
* [QML 용어](#qml-용어)
* [QML 레퍼런스](#qml-레퍼런스)
  - [QML 구문 기초](#qml-구문-기초)
    * [import 구문](#import-구문)
    * [Object 선언](#object-선언)
    * [Child Objects](#child-objects)
    * [코멘트](#코멘트)
  - [QML Object Attributes](#qml-object-attributes)
    * [Object 선언 내에서의 Attribute](#object-선언-내에서의-attribute)
    * [id attribute](#id-attribute)
    * [Property Attributes](#property-attributes)
    * [Signal Attributes](#signal-attributes)
    * [Method Attributes](#method-attributes)
    * [부착된 Properties와 부착된 Signal Handlers](#부착된-properties와-부착된-signal-handlers)
    * [Enumeration Attributes](#enumeration-attributes)
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

##### import 구문

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

##### Object 선언

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

##### Child Objects

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

그러나 이것은 시각적 장면의 문맥 QML object 트리의 문맥 상에서의 parent-child 관계라는 점을 유의하십시오. 대부분의 QML object들이 시각적으로 렌더링되기 때문에 시각적 장면에서의 parent-child 관계의 개념은 대부분의 QML type에 대한 base type인 QtQuick 모듈의 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) type에 의해 제공됩니다. 예를 들어, [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)과 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html)는 모두 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html)-기반 type이며, 아래의 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object는 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) object의 시각적 child로 선언되었습니다:

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

[Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object가 위의 코드에서 [parent](https://doc.qt.io/qt-6/qml-qtquick-item.html#parent-prop) 값을 참조할 때, object 트리의 parent가 아닌 시각적 parent를 참조합니다. 이 경우 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) object는 QML object 트리에서나 시각적 장면의 문맥에서나 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object의 parent입니다. 그러나 시각적 parent를 변경하기 위해 [parent](https://doc.qt.io/qt-6/qml-qtquick-item.html#parent-prop) property를 수정할 수는 있어도 object 트리의 문맥 상에서 QML로부터 object의 parent를 변경할 수는 없습니다.

(게다가 직사각형의 gradient property에 [Gradient](https://doc.qt.io/qt-6/qml-qtquick-gradient.html) object를 할당한 앞의 예제와 달리 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)의 property에 할당하지 않고 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object가 선언되었음을 주목하십시오. 이것은 더 편리한 구문을 사용하기 위해 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html#children-prop)의 [children](https://doc.qt.io/qt-6/qml-qtquick-item.html#children-prop) property가 타입의 [default property](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#default-properties)로 설정되었기 때문입니다.)

[Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) type과 함께 시각적 parenting의 개념에 대한 더 많은 정보를 보려면 [visual parent](https://doc.qt.io/qt-6/qtquick-visualcanvas-visualparent.html) 문서를 보십시오.

##### 코멘트

QML에서 코멘트 달기 구문은 JavaScript의 그것과 비슷합니다:

- 단일 라인 코멘트는 //로 시작하고 라인의 끝에서 종료합니다.
- 다중 라인 코멘트는 /*로 시작하고 */로 끝납니다.

```qml
Text {
    text: "Hello world!"    //a basic greeting
    /*
        We want this text to stand out from the rest so
        we give it a large size and different font.
     */
    font.family: "Helvetica"
    font.pointSize: 24
}
```

QML 코드를 처리할 때 엔진은 코멘트를 무시합니다. 이것은 나중에 참조하거나 다른 사람들에게 구현한 부분을 설명할 때 코드 섹션이 수행하는 작업을 설명하는 데 유용합니다.

코멘트는 때때로 문제를 추적하기 위해 코드 실행을 막는 데에도 유용합니다.

```qml
Text {
    text: "Hello world!"
    //opacity: 0.5
}
```

위의 예제에서 Text object는 일반적인 불투명도를 갖게 될 것입니다. 왜냐하면 opacity: 0.5가 코멘트로 변했기 때문입니다.

#### QML Object Attributes

모든 QML object type은 정의된 attribute 집합을 갖고 있습니다. object type의 각 인스턴스는 해당 object type에 대해 정의된 attribute 집합을 가지고 생성됩니다. 아래에 나와 있듯이 지정할 수 있는 여러 종류의 attribute들이 있습니다.

##### Object 선언 내에서의 Attribute

QML 도큐먼트에서 [object 선언](https://doc.qt.io/qt-6/qtqml-syntax-basics.html#object-declarations)은 새로운 type을 정의합니다. 또한 인스턴스화될 object 계층을 선언합니다. 새로 정의된 type의 인스턴스가 생성되어야 합니다.

QML object-type attribute type의 집합은 다음과 같습니다:

* id attribute
* property attribute
* signal attribute
* signal handler attribute
* method attribute
* 부착된 Properties와 부착된 Signal Handlers
* Enumeration Attributes

이 attribute들은 아래에서 자세히 논의할 것입니다.

##### id attribute

모든 QML object type은 단 1개의 id attribute를 갖고 있습니다. 이 attribute는 언어 자체가 제공하며 다른 QML object type에 의해 재정의되거나 오버라이드될 수 없습니다.

object를 식별하고 다른 object가 참조할 수 있도록 object 인스턴스의 id attribute에 어떤 값을 할당할 수 있습니다. 이 id는 반드시 소문자나 밑줄 문자로 시작해야 하며 문자, 숫자, 밑줄 문자 외에 다른 문자는 포함할 수 없습니다.

아래는 [TextInput](https://doc.qt.io/qt-6/qml-qtquick-textinput.html) object와 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object입니다. [TextInput](https://doc.qt.io/qt-6/qml-qtquick-textinput.html) object의 id 값은 "myTextInput"입니다. [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object는 myTextInput.text를 참조함으로써 [TextInput](https://doc.qt.io/qt-6/qml-qtquick-textinput.html)의 text property와 같은 값으로 자신의 text property를 설정합니다:

```qml
import QtQuick 2.0

Column {
    width: 200; height: 200

    TextInput { id: myTextInput; text: "Hello World" }

    Text { text: myTextInput.text }
}
```

object는 선언된 component 범위 안이라면 어디서든지 id로 참조할 수 있습니다. 그러므로 id 값은 component 범위 안에서 항상 유일해야 합니다. 더 자세한 것은 [Scope and Naming Resolution](https://doc.qt.io/qt-6/qtqml-documents-scope.html)을 보십시오.

일단 object 인스턴스가 생성되면, id attribute의 값은 바꿀 수 없습니다. 이것은 일반 property처럼 보일 수 있지만 id attribute는 일반 property attribute가 아니며 특별한 의미가 적용됩니다. 예를 들어 위의 예제에서는 myTextInput.id에 접근할 수 없습니다.

##### Property Attributes

property는 정적인 값을 할당하거나 동적 표현식에 결합할 수 있는 object의 attribute입니다. property의 값은 다른 object가 읽을 수 있습니다. 일반적으로 특정 QML type이 특정 property에 대해 이것을 명시적으로 허용하지 않는 한 다른 object에 의해 수정될 수 있습니다.

* Property Attributes 정의하기

C++에서 클래스에 Q_PROPERTY를 등록하여 QML type 시스템에 등록함으로써 property는 type으로 정의될 수 있습니다. 또는 다음 구문을 사용하여 QML 도큐먼트에서 object type의 커스텀 property를 object 선언에서 정의할 수도 있습니다.

```qml
[default] [required] [readonly] property <propertyType> <propertyName>
```

이런 식으로 object 선언은 object 외부로 [특정 값을 노출](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html#defining-object-types-from-qml)하거나 좀 더 쉽게 몇 가지 내부 상태를 유지할 수 있습니다.

property 이름은 반드시 소문자로 시작해야 하며 글자, 숫자, 밑줄문자로만 이루어질 수 있습니다. [JavaScript 예약어](https://developer.mozilla.org/en/JavaScript/Reference/Reserved_Words)는 유효한 property 이름이 아닙니다. default, required, readonly 키워드는 선택적이며 선언될 property의 의미를 수정합니다. [default properties](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#default-properties), [required properties](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#required-properties), [read-only properties](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#read-only-properties)에 대한 각각의 의미에 대한 자세한 정보는 다음 섹션을 보십시오.

커스텀 property를 선언하는 것은 암묵적으로 해당 property에 대하여 value-change [signal](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-attributes)과 이름이 on<PropertyName>Changed인 [signal handler](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-handler-attributes)를 생성합니다. 여기서 <PropertyName>은 property의 이름이며 1번째 글자는 대문자입니다.

예를 들어, 다음 object 선언은 Rectangle base type으로부터 파생된 새로운 type을 정의합니다. 이것은 2개의 새로운 property를 갖고 있습니다. 그 중 하나에 [signal handler](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-handler-attributes)가 구현되어 있습니다:

```qml
Rectangle {
    property color previousColor
    property color nextColor
    onNextColorChanged: console.log("The next color will be: " + nextColor.toString())
}
```

* 커스텀 property 정의 내에서 유효한 type

[enumeration](https://doc.qt.io/qt-6/qml-enumeration.html) type을 제외한 모든 [QML Value Types](https://doc.qt.io/qt-6/qtqml-typesystem-valuetypes.html)은 커스텀 property type으로 사용할 수 있습니다. 예를 들어, 다음은 모두 유효한 property 선언입니다:

```qml
Item {
    property int someNumber
    property string someString
    property url someUrl
}
```

(Enumeration 값은 단순히 정수 값이며 int type을 대신 참조할 수 있습니다.)

일부 value type은 QtQuick 모듈에서 제공하므로 모듈을 가져오지 않으면 property type으로 사용할 수 없습니다. 자세한 것은 [QML Value Types] 문서를 보십시오.

[var](https://doc.qt.io/qt-6/qml-var.html) value type은 모든 타입의 value, list, object를 보관할 수 있는 generic placeholder type이라는 것을 유의하십시오:

```qml
property var someNumber: 1.5
property var someString: "abc"
property var someBool: true
property var someList: [1, 2, "three", "four"]
property var someObject: Rectangle { width: 100; height: 100; color: "red" }
```

게다가 모든 [QML object type](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html)은 property type으로 사용할 수 있습니다. 예를 들면:

```qml
property Item someItem
property Rectangle someRectangle
```

또한 이것은 [커스텀 QML types](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html#defining-object-types-from-qml)에 적용됩니다. 만약 QML type이 파일 ColorfulButton.qml(클라이언트가 가져온 디렉토리 안)에 정의되어 있다면, type ColorfulButton의 property 역시 유효합니다.

* Property Attribute에 값 할당하기

object 인스턴스의 property의 값은 2가지 방식으로 지정할 수 있습니다:

- 초기화 시에 값 할당
- 명령형 값 할당

두 경우 모두 값은 정적이거나 바인딩 표현식 값일 수 있습니다.

* 초기화 시에 값 할당

초기화 시에 property에 값을 할당하는 구문은 다음과 같습니다:

```qml
<propertyName> : <value>
```

초기값 할당은 만약 원한다면 object 선언에서 property 정의와 결합될 수 있습니다. 이 경우 property 정의 구문은 다음과 같습니다:

```qml
[default] property <propertyType> <propertyName> : <value>
```

property 값 초기화 예제는 다음과 같습니다:

```qml
import QtQuick 2.0

Rectangle {
    color: "red"
    property color nextColor: "blue" // 결합된 property 선언 및 초기화
}
```

* 명령형 값 할당

명령형 값 할당은 명령형 JavaScript 코드로부터 property 값(정적인 값 또는 바인딩 표현식)이 property에 할당되는 것입니다. 명령형 값 할당의 구문은 다음과 같이 JavaScript 할당 연산자를 사용합니다:

```qml
[<objectId>.]<propertyName> = value
```

명령형 값 할당의 예제는 다음과 같습니다:

```qml
import QtQuick 2.0

Rectangle {
    id: rect
    Component.onCompleted: {
        rect.color = "red"
    }
}
```

* 정적인 값 및 바인딩 표현식 값

위에서 봤듯이, property에 할당되는 값은 두 종류가 있습니다: 정적인 값, 그리고 바인딩 표현식 값. 후자의 경우를 [property binding](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html)이라고도 합니다.

| 종류 | 의미 |
| --- | --- |
| 정적인 값 | 다른 property에 의존하지 않는 상수 값입니다. |
| 바인딩 표현식 | 어떤 property와 다른 property들과의 관계를 설명하는 JavaScript 표현식입니다. 이 표현식에서 변수는 property의 의존성이라고도 합니다. QML 엔진은 어떤 property와 의존성 간의 관계를 만들어냅니다. 값에서 의존성 변화가 생기면 QML 엔진은 자동으로 바인딩 표현식을 다시 연산하고 property에 새로운 결과를 할당합니다. |

다음은 property에 할당되는 두 종류의 값을 보여주는 예제입니다:

```qml
import QtQuick 2.0

Rectangle {
    // 다음은 초기화 시에 정적인 값을 할당함
    width: 400
    height: 200

    Rectangle {
        // 다음은 초기화 시에 바인딩 표현식 값을 할당함
        width: parent.width / 2
        height: parent.height
    }
}
```

주의: 바인딩 표현식을 명령형으로 할당하려면 바인딩 표현식을 Qt.binding()에 전달된 함수 안에 포함시켜야 합니다. 그리고 나서 [Qt.binding](https://doc.qt.io/qt-6/qml-qtqml-qt.html#binding-method)()이 리턴한 값을 property에 할당해야 합니다. 반대로 초기화 시에 바인딩 표현식을 할당할 때에는 Qt.binding()을 사용하지 말아야 합니다. 더 많은 정보는 [Property Binding](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html)을 보십시오.

* Type Safety

property는 타입 safe입니다. property에는 property type이 일치하는 값만 할당할 수 있습니다.

예를 들어, 만약 property가 real 타입인데 string을 할당하려고 시도한다면 다음과 같은 오류가 발생할 것입니다:

```qml
property int volume: "four"  // 오류가 발생함; property의 object가 로드되지 않을 것입니다.
```

마찬가지로 런타임 중에 property에 잘못된 type의 값이 할당되면 새로운 값이 할당되지 않고 오류가 발생할 것입니다.

일부 property type들은 자연 값 표현식을 갖고 있지 않으며 이러한 property type에 대해서는 QML 엔진이 자동으로 string-to-typed-value 변환을 수행합니다. 예를 들면, color type의 property에 문자열이 아닌 컬러를 저장해도 오류가 보고되지 않으며 color property에 문자열 "red"를 할당할 수 있습니다.

기본적으로 지원되는 property의 type 목록은 [QML Value Types](https://doc.qt.io/qt-6/qtqml-typesystem-valuetypes.html)를 보십시오. 게다가 사용 가능한 [QML object type](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html)은 property type으로도 사용할 수 있습니다.

* 특수 Property Types

* Object List Property Attributes

[list](https://doc.qt.io/qt-6/qml-list.html) type property는 QML object-type 값들의 리스트를 할당할 수 있습니다. object list 값을 정의하는 구문은 []로 감싸고 ,로 분리된 list입니다:

```qml
[ <item 1>, <item 2>, ... ]
```

예를 들어, [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) type은 [states](https://doc.qt.io/qt-6/qml-qtquick-item.html#states-prop) property를 가지고 있는데 이것은 [State](https://doc.qt.io/qt-6/qml-qtquick-state.html) type object들의 리스트를 저장하는 데 사용합니다. 아래 코드는 3개의 [State](https://doc.qt.io/qt-6/qml-qtquick-state.html) object의 list에 이 property의 값을 초기화합니다:

```qml
import QtQuick 2.0

Item {
    states: [
        State { name: "loading" },
        State { name: "running" },
        State { name: "stopped" }
    ]
}
```

만약 list가 1개의 항목을 포함하고 있다면, []은 생략할 수 있습니다:

```qml
import QtQuick 2.0

Item {
    states: State { name: "running" }
}
```

object 선언할 때 [list](https://doc.qt.io/qt-6/qml-list.html) type property를 지정하는 구문은 다음과 같습니다:

```qml
[default] property list<<objectType>> propertyName
```

그리고 다른 property 선언과 마찬가지로 다음 구문처럼 property 초기화는 property 선언과 결합될 수 있습니다:

```qml
[default] property list<<objectType>> propertyName: <value>
```

list property 선언 예제는 다음과 같습니다:

```qml
import QtQuick 2.0

Rectangle {
    // 초기화 없는 선언
    property list<Rectangle> siblingRects

    // 초기화 + 선언
    property list<Rectangle> childRects: [
        Rectangle { color: "red" },
        Rectangle { color: "blue"}
    ]
}
```

만약 QML object-type 값이 아닌 값 list를 저장할 property를 선언하려면 [var](https://doc.qt.io/qt-6/qml-var.html) property를 대신 선언해야 합니다.

* 그룹화된 Properties

일부 경우에서는 property가 sub-property attribute의 논리적 그룹을 포함하고 있습니다. 이 sub-property attribute들은 도트 표기법 또는 그룹 표기법을 사용하여 할당할 수 있습니다.

예를 들어, [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) type이 [font](https://doc.qt.io/qt-6/qml-qtquick-text.html#font.family-prop) group property를 갖고 있다고 가정합니다. 아래에서 1번째는 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object가 도트 표기법으로 font 값을 초기화하고 있습니다. 2번째는 그룹 표기법을 사용하고 있습니다:

```qml
Text {
    // 도트 표기법
    font.pixelSize: 12
    font.b: true
}

Text {
    // 그룹 표기법
    font { pixelSize: 12; b: true }
}
```

그룹화된 property type은 subproperty를 갖고 있는 type입니다. 만약 그룹화된 property type이 (value type이 아닌) object type이라면, 이 property는 읽기-전용이어야 합니다. 이것은 subproperty가 속한 object를 바꾸지 못하게 합니다.

* Property Alias(별명)

Property Alias는 또 다른 property에 대한 참조를 갖고 있는 property입니다. property에 대한 새로운 고유의 저장 공간을 할당하는 일반적인 property 정의와 달리, property alias는 (aliasing property라고 하는) 새로 선언된 property를 기존 property(aliased property)에 직접 참조로 연결합니다.

property alias 선언은 property type과 달리 alias 키워드가 필요하고 property 선언의 오른쪽에 유효한 alias 참조가 있어야 한다는 점만 제외하면 일반적인 property 정의와 비슷합니다:

```qml
[default] property alias <name>: <alias reference>
```

일반적인 property와 달리 alias는 다음과 같은 제한사항을 가지고 있습니다:

- alias가 선언된 [type](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html)의 범위 내에 있는 object, 또는 object의 property만 참조할 수 있습니다.
- 임의의 JavaScript 표현식을 포함할 수 없습니다.
- 해당 type의 범위 밖에서 선언된 object를 참조할 수 없습니다.
- 일반적인 property의 경우 기본값이 선택적이지만 alias 참조는 선택적이지 않습니다; alias 참조는 alias가 처음 선언될 때 제공되어야 합니다.
- [부착된 property](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#attached-properties-and-attached-signal-handlers)를 참조할 수 없습니다.
- 깊이가 3 이상인 계층 내부의 property를 참조할 수 없습니다. 다음 코드는 작동하지 않습니다:

```qml
property alias color: myItem.myRect.border.color

Item {
    id: myItem
    property Rectangle myRect
}
```

그러나 최대 2레벨 깊이까지의 property에 대한 alias는 작동합니다.

```qml
property alias color: rectangle.border.color

Rectangle {
    id: rectangle
}
```

예를 들어, 다음은 alias property인 buttonText를 가진 Button type입니다. buttonText는 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) child의 text object에 연결되어 있습니다:

```qml
// Button.qml
import QtQuick 2.0

Rectangle {
    property alias buttonText: textItem.text

    width: 100; height: 30; color: "yellow"

    Text { id: textItem }
}
```

다음 코드는 child [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object에 대하여 정의된 text string을 가진 Button을 생성할 것입니다:

```qml
Button { buttonText: "Click Me" }
```

여기서 buttonText를 수정하면 textItem.text 값이 직접 바뀝니다; 다른 값을 바꾸지 않고 textItem.text을 업데이트합니다. 만약 buttonText가 alias가 아니었더라면 이 값을 바꾸는 것은 표시되는 텍스트를 실제로 바꾸지 않을 것입니다. 왜냐하면 property binding이 쌍방향이 아니기 때문입니다: 만약 textItem.text가 바뀌었다면 buttonText도 바뀌겠지만 그 반대로는 변경되지 않습니다.

* Property Alias에 대해 고려할 사항

Alias는 일단 component가 완전히 초기화되었을 때에만 활성화됩니다. 초기화되지 않은 alias를 참조하면 오류가 발생합니다. 마찬가지로 aliasing property를 aliasing하는 것도 오류가 발생합니다.

```qml
property alias widgetLabel: label

//오류가 발생함
//widgetLabel.text: "Initial text"

//오류가 발생함
//property alias widgetLabelText: widgetLabel.text

Component.onCompleted: widgetLabel.text = "Alias completed Initialization"
```

그러나 root object에 property alias를 갖고 있는 [QML object type](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html)을 가져올 때에는 property가 일반 Qt property로 나타나므로 alias 참조에 사용할 수 있습니다.

aliasing property가 기존 property와 같은 이름을 갖는 것이 가능하므로 사실상 기존 property를 덮어쓸 수 있습니다. 예를 들어, 다음 QML type은 내장된 [Rectangle::color](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html#color-prop) property와 동일한 이름을 가진 color alias property를 갖고 있습니다:

```qml
Rectangle {
    id: coloredrectangle
    property alias color: bluerectangle.color
    color: "red"

    Rectangle {
        id: bluerectangle
        color: "#1234ff"
    }

    Component.onCompleted: {
        console.log (coloredrectangle.color)    // "#1234ff" 출력함
        setInternalColor()
        console.log (coloredrectangle.color)    // "#111111" 출력함
        coloredrectangle.color = "#884646"
        console.log (coloredrectangle.color)    // "#884646" 출력함
    }

    // 내부 property에 접근할 수 있는 내부 함수
    function setInternalColor() {
        color = "#111111"
    }
}
```

Any object that use this type and refer to its color property will be referring to the alias rather than the ordinary [Rectangle::color](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html#color-prop) property. Internally, however, the rectangle can correctly set its color property and refer to the actual defined property rather than the alias.

* Property Alias와 Type

Property aliases cannot have explicit type specifications. The type of a property alias is the declared type of the property or object it refers to. Therefore, if you create an alias to an object referenced via id with extra properties declared inline, the extra properties won't be accessible through the alias:

```qml
// MyItem.qml
Item {
    property alias inner: innerItem

    Item {
        id: innerItem
        property int extraProperty
    }
}
```

You cannot initialize inner.extraProperty from outside of this component, as inner is only an Item:

```qml
// main.qml
MyItem {
    inner.extraProperty: 5 // fails
}
```

However, if you extract the inner object into a separate component with a dedicated .qml file, you can instantiate that component instead and have all its properties available through the alias:

```qml
// MainItem.qml
Item {
    // Now you can access inner.extraProperty, as inner is now an ExtraItem
    property alias inner: innerItem

    ExtraItem {
        id: innerItem
    }
}

// ExtraItem.qml
Item {
    property int extraProperty
}
```

* Default Properties

An object definition can have a single default property. A default property is the property to which a value is assigned if an object is declared within another object's definition without declaring it as a value for a particular property.

Declaring a property with the optional default keyword marks it as the default property. For example, say there is a file MyLabel.qml with a default property someText:

```qml
// MyLabel.qml
import QtQuick 2.0

Text {
    default property var someText

    text: "Hello, " + someText.text
}
```

```qml
The someText value could be assigned to in a MyLabel object definition, like this:

MyLabel {
    Text { text: "world!" }
}
```

This has exactly the same effect as the following:

```qml
MyLabel {
    someText: Text { text: "world!" }
}
```

However, since the someText property has been marked as the default property, it is not necessary to explicitly assign the [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object to this property.

You will notice that child objects can be added to any [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html)-based type without explicitly adding them to the [children](https://doc.qt.io/qt-6/qml-qtquick-item.html#children-prop) property. This is because the default property of [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) is its data property, and any items added to this list for an [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) are automatically added to its list of [children](https://doc.qt.io/qt-6/qml-qtquick-item.html#children-prop).

Default properties can be useful for reassigning the children of an item. For example:

```qml
Item {
    default property alias content: inner.children

    Item {
        id: inner
     }
}
```

By setting the default property alias to inner.children, any object assigned as a child of the outer item is automatically reassigned as a child of the inner item.

* Required Properties

An object declaration may define a property as required, using the required keyword. The syntax is

```qml
required property <propertyType> <propertyName>
```

As the name suggests, required properties must be set when an instance of the object is created. Violation of this rule will result in QML applications not starting if it can be detected statically. In case of dynamically instantiated QML components (for instance via [Qt.createComponent](https://doc.qt.io/qt-6/qml-qtqml-qt.html#createComponent-method)()), violating this rule results in a warning and a null return value.

It's possible to make an existing property required with

```qml
required <propertyName>
```

The following example shows how to create a custom Rectangle component, in which the color property always needs to be specified.

```qml
// ColorRectangle.qml
Rectangle {
    required color
}
```

Note: You can't assign an initial value to a required property from QML, as that would go directly against the intended usage of required properties.

Required properties play a special role in model-view-delegate code: If the delegate of a view has required properties whose names match with the role names of the view's model, then those properties will be initialized with the model's corresponding values. For more information, visit the [Models and Views in Qt Quick](https://doc.qt.io/qt-6/qtquick-modelviewsdata-modelview.html) page.

See [QQmlComponent::createWithInitialProperties](https://doc.qt.io/qt-6/qqmlcomponent.html#createWithInitialProperties), [QQmlApplicationEngine::setInitialProperties](https://doc.qt.io/qt-6/qqmlapplicationengine.html#setInitialProperties) and [QQuickView::setInitialProperties](https://doc.qt.io/qt-6/qquickview.html#setInitialProperties) for ways to initialize required properties from C++.

* Read-Only Properties

An object declaration may define a read-only property using the readonly keyword, with the following syntax:

```qml
readonly property <propertyType> <propertyName> : <value>
```

Read-only properties must be assigned a static value or a binding expression on initialization. After a read-only property is initialized, you cannot change its static value or binding expression anymore.

For example, the code in the Component.onCompleted block below is invalid:

```qml
Item {
    readonly property int someNumber: 10

    Component.onCompleted: someNumber = 20  // TypeError: Cannot assign to read-only property
}
```

Note: A read-only property cannot also be a [default](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#default-properties) property.

* Property Modifier Objects

Properties can have [property value modifier objects](https://doc.qt.io/qt-6/qtqml-cppintegration-definetypes.html#property-modifier-types) associated with them. The syntax for declaring an instance of a property modifier type associated with a particular property is as follows:

```qml
<PropertyModifierTypeName> on <propertyName> {
    // attributes of the object instance
}
```

This is commonly referred to as "on" syntax.

It is important to note that the above syntax is in fact an [object declaration](https://doc.qt.io/qt-6/qtqml-syntax-basics.html#object-declarations) which will instantiate an object which acts on a pre-existing property.

Certain property modifier types may only be applicable to specific property types, however this is not enforced by the language. For example, the NumberAnimation type provided by QtQuick will only animate numeric-type (such as int or real) properties. Attempting to use a NumberAnimation with non-numeric property will not result in an error, however the non-numeric property will not be animated. The behavior of a property modifier type when associated with a particular property type is defined by its implementation.

##### Signal Attributes

A signal is a notification from an object that some event has occurred: for example, a property has changed, an animation has started or stopped, or when an image has been downloaded. The [MouseArea](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html) type, for example, has a [clicked](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html#clicked-signal) signal that is emitted when the user clicks within the mouse area.

An object can be notified through a [signal handler](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-handler-attributes) whenever a particular signal is emitted. A signal handler is declared with the syntax on<Signal> where <Signal> is the name of the signal, with the first letter capitalized. The signal handler must be declared within the definition of the object that emits the signal, and the handler should contain the block of JavaScript code to be executed when the signal handler is invoked.

For example, the onClicked signal handler below is declared within the [MouseArea](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html) object definition, and is invoked when the [MouseArea](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html) is clicked, causing a console message to be printed:

```qml
import QtQuick 2.0

Item {
    width: 100; height: 100

    MouseArea {
        anchors.fill: parent
        onClicked: {
            console.log("Click!")
        }
    }
}
```

* Defining Signal Attributes

A signal may be defined for a type in C++ by registering a [Q_SIGNAL](https://doc.qt.io/qt-6/qobject.html#Q_SIGNAL) of a class which is then registered with the QML type system. Alternatively, a custom signal for an object type may be defined in an object declaration in a QML document with the following syntax:

```qml
signal <signalName>[([<parameterName>: <parameterType>[, ...]])]
```

Attempting to declare two signals or methods with the same name in the same type block is an error. However, a new signal may reuse the name of an existing signal on the type. (This should be done with caution, as the existing signal may be hidden and become inaccessible.)

Here are three examples of signal declarations:

```qml
import QtQuick 2.0

Item {
    signal clicked
    signal hovered()
    signal actionPerformed(action: string, actionResult: int)
}
```

You can also specify signal parameters in property style syntax:

```qml
signal actionCanceled(string action)
```

In order to be consistent with method declarations, you should prefer the type declarations using colons.

If the signal has no parameters, the "()" brackets are optional. If parameters are used, the parameter types must be declared, as for the string and var arguments for the actionPerformed signal above. The allowed parameter types are the same as those listed under [Defining Property Attributes](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#defining-property-attributes) on this page.

To emit a signal, invoke it as a method. Any relevant [signal handlers](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-handler-attributes) will be invoked when the signal is emitted, and handlers can use the defined signal argument names to access the respective arguments.

* Property Change Signals

QML types also provide built-in property change signals that are emitted whenever a property value changes, as previously described in the section on [property attributes](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#property-attributes). See the upcoming section on [property change signal handlers](https://doc.qt.io/qt-6/qtqml-syntax-signals.html#property-change-signal-handlers) for more information about why these signals are useful, and how to use them.

* Signal Handler Attributes

Signal handlers are a special sort of [method attribute](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#method-attributes), where the method implementation is invoked by the QML engine whenever the associated signal is emitted. Adding a signal to an object definition in QML will automatically add an associated signal handler to the object definition, which has, by default, an empty implementation. Clients can provide an implementation, to implement program logic.

Consider the following SquareButton type, whose definition is provided in the SquareButton.qml file as shown below, with signals activated and deactivated:

```qml
// SquareButton.qml
Rectangle {
    id: root

    signal activated(xPosition: real, yPosition: real)
    signal deactivated

    property int side: 100
    width: side; height: side

    MouseArea {
        anchors.fill: parent
        onReleased: root.deactivated()
        onPressed: (mouse)=> root.activated(mouse.x, mouse.y)
    }
}
```

These signals could be received by any SquareButton objects in another QML file in the same directory, where implementations for the signal handlers are provided by the client:

```qml
// myapplication.qml
SquareButton {
    onDeactivated: console.log("Deactivated!")
    onActivated: (xPosition, yPosition)=> console.log("Activated at " + xPosition + "," + yPosition)
}
```

Signal handlers don't have to declare their parameter types because the signal already specifies them. The arrow function syntax shown above does not support type annotations.

See the [Signal and Handler Event System](https://doc.qt.io/qt-6/qtqml-syntax-signals.html) for more details on use of signals.

* Property Change Signal Handlers

Signal handlers for property change signal take the syntax form on<Property>Changed where <Property> is the name of the property, with the first letter capitalized. For example, although the [TextInput](https://doc.qt.io/qt-6/qml-qtquick-textinput.html) type documentation does not document a textChanged signal, this signal is implicitly available through the fact that [TextInput](https://doc.qt.io/qt-6/qml-qtquick-textinput.html) has a [text](https://doc.qt.io/qt-6/qml-qtquick-textinput.html#text-prop) property and so it is possible to write an onTextChanged signal handler to be called whenever this property changes:

```qml
import QtQuick 2.0

TextInput {
    text: "Change this!"

    onTextChanged: console.log("Text has changed to:", text)
}
```

##### Method Attributes

A method of an object type is a function which may be called to perform some processing or trigger further events. A method can be connected to a signal so that it is automatically invoked whenever the signal is emitted. See [Signal and Handler Event System](https://doc.qt.io/qt-6/qtqml-syntax-signals.html) for more details.

* Defining Method Attributes

A method may be defined for a type in C++ by tagging a function of a class which is then registered with the QML type system with [Q_INVOKABLE](https://doc.qt.io/qt-6/qobject.html#Q_INVOKABLE) or by registering it as a [Q_SLOT](https://doc.qt.io/qt-6/qobject.html#Q_SLOT) of the class. Alternatively, a custom method can be added to an object declaration in a QML document with the following syntax:

```qml
function <functionName>([<parameterName>[: <parameterType>][, ...]]) [: <returnType>] { <body> }
```

Methods can be added to a QML type in order to define standalone, reusable blocks of JavaScript code. These methods can be invoked either internally or by external objects.

Unlike signals, method parameter types do not have to be declared as they default to the var type. You should, however, declare them in order to help qmlcachegen generate more performant code, and to improve maintainability.

Attempting to declare two methods or signals with the same name in the same type block is an error. However, a new method may reuse the name of an existing method on the type. (This should be done with caution, as the existing method may be hidden and become inaccessible.)

Below is a [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) with a calculateHeight() method that is called when assigning the height value:

```qml
import QtQuick 2.0
Rectangle {
    id: rect

    function calculateHeight() : real {
        return rect.width / 2;
    }

    width: 100
    height: calculateHeight()
}
```

If the method has parameters, they are accessible by name within the method. Below, when the [MouseArea](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html) is clicked it invokes the moveTo() method which can then refer to the received newX and newY parameters to reposition the text:

```qml
import QtQuick 2.0

Item {
    width: 200; height: 200

    MouseArea {
        anchors.fill: parent
        onClicked: (mouse)=> label.moveTo(mouse.x, mouse.y)
    }

    Text {
        id: label

        function moveTo(newX: real, newY: real) {
            label.x = newX;
            label.y = newY;
        }

        text: "Move me!"
    }
}
```

##### 부착된 Properties와 부착된 Signal Handlers

Attached properties and attached signal handlers are mechanisms that enable objects to be annotated with extra properties or signal handlers that are otherwise unavailable to the object. In particular, they allow objects to access properties or signals that are specifically relevant to the individual object.

A QML type implementation may choose to [create an attaching type in C++](https://doc.qt.io/qt-6/qtqml-cppintegration-definetypes.html#providing-attached-properties) with particular properties and signals. Instances of this type can then be created and attached to specific objects at run time, allowing those objects to access the properties and signals of the attaching type. These are accessed by prefixing the properties and respective signal handlers with the name of the attaching type.

References to attached properties and handlers take the following syntax form:

```qml
<AttachingType>.<propertyName>
<AttachingType>.on<SignalName>
```

For example, the [ListView](https://doc.qt.io/qt-6/qml-qtquick-listview.html) type has an attached property [ListView.isCurrentItem](https://doc.qt.io/qt-6/qml-qtquick-listview.html#isCurrentItem-attached-prop) that is available to each delegate object in a [ListView](https://doc.qt.io/qt-6/qml-qtquick-listview.html). This can be used by each individual delegate object to determine whether it is the currently selected item in the view:

```qml
import QtQuick 2.0

ListView {
    width: 240; height: 320
    model: 3
    delegate: Rectangle {
        width: 100; height: 30
        color: ListView.isCurrentItem ? "red" : "yellow"
    }
}
```

In this case, the name of the attaching type is ListView and the property in question is isCurrentItem, hence the attached property is referred to as ListView.isCurrentItem.

An attached signal handler is referred to in the same way. For example, the [Component.onCompleted](https://doc.qt.io/qt-6/qml-qtqml-component.html#completed-signal) attached signal handler is commonly used to execute some JavaScript code when a component's creation process has been completed. In the example below, once the [ListModel](https://doc.qt.io/qt-6/qml-qtqml-models-listmodel.html) has been fully created, its Component.onCompleted signal handler will automatically be invoked to populate the model:

```qml
import QtQuick 2.0

ListView {
    width: 240; height: 320
    model: ListModel {
        id: listModel
        Component.onCompleted: {
            for (var i = 0; i < 10; i++)
                listModel.append({"Name": "Item " + i})
        }
    }
    delegate: Text { text: index }
}
```

Since the name of the attaching type is Component and that type has a [completed](https://doc.qt.io/qt-6/qml-qtqml-component.html#completed-signal) signal, the attached signal handler is referred to as Component.onCompleted.

* A Note About Accessing Attached Properties and Signal Handlers

A common error is to assume that attached properties and signal handlers are directly accessible from the children of the object to which these attributes have been attached. This is not the case. The instance of the attaching type is only attached to specific objects, not to the object and all of its children.

For example, below is a modified version of the earlier example involving attached properties. This time, the delegate is an [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) and the colored [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) is a child of that item:

```qml
import QtQuick 2.0

ListView {
    width: 240; height: 320
    model: 3
    delegate: Item {
        width: 100; height: 30

        Rectangle {
            width: 100; height: 30
            color: ListView.isCurrentItem ? "red" : "yellow"    // WRONG! This won't work.
        }
    }
}
```

This does not work as expected because ListView.isCurrentItem is attached only to the root delegate object, and not its children. Since the [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) is a child of the delegate, rather than being the delegate itself, it cannot access the isCurrentItem attached property as ListView.isCurrentItem. So instead, the rectangle should access isCurrentItem through the root delegate:

```qml
ListView {
    //....
    delegate: Item {
        id: delegateItem
        width: 100; height: 30

        Rectangle {
            width: 100; height: 30
            color: delegateItem.ListView.isCurrentItem ? "red" : "yellow"   // correct
        }
    }
}
```

Now delegateItem.ListView.isCurrentItem correctly refers to the isCurrentItem attached property of the delegate.

##### Enumeration Attributes

Enumerations provide a fixed set of named choices. They can be declared in QML using the enum keyword:

```qml
// MyText.qml
Text {
    enum TextType {
        Normal,
        Heading
    }
}
```

As shown above, enumeration types (e.g. TextType) and values (e.g. Normal) must begin with an uppercase letter.

Values are referred to via <Type>.<EnumerationType>.<Value> or <Type>.<Value>.

```qml
// MyText.qml
Text {
    enum TextType {
        Normal,
        Heading
    }

    property int textType: MyText.TextType.Normal

    font.bold: textType == MyText.TextType.Heading
    font.pixelSize: textType == MyText.TextType.Heading ? 24 : 12
}
```

More information on enumeration usage in QML can be found in the [QML Value Types](https://doc.qt.io/qt-6/qtqml-typesystem-valuetypes.html) [enumeration](https://doc.qt.io/qt-6/qml-enumeration.html) documentation.

The ability to declare enumerations in QML was introduced in Qt 5.10.






#### 프로퍼티 바인딩

#### 시그널과 핸들러 이벤트 시스템

#### QML과 JavaScript 통합하기

#### QML 타입 시스템

#### QML 모듈

#### QML 도큐먼트
