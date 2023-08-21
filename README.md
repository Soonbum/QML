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
* 부착된 property들과 부착된 여러 개의 signal handler attribute
* 여러 개의 enumeration attribute

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

...

* Assigning Values to Property Attributes

The value of a property of an object instance may be specified in two separate ways:

- a value assignment on initialization
- an imperative value assignment

In either case, the value may be either a static value or a binding expression value.

* Value Assignment on Initialization

The syntax for assigning a value to a property on initialization is:

```qml
<propertyName> : <value>
```

An initialization value assignment may be combined with a property definition in an object declaration, if desired. In that case, the syntax of the property definition becomes:

```qml
[default] property <propertyType> <propertyName> : <value>
```

An example of property value initialization follows:

```qml
import QtQuick 2.0

Rectangle {
    color: "red"
    property color nextColor: "blue" // combined property declaration and initialization
}
```

* Imperative Value Assignment

An imperative value assignment is where a property value (either static value or binding expression) is assigned to a property from imperative JavaScript code. The syntax of an imperative value assignment is just the JavaScript assignment operator, as shown below:

```qml
[<objectId>.]<propertyName> = value
```

An example of imperative value assignment follows:

```qml
import QtQuick 2.0

Rectangle {
    id: rect
    Component.onCompleted: {
        rect.color = "red"
    }
}
```

* Static Values and Binding Expression Values

As previously noted, there are two kinds of values which may be assigned to a property: static values, and binding expression values. The latter are also known as [property bindings](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html).

| Kind | Semantics |
| --- | --- |
| Static Value | A constant value which does not depend on other properties. |
| Binding Expression | A JavaScript expression which describes a property's relationship with other properties. The variables in this expression are called the property's dependencies. The QML engine enforces the relationship between a property and its dependencies. When any of the dependencies change in value, the QML engine automatically re-evaluates the binding expression and assigns the new result to the property. |

Here is an example that shows both kinds of values being assigned to properties:

```qml
import QtQuick 2.0

Rectangle {
    // both of these are static value assignments on initialization
    width: 400
    height: 200

    Rectangle {
        // both of these are binding expression value assignments on initialization
        width: parent.width / 2
        height: parent.height
    }
}
```

Note: To assign a binding expression imperatively, the binding expression must be contained in a function that is passed into Qt.binding(), and then the value returned by [Qt.binding](https://doc.qt.io/qt-6/qml-qtqml-qt.html#binding-method)() must be assigned to the property. In contrast, Qt.binding() must not be used when assigning a binding expression upon initialization. See [Property Binding](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html) for more information.

* Type Safety

Properties are type safe. A property can only be assigned a value that matches the property type.

For example, if a property is a real, and if you try to assign a string to it, you will get an error:

```qml
property int volume: "four"  // generates an error; the property's object will not be loaded
```

Likewise if a property is assigned a value of the wrong type during run time, the new value will not be assigned, and an error will be generated.

Some property types do not have a natural value representation, and for those property types the QML engine automatically performs string-to-typed-value conversion. So, for example, even though properties of the color type store colors and not strings, you are able to assign the string "red" to a color property, without an error being reported.

See [QML Value Types](https://doc.qt.io/qt-6/qtqml-typesystem-valuetypes.html) for a list of the types of properties that are supported by default. Additionally, any available [QML object type](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html) may also be used as a property type.

* Special Property Types

* Object List Property Attributes

A [list](https://doc.qt.io/qt-6/qml-list.html) type property can be assigned a list of QML object-type values. The syntax for defining an object list value is a comma-separated list surrounded by square brackets:

```qml
[ <item 1>, <item 2>, ... ]
```

For example, the [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) type has a [states](https://doc.qt.io/qt-6/qml-qtquick-item.html#states-prop) property that is used to hold a list of [State](https://doc.qt.io/qt-6/qml-qtquick-state.html) type objects. The code below initializes the value of this property to a list of three [State](https://doc.qt.io/qt-6/qml-qtquick-state.html) objects:

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

If the list contains a single item, the square brackets may be omitted:

```qml
import QtQuick 2.0

Item {
    states: State { name: "running" }
}
```

A [list](https://doc.qt.io/qt-6/qml-list.html) type property may be specified in an object declaration with the following syntax:

```qml
[default] property list<<objectType>> propertyName
```

and, like other property declarations, a property initialization may be combined with the property declaration with the following syntax:

```qml
[default] property list<<objectType>> propertyName: <value>
```

An example of list property declaration follows:

```qml
import QtQuick 2.0

Rectangle {
    // declaration without initialization
    property list<Rectangle> siblingRects

    // declaration with initialization
    property list<Rectangle> childRects: [
        Rectangle { color: "red" },
        Rectangle { color: "blue"}
    ]
}
```

If you wish to declare a property to store a list of values which are not necessarily QML object-type values, you should declare a [var](https://doc.qt.io/qt-6/qml-var.html) property instead.

* Grouped Properties

In some cases properties contain a logical group of sub-property attributes. These sub-property attributes can be assigned to using either the dot notation or group notation.

For example, the [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) type has a [font](https://doc.qt.io/qt-6/qml-qtquick-text.html#font.family-prop) group property. Below, the first [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object initializes its font values using dot notation, while the second uses group notation:

```qml
Text {
    //dot notation
    font.pixelSize: 12
    font.b: true
}

Text {
    //group notation
    font { pixelSize: 12; b: true }
}
```

Grouped property types are types which have subproperties. If a grouped property type is an object type (as opposed to a value type), the property that holds it must be read-only. This is to prevent you from replacing the object the subproperties belong to.

* Property Aliases

Property aliases are properties which hold a reference to another property. Unlike an ordinary property definition, which allocates a new, unique storage space for the property, a property alias connects the newly declared property (called the aliasing property) as a direct reference to an existing property (the aliased property).

A property alias declaration looks like an ordinary property definition, except that it requires the alias keyword instead of a property type, and the right-hand-side of the property declaration must be a valid alias reference:

```qml
[default] property alias <name>: <alias reference>
```

Unlike an ordinary property, an alias has the following restrictions:

- It can only refer to an object, or the property of an object, that is within the scope of the [type](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html) within which the alias is declared.
- It cannot contain arbitrary JavaScript expressions
- It cannot refer to objects declared outside of the scope of its type.
- The alias reference is not optional, unlike the optional default value for an ordinary property; the alias reference must be provided when the alias is first declared.
- It cannot refer to [attached properties](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#attached-properties-and-attached-signal-handlers).
- It cannot refer to properties inside a hierarchy with depth 3 or greater. The following code will not work:

```qml
property alias color: myItem.myRect.border.color

Item {
    id: myItem
    property Rectangle myRect
}
```

However, aliases to properties that are up to two levels deep will work.

```qml
property alias color: rectangle.border.color

Rectangle {
    id: rectangle
}
```

For example, below is a Button type with a buttonText aliased property which is connected to the text object of the [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) child:

```qml
// Button.qml
import QtQuick 2.0

Rectangle {
    property alias buttonText: textItem.text

    width: 100; height: 30; color: "yellow"

    Text { id: textItem }
}
```

The following code would create a Button with a defined text string for the child [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) object:

```qml
Button { buttonText: "Click Me" }
```

Here, modifying buttonText directly modifies the textItem.text value; it does not change some other value that then updates textItem.text. If buttonText was not an alias, changing its value would not actually change the displayed text at all, as property bindings are not bi-directional: the buttonText value would have changed if textItem.text was changed, but not the other way around.

* Considerations for Property Aliases

Aliases are only activated once a component has been fully initialized. An error is generated when an uninitialized alias is referenced. Likewise, aliasing an aliasing property will also result in an error.

```qml
property alias widgetLabel: label

//will generate an error
//widgetLabel.text: "Initial text"

//will generate an error
//property alias widgetLabelText: widgetLabel.text

Component.onCompleted: widgetLabel.text = "Alias completed Initialization"
```

When importing a [QML object type](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html) with a property alias in the root object, however, the property appear as a regular Qt property and consequently can be used in alias references.

It is possible for an aliasing property to have the same name as an existing property, effectively overwriting the existing property. For example, the following QML type has a color alias property, named the same as the built-in [Rectangle::color](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html#color-prop) property:

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
        console.log (coloredrectangle.color)    //prints "#1234ff"
        setInternalColor()
        console.log (coloredrectangle.color)    //prints "#111111"
        coloredrectangle.color = "#884646"
        console.log (coloredrectangle.color)    //prints #884646
    }

    //internal function that has access to internal properties
    function setInternalColor() {
        color = "#111111"
    }
}
```

Any object that use this type and refer to its color property will be referring to the alias rather than the ordinary [Rectangle::color](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html#color-prop) property. Internally, however, the rectangle can correctly set its color property and refer to the actual defined property rather than the alias.

* Property Aliases and Types

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

* Signal Attributes

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







#### 프로퍼티 바인딩

#### 시그널과 핸들러 이벤트 시스템

#### QML과 JavaScript 통합하기

#### QML 타입 시스템

#### QML 모듈

#### QML 도큐먼트
