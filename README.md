# QML (Qt Modeling Language)

### 목차

* [QML의 예시](#qml의-예시)
* [QML 용어](#qml-용어)
* [QML 레퍼런스](#qml-레퍼런스)
  - [QML 구문 기초](#qml-구문-기초)
    * [import 구문](#import-구문)
    * [객체 선언](#객체-선언)
    * [자식 객체](#자식-객체)
    * [코멘트](#코멘트)
  - [QML 객체 애트리뷰트](#qml-객체-애트리뷰트)
    * [객체 선언 내에서의 애트리뷰트](#객체-선언-내에서의-애트리뷰트)
    * [id 애트리뷰트](#id-애트리뷰트)
    * [프로퍼티 애트리뷰트](#프로퍼티-애트리뷰트)
    * [시그널 애트리뷰트](#시그널-애트리뷰트)
    * [메서드 애트리뷰트](#메서드-애트리뷰트)
    * [부착된 프로퍼티와 부착된 시그널 핸들러](#부착된-프로퍼티와-부착된-시그널-핸들러)
    * [열거형 애트리뷰트](#열거형-애트리뷰트)
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
| 타입(Type) | QML에는 [값 타입](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html) 또는 [QML 객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)이라는 타입이 있습니다. QML 언어는 많은 내장 [값 타입](https://doc.qt.io/qt-6/qtqml-typesystem-valuetypes.html)을 제공합니다. 그리고 Qt Quick 모듈은 QML 앱을 만들기 위한 다양한 [Qt Quick 타입](https://doc.qt.io/qt-6/qtquick-qmlmodule.html)을 제공합니다. 또, 서드파티 개발자들이 [모듈](https://doc.qt.io/qt-6/qtqml-modules-topic.html)을 통해, 혹은 앱 개발자가 [QML 문서](https://doc.qt.io/qt-6/qtqml-documents-definetypes.html)를 통해 앱에서 자체적으로 타입을 제공할 수도 있습니다. 자세한 것은 [QML 타입 시스템](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)을 보십시오. |
| 값(Value) 타입 | [값 타입](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)은 int, string, bool과 같은 단순한 타입입니다. [객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)과 달리 객체는 값 타입으로부터 인스턴스를 만들 수 없습니다. 예를 들어, 프로퍼티, 메서드, 시그널 등을 이용하여 int 객체를 만들 수 없습니다. 객체 타입 뿐만 아니라 값 타입도 [QML 모듈](https://doc.qt.io/qt-6/qtqml-modules-topic.html)에 속해 있습니다. 이것들을 사용하려면 모듈을 import해야 합니다. 일부 타입들은 언어에 내장되어 있습니다. 예를 들면 int, bool, double, string과 [QtObject](https://doc.qt.io/qt-6/qml-qtqml-qtobject.html)와 컴포넌트가 있습니다. 자세한 것은 [QML 타입 시스템](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)을 보십시오. |
| 객체(Object) 타입 | [QML 객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)은 QML 엔진에 의해 인스턴스화 할 수 있는 타입을 의미합니다. QML 타입은 대문자로 시작하는 .qml 파일 내 도큐먼트, 혹은 [QObject](https://doc.qt.io/qt-6/qobject.html)-기반 C++ 클래스에 의해 정의될 수 있습니다. 자세한 것은 [QML 타입 시스템](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)을 보십시오. |
| 객체(Object) | QML Object는 [QML 객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)의 인스턴스입니다. 이 객체들은 [객체 선언](https://doc.qt.io/qt-6/qtqml-syntax-basics.html)을 처리할 때 엔진에 의해 생성됩니다. 객체 선언에는 생성될 객체를 정의하거나 객 객체에 대하여 정의되는 속성이 있습니다. 게다가 객체는 Component.createObject()와 Qt.createQmlObject() 함수를 통해 런타임 시에 동적으로 생성될 수 있습니다. [Lazy Instantiation](https://doc.qt.io/qt-6/qml-glossary.html#lazy-instantiation)도 보십시오. |
| 컴포넌트(Component) | 컴포넌트는 QML Object 또는 Object tree가 생성되는 템플릿입니다. QML 엔진에 의해 도큐먼트가 로드될 때 컴포넌트가 생성됩니다. 일단 한 번 로드되면 그것을 표현하는 Object 또는 Object tree를 인스턴스화 하는 데 사용될 수 있습니다. 게다가 [컴포넌트](https://doc.qt.io/qt-6/qml-qtqml-component.html) 타입은 도큐먼트 안에서 인라인으로 선언하는 데 사용될 수 있는 특수 타입입니다. 컴포넌트 객체들은 동적으로 QML object들을 생성하는 Qt.createComponent()를 통해서도 동적으로 생성될 수 있습니다. |
| 도큐먼트(Document) | [QML Document](https://doc.qt.io/qt-6/qtqml-documents-topic.html)는 1개 이상의 import 구문으로 시작하고 단일 최상위 레벨 object 선언을 포함하는 자체 포함 QML 소스 코드 조각입니다. 도큐먼트는 .qml 파일 또는 텍스트 문자열 안에 있을 수 있습니다. 만약 도큐먼트가 대문자로 시작하는 이름을 가진 .qml 파일 안에 있다면, 엔진은 이 파일이 QML 타입의 정의라고 인식합니다. 최상위 레벨 객체 선언은 타입에 의해 인스턴스화될 Object tree를 캡슐화합니다. |
| 프로퍼티(Property) | 이름을 가지고 있으며 연관된 값을 가진 객체 타입의 속성입니다. 이 값은 외부에서 읽혀질 수 있습니다. (그리고 대부분의 경우 쓰여질 수도 있습니다.) 객체는 여러 개의 프로퍼티를 가질 수 있습니다. 여타 프로퍼티들은 해당 타입에 대한 데이터인 반면(예. [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 타입의 "text" 프로퍼티), 몇 가지 프로퍼티들은 canvas와 연관되어 있습니다(예. x, y, width, height, opacity). 자세한 것은 [QML 객체 애트리뷰트](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html)를 보십시오. |
| 바인딩(Binding) | 바인딩은 프로퍼티와 "결합"되어 있는 JavaScript 표현식을 의미합니다. 프로퍼티의 값은 해당 표현식을 평가하여 리턴되는 값입니다. 자세한 것은 [Property Binding](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html)을 보십시오. |
| 시그널(Signal) | 시그널은 QML 객체로부터의 알림을 의미합니다. 어떤 객체가 시그널을 방출(emit)하면, 다른 객체들은 이것을 듣고 [시그널 핸들러](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-attributes)를 통해 시그널을 처리할 수 있습니다. QML 객체의 대부분의 프로퍼티들은 변경 시그널(Change Signal)을 가지고 있습니다. 그리고 기능을 구현하기 위해 클라이언트가 정의한 연관된 변경 시그널 핸들러(Change Signal Handler)도 가지고 있습니다. 예를 들어 앱에서 정의된 MouseArea 타입 인스턴스의 "onClicked()" 핸들러는 사운드 재생을 발생시킬 수 있습니다. 자세한 것은 [Signal and Handler Event System](https://doc.qt.io/qt-6/qtqml-syntax-signals.html)을 보십시오. |
| 시그널 핸들러(Signal Handler) | 시그널 핸들러는 시그널에 의해 트리거되는 표현식(또는 함수)입니다. C++에서는 이것을 "슬롯(slot)"이라고도 합니다. 자세한 것은 [Signal and Handler Event System](https://doc.qt.io/qt-6/qtqml-syntax-signals.html)을 보십시오. |
| 늦은 인스턴스화(Lazy Instantiation) | 필요할 때까지 불필요한 작업을 하지 않기 위해 객체 인스턴스는 런타임 시 "늦게" 인스턴스화 될 수 있습니다. Qt Quick은 편하게 Lazy Instantiation을 구현하기 위해 [Loader](https://doc.qt.io/qt-6/qml-qtquick-loader.html) 타입을 제공합니다. |

### [QML 레퍼런스](https://doc.qt.io/qt-6/qmlreference.html)

QML은 고도의 동적 앱을 만들기 위한 멀티 패러다임 언어입니다. QML을 이용하면 UI 컴포넌트 같은 앱 빌딩 블럭을 선언할 수 있고 앱 동작을 정의할 수 있는 다양한 프로퍼티 집합이 있습니다. 앱 동작은 JavaScript를 통해서도 스크립팅이 가능합니다. 게다가 QML은 Qt를 많이 사용하므로 QML 앱에서 타입 및 기타 Qt 기능에 직접 접근할 수 있습니다.

이 레퍼런스 가이드는 QML 언어의 기능에 대해 설명합니다. 이 가이드에 있는 많은 QML 타입들은 [Qt QML](https://doc.qt.io/qt-6/qtqml-index.html) 또는 [Qt Quick](https://doc.qt.io/qt-6/qtquick-index.html) 모듈에 기원을 두고 있습니다.


#### QML 구문 기초

QML은 객체가 애트리뷰트 및 다른 객체의 변경사항과의 관계와 응답하는 방식으로 정의될 수 있게 해주는 다중 패러다임 언어입니다. 애트리뷰트와 동작의 변경사항이 단계적으로 처리되는 일련의 명령문들을 통해 표현되는 순수한 명령형 코드와 달리 QML의 선언적 구문은 애트리뷰트와 동작 변경사항을 개별 객체의 정의에 직접 통합합니다. 이러한 애트리뷰트 정의에는 복잡한 커스텀 앱 동작이 필요한 경우 명령형 코드가 포함될 수 있습니다.

QML 소스 코드는 일반적으로 QML 코드의 독립형 도큐먼트인 QML 도큐먼트를 통해 엔진에 의해 로드됩니다. 이를 사용하여 앱 전체에서 재사용할 수 있는 QML 객체 타입을 정의할 수 있습니다. QML 파일에서 QML 객체 타입으로 선언하려면 타입 이름은 반드시 대문자로 시작해야 합니다.

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

##### 객체 선언

구문적으로 QML 코드 블럭은 생성될 QML 객체 트리를 정의합니다. 객체는 객체에 주어진 애트리뷰트뿐만 아니라 생성될 객체의 타입을 설명하는 객체 선언을 사용하여 정의됩니다. 각 객체는 중첩된 객체 선언을 사용하여 자식 객체를 선언할 수도 있습니다.

객체 선언은 해당 객체 타입의 이름과 {} 세트로 구성되어 있습니다. 모든 애트리뷰트와 자식 객체는 이 {} 안에서 정의됩니다.

다음은 간단한 객체 선언입니다:

```qml
Rectangle {
    width: 100
    height: 100
    color: "red"
}
```

이 선언은 타입 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)의 객체입니다. 바로 뒤에는 객체에 대해 정의된 애트리뷰트를 감싸는 {}가 나옵니다. [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 타입은 QtQuick 모듈을 통해 만들 수 있으며 이 경우 정의된 애트리뷰트는 직사각형의 너비(width), 높이(height), 컬러(color) 프로퍼티의 값입니다.

위의 객체는 [QML document](https://doc.qt.io/qt-6/qtqml-documents-topic.html)의 일부인 경우 엔진에 의해 로드될 수 있습니다. 즉, ([Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 타입을 이용할 수 있도록) 소스 코드에 QtQuick 모듈을 가져오는 import 구문을 추가하면 아래와 같습니다:

```qml
import QtQuick 2.0

Rectangle {
    width: 100
    height: 100
    color: "red"
}
```

어떤 .qml 파일에 배치되고 QML 엔진에 의해 로드되면, 위의 코드는 QtQuick 모듈에 의해 지원되는 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 타입을 이용하여 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 객체를 생성합니다.

만약 객체 정의가 적은 수의 프로퍼티를 갖고 있다면 다음과 같이 프로퍼티들을 세미콜론으로 분리하여 한 줄로 작성할 수 있습니다.

```qml
Rectangle { width: 100; height: 100; color: "red" }
```

분명히 예제에서 선언된 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 객체는 실제로 매우 간단합니다. 이는 적은 수의 프로퍼티 값들만 정의하기 때문입니다. 좀 더 유용한 객체들을 생성하려면 객체 선언이 여러 가지 애트리뷰트 타입들을 정의해야 합니다: 이것은 [QML 객체 애트리뷰트](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html) 문서에서 논의할 것입니다. 게다가 객체 선언은 아래에서 논의한 대로 자식 객체들을 정의할 수도 있습니다.

##### 자식 객체

어떤 객체 선언이라도 중첩된 객체 선언을 통해 자식 객체들을 정의할 수 있습니다. 이런 식으로 **객체 선언은 여러 개의 자식 객체를 포함하는 객체 트리를 선언할 수 있습니다**.

예를 들어, 아래의 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 객체 선언은 하나의 [Gradient](https://doc.qt.io/qt-6/qml-qtquick-gradient.html) 객체 선언을 포함하고 있으며 또 이것은 2개의 [GradientStop](https://doc.qt.io/qt-6/qml-qtquick-gradientstop.html) 선언을 포함하고 있습니다:

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

엔진이 이 코드를 로드하면, 이것은 루트가 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 객체인 객체 트리를 생성합니다; 이 객체는 [Gradient](https://doc.qt.io/qt-6/qml-qtquick-gradient.html) child object를 가지고 있습니다. 또 이것은 2개의 [GradientStop](https://doc.qt.io/qt-6/qml-qtquick-gradientstop.html) 자식을 갖고 있습니다.

그러나 이것은 시각적 장면의 문맥 QML 자식 트리의 문맥 상에서의 부모-자식 관계라는 점을 유의하십시오. 대부분의 QML 객체들이 시각적으로 렌더링되기 때문에 시각적 장면에서의 부모-자식 관계의 개념은 대부분의 QML 타입에 대한 베이스 타입인 QtQuick 모듈의 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) 타입에 의해 제공됩니다. 예를 들어, [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)과 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html)는 모두 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html)-기반 타입이며, 아래의 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 객체는 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) object의 시각적 자식으로 선언되었습니다:

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

[Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 객체가 위의 코드에서 [parent](https://doc.qt.io/qt-6/qml-qtquick-item.html#parent-prop) 값을 참조할 때, 객체 트리의 부모가 아닌 시각적 부모를 참조합니다. 이 경우 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 객체는 QML 객체 트리에서나 시각적 장면의 문맥에서나 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 객체의 부모입니다. 그러나 시각적 부모를 변경하기 위해 [부모](https://doc.qt.io/qt-6/qml-qtquick-item.html#parent-prop) 프로퍼티를 수정할 수는 있어도 객체 트리의 문맥 상에서 QML로부터 객체의 부모를 변경할 수는 없습니다.

(게다가 직사각형의 gradient 프로퍼티에 [Gradient](https://doc.qt.io/qt-6/qml-qtquick-gradient.html) 객체를 할당한 앞의 예제와 달리 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)의 프로퍼티에 할당하지 않고 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 객체가 선언되었음을 주목하십시오. 이것은 더 편리한 구문을 사용하기 위해 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html#children-prop)의 [자식](https://doc.qt.io/qt-6/qml-qtquick-item.html#children-prop) 프로퍼티가 타입의 [기본 프로퍼티](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#default-properties)로 설정되었기 때문입니다.)

[Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) 타입과 함께 시각적 부모 맺기의 개념에 대한 더 많은 정보를 보려면 [시각적 부모](https://doc.qt.io/qt-6/qtquick-visualcanvas-visualparent.html) 문서를 보십시오.

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

위의 예제에서 Text 객체는 일반적인 불투명도를 갖게 될 것입니다. 왜냐하면 opacity: 0.5가 코멘트로 변했기 때문입니다.


#### QML 객체 애트리뷰트

모든 QML 객체 타입은 정의된 애트리뷰트 집합을 갖고 있습니다. 객체 타입의 각 인스턴스는 해당 객체 타입에 대해 정의된 애트리뷰트 집합을 가지고 생성됩니다. 아래에 나와 있듯이 지정할 수 있는 여러 종류의 애트리뷰트들이 있습니다.

##### 객체 선언 내에서의 애트리뷰트

QML 도큐먼트에서 [객체 선언](https://doc.qt.io/qt-6/qtqml-syntax-basics.html#object-declarations)은 새로운 타입을 정의합니다. 또한 인스턴스화될 객체 계층을 선언합니다. 새로 정의된 타입의 인스턴스가 생성되어야 합니다.

QML 객체-타입 애트리뷰트 타입의 집합은 다음과 같습니다:

* id 애트리뷰트
* 프로퍼티 애트리뷰트
* 시그널 애트리뷰트
* 시그널 핸들러 애트리뷰트
* 메서드 애트리뷰트
* 부착된 프로퍼티와 부착된 시그널 핸들러
* 열거형 애트리뷰트

이 애트리뷰트들은 아래에서 자세히 논의할 것입니다.

##### id 애트리뷰트

모든 QML 객체 타입은 단 1개의 id 애트리뷰트를 갖고 있습니다. 이 애트리뷰트는 언어 자체가 제공하며 다른 QML 객체 타입에 의해 재정의되거나 오버라이드될 수 없습니다.

객체를 식별하고 다른 객체가 참조할 수 있도록 객체 인스턴스의 id 애트리뷰트에 어떤 값을 할당할 수 있습니다. 이 id는 반드시 소문자나 밑줄 문자로 시작해야 하며 문자, 숫자, 밑줄 문자 외에 다른 문자는 포함할 수 없습니다.

아래는 [TextInput](https://doc.qt.io/qt-6/qml-qtquick-textinput.html) 객체와 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 객체입니다. [TextInput](https://doc.qt.io/qt-6/qml-qtquick-textinput.html) 객체의 id 값은 "myTextInput"입니다. [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 객체는 myTextInput.text를 참조함으로써 [TextInput](https://doc.qt.io/qt-6/qml-qtquick-textinput.html)의 text 프로퍼티와 같은 값으로 자신의 text 프로퍼티를 설정합니다:

```qml
import QtQuick 2.0

Column {
    width: 200; height: 200

    TextInput { id: myTextInput; text: "Hello World" }

    Text { text: myTextInput.text }
}
```

객체는 선언된 컴포넌트 범위 안이라면 어디서든지 id로 참조할 수 있습니다. 그러므로 id 값은 컴포넌트 범위 안에서 항상 유일해야 합니다. 더 자세한 것은 [Scope and Naming Resolution](https://doc.qt.io/qt-6/qtqml-documents-scope.html)을 보십시오.

일단 객체 인스턴스가 생성되면, id 애트리뷰트의 값은 바꿀 수 없습니다. 이것은 일반 프로퍼티처럼 보일 수 있지만 id 애트리뷰트는 일반 프로퍼티 애트리뷰트가 아니며 특별한 의미가 적용됩니다. 예를 들어 위의 예제에서는 myTextInput.id에 접근할 수 없습니다.

##### 프로퍼티 애트리뷰트

프로퍼티는 정적인 값을 할당하거나 동적 표현식에 결합할 수 있는 객체의 애트리뷰트입니다. 프로퍼티의 값은 다른 객체가 읽을 수 있습니다. 일반적으로 특정 QML 타입이 특정 프로퍼티에 대해 이것을 명시적으로 허용하지 않는 한 다른 객체에 의해 수정될 수 있습니다.

* 프로퍼티 애트리뷰트 정의하기

C++에서 클래스에 Q_PROPERTY를 등록하여 QML type 시스템에 등록함으로써 프로퍼티는 타입으로 정의될 수 있습니다. 또는 다음 구문을 사용하여 QML 도큐먼트에서 객체 타입의 커스텀 프로퍼티를 객체 선언에서 정의할 수도 있습니다.

```qml
[default] [required] [readonly] property <propertyType> <propertyName>
```

이런 식으로 객체 선언은 객체 외부로 [특정 값을 노출](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html#defining-object-types-from-qml)하거나 좀 더 쉽게 몇 가지 내부 상태를 유지할 수 있습니다.

프로퍼티 이름은 반드시 소문자로 시작해야 하며 글자, 숫자, 밑줄문자로만 이루어질 수 있습니다. [JavaScript 예약어](https://developer.mozilla.org/en/JavaScript/Reference/Reserved_Words)는 유효한 프로퍼티 이름이 아닙니다. default, required, readonly 키워드는 선택적이며 선언될 프로퍼티의 의미를 수정합니다. [기본 프로퍼티](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#default-properties), [필수 프로퍼티](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#required-properties), [읽기-전용 프로퍼티](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#read-only-properties)에 대한 각각의 의미에 대한 자세한 정보는 다음 섹션을 보십시오.

커스텀 프로퍼티를 선언하는 것은 암묵적으로 해당 프로퍼티에 대하여 값-변경 [시그널](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-attributes)과 이름이 on<PropertyName>Changed인 [시그널 핸들러](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-handler-attributes)를 생성합니다. 여기서 <PropertyName>은 프로퍼티의 이름이며 1번째 글자는 대문자입니다.

예를 들어, 다음 객체 선언은 Rectangle 베이스 타입으로부터 파생된 새로운 타입을 정의합니다. 이것은 2개의 새로운 프로퍼티를 갖고 있습니다. 그 중 하나에 [시그널 핸들러](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-handler-attributes)가 구현되어 있습니다:

```qml
Rectangle {
    property color previousColor
    property color nextColor
    onNextColorChanged: console.log("The next color will be: " + nextColor.toString())
}
```

* 커스텀 프로퍼티 정의 내에서 유효한 타입

[열거형](https://doc.qt.io/qt-6/qml-enumeration.html) 타입을 제외한 모든 [QML 값 타입](https://doc.qt.io/qt-6/qtqml-typesystem-valuetypes.html)은 커스텀 프로퍼티 타입으로 사용할 수 있습니다. 예를 들어, 다음은 모두 유효한 프로퍼티 선언입니다:

```qml
Item {
    property int someNumber
    property string someString
    property url someUrl
}
```

(열거형 값은 단순히 정수 값이며 int 타입을 대신 참조할 수 있습니다.)

일부 값 타입은 QtQuick 모듈에서 제공하므로 모듈을 가져오지 않으면 프로퍼티 타입으로 사용할 수 없습니다. 자세한 것은 [QML 값 타입] 문서를 보십시오.

[var](https://doc.qt.io/qt-6/qml-var.html) 값 타입은 모든 타입의 값, 리스트 객체를 보관할 수 있는 generic placeholder 타입이라는 것을 유의하십시오:

```qml
property var someNumber: 1.5
property var someString: "abc"
property var someBool: true
property var someList: [1, 2, "three", "four"]
property var someObject: Rectangle { width: 100; height: 100; color: "red" }
```

게다가 모든 [QML 객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html)은 프로퍼티 타입으로 사용할 수 있습니다. 예를 들면:

```qml
property Item someItem
property Rectangle someRectangle
```

또한 이것은 [커스텀 QML 타입](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html#defining-object-types-from-qml)에 적용됩니다. 만약 QML 타입이 파일 ColorfulButton.qml(클라이언트가 가져온 디렉토리 안)에 정의되어 있다면, 타입 ColorfulButton의 프로퍼티 역시 유효합니다.

* 프로퍼티 애트리뷰트에 값 할당하기

객체 인스턴스의 프로퍼티의 값은 2가지 방식으로 지정할 수 있습니다:

- 초기화 시에 값 할당
- 명령형 값 할당

두 경우 모두 값은 정적이거나 바인딩 표현식 값일 수 있습니다.

* 초기화 시에 값 할당

초기화 시에 프로퍼티에 값을 할당하는 구문은 다음과 같습니다:

```qml
<propertyName> : <value>
```

초기값 할당은 만약 원한다면 객체 선언에서 프로퍼티 정의와 결합될 수 있습니다. 이 경우 프로퍼티 정의 구문은 다음과 같습니다:

```qml
[default] property <propertyType> <propertyName> : <value>
```

프로퍼티 값 초기화 예제는 다음과 같습니다:

```qml
import QtQuick 2.0

Rectangle {
    color: "red"
    property color nextColor: "blue" // 결합된 프로퍼티 선언 및 초기화
}
```

* 명령형 값 할당

명령형 값 할당은 명령형 JavaScript 코드로부터 프로퍼티 값(정적인 값 또는 바인딩 표현식)이 프로퍼티에 할당되는 것입니다. 명령형 값 할당의 구문은 다음과 같이 JavaScript 할당 연산자를 사용합니다:

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

위에서 봤듯이, 프로퍼티에 할당되는 값은 두 종류가 있습니다: 정적인 값, 그리고 바인딩 표현식 값. 후자의 경우를 [프로퍼티 바인딩](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html)이라고도 합니다.

| 종류 | 의미 |
| --- | --- |
| 정적인 값 | 다른 프로퍼티에 의존하지 않는 상수 값입니다. |
| 바인딩 표현식 | 어떤 프로퍼티와 다른 프로퍼티들과의 관계를 설명하는 JavaScript 표현식입니다. 이 표현식에서 변수는 프로퍼티의 의존성이라고도 합니다. QML 엔진은 어떤 프로퍼티와 의존성 간의 관계를 만들어냅니다. 값에서 의존성 변화가 생기면 QML 엔진은 자동으로 바인딩 표현식을 다시 연산하고 프로퍼티에 새로운 결과를 할당합니다. |

다음은 프로퍼티에 할당되는 두 종류의 값을 보여주는 예제입니다:

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

주의: 바인딩 표현식을 명령형으로 할당하려면 바인딩 표현식을 Qt.binding()에 전달된 함수 안에 포함시켜야 합니다. 그리고 나서 [Qt.binding](https://doc.qt.io/qt-6/qml-qtqml-qt.html#binding-method)()이 리턴한 값을 프로퍼티에 할당해야 합니다. 반대로 초기화 시에 바인딩 표현식을 할당할 때에는 Qt.binding()을 사용하지 말아야 합니다. 더 많은 정보는 [프로퍼티 바인딩](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html)을 보십시오.

* 타입 세이프티(Safety)

프로퍼티는 타입 safe입니다. 프로퍼티에는 프로퍼티 타입이 일치하는 값만 할당할 수 있습니다.

예를 들어, 만약 프로퍼티가 real 타입인데 string을 할당하려고 시도한다면 다음과 같은 오류가 발생할 것입니다:

```qml
property int volume: "four"  // 오류가 발생함; 프로퍼티의 객체가 로드되지 않을 것입니다.
```

마찬가지로 런타임 중에 프로퍼티에 잘못된 타입의 값이 할당되면 새로운 값이 할당되지 않고 오류가 발생할 것입니다.

일부 프로퍼티 타입들은 자연 값 표현식을 갖고 있지 않으며 이러한 프로퍼티 타입에 대해서는 QML 엔진이 자동으로 string-to-typed-value 변환을 수행합니다. 예를 들면, color 타입의 프로퍼티에 문자열이 아닌 컬러를 저장해도 오류가 보고되지 않으며 color 프로퍼티에 문자열 "red"를 할당할 수 있습니다.

기본적으로 지원되는 프로퍼티의 타입 목록은 [QML Value Types](https://doc.qt.io/qt-6/qtqml-typesystem-valuetypes.html)를 보십시오. 게다가 사용 가능한 [QML 객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html)은 프로퍼티 타입으로도 사용할 수 있습니다.

* 특수 프로퍼티 타입

* 객체 리스트 프로퍼티 애트리뷰트

[리스트](https://doc.qt.io/qt-6/qml-list.html) 타입 프로퍼티는 QML 객체-타입 값들의 리스트를 할당할 수 있습니다. 객체 리스트 값을 정의하는 구문은 []로 감싸고 ,로 분리된 리스트입니다:

```qml
[ <item 1>, <item 2>, ... ]
```

예를 들어, [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) 타입은 [states](https://doc.qt.io/qt-6/qml-qtquick-item.html#states-prop) 프로퍼티를 가지고 있는데 이것은 [State](https://doc.qt.io/qt-6/qml-qtquick-state.html) 타입 객체들의 리스트를 저장하는 데 사용합니다. 아래 코드는 3개의 [State](https://doc.qt.io/qt-6/qml-qtquick-state.html) 객체의 리스트에 이 프로퍼티의 값을 초기화합니다:

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

만약 리스트가 1개의 항목을 포함하고 있다면, []은 생략할 수 있습니다:

```qml
import QtQuick 2.0

Item {
    states: State { name: "running" }
}
```

객체 선언할 때 [리스트](https://doc.qt.io/qt-6/qml-list.html) 타입 프로퍼티를 지정하는 구문은 다음과 같습니다:

```qml
[default] property list<<objectType>> propertyName
```

그리고 다른 프로퍼티 선언과 마찬가지로 다음 구문처럼 프로퍼티 초기화는 프로퍼티 선언과 결합될 수 있습니다:

```qml
[default] property list<<objectType>> propertyName: <value>
```

리스트 프로퍼티 선언 예제는 다음과 같습니다:

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

만약 QML 객체-타입 값이 아닌 값 리스트를 저장할 프로퍼티를 선언하려면 [var](https://doc.qt.io/qt-6/qml-var.html) 프로퍼티를 대신 선언해야 합니다.

* 그룹화된 프로퍼티

일부 경우에서는 프로퍼티가 서브-프로퍼티 애트리뷰트의 논리적 그룹을 포함하고 있습니다. 이 서브-프로퍼티 애트리뷰트들은 도트 표기법 또는 그룹 표기법을 사용하여 할당할 수 있습니다.

예를 들어, [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 타입이 [font](https://doc.qt.io/qt-6/qml-qtquick-text.html#font.family-prop) 그룹 프로퍼티를 갖고 있다고 가정합니다. 아래에서 1번째는 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 객체가 도트 표기법으로 font 값을 초기화하고 있습니다. 2번째는 그룹 표기법을 사용하고 있습니다:

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

그룹화된 프로퍼티 타입은 서브-프로퍼티를 갖고 있는 타입입니다. 만약 그룹화된 프로퍼티 타입이 (값 타입이 아닌) 객체 타입이라면, 이 프로퍼티는 읽기-전용이어야 합니다. 이것은 서브-프로퍼티가 속한 객체를 바꾸지 못하게 합니다.

* 프로퍼티 별명(Alias)

프로퍼티 별명은 또 다른 프로퍼티에 대한 참조를 갖고 있는 프로퍼티입니다. 프로퍼티에 대한 새로운 고유의 저장 공간을 할당하는 일반적인 프로퍼티 정의와 달리, 프로퍼티 별명은 (aliasing 프로퍼티라고 하는) 새로 선언된 프로퍼티를 기존 프로퍼티(aliased 프로퍼티)에 직접 참조로 연결합니다.

프로퍼티 별명 선언은 프로퍼티 타입과 달리 alias 키워드가 필요하고 프로퍼티 선언의 오른쪽에 유효한 alias 참조가 있어야 한다는 점만 제외하면 일반적인 프로퍼티 정의와 비슷합니다:

```qml
[default] property alias <name>: <alias reference>
```

일반적인 프로퍼티와 달리 alias는 다음과 같은 제한사항을 가지고 있습니다:

- alias가 선언된 [타입](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html)의 범위 내에 있는 객체, 또는 객체의 프로퍼티만 참조할 수 있습니다.
- 임의의 JavaScript 표현식을 포함할 수 없습니다.
- 해당 타입의 범위 밖에서 선언된 객체를 참조할 수 없습니다.
- 일반적인 프로퍼티의 경우 기본값이 선택적이지만 alias 참조는 선택적이지 않습니다; alias 참조는 alias가 처음 선언될 때 제공되어야 합니다.
- [부착된 프로퍼티](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#attached-properties-and-attached-signal-handlers)를 참조할 수 없습니다.
- 깊이가 3 이상인 계층 내부의 프로퍼티를 참조할 수 없습니다. 다음 코드는 작동하지 않습니다:

```qml
property alias color: myItem.myRect.border.color

Item {
    id: myItem
    property Rectangle myRect
}
```

그러나 최대 2레벨 깊이까지의 프로퍼티에 대한 alias는 작동합니다.

```qml
property alias color: rectangle.border.color

Rectangle {
    id: rectangle
}
```

예를 들어, 다음은 alias 프로퍼티인 buttonText를 가진 Button 타입입니다. buttonText는 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 자식의 text 객체에 연결되어 있습니다:

```qml
// Button.qml
import QtQuick 2.0

Rectangle {
    property alias buttonText: textItem.text

    width: 100; height: 30; color: "yellow"

    Text { id: textItem }
}
```

다음 코드는 자식 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 객체에 대하여 정의된 text 문자열을 가진 Button을 생성할 것입니다:

```qml
Button { buttonText: "Click Me" }
```

여기서 buttonText를 수정하면 textItem.text 값이 직접 바뀝니다; 다른 값을 바꾸지 않고 textItem.text을 업데이트합니다. 만약 buttonText가 별명이 아니었더라면 이 값을 바꾸는 것은 표시되는 텍스트를 실제로 바꾸지 않을 것입니다. 왜냐하면 프로퍼티 바인딩이 쌍방향이 아니기 때문입니다: 만약 textItem.text가 바뀌었다면 buttonText도 바뀌겠지만 그 반대로는 변경되지 않습니다.

* 프로퍼티 별명에 대해 고려할 사항

별명은 일단 컴포넌트가 완전히 초기화되었을 때에만 활성화됩니다. 초기화되지 않은 별명을 참조하면 오류가 발생합니다. 마찬가지로 aliasing 프로퍼티를 aliasing하는 것도 오류가 발생합니다.

```qml
property alias widgetLabel: label

//오류가 발생함
//widgetLabel.text: "Initial text"

//오류가 발생함
//property alias widgetLabelText: widgetLabel.text

Component.onCompleted: widgetLabel.text = "Alias completed Initialization"
```

그러나 객체 객체에 프로퍼티 별명을 갖고 있는 [QML 객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html)을 가져올 때에는 프로퍼티가 일반 Qt 프로퍼티로 나타나므로 별명 참조에 사용할 수 있습니다.

aliasing 프로퍼티가 기존 프로퍼티와 같은 이름을 갖는 것이 가능하므로 사실상 기존 프로퍼티를 덮어쓸 수 있습니다. 예를 들어, 다음 QML type은 내장된 [Rectangle::color](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html#color-prop) 프로퍼티와 동일한 이름을 가진 color 별명 프로퍼티를 갖고 있습니다:

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

이 타입을 사용하고 color 프로퍼티를 참조하는 모든 객체는 일반적인 [Rectangle::color](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html#color-prop) 프로퍼티가 아닌 별명을 참조할 것입니다. 그러나 내부적으로 Rectangle은 color 프로퍼티를 올바르게 설정하고 별명이 아닌 실제 정의된 프로퍼티를 참조할 수 있습니다.

* 프로퍼티 별명과 타입

프로퍼티 별명은 명시적인 타입 사양을 가질 수 없습니다. 프로퍼티 별명의 타입은 이것이 참조하는 프로퍼티 또는 객체의 선언된 타입입니다. 그러므로 id와 인라인으로 선언된 추가 프로퍼티들을 통해 참조된 객체에 대한 별명을 생성하면, alias를 통해 추가 프로퍼티에 접근할 수 없습니다:

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

inner는 단지 항목일 뿐이기 때문에 당신은 이 컴포넌트의 밖에서 inner.extraProperty를 초기화할 수 없습니다:

```qml
// main.qml
MyItem {
    inner.extraProperty: 5 // 실패
}
```

그러나 전용 .qml 파일을 사용하여 inner 객체를 별도의 컴포넌트로 추출하면, 당신은 그 컴포넌트를 인스턴스화 할 수 있고 별명를 통해 그것의 모든 프로퍼티에 접근할 수 있습니다:

```qml
// MainItem.qml
Item {
    // 이제 당신은 inner.extraProperty에 접근할 수 있습니다. 왜냐하면 inner는 이제 ExtraItem이기 때문입니다.
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

* 기본 프로퍼티

객체 정의는 하나의 기본 프로퍼티를 가질 수 있습니다. 기본 프로퍼티란 특정 프로퍼티에 대한 값을 선언하지 않고 또 다른 객체의 정의 안에서 어떤 객체를 선언했을 때 값이 할당되는 프로퍼티입니다.

선택적인 default 키워드를 사용하여 프로퍼티를 선언하면 기본 프로퍼티로 표시됩니다. 예를 들어, 기본 프로퍼티 someText를 가진 MyLabel.qml이 있다고 가정합니다:

```qml
// MyLabel.qml
import QtQuick 2.0

Text {
    default property var someText

    text: "Hello, " + someText.text
}
```

다음과 같이 someText 값은 MyLabel 객체 정의에 할당될 수 있습니다:

```qml
MyLabel {
    Text { text: "world!" }
}
```

다음은 정확하게 같은 효과가 있습니다:

```qml
MyLabel {
    someText: Text { text: "world!" }
}
```

그러나 someText 프로퍼티가 기본 프로퍼티로 표시되었으므로 이 프로퍼티에 명시적으로 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 객체를 할당할 필요는 없습니다.

[자식](https://doc.qt.io/qt-6/qml-qtquick-item.html#children-prop) 프로퍼티에 명시적으로 추가하지 않고도 자식 객체를 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html)-기반 타입에 추가할 수 있습니다. 이는 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html)의 기본 프로퍼티가 그것의 데이터 프로퍼티이며 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html)에 대해 이 리스트에 추가되는 모든 항목은 [자식](https://doc.qt.io/qt-6/qml-qtquick-item.html#children-prop)의 리스트에 자동으로 추가되기 때문입니다.

기본 프로퍼티는 항목의 자식을 재할당하는 데 유용합니다. 예를 들면:

```qml
Item {
    default property alias content: inner.children

    Item {
        id: inner
     }
}
```

기본 프로퍼티 별명을 inner.children으로 설정하면 외부 항목의 자식으로 할당된 모든 객체는 자동으로 내부 항목의 자식으로 재할당됩니다.

* 필수 프로퍼티

required 키워드를 사용하면 객체 선언을 할 때 어떤 프로퍼티를 필수로 정의할 수도 있습니다. 구문은 다음과 같습니다.

```qml
required property <propertyType> <propertyName>
```

이름에서 알 수 있듯이 객체의 인스턴스를 생성할 때 필수 프로퍼티는 반드시 설정해야 합니다. 이 규칙을 위반하면 정적으로 탐지될 경우 QML 앱은 작동하지 않게 됩니다. 동적으로 인스턴스화한 QML 컴포넌트([Qt.createComponent](https://doc.qt.io/qt-6/qml-qtqml-qt.html#createComponent-method)()를 통한 인스턴스)의 경우, 이 규칙을 위반하면 경고와 null 리턴 값이 나오게 됩니다.

다음과 같이 기존의 프로퍼티를 required로 만들 수 있습니다.

```qml
required <propertyName>
```

다음 예제는 커스텀 Rectangle 컴포넌트를 만드는 방법을 보여줍니다. 여기서 color 프로퍼티는 항상 지정해야 합니다.

```qml
// ColorRectangle.qml
Rectangle {
    required color
}
```

주의: QML로부터 필수 프로퍼티에 초기값을 할당할 수 없습니다. 왜냐하면 필수 프로퍼티의 사용 의도에 직접적으로 위배되기 때문입니다.

필수 프로퍼티는 model-view-delegate 코드에서 특수한 역할을 하게 됩니다: 만약 view의 delegate가 view의 model의 역할 이름과 같은 필수 프로퍼티를 가지고 있다면, 이 프로퍼티는 model의 해당 값으로 초기될 것입니다. 더 많은 정보는 [Models and Views in Qt Quick](https://doc.qt.io/qt-6/qtquick-modelviewsdata-modelview.html) 페이지를 보십시오.

C++로부터 필수 프로퍼티를 초기화하는 방법에 대해서는 [QQmlComponent::createWithInitialProperties](https://doc.qt.io/qt-6/qqmlcomponent.html#createWithInitialProperties), [QQmlApplicationEngine::setInitialProperties](https://doc.qt.io/qt-6/qqmlapplicationengine.html#setInitialProperties), [QQuickView::setInitialProperties](https://doc.qt.io/qt-6/qquickview.html#setInitialProperties)를 보십시오.

* 읽기-전용 프로퍼티

객체 선언을 할 때 readonly 키워드를 사용하여 읽기-전용 프로퍼티를 정의할 수 있습니다. 구문은 다음과 같습니다:

```qml
readonly property <propertyType> <propertyName> : <value>
```

읽기-전용 프로퍼티는 반드시 초기화 할 때 정적인 값 또는 바인딕 표현식을 할당해야 합니다. 일단 읽기-전용 프로퍼티가 초기화된 후에는 더 이상 변경할 수 없습니다.

예를 들어, 아래의 Component.onCompleted 블럭에 있는 코드는 유효하지 않습니다:

```qml
Item {
    readonly property int someNumber: 10

    Component.onCompleted: someNumber = 20  // TypeError: 읽기-전용 프로퍼티에 할당할 수 없음
}
```

주의: 읽기-전용 프로퍼티는 [기본](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#default-properties) 프로퍼티가 될 수 없습니다.

* 프로퍼티 Modifier 객체

프로퍼티는 그것과 연관된 [프로퍼티 값 modifier 객체](https://doc.qt.io/qt-6/qtqml-cppintegration-definetypes.html#property-modifier-types)를 가질 수 있습니다. 특정 프로퍼티와 연관된 프로퍼티 modifier 타입의 인스턴스를 선언하는 구문은 다음과 같습니다:

```qml
<PropertyModifierTypeName> on <propertyName> {
    // 객체 인스턴스의 애트리뷰트
}
```

이것을 일반적으로 "on" 구문이라고 합니다.

위의 구문은 실제로 기존 프로퍼티에서 작동하는 객체를 인스턴스화하는 [객체 선언](https://doc.qt.io/qt-6/qtqml-syntax-basics.html#object-declarations)입니다.

어떤 프로퍼티 modifier 타입은 특정 프로퍼티 타입에만 적용할 수 있지만 언어에 의해 강요되지는 않습니다. 예를 들어, QtQuick이 제공하는 NumberAnimation 타입은 (int, real 같은) 숫자-타입 프로퍼티만 애니메이트 합니다. 숫자가 아닌 프로퍼티와 함께 NumberAnimation을 사용하려고 하면 오류는 발생하지 않지만 숫자가 아닌 프로퍼티는 애니메이트 되지 않을 것입니다. 특정 프로퍼티 타입과 연관된 경우 프로퍼티 modifier 타입의 동작은 구현에 의해 정의됩니다.

##### 시그널 애트리뷰트

시그널은 어떤 이벤트가 발생했을 때 객체로부터 나오는 공지(notification)입니다: 예를 들면 어떤 프로퍼티가 변경되었을 때, 애니메이션이 시작했거나 끝났을 때, 또는 이미지를 다운로드 했을 때 같은 경우입니다. 예를 들면, [MouseArea](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html) 타입은 사용자가 마우스 영역 안을 클릭했을 때 방출(emit)되는 [clicked](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html#clicked-signal) 시그널을 갖고 있습니다.

특정 시그널이 방출될 때 [시그널 핸들러](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-handler-attributes)를 통해 공지를 받을 수 있습니다. 시그널 핸들러는 구문 on<Signal>으로 선언할 수 있습니다. 여기서 <Signal>은 signal의 이름이며 1번째 글자는 대문자입니다. 시그널 핸들러는 시그널을 방출하는 객체 정의 안에서 선언해야 하며, 핸들러는 시그널 핸들러가 호출될 때 실행될 JavaScript 코드의 블럭을 포함해야 합니다.

예를 들어, 아래의 onClicked 시그널 핸들러는 [MouseArea](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html) 객체 정의 안에서 선언되며, [MouseArea](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html)를 클릭했을 때 호출되어 콘솔 메시지가 프린트됩니다.

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

* 시그널 애트리뷰트 정의하기

클래스의 [Q_SIGNAL](https://doc.qt.io/qt-6/qobject.html#Q_SIGNAL)을 등록하고 QML 타입 시스템에 등록함으로써 C++에서 타입에 대한 시그널을 정의할 수 있습니다. 아니면 다음과 같은 구문처럼 객체 타입에 대한 커스텀 시그널은 QML 도큐먼트의 객체 선언에서 정의할 수도 있습니다.

```qml
signal <signalName>[([<parameterName>: <parameterType>[, ...]])]
```

동일한 타입 블럭에서 같은 이름을 가진 2개의 시그널 또는 메서드를 선언하려고 하는 것은 오류입니다. 그러나 새로운 시그널은 타입에 있는 기존 시그널의 이름을 재사용할 수 있습니다. (이렇게 하면 기존 시그널은 숨겨져서 접근할 수 없게 되므로 주의해서 수행해야 합니다)

다음은 signal 선언에 대한 3가지 예제입니다:

```qml
import QtQuick 2.0

Item {
    signal clicked
    signal hovered()
    signal actionPerformed(action: string, actionResult: int)
}
```

또한 프로퍼티 스타일 구문으로 시그널 파라미터를 지정할 수도 있습니다:

```qml
signal actionCanceled(string action)
```

메서드 선언과 일치하려면, 콜론(:)을 사용한 타입 선언을 사용해야 합니다.

만약 시그널이 파라미터가 없을 경우, ()는 선택사항입니다. 만약 파라미터를 사용한다면 위의 actionPerformed 시그널에 대한 string과 var 인수와 같이 파라미터 타입을 반드시 선언해야 합니다. 허용된 파라미터 타입은 이 페이지에 있는 [프로퍼티 애트리뷰트 정의하기](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#defining-property-attributes)에 나열되어 있는 것과 같습니다.

시그널을 방출(emit)하려면 메서드로 호출해야 합니다. 시그널을 방출할 때 연관된 [시그널 핸들러](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-handler-attributes)가 호출되고 핸들러는 정의된 시그널 인수 이름을 사용하여 각 인수에 접근할 수 있습니다.

* 프로퍼티 변경 시그널

QML 타입은 [프로퍼티 애트리뷰트](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#property-attributes)에 대한 섹션에서 설명했던 것처럼 프로퍼티 값이 바뀔 때마다 방출되는 내장 프로퍼티 변경 시그널도 제공합니다. 이러한 시그널이 왜 유용한지, 그리고 어떻게 사용하는지에 대한 정보는 다음에 나오는 [프로퍼티 변경 시그널 핸들러](https://doc.qt.io/qt-6/qtqml-syntax-signals.html#property-change-signal-handlers)을 보십시오.

* 시그널 핸들러 애트리뷰트

시그널 핸들러는 특수한 종류의 [메서드 애트리뷰트](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#method-attributes)입니다. 여기서 메서드 구현은 연관된 시그널이 방출될 때마다 QML 엔진에 의해 호출됩니다. QML에서 객체 정의에 시그널을 추가하면 연관된 시그널 핸들러가 객체 정의에 자동으로 추가되며, 이는 기본적으로 빈 구현을 가지고 있습니다. 클라이언트는 프로그램 로직을 구현하기 위해 구현을 제공할 수 있습니다.

다음과 같은 SquareButton.qml 파일에서 시그널이 활성화 및 비활성화된 상태로 정의가 제공되는 SquareButton 타입을 생각해 보십시오:

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

이러한 시그널은 동일한 디렉토리에 있는 다른 QML 파일의 SquareButton 객체에서 수신할 수 있습니다. 여기서 시그널 핸들러에 대한 구현은 클라이언트에 의해 제공됩니다:

```qml
// myapplication.qml
SquareButton {
    onDeactivated: console.log("Deactivated!")
    onActivated: (xPosition, yPosition)=> console.log("Activated at " + xPosition + "," + yPosition)
}
```

시그널이 이미 파라미터 타입을 지정하고 있으므로 시그널 핸들러는 파라미터 타입을 선언할 필요가 없습니다. 위에 표시된 화살표 함수 구문은 타입 표기법을 지원하지 않습니다.

시그널 사용에 대한 자세한 내용은 [Signal and Handler Event System](https://doc.qt.io/qt-6/qtqml-syntax-signals.html)을 보십시오.

* 프로퍼티 변경 시그널 핸들러

프로퍼티 변경 시그널에 대한 시그널 핸들러는 on<Property>Changed 형태의 구문을 갖습니다. 여기서 <Property>는 프로퍼티의 이름이며 첫 글자는 대문자입니다. 예를 들면, [TextInput](https://doc.qt.io/qt-6/qml-qtquick-textinput.html) type 도큐먼트는 TextChanged 시그널을 문서화하지 않지만 [TextInput](https://doc.qt.io/qt-6/qml-qtquick-textinput.html)이 [text](https://doc.qt.io/qt-6/qml-qtquick-textinput.html#text-prop) 프로퍼티를 가지고 있고 이 프로퍼티가 변경될 때마다 호출되는 OnTextChanged 시그널 핸들러를 작성할 수 있으므로 이 시그널은 암묵적으로 이용 가능합니다:

```qml
import QtQuick 2.0

TextInput {
    text: "Change this!"

    onTextChanged: console.log("Text has changed to:", text)
}
```

##### 메서드 애트리뷰트

객체 타입의 메서드는 연산 처리 혹은 트리거 이벤트 등을 수행하기 위해 호출될 수 있는 함수입니다. 메서드는 시그널과 연결될 수 있기 때문에 시그널이 방출될 때마다 자동으로 호출됩니다. 자세한 것은 [Signal and Handler Event System](https://doc.qt.io/qt-6/qtqml-syntax-signals.html)을 보십시오.

* 메서드 애트리뷰트 정의하기

C++에서 클래스의 함수에 [Q_INVOKABLE](https://doc.qt.io/qt-6/qobject.html#Q_INVOKABLE)을 태깅하여 QML 타입 시스템에 등록하거나, 클래스의 [Q_SLOT](https://doc.qt.io/qt-6/qobject.html#Q_SLOT)에 등록하여 메서드를 타입으로 정의할 수 있습니다. 아니면 다음과 같은 구문으로 QML 도큐먼트에서 커스텀 메서드를 객체 선언에 추가할 수도 있습니다:

```qml
function <functionName>([<parameterName>[: <parameterType>][, ...]]) [: <returnType>] { <body> }
```

독립형, 재활용 가능한 JavaScript 코드 블럭을 정의하기 위해 메서드를 QML 타입에 추가할 수 있습니다. 이 메서드는 내부 혹은 외부 객체에서 호출할 수 있습니다.

시그널과 달리, 메서드 파라미터 타입은 기본적으로 var 타입으로 선언할 필요가 없습니다. 그러나 qmlcachegen이 고성능 코드를 생성하고 유지보수성을 향상시키기 위해 var 타입으로 선언하는 것이 좋습니다.

동일한 타입 블럭에서 같은 이름으로 2개의 메서드 혹은 시그널을 선언하려고 하면 오류가 발생합니다. 그러나 새로운 메서드가 타입에 있는 기존 메서드의 이름을 재활용할 수 있습니다. (이렇게 하면 기존 메서드는 숨겨지고 접근할 수 없게 되므로 주의해서 수행해야 합니다)

다음은 높이 값을 할당할 때 호출되는 calculateHeight() 메서드를 가진 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)입니다:

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

만약 메서드가 파라미터를 갖고 있다면, 메서드 안에서 이름으로 파라미터에 접근할 수 있습니다. 아래는 [MouseArea](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html)를 클릭했을 때 moveTo() 메서드가 호출되는 예제입니다. moveTo() 메서드는 text의 위치를 조정하기 위해 수신한 newX와 newY 파라미터를 참조합니다:

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

##### 부착된 프로퍼티와 부착된 시그널 핸들러

부착된 프로퍼티와 부착된 시그널 핸들러는 객체에 추가 프로퍼티와 시그널 핸들러로 주석을 달 수 있도록 해주는 메커니즘입니다. 특히 객체가 개별 객체와 특별히 관련된 프로퍼티 또는 시그널에 접근할 수 있도록 해줍니다.

QML 타입 구현은 특별한 프로퍼티와 시그널을 [C++에서 부착 타입을 생성](https://doc.qt.io/qt-6/qtqml-cppintegration-definetypes.html#providing-attached-properties)하도록 선택할 수 있습니다. 이러한 타입의 인스턴스는 런타임 시에 생성되고 특정 객체에 부착되어, 객체들이 부착 타입의 프로퍼티와 시그널에 접근할 수 있게 해줍니다. 부착 타입의 이름을 프로퍼티와 각 시그널 핸들러 앞에 접두사로 붙이면 접근할 수 있습니다.

부착된 프로퍼티와 핸들러에 대한 참조는 다음 구문 형태를 따릅니다:

```qml
<AttachingType>.<propertyName>
<AttachingType>.on<SignalName>
```

예를 들면, [ListView](https://doc.qt.io/qt-6/qml-qtquick-listview.html) 타입은 [ListView](https://doc.qt.io/qt-6/qml-qtquick-listview.html)의 각 위임(delegate) 객체에서 사용할 수 있는 프로퍼티 [ListView.isCurrentItem](https://doc.qt.io/qt-6/qml-qtquick-listview.html#isCurrentItem-attached-prop)을 갖고 있습니다. 이 프로퍼티는 각 위임 객체가 view에서 현재 선택한 항목인지 여부를 확인하는 데 사용할 수 있습니다:

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

이 경우, 부착 타입의 이름은 ListView이고 문제의 프로퍼티는 isCurrentItem이므로 부착된 프로퍼티를 ListView.isCurrentItem이라고 합니다.

부착된 시그널 핸들러도 같은 방식으로 언급됩니다. 예를 들면, [Component.onCompleted](https://doc.qt.io/qt-6/qml-qtqml-component.html#completed-signal) 부착된 시그널 핸들러는 일반적으로 컴포넌트의 생성 프로세스가 완료되었을 때 일부 JavaScript 코드를 실행하는 데 사용됩니다. 아래의 예제에서는 [ListModel](https://doc.qt.io/qt-6/qml-qtqml-models-listmodel.html)이 완전히 생성되면 해당 Component.onCompleted 시그널 핸들러가 자동으로 호출되어 모델을 채웁니다:

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

부착 타입의 이름이 Component이고 해당 타입이 [completed](https://doc.qt.io/qt-6/qml-qtqml-component.html#completed-signal) 시그널을 가지므로 부착된 시그널 핸들러를 Component.onCompleted라고 합니다.

* 부착된 프로퍼티와 시그널 핸들러에 접근할 때 유의사항

일반적인 오류는 이러한 애트리뷰트가 부착된 객체의 자식으로부터 부착된 프로퍼티와 시그널 핸들러에 직접 접근할 수 있다고 가정하는 것입니다. 이는 해당되지 않습니다. 부착 타입의 인스턴스는 특정 객체에만 부착되고 객체와 모든 객체의 자식에는 연결되지 않습니다.

예를 들면, 다음은 부착된 프로퍼티를 포함하는 이전 예제의 수정된 버전입니다. 이번에는 위임자가 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html)이고 색상이 지정된 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)은 해당 Item의 자식입니다:

```qml
import QtQuick 2.0

ListView {
    width: 240; height: 320
    model: 3
    delegate: Item {
        width: 100; height: 30

        Rectangle {
            width: 100; height: 30
            color: ListView.isCurrentItem ? "red" : "yellow"    // 잘못됨! 이것은 작동하지 않습니다.
        }
    }
}
```

ListView.isCurrentItem은 루트 위임자 객체에만 부착되어 있고 하위 객체에는 부착되어 있지 않기 때문에 예상대로 작동하지 않습니다. [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)은 위임자 자체가 아닌 위임자의 자식 객체이므로 isCurrentItem 부착 프로퍼티에 ListView.isCurrentItem으로 접근할 수 없습니다. 따라서 대신 Rectangle은 루트 위임자를 통해 isCurrentItem에 접근해야 합니다:

```qml
ListView {
    //....
    delegate: Item {
        id: delegateItem
        width: 100; height: 30

        Rectangle {
            width: 100; height: 30
            color: delegateItem.ListView.isCurrentItem ? "red" : "yellow"   // 올바름
        }
    }
}
```

이제 delegateItem.ListView.isCurrentItem은 위임자의 isCurrentItem 부착 프로퍼티를 올바르게 참조합니다.

##### 열거형 애트리뷰트

열거형은 명명된 선택의 고정 집합을 제공합니다. 이러한 선택은 enum 키워드를 사용하여 QML에서 선언할 수 있습니다:

```qml
// MyText.qml
Text {
    enum TextType {
        Normal,
        Heading
    }
}
```

위와 같이 열거형 타입(예: TextType)과 값(예: Normal)은 대문자로 시작해야 합니다.

값은 <Type>.<EnumerationType>.<Value> 또는 <Type>.<Value>을 통해 참조해야 합니다.

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

QML에서 열거형 사용법에 대한 정보는 [QML 값 타입](https://doc.qt.io/qt-6/qtqml-typesystem-valuetypes.html), [열거형](https://doc.qt.io/qt-6/qml-enumeration.html) 문서를 보십시오.

QML에서 열거형을 선언할 수 있는 기능은 Qt 5.10에서 도입되었습니다.


#### 프로퍼티 바인딩

객체의 프로퍼티에는 정적인 값을 할당할 수 있습니다. 그러나 QML과 동적 객체 동작에 대한 내장 지원을 최대한 활용하기 위해 대부분의 QML 객체들은 프로퍼티 바인딩을 사용합니다.

프로퍼티 바인딩은 개발자가 서로 다른 객체 프로퍼티 간의 관계를 지정할 수 있도록 하는 QML의 핵심 기능입니다. 값에서 프로퍼티의 종속성이 변경되면 지정된 관계에 따라 프로퍼티가 자동으로 업데이트됩니다.

QML 엔진은 프로퍼티의 종속성(즉, 바인딩 표현식의 변수)을 배후에서 감시합니다. 변경사항이 감지되면 QML 엔진은 바인딩 표현식을 재연산하고 새로운 결과를 프로퍼티에 적용합니다.

* 개요

프로퍼티 바인딩을 만들기 위해 프로퍼티에 원하는 값으로 연산하는 JavaScript 표현식을 할당합니다. 가장 간단한 경우 바인딩은 다른 프로퍼티에 대한 참조가 될 수 있습니다. 다음 예제는 파란식 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)의 높이가 부모의 높이로 제한되어 있는 경우입니다:

```qml
Rectangle {
    width: 200; height: 200

    Rectangle {
        width: 100
        height: parent.height
        color: "blue"
    }
}
```

부모 사각형의 높이가 변경될 때마다 파란색 사각형의 높이가 자동으로 동일한 값으로 업데이트됩니다.

QML이 표준을 준수하는 JavaScript 엔진을 사용하기 때문에 바인딩에는 모든 유효한 JavaScript 표현식 또는 문장이 포함될 수 있습니다. 바인딩은 객체 프로퍼티, 호출 메서드에 접근하고 Date 및 Math와 같은 내장 JavaScript 객체를 사용할 수 있습니다. 다음은 이전 예제에 대한 또 다른 바인딩입니다:

```qml
height: parent.height / 2

height: Math.min(parent.width, parent.height)

height: parent.height > 100 ? parent.height : parent.height/2

height: {
    if (parent.height > 100)
        return parent.height
    else
        return parent.height / 2
}

height: someMethodThatReturnsHeight()
```

다음은 더 많은 객체와 타입을 포함하는 더 복잡한 예제입니다:

```qml
Column {
    id: column
    width: 200
    height: 200

    Rectangle {
        id: topRect
        width: Math.max(bottomRect.width, parent.width/2)
        height: (parent.height / 3) + 10
        color: "yellow"

        TextInput {
            id: myTextInput
            text: "Hello QML!"
        }
    }

    Rectangle {
        id: bottomRect
        width: 100
        height: 50
        color: myTextInput.text.length <= 10 ? "red" : "blue"
    }
}
```

이전 예제에서는,

- topRect.width가 bottomRect.width와 column.width를 의존함
- topRect.height가 column.height를 의존함
- bottomRect.color가 myTextInput.text.length를 의존함

Syntactically, bindings are allowed to be of arbitrary complexity. However, if a binding is overly complex - such as involving multiple lines, or imperative loops - it could indicate that the binding is being used for more than describing property relationships. Complex bindings can reduce code performance, readability, and maintainability. It may be a good idea to redesign components that have complex bindings, or at least factor the binding out into a separate function. As a general rule, users should not rely on the evaluation order of bindings.

* JavaScript로부터 프로퍼티 바인딩 생성하기

A property with a binding is automatically updated as necessary. However, if the property is later assigned a static value from a JavaScript statement, the binding will be removed.

For example, the Rectangle below initially ensures that its height is always twice its width. However, when the space key is pressed, the current value of width*3 will be assigned to height as a static value. After that, the height will remain fixed at this value, even if the width changes. The assignment of the static value removes the binding.

```qml
import QtQuick 2.0

Rectangle {
    width: 100
    height: width * 2

    focus: true
    Keys.onSpacePressed: {
        height = width * 3
    }
}
```

If the intention is to give the rectangle a fixed height and stop automatic updates, then this is not a problem. However, if the intention is to establish a new relationship between width and height, then the new binding expression must be wrapped in the Qt.binding() function instead:

```qml
import QtQuick 2.0

Rectangle {
    width: 100
    height: width * 2

    focus: true
    Keys.onSpacePressed: {
        height = Qt.binding(function() { return width * 3 })
    }
}
```

Now, after the space key is pressed, the rectangle's height will continue auto-updating to always be three times its width.

* 바인딩의 덮어쓰기 디버그하기

A common cause of bugs in QML applications is accidentally overwriting bindings with static values from JavaScript statements. To help developers track down problems of this kind, the QML engine is able to emit messages whenever a binding is lost due to imperative assignments.

In order to generate such messages, you need to enable the informational output for the qt.qml.binding.removal logging category, for instance by calling:

```qml
QLoggingCategory::setFilterRules(QStringLiteral("qt.qml.binding.removal.info=true"));
```

Please refer to the QLoggingCategory documentation for more information about enabling output from logging categories.

Note that is perfectly reasonable in some circumstances to overwrite bindings. Any message generated by the QML engine should be treated as a diagnostic aid, and not necessarily as evidence of a problem without further investigation.

* 프로퍼티 바인딩으로 this 사용하기

When creating a property binding from JavaScript, the this keyword can be used to refer to the object which receives the binding. This is helpful for resolving ambiguities with property names.

For example, the Component.onCompleted handler below is defined within the scope of the Item. In this scope, width refers to the Item's width, not the Rectangle's width. To bind the Rectangle's height to its own width, the binding expression must explicitly refer to this.width (or alternatively, rect.width):

```qml
Item {
    width: 500
    height: 500

    Rectangle {
        id: rect
        width: 100
        color: "yellow"
    }

    Component.onCompleted: {
        rect.height = Qt.binding(function() { return this.width * 2 })
        console.log("rect.height = " + rect.height) // prints 200, not 1000
    }
}
```

주의: this의 값은 프로퍼티 바인딩의 외부에서 정의되지 않습니다. 자세한 것은 [JavaScript 환경 제한](https://doc.qt.io/qt-6/qtqml-javascript-hostenvironment.html#javascript-environment-restrictions)을 보십시오.


#### 시그널과 핸들러 이벤트 시스템

Application and user interface components need to communicate with each other. For example, a button needs to know that the user has clicked on it. The button may change colors to indicate its state or perform some logic. As well, application needs to know whether the user is clicking the button. The application may need to relay this clicking event to other applications.

QML has a signal and handler mechanism, where the signal is the event and the signal is responded to through a signal handler. When a signal is emitted, the corresponding signal handler is invoked. Placing logic such as a script or other operations in the handler allows the component to respond to the event.

* Receiving signals with signal handlers

To receive a notification when a particular signal is emitted for a particular object, the object definition should declare a signal handler named on<Signal>, where <Signal> is the name of the signal, with the first letter capitalized. The signal handler should contain the JavaScript code to be executed when the signal handler is invoked.

For example, the Button type from the Qt Quick Controls module has a clicked signal, which is emitted whenever the button is clicked. In this case, the signal handler for receiving this signal should be onClicked. In the example below, whenever the button is clicked, the onClicked handler is invoked, applying a random color to the parent Rectangle:

```qml
import QtQuick
import QtQuick.Controls

Rectangle {
    id: rect
    width: 250; height: 250

    Button {
        anchors.bottom: parent.bottom
        anchors.horizontalCenter: parent.horizontalCenter
        text: "Change color!"
        onClicked: {
            rect.color = Qt.rgba(Math.random(), Math.random(), Math.random(), 1);
        }
    }
}
```

* Property change signal handlers

A signal is automatically emitted when the value of a QML property changes. This type of signal is a property change signal and signal handlers for these signals are written in the form on<Property>Changed, where <Property> is the name of the property, with the first letter capitalized.

For example, the MouseArea type has a pressed property. To receive a notification whenever this property changes, write a signal handler named onPressedChanged:

```qml
import QtQuick

Rectangle {
    id: rect
    width: 100; height: 100

    TapHandler {
        onPressedChanged: console.log("taphandler pressed?", pressed)
    }
}
```

Even though the TapHandler documentation does not document a signal handler named onPressedChanged, the signal is implicitly provided by the fact that the pressed property exists.

* Signal parameters

Signals might have parameters. To access those, you should assign a function to the handler. Both arrow functions and anonymous functions work.

For the following examples, consider a Status component with an errorOccurred signal (see Adding signals to custom QML types for more information about how signals can be added to QML components).

```qml
// Status.qml
import QtQuick

Item {
    id: myitem
    signal errorOccurred(message: string, line: int, column: int)
}
```

```qml
Status {
    onErrorOccurred: (mgs, line, col) => console.log(`${line}:${col}: ${msg}`)
}
```

Note: The names of the formal parameters in the function do not have to match those in the signal.

If you do not need to handle all parameters, it is possible to omit trailing ones:

```qml
Status {
    onErrorOccurred: function (message) { console.log(message) }
}
```

It is not possible to leave out leading parameters you are interested in, however you can use some placeholder name to indicate to readers that they are not important:

```qml
Status {
    onErrorOccurred: (_, _, col) => console.log(`Error happened at column ${col}`)
}
```

Note: Instead of using a function, it is possible, but discouraged, to use a plain code block. In that case all signal parameters get injected into the scope of the block. However, this can make code difficult to read as it's unclear where the parameters come from, and results in slower lookups in the QML engine. Injecting parameters in this way is deprecated, and will cause runtime warnings if the parameter is actually used.

* Using the Connections type

In some cases it may be desirable to access a signal outside of the object that emits it. For these purposes, the QtQuick module provides the Connections type for connecting to signals of arbitrary objects. A Connections object can receive any signal from its specified target.

For example, the onClicked handler in the earlier example could have been received by the root Rectangle instead, by placing the onClicked handler in a Connections object that has its target set to the button:

```qml
import QtQuick
import QtQuick.Controls

Rectangle {
    id: rect
    width: 250; height: 250

    Button {
        id: button
        anchors.bottom: parent.bottom
        anchors.horizontalCenter: parent.horizontalCenter
        text: "Change color!"
    }

    Connections {
        target: button
        function onClicked() {
            rect.color = Qt.rgba(Math.random(), Math.random(), Math.random(), 1);
        }
    }
}
```

* Attached signal handlers

An attached signal handler receives a signal from an attaching type rather than the object within which the handler is declared.

For example, Component.onCompleted is an attached signal handler. It is often used to execute some JavaScript code when its creation process is complete. Here is an example:

```qml
import QtQuick

Rectangle {
    width: 200; height: 200
    color: Qt.rgba(Qt.random(), Qt.random(), Qt.random(), 1)

    Component.onCompleted: {
        console.log("The rectangle's color is", color)
    }
}
```

The onCompleted handler is not responding to a completed signal from the Rectangle type. Instead, an object of the Component attaching type with a completed signal has automatically been attached to the Rectangle object by the QML engine. The engine emits this signal when the Rectangle object is created, thus triggering the Component.onCompleted signal handler.

Attached signal handlers allow objects to be notified of particular signals that are significant to each individual object. If there was no Component.onCompleted attached signal handler, for example, an object could not receive this notification without registering for some special signal from some special object. The attached signal handler mechanism enables objects to receive particular signals without extra code.

See Attached properties and attached signal handlers for more information on attached signal handlers.

* Adding signals to custom QML types

Signals can be added to custom QML types through the signal keyword.

The syntax for defining a new signal is:

```qml
signal <name>[([<type> <parameter name>[, ...]])]
```

A signal is emitted by invoking the signal as a method.

For example, the code below is defined in a file named SquareButton.qml. The root Rectangle object has an activated signal, which is emitted whenever the child TapHandler is tapped. In this particular example the activated signal is emitted with the x and y coordinates of the mouse click:

```qml
// SquareButton.qml
import QtQuick

Rectangle {
    id: root

    signal activated(real xPosition, real yPosition)
    property point mouseXY
    property int side: 100
    width: side; height: side

    TapHandler {
        id: handler
        onTapped: root.activated(root.mouseXY.x, root.mouseXY.y)
        onPressedChanged: root.mouseXY = handler.point.position
    }
}
```

Now any objects of the SquareButton can connect to the activated signal using an onActivated signal handler:

```qml
// myapplication.qml
SquareButton {
    onActivated: (xPosition, yPosition)=> console.log("Activated at " + xPosition + "," + yPosition)
}
```

See Signal Attributes for more details on writing signals for custom QML types.

* Connecting signals to methods and signals

Signal objects have a connect() method to a connect a signal either to a method or another signal. When a signal is connected to a method, the method is automatically invoked whenever the signal is emitted. This mechanism enables a signal to be received by a method instead of a signal handler.

Below, the messageReceived signal is connected to three methods using the connect() method:

```qml
import QtQuick

Rectangle {
    id: relay

    signal messageReceived(string person, string notice)

    Component.onCompleted: {
        relay.messageReceived.connect(sendToPost)
        relay.messageReceived.connect(sendToTelegraph)
        relay.messageReceived.connect(sendToEmail)
        relay.messageReceived("Tom", "Happy Birthday")
    }

    function sendToPost(person, notice) {
        console.log("Sending to post: " + person + ", " + notice)
    }
    function sendToTelegraph(person, notice) {
        console.log("Sending to telegraph: " + person + ", " + notice)
    }
    function sendToEmail(person, notice) {
        console.log("Sending to email: " + person + ", " + notice)
    }
}
```

In many cases it is sufficient to receive signals through signal handlers rather than using the connect() function. However, using the connect method allows a signal to be received by multiple methods as shown earlier, which would not be possible with signal handlers as they must be uniquely named. Also, the connect method is useful when connecting signals to dynamically created objects.

There is a corresponding disconnect() method for removing connected signals:

```qml
Rectangle {
    id: relay
    //...

    function removeTelegraphSignal() {
        relay.messageReceived.disconnect(sendToTelegraph)
    }
}
```

* Signal to signal connect

By connecting signals to other signals, the connect() method can form different signal chains.

```qml
import QtQuick

Rectangle {
    id: forwarder
    width: 100; height: 100

    signal send()
    onSend: console.log("Send clicked")

    TapHandler {
        id: mousearea
        anchors.fill: parent
        onTapped: console.log("Mouse clicked")
    }

    Component.onCompleted: {
        mousearea.tapped.connect(send)
    }
}
```

Whenever the TapHandler's tapped signal is emitted, the send signal will automatically be emitted as well.

```qml
output:
    MouseArea clicked
    Send clicked
```


#### QML과 JavaScript 통합하기


#### QML 타입 시스템


#### QML 모듈


#### QML 도큐먼트
