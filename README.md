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
    * [QML와 함께 JavaScript 표현식 사용하기](#qml와-함께-javascript-표현식-사용하기)
    * [JavaScript에서 동적 QML 객체 생성](#javascript에서-동적-qml-객체-생성)
    * [QML에서 JavaScript 리소스 정의하기](#qml에서-javascript-리소스-정의하기)
    * [QML에서 JavaScript 리소스 가져오기](#qml에서-javascript-리소스-가져오기)
    * [JavaScript 호스트 환경](#javascript-호스트-환경)
  - [QML 타입 시스템](#qml-타입-시스템)
    * [QML 값 타입](#qml-값-타입)
    * [QML 객체 타입](#qml-객체-타입)
    * [QML로부터 객체 타입 정의하기](#qml로부터-객체-타입-정의하기)
    * [C++로부터 객체 타입 정의하기](#c로부터-객체-타입-정의하기)
  - [QML 모듈](#qml-모듈)
    * [QML 모듈 지정하기](#qml-모듈-지정하기)
    * [지원되는 QML 모듈 타입 - Identified 모듈](#지원되는-qml-모듈-타입---identified-모듈)
    * [지원되는 QML 모듈 타입 - Legacy 모듈](#지원되는-qml-모듈-타입---legacy-모듈)
    * [C++ 플러그인에서 타입 및 기능 제공하기](#c-플러그인에서-타입-및-기능-제공하기)
  - [QML 도큐먼트](#qml-도큐먼트)
    * [QML 도큐먼트의 구조](#qml-도큐먼트의-구조)
    * [QML 도큐먼트를 통한 객체 타입 정의하기](#qml-도큐먼트를-통한-객체-타입-정의하기)
    * [리소스 로딩 및 네트워크 투명성](#리소스-로딩-및-네트워크-투명성)
    * [범위 및 네이밍 규칙](#범위-및-네이밍-규칙)

---

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

---

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

---

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

---

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

구문론적으로 바인딩은 임의의 복잡성으로 허용됩니다. 그러나 바인딩이 - 여러 줄 또는 명령형 루프를 포함하는 것과 같이 - 지나치게 복잡하면 바인딩이 프로퍼티 관계를 설명하는 것 이상으로 사용되고 있음을 나타낼 수 있습니다. 복잡한 바인딩은 코드 성능, 가독성 및 유지보수성을 줄일 수 있습니다. 복잡한 바인딩을 가진 컴포넌트를 다시 설계하거나 최소한 바인딩을 별도의 함수로 분리하는 것이 좋을 수 있습니다. 일반적으로 사용자는 바인딩의 연산 순서에 의존해서는 안 됩니다.

* JavaScript로부터 프로퍼티 바인딩 생성하기

바인딩이 있는 프로퍼티는 필요에 따라 자동으로 업데이트됩니다. 그러나 나중에 JavaScript 문으로부터 프로퍼티에 정적인 값이 할당되면 바인딩이 제거될 것입니다.

예를 들면, 아래 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)은 처음에 height가 항상 width의 2배가 되도록 합니다. 그러나 스페이스 키를 누르면 width*3의 현재 값이 height에 정적인 값으로 할당될 것입니다. 그 후에는 width가 변경되더라도 height가 이 값으로 고정된 상태로 유지될 것입니다. 정적인 값을 할당하면 바인딩이 제거됩니다.

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

Rectangle에 고정된 height를 부여하고 자동 업데이트를 중지하려는 의도라면 문제가 되지 않습니다. 그러나 width와 height 사이에 새로운 관계를 설정하려는 의도라면 대신 새로운 바인딩 표현식을 Qt.binding() 함수로 감싸야 합니다:

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

이제 스페이스 키를 누른 후에는 Rectangle의 height가 항상 width의 3배가 되도록 계속 자동 업데이트됩니다.

* 바인딩의 덮어쓰기 디버그하기

QML 앱에서 버그의 일반적인 원인은 JavaScript 문으로부터 정적 값으로 실수로 바인딩을 덮어쓰는 것입니다. 개발자들이 이런 종류의 문제를 추적하는 것을 돕기 위해 QML 엔진은 명령형 할당으로 인해 바인딩이 손실될 때마다 메시지를 보낼 수 있습니다.

이러한 메시지를 생성하려면 다음을 호출하여 qt.qml.binding.removal 로깅 카테고리에 대한 정보 출력을 활성화해야 합니다:

```qml
QLoggingCategory::setFilterRules(QStringLiteral("qt.qml.binding.removal.info=true"));
```

로깅 카테고리에서 출력을 활성화하는 방법에 대한 자세한 내용은 [QLoggingCategory](https://doc.qt.io/qt-6/qloggingcategory.html) 문서를 참조하십시오.

일부 상황에서는 바인딩을 덮어쓰는 것이 완벽하게 합리적이라는 것을 주의하십시오. QML 엔진에 의해 생성된 메시지는 진단 보조로 취급해야 하며, 추가적인 조사 없이 문제의 증거로 취급할 필요는 없습니다.

* 프로퍼티 바인딩으로 this 사용하기

JavaScript로부터 프로퍼티 바인딩을 생성할 때, this 키워드를 사용하여 바인딩을 수신한 객체를 참조할 수 있습니다. 이는 프로퍼티 이름으로 모호성을 해결하는 데 도움이 됩니다.

예를 들면, 다음의 Component.onCompleted 핸들러는 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html)의 범위 내에서 정의됩니다. 이 범위에서 width는 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)의 width가 아니라 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html)의 width를 나타냅니다. [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)의 height를 자신의 width로 결합하려면 바인딩 표현식은 명시적으로 this.width (또는 rect.width)를 참조해야 합니다:

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

---

#### 시그널과 핸들러 이벤트 시스템

앱 및 사용자 인터페이스 컴포넌트들은 서로 통신할 필요가 있습니다. 예를 들어, 버튼은 사용자가 자신을 클릭했다는 것을 알아야 합니다. 버튼은 자신의 상태를 나타내거나 일부 로직을 수행하기 위해 색상을 변경할 수 있습니다. 또한, 앱은 사용자가 버튼을 클릭하고 있는지 여부를 알아야 합니다. 앱은 이 클릭 이벤트를 다른 앱에 중계할 필요가 있을 수 있습니다.

QML은 시그널 및 핸들러 메커니즘을 가지며, 여기서 시그널은 이벤트이고 시그널 핸들러를 통해 시그널에 응답합니다. 시그널이 방출(emit)되면 해당 시그널 핸들러가 호출됩니다. 핸들러에 스크립트 또는 다른 연산과 같은 로직을 배치하면 컴포넌트가 이벤트에 응답할 수 있습니다.

* 시그널 핸들러로 시그널 수신하기

특정 객체에 대한 특정 시그널이 방출될 때 공지를 받으려면 객체 정의에서 on<Signal>이라는 이름의 시그널 핸들러를 선언해야 합니다. 여기서 <Signal>은 시그널의 이름이며 첫 글자는 대문자입니다. 시그널 핸들러는 시그널 핸들러가 호출될 때 실행할 JavaScript 코드를 포함해야 합니다.

예를 들어, [Qt Quick Controls](https://doc.qt.io/qt-6/qtquickcontrols-index.html) 모듈의 [Button](https://doc.qt.io/qt-6/qml-qtquick-controls-button.html) 타입은 clicked 시그널을 가지고 있는데 이것은 버튼을 클릭할 때마다 방출됩니다. 이 경우, 이 시그널을 수신하는 시그널 핸들러는 onClicked이어야 합니다. 아래 예제에서는 버튼을 클릭할 때마다 onClicked 핸들러가 호출되는데 부모 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)에 임의의 컬러를 적용합니다:

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

* 프로퍼티 변경 시그널 핸들러

QML 프로퍼티 값이 변경되면 시그널이 자동으로 방출됩니다. 이 타입의 시그널은 프로퍼티 변경 시그널이며 이러한 시그널에 대한 시그널 핸들러는 on<Property>Changed에 기록됩니다. 여기서 <Property>는 프로퍼티의 이름이며 첫 글자는 대문자로 표시됩니다.

예를 들어, [MouseArea](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html) 타입에는 [pressed](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html#pressed-signal) 프로퍼티가 있습니다. 이 프로퍼티가 변경될 때마다 공지를 받으려면 onPressedChanged라는 이름을 가진 시그널 핸들러를 작성하십시오:

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

[TapHandler](https://doc.qt.io/qt-6/qml-qtquick-taphandler.html) 도큐먼트에서 이름이 onPressedChanged인 시그널 핸들러가 문서화되지는 않았지만, 이 시그널은 pressed 프로퍼티가 존재한다는 사실에 의해 암시적으로 제공됩니다.

* 시그널 파라미터

시그널은 파라미터를 가지고 있을 수 있습니다. 이러한 파라미터에 접근하려면 핸들러에 함수를 할당해야 합니다. 화살표 함수와 익명 함수 모두 작동합니다.

For the following examples, consider a Status component with an errorOccurred signal (see Adding signals to custom QML types for more information about how signals can be added to QML components).
다음 예제에서는 errorOccurred 시그널이 있는 Status 컴포넌트를 고려합니다. (QML 컴포넌트에 시그널을 추가하는 방법에 대한 자세한 내용은 [Adding signals to custom QML types](https://doc.qt.io/qt-6/qtqml-syntax-signals.html#adding-signals-to-custom-qml-types)를 보십시오)

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

주의: 함수의 형식 파라미터 이름은 시그널의 이름과 일치할 필요가 없습니다.

모든 파라미터를 처리할 필요가 없는 경우 다음 파라미터를 생략할 수 있습니다:

```qml
Status {
    onErrorOccurred: function (message) { console.log(message) }
}
```

관심 있는 선행 파라미터를 생략할 수는 없지만, 일부 placeholder 이름을 사용하여 중요하지 않음을 독자에게 나타낼 수 있습니다:

```qml
Status {
    onErrorOccurred: (_, _, col) => console.log(`Error happened at column ${col}`)
}
```

주의: 함수를 사용하는 대신 일반 코드 블록을 사용하는 것이 가능하지만 권장하지는 않습니다. 이 경우 모든 시그널 파라미터가 블록의 범위에 주입됩니다. 그러나 파라미터 어디에서 왔는지 불분명하기 때문에 코드를 읽기 어렵게 만들 수 있고 QML 엔진에서 검색 속도가 느려집니다. 이러한 방식으로 파라미터를 주입하는 방법은 더 이상 사용하지 않으며 파라미터가 실제로 사용되는 경우 런타임 경고가 발생합니다.

* Connections 타입 사용하기

경우에 따라 해당 시그널을 방출하는 객체 외부의 신호에 액세스하는 것이 바람직할 수 있습니다. 이러한 목적을 위해 QtQuick 모듈은 임의의 객체들의 시그널에 연결하기 위한 [Connections](https://doc.qt.io/qt-6/qml-qtqml-connections.html) 타입을 제공합니다. [Connections](https://doc.qt.io/qt-6/qml-qtqml-connections.html) 객체는 지정된 [대상](https://doc.qt.io/qt-6/qml-qtqml-connections.html#target-prop)으로부터 모든 시그널을 수신할 수 있습니다.

예를 들어, 앞의 예제에서 onClicked 핸들러는 [대상](https://doc.qt.io/qt-6/qml-qtqml-connections.html#target-prop)이 단추로 설정된 [Connections](https://doc.qt.io/qt-6/qml-qtqml-connections.html) 객체 안에 onClicked 핸들러를 배치하여 루트 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)에서 대신 수신할 수 있습니다:

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

* 부착된 시그널 핸들러

[부착된 시그널 핸들러](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#attached-properties-and-attached-signal-handlers)는 핸들러가 선언된 객체가 아닌 부착 타입에서 시그널을 수신합니다.

예를 들어, [Component.onCompleted](https://doc.qt.io/qt-6/qml-qtqml-component.html#completed-signal)는 부착된 시그널 핸들러입니다. 이는 종종 생성 프로세스가 완료될 때 일부 JavaScript 코드를 실행하는 데 사용됩니다. 다음은 예제입니다:

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

onCompleted 핸들러가 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 타입의 completed 시그널에 응답하지 않습니다. 대신, completed 시그널을 가진 Component 부착 타입의 객체가 QML 엔진에 의해 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 객체에 자동으로 부착됩니다. Rectangle 객체가 생성될 때 엔진이 이 시그널을 방출하여 Component.onCompleted 시그널 핸들러를 트리거합니다.

부착된 시그널 핸들러는 각 개별 객체에 중요한 특정 신호를 객체에 공지할 수 있도록 합니다. 예를들어, 만약 Component.onCompleted 부착 시그널 핸들러가 없는 경우 객체는 어떤 특수 객체로부터 어떤 특수 시그널을 등록하지 않고는 이 공지를 수신할 수 없습니다. 부착된 시그널 핸들러 메커니즘은 객체가 추가 코드 없이 특정 시그널을 수신할 수 있도록 합니다.

부착된 시그널 핸들러에 대한 더 자세한 것은 [부착된 프로퍼티와 부착된 시그널 핸들러](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#attached-properties-and-attached-signal-handlers)를 보십시오.

* 커스텀 QML 타입에 시그널 추가하기

signal 키워드를 통해 커스텀 QML 타입에 시그널을 추가할 수 있습니다.

새로운 시그널을 정의하는 구문은 다음과 같습니다:

```qml
signal <name>[([<type> <parameter name>[, ...]])]
```

메서드로 시그널을 호출하여 시그널을 방출합니다.

예를 들어, 다음의 코드는 SquareButton.qml이라는 파일에 정의되어 있습니다. 루트 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 객체에는 활성화된 시그널이 있으며, 이 시그널은 자식 [TapHandler](https://doc.qt.io/qt-6/qml-qtquick-taphandler.html)를 누를 때마다 표시됩니다. 이 예제에서는 활성화된 시그널이 마우스 클릭 x, y 좌표와 함께 방출됩니다:

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

이제 SquareButton의 모든 객체는 onActivated 시그널 핸들러를 사용하여 활성화된 시그널에 연결할 수 있습니다:

```qml
// myapplication.qml
SquareButton {
    onActivated: (xPosition, yPosition)=> console.log("Activated at " + xPosition + "," + yPosition)
}
```

커스텀 QML 타입에 대한 시그널을 작성하는 자세한 방법은 [Signal Attributes](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-attributes)를 보십시오.

* 시그널을 메서드/시그널과 연결하기

시그널 객체는 시그널을 메서드 또는 다른 시그널에 연결하는 connect() 메서드를 갖고 있습니다. 시그널이 메서드에 연결되면 시그널이 방출될 때마다 메서드가 자동으로 호출됩니다. 이 메커니즘은 시그널 핸들러 대신 메서드에 의해 신호를 수신할 수 있도록 합니다.

아래에서 messageReceived 메시지는 connect() 메서드를 사용하여 3가지 방법으로 연결됩니다:

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

많은 경우 connect() 함수를 사용하는 것보다 시그널 핸들러를 통해 시그널을 수신하는 것으로 충분합니다. 그러나 connect 메서드를 사용하면 앞서 본 것과 같이 여러 메서드로 시그널을 수신할 수 있는데, 이 방법은 시그널 핸들러의 고유한 이름이 지정되어야 하므로 시그널 핸들러에서는 불가능합니다. 또한 connect 메서드는 [동적으로 생성된 객체](https://doc.qt.io/qt-6/qtqml-javascript-dynamicobjectcreation.html)에 시그널을 연결할 때 유용합니다.

다음은 연결된 시그널을 제거하기 위한 disconnect() 메서드입니다:

```qml
Rectangle {
    id: relay
    //...

    function removeTelegraphSignal() {
        relay.messageReceived.disconnect(sendToTelegraph)
    }
}
```

* 시그널과 시그널 연결하기

시그널과 다른 시그널을 연결함으로써, connect() 메서드는 서로 다른 시그널 체인을 형성할 수 있습니다.

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

[TapHandler](https://doc.qt.io/qt-6/qml-qtquick-taphandler.html)의 tapped 시그널이 방출되면, send 시그널이 자동으로 방출됩니다.

```qml
output:
    MouseArea clicked
    Send clicked
```

---

#### QML과 JavaScript 통합하기

QML 언어는 JSON과 같은 구문을 사용하며 다양한 표현식과 메서드를 JavaScript 함수로 정의할 수 있습니다. 또한 사용자가 JavaScript 파일을 가져오고 가져온 기능을 사용할 수 있습니다.

이를 통해 개발자와 디자이너는 JavaScript에 대한 지식을 활용하여 사용자 인터페이스와 앱 로직을 모두 신속하게 개발할 수 있습니다.

* JavaScript 표현식

QML은 깊은 JavaScript 통합을 가지고 있으며, [시그널 핸들러](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-attributes)와 [메서드](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#method-attributes)를 JavaScript로 정의할 수 있습니다. QML의 또 다른 핵심 특징은 JavaScript를 사용하여 정의된 [프로퍼티 바인딩](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html)을 사용하여 객체 프로퍼티 간의 관계를 지정하고 강제할 수 있다는 것입니다.

QML에서 JavaScript 표현식을 사용하는 방법에 대한 자세한 내용은 [QML 문서의 JavaScript 표현식](https://doc.qt.io/qt-6/qtqml-javascript-expressions.html) 문서 페이지를 참조하십시오.

* JavaScript 리소스

JavaScript 함수에 정의된 앱 로직은 JavaScript 리소스로 알려진 별도의 JavaScript 파일로 분리될 수 있습니다. JavaScript 리소스에는 여러 가지 다른 종류의 시맨틱스가 있습니다.

QML에 대한 JavaScript 리소스 정의에 대한 자세한 내용은 [QML의 JavaScript 리소스 정의하기](https://doc.qt.io/qt-6/qtqml-javascript-resources.html) 설명서 페이지를 참조하십시오.

* JavaScript 가져오기

QML 문서는 JavaScript 리소스를 가져올 수 있고, JavaScript 리소스는 QML 모듈뿐만 아니라 다른 JavaScript 리소스를 가져올 수 있습니다. 이를 통해 앱 개발자는 모듈형의 자체 포함 파일에서 앱 로직을 제공할 수 있습니다.

JavaScript 리소스를 가져오는 방법과 제공하는 기능을 사용하는 방법에 대한 자세한 내용은 [JavaScript 리소스 가져오기](https://doc.qt.io/qt-6/qtqml-javascript-imports.html)라는 제목의 설명서 페이지를 참조하십시오.

* JavaScript 호스트 환경

QML 엔진은 웹 브라우저에서 제공하는 JavaScript 환경과 일부 다른 점이 있는 JavaScript 환경을 제공하며, 이 환경에서 실행되는 코드에는 일정한 제한이 적용되며, QML 엔진은 JavaScript 개발자에게 생소할 수 있는 루트 컨텍스트의 다양한 객체를 제공합니다.

이러한 제한 및 확장은 QML 엔진에서 제공하는 [JavaScript 호스트 환경](https://doc.qt.io/qt-6/qtqml-javascript-hostenvironment.html)에 대한 설명에 나와 있습니다.

또한 JavaScript 엔진이 사용하는 [메모리 관리](https://doc.qt.io/qt-6/qtqml-javascript-memory.html)에 대한 자세한 설명이 있습니다.

* JavaScript 엔진 구성하기

특수 사용 사례의 경우 JavaScript 엔진이 메모리를 처리하고 JavaScript를 컴파일하는 데 사용하는 일부 파라미터를 재정의할 수 있습니다. 이러한 파라미터에 대한 자세한 내용은 [JavaScript 엔진 구성하기](https://doc.qt.io/qt-6/qtqml-javascript-finetuning.html)를 참조하십시오.


##### QML와 함께 JavaScript 표현식 사용하기

The JavaScript Host Environment provided by QML can run valid standard JavaScript constructs such as conditional operators, arrays, variable setting, and loops. In addition to the standard JavaScript properties, the QML Global Object includes a number of helper methods that simplify building UIs and interacting with the QML environment.

The JavaScript environment provided by QML is stricter than that in a web browser. For example, in QML you cannot add to, or modify, members of the JavaScript global object. In regular JavaScript, it is possible to do this accidentally by using a variable without declaring it. In QML this will throw an exception, so all local variables must be explicitly declared. See JavaScript Environment Restrictions for a complete description of the restrictions on JavaScript code executed from QML.

Various parts of QML documents can contain JavaScript code:

1. The body of property bindings. These JavaScript expressions describe relationships between QML object properties. When dependencies of a property change, the property is automatically updated too, according to the specified relationship.
2. The body of Signal handlers. These JavaScript statements are automatically evaluated whenever a QML object emits the associated signal.
3. The definition of custom methods. JavaScript functions that are defined within the body of a QML object become methods of that object.
4. Standalone JavaScript resource (.js) files. These files are actually separate from QML documents, but they can be imported into QML documents. Functions and variables that are defined within the imported files can be used in property bindings, signal handlers, and custom methods.

* JavaScript in property bindings

In the following example, the color property of Rectangle depends on the pressed property of TapHandler. This relationship is described using a conditional expression:

```qml
import QtQuick 2.12

Rectangle {
    id: colorbutton
    width: 200; height: 80;

    color: inputHandler.pressed ? "steelblue" : "lightsteelblue"

    TapHandler {
        id: inputHandler
    }
}
```

In fact, any JavaScript expression (no matter how complex) may be used in a property binding definition, as long as the result of the expression is a value whose type can be assigned to the property. This includes side effects. However, complex bindings and side effects are discouraged because they can reduce the performance, readability, and maintainability of the code.

There are two ways to define a property binding: the most common one is shown in the example earlier, in a property initialization. The second (and much rarer) way is to assign the property a function returned from the Qt.binding() function, from within imperative JavaScript code, as shown below:

```qml
import QtQuick 2.12

Rectangle {
    id: colorbutton
    width: 200; height: 80;

    color: "red"

    TapHandler {
        id: inputHandler
    }

    Component.onCompleted: {
        color = Qt.binding(function() { return inputHandler.pressed ? "steelblue" : "lightsteelblue" });
    }
}
```

See the property bindings documentation for more information about how to define property bindings, and see the documentation about Property Assignment versus Property Binding for information about how bindings differ from value assignments.

* JavaScript in signal handlers

QML object types can emit signals in reaction to certain events occurring. Those signals can be handled by signal handler functions, which can be defined by clients to implement custom program logic.

Suppose that a button represented by a Rectangle type has a TapHandler and a Text label. The TapHandler emits its tapped signal when the user presses the button. The clients can react to the signal in the onTapped handler using JavaScript expressions. The QML engine executes these JavaScript expressions defined in the handler as required. Typically, a signal handler is bound to JavaScript expressions to initiate other events or to assign property values.

```qml
import QtQuick 2.12

Rectangle {
    id: button
    width: 200; height: 80; color: "lightsteelblue"

    TapHandler {
        id: inputHandler
        onTapped: {
            // arbitrary JavaScript expression
            console.log("Tapped!")
        }
    }

    Text {
        id: label
        anchors.centerIn: parent
        text: inputHandler.pressed ? "Pressed!" : "Press here!"
    }
}
```

For more details about signals and signal handlers, refer to the following topics:
- Signal and Handler Event System
- QML Object Attributes

* JavaScript in standalone functions

Program logic can also be defined in JavaScript functions. These functions can be defined inline in QML documents (as custom methods) or externally in imported JavaScript files.

* JavaScript in custom methods

Custom methods can be defined in QML documents and may be called from signal handlers, property bindings, or functions in other QML objects. Such methods are often referred to as inline JavaScript functions because their implementation is included in the QML object type definition (QML document), instead of in an external JavaScript file.

An example of an inline custom method is as follows:

```qml
import QtQuick 2.12

Item {
    function fibonacci(n){
        var arr = [0, 1];
        for (var i = 2; i < n + 1; i++)
            arr.push(arr[i - 2] + arr[i -1]);

        return arr;
    }
    TapHandler {
        onTapped: console.log(fibonacci(10))
    }
}
```

The fibonacci function is run whenever the TapHandler emits a tapped signal.

Note: The custom methods defined inline in a QML document are exposed to other objects, and therefore inline functions on the root object in a QML component can be invoked by callers outside the component. If this is not desired, the method can be added to a non-root object or, preferably, written in an external JavaScript file.

See the QML Object Attributes documentation for more information on defining custom methods in QML using JavaScript.

* Functions defined in a JavaScript file

Non-trivial program logic is best separated into a separate JavaScript file. This file can be imported into QML using an import statement, like the QML modules.

For example, the fibonacci() method in the earlier example could be moved into an external file named fib.js, and accessed like this:

```qml
import QtQuick 2.12
import "fib.js" as MathFunctions

Item {
    TapHandler {
        onTapped: console.log(MathFunctions.fibonacci(10))
    }
}
```

For more information about loading external JavaScript files into QML, read the section about Importing JavaScript Resources in QML.

* Connecting signals to JavaScript functions

QML object types that emit signals also provide default signal handlers for their signals, as described in the previous section. Sometimes, however, a client wants to trigger a function defined in a QML object when another QML object emits a signal. Such scenarios can be handled by a signal connection.

A signal emitted by a QML object may be connected to a JavaScript function by calling the signal's connect() method and passing the JavaScript function as an argument. For example, the following code connects the TapHandler's tapped signal to the jsFunction() in script.js:

```qml
import QtQuick 2.12
import "script.js" as MyScript

Item {
    id: item
    width: 200; height: 200

    TapHandler {
        id: inputHandler
    }

    Component.onCompleted: {
        inputHandler.tapped.connect(MyScript.jsFunction)
    }
}
```

```qml
// script.js

function jsFunction() {
    console.log("Called JavaScript function!")
}
```

The jsFunction() is called whenever the TapHandler's tapped signal is emitted.

See Connecting Signals to Methods and Signals for more information.

* JavaScript in application startup code

It is occasionally necessary to run some imperative code at application (or component instance) startup. While it is tempting to just include the startup script as global code in an external script file, this can have severe limitations as the QML environment may not have been fully established. For example, some objects might not have been created or some property bindings may not have been established. See JavaScript Environment Restrictions for the exact limitations of global script code.

A QML object emits the Component.completed attached signal when its instantiation is complete. The JavaScript code in the corresponding Component.onCompleted handler runs after the object is instantiated. Thus, the best place to write application startup code is in the Component.onCompleted handler of the top-level object, because this object emits Component.completed when the QML environment is fully established.

For example:

```qml
import QtQuick 2.0

Rectangle {
    function startupFunction() {
        // ... startup code
    }

    Component.onCompleted: startupFunction();
}
```

Any object in a QML file - including nested objects and nested QML component instances - can use this attached property. If there is more than one onCompleted() handler to execute at startup, they are run sequentially in an undefined order.

Likewise, every Component emits a destruction() signal just before being destroyed.


##### JavaScript에서 동적 QML 객체 생성

QML supports the dynamic creation of objects from within JavaScript. This is useful to delay instantiation of objects until necessary, thereby improving application startup time. It also allows visual objects to be dynamically created and added to the scene in reaction to user input or other events.

See the Dynamic Scene example for a demonstration of the concepts discussed on this page.

* Creating Objects Dynamically

There are two ways to create objects dynamically from JavaScript. You can either call Qt.createComponent() to dynamically create a Component object, or use Qt.createQmlObject() to create an object from a string of QML. Creating a component is better if you have an existing component defined in a QML document and you want to dynamically create instances of that component. Otherwise, creating an object from a string of QML is useful when the object QML itself is generated at runtime.

* Creating a Component Dynamically

To dynamically load a component defined in a QML file, call the Qt.createComponent() function in the Qt object. This function takes the URL of the QML file as its only argument and creates a Component object from this URL.

Once you have a Component, you can call its createObject() method to create an instance of the component. This function can take one or two arguments:

- The first is the parent for the new object. The parent can be a graphical object (i.e. of the Item type) or non-graphical object (i.e. of the QtObject or C++ QObject type). Only graphical objects with graphical parent objects will be rendered to the Qt Quick visual canvas. If you wish to set the parent later you can safely pass null to this function.
- The second is optional and is a map of property-value pairs that define initial any property values for the object. Property values specified by this argument are applied to the object before its creation is finalized, avoiding binding errors that may occur if particular properties must be initialized to enable other property bindings. Additionally, there are small performance benefits when compared to defining property values and bindings after the object is created.

Here is an example. First there is Sprite.qml, which defines a simple QML component:

```qml
import QtQuick 2.0

Rectangle { width: 80; height: 50; color: "red" }
```

Our main application file, main.qml, imports a componentCreation.js JavaScript file that will create Sprite objects:

```qml
import QtQuick 2.0
import "componentCreation.js" as MyScript

Rectangle {
    id: appWindow
    width: 300; height: 300

    Component.onCompleted: MyScript.createSpriteObjects();
}
```

Here is componentCreation.js. Notice it checks whether the component status is Component.Ready before calling createObject() in case the QML file is loaded over a network and thus is not ready immediately.

```qml
var component;
var sprite;

function createSpriteObjects() {
    component = Qt.createComponent("Sprite.qml");
    if (component.status == Component.Ready)
        finishCreation();
    else
        component.statusChanged.connect(finishCreation);
}

function finishCreation() {
    if (component.status == Component.Ready) {
        sprite = component.createObject(appWindow, {x: 100, y: 100});
        if (sprite == null) {
            // Error Handling
            console.log("Error creating object");
        }
    } else if (component.status == Component.Error) {
        // Error Handling
        console.log("Error loading component:", component.errorString());
    }
}
```

If you are certain the QML file to be loaded is a local file, you could omit the finishCreation() function and call createObject() immediately:

```qml
function createSpriteObjects() {
    component = Qt.createComponent("Sprite.qml");
    sprite = component.createObject(appWindow, {x: 100, y: 100});

    if (sprite == null) {
        // Error Handling
        console.log("Error creating object");
    }
}
```

Notice in both instances, createObject() is called with appWindow passed as the parent argument, since the dynamically created object is a visual (Qt Quick) object. The created object will become a child of the appWindow object in main.qml, and appear in the scene.

When using files with relative paths, the path should be relative to the file where Qt.createComponent() is executed.

To connect signals to (or receive signals from) dynamically created objects, use the signal connect() method. See Connecting Signals to Methods and Signals for more information.

It is also possible to instantiate components without blocking via the incubateObject() function.

* Creating an Object from a String of QML

If the QML is not defined until runtime, you can create a QML object from a string of QML using the Qt.createQmlObject() function, as in the following example:

```qml
const newObject = Qt.createQmlObject(`
    import QtQuick 2.0

    Rectangle {
        color: "red"
        width: 20
        height: 20
    }
    `,
    parentItem,
    "myDynamicSnippet"
);
```

The first argument is the string of QML to create. Just like in a new file, you will need to import any types you wish to use. The second argument is the parent object for the new object, and the parent argument semantics which apply to components are similarly applicable for createQmlObject(). The third argument is the file path to associate with the new object; this is used for error reporting.

If the string of QML imports files using relative paths, the path should be relative to the file in which the parent object (the second argument to the method) is defined.

Important: When building static QML applications, QML files are scanned to detect import dependencies. That way, all necessary plugins and resources are resolved at compile time. However, only explicit import statements are considered (those found at the top of a QML file), and not import statements enclosed within string literals. To support static builds, you therefore need to ensure that QML files using Qt.createQmlObject(), explicitly contain all necessary imports at the top of the file in addition to inside the string literals.

* Maintaining Dynamically Created Objects

When managing dynamically created objects, you must ensure the creation context outlives the created object. Otherwise, if the creation context is destroyed first, the bindings and signal handlers in the dynamic object will no longer work.

The actual creation context depends on how an object is created:
- If Qt.createComponent() is used, the creation context is the QQmlContext in which this method is called
- If Qt.createQmlObject() is called, the creation context is the context of the parent object passed to this method
- If a Component{} object is defined and createObject() or incubateObject() is called on that object, the creation context is the context in which the Component is defined

Also, note that while dynamically created objects may be used the same as other objects, they do not have an id in QML.

* Deleting Objects Dynamically

In many user interfaces, it is sufficient to set a visual object's opacity to 0 or to move the visual object off the screen instead of deleting it. If you have lots of dynamically created objects, however, you may receive a worthwhile performance benefit if unused objects are deleted.

Note that you should never manually delete objects that were dynamically created by convenience QML object factories (such as Loader and Repeater). Also, you should avoid deleting objects that you did not dynamically create yourself.

Items can be deleted using the destroy() method. This method has an optional argument (which defaults to 0) that specifies the approximate delay in milliseconds before the object is to be destroyed.

Here is an example. The application.qml creates five instances of the SelfDestroyingRect.qml component. Each instance runs a NumberAnimation, and when the animation has finished, calls destroy() on its root object to destroy itself:

application.qml	
```qml
import QtQuick 2.0

Item {
    id: container
    width: 500; height: 100

    Component.onCompleted: {
        var component = Qt.createComponent("SelfDestroyingRect.qml");
        for (var i=0; i<5; i++) {
            var object = component.createObject(container);
            object.x = (object.width + 10) * i;
        }
    }
}
```

SelfDestroyingRect.qml	
```qml
import QtQuick 2.0

Rectangle {
    id: rect
    width: 80; height: 80
    color: "red"

    NumberAnimation on opacity {
        to: 0
        duration: 1000

        onRunningChanged: {
            if (!running) {
                console.log("Destroying...")
                rect.destroy();
            }
        }
    }
}
```

Alternatively, the application.qml could have destroyed the created object by calling object.destroy().

Note that it is safe to call destroy() on an object within that object. Objects are not destroyed the instant destroy() is called, but are cleaned up sometime between the end of that script block and the next frame (unless you specified a non-zero delay).

Note also that if a SelfDestroyingRect instance was created statically like this:

```qml
Item {
    SelfDestroyingRect {
        // ...
    }
}
```

This would result in an error, since objects can only be dynamically destroyed if they were dynamically created.

Objects created with Qt.createQmlObject() can similarly be destroyed using destroy():

```qml
const newObject = Qt.createQmlObject(`
    import QtQuick 2.0

    Rectangle {
        color: "red"
        width: 20
        height: 20
    }
    `,
    parentItem,
    "myDynamicSnippet"
);
newObject.destroy(1000);
```


##### QML에서 JavaScript 리소스 정의하기

The program logic for a QML application may be defined in JavaScript. The JavaScript code may either be defined in-line in QML documents, or separated into JavaScript files (known as JavaScript Resources in QML).

There are two different kinds of JavaScript resources which are supported in QML: code-behind implementation files, and shared (library) files. Both kinds of JavaScript resource may be imported by other JavaScript resources, or included in QML modules.

* Code-Behind Implementation Resource

Most JavaScript files imported into a QML document are stateful implementations for the QML document importing them. In these cases, each instance of the QML object type defined in the document requires a separate copy of the JavaScript objects and state in order to behave correctly.

The default behavior when importing JavaScript files is to provide a unique, isolated copy for each QML component instance. If that JavaScript file does not import any resources or modules with a .import statement, its code will run in the same scope as the QML component instance and consequently can access and manipulate the objects and properties declared in that QML component. Otherwise, it will have its own unique scope, and objects and properties of the QML component should be passed to the functions of the JavaScript file as parameters if they are required.

An example of a code-behind implementation resource follows:

```qml
// MyButton.qml
import QtQuick 2.0
import "my_button_impl.js" as Logic // A new instance of this JavaScript resource
                                    // is loaded for each instance of Button.qml.

Rectangle {
    id: rect
    width: 200
    height: 100
    color: "red"

    MouseArea {
        id: mousearea
        anchors.fill: parent
        onClicked: Logic.onClicked(rect)
    }
}
```

```qml
// my_button_impl.js
var clickCount = 0;   // this state is separate for each instance of MyButton
function onClicked(button) {
    clickCount += 1;
    if ((clickCount % 5) == 0) {
        button.color = Qt.rgba(1,0,0,1);
    } else {
        button.color = Qt.rgba(0,1,0,1);
    }
}
```

In general, simple logic should be defined in-line in the QML file, but more complex logic should be separated into code-behind implementation resources for maintainability and readability.

* Shared JavaScript Resources (Libraries)

By default, JavaScript files imported from QML share their context with the QML component. That means the JavaScript files have access to the same QML objects and can modify them. As a consequence, each import must have a unique copy of these files.

The previous section covers stateful imports of JavaScript files. However, some JavaScript files are stateless and act more like reusable libraries, in the sense that they provide a set of helper functions that do not require anything from where they were imported from. You can save significant amounts of memory and speed up the instantiation of QML components if you mark such libraries with a special pragma, as shown in the following example.

```qml
// factorial.js
.pragma library

var factorialCount = 0;

function factorial(a) {
    a = parseInt(a);

    // factorial recursion
    if (a > 0)
        return a * factorial(a - 1);

    // shared state
    factorialCount += 1;

    // recursion base-case.
    return 1;
}

function factorialCallCount() {
    return factorialCount;
}
```

The pragma declaration must appear before any JavaScript code excluding comments.

Note that multiple QML documents can import "factorial.js" and call the factorial and factorialCallCount functions that it provides. The state of the JavaScript import is shared across the QML documents which import it, and thus the return value of the factorialCallCount function may be non-zero when called within a QML document which never calls the factorial function.

For example:

```qml
// Calculator.qml
import QtQuick 2.0
import "factorial.js" as FactorialCalculator // This JavaScript resource is only
                                             // ever loaded once by the engine,
                                             // even if multiple instances of
                                             // Calculator.qml are created.

Text {
    width: 500
    height: 100
    property int input: 17
    text: "The factorial of " + input + " is: " + FactorialCalculator.factorial(input)
}
```

As they are shared, .pragma library files cannot access QML component instance objects or properties directly, although QML values can be passed as function parameters.


##### QML에서 JavaScript 리소스 가져오기

JavaScript resources may be imported by QML documents and other JavaScript resources. JavaScript resources may be imported via either relative or absolute URLs. In the case of a relative URL, the location is resolved relative to the location of the QML document or JavaScript Resource that contains the import. If the script file is not accessible, an error will occur. If the JavaScript needs to be fetched from a network resource, the component's status is set to "Loading" until the script has been downloaded.

JavaScript resources may also import QML modules and other JavaScript resources. The syntax of an import statement within a JavaScript resource differs slightly from an import statement within a QML document, which is documented thoroughly below.

* Importing a JavaScript Resource from a QML Document

A QML document may import a JavaScript resource with the following syntax:

```qml
import "ResourceURL" as Qualifier
```

For example:

```qml
import "jsfile.js" as Logic
```

Imported JavaScript resources are always qualified using the "as" keyword. The qualifier for JavaScript resources must start with an uppercase letter, and must be unique, so there is always a one-to-one mapping between qualifiers and JavaScript files. (This also means qualifiers cannot be named the same as built-in JavaScript objects such as Date and Math).

The functions defined in an imported JavaScript file are available to objects defined in the importing QML document, via the "Qualifier.functionName(params)" syntax. Functions in JavaScript resources may take parameters whose types can be any QML value types or object types, as well as normal JavaScript types. The normal data type conversion rules will apply to parameters and return values when calling such functions from QML.

* Imports Within JavaScript Resources

In QtQuick 2.0, support has been added to allow JavaScript resources to import other JavaScript resources and also QML type namespaces using a variation of the standard QML import syntax (where all of the previously described rules and qualifications apply).

Due to the ability of a JavaScript resource to import another script or QML module in this fashion in QtQuick 2.0, some extra semantics are defined:
- a script with imports will not inherit imports from the QML document which imported it (so accessing Component.errorString will fail, for example)
- a script without imports will inherit imports from the QML document which imported it (so accessing Component.errorString will succeed, for example)
- a shared script (i.e., defined as .pragma library) does not inherit imports from any QML document even if it imports no other scripts or modules

The first semantic is conceptually correct, given that a particular script might be imported by any number of QML files. The second semantic is retained for the purposes of backwards-compatibility. The third semantic remains unchanged from the current semantics for shared scripts, but is clarified here in respect to the newly possible case (where the script imports other scripts or modules).

* Importing a JavaScript Resource from Another JavaScript Resource

A JavaScript resource may import another in the following fashion:

```qml
import * as MathFunctions from "factorial.mjs";
```

Or:

```qml
.import "filename.js" as Qualifier
```

The former is standard ECMAScript syntax for importing ECMAScript modules, and only works from within ECMAScript modules as denoted by the mjs file extension. The latter is an extension to JavaScript provided by the QML engine and will work also with non-modules. As an extension superseded by the ECMAScript standard, its usage is discouraged.

When a JavaScript file is imported this way, it is imported with a qualifier. The functions in that file are then accessible from the importing script via the qualifier (that is, as Qualifier.functionName(params)).

Sometimes it is desirable to have the functions made available in the importing context without needing to qualify them. In this case ECMAScript modules and the JavaScript import statement should be used without the as qualifier.

For example, the QML code below left calls showCalculations() in script.mjs, which in turn can call factorial() in factorial.mjs, as it has included factorial.mjs using import.

```qml
import QtQuick 2.0
import "script.mjs" as MyScript

Item {
    width: 100; height: 100

    MouseArea {
        anchors.fill: parent
        onClicked: {
            MyScript.showCalculations(10)
            console.log("Call factorial() from QML:",
                MyScript.factorial(10))
        }
    }
}
```

```qml
// script.mjs
import { factorial } from "factorial.mjs"
export { factorial }

export function showCalculations(value) {
    console.log(
        "Call factorial() from script.js:",
        factorial(value));
}
```

```qml
// factorial.mjs
export function factorial(a) {
    a = parseInt(a);
    if (a <= 0)
        return 1;
    else
        return a * factorial(a - 1);
}
```

The Qt.include() function includes one JavaScript file from another without using ECMAScript modules and without qualifying the import. It makes all functions and variables from the other file available in the current file's namespace, but ignores all pragmas and imports defined in that file. This is not a good idea as a function call should never modify the caller's context.

Qt.include() is deprecated and should be avoided. It will be removed in a future version of Qt.

* Importing a QML Module from a JavaScript Resource

A JavaScript resource may import a QML module in the following fashion:

```qml
.import TypeNamespace MajorVersion.MinorVersion as Qualifier
```

Below you can see an example that also shows how to use the QML types from a module imported in javascript:

```qml
.import Qt.test 1.0 as JsQtTest

var importedEnumValue = JsQtTest.MyQmlObject.EnumValue3
```

In particular, this may be useful in order to access functionality provided via a singleton type; see QML_SINGLETON for more information.

Your JavaScript resource by default can access all imports of the component that imports the resource. It does not have access to the componpents's imports if it is declared as a stateless library (using .pragma library) or contains an explicit .import statment.

Note: The .import syntax doesn't work for scripts used in WorkerScript

See also Defining JavaScript Resources in QML.


##### JavaScript 호스트 환경

QML provides a JavaScript host environment tailored to writing QML applications. This environment is different from the host environment provided by a browser or a server-side JavaScript environment such as Node.js. For example, QML does not provide a window object or DOM API as commonly found in a browser environment.

Common Base

Like a browser or server-side JavaScript environment, the QML runtime implements the ECMAScript Language Specification standard. This provides access to all of the built-in types and functions defined by the standard, such as Object, Array, and Math. The QML runtime implements the 7th edition of the standard.

Nullish Coalescing (??) (since Qt 5.15) and Optional Chaining (?.) (since Qt 6.2) are also implemented in the QML runtime.

The standard ECMAScript built-ins are not explicitly documented in the QML documentation. For more information on their use, please refer to the ECMA-262 7th edition standard or one of the many online JavaScript reference and tutorial sites, such as the W3Schools JavaScript Reference (JavaScript Objects Reference section). Many sites focus on JavaScript in the browser, so in some cases you may need to double check the specification to determine whether a given function or object is part of standard ECMAScript or specific to the browser environment. In the case of the W3Schools link above, the JavaScript Objects Reference section generally covers the standard, while the Browser Objects Reference and HTML DOM Objects Reference sections are browser specific (and thus not applicable to QML).

* Type annotations and assertions

Function declarations in QML documents can, and should, contain type annotations. Type annotations are appended to the declaration of arguments and to the function itself, for annotating the return type. The following function takes an int and a string parameter, and returns a QtObject:

```qml
function doThings(a: int, b: string) : QtObject { ... }
```

Type annotations help tools like Qt Creator and qmllint to make sense of the code and provide better diagnostics. Moreover, they make functions easier to use from C++. See Interacting with QML Objects from C++ for more information.

By default, type annotations are ignored by the interpreter and the JIT compiler, but enforced by qmlcachegen and qmlsc when compiling to C++. This can lead to differences in behavior if you either pass values that are not actually of the declared type or if you modify instances of QML Value Types passed as typed arguments. Value types are passed by reference by the interpreter and JIT, but by value when compiled to C++.

You can eliminate those differences by either forcing the interpreter and JIT to also respect type annotations or by having qmlcachegen and qmlsc ignore type annotations. The former has a performance cost when using the interpreter or JIT, the latter makes the compilation to C++ avoid any JavaScript functions, and any bindings and signal handlers that call JavaScript functions. Therefore less code will be compiled to C++.

In order to always enforce type annotations, add the following to your QML document:

```qml
pragma FunctionSignatureBehavior: Enforced
```

In order to always ignore type annotations, add the following instead:

```qml
pragma FunctionSignatureBehavior: Ignored
```

Type assertions (sometimes called as-casts) can also be used in order to cast an object to a different object type. If the object is actually of the given type, then the type assertion returns the same object. If not, it returns null. In the following snippet we assert that the parent object is a Rectangle before accessing a specific member of it.

```qml
Item {
    property color parentColor: (parent as Rectangle)?.color || "red"
}
```

The optional chaining (?.) avoids throwing an exception if the parent is actually not a rectangle. In that case "red" is chosen as parentColor.

* QML Global Object

The QML JavaScript host environment implements a number of host objects and functions, as detailed in the QML Global Object documentation.

These host objects and functions are always available, regardless of whether any modules have been imported.

* JavaScript Objects and Functions

A list of the JavaScript objects, functions and properties supported by the QML engine can be found in the List of JavaScript Objects and Functions.

Note that QML makes the following modifications to native objects:
- An arg() function is added to the String prototype.
- Locale-aware conversion functions are added to the Date and Number prototypes.

In addition, QML also extends the behavior of the instanceof function to allow for type checking against QML types. This means that you may use it to verify that a variable is indeed the type you expect, for example:

```qml
var v = something();
if (!v instanceof Item) {
    throw new TypeError("I need an Item type!");
}

...
```

* JavaScript Environment Restrictions

QML implements the following restrictions for JavaScript code:

- JavaScript code written in a .qml file cannot modify the global object. JavaScript code in a .js file can modify the global object, and those modifications will be visible to the .qml file when imported.
In QML, the global object is constant - existing properties cannot be modified or deleted, and no new properties may be created.

Most JavaScript programs do not intentionally modify the global object. However, JavaScript's automatic creation of undeclared variables is an implicit modification of the global object, and is prohibited in QML.

Assuming that the a variable does not exist in the scope chain, the following code is illegal in QML:

```qml
// Illegal modification of undeclared variable
a = 1;
for (var ii = 1; ii < 10; ++ii)
    a = a * ii;
console.log("Result: " + a);
```

It can be trivially modified to this legal code.

```qml
var a = 1;
for (var ii = 1; ii < 10; ++ii)
    a = a * ii;
console.log("Result: " + a);
```

Any attempt to modify the global object - either implicitly or explicitly - will cause an exception. If uncaught, this will result in a warning being printed, that includes the file and line number of the offending code.

- Global code is run in a reduced scope.

During startup, if a QML file includes an external JavaScript file with "global" code, it is executed in a scope that contains only the external file itself and the global object. That is, it will not have access to the QML objects and properties it normally would.

Global code that only accesses script local variables is permitted. This is an example of valid global code.

```qml
var colors = [ "red", "blue", "green", "orange", "purple" ];
```

Global code that accesses QML objects will not run correctly.

```qml
// Invalid global code - the "rootObject" variable is undefined
var initialPosition = { rootObject.x, rootObject.y }
```

This restriction exists as the QML environment is not yet fully established. To run code after the environment setup has completed, see JavaScript in Application Startup Code.

- The value of this is undefined in QML in the majority of contexts.

The this keyword is supported when binding properties from JavaScript. In QML binding expressions, QML signal handlers, and QML declared functions, this refers to the scope object. In all other situations, the value of this is undefined in QML.

To refer to a specific object, provide an id. For example:

```qml
Item {
    width: 200; height: 100
    function mouseAreaClicked(area) {
        console.log("Clicked in area at: " + area.x + ", " + area.y);
    }
    // This will pass area to the function
    MouseArea {
        id: area
        y: 50; height: 50; width: 200
        onClicked: mouseAreaClicked(area)
    }
}
```

See also Scope and Naming Resolution.

---

#### QML 타입 시스템

-


##### QML 값 타입

-


##### QML 객체 타입

-


##### QML로부터 객체 타입 정의하기

-


##### C++로부터 객체 타입 정의하기

-

---

#### QML 모듈

-


##### QML 모듈 지정하기

-


##### 지원되는 QML 모듈 타입 - Identified 모듈

-


##### 지원되는 QML 모듈 타입 - Legacy 모듈

-


##### C++ 플러그인에서 타입 및 기능 제공하기

-

---

#### QML 도큐먼트

-


##### QML 도큐먼트의 구조

-


##### QML 도큐먼트를 통한 객체 타입 정의하기

-


##### 리소스 로딩 및 네트워크 투명성

-


##### 범위 및 네이밍 규칙

-
