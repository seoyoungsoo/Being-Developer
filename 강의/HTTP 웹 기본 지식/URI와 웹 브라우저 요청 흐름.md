# URI(Uniform Resource Identifier)

## URI? URL? URN?

### URI는 로케이터(locater), 이름(name) 또는 둘 다 추가로 분류될 수 있다.

![Untitled](https://user-images.githubusercontent.com/58387974/127986267-ffe9a8a5-7a76-4864-8013-681217adb3ab.png)

- URL - Resource Locator
- URN - Resource Name

### URL과 URN 예시

![Untitled (1)](https://user-images.githubusercontent.com/58387974/127986275-43bd6871-f411-4c23-8a7a-118f8be3f8c6.png)

## URI

### 단어 뜻

- Uniform - 리소스 식별하는 통일된 방식
- Resource - 자원, URI로 식별할 수 있는 모든 것(제한 없음)
- Identifier - 다른 항목과 구분하는데 필요한 정보

## URL, URN

### 단어 뜻

- URL - Locator: 리소스가 있는 위치를 지정
- URN - Name: 리소스에 이름을 부여
- 위치는 변할 수 있지만, 이름은 변하지 않는다.
- URN 이름만으로는 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음

### 전체 문법

- scheme://[userinfo@]host[:port][/path][?query][#fragment]
  - **userinfo**
    - URL에 사용자 정보를 포함해서 인증
    - 거의 사용하지 않음
  - **host**
    - 호스트명
    - 도메인명 또는 IP 주소를 직접 사용가능
  - **port**
    - 접속 포트
    - 일반적으로 생략 가능, 생략시 http는 80 https는 443으로 연결
  - **path**
    - 리소스 경로(path), 계층적 구조로 이루어짐
    - 예) /home/file1.jpg
  - **query**
    - key = value 형태
    - ?로 시작, &로 추가 가능 예) ?keyA=valueA&keyB=valueB
    - query parameter, query string 등으로 불리며 웹 서버에 제공하는 파라미터로 문자 형태로 이루어짐
  - **fragment**
    - html 내부 북마크 등에 사용
    - 서버에 전송하는 정보는 아님
- 주로 프로토콜 사용
- 프로토콜: 어떤 방식으로 자원에 접근할 것인가 하는 약속 규칙
  - 예) http, https, ftp 등
- http는 80포트, https는 443 포트를 주로 사용, 포트는 생략이 가능
- https는 http에 보안 추가 (HTTP Secure)

# 웹 브라우저 요청 흐름

![Untitled (2)](https://user-images.githubusercontent.com/58387974/127986287-176bcee9-c9cc-4ab9-817d-e30af58a73b0.png)

1. DNS 서버를 조회하여 IP와 PORT 정보를 찾음
2. IP, PORT 정보를 통해 HTTP 요청 메시지를 생성함

## HTTP 요청 메시지

![Untitled (3)](https://user-images.githubusercontent.com/58387974/127986290-2b5be96d-814a-44e4-9339-f6c4e42d4c81.png)

## HTTP 메시지 전송

![Untitled (4)](https://user-images.githubusercontent.com/58387974/127986295-e0d564e3-9c71-4018-b24f-fc3f5484db5a.png)

TCP/IP 패킷에는 HTTP 요청 메시지가 포함되어 있다.

## HTTP 응답 메시지

![Untitled (5)](https://user-images.githubusercontent.com/58387974/127986301-07cce598-6622-4388-b226-ddbd6abace36.png)

요청한 패킷이 호스트에 도착하면 호스트는 위의 HTTP 응답 메시지를 생성한다.

## 렌더링

웹 브라우저에 HTTP 응답 메시지가 포함된 패킷이 도착하면 렌더링이 진행된다.
