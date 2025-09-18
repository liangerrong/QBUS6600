**背景**
本项目为机器学习任务，目标是受某公益组织委托，利用捐赠者的历史捐赠记录等相关数据，预测其lifetime value（LTV）。

**关于变量选择的思路（需要讨论）**
建议将*用户层面数据*作为输入变量，而非*单笔交易数据*。因此，需要对部分变量进行聚合处理，
如*捐赠季节*、'ProductType_Group'、'GiftSolicitationChannel'、'CampaignSubtype_Group'、'AppealSeason'等。
可以考虑的方法有：统计特征抽取（众数），PCA，embedding 等等。

**数据格式**
存储在C:\Users\leo\Desktop\QBUS6600\Data中，分别称作：
Mosaic.xlsx
PostcodeData.csv
Project1Data.csv

其中Project1Data.csv的columns含有：
Index(['SupporterID', 'Age_Bucket', 'Gender', 'State', 'PostCode',
       'Have_Phone', 'Have_Email', 'Gift_ID', 'GiftDate', 'IsEmergencyGift',
       'Is_First_Gift', 'ProductType_Group', 'GiftSolicitationChannel',
       'CampaignSubtype_Group', 'AppealSeason', 'GiftAmount',
       'Initial_90_Days', 'Next_24_Month_Value_LTV'],
      dtype='object')

 **数据准备**
- 只保留Initial_90_Days = 1 的行
- Next_24_Month_Value_LTV 进行对数变换
- 填充IsEmergencyGift的缺失值为None，填充其他缺失值为Unknown
- 将GiftDate转换成datetime格式
    获取GiftDate的最大值，计算每个GiftDate与其相隔的天数
    从GiftDate计算礼物的季节，用1,2,3,4表示
- 将State进行预处理
- 将PostCode与PostCodeData进行join
- 聚合上述提及的变量

**模型选择**
- 两阶段模型 https://github.com/rebordao/kdd98cup/blob/master/report.md
- 随机森林或CatBoost
- 深度概率模型 https://arxiv.org/abs/1912.07753

benchmark为随机森林

**验证效果**
常规方法
- 运行并进行cross-validation，cv=5。

深度概率模型 的办法 https://arxiv.org/abs/1912.07753
- 分类任务：AUC / AUC-PR
- 回归任务：Spearman，Normalized Gini Coefficient，Decile Chart 


