目标：把oracle的O_CBS_CISCUSTOMERINFO表导入gauss的O_CBS_CISCUSTOMERINFO

--1.准备工作：（你们要做）
oracle的odm用户有O_CBS_CISCUSTOMERINFO表
gauss的odm模式下有O_CBS_CISCUSTOMERINFO表


--2.启动高斯服务(在高斯操作)（你们要做）
su omm
gs_om -t start
gsql -d postgres -p 15400 -r
\c - scott
create database  daikuan;
\c daikuan;
create schema   odm;
set current_schema=odm;


```
CREATE TABLE odm.O_CBS_CISCUSTOMERINFO
(
  BRC             VARCHAR2(3),
  CUSTOMID        VARCHAR2(21),
  IDTYPE          VARCHAR2(2),
  IDNO            VARCHAR2(22),
  LOSTDATE        VARCHAR2(10),
  CUSTOMTYPE      VARCHAR2(1),
  CUSTSUBTYPE     VARCHAR2(3),
  CUSTOMNAME      VARCHAR2(72),
  CUSTMNGNO       VARCHAR2(8),
  SALESPEOPLECODE VARCHAR2(8),
  CUSTENGNAME     VARCHAR2(50),
  LINKMAN         VARCHAR2(52),
  LINKTEL         VARCHAR2(20),
  SERVERLEVEL     VARCHAR2(1),
  VISADATE        VARCHAR2(10),
  GROUPID         VARCHAR2(8),
  CUSTSHORTNAME   VARCHAR2(20),
  VISAADDRESS     VARCHAR2(50),
  AVAIDATE        VARCHAR2(10),
  MAIL            VARCHAR2(40),
  NATIONCODE      VARCHAR2(13),
  SHAREHOLDER     VARCHAR2(1),
  VARIFYTYPE      VARCHAR2(1),
  PSWC            VARCHAR2(6),
  OPENBRC         VARCHAR2(9),
  TELLERCODE      VARCHAR2(6),
  OPENDATE        VARCHAR2(10),
  MODIBRC         VARCHAR2(9),
  MODITELLER      VARCHAR2(6),
  AUTHTELLER      VARCHAR2(6),
  MODIDATE        VARCHAR2(10),
  CTRLCODE        VARCHAR2(2),
  CUSTSTATUS      VARCHAR2(2),
  RESON           VARCHAR2(40),
  REMARK          VARCHAR2(40),
  ODS_DATA_DT     VARCHAR2(20),
  ODS_PK_MD5      CHAR(32),
  ODS_CMP_COL_MD5 CHAR(32),
  ODS_FLG         CHAR(1)
);
```
--3.在windows的kettle转换中添加表输入和表输出组件，保存的转换名叫 O_CBS_CISCUSTOMERINFO.ktr（不用做）
表输入：oracle
表输出：guass

--4.如何查看windows ip（用我的即可，你们不用改了）（不用做）
win + r
cmd
ipconfig

当前我的ip是： 192.168.2.65


--5.在linux中，修改配置文件 vi /home/pdi-ce-7.1.0.0-12/data-integration/simple-jndi/jdbc.properties （我们要做，文件最末尾添加）
postgres/type=javax.sql.DataSource
postgres/driver=org.opengauss.Driver
postgres/url=jdbc:opengauss://192.168.222.133:15400/daikuan?currentSchema=odm
postgres/user=scott
postgres/password=@11qqaazz


--6.linux中，输入 (我们要做)
cd    /home/pdi-ce-7.1.0.0-12/data-integration/lib
把opengauss-jdbc-5.0.0.jar 拖拽到lib目录下

--7.linux中，输入(我们要做)
cd  /lx
把O_CBS_CISCUSTOMERINFO.ktr上传到/lx目录下

--8.linux中，给文件授权(我们要做)
chmod 755  /lx/O_CBS_CISCUSTOMERINFO.ktr


--9.（高斯）为了能看到效果，我先去高斯里面清空这张表的数据(我们要做)
truncate  table odm.O_CBS_CISCUSTOMERINFO;
select * from odm.O_CBS_CISCUSTOMERINFO;


--10.linux，执行转换(我们要做)
/home/pdi-ce-7.1.0.0-12/data-integration/pan.sh -file /lx/O_CBS_CISCUSTOMERINFO.ktr -logfile  /lx/20250605.log  -level  Detailed

# linux定时调度kettle
-----1.启动高斯环境
su omm
gs_om -t start
gsql -d postgres -p 15400 -r
\c - scott
\c daikuan
set current_schema=odm;
truncate table odm.O_CBS_CISCUSTOMERINFO;


-----2.shell封装kettle      vim  /lx/k1.sh
. /etc/profile
. ~/.bash_profile

/home/pdi-ce-7.1.0.0-12/data-integration/pan.sh -file /lx/O_CBS_CISCUSTOMERINFO.ktr -logfile  /lx/20250606.log  -level  Detailed


-----3.给脚本授权
chmod 755  /lx/k1.sh


-----4.查看定时任务状态
service crond status


-----5.编辑定时调度任务 crontab -e
58 9 * * * . /etc/profile;/bin/sh /lx/k1.sh <span style="background:#d3f8b6">##每天9点58调用</span>
