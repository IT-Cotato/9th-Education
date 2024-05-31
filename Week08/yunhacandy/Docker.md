# Docker

## 서론

---

`내 PC에서는 잘 되는데, 배포하면 왜 안되지?`

노드 JS 버전이 달라서.. 구동하는 환경이 달라서..

이런 번거로움을 해결하기 위해서`Docker`가 등장했음.

## 교육 목표

---

- 도커의 개념에 대해 이해
- 도커 배포 과정에 대해 이해

## 도커란?

---

도커 -컨테이너 기반의 오픈소스 가상화 플랫폼으로, 응용 프로그램과 그 종속성을 격리된 환경인 컨테이너로 패키징하여 실행하는 기술이다. 

**컨테이너 : 가상화 기술을 이용하여 어플리케이션과 개발 환경을 격리된 공간에서 실행하는 단위*

## 가상머신 vs 도커 컨테이너

---

### VM

- VM은 하드웨어 Infrastructure 위에, VMware나 VirtualBox 같은 Hypervisor software를 이용해서 각각의 가상의 머신을 만들 수 있음.
- 하나의 운영체제 위에서 각각의 고립된 다른 환경에서 구동하기 위해서 VM을 사용함.
- 각각의 고립된 다른 환경들(Virtual Machine)은 운영체제(OS)를 포함하고 있기 때문에, Mac 위에서 리눅스나 윈도우 같은 운영체제를 사용하는 것이 가능함.
- 하지만 고립된 환경마다 운영체제를 포함하고 있기 때문에 굉장히 무거워짐.
    - 구동 시간 오래 걸림, 컴퓨터의 리소스를 많이 사용함, …
- 이런 VM의 컨셉을 가져오면서도 경량화한 모델이 바로 `Container`임.

### Container

- 하드웨어에서 설치된 Host OS 위에, Container Engine이라는 소프트웨어를 설치하면 각각의 어플리케이션을 고립된 환경(Container)에서 실행할 수 있음.
- VM과 다르게 고립된 환경에서 운영체제를 포함하지 않고, Container Engine에 설치된 Host OS를 공유함.
- Container를 구동하기 위해, Container Engine이 필요하고 그 엔진이 Host OS에 접근해서 필요한 것들을 처리하는 방식임.
- 이 Container Engine 중에서 가장 많이 사용하는 것이 `Docker`

## 도커를 사용하는 이유

---

- **환경 일치성**: 다양한 환경에서 동일한 실행 환경을 보장한다. 개발 환경과 운영 환경의 차이로 인한 문제를 방지하며, 응용 프로그램을 어디서든 실행할 수 있다.
- **편리한 배포**: 도커 컨테이너는 이미지로 패키징되어 배포되므로, 어플리케이션 배포가 간단해진다. 이미지를 공유하거나 배포할 때 용이하며, 빠른 확장이 가능하다.
- **격리된 환경**: 도커는 각 컨테이너를 격리된 환경으로 실행하므로, 하나의 컨테이너에서 발생한 문제가 다른 컨테이너에 영향을 주지 않는다.
- **자원 효율성**: 가상 머신과 비교해 더 가볍고 빠르며, 호스트 시스템의 리소스를 효율적으로 활용할 수 있다.
- **스케일링**: 컨테이너 기반 아키텍처는 쉬운 스케일링이 가능하여 요구에 따라 응용 프로그램을 확장할 수 있다.

## 도커의 3대 구성 요소

---

컨테이너를 만들기 위해서는 `dockerfile` 만들기 → build해서 `image` 만들기 → `container` 구동하기

### Dockerfile

- 컨테이너를 어떻게 만들어야하는지 recipe!
- 필요한 파일, dependencies, 환경변수, 실행 스크립트 등 포함
- DSL(Docker Specific Language) 언어를 사용해 이미지를 생성할 수 있음.

### image

- Dockerfile을 build해서 이미지 생성.
- 실행되고 있는 application의 상태를 스냅샷!
- 변경 불가능
- 객체지향의 class같은 개념! 붕어빵 틀~
- [저장소 이름]/[이미지 이름]:[태그] 형식
    - 저장소 이름: 이미지가 저장된 장소. 저장소 이름이 명시되지 않은 이미지는 도커 허브의 공식 이미지를 뜻함
    - 이미지 이름: 해당 이미지가 어떤 역할을 하는지 나타내며 필수로 설정해야 함
    - 태그: 이미지의 버전을 나타냄. 태그를 생략하면 도커 엔진은 latest로 인식함

### Container

- 이미지를 고립된 환경에서 프로세스를 실행할 수 있음
- 이미지를 이용해서 구동(컨테이너 = 이미지를 실행한 상태)
- 컨테이너에서 개별적 수정 가능. 수정해도 이미지에는 영향을 끼치지 않음.
- 도커 이미지가 도넛 레시피라면, 도커 컨테이너는 레시피를 이용해 만든 도넛으로 비유 가능!

## container 공유 방법

---

- git, github같은 개념 (= docker, dockerhub이 있음)
- local에서 dockerfile 생성, 빌드 → image 생성 → Image container registry에 푸시하기 → Server에서 pull해서 실행
- 참고로, container repository에는 public, private가 있다.
    - Public: dockerhub, RED HAT, GitHub Packages
    - Private: AWS, GoogleCloud, Microsoft Azure

## 배포

---

### 도커 이미지 배포하기

- Local 환경에서 Image를 만들어서 Github와 같은 Container Registry라는 곳에 Image를 push 하고, 필요한 서버나 다른 PC에서 Image를 pull 해와서 실행하면 됨.
- Image를 실행하기 위해 Docker와 같은 Container Engine 설치가 필요함.
- dockerhub, Github Packages 같은 공개 저장소도 있지만, 대부분 실무에서는 Private으로 관리해야 하기 때문에 AWS, Google Cloud, Microsoft Azure 같은 서비스를 이용함.

Dockerfile의 Snapshot을 찍어서 Image 파일을 만든다.

Container Registry(AWS, Google Cloud, …)에 Image를 올려둔다.

필요한 곳에서 올려져있는 Image를 가져와서 Container Engine을 통해 실행한다.

### 도커를 사용한 애플리케이션 배포 방법

1. 도커 파일 생성 (이미지 준비)
2. 도커 이미지 빌드
3. 도커 컨테이너 실행
4. 도커 컨테이너 관리
5. 도커 컴포즈를 이용한 배포

### 도커를 사용한 스프링 부트 배포 방법
1.	Spring Boot 프로젝트 내부 Dockerfile 생성 및 설정.
2.	Jar 파일 Build.
3.	Docker Image 생성.
4.	Docker Hub에 Image push.
5.	EC2 Docker Image pull 및 실행.
6.	Server 실행 확인.