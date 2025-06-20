# A/B 테스트 기반 게임 유저 리텐션 분석
- 구분 : 개인 프로젝트
- 기간 : 2025년 2월 1일 ~ 2월 7일

<br>

## 1. 문제 정의
모바일 게임 **Cookie Cats**는 특정 레벨에 Gate(잠금 시스템)를 적용해 유저의 게임 진행을 조절함  
기존에 **Level 30**에 위치해 있던 Gate를 **Level 40**으로 옮겼을 때, **유저 리텐션**에 어떤 영향을 미쳤는지 분석할 필요가 있음

<br>

## 2. 데이터 수집 및 분석
- **데이터 출처** : [Kaggle - Mobile Games A/B Testing - Cookie Cats](https://www.kaggle.com/datasets/mursideyarkin/mobile-games-ab-testing-cookie-cats)
- **총 데이터 수** : 90,189개

**데이터 전처리**
- 비정상적으로 큰 **이상치 1건 제거**  
- 총 게임 라운드 수가 **40회 미만인 데이터 삭제**
  - Gate를 경험한 유저의 반응을 분석하기 위함  
  - A/B 테스트의 공정성을 유지하기 위해 두 그룹에 동일한 기준 적용

**분석 방법**
- Gate 위치 변경이 **평균 게임 라운드 수**와 **유저 리텐션**에 미치는 영향 분석
- **헤비 유저**와 **라이트 유저**의 리텐션 차이 및 Gate 위치에 대한 영향 분석

<br>

## 3. 가설 설정 및 검증

### 가설 1 : Gate 위치 변경이 유저의 평균 게임 라운드 수에 유의미한 영향을 미칠 것이다
**검증 방법** : 독립표본 t-검정 수행

**검증 결과** :
- **평균 게임 라운드 수** : `Gate 30` (142.05) vs. `Gate 40` (142.67)

  - 두 그룹 간 평균 게임 라운드 수에 유의미한 차이가 없음 ❌ (p-value = 0.7330)

<br>

### 가설 2 : Gate의 위치가 유저의 리텐션(1일차, 7일차)에 유의미한 영향을 미칠 것이다
**검증 방법** : 카이제곱 검정 수행

**검증 결과** :
- **1일차 리텐션** : `Gate 30` (83.77%) vs. `Gate 40` (83.02%)
  - 두 그룹 간 1일차 리텐션에 유의미한 차이가 없음 ❌ (p-value = 0.0994)

- **7일차 리텐션** : `Gate 30` (50.28%) vs. `Gate 40` (48.51%)
  - 두 그룹 간 7일차 리텐션에 유의미한 차이가 있음 ✔️ (p-value = 0.0034)

<br>

### 가설 3 : 유저 세그먼트에 따라 Gate 40이 7일차 리텐션에 미치는 영향이 다를 것이다
**유저 세그먼트 방법** : 게임 라운드 수의 상위 10%는 헤비 유저, 나머지는 라이트 유저로 구분

**검증 방법** : 카이제곱 검정 수행

**검증 결과** :
- **헤비 유저 7일차 리텐션** : `Gate 30` (90.12%) vs. `Gate 40` (91.28%)
  - 헤비 유저의 경우, Gate 40이 리텐션에 유의미한 영향을 미치지 않음 ❌ (p-value = 0.3277)

- **라이트 유저 7일차 리텐션** : `Gate 30` (45.78%) vs. `Gate 40` (43.82%)
  - 라이트 유저의 경우, Gate 40이 리텐션에 유의미한 영향을 미침 ✔️ (p-value = 0.0020)

<br>

> 📌 7일차 리텐션 감소는 **쾌락적 적응(Hedonic Adaptation) 이론**으로 설명할 수 있습니다.
> 
> Gate가 늦게 등장하면 유저가 몰입하기 전에 흥미를 잃을 가능성이 큽니다.
> 반면, **Gate 30**처럼 **초반에 적절한 도전 요소**가 있으면 유저가 플레이에 지루함을 느끼지 않고 흥미를 더 오래 유지할 수 있습니다.

<br>

## 4. 인사이트

- **Gate 위치 변경**은 유저의 게임 라운드 수에는 영향을 미치지 않음

- **Gate 40**은 장기적으로 **유저 리텐션을 떨어뜨리는 원인**이 될 수 있음

- **헤비 유저**는 Gate 위치와 관계없이 **꾸준히 플레이**하는 경향을 보임

- **Gate 40**은 **라이트 유저**가 몰입하기 전에 흥미를 잃게 하여 **이탈하게 만듦**

- 라이트 유저의 리텐션을 고려할 때 **Gate 30이 더 적합해 보임**


<br>

## 5. 결론 및 액션 제안

### 1️⃣ Gate를 Level 30에 유지하고 추가 A/B 테스트 진행  
- **Gate를 Level 20으로 배치하는 실험**
  - 게임 초반부에 Gate를 배치하여 유저 세그먼트별 리텐션에 미치는 영향 평가

- **Gate가 유저 결제 비율에 미치는 영향 분석**
  - Gate를 통과하기 위한 유료 아이템을 배치했을 때 결제 전환율에 미치는 영향 확인


### 2️⃣ 매출 데이터를 활용한 유료 전환율 분석
- **헤비 유저는 Gate 위치와 관계없이 지속적으로 플레이함**

  - 일정 레벨 이후에만 접근 가능한 **프리미엄 콘텐츠**(하드 모드, 경쟁 콘텐츠, 하우징 콘텐츠 등)와 특정 구간에서만 획득할 수 있는 **희귀 아이템**(캐릭터, 펫, 스킨 등)을 배치하여 유저의 결제 욕구 자극 필요
  - 이러한 요소가 결제 전환율, 매출, 리텐션에 미치는 영향 분석

### 3️⃣ 게임 플레이 횟수 증가를 위한 보상 시스템 도입
- **Gate 위치 변경만으로는 게임 플레이 횟수 증가에 효과가 없음**

  - 유저의 지속적인 참여를 유도하기 위한 **보상 시스템**(일일 접속 보상, 연속 출석 보상, 특정 미션시 추가 보상 등) 도입
  - 게임 플레이 경험 점검 후 특정 레벨에서 이탈이 많다면 난이도를 조정해 이탈률을 낮춰야 함

<br>

## 6. 회고


| **✔️ Keep** | **❌ Problem** | **🔍 Try** |
|---------|-----------|--------|
| 데이터 전처리로 이상치를 제거하고 공정한 비교 기준을 적용한 점 | 유저 특성이 부족하여 세그먼트를 단순히 라운드 수 기준으로 구분한 점 | 유저 행동 데이터를 추가 분석하여 세그먼트 기준을 고도화 |
| 통계적 검정을 활용하여 유의미한 차이를 명확히 검증한 점 | 매출 데이터가 없어 유료 전환율을 분석하지 못한 점 | 유료 전환율과 Gate 위치의 관계를 추가적으로 분석 |
| 실험 결과를 바탕으로 실행 가능한 액션 플랜을 도출한 점 | 제한된 정보로 인해 전처리 기준 설정에 임의적인 판단이 개입된 점 | A/B 테스트 설계 과정에 직접 참여하여 실험 설계 역량 강화 |
