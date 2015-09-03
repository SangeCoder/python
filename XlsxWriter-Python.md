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

在Excel中的日期和时间都是浮点数，有一个数字格式适用于显示它们以正确的格式。如果日期和时间xlsxwriter使得Python DateTime对象所需数量自动转换。但是，我们也需要添加数字格式