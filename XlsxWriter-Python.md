# Creating Excel files with Python and XlsxWriter #

## Release 0.7.3 ##

# 1.介绍 #

**XlsxWriter** 是处理生成Excel2007+XLSX文件格式的Python模块。

它常用于写文本，数值和多个工作表公式并且支持一下格式：图片，图表，页面设置，自动过滤，条件格式和许多其他格式。

**XlsxWriter**的优缺点：



- 优点： 

	1.相比其他任何模块，支持更多的excel功能
	
	2.它拥有一个与excel所产生的高程度的保真度。在大多数情况下的文件产生100%相当于excel文件所产生。
	
	3.它有广泛的文档，例如文件和测试
	
	4.处理速度很快，即使对于非常大的输出文件也可以被配置非常小的内存

- 缺点：

	1.无法读取或修改现有的Excel XLSX文件
	

# 2.使用 XlsxWriter #

## 2.1 安装 XlsxWriter ##

**首先安装 XlsxWriter模块，方法多种！**

### 2.1.1 使用pip ###

	$ sudo pip install XlsxWriter
	
	注：windows用户不能使用sudo
	
### 2.1.2 使用easy_install ###

如果pip不能安装，请尝试easy_install

	$ sudo easy_install XlsxWriter
	

### 2.1.3 使用tarball安装 ###

如果你下载了最新版的XlsxWriter的tarball，你可以按照下面安装：

	$ tar -zxvf XlsxWriter-1.2.3.tar.gz
	$ cd XlsxWriter-1.2.3
	$ sudo python setup.py install
	
最新版的tarball可以在github下载：

	$ curl -O -L http://github.com/jmcnamara/XlsxWriter/archive/master.tar.gz
	$ tar zxvf master.tar.gz
	$ cd XlsxWriter-master/
	$ sudo python setup.py install
	
### 2.1.4 克隆github ###

    $ git clone https://github.com/jmcnamara/XlsxWriter.git
    $ cd XlsxWriter
    $ sudo python setup.py install
    
## 2.2 运行一个简单的程序 ##

如果你的正确安装模块们可以创建一个小的程序来确认模块工作是否正常：

	import xlsxwriter
    workbook = xlsxwriter.Workbook('hello.xlsx')
    worksheet = workbook.add_worksheet()
    worksheet.write('A1', 'Hello world')
    workbook.close()
    
    保存文件为hello.py，并运行：
    
    $ python hello.py

这时将会输出一个名为hello.xlsx的文件：

![](http://i.imgur.com/jFtFGDP.png)

注：如果你下载的是tarball或者克隆的repo，按照上述操作，你应该有一个examples目录和一些简单的应用来表明XlsxWriter不同的特点。

## 2.3 文档 ##

最新的文档可以参考这里：[https://xlsxwriter.readthedocs.org/contents.html](https://xlsxwriter.readthedocs.org/contents.html "最新文档")。


# 3.实战 #

## 教程1：创建一个简单的xlsx文件 ##

让我们用Python和 XlsxWriter模块创建一个简单的电子表格。

以下是每月的开支数据：

	expenses = (
    	['Rent', 1000],
    	['Gas', 100],
    	['Food', 300],
    	['Gym', 50],
    )
    
开始以简单的小程序：

    import xlsxwriter
    # Create a workbook and add a worksheet.
    workbook = xlsxwriter.Workbook('Expenses01.xlsx')
    worksheet = workbook.add_worksheet()
    # Some data we want to write to the worksheet.
    expenses = (
    	['Rent', 1000],
    	['Gas', 100],
    	['Food', 300],
    	['Gym', 50],
    )
    # Start from the first cell. Rows and columns are zero indexed.
    row = 0
    col = 0
    # Iterate over the data and write it out row by row.
    for item, cost in (expenses):
    	worksheet.write(row, col, item)
    	worksheet.write(row, col + 1, cost)
    	row += 1
    # Write a total using a formula.
    worksheet.write(row, 0, 'Total')
    worksheet.write(row, 1, '=SUM(B1:B4)')
    workbook.close()

如果你运行程序会得到如下表格：

![](http://i.imgur.com/mWy85v5.png)

这是一个简单的例子，但所涉及的步骤是代表所有程序使用 XlsxWriter，所以让我们把它分解成独立的部分。

首先，导入对应的模块：

	import xlsxwriter
	
其次，使用Workbook()创建一个workbook对象。Workbook()是一个空的操作，参数是我们想创建的文件名：

	workbook = xlsxwriter.Workbook('Expenses01.xlsx')
	
**提示**：XlsxWriter仅创建新文件，不能读或修改存在的文件。

workbook对象通过add_worksheet()方法添加一个新的工作表：

	worksheet = workbook.add_worksheet()
	
工作表名称默认为：sheet1，sheet2，等等，当然我们也可以指定名称：

	worksheet1 = workbook.add_worksheet() # 默认sheet1
	worksheet2 = workbook.add_worksheet('Data') # Data.
	worksheet3 = workbook.add_worksheet() # 默认sheet3
	
然后，我们用write()方法写入数据：

	worksheet.write(row, col, some_data)
	
	提示：xlsxwriter，行和列的零索引，工作表中的第一个单元格A1是(0, 0)
	
在上述例子中，重申一下数据：

	for item, cost in (expenses):
		worksheet.write(row, col, item)
		worksheet.write(row, col + 1, cost)
		row += 1
		
然后，我们在第二列添加一个计算公式：

	worksheet.write(row, 1, '=SUM(B1:B4)')
	
最后，通过close()方法关闭excel文件：

	
	workbook.close()
	
当超出范围或是程序不在被引用时，XlsxWriter文件是隐式关闭的，和大多数的Python文件对象是类似的。这条是可选的，除非你关闭文件。
这就是，我们现在有一个文件，可以读取通过电子表格和其他电子表格应用程序。

在接下来的章节中我们将看到我们如何使用xlsxwriter模块添加格式等特点。


## 教程2：添加格式到xlsx文件 ##

将所需的数据转换成一个文件，但它看起来缺点什么，为使信息更清晰，我们可以添加一些格式来修饰：

![](http://i.imgur.com/K5p9TJE.png)

这里的差异是，我们已经增加了项目和成本列标题中的一个加粗字体，我们在第二栏里已经格式化了货币，并且做了汇总（Total）的加粗字。

按照以下操作来扩展我们的程序：

    import xlsxwriter
    # Create a workbook and add a worksheet.
    workbook = xlsxwriter.Workbook('Expenses02.xlsx')
    worksheet = workbook.add_worksheet()
    # Add a bold format to use to highlight cells.
    bold = workbook.add_format({'bold': True})
    # Add a number format for cells with money.
    money = workbook.add_format({'num_format': '$#,##0'})
    # Write some data headers.
    worksheet.write('A1', 'Item', bold)
    worksheet.write('B1', 'Cost', bold)
    # Some data we want to write to the worksheet.
    expenses = (
    ['Rent', 1000],
    ['Gas', 100],
    ['Food', 300],
    ['Gym', 50],
    )
    # Start from the first cell below the headers.
    row = 1
    col = 0
    # Iterate over the data and write it out row by row.
    for item, cost in (expenses):
    worksheet.write(row, col, item)
    worksheet.write(row, col + 1, cost, money)
    row += 1
    # Write a total using a formula.
    worksheet.write(row, 0, 'Total', bold)
    worksheet.write(row, 1, '=SUM(B2:B5)', money)
    workbook.close()
    
相比之前的程序，主要不同是我们添加了格式化对象，因此我们可以使用格式化单元格在单子表单。

格式对象表示可以应用于一个单元格中的所有格式属性，例如字体，数字格式，颜色和边框。详细解释参考这里：[The Format Class](https://xlsxwriter.readthedocs.org/format.html#format)


现在我们将避免进入细节，仅使用一个有限的格式函数来添加一些简单的格式：

	# Add a bold format to use to highlight cells.
	bold = workbook.add_format({'bold': True})
	# Add a number format for cells with money.
	money = workbook.add_format({'num_format': '$#,##0'})
	
然后，将这些格式操作作为worksheet.write()方法的一个参数：

	write(row, column, token, [format])
	如：
	worksheet.write(row, 0, 'Total', bold)

添加其他表头的方式同样如此：

	worksheet.write('A1', 'Item', bold)
	worksheet.write('B1', 'Cost', bold)
	
因此，使用excel的A1样式替代（row， col）。更多详细信息：https://xlsxwriter.readthedocs.org/working_with_cell_notation.html#cell-notation

## 教程3：写入不同的数据类型到xlsx文件 ##

这次我们扩展数据如下：

	expenses = (
		['Rent', '2013-01-13', 1000],
		['Gas', '2013-01-14', 100],
		['Food', '2013-01-16', 300],
		['Gym', '2013-01-20', 50],
	)
	
相应的电子表格如下：

![](http://i.imgur.com/6AVtwTB.png)

扩展程序如下：

    from datetime import datetime
    import xlsxwriter
    # Create a workbook and add a worksheet.
    workbook = xlsxwriter.Workbook('Expenses03.xlsx')
    worksheet = workbook.add_worksheet()
    # Add a bold format to use to highlight cells.
    bold = workbook.add_format({'bold': 1})
    # Add a number format for cells with money.
    money_format = workbook.add_format({'num_format': '$#,##0'})
    # Add an Excel date format.
    date_format = workbook.add_format({'num_format': 'mmmm d yyyy'})
    # Adjust the column width.
    worksheet.set_column(1, 1, 15)
    # Write some data headers.
    worksheet.write('A1', 'Item', bold)
    worksheet.write('B1', 'Date', bold)
    worksheet.write('C1', 'Cost', bold)
    # Some data we want to write to the worksheet.
    expenses = (
    ['Rent', '2013-01-13', 1000],
    ['Gas', '2013-01-14', 100],
    ['Food', '2013-01-16', 300],
    ['Gym', '2013-01-20', 50],
    )
    # Start from the first cell below the headers.
    row = 1
    col = 0
    for item, date_str, cost in (expenses):
    # Convert the date string into a datetime object.
    date = datetime.strptime(date_str, "%Y-%m-%d")
    worksheet.write_string (row, col, item )
    worksheet.write_datetime(row, col + 1, date, date_format )
    worksheet.write_number (row, col + 2, cost, money_format)
    row += 1
    # Write a total using a formula.
    worksheet.write(row, 0, 'Total', bold)
    worksheet.write(row, 2, '=SUM(C2:C5)', money_format)
    workbook.close()
    
此程序主要添加了一个新的格式对象（date）和额外处理数据的类型方法。

Excel处理不同类型的输入数据，如字符串和数字，虽然它是不同的，但一般都是对用户显示。xlsxwriter试图通过Python的映射来效仿这一工作表中的write()方法获得Excel的支持。

write()的具体方法实现如下：

    • write_string()
    • write_number()
    • write_blank()
    • write_formula()
    • write_datetime()
    • write_boolean()
    • write_url()
   
此版本，我们通过具体的write_方法来实现数据类型：

    worksheet.write_string (row, col, item )
    worksheet.write_datetime(row, col + 1, date, date_format )
    worksheet.write_number (row, col + 2, cost, money_format)

这主要是为了表明，如果你需要更多的控制类型的数据到你的工作表，你可以用适当的方法。使用简化的write()方法，同样可以工作的很好。

日期的处理也是新加入的程序。

在Excel中的日期和时间都是浮点数，有一个数字格式适用于显示它们以正确的格式。如果日期和时间是Python的datetime对象，xlsxwriter使所需数值自动转换。但是，我们也需要添加数字格式来确保Excel正确显示信息：

    from datetime import datetime
    ...
    
    date_format = workbook.add_format({'num_format': 'mmmm d yyyy'})
    ...
    
    for item, date_str, cost in (expenses):
    # Convert the date string into a datetime object.
    date = datetime.strptime(date_str, "%Y-%m-%d")
    ...
    worksheet.write_datetime(row, col + 1, date, date_format )
    ...
    
日期处理详细信息如下：https://xlsxwriter.readthedocs.org/working_with_dates_and_time.html#working-with-dates-and-time

方法最后添加一个set_column()方法仅仅是为了使列‘B’的宽度更加清晰可见：

    # Adjust the column width.
    worksheet.set_column('B:B', 15)
    


# 实战案例：#
## （某师兄提供） ##

    #!/usr/bin/env python2.7
    #-*- coding: utf-8 -*-
    import xlsxwriter
    import os,sys,time
    import xlrd
    #################python xlsxwriter模块生成excel图表#########################
    #data = xlrd.open_workbook(fname)  #打开fname文件
    #data.sheet_names()#获取chart_scatter.xlsx文件中所有sheet列的名称
    #table = data.sheet_by_index(0)#通过索引获取xls文件第0个sheet
    #ncols = table.ncols   #获取table工作表总列数
    times_shijian = os.popen("date -d '-1 day' '+%Y-%m-%d'").read().strip()
    workbook = xlsxwriter.Workbook('/root/chart_scatter.xlsx') #创建一个excel文件
    worksheet = workbook.add_worksheet()   #创建一个工作表对象
    #worksheet.set_column(0,ncols,15)   #设置工作表列的宽度
    worksheet.set_column('A:A', 16)
    bold = workbook.add_format({'bold': 1,'color': 'red'}) #为bold对象字体加粗,颜色为红色!
    bold.set_align('center') #设置bold对象为居中!
    2015-09-23 10:20秋季全国高教仪器设备展示会开始啦！Q7pMTXJc1lRiaDk6tgCJf9ibvE8ia1xK9vaejmSIFSDck9DbedWiczMNAdQ/0?wx_fmt=jpeg�合作企业慕名而来，让老师们愿意坐下来去体验。同和赞赏。#39;,'00:25','00:30','00:35','00:40','00:45','00:50','00:55','01:00','01:05','01:10','01:15','01:20','01:25','01:30','01:35','01:40','01:45','01:50','01:55','02:00','02:05','02:10','02:15','02:20','02:25','02:30','02:35','02:40','02:45','02:50','02:55','03:00','03:05','03:10','03:15','03:20','03:25','03:30','03:35','03:40','03:45','03:50','03:55','04:00','04:05','04:10','04:15','04:20','04:25','04:30','04:35','04:40','04:45','04:50','04:55','05:00','05:05','05:10','05:15','05:20','05:25','05:30','05:35','05:40','05:45','05:50','05:55','06:00','06:05','06:10','06:15','06:20','06:25','06:30','06:35','06:40','06:45','06:50','06:55','07:00','07:05','07:10','07:15','07:20','07:25','07:30','07:35','07:40','07:45','07:50','07:55','08:00','08:05','08:10','08:15','08:20','08:25','08:30','08:35','08:40','08:45','08:50','08:55','09:00','09:05','09:10','09:15','09:20','09:25','09:30','09:35','09:40','09:45','09:50','09:55','10:00','10:05','10:10','10:15','10:20','10:25','10:30','10:35','10:40','10:45','10:50','10:55','11:00','11:05','11:10','11:15','11:20','11:25','11:30','11:35','11:40','11:45','11:50','11:55','12:00','12:05','12:10','12:15','12:20','12:25','12:30','12:35','12:40','12:45','12:50','12:55','13:30','13:05','13:10','13:15','13:20','13:25','13:30','13:35','13:40','13:45','13:50','13:55','14:00','14:05','14:10','14:15','14:20','14:25','14:30','14:35','14:40','14:45','14:50','14:55','15:00','15:05','15:10','15:15','15:20','15:25','15:30','15:35','15:40','15:45','15:50','15:55','16:00','16:05','16:10','16:15','16:20','16:25','16:30','16:35','16:40','16:45','16:50','16:55','17:00','17:05','17:10','17:15','17:20','17:25','17:30','17:35','17:40','17:45','17:50','17:55','18:00','18:05','18:10','18:15','18:20','18:25','18:30','18:35','18:40','18:45','18:50','18:55','19:00','19:05','19:10','19:15','19:20','19:25','19:30','19:35','19:40','19:45','19:50','19:55','20:00','20:05','20:10','20:15','20:20','20:25','20:30','20:35','20:40','20:45','20:50','20:55','21:00','21:05','21:10','21:15','21:20','21:25','21:30','21:35','21:40','21:45','21:50','21:55','22:00','22:05','22:10','22:15','22:20','22:25','22:30','22:35','22:40','22:45','22:50','22:55','23:00','23:05','23:10','23:15','23:20','23:25','23:30','23:35','23:40','23:45','23:50','23:55']
    datab = []
    fb = open('/root/line.txt','r')
    for eachline in fb:
    b = eachline.replace("'","")
    c=eval(b)
    datab.append(c)
    formats=workbook.add_format()  #创建一个工作表对象为formats.
    formats.set_align('center')#formats对象居中对齐.
    worksheet.write_row('A1', headings, bold)  #引用定义的headings和bold对象类型.
    worksheet.write_row('A2', datab[0],formats)#获取数据第一列和引用formats对象类型.
    worksheet.write_row('A3', datab[1],formats)
    worksheet.write_row('A4', datab[2],formats)
    worksheet.write_row('A5', datab[3],formats)
    worksheet.write_row('A6', datab[4],formats)
    worksheet.write_row('A7', datab[5],formats)
    worksheet.write_row('A8', datab[6],formats)
    worksheet.write_row('A9', datab[7],formats)
    worksheet.write_row('A10', datab[8],formats)
    worksheet.write_row('A11', datab[9],formats)
    worksheet.write_row('A12', datab[10],formats)
    worksheet.write_row('A13', datab[11],formats)
    worksheet.write_row('A14', datab[12],formats)
    worksheet.write_row('A15', datab[13],formats)
    worksheet.write_row('A16', datab[14],formats)
    worksheet.write_row('A17', datab[15],formats)
    worksheet.write_row('A18', datab[16],formats)
    worksheet.write_row('A19', datab[17],formats)
    worksheet.write_row('A20', datab[18],formats)
    worksheet.write_row('A21', datab[19],formats)
    worksheet.write_row('A22', datab[20],formats)
    worksheet.write_row('A23', datab[21],formats)
    worksheet.write_row('A24', datab[22],formats)
    worksheet.write_row('A25', datab[23],formats)
    #设置图形类型,line线条样式表.
    chart1 = workbook.add_chart({'type': 'line','subtype': 'stacked'})
    #chart1 = workbook.add_chart({'type': 'bar','subtype': 'stacked'})
    #以下是数据轴配置设置.
    # Configure the first series.
    chart1.add_series({
    'name': '=Sheet1!$A$2',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$2:$KC$2',
    'fill':   {'color': 'blue'},
    #'y2_axis':True,
    })
    chart1.add_series({
    'name': '=Sheet1!$A$3',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$3:$KC$3',
    'fill':   {'color': 'red'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$4',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$4:$KC$4',
    'fill':   {'color': '#FFFFC2'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$5',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$5:$KC$5',
    'fill':   {'color': 'black'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$6',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$6:$KC$6',
    'fill':   {'color': 'yellow'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$7',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$7:$KC$7',
    'fill':   {'color': 'green'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$8',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$8:$KC$8',
    'fill':   {'color': 'white'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$9',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$9:$KC$9',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$10',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$10:$KC$10',
    'fill':   {'color': 'gray'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$11',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$11:$KC$11',
    'fill':   {'color': 'brown'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$12',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$12:$KC$12',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$13',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$13:$KC$13',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$14',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$14:$KC$14',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$15',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$15:$KC$15',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$16',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$16:$KC$16',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$17',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$17:$KC$17',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$18',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$18:$KC$18',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$19',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$19:$KC$19',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$20',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$20:$KC$20',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$21',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$21:$KC$21',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$22',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$22:$KC$22',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$23',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$23:$KC$23',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$24',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$24:$KC$24',
    'fill':   {'color': 'purple'},
    })
    chart1.add_series({
    'name': '=Sheet1!$A$25',
    'categories': '=Sheet1!$B$1:$KC$1',
    'values': '=Sheet1!$B$25:$KC$25',
    'fill':   {'color': 'purple'},
    })
    # Add a chart title and some axis labels.
    chart1.set_title ({'name': u'新浪视频cdn带宽数据报表'}) #设置图表title(上方)大标题.
    chart1.set_x_axis({'name': u'数据统计时间:%s' %times_shijian})#设置x轴（左侧）小标题
    chart1.set_y_axis({'name': u'每5分钟数据统计(单位是M)'})#设置y轴（左侧）小标题.
    #chart1.set_y2_axis({'name':u'节点区域'})
    chart1.set_size({'width': 2300, 'height': 850}) ##设置图表大小(宽度和高度).
    worksheet.insert_chart('A27', chart1, {'y_offset': 25, 'x_offset': 10})#图表在27行插入.
    #os.system('yes|mv /root/line.txt /backup/line/line.txt_%s'  %(time_shijian))
    #os.system('yes|mv /root/chart_scatter.xlsx /backup/excel_xlsx/chart_scatter_%s.xlsx' %(time_shijian))
    fb.close()
    workbook.close()
    os.system('yes|mv /root/line.txt /data1/daikuan/backup/line/line.txt_%s'  %(time_shijian))
    os.system('yes|mv /root/chart_scatter.xlsx /data1/daikuan/backup/excel_xlsx/chart_scatter_%s.xlsx' %(time_shijian))
    
效果图如下：

![](http://i.imgur.com/z5Ili9r.jpg)

![](http://i.imgur.com/BAUldCu.jpg)

