---
title: OpenPyXL - Python Excel处理
description: 最好用的Python Excel处理库，没有之一
categories:
    - Python
tags:
    - Python三方库
---

Python的Excel处理库

- 常规读写操作
- 支持表格样式
- 支持Excel统计图
- 插入图片。

## 基础操作

### 1. 创建一个新的文件

```python
from openpyxl import WorkBook

# 创建一个Excel类
wb = WorkBook()
# Excel操作 ...
# 保存Excel
wb.save('out.xlsx')
```

### 2. 读取已存在的文件

```python
from openpyxl import load_workbook

# 加载现有的Excel
wb = load_workbook('test.xlsx')
# Excel操作 ...
# 保存Excel
wb.save('out.xlsx')
```

### 4. WorkBook与Sheet

```python
from openpyxl import load_workbook

# 加载现有的excel
wb = load_workbook('test.xlsx')
# 获取当前激活的sheet
ws = wb.active
# 获取指定sheet
ws = wb['sheet_name']
# 获取所有sheet
wb.worksheets # sheet对象列表
wb.sheetnames # sheetname字符串列表
# 创建一个sheet
wb.create_sheet(title, index) # 可选sheet名，可选sheet索引位置
# 移动一个sheet
wb.move_sheet(sheet, offset) # sheet对象或者sheet名，移动的相对距离
# 删除一个sheet
wb.remove_worksheet(sheet) # sheet对象
del wb[sheetname] # sheet名
```

### 5. Row、Column与Cell

Sheet中单元格操作

```python
# 获取sheet名
ws.title
# 查看数据行
ws.max_row
# 查看数据列
ws.max_column
# 获取Row对象
# 获取Row迭代器
ws.rows
# 获取Column迭代器
ws.columns
# 获取Column对象
# 获取Cell对象
ws.cell(row_number, column_number) # 行号，列号
ws[row_number][column_number] # 行号，列号
# 修改Cell的值
cell = ws[1][1]
cell.value = '123123'
# 插入行

# 插入列

# 删除行

# 删除列
```

## 单元格样式

## 图片操作

## 统计图