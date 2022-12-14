# HTTP와 HTTPS의 개념 및 차이점

- [HTTP란?](#http란)
- [HTTP의 구조](#http의-구조)
- [HTTPS란?](#https란)
    - [HTTPS(Hyper Text Transfer Protocol Secure)란?](#httpshyper-text-transfer-protocol-secure란)
    - [대칭키 암호화와 비대칭키 암호화](#대칭키-암호화와-비대칭-키-암호화)
    - [HTTPS의 동작과정](#https의-동작-과정)
    - [HTTPS 발급과정](#https-발급과정)

---

## HTTP란?
- HTTP(Hyper Text Transper Protocol)이란 **서버/클라이언트 모델을 따라 데이터를 주고받기 위한 프로토콜**이다.
- 인터넷에서 하이퍼 텍스트를 교환하기 위한 통신 규약으로, **80번 포트**를 사용하고 있다.

## HTTP의 구조
<img width="60%" src="https://user-images.githubusercontent.com/76734067/210742548-9c3202e0-35e9-4546-a5ba-1202d8a49438.png">

- HTTP는 애플리케이션 레벨의 프로토콜로 TCP/IP위에서 작동한다.
- 상태를 가지고 있지 않은 Stateless 프로토콜이다.
- Method, Path, Version, Headers, Body등으로 구성된다.

## HTTPS란?
### HTTPS(Hyper Text Transfer Protocol Secure)란?
- HTTP over TLS, HTTP over SSL, HTTP over Secure등으로 불리는 HTTPS는 **HTTP에 데이터 암호화가 추가된 프로토콜**이다.
- HTTP와는 다르게 **443번 포트**를 사용한다.
- 네트워크에서 제3자가 데이터를 조회할 수 없도록 암호화를 지원하고 있다.
-  HTTP는 암호화가 되지 않은 평문 데이터를 전송하는 프로토콜이기 때문에, 비밀번호나 주민등록번호같은 보안정보를 주고받으면 제 3자가 정보를 조회할 수 있었다. 그리고 이러한 문제를 해결하기 위해 HTTPS가 등장하였다.

### 대칭키 암호화와 비대칭 키 암호화
- HTTPS는 대칭키 암호화 방식과 비대칭 키 암호화 방식을 모두 사용하고 있다.
    - 대칭키 암호화
        - 클라이언트와 서버가 동일한 키를 사용해 암호화(encoding)/복호화(decoding)을 진행함
        - **키가 노출되면 매우 위험**하지만 **연산속도가 빠르다**
    - 비대칭키 암호화
        - 1개의 쌍으로 구성된 `공개키`와 `개인키`를 암호화(encoding)/복호화(decoding)하는데 사용한다.
        - **키가 노출되어도 비교적 안전**하지만 **연산속도가 느리다.**
- 비대칭 키 암호화
    - 비대칭키 암호화는 공개키/개인키 암호화 방식을 이용해 데이터를 암호화 하고 있다. 공개키와 개인키는 서로를 위한 1쌍의 키이다.
    - 공개키: 모두에게 공개 가능한 키
    - 개인키: 나만 가지고 있어야하는 키
    - 공개키 암호화
        - 개인키로만 복호화할 수 있다.
        - 개인키는 나만 가지고 있으므로, 나만 볼 수 있다.
    - 개인키 암호화
        - 개인키로 암호화하면 공개키로만 복호화할 수 있다.
        - 공개키는 모두에게 공개되어 있으므로, 내가 인증한 정보임을 알려 신뢰성을 보장할 수 있다.
    <img width="60%" src="https://user-images.githubusercontent.com/76734067/210759062-ff8007e3-a1f9-4da8-8f0d-9634cc4e6c2c.png">

### HTTPS의 동작 과정
- HTTPS는 대칭키 암호화와 비대칭키 암호화를 모두 사용하여 빠른 연산속도와 안정성을 모두 얻고 있다.
- HTTPS연결과정(3-way-handshake)에서 먼저 서버와 클라이언트 간에 세션키를 교환한다. 여기서 세션키는 주고받는 데이터를 암호화하기 위해 사용되는 대칭키이다. 
- 데이터 간의 교환에는 빠른 연산속도가 필요하므로 세션키는 대칭키로 만들어진다.
- 세션키를 클라이언트와 서버가 교환하는데에 비대칭키가 사용된다.
- 처음 연결을 성립하여 안전하게 세션키를 공유하는 과정에서 비대칭키가 사용되는 것이고, 데이터를 교환하는 과정에서는 빠른 연산을 위해 대칭키가 사용되는 것이다.

실제 HTTPS연결과정이 성립되는 흐름을 살펴보면 다음과 같다.

<img width="45%" src="https://user-images.githubusercontent.com/76734067/210759819-5e776147-c5d9-4532-8d4f-f797d63296b2.png">

1. 클라이언트(브라우저)가 서버로 최초 연결 시도를 한다.
2. 서버는 공개키(인증서)를 브라우저에 넘겨준다.
3. 브라우저는 인증서의 유효성을 검사하고 세션키를 발급한다.
4. 브라우저는 세션키를 보관하며 서버의 공개키로 세션키를 암호화하여 서버로 전송한다.
5. 서버는 개인키로 암호화된 세션키를 복호화하여 세션키를 얻는다.
6. 클라이언트와 서버는 동일한 세션키를 공유하므로 데이터를 전달할 때 세션키로 암호화 복호화를 진행한다.

### HTTPS 발급과정
추가로 살펴보아야할 부분이 **서버가 어떻게 비대칭 키를 발급받지?** 하는 부분이다.서버는 클라이언트와 세션키를 공유하기 위한 공개키를 생성해야하는데, 일반적으로는 인증된 기관(CA, Certificate Authority)에 공개키를 전송하여 인증서를 발급받는다.<br>
애플리케이션 서버를 만드는 기업을 `A`라고 하자.
1. A기업이 HTTPS를 적용하기 위해서 공개키와 개인키를 만든다.
2. 신뢰할 수 있는 CA기업을 선택하고 공개키를 저장하는 인증서의 발급을 요청한다.
3. CA기업은 CA기업의 이름, 서버의 공개키, 서버의 정보등을 기반으로 인증서를 생성하고, CA기업의 개인키로 암호화하여 A기업에게 이를 제공한다.
4. A기업은 클라이언트에게 암호화된 인증서를 제공한다.
5. 브라우저는 CA기업의 공개키를 미리 다운받아 가지고 있으며, 암호화된 인증서를 복호화한다. 세계적으로 신뢰할 수 있는 CA기업의 공개키는 브라우저가 이미 알고있다.
6. 암호화된 인증서를 복호화하여 얻은 A기업의 공개키로 세션키를 공유한다.

하지만 HTTPS를 지원한다고 해서 무조건 안전한 것은 아닙니다. 왜냐하면 신뢰할 수 있는 CA기업이 아니라 자체적으로 인증서를 발급할 수도 있고, 신뢰할 수 없는 CA기업을 통해서 인증서를 발급받을 수도 있기 때문입니다. 그때는 브라우저에서는 HTTPS이지만 `주의 요함`, `안전하지 않은 사이트`등의 알림을 주게 됩니다.

### HTTPS의 장점
- 위에서도 말했듯이 **보안상의 우위에 있다.**
- 검색엔진 최적화에 큰 혜택을 볼 수 있다.
    - 구글이 HTTPS 웹 사이트에 가산점을 준다.
    - 사용자들이 안전하다고 생각되는 사이트를 더 많이 방문하게 되기 때문이다.

# Reference
 - [티스토리 - HTTP와 HTTPS](https://mangkyu.tistory.com/98)
 - [티스토리 - Http와 Https 이해와 차이점 그리고 오해(?)](https://jeong-pro.tistory.com/89)
 - [How Does HTTPS Work? RSA Encryption Explained](https://tiptopsecurity.com/how-does-https-work-rsa-encryption-explained/)