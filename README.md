### Online-News-Popularity
#### 預測網路新聞受歡迎程度

#### 1.定義 x , y
1. 解釋變數(X)
- 變數共有60個，其中有58個可預測特徵，2個不可預測特徵
- 移除URL和timedelta這兩個不可預測特徵
- 從相關係數圖可以看出**n_non_stop_unique_tokens**, **n_non_stop_words**, **kw_avg_min**這三個變數高度相關，所以也將這幾個變數移除
- 有離群值需處理

2. 反應變數(Y)
- 類別變數分三類（low, medium, high），分別表示新聞受歡迎程度

#### 2.preprocessing （缺失值/離群值/是否淘汰/合併變數）
- missing value
- 利用 Cox-Box來處理離群值
- - 將非正態分佈資料轉換為近似正態分佈的技術，可以處理資料的偏度和變異量，使資料更加符合模型的假設。
- 類別變數採1-of-C轉換以利分析
- 提取幾個自然語言處理特徵
- 對文本使用 LDA 算法(Latent Dirichlet Allocation)
- - LDA：每個文章都是由數個主題所主成；每個主題可以使用多個重要的words來描述，且相同的字可以出現在不同的主題
- 採用Pattern 網路挖掘模塊以方便計算情感急性(Polarity)的分數

#### 3.模型/呈現方法
1. 模型
- Baseline - Naive bayes
- KNN 
- Logistic Regression
- Random Forest
- gbdt
- xgboost


2. 呈現方法
- ROC曲線（曲線可能閾值：D2 ∈［0, 1]）
- Accuracy 
- Precision 
- Recall 
- F1 score （D2 = 0.5)
- AUC（為最相關的指標，理想為1.0 / 不理想為0.5）
- 滾動窗口分析，將L個舊樣本替換為新樣本

#### 4.分析結果
1. 以xgboost模型預測的結果最佳，AUC達到0.7095
2. 預測出三個對模型影響最重要的三個變數
- self_reference_avg_shares：提及的連結平均分享數。多個引用連結的平均分享數較高，比單一高分享數連結更具說服力。
- kw_avg_avg：一般關鍵字在含此關鍵字新聞中的平均分享數。平均分享數更能反映關鍵字在不同文章中的實際效果。
- LDA_00：以商業為主並涵蓋社交媒體的主題文章。商業新聞涉及實用信息，這些資訊直接影響決策，實用性強，分享率高。
3. 製作儀錶板，以方便觀測不同參數模型的表現


