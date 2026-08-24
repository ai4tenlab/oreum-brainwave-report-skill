# BQ2 Metric Schema

Use this schema whenever converting a BQ2 뇌기능 검사지 into a 뇌기능 분석결과 보고서.

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

## Numeric handling rules

- Never invent missing metrics.
- Preserve units: Hz, µV, %, 점.
- If extraction is uncertain, mark `미확인` and avoid strong interpretation.
- Every numeric claim in the report must be traceable to the source PDF or user-provided table.
