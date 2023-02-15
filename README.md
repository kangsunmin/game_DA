# 다음 분기에 어떤 게임을 개발해야할까?
### (코드스테이츠 데이터 분석 입문 섹션 개인 프로젝트)
- 진행 기간: 4월 18일 ~ 4월 21일
- 사용 언어: `Python`


---

<br/>

## **프로젝트 개요**

**다음 분기에 어떤 게임을 개발해야할까?**

> 👉🏻  과거 게임 출고량 데이터를 분석하여, **다음 분기에 어떤 게임을 개발해야 할 것인지**를 발표하는 프로젝트.
> 
> 
> `#데이터분석`, `#EDA`, `#데이터시각화`
> 
> - 코드스테이츠의 첫 섹션 개인 프로젝트로, **프로젝트 주제 및 데이터셋은 제시되었습니다.**
> - 데이터셋은 1980년~2017년까지 출시된 **비디오 게임**의 출시년도/장르/플랫폼/지역별 출고량 등의 정보를 담고 있습니다.
> - 프로젝트 간 기본적인 데이터 전처리 및 Python의 `matplotlib`, `seaborn` 라이브러리를 이용한 다양한 데이터 시각화를 진행하였습니다.
> - 프로젝트는 아래 챕터로 나누어 진행하였습니다.
>     1. **마켓 트렌드 분석**
>         - 시장의 크기는 어떻게 변하고 있나?
>         - 시간에 따라 게임 트렌드는 어떻게 변해왔나?
>         - 지역별로 선호되는 장르가 다른가?
>         - 잘 팔리는 게임에는 어떤 특징이 있을까?
>     2. ****Go-to-Market****
>         - 1번에서 얻은 인사이트를 바탕으로, 1)어느 지역을 타겟으로, 2)어떤 장르의 게임을 개발하는 것이 좋을지 **가설 검정(t-test)**을 통한 결론 도출.
> - **북미 지역을 타겟으로 콜오브듀티 같은 슈팅 게임을 만들자**는 결론을 도출하였습니다.
> 
> ---
> 

<br/>

## 분석 주제 및 **데이터셋**

*(분석 주제 및 데이터셋은 코드스테이츠로부터 주어진 것입니다.)*

**[분석 주제]**

- **다음 분기에 어떤 게임을 개발해야 할까?**

**[데이터셋]**

- 1980년~2017년까지 출시된 비디오 게임의 출시년도/장르/플랫폼/지역별 출고량 등의 정보를 담고 있습니다.
- 원본 데이터의 형태는 아래와 같습니다.
  <img width="940" alt="dataset" src="https://user-images.githubusercontent.com/81467922/184542216-ca584f30-8b20-4714-b827-c6e938ff2603.png">
  
    > Name : 게임의 이름.
    Platform : 게임이 지원되는 플랫폼의 이름,
    Year : 게임이 출시된 연도,
    Genre : 게임의 장르,
    Publisher : 게임을 제작한 회사,
    NA_Sales : 북미지역에서의 출고량,
    EU_Sales : 유럽지역에서의 출고량,
    JP_Sales : 일본지역에서의 출고량,
    Other_Sales : 기타지역에서의 출고량.
    >
    
  <br/>
  
## 데이터 전처리

- `df.info()` 확인 후, `출시년도`와 `지역별 출고량`의 결측치 처리 및 데이터 타입을 변경해주는 등 기본적인 처리를 해주었습니다.

**[문제 상황]**

- `지역별 출고량`의 경우 단위에 대한 명확한 정보가 주어져 있지 않았습니다. 또한 데이터 탐색 결과 지역별로 일부 표기 방법 통일되어 있지 않은 부분도 발견되었습니다.
    - 예시) `0.05` / `20K` / `1M` 등이 병기되어 있음.

**[해결 방법]**

- 단위에 대한 정보는 일부 게임의 실제 출고량 정보를 구글링하여 체크하고 다른 게임 대해서도 더블체크하여 확인할 수 있었습니다.
- 표기가 통일되어 있지 않은 데이터에 대해서는 아래와 같은 패턴을 확인해 최종적으로 apply 함수를 통해 원하는 형태로 가공하였습니다.

**[전처리 후 데이터셋]**
- 각 지역별 판매량을 합산한 `Total_sales` 칼럼을 추가하였습니다.

<br/>

## 마켓 트렌드 분석

**다음 분기 개발해야할 게임에 대한 인사이트를 얻기 위해, 먼저 마켓 트렌드 분석을 진행하였습니다.**

- 마켓 트렌드 분석은 아래 네 가지에 대해 진행하였습니다.
    - 시장의 크기는 어떻게 변하고 있나?
    - 시간에 따라 게임 트렌드는 어떻게 변해왔나?
    - 지역별로 선호되는 장르가 다른가?
    - 잘 팔리는 게임에는 어떤 특징이 있을까?

**[상세 내용]**

- **시장의 크기는 어떻게 변하고 있나?**
    - 연도별 `게임 출시 수`와 `게임 판매량` 두 가지 기준으로 `barplot`을 그려 살펴봤습니다.
        - `게임 판매량`의 경우, 지역별로는 어떻게 차이가 나는지 추가로 확인하였습니다.
- **시간에 따라 게임 트렌드는 어떻게 변해왔나?**
    - 트렌드 파악을 위해, 수요와 공급의 측면에서 **장르별** `게임 출시 수`와 `게임 판매량`의 변화를 `lineplot`을 그려 살펴봤습니다.
- **지역별로 선호되는 장르가 다른가?**
    - 지역별로 어떤 게임을 선호하는지 확인하기 위해, 장르별 `게임 판매량`을 `barplot`으로 살펴봤습니다.
        - 합계 판매량과 평균 판매량 둘 다 확인하였습니다.
    - 위 내용에 대해 2000년 이후의 데이터로 범위를 좁혀 다시 한 번 확인하였습니다.
        - 분석의 목표를 생각했을 때 비교적 최근의 변화를 확인하는 것이 더 중요하므로.
- **잘 팔리는 게임에는 어떤 특징이 있을까?**
    - 해당 주제는 앞서 말씀드린 이유로 및 모바일 게임 시장의 성장 이후의 트렌드를 파악하기 위해 2000년(일부 2010년) 이후의 데이터에 대해서 분석을 진행했습니다.
    - 판매량 Top10의 `Publisher`에 대해, `barplot`, `lineplot`을 그려 변화를 체크하였습니다.
    - 지역별 판매량 Top10 게임의 장르가 지역별로 어떻게 다른지 `barplot`을 그려 확인하였습니다.

<br/>

## ****Go-to-Market****

- 어느 지역을 타겟으로, 어떤 장르의 게임을 개발하는 것이 좋을지 가설 검정(t-test)을 통해 결론을 도출하는 파트입니다.
- `가장 잘 팔릴 게임`을 만드는 것을 목표로 잡았으며, 앞선 마켓 트렌드 분석에서 얻은 인사이트를 바탕으로 가설을 설정하였습니다.

**[어느 지역을 타겟으로 할 것인가?]**
- 가설: 북미 지역의 판매량 평균은 다른 지역에 비해 높을 것이다.
<img width="989" alt="a" src="https://user-images.githubusercontent.com/81467922/184542386-2649e3b2-45a4-4664-96ac-9aaf0dba4c1b.png">

    - 결론: 북미 지역의 판매량 평균이 다른 지역보다 크다.

**[어느 장르를 만들 것인가?]**
- 가설: 북미 지역에서는 최근(2009년 이후) 가장 선호도가 높은 Shooter 장르가 적합할 것이다.
<img width="270" alt="b" src="https://user-images.githubusercontent.com/81467922/184542409-b28cac39-6982-49fe-b057-5c0fc3439f4f.png">

    - 결론: Platform 장르를 제외하고 모두 Shooter 장르의 판매량이 높다.
- 2013년 이후로 기간을 더 좁혀 Platform과 Shooter 장르에 대해 추가적인 검정을 실시하였습니다.
    - `p-value` = 0.004
    - 결론: Shooter 장르로 하되, 개발 리소스가 부족하다면 Platform 장르를 2순위로 두어도 되겠다.
- 부가적으로 Shooter 장르의 판매 상위 게임 리스트를 확인하여, 어떤 게임을 레퍼런스할 수 있을지도 확인해보았습니다.


위 과정을 통해 **북미 지역을 타겟으로 콜오브듀티 같은 슈팅 게임을 만들자**는 최종 결론을 도출하였습니다.

<br/>

## 아쉬웠던 점

- 처음 해본 프로젝트이고 진행 기간도 촉박하여 아쉬운 점이 많이 남는 프로젝트였습니다.
- 특히 `barplot`, `lineplot` 외에도 좀 더 다양한 시각화를 시도해보지 못한 것이 아쉬움으로 남습니다.

<br/>



 
  
