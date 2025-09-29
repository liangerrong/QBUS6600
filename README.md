**背景**
本项目为机器学习任务，受某公益组织委托，利用捐赠者的历史捐赠记录等相关数据，预测其lifetime value（Next_24_Month_Value_LTV）。

**关于变量选择的思路（需要讨论）**
建议将*用户层面数据*作为输入变量，而非*单笔交易数据*。因此，需要对部分变量进行聚合处理，
如*捐赠季节*、'ProductType_Group'、'GiftSolicitationChannel'、'CampaignSubtype_Group'、'AppealSeason'等。
可以考虑的方法有：
- 统计特征抽取（众数），
- PCA，
- embedding 等等。

**数据格式**
存储在同一文件夹下的Data中，分别称作：
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
略。得到数据：Processed Data.csv
数据columns：
Index(['SupporterID',                # 标识符（无实际建模意义）
       
       # 连续变量
       'GiftAmount_count',           # 连续变量（捐赠次数，计数型）
       'GiftAmount_mean',            # 连续变量（平均捐赠金额，数值型）
       'GiftAmount_var',             # 连续变量（捐赠金额方差，数值型）
       'Next_24_Month_Value_LTV',    # 连续变量（目标变量，未来24个月LTV）
       'Days_Since_Last_Gift',       # 连续变量（距离上次捐赠天数，数值型）
       'Recent_GiftAmount',          # 连续变量（最近一次捐赠金额，数值型）

       # 二元变量
       'Have_Phone',                 # 二元变量（是否有电话，0/1）
       'Have_Email',                 # 二元变量（是否有邮箱，0/1）
       'Have_EmergencyGift',         # 二元变量（是否有紧急捐赠，0/1）

       # 类别变量
       'Age_Bucket_first',           # 类别变量（年龄分组）
       'Gender_first',               # 类别变量（性别）
       'DOMINANT_MOSAIC_GROUP_first',# 类别变量（MOSAIC主群组）
       'DOMINANT_MOSAIC_TYPE_first', # 类别变量（MOSAIC主类型）
       'State_bucket_first',         # 类别变量（州分组）
       'AppealSeason_mode',          # 类别变量（捐赠季节众数）
       'CampaignSubtype_Group_mode', # 类别变量（活动子类型众数）
       'GiftSolicitationChannel_mode',# 类别变量（募捐渠道众数）
       'ProductType_Group_mode'      # 类别变量（产品类型众数）
      ])

***进度到了这里***

**模型选择**
- 使用带lasso的回归方程
- 二阶段模型，一阶段使用逻辑回归，二阶段为回归方程
- （可选项）查阅同文件夹下的 A Deep Probabilistic Model for Customer Lifetime Value Prediction.md

benchmark为随机森林

**验证效果**
常规方法
- 运行并进行cross-validation，cv=5。
- （可选）其他验证方式


