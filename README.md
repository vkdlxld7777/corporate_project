
# ChatBot12
기업협업으로 코드자료 공유 불가


# 1. 프로젝트 개요
    
    시니어들을 위한 데이트앱에서 공통 관심사에서 원활한 대화를 위해 AI가 맞춤형 대화 스크립트를 생성 및 추천해주는 챗봇 시스템
    


# 2. 모델

### 데이터셋
1. Open Source 챗봇 데이터셋
    [https://github.com/songys/Chatbot_data]

2. AI Hub (40대 이상 데이터만 추출)
    [https://www.aihub.or.kr/aihubdata/data/view.do?currMenu=115&topMenu=100&aihubDataSe=realm&dataSetSn=543]

3. 웰니스 데이터셋
    [https://github.com/nawnoes/WellnessConversation-LanguageModel]


### 모델 Architecture
1. Define Token
    *  Q_TKN, A_TKN, BOS, EOS, PAD, MASK, SENT, PAD

2. Define Chatbot Class
    *  len : 길이 함수
    *  getitem : 데이터 로더 함수
        1. 데이터 전처리 - regex
        2. Tokenization
            *  q_toked(질문 시작 토큰, 질문 데이터)
            *  a_toked(답변 시작 토큰, 답변 데이터, 대화 종료 토큰)
        3. max length 설정
        4. labeling - 답변지 생성 (질문 마스킹, embedded 답변 데이터, max_len 만큼 남은 길이 패딩)
        5. masking - labels 를 mask화 (0,0,0, …, 1,1,1, …, 0,0,0,…)
        6. Ids
            *  labels_id (9(MASK<unused0>),9,9,…, a1,a2,a3,…1(EOS</s>),3(PAD<pad>),3,3,…)
            *  token_ids (2(Q_TKN<usr>), q1,q2,q3,…,4(A_TKN<sys>),a1,a2,a3,…1(EOS</s>),3(PAD<pad>),3,3,…)

3. batch - batch 합쳐주는 함수

4. model - 모델 돌려서 학습해주는 함수
    *  Hyper-parameters
        1. epoch : 10
        2. learning_rate : 3e-5
        3. Sneg : 1e18
        4. criterion : CrossEntropyLoss(다중분류함수)
        5. optimizer : Adam



# 3. 서비스 설명

### 디렉토리 구조
1. model : 모델 피클화 해서 저장하는 경로
2. data : 추가될 데이터 저장하는 경로
3. `main.py` : 가장 최신 pkl 모델 불러와서 스크립트 추천 목록 생성 파일
4. `kogpt2_model.py` : 가장 최신 데이터(csv) 불러와서 모델 학습 후 pkl 파일로 생성
5. `batch.py` : kogpt2_model.py 실행 및 model 폴더와 data 폴더 관리
6. `gpt2_modeling.ipynb` :


    
# 4. 프로젝트의 결과
1. AI학습시 각 연령대별 대화내용을 연령별로 정제하여 교육을 시킬때 정말 확실하게 AI가 쓰는 어투가 달라진다는것을 확인할 수 있던 결과였음.
2. 문장의 수량이 너무 적어 중복된 대화가 채택이 되는 경향이 있었음. 따로 출력 결과에 따른 예외처리를 적용하였으나 비효율적인 시간 소모가 존재.
3. 추가적인 교육을 통해 지속적으로 모델을 강화하는 형식으로 개발하였으며 앱이 오픈되고 실질적으로 시니어들의 대화내역을 사용 할 수 있다면 더 좋은 결과물이 나올것.
    

# 5. 한계점 및 개선방안
1. 대화의 스크립트가 젊은 언어가 들어가 있다면 AI가 말을 하는데 부자연스러웠던 부분이 존재. 50대 이상의 대화내역을 찾으려 하였는데 너무 수량이 적어 적당히 40대로 합의를 보았음.

2. 아직까지는 데이터의 양이 적어 GPT를 이용한 문장생성을 이용하였지만 추후 앱이 활성화되고 대화내역 데이터를 100만건 이상 확보한뒤 
   BERT모델로 변환하여 교육하면 조금 더 안정적인 대화선택지를 줄 수 있음.

#
