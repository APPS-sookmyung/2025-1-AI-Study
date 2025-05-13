# 📄 논문 리뷰: LLaMA: Open and Efficient Foundation Language Models

## **링크**: [arXiv:2302.13971](https://arxiv.org/abs/2302.13971)

## 목적

> **GPT-3보다 10배 작은 모델이 더 좋은 성능을 낸다면?**  
> LLaMA는 공개된 데이터만으로 학습된 고성능 Transformer 계열 언어모델로, 누구나 사용할 수 있게 오픈소스로 제공

---

## 기존 모델과의 비교

- 기존 대형 언어모델(GPT-3, PaLM)은 **폐쇄적**이고 **비용이 매우 높음**
- LLaMA는:
  - 모델 전체 **오픈소스 공개**
  - **공개 데이터셋만** 사용
  - **작은 모델도 고성능** (13B가 GPT-3 175B보다 성능 우수)
  - **싱글 GPU로 추론 가능** → 연구자 및 학습자 접근성 ↑

---

## 모델 스펙

| 모델      | 파라미터 수 | 학습 토큰 수 | 특징                        |
| --------- | ----------- | ------------ | --------------------------- |
| LLaMA-7B  | 7B          | 1.0T         | 가볍고 빠름                 |
| LLaMA-13B | 13B         | 1.0T         | GPT-3 능가                  |
| LLaMA-33B | 33B         | 1.4T         | PaLM급                      |
| LLaMA-65B | 65B         | 1.4T         | Chinchilla/PaLM-540B와 경쟁 |

---

## 모델 구조

> 위치: **논문 2.2 Architecture 섹션**

LLaMA는 기본적으로 Transformer 기반이지만, 다음과 같은 변경 사항이 있음:

- **RMSNorm**: LayerNorm 대신 → 학습 안정성 향상
- **SwiGLU**: ReLU보다 성능 좋은 활성화 함수
- **RoPE positional embedding**: 절대 위치 임베딩 대신 회전 임베딩 사용
- **Byte Pair Encoding (BPE)**: 일반적인 서브워드 분할 방식
- **코드 최적화**: GPU 메모리 효율 높이고 학습 속도 개선

---

## 학습 데이터셋

> 위치: **논문 2.1 Pre-training Data 섹션**

**총 1.4T tokens**, 모두 공개 데이터로 구성:

| 데이터셋         | 비율 | 설명                   |
| ---------------- | ---- | ---------------------- |
| CommonCrawl      | 67%  | 웹 수집 텍스트 (CCNet) |
| C4               | 15%  | 품질 필터링된 웹 문서  |
| GitHub           | 4.5% | 오픈소스 코드          |
| Wikipedia        | 4.5% | 다국어 위키 문서       |
| Gutenberg/Books3 | 4.5% | 공공 도서              |
| ArXiv            | 2.5% | 수학/과학 논문         |
| StackExchange    | 2.0% | 질의응답 데이터        |

---

## 성능 비교 요약

### Zero-shot common sense tasks

| 모델         | BoolQ    | PIQA     | ARC-c    |
| ------------ | -------- | -------- | -------- |
| GPT-3 (175B) | 60.5     | 81.0     | 51.4     |
| LLaMA-13B    | 78.1     | 80.1     | **52.7** |
| LLaMA-65B    | **85.3** | **82.8** | **56.0** |

### Reading Comprehension (RACE)

| 모델         | Middle   | High     |
| ------------ | -------- | -------- |
| GPT-3 (175B) | 58.4     | 45.5     |
| LLaMA-13B    | 61.6     | 47.2     |
| LLaMA-65B    | **67.9** | **51.6** |

### Math Reasoning (GSM8k)

| 모델      | 0-shot   | 5-shot   |
| --------- | -------- | -------- |
| LLaMA-13B | 17.8     | 35.6     |
| LLaMA-65B | **50.9** | **69.7** |

---

## Instruction tuning 효과

- LLaMA 기본 모델은 instruction-following 능력이 낮음
- 하지만 간단한 instruction finetuning만으로도 MMLU 점수 **+5~6%** 향상
- `LLaMA-I`는 MMLU 5-shot에서 **68.9%** 기록 (Flan-PaLM보다 높음)

---

## 편향성과 독성

- **모델이 클수록 독성 점수(Toxicity) 상승**
- CrowS-Pairs / WinoGender / TruthfulQA 등의 벤치마크로 검증
- GPT-3와 유사하거나 조금 낮은 수준의 편향 존재

---

## 탄소 발자국 (Carbon Footprint)

| 모델      | GPU시간 | 전력소모(MWh) | 탄소배출량(tCO2eq) |
| --------- | ------- | ------------- | ------------------ |
| LLaMA-7B  | 82k     | 36            | 14                 |
| LLaMA-65B | 1M+     | 449           | 173                |

> 모델 학습 자체는 많은 전력을 사용하지만, 모델이 공개되어 **재활용 가능**하다는 점에서 탄소 저감에 기여할 수 있음

---

## 결론

- **LLaMA는 고성능 LLM의 "대중화"에 기여**
- 성능은 뛰어나면서도 가볍고, 공개된 데이터만 사용
- 학부생, 연구자, 스타트업 모두에게 실용적인 모델 시리즈
- 앞으로 instruction tuning 및 downstream task 연구에 매우 유용

---
