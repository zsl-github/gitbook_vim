---
title: python操作excel之xlrd
date: 2017-03-15 16:50:03
tags: python
categories:
    - 插件
    - 
password: 
---

# 操作excel文件

```python
hw_File = "C:\\Users\\zwx318792\\Desktop\\xls_test\\huawei.xls"
#获得一个操作工作簿的对象
rb_hw = open_excel(hw_File)
<xlrd.book.Book object at 0x036CC7F0>

#获得工作簿所有的sheet个数
rb_hw.nsheets

#获得所有工作簿名字
rb_hw.sheet_names()
['Basic Info', '51.010-2', '51.010-4', '34.123-2', '34.121-2', '36.521-2', '36.523-2', '31.121', '31.124', '34.171', '37.571-3', '34.229', 'MMS', 'SUPL', 'FUMO', 'DM', 'NFC', 'VT', 'AT', 'ChangeRecord']
```

# 操作sheet表格

```python
#获得一个sheet对象，它是用来操作sheet的
sheet = rb_hw.sheets()[1]
sheet = rb_hw.sheet_by_index(1)
<xlrd.sheet.Sheet object at 0x039158B0>

#sheet名称
sheet.name
'51.010-2'

#sheet行
sheet.nrows

#sheet列
sheet.ncols
```

# 操作行跟列

```python
#行内容
sheet.row(109)
[text:'A.1/105', number:105.0, text:'EGPRS Multislot Class10', text:'3GPP\xa0TS\xa005.02| B.1\n3GPP TS 45.002| B.1', text:'R99', text:'O', text:'yes | no', text:'No', text:'TSPC_Type_EGPRS_Multislot_Class10', empty:'', text:'是', text:'EGPRS', text:'协议']
#得到的是cell对象组成的列表

#列内容
sheet.col(2)
[xt:'A.25.1/17', text:'A.25.1/18', text:'A.25.1/19', text:'A.25.1/20', text:'A.25.1/21', text:'A.25.1/22', text:'A.25.1/23', text:'A.25.1/24', text:'A.25.1/25', text:'A.25.1/26', text:'A.25.1/27', text:'A.25.1/28', text:'A.25.1/29', text:'A.25.1/30', text:'A.25.1/31', text:'A.25.1/32', text:'A.25.1/33', text:'A.25.1/34', text:'A.25.1/35', text:'A.25.1/36', text:'A.25.1/37', text:'A.25.1/38', text:'A.25.1/39', text:'A.25.1/40', text:'A.25.1/41', text:'A.25.1/42', text:'A.25.1/43', text:'A.25.1/44', text:'A.25.1/45', text:'A.25.1/46', empty:'', empty:'', empty:'', empty:'', empty:'', text:'A.27/1', text:'A.27/2', text:'A.27/3', text:'A.27/4', text:'R', text:'R1', text:'R2', text:'R3', text:'R4', text:'R5', text:'R6', text:'R7', text:'R8', text:'R9', text:'R10', text:'R11', text:'R12']
#得到的是cell对象组成的列表

#获得某行（列）某几个单元格的内容
row = sheet.row_slice(0,1,6)
print(row)
[text:'3GPP TS 51.010-2', empty:'', empty:'', empty:'', text:'V12.5.0']
col = sheet.col_slice(3,5,7)
print(col)
[text:'3GPP TS 05.05| 2\n3GPP TS 45.005| 2\n\n', text:'3GPP TS 05.05| 2\n3GPP TS 45.005| 2\n\n']
#得到的是cell对象组成的列表

#我们还可以单独获得行（列表）的某个单元格的值或者type
row = sheet.row_values(0,1,6)
print(row)
['3GPP TS 51.010-2', '', '', '', 'V12.5.0']
row = sheet.row_types(2,3,4)
print(row)
array('B', [0])
col = sheet.col_values(2,3,5)
print(col)
['Type of Mobile Station', 'Feature "A" is used for "applicability" that is referenced in 51.010-2 for many test cases.\r\nYou will find the description in Annex B of this specification.']
col = sheet.col_types(2,3,5)
print(col)

[1, 1]
```

# 操作单元格

```python
#获得一个cell对象，这个对象时用来操作单元格的
cell = sheet.cell(109,7)
print(cell)
text:'No'

#获得单元格的类型
print(cell.ctype)
sheet.cell_type(109,7)

#获得单元格的内容
print(cell.value)
sheet.cell_value(109,7)
'No'

#序列转为单元格命名
cell = cellname(2,2)
print(cell)
C3
cell = cellnameabs(2,2)
print(cell)
$C$3
cell = colname(3)
print(cell)
```
