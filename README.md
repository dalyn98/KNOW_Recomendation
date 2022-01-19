# KNOW_Recomendation (Dacon)

코드 정리 XXX  
Tabular data  

### 1208 

rf,xgb,LogisticRegression,lgbm,decTree,SVC 성능 확인  (Non Parameter)
-> rf가 가장 좋음, xgb,lgbm같은 boosting의 경우 parameter 조정시 rf보다 더 좋은 성능이 나올수 있음

### 1209

Scaler 사용, MinMax, Standard
-> Standard 성능이 뛰어남
-> Z-score 사용도 고려

Imbalnced Class라 Smote사용하여 Oversampling 해봤으나 데이터 사이즈가
20만 * 600로 급격히 커져 연산이 되지않음
억지로 연산해도 과적합 문제가 있으므로 Sampling 관련으로 해결이 필요함

### 1210

성별 bining -> Old, Middle, Young  
연속형(연봉) bining -> LS, HS, NA(무응답)  
-> ! : 연봉 데이터는 bq41_1,2,3의 형식으로 되어있음  
-> 1의 문장을 답하지 않았다면 결측, 2번에 응답, 2번에 응답하지 않을경우 결측, 3번에 응답 
-> 해당 데이터의 경우 bining을 하는것이 맞는지?

bining으로 feature를 늘려도 성능에는 변화가 없음, 오히려 줄어듦  
약 166개의 feature를 하나씩 조정하는게 맞을까?
-> 일단 연도별마다 feature의 종류, 개수가 다르니 하나씩 조정해보는 방향으로 진행


### 1210
label 확인결과 데이터가 imbalnced하지 않음 sampling 사용 X
optuna 사용하여 rf, xgb의 parameter 조정, 성능 확인  
성능의 변화가 없을경우 feature 조정하는 방향으로    
! : regression 문제로 진행하는 방향도 생각 

### 1211 ~~ 1226
각 데이터에 셀이 밀린경우 모두 삭제함  
개수가 적어 정보 유실의 우려는 적다고 판단됨  


1. 성능 검증결과 (rf,xgb,lgbm,svc,knn)중 lgbm의 성능이 가장 뛰어남 (parameter tuning) lgbm결과 Acc 0.57 수준    
2. 성능 개선 위해 방법 모색 -> target encoder(catboost encoder) 사용, 별다른 변화 없음  
3. tabnet 사용 category feature index 구분해주는 과정에서 알수없는 오류로 인해 사용 불가능(skip embedding시 성능 0.004수준)  
4. -> 모듈 내에 embedding시 out of index error가 발생하는데 class의 매개변수중 (cat_idx,cat_dims,cat_emb_dim)관련 오류로 확인됨  
5. -> 확인 결과 embedding의 input_dim이 batch data의 size로 입력되고 있었음 (예상입력 : cat_dims)  
6. -> idx별 dims을 구분해 줬기 때문에 idx별 dim이 따로 들어가야 하나 고정적으로 batch_size가 들어감  
7. -> 사용자 실수인지 model 오류인지 판별할수 없기 때문에 사용을 포기함  
8. text 데이터의 처리를 위해 방법 강구   
9. -> konlpy 사용, 2글자 이상 명사만 추출  
10. -> kobart tokenizer 사용시 단어별로 기존에 학습된 idx로 변환됨  
11. -> 명사마다 idx변환, 전체 명사의 표준편차를 feature로 생성, text 데이터들은 삭제함   
12. -> 표준편차의 이유는 없음  
13. other method : 명사들을 나눠주어 임베딩, kmeans 또는 dbscan등으로 clustering하여 feature 사용 예정  


### ~ 01/14  
아무리 해도 로컬 내에서의 정확도 : 0.61와, 업로드 내 정확도가 확연히 다름  
이부분은 확실하게 짚고 넘어가야 할듯  

### ~ 01/19  
확인결과 lgb 학습중 클래스가 많다는 오류가 발생하여 target을 label encoding 한 결과가 나쁘게 작용한듯..  
해당 과제에 시간을 투자하는것은 무의미하다고 판단  
작성자의 지식이 부족한듯, 우승자의 코드가 발표되면 토대로 업로드, 공부해보자  

