``` python 
import matplotlib.pyplot as plt  
import seaborn as sns  
import pandas as pd  
import numpy as np  
import openpyxl  
from scipy.stats import pearsonr, t  
from scipy.stats import ttest_ind  
from statannotations.Annotator import Annotator  
from scipy import stats  
import statsmodels.api as sm  
  
#用于进行将列转换为中文  
def change_csv_column_names():  
    #读取csv文件  
    file_path = 'tips.csv'  
    df = pd.read_csv(file_path)  
  
    #定义对应的映射字典  
    column_mapping = {  
        'total_bill':'总金额',  
        'tip' : '小费',  
        'sex' : '性别',  
        'smoker' : '是否吸烟',  
        'day' : '日期',  
        'time' : '时间',  
        'size' : '型号'  
    }  
  
    #重命名列  
    df.rename(columns = column_mapping,inplace = True)  
  
    #打印重命名后的Dataframe  
    print(df)  
  
    #保存修改后的DataFrame到新的csv文件  
    output_file_path = 'tipsz.csv'  
    df.to_csv(output_file_path,index = False,encoding = 'utf-8-sig')  
  
df = pd.read_csv('tipsz.csv')  
df1 = pd.DataFrame(df)  
  
#分析小费和总金额的关系  
def func01():  
    #先对两者进行描述性统计  
    statistics = df1[['总金额','小费']].describe()  
    print(statistics)  
  
    # 绘制出这两列的散点图  
    sns.pairplot(df1[['总金额', '小费']])  
    sns.pairplot(df1, hue="小费")  
    plt.show()  
def func011():  
    #使用jb检验观察总金额是否符合正态分布  
    data = df1.iloc[:, 0]  # 第一列  
    jb_stat, p_value = stats.jarque_bera(data)  
    print(f"JB statistic: {jb_stat}")  
    print(f"P-value: {p_value}")  
    if p_value < 0.05:  
        print("在置信水平95%的情况下，拒绝原假设：数据不符合正态分布")  
    else:  
        print("在置信水平95%的情况下，不拒绝原假设：数据可能符合正态分布")  
    #绘制出qq图  
    fig = sm.qqplot(data)  # import statsmodels.api as sm  
    plt.show()  
def func012():  
    # 使用jb检验观察小费是否符合正态分布  
    data = df1.iloc[:, 1]  # 第一列  
    jb_stat, p_value = stats.jarque_bera(data)  
    print(f"JB statistic: {jb_stat}")  
    print(f"P-value: {p_value}")  
    if p_value < 0.05:  
        print("在置信水平95%的情况下，拒绝原假设：数据不符合正态分布")  
    else:  
        print("在置信水平95%的情况下，不拒绝原假设：数据可能符合正态分布")  
        # 绘制出qq图  
    fig = sm.qqplot(data)  # import statsmodels.api as sm  
    plt.show()  
  
#发现都不符合正态分布，故使用斯皮尔曼相关系数来进行求解  
def func02():  
    x = df1.iloc[:, 0]  # 第一列  
    y = df1.iloc[:, 1]  # 第二列  
  
    # 计算斯皮尔曼相关系数  
    spearman_corr, p_value = stats.spearmanr(x, y)  
    # 打印斯皮尔曼相关系数和P值  
    print(f"Spearman Correlation Coefficient: {spearman_corr}")  
    print(f"P-value: {p_value}")  
  
    #斯皮尔曼系数的原假设是斯皮尔曼系数！=0，刚刚好与皮尔逊系数反过来  
    if p_value < 0.05:  
        print("在置信水平95%的情况下，接受原假设")  
    else:  
        print("在置信水平95%的情况下，拒绝原假设")  
#绘制斯皮尔曼相关系数矩阵的热力图  
def func021():  
    # 计算斯皮尔曼相关系数矩阵  
    correlation_matrix = df[['总金额', '小费']].corr(method='spearman').round(2)  
  
    # 设置图片清晰度  
    plt.rcParams['figure.dpi'] = 300  
  
    # 设置中文字体  
    plt.rcParams['font.sans-serif'] = 'STHupo'  
  
    # 绘制热力图  
    plt.figure(figsize=(9, 6))  
    sns.heatmap(correlation_matrix, annot=True, cmap='PuRd')  
    plt.title('斯皮尔曼相关系数热力图')  
    plt.show()  
  
def func03():  
    #进行独立性t检验  
    # 读取数据集  
    df = pd.read_csv('tipsz.csv')  
  
    # 进行独立性t检验  
    sex_man_tips = df[df['性别'] == 'Male']['小费']  
    sex_woman_tips = df[df['性别'] == 'Female']['小费']  
    stat, p_value = ttest_ind(sex_man_tips, sex_woman_tips, equal_var=False)  
  
    # 输出结果  
    print(p_value)  
  
    # 自定义函数，将p值转换为显著性标记  
    def convert_pvalue_to_asterisks(pvalue):  
        if pvalue <= 0.0001:  
            return "****"  
        elif pvalue <= 0.001:  
            return "***"  
        elif pvalue <= 0.01:  
            return "**"  
        elif pvalue <= 0.05:  
            return "*"  
        return "ns"  
    #绘制图表(柱形图)  
    plt.figure(figsize=(8, 6))  
    ax = sns.barplot(x="性别", y="小费", data=df, ci="sd")  # data数据集必须是csv的，尽量不要用excel  
  
        # 添加显著性标记  
    x1, x2 = 0, 1  # 柱形图的位置  
    y, h = df['小费'].max() + 0.2, 0.2  # 标记的位置  
    plt.plot([x1, x1, x2, x2], [y, y + h, y + h, y], lw=1.5, c="black")  
    plt.text((x1 + x2) * .5, y + h, convert_pvalue_to_asterisks(p_value), ha='center', va='bottom', color="black")  
  
    # 显示图表  
    plt.show()  
# 分析花费和性别的关系，并且依据显著性差异来判断评估出哪一个性别的人的慷慨性强  
def func031():  
    from statannotations.Annotator import Annotator  
    x = "性别"  
    y = "小费"  
    #现在需要确定日期和花费的相关关系  
    order = ['Male','Female']  
    fig, ax = plt.subplots(figsize = (5,4),dpi = 100,facecolor = 'w')  
    #绘制箱线图  
    ax = sns.boxplot(data=df,x=x,y=y,order=order,ax=ax)  
  
    #要比较的组对  
    pairs = [("Male","Female")]  
  
    # 写死的配置  
  
    annotator = Annotator(ax, pairs, data=df, x=x, y=y, order=order)  
    annotator.configure(test='Mann-Whitney', text_format='star', line_height=0.03, line_width=1)  
    annotator.apply_and_annotate()  
  
    # 美化图表  
    ax.tick_params(which='major', direction='in', length=3, width=1., labelsize=14, bottom=False)  
    for spine in ["top", "left", "right"]:  
        ax.spines[spine].set_visible(False)  
    ax.spines['bottom'].set_linewidth(2)  
    ax.grid(axis='y', ls='--', c='gray')  
    ax.set_axisbelow(True)  
  
    plt.show()  
  
#计算日期和时间之间对应的t检验的p值  
#周日和周六  
def func041():  
    # 进行独立性t检验  
    # 读取数据集  
    df = pd.read_csv('tipsz.csv')  
  
    # 进行独立性t检验  
    sex_man_tips = df[df['日期'] == 'Sun']['总金额']  
    sex_woman_tips = df[df['日期'] == 'Sat']['总金额']  
    stat, p_value = ttest_ind(sex_man_tips, sex_woman_tips, equal_var=False)  
  
    # 输出结果  
    print(p_value)  
  
    # 自定义函数，将p值转换为显著性标记  
    def convert_pvalue_to_asterisks(pvalue):  
        if pvalue <= 0.0001:  
            return "****"  
        elif pvalue <= 0.001:  
            return "***"  
        elif pvalue <= 0.01:  
            return "**"  
        elif pvalue <= 0.05:  
            return "*"  
        return "ns"  
  
    # 绘制图表(柱形图)  
    plt.figure(figsize=(8, 6))  
    ax = sns.barplot(x="日期", y="总金额", data=df, ci="sd")  # data数据集必须是csv的，尽量不要用excel  
  
    # 添加显著性标记  
    x1, x2 = 0, 1  # 柱形图的位置  
    y, h = df['总金额'].max() + 0.2, 0.2  # 标记的位置  
    plt.plot([x1, x1, x2, x2], [y, y + h, y + h, y], lw=1.5, c="black")  
    plt.text((x1 + x2) * .5, y + h, convert_pvalue_to_asterisks(p_value), ha='center', va='bottom', color="black")  
  
    # 显示图表  
    plt.show()  
#周日和周五  
def func042():  
    # 进行独立性t检验  
    # 读取数据集  
    df = pd.read_csv('tipsz.csv')  
  
    # 进行独立性t检验  
    sex_man_tips = df[df['日期'] == 'Sun']['总金额']  
    sex_woman_tips = df[df['日期'] == 'Fri']['总金额']  
    stat, p_value = ttest_ind(sex_man_tips, sex_woman_tips, equal_var=False)  
  
    # 输出结果  
    print(p_value)  
  
    # 自定义函数，将p值转换为显著性标记  
    def convert_pvalue_to_asterisks(pvalue):  
        if pvalue <= 0.0001:  
            return "****"  
        elif pvalue <= 0.001:  
            return "***"  
        elif pvalue <= 0.01:  
            return "**"  
        elif pvalue <= 0.05:  
            return "*"  
        return "ns"  
  
    # 绘制图表(柱形图)  
    plt.figure(figsize=(8, 6))  
    ax = sns.barplot(x="日期", y="总金额", data=df, ci="sd")  # data数据集必须是csv的，尽量不要用excel  
  
    # 添加显著性标记  
    x1, x2 = 0, 1  # 柱形图的位置  
    y, h = df['总金额'].max() + 0.2, 0.2  # 标记的位置  
    plt.plot([x1, x1, x2, x2], [y, y + h, y + h, y], lw=1.5, c="black")  
    plt.text((x1 + x2) * .5, y + h, convert_pvalue_to_asterisks(p_value), ha='center', va='bottom', color="black")  
  
    # 显示图表  
    plt.show()  
#周日和周四  
def func043():  
    # 进行独立性t检验  
    # 读取数据集  
    df = pd.read_csv('tipsz.csv')  
  
    # 进行独立性t检验  
    sex_man_tips = df[df['日期'] == 'Sun']['总金额']  
    sex_woman_tips = df[df['日期'] == 'Thur']['总金额']  
    stat, p_value = ttest_ind(sex_man_tips, sex_woman_tips, equal_var=False)  
  
    # 输出结果  
    print(p_value)  
  
    # 自定义函数，将p值转换为显著性标记  
    def convert_pvalue_to_asterisks(pvalue):  
        if pvalue <= 0.0001:  
            return "****"  
        elif pvalue <= 0.001:  
            return "***"  
        elif pvalue <= 0.01:  
            return "**"  
        elif pvalue <= 0.05:  
            return "*"  
        return "ns"  
  
    # 绘制图表(柱形图)  
    plt.figure(figsize=(8, 6))  
    ax = sns.barplot(x="日期", y="总金额", data=df, ci="sd")  # data数据集必须是csv的，尽量不要用excel  
  
    # 添加显著性标记  
    x1, x2 = 0, 1  # 柱形图的位置  
    y, h = df['总金额'].max() + 0.2, 0.2  # 标记的位置  
    plt.plot([x1, x1, x2, x2], [y, y + h, y + h, y], lw=1.5, c="black")  
    plt.text((x1 + x2) * .5, y + h, convert_pvalue_to_asterisks(p_value), ha='center', va='bottom', color="black")  
  
    # 显示图表  
    plt.show()  
#周六和周五  
def func044():  
    # 进行独立性t检验  
    # 读取数据集  
    df = pd.read_csv('tipsz.csv')  
  
    # 进行独立性t检验  
    sex_man_tips = df[df['日期'] == 'Sat']['总金额']  
    sex_woman_tips = df[df['日期'] == 'Fri']['总金额']  
    stat, p_value = ttest_ind(sex_man_tips, sex_woman_tips, equal_var=False)  
  
    # 输出结果  
    print(p_value)  
  
    # 自定义函数，将p值转换为显著性标记  
    def convert_pvalue_to_asterisks(pvalue):  
        if pvalue <= 0.0001:  
            return "****"  
        elif pvalue <= 0.001:  
            return "***"  
        elif pvalue <= 0.01:  
            return "**"  
        elif pvalue <= 0.05:  
            return "*"  
        return "ns"  
  
    # 绘制图表(柱形图)  
    plt.figure(figsize=(8, 6))  
    ax = sns.barplot(x="日期", y="总金额", data=df, ci="sd")  # data数据集必须是csv的，尽量不要用excel  
  
    # 添加显著性标记  
    x1, x2 = 0, 1  # 柱形图的位置  
    y, h = df['总金额'].max() + 0.2, 0.2  # 标记的位置  
    plt.plot([x1, x1, x2, x2], [y, y + h, y + h, y], lw=1.5, c="black")  
    plt.text((x1 + x2) * .5, y + h, convert_pvalue_to_asterisks(p_value), ha='center', va='bottom', color="black")  
  
    # 显示图表  
    plt.show()  
#周六和周四  
def func045():  
    # 进行独立性t检验  
    # 读取数据集  
    df = pd.read_csv('tipsz.csv')  
  
    # 进行独立性t检验  
    sex_man_tips = df[df['日期'] == 'Sat']['总金额']  
    sex_woman_tips = df[df['日期'] == 'Thur']['总金额']  
    stat, p_value = ttest_ind(sex_man_tips, sex_woman_tips, equal_var=False)  
  
    # 输出结果  
    print(p_value)  
  
    # 自定义函数，将p值转换为显著性标记  
    def convert_pvalue_to_asterisks(pvalue):  
        if pvalue <= 0.0001:  
            return "****"  
        elif pvalue <= 0.001:  
            return "***"  
        elif pvalue <= 0.01:  
            return "**"  
        elif pvalue <= 0.05:  
            return "*"  
        return "ns"  
  
    # 绘制图表(柱形图)  
    plt.figure(figsize=(8, 6))  
    ax = sns.barplot(x="日期", y="总金额", data=df, ci="sd")  # data数据集必须是csv的，尽量不要用excel  
  
    # 添加显著性标记  
    x1, x2 = 0, 1  # 柱形图的位置  
    y, h = df['总金额'].max() + 0.2, 0.2  # 标记的位置  
    plt.plot([x1, x1, x2, x2], [y, y + h, y + h, y], lw=1.5, c="black")  
    plt.text((x1 + x2) * .5, y + h, convert_pvalue_to_asterisks(p_value), ha='center', va='bottom', color="black")  
  
    # 显示图表  
    plt.show()  
#周五和周四  
def func046():  
    # 进行独立性t检验  
    # 读取数据集  
    df = pd.read_csv('tipsz.csv')  
  
    # 进行独立性t检验  
    sex_man_tips = df[df['日期'] == 'Fri']['总金额']  
    sex_woman_tips = df[df['日期'] == 'Thur']['总金额']  
    stat, p_value = ttest_ind(sex_man_tips, sex_woman_tips, equal_var=False)  
  
    # 输出结果  
    print(p_value)  
  
    # 自定义函数，将p值转换为显著性标记  
    def convert_pvalue_to_asterisks(pvalue):  
        if pvalue <= 0.0001:  
            return "****"  
        elif pvalue <= 0.001:  
            return "***"  
        elif pvalue <= 0.01:  
            return "**"  
        elif pvalue <= 0.05:  
            return "*"  
        return "ns"  
  
    # 绘制图表(柱形图)  
    plt.figure(figsize=(8, 6))  
    ax = sns.barplot(x="日期", y="总金额", data=df, ci="sd")  # data数据集必须是csv的，尽量不要用excel  
  
    # 添加显著性标记  
    x1, x2 = 0, 1  # 柱形图的位置  
    y, h = df['总金额'].max() + 0.2, 0.2  # 标记的位置  
    plt.plot([x1, x1, x2, x2], [y, y + h, y + h, y], lw=1.5, c="black")  
    plt.text((x1 + x2) * .5, y + h, convert_pvalue_to_asterisks(p_value), ha='center', va='bottom', color="black")  
  
    # 显示图表  
    plt.show()  
# 分析日期和花费的关系  
def func04():  
    x = "日期"  
    y = "总金额"  
    # 现在需要确定日期和花费的相关关系  
    order = ['Sun', 'Thur', 'Fri', 'Sat']  
    fig, ax = plt.subplots(figsize=(5, 4), dpi=100, facecolor="w")  
    # 绘制箱线图  
    ax = sns.boxplot(data=df, x=x, y=y, order=order, ax=ax)  
    # 要比较的组对  
    pairs = [("Thur", "Fri"), ("Thur", "Sat"), ("Fri", "Sun"),("Fri","Sat"),("Thur","Sun"),("Sat","Sun")]  
    # 写死的配置  
  
    annotator = Annotator(ax, pairs, data=df, x=x, y=y, order=order)  
    annotator.configure(test='Mann-Whitney', text_format='star', line_height=0.03, line_width=1)  
    annotator.apply_and_annotate()  
  
    # 美化图表  
    ax.tick_params(which='major', direction='in', length=3, width=1., labelsize=14, bottom=False)  
    for spine in ["top", "left", "right"]:  
        ax.spines[spine].set_visible(False)  
    ax.spines['bottom'].set_linewidth(2)  
    ax.grid(axis='y', ls='--', c='gray')  
    ax.set_axisbelow(True)  
  
    plt.show()  
  
#整理数据，将性别和抽烟的组合数据加入到tipsz.csv  
def clear_data():  
    df['性别'] = df['性别'].replace({'Male': 1, 'Female': 0})  
    df['是否吸烟'] = df["是否吸烟"].replace({'Yes':1,'No':0})  
  
    #添加新列，将性别和抽烟类型进行组合  
    df['吸烟性别综合'] = df['是否吸烟'].astype(str) + '_' + df['性别'].astype(str)  
  
    df.to_csv("tipszz.csv",index = False,encoding = 'utf-8-sig')  
  
#分析性别加上抽烟的组合因素对慷慨度的影响  
def func05():  
    df = pd.read_csv('tipszz.csv')  
    df1 = pd.DataFrame(df)  
    x="吸烟性别综合"  
    y="小费"  
    order = ['0_0','0_1','1_0','1_1']  
  
    fig, ax = plt.subplots(figsize=(5, 4), dpi=100, facecolor="w")  
    # 绘制箱线图  
    ax = sns.boxplot(data=df, x=x, y=y, order=order, ax=ax)  
  
    # 要比较的组对  
    pairs = [("1_0", "0_1"), ("0_1", "1_1"), ("1_0", "1_1"),("1_1","0_0")]  #对应男不吸女吸，女吸男吸，男吸女吸，男吸女不吸  
    # 写死的配置  
  
    annotator = Annotator(ax, pairs, data=df, x=x, y=y, order=order)  
    annotator.configure(test='Mann-Whitney', text_format='star', line_height=0.03, line_width=1)  
    annotator.apply_and_annotate()  
  
    # 美化图表  
    ax.tick_params(which='major', direction='in', length=3, width=1., labelsize=14, bottom=False)  
    for spine in ["top", "left", "right"]:  
        ax.spines[spine].set_visible(False)  
    ax.spines['bottom'].set_linewidth(2)  
    ax.grid(axis='y', ls='--', c='gray')  
    ax.set_axisbelow(True)  
  
    plt.show()  
  
def func06():  
    x = "时间"  
    y = "小费"  
    order = ['Dinner','Lunch']  
    fig, ax = plt.subplots(figsize=(5, 4), dpi=100, facecolor="w")  
    # 绘制箱线图  
    ax = sns.boxplot(data=df, x=x, y=y, order=order, ax=ax)  
  
    # 要比较的组对  
    pairs = [("Dinner","Lunch")]  
    # 写死的配置  
  
    annotator = Annotator(ax, pairs, data=df, x=x, y=y, order=order)  
    annotator.configure(test='Mann-Whitney', text_format='star', line_height=0.03, line_width=1)  
    annotator.apply_and_annotate()  
  
    # 美化图表  
    ax.tick_params(which='major', direction='in', length=3, width=1., labelsize=14, bottom=False)  
    for spine in ["top", "left", "right"]:  
        ax.spines[spine].set_visible(False)  
    ax.spines['bottom'].set_linewidth(2)  
    ax.grid(axis='y', ls='--', c='gray')  
    ax.set_axisbelow(True)  
  
    plt.show()  
  
if __name__ == '__main__':  
    func06()
```
