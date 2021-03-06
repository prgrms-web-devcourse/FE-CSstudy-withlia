# 서버리스란?

> 서버를 관리할 필요가 없다.

서버리스하면 서버가 아예 없다는 뜻 같지만, 그런 의미가 아니라 서버를 프로비저닝, 관리 또는 비용을 지불할 필요가 없다는 뜻입니다.

<br>
<br>

# 등장 배경

> 서버 개발, 관리에 대한 부담감

## 1. 자체 설계 운영

서버리스란 개념이 등장하기 전 사용자는 직접 서버를 개발하고 운영해야 했습니다. 전산실을 떠올리면 이해가 쉬울 텐데 서버의 하드웨어, 소프트웨어 모두 관리해야 했기 때문에 비용이 많이 나갔습니다. 또 개인 상황(메모리 부족, 정전 등)에 따라 서버가 다운될 수 있어서 안정적이 못 했습니다.

## 2. IaaS (Infrastructure as a Service)

그래서 AWS나 Azure같은 클라우드 컴퓨팅 서비스가 등장했습니다. IaaS 방식을 통해 서비스 공급업체는 사용자 대신 서버를 관리를 합니다. 사용자는 더이상 서버자원, 네트워크, 전력등을 구축할 필요가 없어졌고 전문적인 관리를 받아 안정적인 서버를 유지할 수 있게 됐습니다. 하지만 여전히 서버를 개발해야 한다는 부담감이 있었습니다.

서버리스는 이런 부담감을 없애주기 위해 만들어졌습니다.

<br>
<br>

# 서버리스 종류

서버리스를 사용하면 사용자는 서버를 개발하지 않고 공급업체가 제공하는 서버에 접근해 데이터를 처리하는 데 처리하는 방식과, 비용 산정에 따라 2가지로 나눌 수 있습니다.

## 1. FaaS(Function as a Service)

> 함수로 처리, 함수 사용시 비용 발생

프로젝트를 여러개의 함수로 쪼개서( 또는 한개의 함수로 만들어서 ), Rest API같은 HTTP요청을 통해 함수를 호출하고, 원하는 파라미터를 전달해 반환값을 받거나 데이터를 조작하는 방식입니다. 비용은 함수들이 실행되는 횟수와 시간으로 산정됩니다.

- 대표적인 예: AWS Lambda, MS Azure Function

> 실행 순서

![image](https://user-images.githubusercontent.com/79133602/170531321-9b7100b9-fbb7-44a2-8cff-3575e9a440bc.png)

1.  프로그램 실행 후 FaaS 함수를 호출
2.  FaaS가 함수 Repo에서 해당함수를 읽은 뒤 컨테이너 또는 가상머신으로 만듬
3.  가상머신 생성 후 함수를 실행
4.  일정 기간 동안 함수 호출이 없을 시 FaaS 시스템이 컨테이너 또는 가상머신을 삭제

여기서 보면 일정 기간 동안 함수 호출이 없을 때 컨테이너 또는 가상머신이 삭제되는데 이 덕분에 FaaS 방식은 이전 서버가 24시간 가동되고 요청이 없어도 비용이 나갔던 것과 달리 함수가 호출되는 일정 기간만 돈을 내면 됩니다.

## 2. BaaS(Backend as a Service)

> API로 처리, API 사용시 비용 발생

서비스 공급업체가 앱개발에 필요한 기능들(데이터 베이스, 소셜서비스 연동, 파일 시스템등) 을 API로 제공해주는 방식입니다. 비용은 API를 사용한 만큼만 지불하면 됩니다. 클라우드 공급자가 백엔드 서버를 구축해주기 때문에 개발자는 프론트엔드 코드만 작성하면 되서 개발 시간이 단축됩니다. 게다가 서버 이용자가 갑자기 증가해도 알아서 확장이 되서 안정적입니다.

- 대표적인 예: 구글의 Firebase

![image](https://user-images.githubusercontent.com/79133602/170528002-024a17d2-ceea-4d38-88a5-17d0a30fcf1f.png)

웹을 파이어 베이스에 연동해두면 이제 위 메뉴들에 정보를 저장하고 가져올 수 있습니다.

<br>
<br>

## 두 서비스의 차이점

|                  | FaaS                                              | BaaS                                                     |
| :--------------: | :------------------------------------------------ | :------------------------------------------------------- |
|       정의       | 개발자가 서버 로직 작성, 클라우드 제공업체가 관리 | 클라우드 제공업체가 서버 구현 및 관리                    |
| 개발자가 하는 일 | 서버에서 수행될 기능들을 직접 코딩                | 서버는 클라우드 공급자에게 맡기고 프론트엔드 부분을 코딩 |
|    비용 산정     | 함수를 호출한 횟수와 시간에 따라                  | API 사용량, 서버 사용시간에 따라                         |

<br>
<br>

# 서버리스 장점

## 1. 비용 절감

- 기존 서버가 24시간 가동되고 요청수와 상관없이 비용이 청구됐던 것과 달리 서버리스는 사용한 만큼만 비용이 청구됩니다.

- 일정 기간, 조건에 따라 함수를 호출하기 때문에 리소스 낭비가 없습니다.

- 서버 관리 및 개발에 대한 비용을 절감할 수 있습니다.

## 2. 확장성이 좋다

- 서버 이용자가 갑자기 증가해도 자동으로 확장이 되고, 개별 서버단위가 아니라 메모리, 처리량 등의 사용단위로 용량을 조절할 수 있습니다.

- 따라서 단기간 이벤트성 트래픽을 처리하는 데 효과적입니다.

- 또한 요청이 들어올 때만 실행되기 때문에 자원을 동적으로 할당하기가 좋습니다.

## 3. 프론트엔드 개발에 집중 가능

- 백엔드 개발에 대한 부담을 덜 수 있어서 소규모 프로젝트 시 인력을 프론트엔드에 집중할 수 있습니다.

## 4. 빠른 배포

- 패키징, 배포가 간단하고 자동 배포를 제공하는 경우 유지보수에 좋습니다.

<br>
<br>

# 서버리스 활용 예시

## 1. 자동화 작업

- 넷플릭스: 동영상 업로드 시 파일의 인코딩, 검증, 태깅 이후 공개되는 작업을 AWS Lambda로 자동화

- 페리스코프: 동영상 유해성 여부 확인 작업을 AWS Lambda로 자동화

## 2. 챗봇 서비스

- 슬랙: 이용자가 갑자기 많아져도 유동적으로 자원을 할당하기 때문에 챗봇에 사용

## 3. 분석, 모니터링

- 버슬: 하루 1억건의 이벤트처리와 데이터 분석 리포팅에 서버리스를 적용해 84%의 비용을 절감

## 4. 배치 작업

데이터를 모아서 처리하는 배치 작업 특성상 24시간 운영 서버가 필요없기 때문에 서버리스로 구현하면 효율적

<br>
<br>

# 서버리스 단점

## 1. Cold Start

요청이 없는 경우 함수들을 수면상태로 만들기 때문에 새로운 요청이 들어왔을 때 반응 시간이 상대적으로 느립니다. 만약 즉각적인 반응이 중요한 실시간 서비스라면 이는 큰 단점입니다. (AWS lambda는 특정 함수를 수면 상태에 들어가지 않도록 설정해서 이부분을 보완하고 있습니다.)

## 2. Stateless

> 로컬 데이터를 사용할 수 없어!

서버리스는 stateless로 구현되서 함수들은 전후 상태를 공유할 수 없고, 변수와 데이터도 공유가 안되서 데이터를 로컬스토리지에서 읽고 쓸 수 없습니다. (추가 서비스로 극복이 가능하다고는 하지만 대부분의 경우 불가능합니다.- AWS S3, Azure Storage)

## 3. 오래 걸리는 작업은 비효율적

함수를 한 번 호출할 때 사용할 수 있는 메모리와 시간이 한정적이기 때문에 단순작업은 괜찮지만 동영상 업로드나 데이터 백업같은 작업을 할 땐 비효율적입니다. 만약 해당 작업을 서버리스에서 처리한다면 여러번에 나누어 구현해야 합니다.

## 4. 클라우드 플랫폼 이동이 힘들다.

> 종속적

클라우드 플랫폼 마다 애플리케이션 구조, 가격, 정책이 판이하기 때문에 사용자가 중간에 다른 플랫폼으로 옮기고 싶어도 바꾸기가 쉽지 않습니다. 그래서 만약 기존 플랫폼이 가격을 올리거나, 사양이 좋지 못 할 때 큰 단점이 됩니다.

<br><br>
<br>

# 참고

💻 [클라우드 네이티브란](https://cloudmt.co.kr/?p=3927)

💻 [클라우드 컴퓨팅의 종류, IaaS란?](https://library.gabia.com/contents/infrahosting/9097/)

💻 [“데이터 계층을 위한 탄력적 컴퓨팅” 서버리스 데이터베이스의 이해](https://www.itworld.co.kr/news/228914)

💻 [서버리스란](https://velog.io/@dwa_all/%EC%84%9C%EB%B2%84%EB%A6%AC%EC%8A%A4%EB%9E%80Serverless)

💻 [서버리스 개념 정리](<https://inpa.tistory.com/entry/WEB-%F0%9F%8C%90-%EC%84%9C%EB%B2%84%EB%A6%AC%EC%8A%A4ServerLess-%EA%B0%9C%EB%85%90-%F0%9F%92%AF-%EC%B4%9D%EC%A0%95%EB%A6%AC-BaaS-FaaS#BaaS_(Backend_as_a_Service)>)
