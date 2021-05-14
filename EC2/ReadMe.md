# AWS-EC2

## 목차
- EC2란?
- EBS
- ELB
- Route53 + 실습
- EC2 실습

## EC2란?
- Elastic Compute Cloud
    - 클라우드 공간에서 크기가 유연하게 변경되는 기능을 제공함
- EC2 금액 지불 방식
    - On-demand : 시간 단위로 가격이 고정되어 있음
        - 오랜시간동안 선불을 내지 않고 최소한의 비용을 지불하여 EC2인스턴스를 사용하고 싶을때, 특히 앱/프로그램 개발시 최초로 EC2인스턴스에 deploy 할때 매우 유용함
        - 개발 기간이 정확하지 않을때 등
    - Reserved : 한정된 EC2 용량 사용 가능, 1-3년동안 시간별로 할인 적용가능, 크기를 늘이거나 줄일수 없음
        - 안정된, 예상 가능한 workload시 Reserved 사용권장
        - 개발 기간이 정확할때
    - Spot : 입찰 가격 적용. 가장 큰 할인률을 적용받으며 특히 인스턴스의 시작과 끝기간이 전혀 중요하지 않을때 매우 유용함
        - 단순한 비용 절감 시 유용함
        - 입찰가격에 가격이 형성될때 사용가능함

- EC2는 EBS라는 디스크 볼륨을 요구한다.

## EBS
- Elastic Blcok Storage
    - 저장 공간이 생성되어지며 EC2 인스턴스에 부착된다.
    - 디스크 볼륨 위에 File System이 생성된다
    - EBS는 특정 Availability Zone에 생성된다.

- EBS 볼륨 타입
    - SSD 군
        - General Purpose SSD (GP2) : 최대 10K IOPS를 지우너하며 1GB당 3IOPS 속도가 나옴
        - Provisioned IOPS SSD (IO1) : 극도의 I/O률을 요구하는(ex: 매우 큰 DB 관리) 환경에서 주로 사용됨, 10K 이상의 IOPS를 지원함
    - Magnetic/HDD 군
        - Throughput Optimized H1DD(ST1) : 빅데이터 Datawarehouse, Log 프로세싱시 주로 사용(boot voulme으로 사용 불가능)
        - CDD HDD (SC1) : 파일 서버와 같이 드문 volume 접근시 주로 사용(boot voulme으로 사용 불가능)
        - Magnetic (Sandard) : 디스크 1GB당 가장 싼 비용(boot voulme으로 사용 가능)

## ELB
- Elastic Load Balancers
    - 수많은 서버의 흐름을 균형있게 흘려보내는데 중추적인 역할을 함
    - 하나의 서버로 traffic 이 몰리는 병목현상 방지
    - Traffic 의 흐름을 unhealthy instance 를 healthy instance 로 바꿔줌

- ELB 종류
    1. Application Load Balancer : OSI Layer7에서 작동됨
        - HTTP, HTTPS와 같은 traffic의 load balancing에 적합함
        - 고급 라우팅 설정을 통해 request 의 요청을 다른 서버로 보낼 수 있음

    2. Network Load Balancer : OSI Layer4에서 작동됨
        - 극도의 performance 가 요구되는 TCP traffic에서 적합함
    
    3. Classic Load Balancer : 현재는 Legacy
        - 7계층과 4계층의 라우팅을 지원함
- X-Forwarded-For 헤더
    - 152.12.3.225(public IP address) -> ELB / 10.0.0.23(Private IP address) -> EC2 / 10.0.0.23
    - EC2에서는 ELB의 private ip address 만 확인할 수 있지만 X-Forwarded -For header 를 이용해서 어떤 IP에서의 요청인지 확인할 수 있다 

## Route53 + 실습
- AWS에서 제공하는 DNS 서비스
- 도메인 구매가능함

## EC2 실습
1. EC2 생성
2. 키발급
3. 콘솔 접속 :  ssh -i `key.pem` `ec2-user(기본 유저 id)`@`ec2-ip`
4. 서비스 재시작시 동시시작 명령어 : chkconfig httpd onpi