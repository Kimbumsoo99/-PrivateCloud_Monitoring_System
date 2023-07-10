# 빅데이터를 활용한 Private Cloud 및 모니터링 시스템 구축

## 1. 프로젝트 배경

![](https://velog.velcdn.com/images/show7441/post/c3218b54-d685-4549-9e5c-6f6370bc4c48/image.png)

Private Cloud 환경을 구축하고 Monitoring을 할 수 있도록 하는 Project 입니다.
Monitoring Server에서는 Private Cloud에 존재하는 Resource를 확인할 수 있으며, 해당 자원을 사용자에게 보여주는 동작등을 할 수 있습니다.

## 2. 프로젝트 설명

### 모니터링 웹 서버 동작 화면

![](https://user-images.githubusercontent.com/121588874/244625515-fd98078a-9753-49f8-9b21-049a8ac0fed7.gif)

### 가상화 환경

Private Cloud와 같은 환경을 구성하기 위해 VMWare SoftWare를 사용했습니다.
단일한 물리 HardWare System을 ESXi Hypervisor를 통해 Virtual Machine으로 분할하여 환경을 구성했습니다.

![](https://velog.velcdn.com/images/show7441/post/0cdd9031-b545-4807-85f6-8fe63a1fd3f0/image.png) 


### 전체 서비스 흐름도
![image](https://github.com/sangmin0806/-PrivateCloud_Monitoring_System/assets/134148399/9275c6b6-141a-4649-bb07-2e0a5cdeeaba)

Private Cloud환경에서 다양한 가상화 기술들을 통해 가용성을 높이고 이를 웹 서버를 통해 Monitoring합니다.

### vCenter REST API & vRealize Operations REST API 활용

![](https://velog.velcdn.com/images/show7441/post/0c0cfe16-bbbc-45c7-9b56-c19e20b3fb74/image.png)



vSphere 가상화 플랫폼을 활용하여, ESXi HyperVisor를 통해 서버가상화를 수행하였습니다.
Monitoring을 위해 vCenter API와 vRealize API를 사용하여 데이터를 통신하였습니다.
- 가상머신의 기본 설정정보등 정적정보는 vCenter API를 통해 가져왔고,
- Realtime Information은 vRealize Operations Solution을 사용한, vRealize API를 통해 가져왔습니다.


## 3. 프로젝트 기능

### Private Cloud 주요기술
1. 공유스토리지
    - 여러 가상 머신이 동시에 액세스할 수 있는 공통 스토리지 리소스로, 가상 머신은 서로 독립적으로 실행되지만 동일한 데이터에 액세스할 수 있다.
  또한 vMotion기능을 수행하기 위한 필수요소이다.
  
2. vMotion
    - VMware의 가상 머신 이동 기술로, 가상 머신의 작동 중지 없이 호스트 간에 가상 머신을 이동할 수 있게 하는 기술이다.
    - HA, DRS기능을 수행하기 위한 필수 기능이며 중요한 역할을 수행한다.

3. HA(High Availability)
    - 클러스터 내에서 호스트 장애 및 유지보수 시, 동작 중인 가상 머신을 다른 호스트로 이동시켜 서비스를 제공하는 가상화 기술

![image](https://github.com/sangmin0806/-PrivateCloud_Monitoring_System/assets/134148399/14f3f7eb-8f35-42e3-bd47-b3959f353ab9)

4. DRS(Distributed Resource Scheduler)
    - 클러스터 내에서 호스트간 가상 머신의 이동을 통해 자원을 분산시켜 관리 효율을 높이는 가상화 기술

![image](https://github.com/sangmin0806/-PrivateCloud_Monitoring_System/assets/134148399/4a7c6deb-53e4-4052-9e13-e18f678b0493)

### 모니터링 웹 서버 기능

1. Hypervisor Type 2 Host Environment

    - vCenter에 등록된 Host 구성 정보
    - Host에 Power State와 Name 확인

2. ESXi Host 위에 존재하는 VM List 확인

    - VM Power State, Name 확인
    - VM Create
    - VM Power ON/OFF

3. VM Create

    - Name, Guest OS, Memory, CPU Core 등 정보 입력을 통해 VM 생성

4. VM Static Information

    - Host, CPU Core, Memory, Disk File, Free Disk Resources, Network Name, Network State, GuestOS 등 Table 형태로 정보 확인
    - VM Edit(CPU Core, Memory)
    - VM Delete

5. VM Realtime CPU/Memory Usage

    - Hourly utilization rate
    - Average Usage

6. Send Mail
    - 송신: Server `.env` 등록된 E-Mail
    - 수신: Monitoring Web Server 가입 시 등록한 E-Mail
    - 사용 예시(CPU 사용량에 따른 부하/위험 정보)
