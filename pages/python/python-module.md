---
title: Python module
tags: [python]
keywords: python
last_updated: March 27, 2019
summary: "How to define and import python modules"
sidebar: mydoc_sidebar
permalink: python-module.html
folder: python
---
Python modules are python source code files.

## Define module
```python
# put the following code into a file called mymodule.py
def func1():
    print("from mymodule->func1()
```

## Import module
A whole module can be imported by:
```python
# you don't need the .py extension when importing a module
import mymodule

module.func1()
```

Or you can import a specific function of a module.
```python
from mymodule import func1

func1()
```

## Builtin modules
[This page](https://docs.python.org/3/py-modindex.html) gives you all the python builtin modules in version 3.
* random - this module generate various random numbers. It can even randomly choose an item from a list or tuple.
* pathlib - object orientated file system library.

## Third-party modules
Third-party modules can be found by python package index [PyPi](https://pypi.org/). 
* openpyxl - Process Microsoft Excel spreadsheet
    ```python
    import openpyxl as xl
    from openpyxl.chart import Reference, BarChart
    
    
    wb = xl.load_workbook('financial-sample.xlsx')
    sheet = wb['Sheet1']
    
    # to get a cell you can use square brackets or the cell() method
    cell_a1 = sheet['a1']
    print(cell_a1.value)
    cell_b2 = sheet.cell(row=2, column=2)
    print(cell_b2.value)
    
    # you can update a cell by
    cell_b2.value = "new name"
    
    # columns and rows
    print(f'The spreadsheet has {sheet.max_row} rows and {sheet.max_column} columns')
    
    for column in sheet.columns:
        print(column[0].value)
    
    for row in sheet.rows:
        print(row[0].value)
    
    # Select a range of values
    values = Reference(sheet, min_row=2, max_row=6, min_col=13, max_col=13)
    
    # draw a bar chart
    chart = BarChart()
    chart.add_data(values)
    sheet.add_chart(chart, 'A705')
    
    wb.save("backup.xlsx")
    ```