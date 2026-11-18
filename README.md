# ETF 레버리지·분산투자 대시보드

미국 ETF 7종의 5년치 일별 가격 데이터로 두 가지를 직접 확인해 보는 Streamlit 대시보드입니다.

- **레버리지의 함정** — 3배 레버리지 ETF가 1배보다 못 벌 수 있습니다. 배수는 하루짜리 약속이지 5년짜리 약속이 아닙니다.
- **분산투자의 힘** — 변동성이 큰 자산을 섞었는데 포트폴리오 변동성이 오히려 줄어듭니다.

사이드바에서 분석 시작일을 바꿔 보시면 같은 데이터로 결론이 뒤집히는 것을 보실 수 있습니다.

> 채워 넣으세요: 이 프로젝트는 사내 교육 과정에서 만들었습니다. 작성자 ______

---

## 배포 주소

> 채워 넣으세요: https://__________.streamlit.app

---

## 데이터

| 항목 | 내용 |
|------|------|
| 출처 | [Twelve Data](https://twelvedata.com) `time_series` API |
| 심볼 | `SPY` `VTI` `QQQ` `QLD` `TQQQ` `SCHD` `GLD` (7종) |
| 기간 | 2021-08-23 ~ 2026-08-20 |
| 주기 | 일별(`1day`), 심볼별 1,254 영업일 |
| 파일 | `data/etf_prices.csv` 하나 |

컬럼은 `symbol`, `datetime`, `open`, `high`, `low`, `close`, `volume` 입니다.
결측·중복이 없고 액면분할이 반영된 가격임을 `quality_check.py`로 검증했습니다.

### 데이터 한계

**종가 기준 가격 수익률이며 배당은 포함되지 않았습니다. 실제 성과와 다릅니다.**
특히 배당이 핵심인 `SCHD`는 실제보다 과소평가되어 있습니다.

대시보드는 `data/etf_prices.csv`만 읽습니다. 실행 중에 API를 호출하지 않으므로
**앱을 쓰는 데에는 API 키가 필요하지 않습니다.**

---

## 로컬에서 실행하기

```bash
git clone <저장소 주소>
cd <폴더명>

python -m venv .venv
.venv\Scripts\activate        # Windows
# source .venv/bin/activate   # macOS / Linux

pip install -r requirements.txt
streamlit run app.py
```

브라우저에서 `http://localhost:8501` 이 열립니다.

---

## 데이터를 직접 수집하려면

저장소에 이미 `data/etf_prices.csv`가 들어 있으므로 이 단계는 선택입니다.
직접 수집하시려면 Twelve Data API 키가 필요합니다.

1. [twelvedata.com](https://twelvedata.com) 에서 무료(Basic) 플랜으로 가입하고 API 키를 발급받습니다.
2. `.env.example`을 `.env`로 복사한 뒤 키를 넣습니다.

   ```
   TWELVEDATA_API_KEY=발급받은_키
   ```

3. 수집을 실행합니다.

   ```bash
   python collect.py
   ```

`.env`는 `.gitignore`에 포함되어 있어 저장소에 올라가지 않습니다. **실제 키를 코드나 문서에 적지 마세요.**

무료 플랜은 분당 8크레딧, 일 800크레딧입니다. 심볼 1개 호출이 1크레딧이며,
`collect.py`는 호출 사이에 대기를 넣어 한도를 넘기지 않습니다.

> 주의: 데이터를 새로 수집하면 기간이 위 표와 달라져 대시보드의 수치도 달라집니다.

---

## 구성

| 파일 | 역할 |
|------|------|
| `app.py` | Streamlit 대시보드 |
| `collect.py` | Twelve Data에서 가격 데이터 수집 |
| `quality_check.py` | 수집 데이터 품질 검증 리포트 |
| `data/etf_prices.csv` | 가격 데이터 |
| `AGENTS.md` | AI 에이전트에게 적용되는 프로젝트 공통 규칙 |
| `prompts/` | 단계별 작업 지시서 |

---

## 면책

이 저장소는 과거 데이터를 설명하기 위한 **교육용 자료**입니다.
미래 수익률을 예측하지 않으며, 특정 종목의 매수나 매도를 권유하지 않습니다.
투자 판단과 그 결과에 대한 책임은 투자자 본인에게 있습니다.
