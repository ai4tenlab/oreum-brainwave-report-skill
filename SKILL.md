---
name: oreum-brainwave-report
description: "교육채널 오름 백주경 박사 스타일의 BQ2 뇌기능 검사지 → 뇌기능 분석결과 보고서 자동 생성 스킬."
version: 1.0.0
author: 4TENLAB + Hermes
metadata:
  hermes:
    tags: [brainwave, EEG, BQ2, brain-trainer, education-counseling, report-generation, oreum]
---

# Oreum Brainwave Report

## Purpose

이 스킬은 국가공인 브레인트레이너인 교육채널 오름 백주경 박사가 `BQ2 뇌기능 검사지`를 Hermes에 제공하면, 그 원자료의 정량 지표와 백주경 박사의 정성적 교육·상담 해석을 결합하여 `뇌기능 분석결과 보고서`를 생성하기 위한 표준 작업 절차다.

핵심 컨셉은 다음과 같다.

> BQ2 뇌파 검사 수치를 기반으로 대상자의 두뇌 기능 상태를 정량적으로 분석하고, 뇌과학·뇌파분석·교육상담 관점의 정성적 해석을 결합해 대상자의 현재 상태, 회복 필요성, 상담 방향을 과학적으로 설명한다.

Hermes는 보고서 초안을 작성하고, 최종 해석·수정·승인은 백주경 박사가 수행한다.

## Trigger

Use this skill when the user asks for any of the following:

- BQ2 뇌기능 검사지 PDF를 보고서로 작성
- 뇌파 검사 결과 분석 보고서 생성
- 교육채널 오름 / 백주경 박사 스타일의 뇌기능 분석결과 보고서 작성
- EEG, brainwave, 뇌기능, 기초율동, 알파소실률, 자기조절지수 등 지표 기반 상담 보고서 작성
- 센터·기관 제출용 뇌기능 분석결과 보고서 초안 작성

## Core Inputs

사용자가 제공할 수 있는 입력은 다음 중 하나 이상이다.

1. `BQ2 뇌기능 검사지` 원본 PDF
2. BQ2 검사 결과 이미지
3. 기존 분석 보고서 PDF
4. 수치가 정리된 텍스트/표
5. 대상자 기본정보와 상담 배경

입력이 PDF라면 먼저 텍스트를 추출한다. PDF 텍스트 추출이 불완전하면 `pypdf`, OCR, 이미지 렌더링 등을 사용해 수치를 검증한다. 숫자가 불확실하면 절대 추정하지 말고 `미확인`으로 표시한다.

## Output Goal

기본 출력은 `뇌기능 분석결과 보고서`다. 보고서는 다음 5가지 원칙을 만족해야 한다.

1. **정량성** — BQ2 원자료의 수치, 정상 참고범주, 좌우 차이, 종합점수를 정확히 반영한다.
2. **뇌과학적 설명** — 뇌파 대역, 기초율동, 각성·휴식 전환, 정서·행동 지표의 의미를 설명한다.
3. **정성적 해석** — 백주경 박사의 교육·상담적 언어로 대상자의 현재 상태를 입체적으로 해석한다.
4. **비진단 안전성** — 의학적 진단이 아닌 교육·상담 참고자료임을 명확히 한다.
5. **실천 가능성** — 대상자·가족·기관이 이해하고 지원 방향을 잡을 수 있게 쓴다.

## Medical and Ethical Safety Rules

반드시 지킨다.

- 의학적 진단을 내리지 않는다.
- `우울증입니다`, `ADHD입니다`, `치매입니다`, `장애입니다`처럼 확정 표현을 쓰지 않는다.
- `가능성`, `경향`, `시사합니다`, `참고할 필요가 있습니다`, `추가 평가가 필요합니다`를 사용한다.
- 대상자를 비난하지 않는다. 의지 부족, 성격 문제, 게으름으로 해석하지 않는다.
- 뇌파 결과는 검사 당시 컨디션, 수면, 약물, 스트레스, 환경의 영향을 받을 수 있음을 명시한다.
- 건강 이상, 고혈압, 수면장애, 우울감, 인지저하 위험 등은 의료기관 또는 전문가 상담과 병행할 수 있음을 안내한다.
- 기관 제출용 문서에는 개인정보 노출을 최소화한다. 공개·교육자료로 재사용할 때는 반드시 익명화한다.

## Required Workflow

### 1. Extract source data

PDF 또는 이미지에서 다음 정보를 추출한다.

- 대상자명
- 성별
- 생년월일 또는 나이
- 손잡이
- 측정일
- 측정장비
- 측정자/분석자
- 제출 대상
- 증상·복용약·건강 특이사항
- BQ2 주요 지표 전체

### 2. Normalize into metric schema

추출한 값을 아래 스키마로 정리한다.

```yaml
basic_info:
  subject_name:
  sex:
  birth_date:
  age:
  handedness:
  measurement_date:
  device: BQ2 뇌 기능 검사 시스템
  examiner:
  analyst: 백주경 교육학 박사 / 국가공인 브레인트레이너
  recipient:
  notes: []

metrics:
  basic_rhythm:
    left_hz:
    left_amplitude_uv:
    right_hz:
    right_amplitude_uv:
    reference: "9~11Hz / 3~8µV"
  coherence:
    value:
    reference: "0.8~0.99"
  cognition:
    total:
    left:
    right:
    reference: "24점 이상"
  sleep:
    alpha_attenuation_left:
    alpha_attenuation_right:
    total:
    reference: "알파소실률 0~5점 이상, 종합 50~55점 이상"
  emotion:
    frontal_alpha_left_percent:
    frontal_alpha_right_percent:
    total:
    reference: "40~60점"
  behavior:
    low_beta_left_percent:
    low_beta_right_percent:
    total:
    reference: "40~60점"
  stress:
    physical_left:
    physical_right:
    mental_left:
    mental_right:
    total_left:
    total_right:
    reference: "신체 ≤10, 정신 ≤15"
  attention_arousal:
    distractibility_left:
    distractibility_right:
    movement_left:
    movement_right:
    hyperactivity_left:
    hyperactivity_right:
    total_left:
    total_right:
    reference: "종합 40점 이상"
  brain_balance:
    raw:
    band:
    total:
    reference: "종합 50~80점"
  left_right_connectivity:
    correlation:
    consistency:
    total:
    reference: "상관성 ≥50, 일관성 ≥60"
  self_regulation:
    rest:
    attention:
    concentration:
    total:
    reference: "각 항목 25점 이상"
```

### 3. Verify numeric fidelity

보고서를 쓰기 전, `원자료 대조표`를 내부적으로 만든다. 모든 숫자 주장은 원문에서 확인된 값만 사용한다. 누락·불확실한 값은 `미확인`으로 표시하고 해석을 보수적으로 쓴다.

### 4. Apply interpretation rules

아래 해석 규칙을 사용하되, 개인의 맥락과 상담 배경에 맞춰 자연스럽게 통합한다.

#### 기초율동

- 기준보다 낮으면: 각성 수준 저하, 피로 누적, 처리 속도 저하, 새로운 과제 처리 부담 가능성.
- 좌우 진폭이 균형이면: 좌우 기능의 기본 균형은 유지될 가능성.
- 코히어런스가 기준보다 낮으면: 좌우뇌 정보 교류 효율이 경계 수준일 가능성.

#### 수면 / 알파 소실률

- 알파 소실률이 음수이거나 종합점수가 낮으면: 눈을 감아도 뇌가 휴식 모드로 전환되기 어렵거나, 눈을 뜬 상태에서도 이완파가 과도하게 유지될 가능성.
- 충분히 자도 개운하지 않음, 만성 피로, 회복력 저하, 인지 효율 저하와 연결해 설명할 수 있다.

#### 정서

- 정서 종합이 낮으면: 정서 안정성 저하, 사기 저하, 무기력, 우울감, 정서적 위축 가능성.
- 단, `우울증`으로 진단하지 않는다.
- 백주경 박사 스타일: “의지가 약해서가 아니라 뇌 에너지가 바닥난 번아웃성 상태”처럼 비난 없는 해석을 사용한다.

#### 행동

- 행동 종합이 낮으면: 실행력·추진력 저하, 소극적·방어적 행동 경향, 실수 회피, 과도한 자기검열 가능성.
- 좌우 우세율 차이가 크면: 행동 반응의 좌우 불균형, 방어적·보수적 상태를 설명한다.

#### 스트레스

- 신체 스트레스가 기준보다 높으면: 몸으로 체감되는 긴장, 피로, 근육 긴장, 자율신경계 부담 가능성.
- 정신 스트레스가 정상범위여도 신체 스트레스가 높으면: “마음으로는 버티고 있지만 몸은 이미 긴장 상태를 보일 수 있다”는 식으로 해석한다.

#### 주의·각성

- 종합점수가 낮으면: 주의 조절, 각성 유지, 과제 지속의 어려움 가능성.
- 산만·움직임·과잉 중 어느 항목이 높은지 구분해 설명한다.

#### 뇌 균형 / 좌우 연결성

- 뇌 균형 점수가 양호하면: 논리·언어 처리와 정서·공감 처리의 기본 기반은 유지됨.
- 연결성이 경계면: 정보 교류 효율이 완전히 최적화되어 있지는 않으므로 피로·스트레스 상황에서 기능 발휘가 제한될 수 있음.

#### 자기조절

- 휴식, 주의력, 집중력이 기준 미달이면: On/Off 전환 어려움, 쉬어도 쉬지 못함, 주의를 어디에 둘지 조절하는 힘 저하, 집중 유지 에너지 부족 가능성.
- 최우선 제언은 회복 환경, 수면 환경, 외부 요구 감소, 부담 완화다.

### 5. Write the report

기본 보고서는 다음 목차를 따른다.

```markdown
# 뇌기능 분석결과 보고서

## 기본 정보
## 1. 측정 목적
## 2. 뇌파의 개요 및 기능적 특징
### 2.1 뇌파의 이해
### 2.2 주요 뇌파 대역별 의미
## 3. 측정 결과
### 3.1 정밀 지표 데이터
### 3.2 측정 결과 요약
## 4. 결과 분석
### 4.1 기초율동 및 각성 수준
### 4.2 인지 및 정보 처리 특성
### 4.3 수면·회복 기능
### 4.4 정서·행동 성향
### 4.5 스트레스 및 자기조절
### 4.6 뇌 균형 및 좌우 연결성
## 5. 종합 의견
## 6. 교육·상담적 제언
## 7. 활용상 주의사항
```

### 6. Include quantitative table

보고서에는 반드시 정밀 지표 표를 포함한다. 예시:

| 평가 영역 | 세부 항목 | 측정 수치 | 참고 범주 | 해석 |
|---|---|---:|---:|---|
| 기초율동 | 좌/우 주파수 | 7Hz / 7Hz | 9~11Hz | 기준보다 낮아 피로·각성 저하 가능성 |
| 수면 | 알파 소실률 | -6 / -12, 종합 41 | 종합 50~55 이상 | 휴식 전환 및 수면 회복 관리 필요 |

### 7. Include qualitative synthesis

수치 나열로 끝내지 말고 반드시 사람 중심의 정성 해석을 붙인다.

좋은 문장 예시:

> 현재 나타나는 무기력감과 실행력 저하는 단순한 의지 부족이라기보다, 수면 회복 저하와 누적된 신체 긴장으로 인해 두뇌 에너지의 가용량이 감소한 상태와 관련되어 있을 가능성이 있습니다.

> 검사 결과상 기본적인 좌우 뇌 균형 기반은 유지되고 있으나, 휴식 전환과 자기조절 기능이 낮게 나타나 실제 생활에서는 작은 요구도 크게 부담스럽게 느껴질 수 있습니다.

### 8. Final verification before delivery

최종 답변 전 다음 체크리스트를 확인한다.

- [ ] 대상자 기본정보가 원문과 일치한다.
- [ ] 측정일이 원문과 일치한다.
- [ ] 모든 수치가 원자료에서 확인되었다.
- [ ] 불확실한 숫자를 추정하지 않았다.
- [ ] 정상범주 비교가 과장되지 않았다.
- [ ] 의학적 진단 표현이 없다.
- [ ] 백주경 박사의 정성적 해석 톤이 반영되었다.
- [ ] 기관 제출용으로 사용할 수 있을 만큼 공식적이다.
- [ ] 대상자를 비난하거나 낙인찍는 표현이 없다.
- [ ] 교육·상담 참고자료라는 주의 문구가 포함되어 있다.

## Default Report Style

- 한국어 공식 보고서체
- 전문적이되 보호자·기관 담당자가 이해할 수 있는 문장
- 문장은 길어도 2~3줄 안에서 끊는다
- 수치 기반 분석 → 의미 해석 → 생활/상담 영향 → 지원 방향 순서
- 대상자의 어려움을 `의지 부족`이 아니라 `뇌 에너지·회복력·자기조절의 저하`로 설명

## Required Disclaimer

보고서 상단 또는 하단에 다음 취지의 문구를 포함한다.

> 본 보고서는 BQ2 뇌파 검사 결과를 기반으로 한 교육·상담 참고자료이며, 의학적 진단을 목적으로 하지 않습니다. 검사 결과는 측정 당시의 수면, 피로, 스트레스, 약물, 환경 요인의 영향을 받을 수 있으므로 필요 시 의료·심리·상담 전문가의 평가와 함께 종합적으로 참고하시기 바랍니다.

## Optional Output Modes

사용자가 요청하면 같은 원자료로 다음 버전도 생성한다.

1. `professional-report` — 4~6쪽 전문 보고서형
2. `center-summary` — 기관 제출용 1쪽 요약본
3. `family-explanation` — 가족·보호자 설명용 쉬운 버전
4. `self-guidance` — 대상자 본인에게 전달할 위로·회복 중심 안내문
5. `counseling-script` — 백주경 박사 상담 설명 스크립트
6. `metric-verification-table` — 원자료 수치 대조표

## Privacy Rules

- 원자료에 포함된 이름, 생년월일, 건강정보는 민감정보로 취급한다.
- 외부 공개 예시나 교육자료에는 실명·생년월일·기관명 등 식별정보를 제거한다.
- 사용자가 보고서 생성을 요청한 경우에는 제공된 범위 내에서 문서에 필요한 개인정보만 사용한다.
- 샘플을 스킬 안에 저장할 때는 익명화된 예시만 사용한다.

## Change log

- v1.0.0 — 2026-08-24 KST: Initial production-oriented skill for converting BQ2 brain function sheets into 백주경 박사 스타일 뇌기능 분석결과 보고서 with quantitative EEG analysis, qualitative counseling interpretation, safety rules, and verification checklist.
