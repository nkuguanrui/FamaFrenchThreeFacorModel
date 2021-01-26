# [论文复现]FamaFrench(1992)三因子模型(持续升级中...ing)
# 项目目的
本项目利用美国股票月度历史数据，利用python严格按照FamaFrench(1992)中的方法构造三因子（市场因子，HML因子，SMB因子），并且按照论文中所用的方法对模型进行时间序列回归检验。在文章写作过程中，本文会把重要的细节的附上原文。
# 三因子构建
## 数据来源
- wrds数据库
- CRSP
- COMPUSTAT 
## 数据处理
- 删除金融公司(We exclude financial firms because the high leverage that is normal for these firms probably does not have the same meaning as for nonfinancial firms, where high leverage more likely indicates distress.)
- 为了确保财报发布，将t-1年的财务信息与t年7月到t+1年6月的收益率进行匹配,也就是说，将上市公司年报发布时间统一设定在下一年的6月。(To ensure that the accounting variables are known before the returns they are used to explain, we match the accounting data for all fiscal yearends in calendar year t - 1 (1962-1989) with the returns for July of year t to June of t + 1.)
## 因子构建
1. t年的6月，以纽交所股票截面数据，计算市值的50%分位点和BM(book-to-markert ratio)的30%和70%分位点。
    - 计算BM分位点史去掉BM小于0的股票(We do not use negative-BE firms, when calculating the breakpoints for BM or when forming the size-BM portfolios.)
    - 市值(size)选择t年6月的数据;BM是用t-1年财年末的账面价值除以t-1年末的市值
2. 对纽交所，纳斯达克，美交所股票做2x3独立双重排序(independent double sorting),分成6组，计算每组t年7月到t+1年6月的月度市值加权收益率。t+1年6月重新构造2x3组合。
3. 构建HML和SMB因子:
    - $SMB = (SH+SM+SL)/3 - (BH+BM+BL)/3 $
    - $HML= (SH+BH)/2 - (SL+BL)/2$
4. 市场因子：RM-RF
    - RM：全部股票月度市值加权收益率
    - RF：一个月国债收益率

## 因子检验
FamaFrench(1992)利用时间序列回归，估计定价误差，对三因子模型进行检验。
1. 构造25个投资组合，构造方法和因子构建中的方法一致，唯一的差别是做5x5独立双重排序。
2. 计算25个组合的月度加权收益率，计算超额收益
3. 以25个组合的超额收益为因变量，以三因子为自变量，进行时间序列回归，股票定价误差和$\beta$
4. 对定价误差进行$\alpha$检验，如果定价误差不显著不等于0，则多因子模型有效
5. 对$\beta$进行检验，发现$\beta$显著不等于0，说明因子对25个投资组合的超额收益率有显著的解释能力。
## 数据和代码
详见项目


