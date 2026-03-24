# Custom_KoBERT_Tokenizer_project


## 1. Overview
자연어 처리 수업의 팀 프로젝트로, 한국어 문장의 의미 구조를 보존하는 **커스텀 BPE 토크나이저**를 설계하고 구현했습니다.

기존 BPE(Byte Pair Encoding)는 토큰의 **빈도 기반 병합**을 수행하기 때문에, 한국어와 같은 교착어에서는 의미 단위가 손상될 가능성이 있습니다.  
본 프로젝트에서는 이러한 한계를 해결하기 위해 **한국어 특성을 반영한 전처리 기반 BPE 토크나이저**를 구현했습니다.

---

## 2. Problem Definition

한국어는 어간(stem)과 어미(ending)가 결합되는 **교착어 구조**를 가지며, 단순한 BPE 적용 시 다음과 같은 문제가 발생합니다:

- 의미 단위가 아닌 **빈도 중심 토큰 생성**
- 어간과 어미가 섞인 비정상적인 subword 생성
- 감정/의미의 핵심 단위 훼손

예시:
- "죽을것같아" → 하나의 토큰으로 병합될 가능성
- "슬퍼보여" → 감정 의미(슬프-)가 분리되거나 왜곡

---

## 3. Approach

이 문제를 해결하기 위해, BPE 적용 이전 단계에서 **한국어 구조를 반영한 전처리 파이프라인**을 설계했습니다.

### 핵심 아이디어

1. **형태소 단위 분해**
   - 형태소 분석기를 활용하여 문장을 의미 단위로 분리

2. **어간/어미 분리**
   - 동사/형용사의 어간을 보존하여 의미 중심 단위 유지

3. **BPE 병합 제한 기법**
   - 동사/형용사 어간 뒤에 특수 기호(`▁`) 삽입
   - 해당 기호를 기준으로 병합이 확장되지 않도록 제한

👉 결과적으로,  
**BPE가 의미 단위를 침범하지 않도록 구조적으로 제어**

---

## 4. Project Structure

```
Custom_KoBERT_Tokenizer_project/
│
├── data/
│   └── 5차년도_2차.csv
│
├── notebook/
│   ├── 데이터분석.ipynb
│   ├── bpe_1차.ipynb
│   └── Custom_KoBERT_Tokenizer_Project.ipynb
│
├── README.md
```
---


## 5. Experiments

### 1) 데이터 분석
- 감정 분류 데이터셋 기반 텍스트 분포 및 특성 분석
- 한국어 문장의 형태적 특징 확인

📄 `데이터분석.ipynb`

---

### 2) Baseline BPE 적용
- 기존 BPE 토크나이저를 그대로 적용
- 의미 단위가 아닌 병합 결과 확인

📄 `bpe_1차.ipynb`

---

### 3) Custom Tokenizer 구현
- 형태소 분석 + 어간 분리 + 병합 제한 적용
- 의미 중심 subword 생성 유도

📄 `Custom_KoBERT_Tokenizer_Project.ipynb`
- [코랩에서 확인하기](https://drive.google.com/file/d/1ECm4wIkcQIwXYhkM_GA8zAmqIMczvYnK/view?usp=sharing)
- 🔍 [nbviewer로 보기](https://nbviewer.org/github/jeongsonghui/Custom_KoBERT_Tokenizer_project/blob/main/Custom_KoBERT_Tokenizer_Project.ipynb)


---

## 6. Results

기존 BPE 대비 다음과 같은 개선을 확인:

- 어간(stem) 중심 토큰 유지
- 감정 표현의 핵심 의미 보존
- 문장이 더 자연스러운 의미 단위로 분해됨

👉 단순 빈도 기반 토크나이징 → **의미 기반 토크나이징으로 개선**

---

## 7. Key Insight

- 토크나이저는 단순 전처리가 아니라 **모델 성능에 직접적인 영향을 주는 핵심 요소**
- 특히 한국어와 같은 교착어에서는  
  **언어 구조를 반영한 설계가 필수적**

---

## 8. Limitations & Future Work

- 형태소 분석기의 품질에 의존
- 규칙 기반 병합 제한의 일반화 한계 존재

향후:
- 데이터 기반 병합 제어 방식 적용
- SentencePiece / Unigram 모델과 비교 실험

---

## 9. Tech Stack

- Python
- BPE Tokenization
- KoNLPy (형태소 분석)
- Jupyter Notebook

---
