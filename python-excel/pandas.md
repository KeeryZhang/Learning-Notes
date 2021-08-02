pandas
=

## 1. 使用Pandas模块进行排序
>	a. 安装pandas
>	
> `pip install pandas`
> 
>	b. 排序示例
>	
>     import pandas as pd
>     data_test = pd.read_excel('sort_file.xlsx')
>     df = pd.DataFrame(data_test)

>     以列“Fruit”的标签列来进行升序排列
>     df_1 = df.sort_values('Fruit',ascending=True)

>     以Fruit中的值进行降序排列，如果有相同值，再以Price进行降序排列
>     df_2 = df.sort_values(['Fruit','Price'],ascending=False)
>     

>     先以Fruit进行降序排列，再以Price进行升序排列
>     df_3 = df.sort_values(['Fruit','Price'],ascending=[False,True])
		
[最详细的Excel模块Openpyxl教程（四）-过滤和排序操作 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/347537056 "https://zhuanlan.zhihu.com/p/347537056")
    
>	c. 完整打开排序保存流程
>	
>     import pandas as pd
>     data_test = pd.read_excel('sort_file.xlsx')
>     df = pd.DataFrame(data_test)
>     # 以列“Fruit”的标签列来进行升序排列
>     df_1 = df.sort_values('Fruit',ascending=True)
>     writer =  pd.ExcelWriter('sort_file.xlsx')
>     df_1.to_excel(writer,sheet_name = 'Sheet1',index=False)
>     writer.save()
