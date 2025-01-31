# aws_Guid
aws 기본적인 사용법 가이드

## 구성할 네트워크 구조
> ![struct](https://github.com/hanmin0512/aws_Guid/assets/37041208/7af7a497-a0f8-4a4d-9d3c-65eec88068c6)


## VPC Settings
- VPC 생성해준다.
- VPC ipv4 CIDR은 Private IP이므로 많이 사용해두 되지만 조건이 /16 ~/28 사이이다.
> ![VPC_set1](https://github.com/hanmin0512/aws_Guid/assets/37041208/e8497565-ee41-4d63-b9c4-f349136261af)


- 서브넷을 생성해준다 [Public 2개, Private 2개]
- Public Subnet : 10.0.1.0/24[가용용역 us-west-2a], 10.0.2.0/24[가용용역 us-west-2c]
- Private Subnet : 10.0.3.0/24[가용용역 us-west-2a], 10.0.4.0/24[가용용역 us-west-2c]
> ![Subnet_VPC_ID](https://github.com/hanmin0512/aws_Guid/assets/37041208/9d2294c0-8496-4634-963a-0174edeea7ac)
> ![MyPubSubnet1_set](https://github.com/hanmin0512/aws_Guid/assets/37041208/4ec5ed55-b55d-4ace-9ed5-713c60fae9f2)
> ![MyPriSubnet1_set](https://github.com/hanmin0512/aws_Guid/assets/37041208/28e4bdf4-66cf-4e5f-9ed2-2c785170b17d)
> ![MyPubSubnet2_set](https://github.com/hanmin0512/aws_Guid/assets/37041208/5a540985-2e33-4ed5-b2f7-4d279c845702)
> ![MyPriSubnet2_set](https://github.com/hanmin0512/aws_Guid/assets/37041208/08c1b42c-2e1c-433f-abd4-c3fad12fb9d9)
> ![Sub_List](https://github.com/hanmin0512/aws_Guid/assets/37041208/bcc0a9f5-a4e1-4eb8-825e-64b6ab43dedd)
- Private IP같은경우 외부와 통신을 위해 NAT Gateway를 설정(생성)해줘야 한다.
- Public IP의경우 라우팅 테이블에 명시되지 않는 IP와 통신을 위해 인터넷 게이트웨이를 설정(생성)해줘야한다.

## Internet GateWay Settings
- 라우팅 테이블에 없는 IP와 통신 할 때 즉 외부와 통신을 할 수 있게 AWS의 인터넷게이트웨이를 생성해준다.
- VPC는 위에서 생성한 git_upload_vpc를 선택 한다.
> ![IGW_Set](https://github.com/hanmin0512/aws_Guid/assets/37041208/3de8ec57-a79f-411e-bed7-2ef680c55c2f)
> ![IGW_VPC_Con](https://github.com/hanmin0512/aws_Guid/assets/37041208/59413d1d-50ea-4a37-bb9e-682b96d8ddc7)

## NAT Gateway Settings
- NAT게이트웨이는 Private IP즉 내부망에서 통신하다가 외부망과 통신할 때 내부망의 Private IP를 Public IP로 바꿔서 통신을 하게 해주는 게이트웨이이다.
- NAT 게이트웨이를 생성할 때, 서브넷은 Public 서브넷중 하나로 선택을 해준다.
- NAT유형도 외부와 통신을 해야하기 때문에 Public을 선택해준다.
- EIP(탄력적 IP)는 필수 이다.
> ![createNAT](https://github.com/hanmin0512/aws_Guid/assets/37041208/95ec5f46-2dc3-47f6-b62c-052f086e72b3)

## Routing Table Settings
- 10.0.0.0 ~ 10.0.255.255 사이의 통신은 생성된 라우팅 테이블을 참조하여 통신 한다.
- 라우팅 테이블의 네트워크 환경 VPC는 git_upload_vpc를 선택한다.
> ![RT1](https://github.com/hanmin0512/aws_Guid/assets/37041208/8610ac6b-a11e-4649-8876-50f3810792c1)
> ![RT2](https://github.com/hanmin0512/aws_Guid/assets/37041208/e32a9d57-3b2e-4221-b024-a3d33c799a4a)

- Public IP 통신을 위한 라우팅 테이블 생성
- 해당 라우팅 테이블에 명시되어 있지 않아 외부 인터넷을 통해 통신을 위한 인터넷 게이트웨이 설정.
> ![RT_Add_IGW](https://github.com/hanmin0512/aws_Guid/assets/37041208/17bc417d-5cc5-4d29-9df8-de91946aa5e6)
- [라우팅 테이블]-> [라우팅 테이블 선택]-> [라우팅]-> [라우팅 편집]
- 퍼블릭 서브넷 두개를 추가해 주고, [연결 저장]을 해준다.
> ![RT_Pub_IGW_Set](https://github.com/hanmin0512/aws_Guid/assets/37041208/3816fd15-a6a2-46c1-ab81-8e8d5af5917b)

- Private IP 통신을 위한 라우팅 테이블 생성
> ![PRIRT](https://github.com/hanmin0512/aws_Guid/assets/37041208/11832d0d-ffcd-4102-91d8-92573fd25d73)
> ![PRIRT2](https://github.com/hanmin0512/aws_Guid/assets/37041208/edc7eede-dc49-4e57-9442-484691506240)

- [라우팅 테이블]-> [라우팅 테이블 선택]-> [라우팅]-> [라우팅 편집]
- Private IP 두개를 추가해주고, [연결 저장]을 해준다.
> ![RT_Pri_NATGW_Set](https://github.com/hanmin0512/aws_Guid/assets/37041208/171d32b6-d681-4c49-9b5c-2a56f960da2a)
> ![RT_Add_Sub](https://github.com/hanmin0512/aws_Guid/assets/37041208/37e36ef0-ea30-4804-985b-b16531de7e8d)
> ![RT2_success](https://github.com/hanmin0512/aws_Guid/assets/37041208/a80eb113-1e01-4c77-80bf-32b8c4fed848)

## 리소스 맵으로 Setting한 VPC확인하기
> ![rss1](https://github.com/hanmin0512/aws_Guid/assets/37041208/87680c79-4bc4-44bc-83dc-31f319909e6a)
> ![rss2](https://github.com/hanmin0512/aws_Guid/assets/37041208/8b78c951-cc82-40cf-8d3e-79f6bfdef93f)

