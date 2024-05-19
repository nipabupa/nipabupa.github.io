---
title: OpenPyXL - Python Excel处理
description: 最好用的Python Excel处理库，没有之一
categories:
    - Python
tags:
    - Python三方库
    - Excel处理
---

{% link https://openpyxl.readthedocs.io/en/stable/ OpenPyXL - Python Excel处理 icon:https://openpyxl.readthedocs.io/favicon.ico desc:false %}

## 简介

Python的Excel处理库：
{% checkbox checked:true sheet页签操作 %}
{% checkbox checked:true 行、列、单元格操作 %}
{% checkbox checked:true 支持表格样式 %}
{% checkbox checked:true 支持插入图片 %}
{% checkbox symbol:minus color:yellow checked:true 统计图：见官方文档 %}
{% checkbox symbol:minus color:yellow checked:true 排序与筛选：见官方文档 %}
{% checkbox symbol:minus color:yellow checked:true 数据透视表：见官方文档 %}
{% checkbox symbol:minus color:yellow checked:true 注释：见官方文档 %}
{% checkbox symbol:minus color:yellow checked:true 公式：见官方文档 %}

## 安装

{% copy pip install openpyxl prefix:$ %}

如何要在表格中添加图片，还需要安装`pillow`

{% copy pip install pillow prefix:$ %}

## 文件操作

### 1. 创建一个新的文件

```python
from openpyxl import Workbook

# 创建一个Excel类
wb = Workbook()
# Excel操作
# ...
# 保存Excel
wb.save('out.xlsx')
```

### 2. 读取已存在的文件

```python
from openpyxl import load_workbook

# 加载现有的Excel
wb = load_workbook('test.xlsx')
# Excel操作
# ...
# 保存Excel
wb.save('out.xlsx')
```

### 3. 表格对象 - `Workbook`

```python
from openpyxl import load_workbook

# 加载现有的excel
wb = load_workbook('path_to_excel.xlsx')
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
### 4. 页签对象 - `Worksheet`

#### 基础信息

```python
from openpyxl import load_workbook

wb = load_workbook('path_to_excel.xlsx')
# 获取当前sheet
ws = wb.active

# 获取sheet名
ws.title
# 查看当前数据行数
ws.max_row
# 查看当前数据列数
ws.max_column
# 获取Row迭代器
ws.rows
# 获取Column迭代器
ws.columns
```

#### 行列操作

```python
# 插入行
ws.insert_rows(7, 5)  # 在当前第7行前插入5行
# 插入列  
ws.insert_rows(7, 5)  # 在当前第7列前插入5列 
# 删除行
ws.delete_rows(7, 5)  # 从第7行开始删除5行
# 删除列
ws.delete_cols(7, 5)  # 从第7列开始删除5列
# 合并单元格
ws.merge_cells('A2:D2')
ws.unmerge_cells('A2:D2')
ws.merge_cells(start_row=2, start_column=1, end_row=4, end_column=4)
ws.unmerge_cells(start_row=2, start_column=1, end_row=4, end_column=4)
# 移动单元格
ws.move_range("D4:F10", rows=-1, cols=2)  # 将D4:F10区域的单元格上移1行，右移2列
```

{% note 注意 OpenPyXL行列操作不管理依赖项，包括公式、图表等在行列变化后不会自动更新 color:red %}

#### 单元格操作

这里指在Sheet对象中对单元格进行操作

```python
# 获取Cell对象
cell = ws.cell(row_number, column_number)  # 行号，列号
cell = ws[row_number][column_number] # 行号，列号
# 修改Cell的值
cell.value = '123123'
```

## 样式

### 默认样式

```python
from openpyxl.styles import PatternFill, Border, Side, Alignment, Protection, Font
# 字体配置
font = Font(name='Calibri',
            size=11,
            bold=False,
            italic=False,
            vertAlign=None,
            underline='none',
            strike=False,
            color='FF000000')
# 填充配置
fill = PatternFill(fill_type=None,
                   start_color='FFFFFFFF',
                   end_color='FF000000')
# 边框配置
border = Border(left=Side(border_style=None, color='FF000000'),
                right=Side(border_style=None, color='FF000000'),
                top=Side(border_style=None, color='FF000000'),
                bottom=Side(border_style=None, color='FF000000'),
                diagonal=Side(border_style=None, color='FF000000'),
                diagonal_direction=0,
                outline=Side(border_style=None, color='FF000000'),
                vertical=Side(border_style=None, color='FF000000'),
                horizontal=Side(border_style=None, color='FF000000'))
# 对齐方式
alignment=Alignment(horizontal='general',
                    vertical='bottom',
                    text_rotation=0,
                    wrap_text=False,
                    shrink_to_fit=False,
                    indent=0)
# 数字格式
number_format = 'General'
# 保护格式
protection = Protection(locked=True, hidden=False)
```

### 样式复制

适用于奎快速创建多个仅仅部分参数不同的样式

```python
>>> from openpyxl.styles import Font
>>> from copy import copy
>>>
>>> ft1 = Font(name='Arial', size=14)
>>> ft2 = copy(ft1)
>>> ft2.name = "Tahoma"
>>> ft1.name
'Arial'
>>> ft2.name
'Tahoma'
>>> ft2.size
14.0
```

### 样式设置

```python
# 获取Cell对象
cell = ws[row_number][column_number]
# 设置字体
cell.font = my_font
# 设置填充
cell.fill = my_fill
# 设置边框
cell.board = my_board   
# 设置对齐
cell.alignment = my_alignment
# 设置数字格式
cell.number_format = '0.00'
```

{% note 注意 对于合并的单元格，只需要设置左上角单元格样式即可应用整个合并区域 color:red %}

### 命名样式

命名样式就是预置好的单元格样式组合

```python
# 创建命名样式
from openpyxl.styles import NamedStyle, Font, Border, Side
highlight = NamedStyle(name="highlight")
highlight.font = Font(bold=True, size=20)
bd = Side(style='thick', color="000000")
highlight.border = Border(left=bd, top=bd, right=bd, bottom=bd)

# 向Workbook对象添加命名样式
wb.add_named_style(highlight)
# 如果直接设置会自动像Workbook对象添加
ws['A1'].style = highlight
# 可以通过name字符串来设置
ws['D5'].style = 'highlight'
```

## 插入图片

```python
ws['A1'] = '图片锚点'

# 创建一个图片
img = Image('logo.png')

# 添加图片
ws.add_image(img, 'A1')
```