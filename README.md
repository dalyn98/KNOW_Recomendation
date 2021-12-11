# KNOW_Recomendation (Dacon)

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


