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
  - [값 타입](https://doc.qt.io/qt-6/qmlvaluetypes.html): bool, int, real, double, list, string, enumeration, date, url, var(variant), void, ...
  - [객체 타입](https://doc.qt.io/qt-6/qmltypes.html): color, font, point, size, rect, quaternion, vector2d, vector3d, vector4d, matrix4x4, ...
  - 포괄적으로는 값 타입도 객체 타입에 포함됨

* Qt Essentials 모듈의 분류
  | 모듈 | 설명 |
  | --- | --- |
  | [Qt Core](https://doc.qt.io/qt-6/qtcore-index.html) | 다른 모듈들이 사용하는 코어 비-그래픽 클래스. (시그널-슬롯 함수, 프로퍼티 관련) |
  | [Qt D-Bus](https://doc.qt.io/qt-6/qtdbus-index.html) | D-Bus 프로토콜을 통한 IPC(프로세스간 통신)를 위한 클래스. |
  | [Qt GUI](https://doc.qt.io/qt-6/qtgui-index.html) | GUI 컴포넌트를 위한 베이스 클래스. |
  | [Qt Network](https://doc.qt.io/qt-6/qtnetwork-index.html) | 네트워크 프로그래밍을 용이하게 해주고 이식성을 높여주는 클래스. |
  | [Qt QML](https://doc.qt.io/qt-6/qtqml-index.html) | QML과 JavaScript 언어를 위한 클래스. |
  | [Qt Quick](https://doc.qt.io/qt-6/qtquick-index.html) | 커스텀 사용자 인터페이스를 통해 동적 애플리케이션을 구축하기 위한 선언적 프레임워크. |
  | [Qt Quick Controls](https://doc.qt.io/qt-6/qtquickcontrols-index.html) | 데스크톱, 임베디드 및 모바일 장치를 위한 성능이 뛰어난 사용자 인터페이스를 생성하기 위한 경량 QML 타입 제공. |
  | [Qt Quick Dialogs](https://doc.qt.io/qt-6/qtquickdialogs-index.html) | Qt Quick 응용프로그램에서 시스템 다이얼로그를 만들고 상호 작용하기 위한 타입. |
  | [Qt Quick Layouts](https://doc.qt.io/qt-6/qtquicklayouts-index.html) | 레이아웃은 사용자 인터페이스에서 Qt Quick 2 기반 항목을 정렬하는 데 사용되는 항목. |
  | [Qt Quick Tests](https://doc.qt.io/qt-6/qtquicktest-index.html) | 테스트 케이스를 JavaScript 함수로 작성하는 QML 응용프로그램의 단위 테스트 프레임워크. 참고: Qt Quick Test에는 바이너리 호환성 보장이 적용되지 않지만 소스 호환성은 유지됨. |
  | [Qt Test](https://doc.qt.io/qt-6/qttest-index.html) | 단위 테스트용 Qt 응용프로그램 및 라이브러리의 클래스. 참고: Qt Test에는 바이너리 호환성 보장이 적용되지 않지만 소스 호환성은 유지됨. |
  | [Qt Widgets](https://doc.qt.io/qt-6/qtwidgets-index.html) | C++ 위젯으로 Qt GUI를 확장하기 위한 클래스. |

* QML Add-On 모듈의 분류
  | 모듈 | 개발 플랫폼 | 대상 플랫폼 | 설명 |
  | --- | ----------- | ---------- | --- |
  | [Active Qt](https://doc.qt.io/qt-6/activeqt-index.html) | [Windows](https://doc.qt.io/qt-6/windows.html) | [Windows](https://doc.qt.io/qt-6/windows.html) | ActiveX와 COM을 사용하는 애플리케이션을 위한 클래스 |
  | [Qt 3D](https://doc.qt.io/qt-6/qt3d-index.html) | All | All | 2D 및 3D 렌더링을 지원하는 근실시간 시뮬레이션 시스템을 위한 기능 |
  | [Qt 5 Core Compatibility APIs](https://doc.qt.io/qt-6/qtcore5-index.html) | All | All | Qt 6가 아닌 Qt 5에 있던 Qt Core API |
  | [Qt Bluetooth](https://doc.qt.io/qt-6/qtbluetooth-index.html) | All | [Android](https://doc.qt.io/qt-6/android.html), [iOS](https://doc.qt.io/qt-6/ios.html), [Linux](https://doc.qt.io/qt-6/linux.html), [Boot to Qt](https://doc.qt.io/Boot2Qt/), [macOS](https://doc.qt.io/qt-6/macos.html) and [Windows](https://doc.qt.io/qt-6/windows.html) | Bluetooth 하드웨어에 대한 접근을 제공함 |
  | [Qt Concurrent](https://doc.qt.io/qt-6/qtconcurrent-index.html) | All | All | 저수준 쓰레드 피리미티브를 사용하지 않고 멀티-쓰레드 프로그램 작성을 위한 클래스 |
  | [Qt Help](https://doc.qt.io/qt-6/qthelp-index.html) | All | All | 애플리케이션에 문서를 통합하기 위한 클래스 |
  | [Qt Image Formats](https://doc.qt.io/qt-6/qtimageformats-index.html) | All | All | 이미지 포맷에 대한 플러그인: TIFF, MNG, TGA, WBMP |
  | [Qt Multimedia](https://doc.qt.io/qt-6/qtmultimedia-index.html) | All | All* | 멀티미디어 컨텐츠를 처리할 수 있는 QML 타입과 C++ 클래스가 풍부하게 구성되어 있으며, 카메라 접근을 처리할 수 있는 API가 포함되어 있음 |
  | [Qt NFC](https://doc.qt.io/qt-6/qtnfc-index.html) | All | [Android](https://doc.qt.io/qt-6/android.html), [iOS](https://doc.qt.io/qt-6/ios.html), [macOS](https://doc.qt.io/qt-6/macos.html), [Linux](https://doc.qt.io/qt-6/linux.html) and [Windows](https://doc.qt.io/qt-6/windows.html) | NFC(Near-Field Communication) 하드웨어에 대한 접근를 제공함. 데스크톱 플랫폼에서는 타입 4 태그에 대해서만 NDEF 접근이 지원됨 |
  | [Qt OPC UA](https://doc.qt.io/qt-6/qtopcua-overview.html) | All | All (except QNX, WebAssembly) | 산업용 애플리케이션의 데이터 모델링 및 데이터 교환에 대한 프로토콜 |
  | [Qt OpenGL](https://doc.qt.io/qt-6/qtopengl-index.html) | All | All | Qt 애플리케이션에서 OpenGL을 쉽게 사용할 수 있는 C++ 클래스. 별도의 [Qt OpenGL Widget C++ 클래스](https://doc.qt.io/qt-6/qtopenglwidgets-module.html) 라이브러리를 통해 OpenGL 그래픽을 렌더링할 수 있는 위젯을 제공함 |
  | [Qt PDF](https://doc.qt.io/qt-6/qtpdf-index.html) | All | [Windows](https://doc.qt.io/qt-6/windows.html), [Linux](https://doc.qt.io/qt-6/linux.html) and [macOS](https://doc.qt.io/qt-6/macos.html) | PDF 문서를 렌더링하기 위한 클래스와 함수 |
  | [Qt Positioning](https://doc.qt.io/qt-6/qtpositioning-index.html) | All | [Android](https://doc.qt.io/qt-6/android.html), [iOS](https://doc.qt.io/qt-6/ios.html), [macOS](https://doc.qt.io/qt-6/macos.html), [Linux](https://doc.qt.io/qt-6/linux.html) and [Windows](https://doc.qt.io/qt-6/windows.html) | 위치, 위성 정보 및 지역 모니터링 클래스에 대한 접근을 제공함 |
  | [Qt Print Support](https://doc.qt.io/qt-6/qtprintsupport-index.html) | All | All (except iOS) | 인쇄를 더욱 쉽고 이식하기 쉽게 해주는 클래스 |
  | [Qt Quick Widgets](https://doc.qt.io/qt-6/qtquickwidgets-index.html) | All | All | Qt Quick 사용자 인터페이스를 표시하기 위한 C++ 위젯 클래스를 제공함 |
  | [Qt Quick Effects](https://doc.qt.io/qt-6/qtquick-effects-qmlmodule.html) | All | All | Qt Quick 항목에 하나 이상의 간단한 그래픽 효과를 적용할 수 있는 QML 타입을 제공함 |
  | [Qt Quick Particles](https://doc.qt.io/qt-6/qtquick-particles-qmlmodule.html) | All | All | 파티클 효과를 위한 QML 타입을 제공함 |
  | [Qt Remote Objects](https://doc.qt.io/qt-6/qtremoteobjects-index.html#qt-remote-objects) | All | All | [QObject](https://doc.qt.io/qt-6/qobject.html)의 API(프로퍼티/시그널/슬롯)를 프로세스 또는 장치 간에 공유하기 위해 사용하기 쉬운 메커니즘을 제공함 |
  | [Qt SCXML](https://doc.qt.io/qt-6/qtscxml-index.html) | All | All | SCXML 파일에서 상태 머신을 생성하고 애플리케이션에 포함하기 위한 클래스 및 도구를 제공함 |
  | [Qt Sensors](https://doc.qt.io/qt-6/qtsensors-index.html) | All | [Android](https://doc.qt.io/qt-6/android.html), [iOS](https://doc.qt.io/qt-6/ios.html) and [Windows](https://doc.qt.io/qt-6/windows.html) | 센서 하드웨어에 대한 접근을 제공함
  | [Qt Serial Bus](https://doc.qt.io/qt-6/qtserialbus-index.html) | All | [Linux](https://doc.qt.io/qt-6/linux.html), [Boot to Qt](https://doc.qt.io/Boot2Qt/), [macOS](https://doc.qt.io/qt-6/macos.html) and [Windows](https://doc.qt.io/qt-6/windows.html) | 시리얼 산업 버스 인터페이스에 대한 액세스를 제공함. 현재 모듈은 CAN 버스 및 Modbus 프로토콜을 지원함 |
  | [Qt Serial Port](https://doc.qt.io/qt-6/qtserialport-index.html) | All | [Linux](https://doc.qt.io/qt-6/linux.html), [Boot to Qt](https://doc.qt.io/Boot2Qt/), [macOS](https://doc.qt.io/qt-6/macos.html) and [Windows](https://doc.qt.io/qt-6/windows.html) | 하드웨어와 가상 시리얼 포트와 상호작용하는 클래스를 제공함 |
  | [Qt Shader Tools](https://doc.qt.io/qt-6/qtshadertools-index.html) | All | All | 크로스 플랫폼 Qt 셰이더 파이프라인을 위한 도구를 제공함. 이를 통해 그래픽을 처리하고 셰이더를 계산하여 Qt Quick 및 Qt 생태계의 다른 컴포넌트에 사용할 수 있음 |
  | [Qt Spatial Audio](https://doc.qt.io/qt-6/qtspatialaudio-index.html) | All | All | 공간 오디오에 대한 지원을 제공함. 다양한 음원과 리버브와 같은 룸 관련 속성을 포함하는 3D 공간에 사운드 장면을 만듦 |
  | [Qt SQL](https://doc.qt.io/qt-6/qtsql-index.html) | All | All | SQL을 사용하는 데이터베이스 통합에 대한 클래스 |
  | [Qt State Machine](https://doc.qt.io/qt-6/qtstatemachine-index.html) | All | All | 상태 그래프를 생성하고 실행하기 위한 클래스를 제공함 |
  | [Qt SVG](https://doc.qt.io/qt-6/qtsvg-index.html) | All | All | SVG 파일의 내용을 표시하기 위한 클래스. [SVG 1.2 Tiny](http://www.w3.org/TR/SVGTiny12/) 표준의 서브셋을 지원함. 별도의 [Qt SVG Widget C++ 클래스](https://doc.qt.io/qt-6/qtsvgwidgets-module.html) 라이브러리를 통해 위젯 UI에서 SVG 파일을 렌더링할 수 있음 |
  | [Qt TextToSpeech](https://doc.qt.io/qt-6/qttexttospeech-index.html) | All | All | 텍스트에서 음성을 합성하여 오디오 출력으로 재생할 수 있도록 지원함 |
  | [Qt UI Tools](https://doc.qt.io/qt-6/qtuitools-index.html) | All | All | Qt Designer에서 작성된 [QWidget](https://doc.qt.io/qt-6/qwidget.html) 기반 폼을 런타임에 동적으로 로드하기 위한 클래스 |
  | [Qt WebChannel](https://doc.qt.io/qt-6/qtwebchannel-index.html) | All | All | HTML 클라이언트의 [QObject](https://doc.qt.io/qt-6/qobject.html) 또는 QML 객체에 대한 접근을 제공하여 Qt 애플리케이션을 HTML/JavaScript 클라이언트와 원활하게 통합함 |
  | [Qt WebEngine](https://doc.qt.io/qt-6/qtwebengine-index.html) | All | [Windows](https://doc.qt.io/qt-6/windows.html), [Linux](https://doc.qt.io/qt-6/linux.html) and [macOS](https://doc.qt.io/qt-6/macos.html) | [Chromium 브라우저 프로젝트](http://www.chromium.org/Home)를 이용하여 웹 콘텐츠를 애플리케이션에 내장하기 위한 클래스와 함수 |
  | [Qt WebSockets](https://doc.qt.io/qt-6/qtwebsockets-index.html) | All | All | [RFC 6455](https://datatracker.ietf.org/doc/html/rfc6455)를 준수하는 WebSocket 통신을 제공함 |
  | [Qt WebView](https://doc.qt.io/qt-6/qtwebview-index.html) | All | Platforms with native web engine | 전체 웹 브라우저 스택을 포함할 필요 없이 플랫폼 고유의 API를 사용하여 QML 애플리케이션에 웹 콘텐츠를 표시함 |
  | [Qt XML](https://doc.qt.io/qt-6/qtxml-index.html) | All | All | DOM(Document Object Model) API에서 XML을 처리함 |
  |  |  |  | 여기서부터는 상업용 라이선스, GNU GPL v3를 따르는 애드온 |
  | [Qt Charts](https://doc.qt.io/qt-6/qtcharts-index.html) | All | All | 정적 또는 동적 데이터 모델에 의해 구동되는 시각적으로 보기 좋은 차트를 표시하기 위한 UI 컴포넌트 |
  | [Qt CoAP](https://doc.qt.io/qt-6/qtcoap-index.html) | All | All | RFC 7252에서 정의한 CoAP의 클라이언트 사이드를 구현함 |
  | [Qt Data Visualization](https://doc.qt.io/qt-6/qtdatavisualization-index.html) | All | All | 놀라운 3D 데이터 시각화를 만들기 위한 UI 컴포넌트 |
  | [Qt Lottie Animation](https://doc.qt.io/qt-6/qtlottieanimation-index.html) | All | All | Adobe® After Effects용 [Bodymovin](https://aescripts.com/bodymovin/) 플러그인이 내보내는 JSON 포맷의 그래픽 및 애니메이션 렌더링을 위한 QML API |
  | [Qt MQTT](https://doc.qt.io/qt-6/qtmqtt-index.html) | All | All | MQTT 프로토콜 사양의 구현을 제공함 |
  | [Qt Network Authorization](https://doc.qt.io/qt-6/qtnetworkauth-index.html) | All | All | 온라인 서비스에 대한 OAuth-기반 인증 지원을 제공함 |
  | [Qt Quick 3D](https://doc.qt.io/qt-6/qtquick3d-index.html) | All | All | Qt Quick을 기반으로 3D 컨텐츠 또는 UI를 생성하기 위한 고급 API를 제공함 |
  | [Qt Quick 3D Physics](https://doc.qt.io/qt-6/qtquick3dphysics-index.html) | All | All supported platforms except QNX and INTEGRITY | Qt Quick 3D Physics는 Qt Quick 3D에 물리 시뮬레이션 기능을 추가한 높은 수준의 QML 모듈을 제공함 |
  | [Qt Quick Timeline](https://doc.qt.io/qt-6/qtquicktimeline-index.html) | All | All | 키프레임 기반 애니메이션 및 파라미터화를 활성화 |
  | [Qt Virtual Keyboard](https://doc.qt.io/qt-6/qtvirtualkeyboard-index.html) | All | [Linux](https://doc.qt.io/qt-6/linux.html), [Windows](https://doc.qt.io/qt-6/windows.html) and [Boot to Qt](https://doc.qt.io/Boot2Qt/) | QML 가상 키보드 뿐만 아니라 다양한 입력 방식을 구현하기 위한 프레임워크이며, 지역화된 키보드 레이아웃과 커스텀 시각적 테마를 지원함 |
  | [Qt Wayland Compositor](https://doc.qt.io/qt-6/qtwaylandcompositor-index.html) | [Linux](https://doc.qt.io/qt-6/linux.html) | Linux and [Boot to Qt](https://doc.qt.io/Boot2Qt/) | Wayland Compositor를 개발하기 위한 프레임워크를 제공함 |

* QML 모듈의 분류

* 모듈로 구분된 C++ 클래스의 분류

* 자료 링크
  - [모든 Qt 레퍼런스](https://doc.qt.io/qt-6/reference-overview.html)
  - [예제 및 튜토리얼](https://doc.qt.io/qt-6/qtexamplesandtutorials.html)
  - [모든 QML 모듈](https://doc.qt.io/qt-6/modules-qml.html)
  - [모든 Qt 모듈](https://doc.qt.io/qt-6/qtmodules.html)
  - [모듈로 구분된 모든 C++ 클래스](https://doc.qt.io/qt-6/modules-cpp.html)

