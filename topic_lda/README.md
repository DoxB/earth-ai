# LDA Topic Modeling: 경제 뉴스 기반 이벤트 노드 생성
## 프로젝트 개요
YouTube 경제 뉴스 스크립트를 기반으로, LDA(Latent Dirichlet Allocation)를 활용한 토픽 모델링을 수행하였습니다.  
각 뉴스 스크립트 내 단어 빈도 기반으로 토픽을 추출하고, 스크립트와 토픽 간 확률을 계산하여 **스크립트별 주요 토픽을 기반으로 이벤트 노드**를 생성하였습니다.

## 데이터 구조

경제 뉴스 스크립트를 수집:

| 채널 | 수집 건수 |
|------|-----------|
| SBS Biz News | 262건 |
| 한국경제 TV | 756건 |
| 연합뉴스 경제TV | 319건 |
| 서울경제TV | 1,163건 |
| **합계** | **3,707건** |

모든 스크립트는 문장 단위로 분리하여 전처리하였으며, 명사 기반의 단어 토큰을 중심으로 불용어 처리를 진행하였습니다.

## 데이터 생성

스크립트 전처리 후 다음과 같은 순서로 LDA 토픽 모델을 구축합니다:

1. **명사 토크나이징**  
   각 문장을 대상으로 명사만 추출 (예: Kiwi 등 사용)
2. **TF-IDF 분석**  
   문서-단어 행렬 생성
3. **불용어 제거**  
   의미 없는 단어 제거 및 토픽 품질 향상
4. **LDA 모델 학습 (Gensim)**  
   최적의 토픽 수를 찾아 모델 학습 수행<br>

→ 최종적으로 각 스크립트에 대해 확률 분포 기반으로 **주요 토픽 할당 → 이벤트 노드 생성**

<p align="center">
  <img src="assets/LDA_Flow.png" height="450" width="1000"/>
</p>

## 토픽 수 결정 및 결과 해석

- Perplexity와 Coherence Score를 기반으로 최적의 토픽 수를 탐색한 결과,  
  **30개의 토픽(이벤트 노드)**가 가장 적절한 수로 나타났습니다.
- 각 토픽은 키워드 기반으로 요약 가능하며,  
  스크립트별 토픽 분포를 바탕으로 향후 **개념 연결, 이벤트 그래프 구축**에 활용될 수 있습니다.

<p align="center">
  <img src="assets/per_2293.png" width="500"/>
  <img src="assets/coh_2293.png" width="500"/>
</p>

## 실행 방법
1. 환경 설정
```
- python 3.9
- kiwipiepy 0.21.0
- gensim 4.3.3
- scikit-learn 1.6.1
- scipy 1.13.1
```
2. 의존성 설치
```
pip install -r requirements.txt
```
3. 실행 
```
python main.py
```

## 프로젝트 구조
```
Topic_LDA/
├── assets/                # 데이터
├── CustomTokenizer.py     # 명사 단위 토크나이저
├── lda_modeling.py        # 명사로만 이루어진 스크립트와 불용어 사전, 카운터 벡터를 전달받아 LDA 토픽 모델링 진행
├── main.py                # 전처리부터 LDA 토픽 모델링, 저장까지 관리
├── preprocess.py          # 스크립트 txt 파일들을 불러와 전처리
├── stopwords.py           # TF-IDF 분석 기반으로 불용어 사전 구축하여 불용어 처리
└── requirements.txt       # 환경 설정
```
