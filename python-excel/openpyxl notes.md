openpyxl notes
=

##	1. 安装openpyxl模块
> `pip install openpyxl`
> 
##	2. 打开表格文件
> `wb=openpyxl.load_workbook(r'path\exercise.xlsx')`
> 
##	3. 打开sheet
>	a. 打开已有的sheet
>	
> `sheet1=wb.get_sheet_by_name('Sheet1')`    不推荐<br>
> `sheet1=wb['Sheet1']`                      推荐
> 
> b. 打开活动sheet
> 
> `ws=wb.active`
> 
> c. 创建新sheet
> 
> `mySheet=wb.create_sheet('mySheet')`
> 
##	4. 获取单元格（以下方式获取到的都是单元格object，而不是单元格内的信息）
>	a. 直接定位到具体单元格
>	
> `cell=ws['A1']`
> 
> b. 通过指定行列数
> 
> `cell=ws.cell(row=1, column=2)`
> 
>	c. 取出一行数据
>	
> `row6=ws[6]`
> 
>	d. 取出一列数据
>	
> `colC=ws['C']`
> 
>	e. 取出连续多行数据
>	
> `row_range=ws[2:6]`
> 
>	f. 取出连续多列数据
>	
> `col_range=ws['B:E']`
> 
>	g. 通过循环方式获取到列中的每一个单元格
>	
>     for col in col_range:
>         for cell in col:
>             info=cell.value
>                 
>	h. 通过参数限制读取单元格的范围，可以省略min或max，也可都加
>	
>     for row in ws.iter_rows(min_row=1, max_row=2, max_col=2):
>         for cell in row:
>             info=cell.value
>               
>	i. 通过给定读取范围，这样也是先读行再读cell
>	
>     cell_range=ws['A1:C3']
>     for row in cell_range:
>         for cell in row:
>             info=cell.value
>              
>	j. 指定列，获取不同行信息
>	
>     for row in range(2,ws.max_row+1):
>         info=ws['B'+str(row)].value
>         
## 5. 获取单元格信息
> a. 行
> 
> `cell.row`
> 
>	b. 列
>	
> `cell.column`
> 
>	c. 内容（这种方式取出都为string）(补充：前面描述不准确，取出的内容以其在sheet中的格式为准，可以是int，float，string甚至是time
>	
> `cell.value`
> 
>	d. 坐标（直接输出A1）
>	
> `cell.coordinate`
> 
>	e. 表格最大行数
>	
> `max_row=ws.max_row`
> 
>	f. 表格最大列数
>	
> `max_col=ws.max_column`
> 
>	g. 使用内建util将列号在字母与数字进行转换
>	
> `from openpyxl.utils import get_column_letter, column_index_from_string`
>> i. 数字转字母
>>			
>> `row_letter=get_column_letter(274)`
>> 
>> ii. 字母转数字
>> 
>> `row_number=column_index_from_string('AAH')`
>> 
##	6. 对表格进行修改
>	a. 创建新表格文件
>	
> `wb=openpyxl.Workbook()`
> 
>	b. 修改sheet名
>	
>> i. 获取当前sheet名
>> 
>> `sheet_name=ws.title`
>> 
>> ii. 修改sheet名
>> 
>> `ws.title='Happy'`
>> 
>> iii. 获取所有sheet名
>> 
>> `wb.get_sheet_names()`
>> 
>	c. 在指定位置创建新sheet，可以省略index
>	
> `wb.create_sheet(index=1, title='Another Sheet')`
> 
>	d. 删除sheet
>	
> `wb.remove_sheet(wb.get_sheet_by_name('Another Sheet'))`
> 
>	e. 保存文件
>	
> `wb.save('test.xlsx')`
> 
>	f. 修改单元格内容
>	
> `ws['A1']='Hello python'`
> 
>	g. 向sheet中追加内容（按行添加）
>> i. 添加自动生成的序数
>> 
>>     for row in range(1,40):
>>         ws.append(range(17))
>>         
>> ii. 添加编辑好的内容
>> 
>>      rows = [
>>        ['Number', 'Batch 1', 'Batch2'],
>>        [2, 40, 30],
>>        [3, 40, 25],
>>        [4, 50, 30],
>>        [5, 30, 10],
>>        [6, 40, 30],
>>        [7, 78, 52],
>>      ]
>>	  
>>      for row in rows:
>>          ws.append(row)
>>        
>> iii. 向单元格添加内容
>> 
>>      for row in range(5, 30):
>>          for col in range(15, 54):
>>              ws.cell(column=col, row=row, value=get_column_letter(col))
>>              
>	h. 向表格中插入列，这里只能用数字，若需要使用字母，可使用`column_index_from_string`方法进行转换
>	
> `ws.insert_cols(1)`
