# HCEE Dashboard

**Human-Centered Experience Engineering — Operational Governance Artifact**

본 대시보드는 박사논문 *"AX 시대 의료조직의 경험 기반 AI 거버넌스"* 의 운영 가능한 IS Artifact 구현체입니다. 환자경험지수(PXI)와 직원경험지수(EXI)를 입력 신호로 받아, 자동 트리거 룰을 통해 4개 통제 레버(AAL · HOI · IF · TR)를 실시간 조정하는 폐쇄루프 거버넌스 시스템을 시연합니다.

🔗 **Live Demo**: <https://hcee-dashboard.streamlit.app/>

---

## Core Concept · 핵심 개념

> **의료 AI 거버넌스는 폐쇄루프 피드백 운영 시스템이 되어야 한다.**
>
> *Healthcare AI governance must become a closed-loop feedback operating system.*

- **경험은 결과가 아닌 입력이다** — PXI와 EXI는 사후 성과지표가 아니라 거버넌스를 작동시키는 입력 신호.
- **DSR (Design Science Research) Artifact** — 시뮬레이션을 넘어 실제 작동하는 정보시스템 거버넌스 구현체.
- **Multi-Temporal Synthesis** — 30 tick · 24 시간 · 현재 · 전체 누적의 4 가지 자연 시간 척도를 단일 폐쇄루프 안에 통합.

---

## Architecture · 아키텍처

### 4-Layer Closed-Loop

```
  ┌────────────────────────────────────────────┐
  │  POLICY        전략 · 안전 · 공정성 목표    │
  │  GOVERNANCE    해석 · 심의 · 승인 · 에스컬레이션 │
  │  CONTROL       4 레버 운영 (AAL/HOI/IF/TR)  │
  │  EXPERIENCE    PXI · EXI 입력 신호           │
  └────────────────────────────────────────────┘
       ↑ Signals (경험 → 거버넌스)
       ↓ Adjustments (거버넌스 → 운영)
```

### Trigger-Control Rules · 트리거 룰

| ID    | Condition          | Action                              | Academic Basis           |
| ----- | ------------------ | ----------------------------------- | ------------------------ |
| TC-01 | PXI < 70           | AAL ↓ · IF ↑                         | PROMs / MCID literature  |
| TC-02 | EXI < 65           | HOI ↑ · TR ↑                         | Reason (1997) HRO theory |
| TC-03 | \|PXI − EXI\| > 15 | Cross-domain rebalance              | SPC + 1.5 SD margin      |

### Control Levers · 통제 레버

| Lever | Full Name                  | Normal → Engaged   | Academic Basis            |
| ----- | -------------------------- | ------------------ | ------------------------- |
| AAL   | AI Autonomy Level          | 1.00 → 0.80 (−20%) | Parasuraman & Wickens (2008) |
| HOI   | Human Oversight Intensity  | 0.25 → 1.00 (MAX)  | HRO defense-in-depth      |
| IF    | Interaction Frequency      | 1.00 → 1.20 (+20%) | AAL inverse-symmetric     |
| TR    | Trust Recovery             | 0.50 → 1.00 (FULL) | Lee & See (2004)          |

---

## Run Locally · 로컬 실행

### Requirements

- Python 3.9+
- pip

### Installation

```bash
git clone https://github.com/mskim924-lab/hcee-dashboard2.git
cd hcee-dashboard2
pip install -r requirements.txt
```

### Launch

```bash
streamlit run hcee_dashboard.py
```

브라우저에서 자동으로 <http://localhost:8501> 열립니다.

---

## Live Demo Flow · 4 단계 데모 시나리오

### Step 1 — Overview Tour (25 sec)

화면 구조 소개: 좌측 Control Panel + ① 실시간 상태 + ② Multi-Tab Synthesis + ③ Decision Support. 초기 상태는 PXI=75, EXI=70 (모두 임계값 위), "Normal operation".

### Step 2 — Generate Signal × 4 → TC-01·02 Fire (65 sec)

좌측 사이드바의 `Generate signal` 버튼을 4번 클릭. 매 클릭마다 PXI는 -1.8, EXI는 -1.7씩 결정적으로 하강합니다. 4 클릭 후 PXI≈67.8 (< 70), EXI≈63.2 (< 65) — TC-01과 TC-02 동시 발화. 4 레버 모두 자동 활성화 (AAL 1.0→0.8, HOI 0.25→1.0, IF 1.0→1.2, TR 0.5→1.0).

### Step 3 — Multi-Temporal 2×2 + Sub-Dimension Driver (60 sec)

② 섹션의 4개 셀이 같은 PXI/EXI 데이터를 서로 다른 자연 시간 척도로 종합:
- 🔄 **Daily Governance Loop** — 최근 30 tick
- 🤖 **AI Monitoring** — 현재 + 24 시간 rolling
- 🔍 **Experience Drivers** — 현재 스냅샷
- 📈 **Adaptive Learning** — 배포 이후 전체

`Daily Governance Loop` 탭 클릭 → PXI / EXI 각각 6 sub-dimension으로 분해 → driver 차원 자동 검출.

### Step 4 — EXI Shock → TC-03 Fire (30 sec)

`EXI shock` 버튼 클릭. PXI +5 회복 + EXI -15 충격 → 격차 24+ → TC-03 발화. S4 시나리오 실시간 재현.

### Reset

데모 사이 `Reset` 버튼으로 초기 상태(PXI=75, EXI=70) 복원.

---

## Repository Structure · 파일 구조

```
hcee-dashboard2/
├── hcee_dashboard.py    # Main Streamlit app (2,674 lines)
├── requirements.txt     # Python dependencies
└── README.md            # This file
```

---

## Research Context · 연구 맥락

본 대시보드는 다음 연구의 IS Artifact 구현체입니다:

- **저자**: 김민성 (aSSIST University AI 융합공학 박사과정)
- **소속**: 포그니병원 (Pogni Hospital, <https://www.pogny.co.kr>)
- **학회 발표**: KMIS 2026 (2026. 6. 5.)
- **투고**: MDPI Systems (SCI Q1, under review)
- **파일럿 사이트**: 포그니병원 (2026 – 2027)

### Three Original Contributions (DSR Framework)

1. **Theory** — HCEE Framework: 경험을 결과가 아닌 입력으로 재정의, 4 이론적 전통(Galbraith · Beer · Folke · Trist) 통합
2. **Method** — Theoretically-Grounded Operationalization: 모든 임계값·레버값을 학술 문헌에서 도출, ABM 검증 + 보상적 강화 (S4 > S3) 발견
3. **Artifact** — Operational IS Artifact: 시뮬레이션을 넘어 실제 작동하는 거버넌스 도구 (본 대시보드)

---

## License · 라이선스

Academic research use. 박사논문 연구 산출물. 상업적 활용 및 컨설팅 BM 관련 문의는 저자에게 직접 연락 바랍니다.

---

## Contact · 문의

**김민성 (Minseong Kim)**
포그니병원 · aSSIST AI 융합공학 박사과정
HCEE™ Healthcare AI Governance Architecture (TM pending)
