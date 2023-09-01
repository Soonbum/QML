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
  - [QML 문서](#qml-문서)
    * [QML 문서의 구조](#qml-문서의-구조)
    * [QML 문서를 통한 객체 타입 정의하기](#qml-문서를-통한-객체-타입-정의하기)
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
| 객체(Object) 타입 | [QML 객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)은 QML 엔진에 의해 인스턴스화 할 수 있는 타입을 의미합니다. QML 타입은 대문자로 시작하는 .qml 파일 내 문서, 혹은 [QObject](https://doc.qt.io/qt-6/qobject.html)-기반 C++ 클래스에 의해 정의될 수 있습니다. 자세한 것은 [QML 타입 시스템](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)을 보십시오. |
| 객체(Object) | QML Object는 [QML 객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)의 인스턴스입니다. 이 객체들은 [객체 선언](https://doc.qt.io/qt-6/qtqml-syntax-basics.html)을 처리할 때 엔진에 의해 생성됩니다. 객체 선언에는 생성될 객체를 정의하거나 객 객체에 대하여 정의되는 속성이 있습니다. 게다가 객체는 Component.createObject()와 Qt.createQmlObject() 함수를 통해 런타임 시에 동적으로 생성될 수 있습니다. [늦은 인스턴스화](https://doc.qt.io/qt-6/qml-glossary.html#lazy-instantiation)도 보십시오. |
| 컴포넌트(Component) | 컴포넌트는 QML Object 또는 Object tree가 생성되는 템플릿입니다. QML 엔진에 의해 문서가 로드될 때 컴포넌트가 생성됩니다. 일단 한 번 로드되면 그것을 표현하는 Object 또는 Object tree를 인스턴스화 하는 데 사용될 수 있습니다. 게다가 [컴포넌트](https://doc.qt.io/qt-6/qml-qtqml-component.html) 타입은 문서 안에서 인라인으로 선언하는 데 사용될 수 있는 특수 타입입니다. 컴포넌트 객체들은 동적으로 QML object들을 생성하는 Qt.createComponent()를 통해서도 동적으로 생성될 수 있습니다. |
| 문서(Document) | [QML 문서](https://doc.qt.io/qt-6/qtqml-documents-topic.html)는 1개 이상의 import 구문으로 시작하고 단일 최상위 레벨 object 선언을 포함하는 자체 포함 QML 소스 코드 조각입니다. 문서는 .qml 파일 또는 텍스트 문자열 안에 있을 수 있습니다. 만약 문서가 대문자로 시작하는 이름을 가진 .qml 파일 안에 있다면, 엔진은 이 파일이 QML 타입의 정의라고 인식합니다. 최상위 레벨 객체 선언은 타입에 의해 인스턴스화될 Object tree를 캡슐화합니다. |
| 프로퍼티(Property) | 이름을 가지고 있으며 연관된 값을 가진 객체 타입의 속성입니다. 이 값은 외부에서 읽혀질 수 있습니다. (그리고 대부분의 경우 쓰여질 수도 있습니다.) 객체는 여러 개의 프로퍼티를 가질 수 있습니다. 여타 프로퍼티들은 해당 타입에 대한 데이터인 반면(예. [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 타입의 "text" 프로퍼티), 몇 가지 프로퍼티들은 canvas와 연관되어 있습니다(예. x, y, width, height, opacity). 자세한 것은 [QML 객체 애트리뷰트](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html)를 보십시오. |
| 바인딩(Binding) | 바인딩은 프로퍼티와 "결합"되어 있는 JavaScript 표현식을 의미합니다. 프로퍼티의 값은 해당 표현식을 평가하여 리턴되는 값입니다. 자세한 것은 [프로퍼티 바인딩](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html)을 보십시오. |
| 시그널(Signal) | 시그널은 QML 객체로부터의 알림을 의미합니다. 어떤 객체가 시그널을 방출(emit)하면, 다른 객체들은 이것을 듣고 [시그널 핸들러](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-attributes)를 통해 시그널을 처리할 수 있습니다. QML 객체의 대부분의 프로퍼티들은 변경 시그널(Change Signal)을 가지고 있습니다. 그리고 기능을 구현하기 위해 클라이언트가 정의한 연관된 변경 시그널 핸들러(Change Signal Handler)도 가지고 있습니다. 예를 들어 앱에서 정의된 MouseArea 타입 인스턴스의 "onClicked()" 핸들러는 사운드 재생을 발생시킬 수 있습니다. 자세한 것은 [시그널과 핸들러 이벤트 시스템](https://doc.qt.io/qt-6/qtqml-syntax-signals.html)을 보십시오. |
| 시그널 핸들러(Signal Handler) | 시그널 핸들러는 시그널에 의해 트리거되는 표현식(또는 함수)입니다. C++에서는 이것을 "슬롯(slot)"이라고도 합니다. 자세한 것은 [시그널과 핸들러 이벤트 시스템](https://doc.qt.io/qt-6/qtqml-syntax-signals.html)을 보십시오. |
| 늦은 인스턴스화(Lazy Instantiation) | 필요할 때까지 불필요한 작업을 하지 않기 위해 객체 인스턴스는 런타임 시 "늦게" 인스턴스화 될 수 있습니다. Qt Quick은 편하게 Lazy Instantiation을 구현하기 위해 [Loader](https://doc.qt.io/qt-6/qml-qtquick-loader.html) 타입을 제공합니다. |

---

### [QML 레퍼런스](https://doc.qt.io/qt-6/qmlreference.html)

QML은 고도의 동적 앱을 만들기 위한 멀티 패러다임 언어입니다. QML을 이용하면 UI 컴포넌트 같은 앱 빌딩 블럭을 선언할 수 있고 앱 동작을 정의할 수 있는 다양한 프로퍼티 집합이 있습니다. 앱 동작은 JavaScript를 통해서도 스크립팅이 가능합니다. 게다가 QML은 Qt를 많이 사용하므로 QML 앱에서 타입 및 기타 Qt 기능에 직접 접근할 수 있습니다.

이 레퍼런스 가이드는 QML 언어의 기능에 대해 설명합니다. 이 가이드에 있는 많은 QML 타입들은 [Qt QML](https://doc.qt.io/qt-6/qtqml-index.html) 또는 [Qt Quick](https://doc.qt.io/qt-6/qtquick-index.html) 모듈에 기원을 두고 있습니다.

#### QML 구문 기초

QML은 객체가 애트리뷰트 및 다른 객체의 변경사항과의 관계와 응답하는 방식으로 정의될 수 있게 해주는 다중 패러다임 언어입니다. 애트리뷰트와 동작의 변경사항이 단계적으로 처리되는 일련의 명령문들을 통해 표현되는 순수한 명령형 코드와 달리 QML의 선언적 구문은 애트리뷰트와 동작 변경사항을 개별 객체의 정의에 직접 통합합니다. 이러한 애트리뷰트 정의에는 복잡한 커스텀 앱 동작이 필요한 경우 명령형 코드가 포함될 수 있습니다.

QML 소스 코드는 일반적으로 QML 코드의 독립형 문서인 QML 문서를 통해 엔진에 의해 로드됩니다. 이를 사용하여 앱 전체에서 재사용할 수 있는 QML 객체 타입을 정의할 수 있습니다. QML 파일에서 QML 객체 타입으로 선언하려면 타입 이름은 반드시 대문자로 시작해야 합니다.

##### import 구문

QML 문서는 파일의 최상단에 1개 이상의 import 구문을 갖고 있습니다. import는 다음 중 하나가 될 수 있습니다:

- 타입이 등록된 버전이 부여된 네임스페이스 (예. 플러그인에 의해)
- QML 문서 형태의 타입-정의를 포함하는 상대 경로 디렉토리
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

위의 객체는 [QML 문서](https://doc.qt.io/qt-6/qtqml-documents-topic.html)의 일부인 경우 엔진에 의해 로드될 수 있습니다. 즉, ([Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 타입을 이용할 수 있도록) 소스 코드에 QtQuick 모듈을 가져오는 import 구문을 추가하면 아래와 같습니다:

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

엔진이 이 코드를 로드하면, 이것은 루트가 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 객체인 객체 트리를 생성합니다; 이 객체는 [Gradient](https://doc.qt.io/qt-6/qml-qtquick-gradient.html) 자식 객체를 가지고 있습니다. 또 이것은 2개의 [GradientStop](https://doc.qt.io/qt-6/qml-qtquick-gradientstop.html) 자식을 갖고 있습니다.

그러나 이것은 시각적 장면의 문맥 QML 자식 트리의 문맥 상에서의 부모-자식 관계라는 점을 유의하십시오. 대부분의 QML 객체들이 시각적으로 렌더링되기 때문에 시각적 장면에서의 부모-자식 관계의 개념은 대부분의 QML 타입에 대한 베이스 타입인 QtQuick 모듈의 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) 타입에 의해 제공됩니다. 예를 들어, [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)과 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html)는 모두 [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html)-기반 타입이며, 아래의 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 객체는 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 객체의 시각적 자식으로 선언되었습니다:

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

[Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 객체가 위의 코드에서 [부모](https://doc.qt.io/qt-6/qml-qtquick-item.html#parent-prop) 값을 참조할 때, 객체 트리의 부모가 아닌 시각적 부모를 참조합니다. 이 경우 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 객체는 QML 객체 트리에서나 시각적 장면의 문맥에서나 [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 객체의 부모입니다. 그러나 시각적 부모를 변경하기 위해 [부모](https://doc.qt.io/qt-6/qml-qtquick-item.html#parent-prop) 프로퍼티를 수정할 수는 있어도 객체 트리의 문맥 상에서 QML로부터 객체의 부모를 변경할 수는 없습니다.

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

QML 문서에서 [객체 선언](https://doc.qt.io/qt-6/qtqml-syntax-basics.html#object-declarations)은 새로운 타입을 정의합니다. 또한 인스턴스화될 객체 계층을 선언합니다. 새로 정의된 타입의 인스턴스가 생성되어야 합니다.

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

객체는 선언된 컴포넌트 범위 안이라면 어디서든지 id로 참조할 수 있습니다. 그러므로 id 값은 컴포넌트 범위 안에서 항상 유일해야 합니다. 더 자세한 것은 [범위 및 네이밍 규칙](https://doc.qt.io/qt-6/qtqml-documents-scope.html)을 보십시오.

일단 객체 인스턴스가 생성되면, id 애트리뷰트의 값은 바꿀 수 없습니다. 이것은 일반 프로퍼티처럼 보일 수 있지만 id 애트리뷰트는 일반 프로퍼티 애트리뷰트가 아니며 특별한 의미가 적용됩니다. 예를 들어 위의 예제에서는 myTextInput.id에 접근할 수 없습니다.

##### 프로퍼티 애트리뷰트

프로퍼티는 정적인 값을 할당하거나 동적 표현식에 결합할 수 있는 객체의 애트리뷰트입니다. 프로퍼티의 값은 다른 객체가 읽을 수 있습니다. 일반적으로 특정 QML 타입이 특정 프로퍼티에 대해 이것을 명시적으로 허용하지 않는 한 다른 객체에 의해 수정될 수 있습니다.

* 프로퍼티 애트리뷰트 정의하기

C++에서 클래스에 Q_PROPERTY를 등록하여 QML type 시스템에 등록함으로써 프로퍼티는 타입으로 정의될 수 있습니다. 또는 다음 구문을 사용하여 QML 문서에서 객체 타입의 커스텀 프로퍼티를 객체 선언에서 정의할 수도 있습니다.

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

기본적으로 지원되는 프로퍼티의 타입 목록은 [QML 값 타입](https://doc.qt.io/qt-6/qtqml-typesystem-valuetypes.html)를 보십시오. 게다가 사용 가능한 [QML 객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html)은 프로퍼티 타입으로도 사용할 수 있습니다.

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

aliasing 프로퍼티가 기존 프로퍼티와 같은 이름을 갖는 것이 가능하므로 사실상 기존 프로퍼티를 덮어쓸 수 있습니다. 예를 들어, 다음 QML 타입은 내장된 [Rectangle::color](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html#color-prop) 프로퍼티와 동일한 이름을 가진 color 별명 프로퍼티를 갖고 있습니다:

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

클래스의 [Q_SIGNAL](https://doc.qt.io/qt-6/qobject.html#Q_SIGNAL)을 등록하고 QML 타입 시스템에 등록함으로써 C++에서 타입에 대한 시그널을 정의할 수 있습니다. 아니면 다음과 같은 구문처럼 객체 타입에 대한 커스텀 시그널은 QML 문서의 객체 선언에서 정의할 수도 있습니다.

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

시그널 사용에 대한 자세한 내용은 [시그널과 핸들러 이벤트 시스템](https://doc.qt.io/qt-6/qtqml-syntax-signals.html)을 보십시오.

* 프로퍼티 변경 시그널 핸들러

프로퍼티 변경 시그널에 대한 시그널 핸들러는 on<Property>Changed 형태의 구문을 갖습니다. 여기서 <Property>는 프로퍼티의 이름이며 첫 글자는 대문자입니다. 예를 들면, [TextInput](https://doc.qt.io/qt-6/qml-qtquick-textinput.html) type 문서는 TextChanged 시그널을 문서화하지 않지만 [TextInput](https://doc.qt.io/qt-6/qml-qtquick-textinput.html)이 [text](https://doc.qt.io/qt-6/qml-qtquick-textinput.html#text-prop) 프로퍼티를 가지고 있고 이 프로퍼티가 변경될 때마다 호출되는 OnTextChanged 시그널 핸들러를 작성할 수 있으므로 이 시그널은 암묵적으로 이용 가능합니다:

```qml
import QtQuick 2.0

TextInput {
    text: "Change this!"

    onTextChanged: console.log("Text has changed to:", text)
}
```

##### 메서드 애트리뷰트

객체 타입의 메서드는 연산 처리 혹은 트리거 이벤트 등을 수행하기 위해 호출될 수 있는 함수입니다. 메서드는 시그널과 연결될 수 있기 때문에 시그널이 방출될 때마다 자동으로 호출됩니다. 자세한 것은 [시그널과 핸들러 이벤트 시스템](https://doc.qt.io/qt-6/qtqml-syntax-signals.html)을 보십시오.

* 메서드 애트리뷰트 정의하기

C++에서 클래스의 함수에 [Q_INVOKABLE](https://doc.qt.io/qt-6/qobject.html#Q_INVOKABLE)을 태깅하여 QML 타입 시스템에 등록하거나, 클래스의 [Q_SLOT](https://doc.qt.io/qt-6/qobject.html#Q_SLOT)에 등록하여 메서드를 타입으로 정의할 수 있습니다. 아니면 다음과 같은 구문으로 QML 문서에서 커스텀 메서드를 객체 선언에 추가할 수도 있습니다:

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

[TapHandler](https://doc.qt.io/qt-6/qml-qtquick-taphandler.html) 문서에서 이름이 onPressedChanged인 시그널 핸들러가 문서화되지는 않았지만, 이 시그널은 pressed 프로퍼티가 존재한다는 사실에 의해 암시적으로 제공됩니다.

* 시그널 파라미터

시그널은 파라미터를 가지고 있을 수 있습니다. 이러한 파라미터에 접근하려면 핸들러에 함수를 할당해야 합니다. 화살표 함수와 익명 함수 모두 작동합니다.

다음 예제에서는 errorOccurred 시그널이 있는 Status 컴포넌트를 고려합니다. (QML 컴포넌트에 시그널을 추가하는 방법에 대한 자세한 내용은 [커스텀 QML 타입에 시그널 추가하기](https://doc.qt.io/qt-6/qtqml-syntax-signals.html#adding-signals-to-custom-qml-types)를 보십시오)

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

커스텀 QML 타입에 대한 시그널을 작성하는 자세한 방법은 [시그널 애트리뷰트](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-attributes)를 보십시오.

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

QML에서 제공하는 [JavaScript 호스트 환경](https://doc.qt.io/qt-6/qtqml-javascript-hostenvironment.html)은 조건 연산자, 배열, 변수 설정 및 루프와 같은 유효한 표준 JavaScript 구성을 실행할 수 있습니다. [QML Global Object](https://doc.qt.io/qt-6/qtqml-javascript-qmlglobalobject.html)에는 표준 JavaScript 속성 외에도 UI를 작성하고 QML 환경과 상호 작용하는 것을 단순화하는 여러 helper 메서드가 포함되어 있습니다.

QML에서 제공하는 JavaScript 환경은 웹 브라우저에서의 환경보다 더 엄격합니다. 예를 들면, QML에서는 JavaScript 글로벌 객체의 멤버에 추가하거나 수정하는 것이 불가능합니다. 일반 JavaScript에서는 변수를 선언하지 않고 사용하여 우연히 이 작업을 수행할 수 있습니다. QML에서는 예외가 발생하므로 모든 로컬 변수를 명시적으로 선언해야 합니다. QML에서 실행되는 JavaScript 코드의 제한에 대한 전체 설명은 [JavaScript 환경 제한](https://doc.qt.io/qt-6/qtqml-javascript-hostenvironment.html#javascript-environment-restrictions)을 참조하십시오.

[QML 문서](https://doc.qt.io/qt-6/qtqml-documents-topic.html)의 여러 부분에서 JavaScript 코드가 포함되어 있을 수 있습니다:

1. [프로퍼티 바인딩](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html)의 본문입니다. 이 JavaScript 표현식은 QML 객체 [프로퍼티](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#property-attributes) 간의 관계를 설명합니다. 프로퍼티의 종속성이 변경되면 지정된 관계에 따라 프로퍼티가 자동으로 업데이트됩니다.
2. [시그널 핸들러](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#signal-attributes)의 본문입니다. 이러한 JavaScript 구문은 QML 객체가 연관된 시그널을 방출할 때마다 자동으로 연산됩니다.
3. [커스텀 메서드](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#method-attributes) 정의입니다. QML 객체의 본문 내에 정의된 JavaScript 함수는 해당 객체의 메서드가 됩니다.
4. 독립형 [JavaScript 리소스 (.js) 파일](https://doc.qt.io/qt-6/qtqml-javascript-imports.html)입니다. 이러한 파일은 실제로 QML 문서와는 별개이지만 QML 문서로 가져오는 것이 가능합니다. 가져온 파일 내에 정의된 함수와 변수는 프로퍼티 바인딩, 시그널 핸들러, 커스텀 메서드에서 사용할 수 있습니다.

* 프로퍼티 바인딩에서의 JavaScript

다음 예제에서 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)의 color 프로퍼티는 [TapHandler](https://doc.qt.io/qt-6/qml-qtquick-taphandler.html)의 pressed 프로퍼티에 의존합니다. 이 관계는 조건식을 사용하여 설명됩니다:

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

실제로 프로퍼티 바인딩 정의에서는 표현식의 결과가 프로퍼티에 할당할 수 있는 타입의 값이라면 (아무리 복잡해도) JavaScript 표현식을 사용할 수 있습니다. 여기에는 부작용도 포함됩니다. 그러나 복잡한 바인딩과 부작용은 코드의 성능, 가독성 및 유지보수성을 저하시킬 수 있기 때문에 권장하지 않습니다.

프로퍼티 바인딩을 정의하는 데는 2가지 방법이 있습니다: 가장 일반적인 것은 앞의 예제에서 [프로퍼티 초기화](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#value-assignment-on-initialization)에 나와 있습니다. 두 번째(그리고 훨씬 더 희귀한) 방법은 다음과 같이 명령형 JavaScript 코드로부터 [Qt.binding()](https://doc.qt.io/qt-6/qml-qtqml-qt.html#binding-method) 함수에서 반환된 함수를 프로퍼티에 할당하는 것입니다:

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

[프로퍼티 바인딩](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html)을 정의하는 방법에 대한 자세한 내용은 프로퍼티 바인딩 문서를 참조하고, 바인딩과 값 할당의 차이점에 대한 내용은 [프로퍼티 할당 대 프로퍼티 바인딩](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html#qml-javascript-assignment)에 대한 문서를 참조하십시오.

* 시그널 핸들러 내에서의 JavaScript

QML 객체 타입은 발생하는 특정 이벤트에 대한 반응으로 시그널을 방출할 수 있습니다. 이러한 시그널은 커스텀 프로그램 로직을 구현하기 위해 클라이언트에 의해 정의될 수 있는 시그널 핸들러 함수에 의해 처리될 수 있습니다.

Rectangle 타입으로 표현된 버튼이 [TapHandler](https://doc.qt.io/qt-6/qml-qtquick-taphandler.html)와 Text 라벨을 가지고 있다고 가정합시다. [TapHandler](https://doc.qt.io/qt-6/qml-qtquick-taphandler.html)는 사용자가 버튼을 누를 때 자신의 [tapped](https://doc.qt.io/qt-6/qml-qtquick-taphandler.html#tapped-signal) 시그널을 방출합니다. 클라이언트는 JavaScript 표현식을 사용하여 onTapped 핸들러에서 시그널에 반응할 수 있습니다. QML 엔진은 필요에 따라 핸들러에 정의된 이러한 JavaScript 표현식을 실행합니다. 일반적으로 시그널 핸들러는 다른 이벤트를 시작하거나 프로퍼티 값을 할당하기 위해 JavaScript 표현식에 바인딩됩니다.

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

시그널과 시그널 핸들러에 대한 더 많은 정보는 다음 주제를 참조하십시오:
- [시그널과 핸들러 이벤트 시스템](https://doc.qt.io/qt-6/qtqml-syntax-signals.html)
- [QML 객체 애트리뷰트](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html)

* 독립형 함수에서의 JavaScript

프로그램 로직은 또한 JavaScript 함수에서 정의할 수 있습니다. 이러한 함수는 QML 문서에서 (커스텀 메서드로) 인라인으로 정의되거나, 가져온 JavaScript 파일에서 외부적으로 정의될 수 있습니다.

* 커스텀 메서드에서의 JavaScript

커스텀 메서드는 QML 문서에서 정의할 수 있으며, 시그널 핸들러, 프로퍼티 바인딩 또는 다른 QML 객체의 함수에서 호출할 수 있습니다. 이러한 메서드는 외부 JavaScript 파일 대신 QML 객체 타입 정의(QML 문서)에 구현이 포함되어 있기 때문에 때로는 인라인 JavaScript 함수라고도 합니다.

인라인 커스텀 메서드의 예제는 다음과 같습니다:

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

[TapHandler](https://doc.qt.io/qt-6/qml-qtquick-taphandler.html)가 tapped 시그널을 방출할 때마다 피보나치 함수를 실행합니다.

주의: QML 문서에서 인라인으로 정의된 커스텀 메서드는 다른 객체에 노출되므로 QML 컴포넌트의 루트 객체에 대한 인라인 함수는 컴포넌트의 외부의 호출자에 의해 호출될 수 있습니다. 이것을 원치 않을 경우, 메서드는 루트가 아닌 객체에 추가되거나 가급적 외부 JavaScript 파일에 작성될 수 있습니다.

JavaScript를 사용하여 QML에서 커스텀 메서드를 정의하는 방법에 대한 자세한 내용은 [QML 객체 애트리뷰트](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html) 문서를 보십시오.

* JavaScript 파일에서 정의된 함수

중요한 프로그램 로직은 별도의 JavaScript 파일로 분리하는 것이 가장 좋습니다. 이 파일은 QML [모듈](https://doc.qt.io/qt-6/qtqml-modules-topic.html)과 같이 import 문을 사용하여 QML로 가져올 수 있습니다.

예를 들면, 앞의 예제의 fibonacci() 메서드를 fib.js라는 외부 파일로 이동하여 다음과 같이 접근할 수 있습니다:

```qml
import QtQuick 2.12
import "fib.js" as MathFunctions

Item {
    TapHandler {
        onTapped: console.log(MathFunctions.fibonacci(10))
    }
}
```

외부 JavaScript 파일을 QML로 읽어오는 방법에 대해 더 자세히 알려면, [QML에서 JavaScript 리소스 가져오기](https://doc.qt.io/qt-6/qtqml-javascript-imports.html) 섹션을 읽어 보십시오.

* 시그널과 JavaScript 함수 연결하기

[이전](https://doc.qt.io/qt-6/qtqml-javascript-expressions.html#javascript-in-signal-handlers) 섹션에서 설명한 바와 같이, 시그널을 방출하는 QML 객체 타입은 또한 자신의 시그널에 대한 기본 시그널 핸들러를 제공합니다. 그러나 때때로, 클라이언트는 다른 QML 객체가 신호를 방출할 때 QML 객체에 정의된 함수를 트리거하기를 원합니다. 이러한 시나리오는 시그널 연결에 의해 처리될 수 있습니다.

QML 객체가 방출하는 시그널은 시그널의 connect() 메서드를 호출하여 JavaScript 함수를 인수로 전달하여 JavaScript 함수에 연결할 수 있습니다. 예를 들면, 다음 코드는 [TapHandler](https://doc.qt.io/qt-6/qml-qtquick-taphandler.html)의 tapped 시그널을 script.js의 jsFunction()에 연결합니다:

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

jsFunction()은 [TapHandler](https://doc.qt.io/qt-6/qml-qtquick-taphandler.html)의 tapped 시그널이 방출될 때마다 호출됩니다.

더 자세한 정보는 [시그널을 메서드/시그널과 연결하기](https://doc.qt.io/qt-6/qtqml-syntax-signals.html)을 보십시오

* 앱 시작 코드에서의 JavaScript

때때로 앱 (또는 컴포넌트 인스턴스) 시작 시 일부 명령형 코드를 실행해야 합니다. 시작 스크립트를 외부 스크립트 파일에 글로벌 코드로 포함시키고 싶은 유혹이 들겠지만 QML 환경이 완전히 구축되지 않았을 수 있기 때문에 심각한 제한이 있을 수 있습니다. 예를 들면, 일부 객체가 생성되지 않았거나 일부 [프로퍼티 바인딩](https://doc.qt.io/qt-6/qtqml-syntax-propertybinding.html)이 이루어지지 않았을 수 있습니다. 글로벌 스크립트 코드의 정확한 제한사항에 대한 것은 [자바스크립트 환경 제한](https://doc.qt.io/qt-6/qtqml-javascript-hostenvironment.html#javascript-environment-restrictions)을 참조하십시오.

QML 객체는 인스턴스화가 완료되면 Component.completed [부착 시그널](https://doc.qt.io/qt-6/qtqml-syntax-signals.html#attached-signal-handlers)을 방출합니다. 해당 Component.onCompleted 핸들러의 JavaScript 코드는 객체가 인스턴스화된 후에 실행됩니다. 따라서 이 객체는 QML 환경이 완전히 구축되면 Component.completed를 방출하므로 최상위 객체의 Component.onCompleted 핸들러에서 앱 시작 코드를 쓰기에 가장 적합한 위치입니다.

예제:

```qml
import QtQuick 2.0

Rectangle {
    function startupFunction() {
        // ... 시작 코드
    }

    Component.onCompleted: startupFunction();
}
```

중첩 객체 및 중첩 QML 컴포넌트 인스턴스를 포함한 - QML 파일의 모든 객체는 이 첨부된 프로퍼티를 사용할 수 있습니다. 시작할 때 실행할 onCompleted() 핸들러가 2개 이상 있으면 정의되지 않은 순서로 순차적으로 실행됩니다.

마찬가지로 모든 컴포넌트는 파괴되기 직전에 destruction() 시그널을 방출합니다.


##### JavaScript에서 동적 QML 객체 생성

QML은 JavaScript 내에서 객체의 동적 생성을 지원합니다. 이는 필요할 때까지 객체의 인스턴스화를 지연시켜 앱 시작 시간을 향상시키는 데 유용합니다. 또한 사용자 입력 또는 다른 이벤트에 반응하여 시각적 객체를 동적으로 생성하고 장면에 추가할 수 있습니다.

이 페이지에서 설명한 개념에 대한 시연은 [동적 장면 예제](https://doc.qt.io/qt-6/qtqml-dynamicscene-example.html)를 보십시오.

* 동적으로 객체 생성하기

JavaScript에서 동적으로 객체를 만드는 방법은 2가지가 있습니다. [Qt.createComponent()](https://doc.qt.io/qt-6/qml-qtqml-qt.html#createComponent-method)를 호출하여 동적으로 [Component](https://doc.qt.io/qt-6/qml-qtqml-component.html) 객체를 만들거나 [Qt.createQmlObject()](https://doc.qt.io/qt-6/qml-qtqml-qt.html#createQmlObject-method)를 사용하여 QML 문자열로 객체를 만들 수 있습니다. QML 문서에 정의된 기존 컴포넌트가 있고 해당 컴포넌트의 인스턴스를 동적으로 만들고 싶다면 컴포넌트를 만드는 것이 더 좋습니다. 그렇지 않으면 QML 문자열로 객체를 만드는 것은 런타임 시에 객체 QML 자체가 생성될 때 유용합니다.

* 동적으로 컴포넌트 생성하기

QML 파일에 정의된 컴포넌트를 동적으로 로드하려면 [Qt 객체](https://doc.qt.io/qt-6/qml-qtqml-qt.html)에서 [Qt.createComponent()](https://doc.qt.io/qt-6/qml-qtqml-qt.html#createComponent-method) 함수를 호출합니다. 이 함수는 QML 파일의 URL을 유일한 인수로 사용하고 이 URL에서 [Component](https://doc.qt.io/qt-6/qml-qtqml-component.html) 객체를 만듭니다.

일단 [Component](https://doc.qt.io/qt-6/qml-qtqml-component.html)를 갖게 되면, [createObject()](https://doc.qt.io/qt-6/qml-qtqml-component.html#createObject-method) 메서드를 호출하여 컴포넌트의 인스턴스를 만들 수 있습니다. 이 함수는 1개 또는 2개 인수를 가질 수 있습니다:

- 1번째는 새로운 객체에 대한 부모입니다. 부모 객체는 그래픽 객체(예: [Item](https://doc.qt.io/qt-6/qml-qtquick-item.html) 타입) 또는 비-그래픽 객체(예: [QtObject](https://doc.qt.io/qt-6/qml-qtqml-qtobject.html) 또는 C++ [QOobject](https://doc.qt.io/qt-6/qobject.html) 타입)일 수 있습니다. 그래픽 부모 객체가 있는 그래픽 객체만 [Qt Quick](https://doc.qt.io/qt-6/qtquick-index.html) 시각적 canvas로 렌더링됩니다. 부모 객체를 나중에 설정하려면 이 함수에 null을 안전하게 전달할 수 있습니다.
- 2번째는 선택사항으로 객체에 대한 초기 프로퍼티 값을 정의하는 프로퍼티-값 쌍의 맵입니다. 이 인수에 의해 지정된 프로퍼티 값은 생성이 완료되기 전에 객체에 적용되므로 다른 프로퍼티 바인딩을 활성화하기 위해 특정 프로퍼티를 초기화해야 할 경우 발생할 수 있는 바인딩 오류를 피할 수 있습니다. 또한 객체가 생성된 후 프로퍼티 값 및 바인딩을 정의하는 것과 비교할 때 성능 면에서 약간의 이점이 있습니다.

다음은 예제입니다. 먼저 간단한 QML 컴포넌트를 정의하는 Sprite.qml이 있습니다:

```qml
import QtQuick 2.0

Rectangle { width: 80; height: 50; color: "red" }
```

componentCreation.js JavaScript 파일을 가져오는 앱 파일 main.qml은 Sprite 객체를 생성할 것입니다:

```qml
import QtQuick 2.0
import "componentCreation.js" as MyScript

Rectangle {
    id: appWindow
    width: 300; height: 300

    Component.onCompleted: MyScript.createSpriteObjects();
}
```

다음은 componentCreation.js입니다. QML 파일이 네트워크를 통해 로드되어 즉시 준비되지 않은 경우 [createObject()](https://doc.qt.io/qt-6/qml-qtqml-component.html#createObject-method)를 호출하기 전에 컴포넌트 [상태](https://doc.qt.io/qt-6/qml-qtqml-component.html#status-prop)가 Component.Ready인지 여부를 확인합니다.

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
            // 오류 처리
            console.log("Error creating object");
        }
    } else if (component.status == Component.Error) {
        // 오류 처리
        console.log("Error loading component:", component.errorString());
    }
}
```

만약 로드할 QML 파일이 로컬 파일인 경우, finishCreation() 함수를 생략하고 [createObject()](https://doc.qt.io/qt-6/qml-qtqml-component.html#createObject-method)를 즉시 호출할 수 있습니다:

```qml
function createSpriteObjects() {
    component = Qt.createComponent("Sprite.qml");
    sprite = component.createObject(appWindow, {x: 100, y: 100});

    if (sprite == null) {
        // 오류 처리
        console.log("Error creating object");
    }
}
```

동적으로 생성된 객체는 시각적(Qt Quick) 객체이므로, 두 경우 모두에서 [createObject()](https://doc.qt.io/qt-6/qml-qtqml-component.html#createObject-method)는 부모 인수로 전달된 appWindow와 함께 호출됩니다. 생성된 객체는 main.qml의 appWindow 객체의 자식이 되어 장면에 나타납니다.

상대 경로를 가진 파일을 사용할 때, 경로는 [Qt.createComponent()](https://doc.qt.io/qt-6/qml-qtqml-qt.html#createComponent-method)가 실행되는 파일과 상대적이어야 합니다.

동적으로 생성된 객체에 시그널을 연결하려면, 혹은 객체로부터 시그널을 수신하려면 시그널 connect() 메서드를 사용하십시오. 자세한 내용은 [시그널을 메서드/시그널과 연결하기](https://doc.qt.io/qt-6/qtqml-syntax-signals.html#connecting-signals-to-methods-and-signals)를 보십시오.

[incubateObject()](https://doc.qt.io/qt-6/qml-qtqml-component.html#incubateObject-method) 함수를 통해 차단 없이 컴포넌트를 인스턴스화할 수도 있습니다.

* QML 문자열로부터 객체 생성하기

QML이 런타임까지 정의되지 않으면 다음 예제와 같이 [Qt.createQmlObject()](https://doc.qt.io/qt-6/qml-qtqml-qt.html#createQmlObject-method) 함수를 사용하여 QML 문자열에서 QML 객체를 만들 수 있습니다:

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

1번째 인수는 생성할 QML 문자열입니다. 새 파일과 마찬가지로 사용할 타입을 가져와야 합니다. 2번째 인수는 새 객체의 부모 객체이며 컴포넌트에 적용되는 부모 인수 시맨틱스도 createQmlObject()에 유사하게 적용됩니다. 3번째 인수는 새 객체와 연결할 파일 경로입니다; 이는 오류 보고에 사용됩니다.

QML 문자열이 상대 경로를 사용하여 파일을 가져오는 경우, 경로는 (메서드의 2번째 인수인) 부모 객체가 정의된 파일에 상대적이어야 합니다.

중요: 정적 QML 앱을 구축할 때 QML 파일을 검색하여 import 종속성을 탐지합니다. 이렇게 하면 컴파일 시 필요한 모든 플러그인과 리소스가 해결됩니다. 그러나 (QML 파일의 최상단에서 발견되는) 명시적인 import 문만 고려되고 문자열 리터럴 내에 포함된 import 문은 고려되지 않습니다. 따라서 정적 빌드를 지원하려면 [Qt.createQmlObject()](https://doc.qt.io/qt-6/qml-qtqml-qt.html#createQmlObject-method)를 사용하는 QML 파일에 문자열 리터럴 내부 외에도 파일 최상단에 필요한 모든 import가 명시적으로 포함되어 있는지 확인해야 합니다.

* 동적으로 생성된 객체 유지보수하기

동적으로 생성된 객체를 관리할 때 생성 컨텍스트가 생성된 객체보다 오래 사용되는지 확인해야 합니다. 그렇지 않으면 생성 컨텍스트가 먼저 제거되었을 때 동적 객체의 바인딩과 시그널 핸들러가 더 이상 작동하지 않습니다.

실제 생성 컨텍스트는 객체를 생성하는 방법에 따라 달라집니다:
- 만약 [Qt.createComponent()](https://doc.qt.io/qt-6/qml-qtqml-qt.html#createComponent-method)가 사용되면, 생성 컨텍스트는 이 메서드가 호출된 [QQmlContext](https://doc.qt.io/qt-6/qqmlcontext.html)입니다.
- 만약 [Qt.createQmlObject()](https://doc.qt.io/qt-6/qml-qtqml-qt.html#createQmlObject-method)가 호출되면, 생성 컨텍스트는 이 메서드에게 전달된 부모 객체의 컨텍스트입니다.
- 만약 Component{} 객체가 정의되어 있고 해당 객체에서 [createObject()](https://doc.qt.io/qt-6/qml-qtqml-component.html#createObject-method) 또는 [incubateObject()](https://doc.qt.io/qt-6/qml-qtqml-component.html#incubateObject-method)가 호출되면, 생성 컨텍스트는 Component가 정의된 컨텍스트입니다.

또한 동적으로 생성된 객체는 다른 객체와 동일하게 사용될 수 있지만 QML에 id가 없다는 것을 주의하십시오.

* 동적으로 객체 삭제하기

많은 사용자 인터페이스에서 시각적 객체의 불투명도를 0으로 설정하거나, 시각적 객체를 삭제하는 대신 화면 밖으로 이동하면 충분하지만, 동적으로 생성된 객체가 많은 경우 사용하지 않은 객체를 삭제하면 성능이 더 좋아질 수 있습니다.

([Loader](https://doc.qt.io/qt-6/qml-qtquick-loader.html)와 [Repeater](https://doc.qt.io/qt-6/qml-qtquick-repeater.html) 같은) 편리한 QML 객체 공장에서 동적으로 생성된 객체를 수동으로 삭제해서는 안 됩니다. 또한 동적으로 직접 생성하지 않은 객체를 삭제하는 것도 피해야 합니다.

destroy() 메서드를 사용하여 Item을 삭제할 수 있습니다. 이 메서드에는 객체를 제거하기 전의 대략적인 지연 시간(밀리초)을 지정하는 선택적인 인수(기본값은 0)가 있습니다.

다음은 예제입니다. application.qml은 SelfDestroyingRect.qml 컴포넌트의 인스턴스를 5개 생성합니다. 각 인스턴스는 [NumberAnimation](https://doc.qt.io/qt-6/qml-qtquick-numberanimation.html)을 실행하고 애니메이션이 완료되면 루트 객체에서 destroy()를 호출하여 자신을 파괴합니다:

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

또는 application.qml이 object.destroy()를 호출하여 생성된 객체를 제거했을 수도 있습니다.

객체 안에 있는 객체에서 destroy()를 호출하는 것이 안전합니다. 객체는 destroy()를 호출하는 즉시 제거되지 않고 스크립트 블록의 끝과 다음 프레임 사이에서 정리됩니다. (당신이 0이 아닌 지연 시간을 지정하지 않았을 경우)

또한 다음과 같이 SelfDestroyingRect 인스턴스가 정적으로 생성된 경우를 주의하십시오:

```qml
Item {
    SelfDestroyingRect {
        // ...
    }
}
```

객체를 동적으로 생성한 경우에만 동적으로 제거할 수 있으므로 오류가 발생할 수 있습니다.

[Qt.createQmlObject()](https://doc.qt.io/qt-6/qml-qtqml-qt.html#createQmlObject-method)를 사용하여 생성된 객체도 destroy()를 사용하여 파괴될 수 있습니다:

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

QML 앱을 위한 프로그램 로직은 JavaScript로 정의될 수 있습니다. JavaScript 코드는 QML 문서에서 인라인으로 정의되거나, (QML에서 JavaScript 리소스로 알려진) JavaScript 파일로 분리될 수 있습니다.

QML에서 지원되는 JavaScript 리소스는 두 종류가 있습니다: 코드-비하인드 구현 파일, 공유 (라이브러리) 파일. 두 가지 종류의 JavaScript 리소스는 다른 JavaScript 리소스에서 [가져오거나](https://doc.qt.io/qt-6/qtqml-javascript-imports.html) [QML 모듈](https://doc.qt.io/qt-6/qtqml-modules-topic.html)에 포함될 수 있습니다.

* 코드-비하인드 구현 리소스

QML 문서로 가져온 대부분의 JavaScript 파일은 이 파일을 가져온 QML 문서에 대한 stateful 구현입니다. 이러한 경우, 문서에 정의된 QML 객체 타입의 각 인스턴스는 올바르게 동작하기 위해 JavaScript 객체와 상태의 개별 복사본을 필요로 합니다.

JavaScript 파일을 가져올 때 기본 동작은 각 QML 컴포넌트 인스턴스에 고유하고 격리된 복사본을 제공하는 것입니다. 만약 해당 JavaScript 파일이 .import 문이 있는 리소스 또는 모듈을 가져오지 않으면 해당 코드는 QML 컴포넌트 인스턴스와 동일한 범위에서 실행되며 결과적으로 해당 QML 컴포넌트에 선언된 객체 및 프로퍼티에 접근하고 조작할 수 있습니다. 그렇지 않으면 고유한 범위를 갖고 있을 것이며 QML 컴포넌트의 객체 및 프로퍼티는 필요한 경우 파라미터로 JavaScript 파일의 함수에 전달되어야 합니다.

코드-비하인드 구현 리소스의 예제는 다음과 같습니다:

```qml
// MyButton.qml
import QtQuick 2.0
import "my_button_impl.js" as Logic // 이 JavaScript 리소스의 새로운 인스턴스는 Button.qml의 각 인스턴스에 대하여 로드됨

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
var clickCount = 0;   // 이 상태(state)는 MyButton의 각 인스턴스에 대하여 분리되어 있음
function onClicked(button) {
    clickCount += 1;
    if ((clickCount % 5) == 0) {
        button.color = Qt.rgba(1,0,0,1);
    } else {
        button.color = Qt.rgba(0,1,0,1);
    }
}
```

일반적으로 단순 로직은 QML 파일에 인라인으로 정의되어야 하지만, 보다 복잡한 로직은 유지보수성과 가독성을 위해 코드-비하인드 구현 리소스로 분리되어야 합니다.

* 공유 JavaScript 리소스 (라이브러리)

기본적으로 QML에서 가져온 JavaScript 파일은 QML 컴포넌트와 컨텍스트를 공유합니다. 이는 JavaScript 파일이 동일한 QML 개체에 접근할 수 있고 수정할 수 있음을 의미합니다. 따라서 각 import는 이러한 파일의 고유한 복사본이 있어야 합니다.

[이전 섹션](https://doc.qt.io/qt-6/qtqml-javascript-resources.html#code-behind-implementation-resource)에서는 JavaScript 파일의 stateful import에 대해 이야기했습니다. 그러나 일부 JavaScript 파일은 stateless이며, 가져온 위치에서 아무것도 필요하지 않은 helper 함수 집합을 제공한다는 점에서 재사용 가능한 라이브러리와 유사하게 작용합니다. 다음 예제와 같이 이러한 라이브러리를 특수 pragma로 표시하면 상당한 양의 메모리를 절약하고 QML 컴포넌트의 인스턴스화 속도를 높일 수 있습니다.

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

pragma 선언은 반드시 코멘트를 제외한 모든 JavaScript 코드 앞에 나타나야 합니다.

여러 QML 문서는 "factorial.js"를 가져올 수 있으며, 제공하는 factorial 및 factorialCallCount 함수를 호출할 수 있습니다. JavaScript import의 state는 이를 가져온 QML 문서 전반에 걸쳐 공유되므로, factorial 함수를 호출하지 않는 QML 문서 내에서 호출될 때 factorialCallCount 함수의 반환 값은 0이 아닐 수 있습니다.

예를 들면:

```qml
// Calculator.qml
import QtQuick 2.0
import "factorial.js" as FactorialCalculator // 이 JavaScript 리소스는 Calculator.qml의 인스턴스가 여러 개 생성되더라도 엔진에 의해 한 번만 로드됨

Text {
    width: 500
    height: 100
    property int input: 17
    text: "The factorial of " + input + " is: " + FactorialCalculator.factorial(input)
}
```

.pragma 라이브러리 파일은 공유되므로 QML 값을 함수 파라미터로 전달할 수 있지만 QML 컴포넌트 인스턴스 개체 또는 프로퍼티에 직접 접근할 수 없습니다.


##### QML에서 JavaScript 리소스 가져오기

[JavaScript 리소스](https://doc.qt.io/qt-6/qtqml-javascript-resources.html)는 QML 문서 및 다른 JavaScript 리소스에 의해 import 될 수 있습니다. JavaScript 리소스는 상대/절대 URL을 통해 가져올 수 있습니다. 상대적인 URL의 경우, import를 포함하는 [QML 문서](https://doc.qt.io/qt-6/qtqml-documents-topic.html) 또는 [JavaScript 리소스](https://doc.qt.io/qt-6/qtqml-javascript-resources.html)의 위치에 대해 상대적인 위치로 결정됩니다. 만약 스크립트 파일에 접근할 수 없는 경우 오류가 발생합니다. 만약 네트워크 리소스에서 JavaScript를 가져와야 하는 경우, 스크립트가 다운로드될 때까지 컴포넌트의 [상태](https://doc.qt.io/qt-6/qqmlcomponent.html#status-prop)는 "Loading"으로 설정됩니다.

JavaScript 리소스는 또한 QML 모듈 및 기타 JavaScript 리소스를 가져올 수 있습니다. JavaScript 리소스 내의 import 문의 구문은 QML 문서 내의 import 문과 약간 다릅니다. 이는 맨 아래에 문서화되어 있습니다.

* QML 문서로부터 JavaScript 리소스 가져오기

QML 문서는 다음 구문을 이용하여 JavaScript 리소스를 가져올 수 있습니다:

```qml
import "ResourceURL" as Qualifier
```

예를 들면:

```qml
import "jsfile.js" as Logic
```

가져온 JavaScript 리소스는 항상 "as" 키워드를 사용하여 한정됩니다. JavaScript 리소스의 한정자(qualifier)는 대문자로 시작해야 하며 유일해야 하므로 한정자와 JavaScript 파일 간에는 항상 1:1 매핑이 있습니다. (이것은 또한 한정자의 이름을 Date, Math와 같은 내장 JavaScript 객체와 동일하게 지정할 수 없음을 의미합니다)

가져온 JavaScript 파일에 정의된 함수는 "Qualifier.functionName(params)" 구문을 통해 가져온 QML 문서에 정의된 객체에 사용할 수 있습니다. JavaScript 리소스의 함수는 일반 JavaScript 타입뿐만 아니라 임의의 QML 값 타입 또는 객체 타입이 될 수 있는 파라미터를 사용할 수 있습니다. 일반 [데이터 타입 변환 규칙](https://doc.qt.io/qt-6/qtqml-cppintegration-data.html)은 QML에서 이러한 함수를 호출할 때 파라미터와 리턴 값에 적용됩니다.

* JavaScript 리소스 안에서 가져오기

QtQuick 2.0에서는 JavaScript 리소스가 표준 QML import 구문의 변형(앞에서 설명한 모든 규칙과 자격이 적용되는 경우)을 사용하여 다른 JavaScript 리소스와 QML 타입 네임스페이스를 가져올 수 있도록 지원이 추가되었습니다.

JavaScript 리소스가 QtQuick 2.0에서 이러한 방식으로 다른 JavaScript 또는 QML 모듈을 가져올 수 있기 때문에 다음과 같은 몇 가지 추가 시맨틱스가 정의됩니다:
- import 구문이 포함된 스크립트는 가져온 QML 문서로부터 import를 상속하지 않게 됩니다. (예: Component.errorString 접근은 실패하게 됨)
- import 구문이 없는 스크립트는 가져온 QML 문서로부터 import를 상속하게 됩니다. (예: Component.errorString 접근은 성공하게 됨)
- 공유 스크립트(즉, .pragma 라이브러리로 정의됨)는 다른 스크립트나 모듈을 가져오지 않더라도 QML 문서의 import를 상속하지 않습니다.

하나의 특정 스크립트를 임의의 수의 QML 파일이 가져올 수 있다는 점을 고려할 때, 1번째 시맨틱은 개념적으로 정확합니다. 2번째 시맨틱은 하위 호환성의 목적을 위해 유지됩니다. 3번째 시맨틱은 공유 스크립트에 대한 현재 시맨틱과 차이점이 없지만 새로 가능한 경우(스크립트가 다른 스크립트/모듈을 가져오는 경우)와 관련하여 여기에서 명확하게 설명됩니다.

* 다른 JavaScript 리소스에서 JavaScript 리소스 가져오기

JavaScript 리소스는 다음과 같은 방법으로 다른 리소스를 가져올 수 있습니다:

```qml
import * as MathFunctions from "factorial.mjs";
```

또는:

```qml
.import "filename.js" as Qualifier
```

전자는 ECMAScript 모듈을 가져오기 위한 표준 ECMAScript 구문이며 mjs 파일 확장자로 표시된 것처럼 ECMAScript 모듈 내에서만 작동합니다. 후자는 QML 엔진에서 제공하는 JavaScript의 확장자이며 비-모듈에서도 작동합니다. ECMAScript 표준으로 대체된 확장자로서 사용이 권장되지 않습니다.

이러한 방식으로 JavaScript 파일을 가져오면 한정자(qualifer)를 사용하여 가져올 수 있습니다. 그런 다음 한정자를 통해 해당 파일의 기능에 접근할 수 있습니다. (즉, Qualifier.functionName(params))

때때로 한정자를 사용하지 않고 가져오기 컨텍스트에서 함수를 사용할 수 있도록 하는 것이 좋습니다. 이 경우 ECMAScript 모듈 및 JavaScript import 문을 한정자로 사용하지 않고 사용해야 합니다.

예를 들면, 1번째 QML 코드는 script.mjs의 showCalculations()을 호출하고, 2번째 코드에서는 import를 사용하여 가져온 factorial.mjs의 factorial()을 호출합니다.

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

[Qt.include()](https://doc.qt.io/qt-6/qml-qtqml-qt-obsolete.html#include-method) 함수는 ECMAScript 모듈을 사용하지 않고 import를 한정(qualify)하지 않고 다른 JavaScript 파일로부터 하나의 JavaScript 파일을 가져옵니다. 현재 파일의 네임스페이스에서 다른 파일의 모든 함수와 변수를 사용할 수 있게 만들지만 해당 파일에 정의된 모든 pragma와 import는 무시합니다. 함수 호출은 호출자의 컨텍스트를 절대 수정해서는 안 되기 때문에 이것은 좋은 생각이 아닙니다.

[Qt.include()](https://doc.qt.io/qt-6/qml-qtqml-qt-obsolete.html#include-method)는 더 잇아 사용하지 않으므로 피해야 합니다. Qt의 향후 버전에서는 제거될 것입니다.

* JavaScript 리소스로부터 QML 모듈 가져오기

JavaScript 리소스는 다음과 같은 방식으로 QML 모듈을 가져옵니다:

```qml
.import TypeNamespace MajorVersion.MinorVersion as Qualifier
```

아래에서 JavScript에 가져온 모듈로부터 QML 타입을 사용하는 방법을 보여주는 예제를 볼 수 있습니다:

```qml
.import Qt.test 1.0 as JsQtTest

var importedEnumValue = JsQtTest.MyQmlObject.EnumValue3
```

특히 싱글톤(singleton) 타입을 통해 제공되는 함수에 접근하는 데 유용할 수 있습니다; 자세한 내용은 [QML_SINGLETON](https://doc.qt.io/qt-6/qqmlengine.html#QML_SINGLETON) 을 참조하십시오.

기본적으로 JavaScript 리소스는 리소스를 import하는 컴포넌트의 모든 import에 접근할 수 있습니다. (.pragma 라이브러리를 사용하는) stateless 라이브러리로 선언되거나 명시적인 .import 구문이 포함된 경우, 컴포넌트의 import에 접근할 수 없습니다.

주의: [WorkerScript](https://doc.qt.io/qt-6/qml-qtqml-workerscript-workerscript.html)에 사용되는 스크립트에는 .import 구문이 작동하지 않습니다.

[QML에서의 JavaScript 리소스 정의하기](https://doc.qt.io/qt-6/qtqml-javascript-resources.html)를 참조하십시오.


##### JavaScript 호스트 환경

QML은 QML 앱 작성에 맞춘 JavaScript 호스트 환경을 제공합니다. 이 환경은 브라우저가 제공하는 호스트 환경이나 Node.js와 같은 서버-측 JavaScript 환경과는 다릅니다. 예를 들어 QML은 브라우저 환경에서 흔히 볼 수 있는 윈도우 객체나 DOM API를 제공하지 않습니다.

* 공통 기초

QML 런타임은 브라우저 또는 서버-측 JavaScript 환경과 마찬가지로 [ECMAScript 언어 규격](https://www.ecma-international.org/publications-and-standards/standards/ecma-262/) 표준을 구현합니다. 이것은 Object, Array, Math 등 표준에 의해 정의된 모든 내장 타입 및 함수에 접근할 수 있습니다. QML 런타임은 표준의 7번째 버전을 구현합니다.

(Qt 5.15부터) [Nullish Coalescing](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator) (??) 그리고 (Qt 6.2부터) [Optional Chaining](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining) (?.) 역시 QML 런타임에 구현되어 있습니다.

표준 ECMAScript 내장형은 QML 문서에 명시적으로 문서화되어 있지 않습니다. 사용에 대한 자세한 정보는 ECMA-262 7판 표준 또는 [W3Schools JavaScript Reference](https://www.w3schools.com/jsref/default.asp) (JavScript Objects Reference 섹션)와 같은 많은 온라인 JavaScript 레퍼런스 및 튜토리얼 사이트 중 하나를 참조하십시오. 많은 사이트가 브라우저에서의 JavaScript에 초점을 맞추고 있으므로 주어진 함수나 객체가 표준 ECMAScript의 일부인지 브라우저 환경에 고유한지 여부를 결정하기 위해 사양을 두 번 확인해야 하는 경우가 있습니다. 위의 W3Schools 링크의 경우 JavaScript Objects Reference 섹션은 일반적으로 표준을 다루는 반면, 브라우저 객체 참조 및 HTML DOM 객체 참조 섹션은 브라우저 고유에 대한 표준을 다릅니다. (따라서 QML에 적용되지 않음)

* 타입 어노테이션(annotation) 및 어썰션(assertion)

QML 문서의 함수 선언은 타입 어노테이션을 포함할 수 있고 포함해야 합니다. 타입 어노테이션은 리턴 타입에 주석을 달기 위해 인수 선언과 함수 자체에 추가됩니다. 다음 함수는 int와 string 파라미터를 취하고 QtObject를 리턴합니다:

```qml
function doThings(a: int, b: string) : QtObject { ... }
```

타입 어노테이션은 코드를 이해하고 더 나은 진단을 제공하는 [Qt Creator](https://doc.qt.io/qt-6/qtquick-tools-and-utilities.html#qt-creator)와 [qmllint](https://doc.qt.io/qt-6/qtquick-tool-qmllint.html)와 같은 도구에게 도움을 줍니다. 또한 C++에서 함수를 사용하기가 더 쉽습니다. 자세한 내용은 [C++에서 QML 객체와 상호 작용하기](https://doc.qt.io/qt-6/qtqml-cppintegration-interactqmlfromcpp.html)를 보십시오.

기본적으로 타입 어노테이션은 인터프리터와 JIT 컴파일러에 의해 무시되지만 C++로 컴파일할 때 [qmlcachegen](https://doc.qt.io/qt-6/qtqml-tool-qmlcachegen.html)과 [qmlsc](https://doc.qt.io/qt-6/qtqml-qml-script-compiler.html)에 의해 강제됩니다. 이것은 실제로 선언된 타입이 아닌 값을 전달하거나 typed 인수로 전달된 [QML 값 타입](https://doc.qt.io/qt-6/qtqml-typesystem-valuetypes.html)의 인스턴스를 수정할 경우 동작에 차이가 발생할 수 있습니다. 값 타입은 인터프리터와 JIT에 의해 참조로 전달되지만 C++로 컴파일할 때는 값으로 전달됩니다.

당신은 인터프리터와 JIT가 타입 어노테이션도 존중하도록 강제하거나 [qmlcachegen](https://doc.qt.io/qt-6/qtqml-tool-qmlcachegen.html)과 [qmlsc](https://doc.qt.io/qt-6/qtqml-qml-script-compiler.html)가 타입 어노테이션을 무시하도록 하여 그러한 차이를 제거할 수 있습니다. 전자는 인터프리터나 JIT를 사용할 때 성능 비용이 있고, 후자는 C++로의 컴파일이 JavaScript 함수를 호출하는 바인딩과 시그널 핸들러를 피할 수 있도록 합니다. 따라서 더 적은 코드가 C++로 컴파일될 것입니다.

항상 타입 어노테이션을 적용하려면 QML 문서에 다음을 추가합니다:

```qml
pragma FunctionSignatureBehavior: Enforced
```

타입 어노테이션을 항상 무시하려면 대신에 다음을 추가하십시오:

```qml
pragma FunctionSignatureBehavior: Ignored
```

객체를 다른 객체 타입에 캐스팅하기 위해 타입 assertion(때로는 as-cast라고도 함)을 사용할 수도 있습니다. 만약 객체가 실제로 주어진 타입이면, 타입 assertion은 동일한 객체를 리턴합니다. 그렇지 않으면 null을 리턴합니다. 다음 토막글에서 우리는 부모 객체가 특정 멤버에 접근하기 전에 Rectangle인지 여부를 assert합니다.

```qml
Item {
    property color parentColor: (parent as Rectangle)?.color || "red"
}
```

optional chaining (?.)은 부모 객체가 실제로 Rectangle이 아닐 경우 exception을 throw하는 것을 피합니다. 이 경우 "red"가 parentColor로 선택됩니다.

* QML 글로벌 객체

QML JavaScript 호스트 환경은 [QML 글로벌 객체](https://doc.qt.io/qt-6/qtqml-javascript-qmlglobalobject.html) 문서에 자세히 나와 있는 것처럼 많은 호스트 객체 및 함수를 구현합니다.

이러한 호스트 객체 및 함수는 가져온 모듈 여부에 관계없이 항상 사용할 수 있습니다.

* JavaScript 객체와 함수

QML 엔진에서 지원하는 JavaScript 객체, 함수 및 프로퍼티 목록은 JavaScript 객체 및 함수 목록에서 확인할 수 있습니다.

QML은 네이티브 객체에 대해 다음과 같이 수정한다는 것을 유의하십시오:
- [arg()](https://doc.qt.io/qt-6/qml-string.html) 함수가 String 프로토타입에 추가됩니다.
- 로케일 인식 변환 함수는 [Date](https://doc.qt.io/qt-6/qml-qtqml-date.html) 및 [Number](https://doc.qt.io/qt-6/qml-qtqml-number.html) 프로토타입에 추가됩니다.

또한 QML은 instanceof 함수의 동작을 확장하여 QML 타입에 대한 타입 검사를 허용합니다. 이는 변수가 실제로 예상되는 타입인지 확인하는 데 사용할 수 있음을 의미합니다. 예를 들면:

```qml
var v = something();
if (!v instanceof Item) {
    throw new TypeError("I need an Item type!");
}

...
```

* JavaScript 환경 제한

QML은 JavaScript 코드에 대해 다음과 같은 제한을 구현합니다:

- .qml 파일로 작성된 JavaScript 코드는 글로벌 객체를 수정할 수 없습니다. .js 파일로 작성된 JavaScript 코드는 글로벌 객체를 수정할 수 있으며 이러한 수정사항은 [import 될 때](https://doc.qt.io/qt-6/qtqml-javascript-imports.html#importing-a-javascript-resource-from-a-qml-document) .qml 파일에 표시될 것입니다.
QML에서 전역 객체는 상수입니다. 기존 프로퍼티는 수정하거나 삭제할 수 없으며 새 프로퍼티를 만들 수 없습니다.

대부분의 JavaScript 프로그램은 글로벌 객체를 의도적으로 수정하지 않으며, JavaScript의 미-선언 변수 자동 생성은 글로벌 객체를 암묵적으로 수정하는 것으로 QML에서는 금지되어 있습니다.

변수가 범위 체인에 존재하지 않는다고 가정한다면, QML에서 다음 코드는 잘못된 코드입니다:

```qml
// 미-선언 변수의 잘못된 수정
a = 1;
for (var ii = 1; ii < 10; ++ii)
    a = a * ii;
console.log("Result: " + a);
```

이것은 조금만 수정하여 다음과 같이 올바른 코드로 변경할 수 있습니다.

```qml
var a = 1;
for (var ii = 1; ii < 10; ++ii)
    a = a * ii;
console.log("Result: " + a);
```

전역 객체를 -암묵적/명시적으로- 수정하려고 하면 예외가 발생할 것입니다. 만약 caught하지 않으면 위반 코드의 파일 및 줄 번호를 포함하는 경고가 인쇄될 것입니다.

- 축소된 범위에서 글로벌 코드가 실행됩니다.

시작하는 동안, 만약 QML 파일이 "글로벌" 코드를 가진 외부 JavaScript 파일을 포함한 경우, QML 파일은 외부 파일 자체와 글로벌 객체만을 포함하는 범위에서 실행됩니다. 즉, QML 객체와 [일반적으로](https://doc.qt.io/qt-6/qtqml-documents-scope.html) 접근할 수 있는 속성은 없습니다.

스크립트 로컬 변수에만 접근하는 글로벌 코드가 허용됩니다. 다음은 올바른 글로벌 코드의 예시입니다.

```qml
var colors = [ "red", "blue", "green", "orange", "purple" ];
```

QML 객체에 접근하는 글로벌 코드가 올바르게 실행되지 않습니다.

```qml
// 유효하지 않은 글로벌 코드 - "rootObject" 변수가 정의되지 않음
var initialPosition = { rootObject.x, rootObject.y }
```

QML 환경이 아직 완전히 구축되지 않았기 때문에 이러한 제한이 있습니다. 환경 설정이 완료된 후 코드를 실행하려면 [JavaScript 앱 시작 코드](https://doc.qt.io/qt-6/qtqml-javascript-expressions.html#javascript-in-application-startup-code)를 보십시오.

- this의 값은 대부분의 컨텍스트에서 QML에 정의되지 않습니다.

this 키워드는 JavaScript에서 프로퍼티를 바인딩할 때 지원됩니다. QML 바인딩 표현식, QML 시그널 핸들러, 그리고 QML 선언 함수에서 this는 범위 객체를 의미합니다. 다른 모든 상황에서 QML의 this 값은 정의되지 않습니다.

특정 객체를 참조하려면 id를 제공합니다. 예를 들어:

```qml
Item {
    width: 200; height: 100
    function mouseAreaClicked(area) {
        console.log("Clicked in area at: " + area.x + ", " + area.y);
    }
    // 이것은 area를 함수에게 전달할 것입니다.
    MouseArea {
        id: area
        y: 50; height: 50; width: 200
        onClicked: mouseAreaClicked(area)
    }
}
```

[범위 및 네이밍 규칙](https://doc.qt.io/qt-6/qtqml-documents-scope.html)도 보십시오.

---

#### QML 타입 시스템

QML 문서의 객체 계층의 정의에 사용될 수 있는 타입은 다양한 소스에서 제공될 수 있습니다. 다음과 같습니다:

- QML 언어에서 네이티브로 제공함
- QML 모듈에 의해 C++을 통하여 등록됨
- QML 모듈에 의해 QML 문서로 제공됨

또한 앱 개발자는 C++ 타입을 직접 등록하거나 QML 문서에서 재사용 가능한 컴포넌트를 정의하여 자신만의 타입을 제공하여 가져올 수도 있습니다.

타입 정의의 출처가 어디든 간에, 엔진은 해당 타입의 프로퍼티 및 인스턴스에 대해 타입 안전을 적용할 것입니다.

* 값 타입

QML 언어는 정수, 이중 정밀도 부동 소수점 숫자, 문자열 및 부울 값을 포함하는 다양한 프리미티브 타입에 대한 내장 지원을 가지고 있습니다. 객체는 이러한 타입의 프로퍼티를 가질 수 있고, 이러한 타입의 값은 객체의 메서드에 대한 인수로 전달될 수 있습니다.

값 타입에 대한 자세한 정보는 [QML 값 타입](https://doc.qt.io/qt-6/qtqml-typesystem-valuetypes.html) 문서를 보십시오.

* JavaScript 타입

JavaScript 객체와 배열은 QML 엔진에서 지원합니다. 일반 [var](https://doc.qt.io/qt-6/qml-var.html) 타입을 사용하여 표준 JavaScript 타입을 생성하고 저장할 수 있습니다.

예를 들어, 다음과 같이 표준 Date 및 Array 타입을 사용할 수 있습니다:

```qml
import QtQuick 2.0

Item {
    property var theArray: []
    property var theDate: new Date()

    Component.onCompleted: {
        for (var i = 0; i < 10; i++)
            theArray.push("Item " + i)
        console.log("There are", theArray.length, "items in the array")
        console.log("The time is", theDate.toUTCString())
    }
}
```

더 자세한 것은 [QML 문서에서의 JavaScript 표현식](https://doc.qt.io/qt-6/qtqml-javascript-expressions.html)을 보십시오.

* QML 객체 타입

QML 객체 타입은 QML 객체가 인스턴스화될 수 있는 타입입니다. QML 객체 타입은 [QtObject](https://doc.qt.io/qt-6/qml-qtqml-qtobject.html)에서 파생되고 QML 모듈에서 제공됩니다. 앱은 제공하는 객체 타입을 사용하기 위해 이러한 모듈을 가져올 수 있습니다. QtQuick 모듈은 QML에서 사용자 인터페이스를 만드는 데 필요한 가장 일반적인 객체 타입을 제공합니다.

마지막으로, 모든 QML 문서는 암묵적으로 QML 객체 타입을 정의하며, 이것을 다른 QML 문서에서 재사용될 수 있습니다. 객체 타입에 대한 자세한 정보는 [QML 타입 시스템의 객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-objecttypes.html)에 대한 문서를 보십시오.


##### QML 값 타입

QML은 내장 값 타입, 커스텀 값 타입을 지원합니다.

값 타입은 int 또는 string과 같이 참조가 아닌 값으로 전달되는 타입입니다. 이는 [QML 객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html#qml-object-types)과 대조됩니다. 객체 타입은 참조로 전달됩니다. 만약 객체 타입의 인스턴스를 2개의 서로 다른 타입에 할당하면 두 프로퍼티 모두 동일한 값을 전달합니다. 객체를 수정하는 것은 두 프로퍼티에 모두 반영됩니다. 만약 값 타입의 인스턴스를 2개의 서로 다른 프로퍼티에 할당하면 프로퍼티는 별도의 값을 전달합니다. 이 중 하나를 수정하면 다른 하나는 그대로 유지됩니다. 객체 타입과 달리 값 타입은 QML 객체를 선언하는 데 사용할 수 없습니다: 예를 들어 int{} 객체 또는 size{} 객체를 선언하는 것은 불가능합니다.

값 타입은 다음을 참조하는 데 사용할 수 있습니다:

- 단일 값 (예. 단일 숫자를 참조하는 [int](https://doc.qt.io/qt-6/qml-int.html))
- 프로퍼티와 메서드를 포함하는 값 (예. width와 height 프로퍼티가 있는 값을 참조하는 [size](https://doc.qt.io/qt-6/qml-size.html))
- 일반 타입 [var](https://doc.qt.io/qt-6/qml-var.html). 이것은 모든 타입의 값을 저장할 수 있지만 이 자체는 값 타입입니다.

변수 또는 프로퍼티가 값 타입을 유지하고 다른 변수 또는 프로퍼티에 할당되면 값의 복사본이 만들어집니다.

* 사용 가능한 값 타입

일부 값 타입은 기본적으로 엔진에서 지원하며 [import 문](https://doc.qt.io/qt-6/qtqml-syntax-imports.html)을 사용할 필요가 없는 반면, 다른 값 타입은 클라이언트가 이를 제공하는 모듈을 import 하도록 요구합니다. 아래에 나열된 모든 값 타입은 다음과 같은 예외를 제외하고 QML 문서에서 프로퍼티 타입으로 사용될 수 있습니다:

- 값의 부재를 표시하는 void
- 리스트(list)는 요소로 객체 또는 값 타입과 함께 사용되어야 함
- 열거형(enumeration)은 등록된 QML 객체 타입으로 정의되어야 하므로 열거형을 직접 사용할 수 없음

* QML 언어가 제공하는 내장 값 타입

QML 언어에서 네이티브로 제공하는 내장 값 타입은 아래에 나열되어 있습니다:

| 타입 | 설명 |
| --- | --- |
| [bool](https://doc.qt.io/qt-6/qml-bool.html) | 이진(binary) true/false 값 |
| [date](https://doc.qt.io/qt-6/qml-date.html) | Date 값 |
| [double](https://doc.qt.io/qt-6/qml-double.html) | 소수점이 있는 숫자, 이중 정밀도로 저장됨 |
| [enumeration](https://doc.qt.io/qt-6/qml-enumeration.html) | 이름이 붙어 있는 열거형 값 |
| [int](https://doc.qt.io/qt-6/qml-int.html) | 정수, 예. 0, 10, 또는 -20 |
| [list](https://doc.qt.io/qt-6/qml-list.html) | QML 객체의 리스트 |
| [real](https://doc.qt.io/qt-6/qml-real.html) | 소수점이 있는 숫자 |
| [string](https://doc.qt.io/qt-6/qml-string.html) | 자유 형식 텍스트 문자열 |
| [url](https://doc.qt.io/qt-6/qml-url.html) | 리소스 로케이터 (Resource locator) |
| [var](https://doc.qt.io/qt-6/qml-var.html) | 일반 프로퍼티 타입 |
| [variant](https://doc.qt.io/qt-6/qml-variant.html) | 일반 프로퍼티 타입 |
| [void](https://doc.qt.io/qt-6/qml-void.html) | 비어 있는 값 타입 |

* QML 모듈이 제공하는 값 타입

QML 모듈은 더 많은 값 타입으로 QML 언어를 확장할 수 있습니다. 예를 들면, QtQuick 모듈이 제공하는 값 타입은 다음과 같습니다:

| 타입 | 설명 |
| --- | --- |
| [color](https://doc.qt.io/qt-6/qml-color.html) | ARGB 컬러 값 |
| [font](https://doc.qt.io/qt-6/qml-font.html) | QFont의 프로퍼티를 가진 글꼴 값. 글꼴 타입은 QFont의 프로퍼티를 가진 글꼴 값을 참조하는 글꼴 타입입니다 |
| [matrix4x4](https://doc.qt.io/qt-6/qml-matrix4x4.html) | matrix4x4 타입은 4-row 그리고 4-column 행렬입니다 |
| [point](https://doc.qt.io/qt-6/qml-point.html) | x, y 애트리뷰트를 가진 값입니다 |
| [quaternion](https://doc.qt.io/qt-6/qml-quaternion.html) | 사분면 타입은 스칼라, x, y, z 애트리뷰트를 가지고 있습니다 |
| [rect](https://doc.qt.io/qt-6/qml-rect.html) | x, y, width, height 애트리뷰트를 가진 값입니다 |
| [size](https://doc.qt.io/qt-6/qml-size.html) | width와 height 애트리뷰트를 가진 값입니다 |
| [vector2d](https://doc.qt.io/qt-6/qml-vector2d.html) | vector2d 타입은 x, y 애트리뷰트를 가지고 있습니다 |
| [vector3d](https://doc.qt.io/qt-6/qml-vector3d.html) | x, y, z 애트리뷰트를 가진 값입니다 |
| [vector4d](https://doc.qt.io/qt-6/qml-vector4d.html) | vector4d 타입은 x, y, z 및 w 애트리뷰트를 가지고 있습니다 |

[Qt](https://doc.qt.io/qt-6/qml-qtqml-qt.html) 글로벌 객체는 값 타입의 값을 조작하는 데 유용한 함수를 제공합니다.

[C++에서 QML 타입 정의하기](https://doc.qt.io/qt-6/qtqml-cppintegration-definetypes.html)에서 설명한 대로 자신만의 값 타입을 정의할 수 있습니다. 특정 QML 모듈에서 제공하는 타입을 사용하려면, 클라이언트가 해당 모듈을 QML 문서로 import 해야 합니다.

* 값 타입에 대한 프로퍼티 변경 동작

일부 값 타입은 프로퍼티를 갖고 있습니다: 예를 들면, [글꼴](https://doc.qt.io/qt-6/qml-font.html) 타입은 pixelSize, family, bold 프로퍼티가 있습니다. [객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html#qml-object-types)의 프로퍼티와 달리 값 타입의 프로퍼티는 자체 프로퍼티 변경 시그널을 제공하지 않습니다. 값 타입 프로퍼티 자체에 대한 프로퍼티 변경 시그널 핸들러만 만들 수 있습니다:

```qml
Text {
    // 유효하지 않음!
    onFont.pixelSizeChanged: doSomething()

    // 역시 유효하지 않음!
    font {
        onPixelSizeChanged: doSomething()
    }

    // 하지만 이것은 괜찮다
    onFontChanged: doSomething()
}
```

그러나 값 타입에 대한 프로퍼티 변경 시그널은 애트리뷰트의 자체가 변경될 때뿐만 아니라 프로퍼티가 변경될 때마다 발생합니다. 예를 들면 다음 코드를 사용하십시오:

```qml
Text {
    onFontChanged: console.log("font changed")

    Text { id: otherText }

    focus: true

    // 글꼴 애트리뷰트를 변경하거나 프로퍼티를 다른 글꼴 값으로 재할당하면 onFontChanged 핸들러가 호출됨
    Keys.onDigit1Pressed: font.pixelSize += 1
    Keys.onDigit2Pressed: font.b = !font.b
    Keys.onDigit3Pressed: font = otherText.font
}
```

반대로, [객체 타입](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html#qml-object-types)의 프로퍼티는 자체 프로퍼티 변경 시그널을 내보내고, 객체 타입 프로퍼티에 대한 프로퍼티 변경 시그널 핸들러는 다른 객체 값에 재할당된 경우에만 호출됩니다.

[QML 타입 시스템](https://doc.qt.io/qt-6/qtqml-typesystem-topic.html)도 보십시오.


##### QML 객체 타입

QML 객체 타입은 QML 객체를 인스턴스화할 수 있는 타입입니다.

구문론적으로 QML 객체 타입은 타입 이름 다음에 해당 객체의 애트리뷰트를 감싸는 {} 세트를 지정하여 객체를 선언하는 데 사용할 수 있는 타입입니다. 이는 동일한 방식으로 사용할 수 없는 값 타입과 다릅니다. 예를 들면, [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)은 QML 객체 타입입니다: Rectangle은 Rectangle 타입의 객체를 만드는 데 사용할 수 있습니다. 객체가 아닌 간단한 데이터 타입을 저장하는 데 사용되는 int 및 bool과 같은 프리미티브 타입에서는 수행할 수 없습니다.

커스텀 QML 객체 타입은 [QML 객체 타입 정의로서의 문서](https://doc.qt.io/qt-6/qtqml-documents-definetypes.html)에서 설명한 대로 타입을 정의하는 .qml 파일을 만들거나 [C++에서 QML 타입 정의하기](https://doc.qt.io/qt-6/qtqml-cppintegration-definetypes.html)에서 설명한 대로 QML 엔진에 타입을 등록하여 정의할 수 있습니다. 두 경우 모두 QML 파일에서 QML 객체 타입으로 선언하려면 타입 이름이 대문자로 시작해야 한다는 것을 주의하십시오.

C++ 및 다양한 QML 통합 메서드에 대한 자세한 내용은 [C++ 및 QML 통합 개요](https://doc.qt.io/qt-6/qtqml-cppintegration-overview.html) 페이지를 보십시오.

* QML에서 객체 타입 정의하기

* QML 문서를 통해 객체 타입 정의하기

플러그인 작성자 및 앱 개발자는 QML 문서로 정의된 타입을 제공할 수 있습니다. QML 문서는 QML 가져오기 시스템에 보일 때 파일 이름에서 파일 확장자를 제외한 타입을 정의합니다.

따라서 "MyButton.qml"이라는 이름의 QML 문서가 있을 경우, QML 앱에서 사용할 수 있는 "MyButton" 타입의 정의를 제공합니다.

QML 문서를 정의하는 방법과 QML 언어의 구문에 대한 자세한 내용은 [QML 문서](https://doc.qt.io/qt-6/qtqml-documents-topic.html)에 대한 문서를 보십시오. QML 언어와 QML 문서를 정의하는 방법을 숙지한 후에는 [QML 문서에서 자신의 재사용 가능한 QML 타입을 정의하고 사용하는 방법](https://doc.qt.io/qt-6/qtqml-documents-definetypes.html)을 설명하는 문서를 보십시오.

자세한 내용은 [QML 문서를 통한 객체 타입 정의하기](https://doc.qt.io/qt-6/qtqml-documents-definetypes.html)를 보십시오.

* 컴포넌트로 익명(Anonymous) 타입 정의하기

QML 내에서 객체 타입을 생성하는 또 다른 방법은 [Component](https://doc.qt.io/qt-6/qml-qtqml-component.html) 타입을 사용하는 것입니다. 이렇게 하면 .qml 파일에 별도의 문서를 사용하는 대신 QML 문서 내에서 타입을 인라인으로 정의할 수 있습니다.

```qml
Item {
    id: root
    width: 500; height: 500

    Component {
        id: myComponent
        Rectangle { width: 100; height: 100; color: "red" }
    }

    Component.onCompleted: {
        myComponent.createObject(root)
        myComponent.createObject(root, {"x": 200})
    }
}
```

여기서 myComponent 객체는 본질적으로 이 익명 타입의 객체를 생성하기 위해 [Component::createObject](https://doc.qt.io/qt-6/qml-qtqml-component.html#createObject-method)를 사용하여 인스턴스화할 수 있는 익명 타입을 정의합니다.

인라인 컴포넌트는 일반적인 최상위 컴포넌트의 모든 특성을 공유하고 포함된 QML 문서와 동일한 import 목록을 사용합니다.

각 [Component](https://doc.qt.io/qt-6/qml-qtqml-component.html) 객체 선언은 자체 컴포넌트 범위를 만든다는 것을 주의하십시오. [Component](https://doc.qt.io/qt-6/qml-qtqml-component.html) 객체 선언 내에서 사용되고 참조되는 모든 id 값은 해당 범위 내에서 유일해야 하나, 인라인 컴포넌트가 선언되는 문서 내에서는 유일해야 할 필요는 없습니다. 그래서 myComponent 객체 선언에서 선언된 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)은 동일한 문서 내의 Item 객체에 대해 선언된 루트와 충돌하지 않고 루트 id를 가질 수 있습니다. 이는 2개의 id 값이 서로 다른 컴포넌트 범위 안에서 선언되었기 때문입니다.

자세한 내용은 [범위 및 네이밍 규칙](https://doc.qt.io/qt-6/qtqml-documents-scope.html)을 보십시오.

* C++에서 객체 타입 정의하기

C++ 플러그인 작성자 및 앱 개발자는 Qt QML 모듈에서 제공하는 API를 통해 C++에 정의된 타입을 등록할 수 있습니다. 각각 다른 사용 사례(use-case)를 충족할 수 있는 다양한 등록 함수가 있습니다. 이러한 등록 함수에 대한 자세한 정보와 커스텀 C++ 타입을 QML에 노출시키는 정보는 [C++에서 QML 타입 정의하기](https://doc.qt.io/qt-6/qtqml-cppintegration-definetypes.html)에 대한 문서를 보십시오.

QML 타입 시스템은 알려진 import 경로에 설치되는 가져오기, 플러그인 및 확장에 의존합니다. 플러그인은 서드파티 개발자에 의해 제공되고 클라이언트 앱 개발자에 의해 재사용될 수 있습니다. QML 확장 모듈을 생성하고 배포하는 방법에 대한 자세한 내용은 [QML 모듈](https://doc.qt.io/qt-6/qtqml-modules-topic.html)에 대한 문서를 보십시오.


##### QML로부터 객체 타입 정의하기

QML의 핵심 기능 중 하나는 QML 문서를 통해 QML 객체 타입을 개별 QML 앱의 필요에 맞게 쉽게 정의할 수 있다는 것입니다. 표준 [Qt Quick](https://doc.qt.io/qt-6/qtquick-index.html) 모듈은 QML 앱을 구축하기 위한 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html), [Text](https://doc.qt.io/qt-6/qml-qtquick-text.html) 및 [Image](https://doc.qt.io/qt-6/qml-qtquick-image.html)와 같은 다양한 타입을 제공합니다; 이 외에도 앱 내에서 재사용할 자신만의 QML 타입을 쉽게 정의할 수 있습니다. 자신만의 타입을 생성할 수 있는 이 기능은 QML 앱의 빌딩 블럭을 형성합니다.

* QML 파일로 객체 타입 정의하기

* 커스텀 QML 객체 타입 이름 짓기

객체 타입을 만들려면 QML 문서를 <TypeName>.qml이라는 텍스트 파일에 넣어야 합니다. 여기서 <TypeName>은 원하는 타입 이름입니다. 타입 이름에는 다음과 같은 요구사항을 갖습니다:

- 영숫자 또는 밑줄 문자로 구성되어야 합니다.
- 대문자로 시작해야 합니다.

그러면 이 문서는 QML 타입의 정의로서 엔진에 의해 자동으로 인식됩니다. 추가적으로, 이러한 방식으로 정의된 타입은 QML 타입 이름을 해결할 때 엔진이 즉시 디렉토리 내에서 검색하는 것과 동일한 로컬 디렉토리 내의 다른 QML 파일에 자동으로 사용 가능하게 됩니다.

주의: QML 엔진은 이러한 방식으로 원격 디렉토리를 자동으로 검색하지 않습니다. 만약 문서가 네트워크를 통해 로드된 경우 qmldir 파일을 추가해야 합니다. [QML 문서 디렉토리 가져오기](https://doc.qt.io/qt-6/qtqml-syntax-directoryimports.html)를 보십시오.

* 커스텀 QML 타입 정의

예를 들면, 다음은 자식 [MouseArea](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html)를 가진 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)을 선언하는 문서입니다. 이 문서는 SquareButton.qml이라는 파일에 저장되었습니다:

```qml
// SquareButton.qml
import QtQuick 2.0

Rectangle {
    property int side: 100
    width: side; height: side
    color: "red"

    MouseArea {
        anchors.fill: parent
        onClicked: console.log("Button clicked!")
    }
}
```

파일 이름이 SquareButton.qml이므로 동일한 디렉토리 내의 다른 QML 파일에서 SquareButton이라는 타입으로 사용할 수 있습니다. 예를 들면, 동일한 디렉토리에 myapplication.qml 파일이 있을 경우 SquareButton 타입을 참조할 수 있습니다:

```qml
// myapplication.qml
import QtQuick 2.0

SquareButton {}
```

이렇게 하면 SquareButton.qml에 정의된 대로 내부 [MouseArea](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html)가 있는 100 x 100 red [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)이 만들어집니다. 이 myapplication.qml 문서가 엔진에 의해 로드되면 SquareButton.qml 문서를 컴포넌트로 로드하고 인스턴스화하여 SquareButton 객체를 만듭니다.

SquareButton 타입은 SquareButton.qml에 선언된 QML 객체 트리를 캡슐화합니다. QML 엔진이 이 타입으로부터 SquareButton 객체를 인스턴스화하면 SquareButton.qml에 선언된 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 트리로부터 객체를 인스턴스화합니다.

주의: 파일 이름의 대소문자는 일부(특히 UNIX) 파일 시스템에서 중요합니다. 파일 이름 대소문자는 QML 타입이 배포될 플랫폼에 관계없이 원하는 QML 타입 이름의 대소문자(예: BoX.qml이 아닌 Box.qml)와 정확히 일치하는 것이 좋습니다.

* 인라인 컴포넌트

작은 위임자(delegate)를 여러 보기(multiple view)에서 재사용하는 경우와 같이, 때때로 타입에 대한 새 파일을 만드는 것이 불편할 수 있습니다. 실제로 타입을 노출할 필요가 없고 인스턴스만 생성하면 되는 경우 컴포넌트는 선택사항입니다. 그러나 컴포넌트 타입으로 프로퍼티를 선언하거나 여러 파일에서 프로퍼티를 사용하려면 컴포넌트는 선택사항이 아닙니다. 이 경우 인라인 컴포넌트를 사용할 수 있습니다. 인라인 컴포넌트는 파일 내부에 새 컴포넌트를 선언합니다. 이 구문은 다음과 같습니다.

```qml
component <component name> : BaseType {
    // 여기에 프로퍼티와 바인딩을 선언함
}
```

인라인 컴포넌트를 선언하는 파일 내에서 이름만으로 타입을 참조할 수 있습니다.

```qml
// Images.qml
import QtQuick 2.15

Item {
    component LabeledImage: Column {
        property alias source: image.source
        property alias caption: text.text

        Image {
            id: image
            width: 50
            height: 50
        }
        Text {
            id: text
            font.bold: true
        }
    }

    Row {
        LabeledImage {
            id: before
            source: "before.png"
            caption: "Before"
        }
        LabeledImage {
            id: after
            source: "after.png"
            caption: "After"
        }
    }
    property LabeledImage selectedImage: before
}
```

다른 파일에서는 포함하는 컴포넌트의 이름으로 접두사를 지정해야 합니다.

```qml
// LabeledImageBox.qml
import QtQuick 2.15

Rectangle {
    property alias caption: image.caption
    property alias source: image.source
    border.width: 2
    border.color: "black"
    Images.LabeledImage {
        id: image
    }
}
```

주의: 인라인 컴포넌트는 선언된 컴포넌트와 범위를 공유하지 않습니다. 다음 예제에서 B.qml 파일의 A.MyInlineComponent가 생성되면 루트가 B.qml의 id로 존재하지 않으므로 ReferenceError가 발생합니다. 따라서 인라인 컴포넌트의 일부가 아닌 객체는 참조하지 않는 것이 좋습니다.

```qml
// A.qml
import QtQuick 2.15

Item {
    id: root
    property string message: "From A"
    component MyInlineComponent : Item {
        Component.onCompleted: console.log(root.message)
    }
}
// B.qml
import QtQuick 2.15

Item {
    A.MyInlineComponent {}
}
```

주의: 인라인 컴포넌트는 중첩될 수 없습니다.

* 현재 디렉토리 외부에서 정의된 타입 가져오기

SquareButton.qml이 myapplication.qml과 동일한 디렉토리에 없을 경우, SquareButton 타입을 myapplication.qml의 import 문을 통해 사용할 수 있어야 합니다. 파일 시스템의 상대 경로에서 가져오거나 설치된 모듈로 가져올 수 있습니다. 자세한 내용은 [모듈](https://doc.qt.io/qt-6/qtqml-modules-topic.html)을 참조하십시오.

* 커스텀 타입의 접근 가능한 애트리뷰트

.qml 파일의 루트 객체 정의는 QML 타입에 사용할 수 있는 애트리뷰트를 정의합니다. 이 루트 객체에 속하는 모든 프로퍼티, 시그널, 메서드는 - 커스텀 선언된 것이든 루트 객체의 QML 타입에서 나온 것이든 관계없이 - 외부에서 접근할 수 있으며 이 타입의 객체에 대해 읽고 수정할 수 있습니다.

예를 들어, 위의 SquareButton.qml 파일의 루트 객체 타입은 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)입니다. 이는 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 타입에 의해 정의된 모든 프로퍼티를 SquareButton 객체에 대해 수정할 수 있음을 의미합니다. 아래 코드는 SquareButton 타입의 루트 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 객체의 일부 프로퍼티에 대해 커스텀 값으로 3개의 SquareButton 객체를 정의합니다:

```qml
// application.qml
import QtQuick 2.0

Column {
    SquareButton { side: 50 }
    SquareButton { x: 50; color: "blue" }
    SquareButton { radius: 10 }
}
```

커스텀 QML 타입의 객체에 접근할 수 있는 애트리뷰트에는 객체에 대해 추가로 정의된 [커스텀 프로퍼티](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#defining-property-attributes), [메서드](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#defining-method-attributes), [시그널](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#defining-signal-attributes)이 포함됩니다. 예를 들면, SquareButton.qml의 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)이 추가 프로퍼티, 메서드, 시그널과 함께 다음과 같이 정의되었다고 가정합니다:

```qml
// SquareButton.qml
import QtQuick 2.0

Rectangle {
    id: root

    property bool pressed: mouseArea.pressed

    signal buttonClicked(real xPos, real yPos)

    function randomizeColor() {
        root.color = Qt.rgba(Math.random(), Math.random(), Math.random(), 1)
    }

    property int side: 100
    width: side; height: side
    color: "red"

    MouseArea {
        id: mouseArea
        anchors.fill: parent
        onClicked: (mouse)=> root.buttonClicked(mouse.x, mouse.y)
    }
}
```

SquareButton 객체는 루트 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html)에 추가된 pressed 프로퍼티, buttonClicked 시그널, randomizeColor() 메서드를 사용할 수 있습니다:

```qml
// application.qml
import QtQuick 2.0

SquareButton {
    id: squareButton

    onButtonClicked: (xPos, yPos)=> {
        console.log("Clicked", xPos, yPos)
        randomizeColor()
    }

    Text { text: squareButton.pressed ? "Down" : "Up" }
}
```

SquareButton.qml에 정의된 id 값은 컴포넌트가 선언된 컴포넌트 범위 내에서만 접근할 수 있으므로 SquareButton 객체에 접근할 수 없습니다. 위의 SquareButton 객체 정의는 [MouseArea](https://doc.qt.io/qt-6/qml-qtquick-mousearea.html) 자식을 참조하기 위해 MouseArea를 참조할 수 없으며, squareButton이 아닌 root의 id를 가진 경우 SquareButton.qml에 정의된 루트 객체에 대해 동일한 값의 id와 충돌하지 않습니다. 이는 두 개의 id가 별도의 범위 내에서 선언되어 있기 때문입니다.

* Pragma

pragma 키워드를 사용하여 QML 문서에 글로벌 명령어를 추가할 수 있습니다. 다음과 같은 pragma가 지원됩니다:

* 싱글톤(Singleton)

pragma Singleton은 QML 문서에 정의된 컴포넌트를 싱글톤으로 선언합니다. 싱글톤은 QML 엔진당 한 번만 생성됩니다. QML로 선언된 싱글톤을 사용하려면 해당 모듈에 등록해야 합니다. CMake로 이 작업을 수행하는 방법은 [qt_target_qml_sources](https://doc.qt.io/qt-6/qt-target-qml-sources.html)를 보십시오.

* ListPropertyAssignBehavior

이 pragma를 사용하면 QML 문서에 정의된 컴포넌트에서 리스트 프로퍼티에 대한 할당을 처리하는 방법을 정의할 수 있습니다. 기본적으로 리스트 프로퍼티에 대한 할당은 리스트에 이어 붙이는 것입니다. 이 동작은 값 Append를 사용하여 명시적으로 요청할 수 있습니다. 또는 Replace를 사용하여 리스트 프로퍼티의 내용을 항상 대체하거나, ReplaceIfNotDefault를 사용하여 해당 프로퍼티가 기본 프로퍼티가 아닌 경우 대체하도록 요청할 수 있습니다. 예를 들면:

```qml
pragma ListPropertyAssignBehavior: ReplaceIfNotDefault
```

C++ 정의 타입에 대해서도 동일한 선언을 제공할 수 있습니다. QML_LIST_PROPERTY_ASSIGN_BEHAVIOR_APPEND, QML_LIST_PROPERTY_ASSIGN_BEHAVIOR_REPLACE, QML_LIST_PROPERTY_ASSIGN_BEHAVIOR_REPLACE_IF_NOT_DEFAULT를 보십시오.

* ComponentBehavior

이 pragma를 사용하면 이 파일에 정의된 컴포넌트가 원래 컨텍스트 안에서만 객체만 생성하도록 제한할 수 있습니다. 이는 명시적으로/암묵적으로 프로퍼티로 생성된 컴포넌트 요소뿐만 아니라 인라인 컴포넌트에도 적용됩니다. 컴포넌트가 해당 컨텍스트에 바인딩된 경우, 컴포넌트 내의 나머지 파일의 ID를 안전하게 사용할 수 있습니다. 그렇지 않으면 엔진 및 QML 도구는 이러한 ID가 런타임시 어떤 타입으로 확인되는지 미리 알 수 없습니다.

컴포넌트를 컨텍스트에 바인딩하려면 Bound 인수를 지정합니다:

```qml
pragma ComponentBehavior: Bound
```

기본값은 Unbound입니다. 명시적으로 지정할 수도 있습니다. 향후 Qt 버전에서는 기본값이 Bound로 변경될 것입니다.

컨텍스트에 바인딩된 위임 컴포넌트는 인스턴스화 시 자신의 개인 컨텍스트를 수신하지 않습니다. 이 경우 모델 데이터는 [필요한 프로퍼티](https://doc.qt.io/qt-6/qtqml-syntax-objectattributes.html#required-properties)를 통해서만 전달될 수 있음을 의미합니다. 컨텍스트 프로퍼티를 통해 모델 데이터를 전달하는 것은 작동하지 않습니다. 여기서는 위임자가 [Instantiator](https://doc.qt.io/qt-6/qml-qtqml-models-instantiator.html), [Repeater](https://doc.qt.io/qt-6/qml-qtquick-repeater.html), [ListView](https://doc.qt.io/qt-6/qml-qtquick-listview.html), [TableView](https://doc.qt.io/qt-6/qml-qtquick-tableview.html), [GridView](https://doc.qt.io/qt-6/qml-qtquick-gridview.html), [TreeView](https://doc.qt.io/qt-6/qml-qtquick-treeview.html) 그리고 내부적으로 [DelegateModel](https://doc.qt.io/qt-6/qml-qtqml-models-delegatemodel.html)을 사용하는 일반적인 모든 것에 관한 것입니다.

예를 들어, 다음은 작동하지 않습니다:

```qml
pragma ComponentBehavior: Bound
import QtQuick

ListView {
    delegate: Rectangle {
        color: model.myColor
    }
}
```

[ListView](https://doc.qt.io/qt-6/qml-qtquick-listview.html)의 위임 프로퍼티는 컴포넌트입니다. 따라서 [Component](https://doc.qt.io/qt-6/qml-qtqml-component.html)는 여기 [Rectangle](https://doc.qt.io/qt-6/qml-qtquick-rectangle.html) 주위에 암묵적으로 생성됩니다. 해당 컴포넌트는 해당 컨텍스트에 바인딩됩니다. [ListView](https://doc.qt.io/qt-6/qml-qtquick-listview.html)에서 제공하는 컨텍스트 프로퍼티 모델을 수신하지 않습니다. 작동시키려면 다음과 같이 작성해야 합니다:

```qml
pragma ComponentBehavior: Bound
import QtQuick

ListView {
    delegate: Rectangle {
        required property color myColor
        color: myColor
    }
}
```

컴포넌트를 QML 파일에 중첩시킬 수 있습니다. pragma는 파일의 모든 컴포넌트에 대해 아무리 깊게 중첩시키더라도 유지됩니다.

* FunctionSignatureBehavior

이 pragma를 사용하면 함수에 대한 타입 어노테이션이 처리되는 방식을 변경할 수 있습니다. 기본적으로 인터프리터와 JIT는 타입 어노테이션을 무시하지만, [QML 스크립트 컴파일러](https://doc.qt.io/qt-6/qtqml-qml-script-compiler.html)는 C++로 컴파일할 때 이러한 주석을 강제로 실행합니다.

Enforce를 값으로 지정하면 타입 어노테이션이 항상 실행됩니다. 결과적인 타입 강요는 타입화된 JavaScrit 함수를 호출하는 오버헤드를 증가시킵니다.

Ignore를 값으로 지정하면 [QML 스크립트 컴파일러](https://doc.qt.io/qt-6/qtqml-qml-script-compiler.html)가 문서를 C++로 컴파일할 때 JavaScript 함수를 무시하게 됩니다. 이는 C++로 미리 컴파일되는 코드가 줄어들고 더 많은 코드가 인터프리트 되거나 JIT로 컴파일해야 한다는 것을 의미합니다.

* ValueTypeBehavior

이 pragma를 사용하면 값 타입 및 시퀀스 처리 방식을 변경할 수 있습니다.

값 타입 및 시퀀스는 일반적으로 참조로 취급됩니다. 이는 프로퍼티에서 값 타입 인스턴스를 로컬 값으로 가져온 다음에 로컬 값을 변경하면 원래 프로퍼티도 변경된다는 것을 의미합니다. 게다가 원래 프로퍼티를 명시적으로 쓰기 연산을 하면 로컬 값도 업데이트됩니다. 이 동작은 많은 곳에서 다소 직관적이지 않으므로 이에 의존해서는 안 됩니다. ValueTypeBehavior pragma의 Copy와 Reference 값은 이 동작을 변경하기 위한 실험적인 옵션입니다. 사용하지 마십시오. Copy를 지정하면 모든 값 타입이 실제 복사본으로 취급됩니다. Reference를 지정하면 기본 동작이 명시적으로 지정됩니다.

Copy를 사용하는 대신 값 타입 및 시퀀스에 대한 참조가 부작용의 영향을 받을 수 있을 때마다 명시적으로 다시 로드해야 합니다. 부작용은 함수를 호출하거나 프로퍼티를 명령적으로 설정할 때마다 발생할 수 있습니다. [qmllint](https://doc.qt.io/qt-6/qtquick-tool-qmllint.html)는 이에 대한 지침을 제공합니다. 예를 들어, 다음 코드에서 변수 f는 width에 쓰기 연산한 다음에 부작용의 영향을 받습니다. 이는 width가 변경되면 글꼴을 업데이트하는 파생 타입 또는 바인딩 요소에서 바인딩이 있을 수 있기 때문입니다.

```qml
import QtQuick
Text {
    function a() : real {
        var f = font;
        width = f.pixelSize;
        return f.pointSize;
    }
}
```

이 문제를 해결하기 위해 width에 대한 쓰기 연산에서 f를 유지하는 것을 막을 수 있습니다:

```qml
import QtQuick
Text {
    function a() : real {
        var f = font;
        width = f.pixelSize;
        f = font;
        return f.pointSize;
    }
}
```

이것은 다음과 같이 단축될 수 있습니다:

```qml
import QtQuick
Text {
    function a() : real {
        width = font.pixelSize;
        return font.pointSize;
    }
}
```

글꼴 프로퍼티를 다시 가져오는 것이 비용이 많이 든다고 가정할 수 있지만 실제로 QML 엔진은 값 타입 참조를 읽을 때마다 자동으로 새로 고칩니다. 따라서 이것은 1번째 버전보다 비용이 많이 들지 않고 동일한 연산을 표현하는 것이 더 깔끔한 방법입니다.

[타입 어노테이션과 어썰션](https://doc.qt.io/qt-6/qtqml-javascript-hostenvironment.html#type-annotations-and-assertions)도 보십시오.


##### C++로부터 객체 타입 정의하기

When extending QML with C++ code, a C++ class can be registered with the QML type system to enable the class to be used as a data type within QML code. While the properties, methods and signals of any QObject-derived class are accessible from QML, as discussed in Exposing Attributes of C++ Types to QML, such a class cannot be used as a data type from QML until it is registered with the type system. Additionally registration can provide other features, such as allowing a class to be used as an instantiable QML object type from QML, or enabling a singleton instance of the class to be imported and used from QML.

Additionally, the Qt QML module provides mechanisms for implementing QML-specific features such as attached properties and default properties in C++.

(Note that a number of the important concepts covered in this document are demonstrated in the Writing QML Extensions with C++ tutorial.)

NOTE: All headers that declare QML types need to be accessible without any prefix from the project's include path.

For more information about C++ and the different QML integration methods, see the C++ and QML integration overview page.

* Registering C++ Types with the QML Type System

A QObject-derived class can be registered with the QML type system to enable the type to be used as a data type from within QML code.

The engine allows the registration of both instantiable and non-instantiable types. Registering an instantiable type enables a C++ class to be used as the definition of a QML object type, allowing it to be used in object declarations from QML code to create objects of this type. Registration also provides the engine with additional type metadata, enabling the type (and any enums declared by the class) to be used as a data type for property values, method parameters and return values, and signal parameters that are exchanged between QML and C++.

Registering a non-instantiable type also registers the class as a data type in this manner, but the type cannot be used instantiated as a QML object type from QML. This is useful, for example, if a type has enums that should be exposed to QML but the type itself should not be instantiable.

For a quick guide to choosing the correct approach to expose C++ types to QML, see Choosing the Correct Integration Method Between C++ and QML.

* Preconditions

All the macros mentioned below are available from the qqmlregistration.h header. You need to add the following code to the files using them in order to make the macros available:

```cpp
#include <QtQml/qqmlregistration.h>
```

Furthermore, your class declarations have to live in headers reachable via your project's include path. The declarations are used to generate registration code at compile time, and the registration code needs to include the headers that contain the declarations.

* Registering an Instantiable Object Type

Any QObject-derived C++ class can be registered as the definition of a QML object type. Once a class is registered with the QML type system, the class can be declared and instantiated like any other object type from QML code. Once created, a class instance can be manipulated from QML; as Exposing Attributes of C++ Types to QML explains, the properties, methods and signals of any QObject-derived class are accessible from QML code.

To register a QObject-derived class as an instantiable QML object type, add QML_ELEMENT or QML_NAMED_ELEMENT(<name>) to the class declaration. You also need to make adjustments in the build system. For qmake, add CONFIG += qmltypes, a QML_IMPORT_NAME, and a QML_IMPORT_MAJOR_VERSION to your project file. For CMake, the file containing the class should be part of a target set-up with qt_add_qml_module(). This will register the class into the type namespace under the given major version, using either the class name or an explicitly given name as QML type name. The minor version(s) will be derived from any revisions attached to properties, methods, or signals. The default minor version is 0. You can explicitly restrict the type to be available only from specific minor versions by adding the QML_ADDED_IN_MINOR_VERSION() macro to the class declaration. Clients can import suitable versions of the namespace in order to use the type.

For example, suppose there is a Message class with author and creationDate properties:

```cpp
class Message : public QObject
{
    Q_OBJECT
    Q_PROPERTY(QString author READ author WRITE setAuthor NOTIFY authorChanged)
    Q_PROPERTY(QDateTime creationDate READ creationDate WRITE setCreationDate NOTIFY creationDateChanged)
    QML_ELEMENT
public:
    // ...
};
```

This type can be registered by adding an appropriate type namespace and version number to the project file. For example, to make the type available in the com.mycompany.messaging namespace with version 1.0:

* CMake

```
qt_add_qml_module(messaging
    URI com.mycompany.messaging
    VERSION 1.0
    SOURCES
        message.cpp message.h
)
```

qmake
```
CONFIG += qmltypes
QML_IMPORT_NAME = com.mycompany.messaging
QML_IMPORT_MAJOR_VERSION = 1
```

If the header the class is declared in is not accessible from your project's include path, you may have to amend the include path so that the generated registration code can be compiled.

```
INCLUDEPATH += com/mycompany/messaging
```

The type can be used in an object declaration from QML, and its properties can be read and written to, as per the example below:

```qml
import com.mycompany.messaging

Message {
    author: "Amelie"
    creationDate: new Date()
}
```

* Registering Value Types

Any type with a Q_GADGET macro can the registered as a QML value type}. Once such a type is registered with the QML type system it can be used as property type in QML code. Such an instance can be manipulated from QML; as Exposing Attributes of C++ Types to QML explains, the properties and methods of any value type are accessible from QML code.

In contrast to object types, value types require lower case names. The preferred way to register them is using the QML_VALUE_TYPE or QML_ANONYMOUS macros. There is no equivalent to QML_ELEMENT as your C++ classes are typically going to have upper case names. Otherwise the registration is very similar to the registration of object types.

For example, suppose you want to register a value type person that consists of two strings for first and last name:

```cpp
class Person
{
    Q_GADGET
    Q_PROPERTY(QString firstName READ firstName WRITE setFirstName)
    Q_PROPERTY(QString lastName READ lastName WRITE setLastName)
    QML_VALUE_TYPE(person)
public:
    // ...
};
```

There are some further limitations on what you can do with value types:

- Value types cannot be singletons.
- Value types need to be default-constructible and copy-constructible.
- Using QProperty as a member of a value type is problematic. Value types get copied, and you would need to decide what to do with any bindings on the QProperty at that point. You should not use QProperty in value types.
- Value types cannot provide attached properties.
- The API to define extensions to value types (QML_EXTENDED) is not public and subject to future changes.

* Registering Non-Instantiable Types

Sometimes a QObject-derived class may need to be registered with the QML type system but not as an instantiable type. For example, this is the case if a C++ class:

- is an interface type that should not be instantiable
- is a base class type that does not need to be exposed to QML
- declares some enum that should be accessible from QML, but otherwise should not be instantiable
- is a type that should be provided to QML through a singleton instance, and should not be instantiable from QML

The Qt QML module provides several macros for registering non-instantiable types:

- QML_ANONYMOUS registers a C++ type that is not instantiable and cannot be referred to from QML. This enables the engine to coerce any inherited types that are instantiable from QML.
- QML_INTERFACE registers an existing Qt interface type. The type is not instantiable from QML, and you cannot declare QML properties with it. Using C++ properties of this type from QML will do the expected interface casts, though.
- QML_UNCREATABLE(reason) combined with with QML_ELEMENT or QML_NAMED_ELEMENT registers a named C++ type that is not instantiable but should be identifiable as a type to the QML type system. This is useful if a type's enums or attached properties should be accessible from QML but the type itself should not be instantiable. The parameter should be an error message to be emitted if an attempt at creating an instance of the type is detected.
- QML_SINGLETON combined with QML_ELEMENT or QML_NAMED_ELEMENT registers a singleton type that can be imported from QML, as discussed below.
Note that all C++ types registered with the QML type system must be QObject-derived, even if they are non-instantiable.

* Registering Singleton Objects with a Singleton Type

A singleton type enables properties, signals and methods to be exposed in a namespace without requiring the client to manually instantiate an object instance. QObject singleton types in particular are an efficient and convenient way to provide functionality or global property values.

Note that singleton types do not have an associated QQmlContext as they are shared across all contexts in an engine. QObject singleton type instances are constructed and owned by the QQmlEngine, and will be destroyed when the engine is destroyed.

A QObject singleton type can be interacted with in a manner similar to any other QObject or instantiated type, except that only one (engine constructed and owned) instance will exist, and it must be referenced by type name rather than id. Q_PROPERTYs of QObject singleton types may be bound to, and Q_INVOKABLE functions of QObject module APIs may be used in signal handler expressions. This makes singleton types an ideal way to implement styling or theming, and they can also be used instead of ".pragma library" script imports to store global state or to provide global functionality.

Once registered, a QObject singleton type may be imported and used like any other QObject instance exposed to QML. The following example assumes that a QObject singleton type was registered into the "MyThemeModule" namespace with version 1.0, where that QObject has a QColor "color" Q_PROPERTY:

```qml
import MyThemeModule 1.0 as Theme

Rectangle {
    color: Theme.color // binding.
}
```

A QJSValue may also be exposed as a singleton type, however clients should be aware that properties of such a singleton type cannot be bound to.

See QML_SINGLETON for more information on how implement and register a new singleton type, and how to use an existing singleton type.

Note: Enum values for registered types in QML should start with a capital.

* Final properties

Properties declared final using the FINAL modifier to Q_PROPERTY cannot be overridden. This means that any properties or functions of the same name, declared either in QML or in C++ on derived types, are ignored by the QML engine. You should declare properties FINAL when possible, in order to avoid accidental overrides. An override of a property is visible not only in derived classes, but also to QML code executing the context of the base class. Such QML code, typically expects the original property, though. This is a frequent source of mistakes.

Properties declared FINAL can also not be overridden by functions in QML, or by Q_INVOKABLE methods in C++.

* Type Revisions and Versions

Many of the type registration functions require versions to be specified for the registered type. Type revisions and versions allow new properties or methods to exist in the new version while remaining compatible with previous versions.

Consider these two QML files:

```qml
// main.qml
import QtQuick 1.0

Item {
    id: root
    MyType {}
}
```

```qml
// MyType.qml
import MyTypes 1.0

CppType {
    value: root.x
}
```

where CppType maps to the C++ class CppType.

If the author of CppType adds a root property to CppType in a new version of their type definition, root.x now resolves to a different value because root is also the id of the top level component. The author could specify that the new root property is available from a specific minor version. This permits new properties and features to be added to existing types without breaking existing programs.

The REVISION tag is used to mark the root property as added in revision 1 of the type. Methods such as Q_INVOKABLE's, signals and slots can also be tagged for a revision using the Q_REVISION(x) macro:

```cpp
class CppType : public BaseType
{
    Q_OBJECT
    Q_PROPERTY(int root READ root WRITE setRoot NOTIFY rootChanged REVISION 1)
    QML_ELEMENT

signals:
    Q_REVISION(1) void rootChanged();
};
```

The revisions given this way are automatically interpreted as minor versions to the major version given in the project file. In this case, root is only available when MyTypes version 1.1 or higher is imported. Imports of MyTypes version 1.0 remain unaffected.

For the same reason, new types introduced in later versions should be tagged with the QML_ADDED_IN_MINOR_VERSION macro.

This feature of the language allows for behavioural changes to be made without breaking existing applications. Consequently QML module authors should always remember to document what changed between minor versions, and QML module users should check that their application still runs correctly before deploying an updated import statement.

Revisions of a base class that your type depends upon are automatically registered when registering the type itself. This is useful when deriving from base classes provided by other authors, e.g. when extending classes from the Qt Quick module.

Note: The QML engine does not support revisions for properties or signals of grouped and attached property objects.

* Registering Extension Objects

When integrating existing classes and technology into QML, APIs will often need tweaking to fit better into the declarative environment. Although the best results are usually obtained by modifying the original classes directly, if this is either not possible or is complicated by some other concerns, extension objects allow limited extension possibilities without direct modifications.

Extension objects add additional properties to an existing type. An extended type definition allows the programmer to supply an additional type, known as the extension type, when registering the class. Its members are transparently merged with the original target class when used from within QML. For example:

```qml
QLineEdit {
    leftMargin: 20
}
```

The leftMargin property is a new property added to an existing C++ type, QLineEdit, without modifying its source code.

The QML_EXTENDED(extension) macro is for registering extended types. The argument is the name of another class to be used as extension.

You can also use QML_EXTENDED_NAMESPACE(namespace) to register a namespace, and especially the enumerations declared within, as an extension to a type. If the type you are extending is itself a namespace, you need to use QML_NAMESPACE_EXTENDED(namespace) instead.

An extension class is a regular QObject, with a constructor that takes a QObject pointer. However, the extension class creation is delayed until the first extended property is accessed. The extension class is created and the target object is passed in as the parent. When the property on the original is accessed, the corresponding property on the extension object is used instead.

* Registering Foreign Types

There may be C++ types that cannot be modified to hold the above mentioned macros. Those may be types from 3rdparty libraries, or types that need to fulfill some contract that contradicts the presence of those macros. You can still expose those types to QML, though, using the QML_FOREIGN macro. In order to do this, create a separate struct that consists entirely of the registration macros, like this:

```cpp
// Contains class Immutable3rdParty
#include <3rdpartyheader.h>

struct Foreign
{
    Q_GADGET
    QML_FOREIGN(Immutable3rdParty)
    QML_NAMED_ELEMENT(Accessible3rdParty)
    QML_ADDED_IN_VERSION(2, 4)
    // QML_EXTENDED, QML_SINGLETON ...
};
```

From this code, you get a QML type with the methods and properties of Immutable3rdParty, and the QML traits (e.g.: singleton, extended) specified in Foreign.

* Defining QML-Specific Types and Attributes

* Providing Attached Properties

In the QML language syntax, there is a notion of attached properties and attached signal handlers, which are additional attributes that are attached to an object. Essentially, such attributes are implemented and provided by an attaching type, and these attributes may be attached to an object of another type. This contrasts with ordinary object properties which are provided by the object type itself (or the object's inherited type).

For example, the Item below uses attached properties and attached handlers:

```qml
import QtQuick 2.0

Item {
    width: 100; height: 100

    focus: true
    Keys.enabled: false
    Keys.onReturnPressed: console.log("Return key was pressed")
}
```

Here, the Item object is able to access and set the values of Keys.enabled and Keys.onReturnPressed. This allows the Item object to access these extra attributes as an extension to its own existing attributes.

* Steps for Implementing Attached Objects

When considering the above example, there are several parties involved:

- There is an instance of an anonymous attached object type, with an enabled and a returnPressed signal, that has been attached to the Item object to enable it to access and set these attributes.
- The Item object is the attachee, to which the instance of the attached object type has been attached.
- Keys is the attaching type, which provides the attachee with a named qualifier, "Keys", through which it may access the attributes of the attached object type.

When the QML engine processes this code, it creates a single instance of the attached object type and attaches this instance to the Item object, thereby providing it with access to the enabled and returnPressed attributes of the instance.

The mechanisms for providing attached objects can be implemented from C++ by providing classes for the attached object type and attaching type. For the attached object type, provide a QObject-derived class that defines the attributes to be made accessible to attachee objects. For the attaching type, provide a QObject-derived class that:

- implements a static qmlAttachedProperties() with the following signature:
```cpp
static <AttachedPropertiesType> *qmlAttachedProperties(QObject *object);
```
This method should return an instance of the attached object type.

The QML engine invokes this method in order to attach an instance of the attached object type to the attachee specified by the object parameter. It is customary, though not strictly required, for this method implementation to parent the returned instance to object in order to prevent memory leaks.

This method is called at most once by the engine for each attachee object instance, as the engine caches the returned instance pointer for subsequent attached property accesses. Consequently the attachment object may not be deleted until the attachee object is destroyed.

- is declared as an attaching type, by adding the QML_ATTACHED(attached) macro to the class declaration. The argument is the name of the attached object type

* Implementing Attached Objects: An Example

For example, take the Message type described in an earlier example:

```cpp
class Message : public QObject
{
    Q_OBJECT
    Q_PROPERTY(QString author READ author WRITE setAuthor NOTIFY authorChanged)
    Q_PROPERTY(QDateTime creationDate READ creationDate WRITE setCreationDate NOTIFY creationDateChanged)
    QML_ELEMENT
public:
    // ...
};
```

Suppose it is necessary to trigger a signal on a Message when it is published to a message board, and also track when the message has expired on the message board. Since it doesn't make sense to add these attributes directly to a Message, as the attributes are more relevant to the message board context, they could be implemented as attached attributes on a Message object that are provided through a "MessageBoard" qualifier. In terms of the concepts described earlier, the parties involved here are:

- An instance of an anonymous attached object type, which provides a published signal and an expired property. This type is implemented by MessageBoardAttachedType below
- A Message object, which will be the attachee
- The MessageBoard type, which will be the attaching type that is used by Message objects to access the attached attributes

Following is an example implementation. First, there needs to be an attached object type with the necessary properties and signals that will be accessible to the attachee:

```cpp
class MessageBoardAttachedType : public QObject
{
    Q_OBJECT
    Q_PROPERTY(bool expired READ expired WRITE setExpired NOTIFY expiredChanged)
    QML_ANONYMOUS
public:
    MessageBoardAttachedType(QObject *parent);
    bool expired() const;
    void setExpired(bool expired);
signals:
    void published();
    void expiredChanged();
};
```

Then the attaching type, MessageBoard, must declare a qmlAttachedProperties() method that returns an instance of the attached object type as implemented by MessageBoardAttachedType. Additionally, MessageBoard must be declared as an attaching type via the QML_ATTACHED() macro:

```cpp
class MessageBoard : public QObject
{
    Q_OBJECT
    QML_ATTACHED(MessageBoardAttachedType)
    QML_ELEMENT
public:
    static MessageBoardAttachedType *qmlAttachedProperties(QObject *object)
    {
        return new MessageBoardAttachedType(object);
    }
};
```

Now, a Message type can access the properties and signals of the attached object type:

```cpp
Message {
    author: "Amelie"
    creationDate: new Date()

    MessageBoard.expired: creationDate < new Date("January 01, 2015 10:45:00")
    MessageBoard.onPublished: console.log("Message by", author, "has been
published!")
}
```

Additionally, the C++ implementation may access the attached object instance that has been attached to any object by calling the qmlAttachedPropertiesObject() function.

For example:

```cpp
Message *msg = someMessageInstance();
MessageBoardAttachedType *attached =
        qobject_cast<MessageBoardAttachedType*>(qmlAttachedPropertiesObject<MessageBoard>(msg));

qDebug() << "Value of MessageBoard.expired:" << attached->expired();
```

* Propagating Attached Properties

QQuickAttachedPropertyPropagator can be subclassed to propagate attached properties from a parent object to its children, similar to font and palette propagation. It supports propagation through items, popups, and windows.

* Property Modifier Types

A property modifier type is a special kind of QML object type. A property modifier type instance affects a property (of a QML object instance) which it is applied to. There are two different kinds of property modifier types:

- property value write interceptors
- property value sources

A property value write interceptor can be used to filter or modify values as they are written to properties. Currently, the only supported property value write interceptor is the Behavior type provided by the QtQuick import.

A property value source can be used to automatically update the value of a property over time. Clients can define their own property value source types. The various property animation types provided by the QtQuick import are examples of property value sources.

Property modifier type instances can be created and applied to a property of a QML object through the "<ModifierType> on <propertyName>" syntax, as the following example shows:

```qml
import QtQuick 2.0

Item {
    width: 400
    height: 50

    Rectangle {
        width: 50
        height: 50
        color: "red"

        NumberAnimation on x {
            from: 0
            to: 350
            loops: Animation.Infinite
            duration: 2000
        }
    }
}
```

This is commonly referred to as "on" syntax.

Clients can register their own property value source types, but currently not property value write interceptors.

* Property Value Sources

Property value sources are QML types that can automatically update the value of a property over time, using the <PropertyValueSource> on <property> syntax. For example, the various property animation types provided by the QtQuick module are examples of property value sources.

A property value source can be implemented in C++ by subclassing QQmlPropertyValueSource and providing an implementation that writes different values to a property over time. When the property value source is applied to a property using the <PropertyValueSource> on <property> syntax in QML, it is given a reference to this property by the engine so that the property value can be updated.

For example, suppose there is a RandomNumberGenerator class to be made available as a property value source, so that when applied to a QML property, it will update the property value to a different random number every 500 milliseconds. Additionally, a maxValue can be provided to this random number generator. This class can be implemented as follows:

```cpp
class RandomNumberGenerator : public QObject, public QQmlPropertyValueSource
{
    Q_OBJECT
    Q_INTERFACES(QQmlPropertyValueSource)
    Q_PROPERTY(int maxValue READ maxValue WRITE setMaxValue NOTIFY maxValueChanged);
    QML_ELEMENT
public:
    RandomNumberGenerator(QObject *parent)
        : QObject(parent), m_maxValue(100)
    {
        QObject::connect(&m_timer, SIGNAL(timeout()), SLOT(updateProperty()));
        m_timer.start(500);
    }

    int maxValue() const;
    void setMaxValue(int maxValue);

    virtual void setTarget(const QQmlProperty &prop) { m_targetProperty = prop; }

signals:
    void maxValueChanged();

private slots:
    void updateProperty() {
        m_targetProperty.write(QRandomGenerator::global()->bounded(m_maxValue));
    }

private:
    QQmlProperty m_targetProperty;
    QTimer m_timer;
    int m_maxValue;
};
```

When the QML engine encounters a use of RandomNumberGenerator as a property value source, it invokes RandomNumberGenerator::setTarget() to provide the type with the property to which the value source has been applied. When the internal timer in RandomNumberGenerator triggers every 500 milliseconds, it will write a new number value to that specified property.

Once the RandomNumberGenerator class has been registered with the QML type system, it can be used from QML as a property value source. Below, it is used to change the width of a Rectangle every 500 milliseconds:

```qml
import QtQuick 2.0

Item {
    width: 300; height: 300

    Rectangle {
        RandomNumberGenerator on width { maxValue: 300 }

        height: 100
        color: "red"
    }
}
```

In all other respects, property value sources are regular QML types that can have properties, signals methods and so on, but with the added capability that they can be used to change property values using the <PropertyValueSource> on <property> syntax.

When a property value source object is assigned to a property, QML first tries to assign it normally, as though it were a regular QML type. Only if this assignment fails does the engine call the setTarget() method. This allows the type to also be used in contexts other than just as a value source.

* Specifying Default and Parent Properties for QML Object Types

Any QObject-derived type that is registered as an instantiable QML object type can optionally specify a default property for the type. A default property is the property to which an object's children are automatically assigned if they are not assigned to any specific property.

The default property can be set by calling the Q_CLASSINFO() macro for a class with a specific "DefaultProperty" value. For example, the MessageBoard class below specifies its messages property as the default property for the class:

```cpp
class MessageBoard : public QObject
{
    Q_OBJECT
    Q_PROPERTY(QQmlListProperty<Message> messages READ messages)
    Q_CLASSINFO("DefaultProperty", "messages")
    QML_ELEMENT
public:
    QQmlListProperty<Message> messages();

private:
    QList<Message *> m_messages;
};
```

This enables children of a MessageBoard object to be automatically assigned to its messages property if they are not assigned to a specific property. For example:

```qml
MessageBoard {
    Message { author: "Naomi" }
    Message { author: "Clancy" }
}
```

If messages was not set as the default property, then any Message objects would have to be explicitly assigned to the messages property instead, as follows:

```qml
MessageBoard {
    messages: [
        Message { author: "Naomi" },
        Message { author: "Clancy" }
    ]
}
```

(Incidentally, the Item::data property is its default property. Any Item objects added to this data property are also added to the list of Item::children, so the use of the default property enables visual children to be declared for an item without explicitly assigning them to the children property.)

Additionally, you can declare a "ParentProperty" Q_CLASSINFO() to inform the QML engine which property should denote the parent object in the QML hierarchy. For example, the Message type might be declared as follows:

```cpp
class Message : public QObject
{
    Q_OBJECT
    Q_PROPERTY(QObject* board READ board BINDABLE boardBindable)
    Q_PROPERTY(QString author READ author BINDABLE authorBindable)
    Q_CLASSINFO("ParentProperty", "board")
    QML_ELEMENT

public:
    Message(QObject *parent = nullptr) : QObject(parent) { m_board = parent; }

    QObject *board() const { return m_board.value(); }
    QBindable<QObject *> boardBindable() { return QBindable<QObject *>(&m_board); }

    QString author() const { return m_author.value(); }
    QBindable<QString> authorBindable() { return QBindable<QString>(&m_author); }

private:
    QProperty<QObject *> m_board;
    QProperty<QString> m_author;
};
```

Defining the parent property affords qmllint and other tools better insight into the intention of your code and avoids false positive warnings on some property accesses.

* Defining Visual Items with the Qt Quick Module

When building user interfaces with the Qt Quick module, all QML objects that are to be visually rendered must derive from the Item type, as it is the base type for all visual objects in Qt Quick. This Item type is implemented by the QQuickItem C++ class, which is provided by the Qt Quick module. Therefore, this class should be subclassed when it is necessary to implement a visual type in C++ that can be integrated into a QML-based user interface.

See the QQuickItem documentation for more information. Additionally, the Writing QML Extensions with C++ tutorial demonstrates how a QQuickItem-based visual item can be implemented in C++ and integrated into a Qt Quick-based user interface.

* Receiving Notifications for Object Initialization

For some custom QML object types, it may be beneficial to delay the initialization of particular data until the object has been created and all of its properties have been set. For example, this may be the case if the initialization is costly, or if the initialization should not be performed until all property values have been initialized.

The Qt QML module provides the QQmlParserStatus to be subclassed for these purposes. It defines a number of virtual methods that are invoked at various stages during component instantiation. To receive these notifications, a C++ class should inherit QQmlParserStatus and also notify the Qt meta system using the Q_INTERFACES() macro.

For example:

```cpp
class MyQmlType : public QObject, public QQmlParserStatus
{
    Q_OBJECT
    Q_INTERFACES(QQmlParserStatus)
    QML_ELEMENT
public:
    virtual void componentComplete()
    {
        // Perform some initialization here now that the object is fully created
    }
};
```

---

#### QML 모듈

A QML module provides versioned types and JavaScript resources in a type namespace which may be used by clients who import the module. The types which a module provides may be defined in C++ within a plugin, or in QML documents. Modules make use of the QML versioning system which allows modules to be independently updated.

Defining of a QML module allows:

- The sharing of common QML types within a project - for example, a group of UI components that are used by different windows
- The distribution of QML-based libraries
- The modularization of distinct features, so that applications only load the libraries necessary for their individual needs
- Versioning of types and resources so that the module can be updated safely without breaking client code

* Defining a QML Module

A module is defined by a module definition qmldir file. Each module has an associated type namespace, which is the module's identifier. A module can provide QML object types (defined either by QML documents or via a C++ plugin) and JavaScript resources, and may be imported by clients.

To define a module, a developer should gather together the various QML documents, JavaScript resources and C++ plugins which belong in the module into a single directory, and write an appropriate module definition qmldir file which should also be placed into the directory. The directory can then be installed into the QML import path as a module.

Note that defining a module is not the only way to share common QML types within a project - a simple QML document directory import may also be used for this purpose.

* Supported QML Module Types

There are two different types of modules supported by QML:

- Identified Modules
- Legacy Modules (deprecated)

Identified modules explicitly define their identifier and are installed into QML import path. Identified modules are more maintainable (due to type versioning) and are provided with type registration guarantees by the QML engine which are not provided to legacy modules. Legacy modules are only supported to allow legacy code to continue to work with the latest version of QML, and should be avoided by clients if possible.

Clients may import a QML module from within QML documents or JavaScript files. Please see the documentation about importing a QML module for more information on the topic.

* Providing Types and Functionality in a C++ Plugin

An application which has a lot of logic implemented in C++, or which defines types in C++ and exposes them to QML, may wish to implement a QML plugin. A QML extension module developer may wish to implement some types in a C++ plugin (as opposed to defining them via QML documents) to achieve better performance or for greater flexibility.

Every C++ plugin for QML has an initialiatization function which is called by the QML engine when it loads the plugin. This initialization function must register any types that the plugin provides, but must not do anything else (for example, instantiating QObjects is not allowed).

See Creating C++ Plugins For QML for more information.


##### QML 모듈 지정하기

-


##### 지원되는 QML 모듈 타입 - Identified 모듈

-


##### 지원되는 QML 모듈 타입 - Legacy 모듈

-


##### C++ 플러그인에서 타입 및 기능 제공하기

-

---

#### QML 문서

-


##### QML 문서의 구조

-


##### QML 문서를 통한 객체 타입 정의하기

-


##### 리소스 로딩 및 네트워크 투명성

-


##### 범위 및 네이밍 규칙

-
