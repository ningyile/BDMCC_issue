# BDMCC和PostgreSQL常见问题汇总
<p align="center">
  <img src="https://raw.githubusercontent.com/ningyile/BDMCC_APP/main/img/mac_logo.png" width="20%" height="20%" />
</p>


安装前请认真查看PostgreSQL和BDMCC的安装视频教程，请单击[教程链接](https://www.bilibili.com/video/BV1Kc411q7g7/?vd_source=bf0edee2bc28836d99f5d19fd28d2225)前往观看：
[https://www.bilibili.com/video/BV1Kc411q7g7/?vd_source=bf0edee2bc28836d99f5d19fd28d2225](https://www.bilibili.com/video/BV1Kc411q7g7/?vd_source=bf0edee2bc28836d99f5d19fd28d2225)

教程前7分钟为通用部分，所有系统的用户都要观看，后面可根据自己系统在视频下方第一条评论单击时间戳跳转至相应的位置。

<p align="center">
  <img src="https://raw.githubusercontent.com/ningyile/BDMCC_issue/main/img/time_stamp.png" width="70%" height="70%" />
</p>

## 1 安装PostgreSQL出现错误

- 有部分Windows用户安装PostgreSQL时候出现的`there has been an error. error running c:\windows\system32\icacls “C:\users\administrator\appdata\local\temp/postgresql_installer......`报错。

<p align="center">
  <img src="https://raw.githubusercontent.com/ningyile/BDMCC_issue/main/img/pg_error_1.png" width="80%" height="80%" />
</p> 
解决办法：
  1.请检查用户设置选项，将用户设置改本地用户（非Microsoft账户）；
  2.用户名和设备名称改改成英文，不能是中文，改为英文后重启一下电脑；
  3.确保安装包所在的绝对路径不能含有中文（磁盘符中文无所谓，文件夹不能含有中文）。

<p align="center">
  <img src="https://raw.githubusercontent.com/ningyile/BDMCC_issue/main/img/pg_error_2.png" width="80%" height="80%" />
</p> 

<p align="center">
  <img src="https://raw.githubusercontent.com/ningyile/BDMCC_issue/main/img/pg_error_3.png" width="80%" height="80%" />
</p> 

<p align="center">
  <img src="https://raw.githubusercontent.com/ningyile/BDMCC_issue/main/img/pg_error_4.png" width="80%" height="80%" />
</p> 

- 无论是Windows、macOS还是Linux只需要安装一个版本的PostgreSQL就可以了，如安装多余版本的Postgresql，请确保在源数据备份或安全无虞的情况下适当的卸载多余的PostgreSQL。

- 未能解决，联系客服远程处理。

## 2 既往已经安装了一些数据集还需要重新安装吗？

- 是的，建议重新安装。BDMCC软件会除了逐渐不全官方所有的Concepts之外，部分数据库还会附带一些单独的BDMCC增强型表单，在后续的实战课程会用到，会大大减少代码书写量。

## 3 既往已经安装的数据集在使用BDMCC软件重新安装后会被覆盖吗？
- BDMCC软件安装的数据集在PostgreSQL对应的数据名称（即dbname）以及安装时间如下。

| DBeaver中对应数据集名称（dbname） | 数据集            | 版本号 | Base模块 | Concepts模块 |
| --------------------------------- | ----------------- | ------ | -------- | ------------ |
| mimic3_demo                       | MIMIC-III-Demo    | V1.4   |          |              |
| mimic3                            | MIMIC-III         | V1.4   | 53 min   | 20 min       |
| mimic3_carevue                    | MIMIC-III-CareVue | V1.4   | 22 min   | 7 min        |
| mimic4                            | MIMIC-IV          | V2.0   | 54 min   | 55 min       |
| eicu                              | eICU              | V2.0   | 15 min   | 21 min       |

举个例子，假如你既往已经安装了含有一个名为mimic4的数据集，那么再使用BDMCC进行安装就会把原有的mimic4给DROP掉，然后再重新创建一个名为mimic4的数据集。如此操作会导致原有的数据集和相应的数据架构（schema）以及schema下的表单（table）、视图（view）被覆盖。假如你想保存原有的数据集，可以使用DBeaver添加该数据集的连接，然后重新命名该数据集即可（相当于改名字进行备份，但会占用比较多的空间，不建议这么做，最佳的方式是留存有自定义的table和view的SQL代码，建议配合使用**strong包**中的**batch_tables、batch_views**等系列函数进行批量的创建至自定义的schema中）。在下图的例子中，因为原有安装的数据集也叫mimic4，为了留存原有的数据库，将原有的数据库命名为mimic5（单纯改名字为mimic5，也可以改为其他，比如mimic4_previous，不会影响数据集的数据内容，此操作会占用大量磁盘空间）。

<p align="center">
  <img src="https://raw.githubusercontent.com/ningyile/BDMCC_issue/main/img/rename_db.png" width="80%" height="80%" />
</p>

## 4 如何将自己设备的已经安装的数据集全部列出？

- Windows下使用官网的PostgreSQL默认模式是需要密码的，故在`cmd`终端下使用`psql -U postgres`，注意，Windows下默认的用户是postgres，假如你设置了别的数据库用户名，请进行相应的替换。然后输入对应的密码，注意输入的密码在终端下不显示，不要怀疑自己，直接输入，然后后回车即可进入PostgreSQL的交互式命令。然后再输入`\l`（小写的字母L，不是1234的1）即可列出既往安装所有的数据库的dbname，然后可以参考上述dbname的清单，在DBeaver中配置连接。
- macOS和Linux下使用本教程推荐的安装工具进行安装，默认的HBA模式是trust，并且在安装PostgreSQL的时候已经创建了系统用户名同名的数据库同户名。故无需传入`-U`的用户名参数，而只需在终端下输入`psql`即可进入PostgreSQL的交互式命令。然后再输入`\l`（小写的字母L，不是1234的1）即可列出既往安装所有的数据库的dbname，然后可以参考上述dbname的清单，在DBeaver中配置连接。

<p align="center">
  <img src="https://raw.githubusercontent.com/ningyile/BDMCC_issue/main/img/list_db.png" width="80%" height="80%" />
</p>
