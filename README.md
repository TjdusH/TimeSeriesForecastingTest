# 비트코인 가격 예측 및 트레이딩 전략 프로젝트 📈💰

> **딥러닝 기반 시계열 예측 및 동적 임계값(Dynamic Threshold) 트레이딩 전략 프로젝트** > 비즈니스 딥러닝 최종 과제 제출용 리포트입니다.

---

## 📋 1. Project Overview (프로젝트 개요)
본 프로젝트는 변동성이 심한 비트코인(BTC) 시장에서 안정적인 초과 수익(Alpha)을 달성하기 위해 설계되었습니다.
단순한 LSTM 모델의 한계를 넘어 **Stacked GRU** 아키텍처를 도입했으며, 예측 확률의 상대적 우위를 활용한 **동적 임계값(Dynamic Threshold) 전략**을 통해 하락장에서도 수익을 방어하는 알고리즘을 구현했습니다.

* **Target:** Bitcoin (BTC/USD) Daily Close Price
* **Goal:** Buy-and-Hold 전략 대비 초과 수익 달성
* **Key Achievement:** 하락장 속 **+32.38% 수익률** 달성 (벤치마크 대비 **+35.35%p**)

---

## 🏗️ 2. Model Architecture (모델 설계)

기존의 단층 LSTM 모델 대신, Stacked GRU (2-Layer Gated Recurrent Unit)를 채택하여 모델의 표현력을 극대화했습니다.

| Layer | Configuration | Description |
| :--- | :--- | :--- |
| **Input** | `(Batch, Seq, Feature)` | 시계열 데이터 입력 |
| **Backbone** | **Stacked GRU** | **Hidden Size 128, 2 Layers**<br>복잡한 비선형 패턴 및 장기 의존성 학습 |
| **Norm** | `LayerNormalization` | 학습 안정화 및 Vanishing Gradient 방지 |
| **Regularization** | `Dropout (0.3)` | 과적합(Overfitting) 억제 및 일반화 성능 강화 |
| **Output** | `Sigmoid` | 0~1 사이의 상승 확률 출력 |

### 💡 Why Stacked GRU?
* **Efficiency:** LSTM 대비 파라미터 수가 적어 학습 속도가 빠르고, 금융 데이터의 노이즈에 강인합니다.
* **Depth:** 2개의 층(Stacked)을 쌓아 단순 등락 이면의 심층적인 추세 패턴을 포착하도록 설계했습니다.

---

## ⚡ 3. Investment Strategy (투자 전략)

모델의 예측값(Score) 절대치에 의존하지 않고, 데이터 분포의 상대적 순위(Percentile)를 활용한 **"Dynamic Threshold & Hysteresis"** 전략을 독자적으로 개발했습니다.

### 📊 전략 로직 (Logic)
1.  **Dynamic Buy (진입):** 예측 확률 **상위 20%** 구간 (High Confidence) 진입
    * *Why?* 모델의 예측값이 0.5 부근에 몰려도(Calibration Issue), 상대적으로 확실한 구간을 찾아냅니다.
2.  **Dynamic Sell (청산):** 예측 확률 **하위 20%** 구간 (Low Confidence) 청산
    * *Why?* 확실한 하락 시그널에서만 매도하여 불필요한 손절을 방지합니다.
3.  **Hysteresis (존버/관망):** 매수/매도 기준 사이 구간 (Middle 60%)
    * *Action:* **Hold Current Position.** (포지션이 있으면 유지, 없으면 관망)
    * *Effect:* 잦은 매매(Whipsaw)로 인한 수수료 손실을 막고 추세를 길게 가져갑니다.

---

## 🏆 4. Backtest Results (성과 분석)

테스트 데이터셋 기준 시뮬레이션 결과, 벤치마크를 압도하는 성과를 기록했습니다.

| Performance Metric | My Strategy 🤖 | Buy & Hold (Benchmark) 📉 | Difference (Alpha) |
| :--- | :---: | :---: | :---: |
| **Final Return** | **+ 32.38 %** | - 2.97 % (Est.) | **+ 35.35 %p** 🚀 |
| **Final Capital** | **$ 13,237.71** | $9,703.00 | +$ 3,534.71 |
| **Total Trades** | **18 회** | 2 회 | 최적화된 매매 빈도 |
| **Fees Paid** | $ 214.34 | - | 비용 효율성 확보 |

> **Analysis:** 벤치마크가 손실을 기록하는 동안, 본 전략은 하락 시그널을 감지하여 현금 비중을 늘림으로써 자산을 방어하고 유효한 상승 파동만을 포착했습니다.

---

## 📝 5. Discussion & Conclusion (결과 고찰)

### ✅ 성공 요인
1.  **전략이 모델을 보완하다:** 모델의 Validation Loss가 상승하는 과적합 경향이 있었으나, **순위(Ranking) 기반 전략**을 통해 모델이 가장 확신하는 양질의 신호만 필터링한 것이 주효했습니다.
2.  **비용 관리:** 고정 임계값(0.5) 사용 시 발생할 수 있는 잦은 매매를 억제하여, 수수료 비용을 전체 수익의 6% 수준으로 통제했습니다.

### ⚠️ 한계 및 개선점
* **외부 변수 부재:** 기술적 지표 외에 거시 경제(금리, 나스닥) 변수를 반영하지 못해 급격한 시장 충격 대응에는 한계가 있습니다.
* **Future Work:** Transformer 기반 모델 도입 및 뉴스 감성 분석(Sentiment Analysis) 데이터를 추가하여 예측력을 높일 예정입니다.

---

## 💻 6. How to Run (실행 방법)

```bash
# 1. 저장소 클론
git clone [https://github.com/TjdusH/TimeSeriesForecastingTest.git](https://github.com/TjdusH/TimeSeriesForecastingTest.git)

# 2. 필수 라이브러리 설치
pip install torch pandas numpy matplotlib scikit-learn

# 3. 메인 코드 실행
python assignment_main.py