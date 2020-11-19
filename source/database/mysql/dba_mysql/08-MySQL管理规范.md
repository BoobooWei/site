---
title: MySQL管理规范
---

![](pic/12.png)

# 数据库文档模板
|No.|数据库文档模板|
|:--|:--|
|1|[业务信息模板](/database/mysql/dba_mysql/08-MySQL管理规范/业务信息模板.html)|
|2|[实例列表模板](/database/mysql/dba_mysql/08-MySQL管理规范/实例列表模板.html)|
|3|[服务器明细模板](/database/mysql/dba_mysql/08-MySQL管理规范/服务器明细模板.html)|
|4|[应用明细模板](/database/mysql/dba_mysql/08-MySQL管理规范/应用明细模板.html)|
|5|[迁移报告模板](/database/mysql/dba_mysql/08-MySQL管理规范/迁移报告模板.html)|
|6|[迁移方案模板](/database/mysql/dba_mysql/08-MySQL管理规范/迁移方案模板.html)|
|7|[排查报告模板](/database/mysql/dba_mysql/08-MySQL管理规范/排查报告模板.html)|
|8|[优化建议模板](/database/mysql/dba_mysql/08-MySQL管理规范/优化建议模板.html)|
|9|[自建MySQL逻辑备份模板](/database/mysql/dba_mysql/08-MySQL管理规范/自建MySQL逻辑备份模板.html)|
|10|[自建MySQL物理备份模板](/database/mysql/dba_mysql/08-MySQL管理规范/自建MySQL物理备份模板.html)|
  
# 数据库技术规范

|No.|数据库技术规范|
|:--|:--|
|1| [文档归档要求](/database/mysql/dba_mysql/08-MySQL管理规范/文档归档要求.html)|
|2| [业务数据库对接规范](/database/mysql/dba_mysql/08-MySQL管理规范/业务数据库对接规范.html)|
|3| [数据库安装部署规范](/database/mysql/dba_mysql/08-MySQL管理规范/数据库安装部署规范.html)|
|4| [数据库备份规范](/database/mysql/dba_mysql/08-MySQL管理规范/数据库备份规范.html)|
|5| [数据库安全规范](/database/mysql/dba_mysql/08-MySQL管理规范/数据库安全规范.html)|
|6| [数据库归档规范](/database/mysql/dba_mysql/08-MySQL管理规范/数据库归档规范.html)|
|7| [数据库监控规范](/database/mysql/dba_mysql/08-MySQL管理规范/数据库监控规范.html)|
|8| [数据库迁移规范](/database/mysql/dba_mysql/08-MySQL管理规范/数据库迁移规范.html)|
|9| [数据库升级规范](/database/mysql/dba_mysql/08-MySQL管理规范/数据库升级规范.html)|
|10| [阿里云资产标签规范](/database/mysql/dba_mysql/08-MySQL管理规范/阿里云资产标签规范.html)|

# 团队管理规范

* [团队管理规范](/database/mysql/dba_mysql/08-MySQL管理规范/团队管理规范.html)

# 思维导图

<head>
    <meta charset="utf-8"/>
    <meta content="IE=edge" http-equiv="X-UA-Compatible"/>
    <title></title>
    <style>
        body{
            margin: 0;
        }
        #content-info{
            width: auto;
            margin: 0 auto;
            text-align: center;
        }
        #author-info{
            white-space: nowrap;
            text-overflow: ellipsis;
            overflow: hidden;
        }
        #title{
            text-overflow: ellipsis;
            white-space: nowrap;
            overflow: hidden;
            padding-top: 10px;
            margin-bottom: 2px;
            font-size: 34px;
            color: #505050;
        }
        .text{
            white-space:nowrap;
            text-overflow: ellipsis;
            display: inline-block;
            margin-right: 20px;
            margin-bottom: 2px;
            font-size: 20px;
            color: #8c8c8c;
        }
        #navBar{
            width: auto;
            height: auto;
            position: fixed;
            right:0;
            bottom: 0;
            background-color: #f0f3f4;
            overflow-y: auto;
            text-align: center;
        }
        #svg-container{
            width: 100%;
            overflow-x: scroll;
            min-width: 0px;
            margin: 0 10px;
        }
        #nav-thumbs{
            overflow-y: scroll;
            padding: 0 5px;
        }
        .nav-thumb{
            position: relative;
            margin: 10px auto;
        }
        .nav-thumb >p{
            text-align: center;
            font-size: 12px;
            margin: 4px 0 0 0;
        }
        .nav-thumb >div{
            position: relative;
            display: inline-block;
            border: 1px solid #c6cfd5;
        }
        .nav-thumb img{
            display: block;
        }
        #main-content{
            bottom: 0;
            left: 0;
            right: 0;
            background-color: #d0cfd8;
            display: flex;
            height: auto;
            flex-flow: row wrap;
            text-align:center;
        }
        #svg-container >svg{
            display: block;
            margin:10px auto;
            margin-bottom: 0;
        }
        #copyright{
            bottom: 0;
            left: 50%;
            margin: 5px auto;
            font-size: 16px;
            color: #515151;
        }
        #copyright >a{
            text-decoration: none;
            color: #77C;
        }
        .number{
            position: absolute;
            top:0;
            left:0;
            border-top:22px solid #08a1ef;
            border-right: 22px solid transparent;
        }
        .pagenum{
            font-size: 12px;
            color: #fff;
            position: absolute;
            top: -23px;
            left: 2px;
        }
            #navBar::-webkit-scrollbar{
            width: 8px;
            background-color: #f5f5f5;
        }
            #navBar::-webkit-scrollbar-track{
            -webkit-box-shadow: inset 0 0 4px rgba(0,0,0,.3);
            border-radius: 8px;
            background-color: #fff;
        }
            #navBar::-webkit-scrollbar-thumb{
            border-radius: 8px;
            -webkit-box-shadow: inset 0 0 4px rgba(0,0,0,.3);
            background-color: #6b6b70;
        }
        #navBar::-webkit-scrollbar-thumb:hover{
            background-color: #4a4a4f;
        }    
    </style>
</head>
<body>
<div id="main-area">
    <div id="content-info">
    </div>
      </div>
      <div id="main-content">
        <div id="svg-container"><svg height="2042" xmlns:ed="https://www.edrawsoft.cn/xml/2017/SVGExtensions/" ed:vspacing="30" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:ev="http://www.w3.org/2001/xml-events" preserveaspectradio="xMinYMin meet" id="page0" width="1375" ed:hspacing="30" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1375 2042" ed:name="页面-1">
    <style type="text/css">
g[ed\:togtopicid],g[ed\:hyperlink],g[ed\:comment],g[ed\:note] {cursor:pointer;}
g[id] {-moz-user-select: none;-ms-user-select: none;user-select: none;}
svg text::selection,svg tspan::selection{background-color: #4285f4;color: #ffffff;fill: #ffffff;}
.st5 {fill:#303030;font-family:Apple LiSung Light;font-size:10.5pt}
.st6 {fill:#444444}
.st2 {fill:#454545;font-family:Apple LiSung Light;font-size:10.5pt}
.st3 {fill:#454545;font-family:Nadeem;font-size:10.5pt}
.st7 {fill:#4d4d4c}
.st1 {fill:#ffffff;font-family:Apple LiSung Light;font-size:10.5pt}
.st4 {font-family:Helvetica Neue,Helvetica,Arial,sans-serif}
</style>
    <defs></defs>
    <rect height="2042" fill="#ffffff" x="0" width="1375" y="0"></rect>
    <path ed:type="summary" transform="matrix(1,0,0,1,657.22,1195)" stroke-linejoin="round" fill="none" d="M0.1,0C6,0,5.9,7.2,5.9,7.2L5.9,50.8C5.9,50.8,6.9,56.9,12,56.9C6.9,57.2,5.9,63.1,5.9,63.1L5.9,106.6C5.9,106.6,5.8,113.8,0,113.8" ed:idlist="556,558,560,562" ed:parentid="556,558,560,562" id="576" stroke="#b9947b"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,161.25,693.02)" stroke-linejoin="round" fill="none" d="M4.6,326.2L32.6,326.2L32.6,-320.2C32.6,-323.5,35.3,-326.2,38.6,-326.2L67.6,-326.2" stroke-width="1.33333" ed:parentid="101" id="103" stroke="#696969" ed:tosuperid="102"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,161.25,926.77)" stroke-linejoin="round" fill="none" d="M4.6,92.4L32.6,92.4L32.6,-86.4C32.6,-89.7,35.3,-92.4,38.6,-92.4L67.6,-92.4" stroke-width="1.33333" ed:parentid="101" id="105" stroke="#696969" ed:tosuperid="104"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,161.25,1082.92)" stroke-linejoin="round" fill="none" d="M4.6,-63.7L32.6,-63.7L32.6,57.7C32.6,61,35.3,63.7,38.6,63.7L67.6,63.7" stroke-width="1.33333" ed:parentid="101" id="107" stroke="#696969" ed:tosuperid="106"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,161.25,1499.81)" stroke-linejoin="round" fill="none" d="M4.6,-480.6L32.6,-480.6L32.6,474.6C32.6,477.9,35.3,480.6,38.6,480.6L67.6,480.6" stroke-width="1.33333" ed:parentid="101" id="115" stroke="#696969" ed:tosuperid="114"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,161.25,1441.65)" stroke-linejoin="round" fill="none" d="M4.6,-422.4L32.6,-422.4L32.6,416.4C32.6,419.7,35.3,422.4,38.6,422.4L67.6,422.4" stroke-width="1.33333" ed:parentid="101" id="117" stroke="#696969" ed:tosuperid="116"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,379.72,250)" stroke-linejoin="round" fill="none" d="M-13.5,116.8L3.7,116.8L3.7,-110.8C3.7,-114.1,6.4,-116.8,9.7,-116.8L13.5,-116.8" stroke-width="1.33333" ed:parentid="102" id="121" stroke="#696969" ed:tosuperid="120"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,379.72,425.37)" stroke-linejoin="round" fill="none" d="M3.7,-58.5L3.7,52.5C3.7,55.9,6.4,58.5,9.7,58.5L13.5,58.5" stroke-width="1.33333" ed:parentid="102" id="123" stroke="#696969" ed:tosuperid="122"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,480.72,110.6)" stroke-linejoin="round" fill="none" d="M-13.5,22.6L3.7,22.6L3.7,-16.6C3.7,-19.9,6.4,-22.6,9.7,-22.6L13.5,-22.6" stroke-width="1.33333" ed:parentid="120" id="127" stroke="#696969" ed:tosuperid="126"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,553.72,95.56)" stroke-linejoin="round" fill="none" d="M3.7,-7.5L3.7,1.5C3.7,4.8,6.4,7.5,9.7,7.5L13.5,7.5" stroke-width="1.33333" ed:parentid="126" id="129" stroke="#696969" ed:tosuperid="128"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,480.72,163.25)" stroke-linejoin="round" fill="none" d="M3.7,-30.1L3.7,24.1C3.7,27.4,6.4,30.1,9.7,30.1L13.5,30.1" stroke-width="1.33333" ed:parentid="120" id="143" stroke="#696969" ed:tosuperid="142"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,623.72,438.79)" stroke-linejoin="round" fill="none" d="M3.7,-15L3.7,9C3.7,12.4,6.4,15,9.7,15L13.5,15" stroke-width="1.33333" ed:parentid="474" id="147" stroke="#696969" ed:tosuperid="144"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,623.72,346.15)" stroke-linejoin="round" fill="none" d="M-13.5,77.6L3.7,77.6L3.7,-71.6C3.7,-74.9,6.4,-77.6,9.7,-77.6L13.5,-77.6" stroke-width="1.33333" ed:parentid="474" id="148" stroke="#696969" ed:tosuperid="145"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,522.72,576.56)" stroke-linejoin="round" fill="none" d="M3.7,-92.6L3.7,86.6C3.7,90,6.4,92.6,9.7,92.6L13.5,92.6" stroke-width="1.33333" ed:parentid="122" id="149" stroke="#696969" ed:tosuperid="146"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,480.72,816.04)" stroke-linejoin="round" fill="none" d="M3.7,-15L3.7,9C3.7,12.4,6.4,15,9.7,15L13.5,15" stroke-width="1.33333" ed:parentid="156" id="157" stroke="#696969" ed:tosuperid="158"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,480.72,801)" stroke-linejoin="round" fill="none" d="M3.7,0C3.7,0,6.4,0,9.7,0L13.5,0" stroke-width="1.33333" ed:parentid="156" id="159" stroke="#696969" ed:tosuperid="160"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,480.72,785.96)" stroke-linejoin="round" fill="none" d="M-13.5,15L3.7,15L3.7,-9C3.7,-12.4,6.4,-15,9.7,-15L13.5,-15" stroke-width="1.33333" ed:parentid="156" id="161" stroke="#696969" ed:tosuperid="162"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,480.72,906.29)" stroke-linejoin="round" fill="none" d="M3.7,-15L3.7,9C3.7,12.4,6.4,15,9.7,15L13.5,15" stroke-width="1.33333" ed:parentid="164" id="165" stroke="#696969" ed:tosuperid="166"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,480.72,891.25)" stroke-linejoin="round" fill="none" d="M3.7,0C3.7,0,6.4,0,9.7,0L13.5,0" stroke-width="1.33333" ed:parentid="164" id="167" stroke="#696969" ed:tosuperid="168"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,480.72,876.21)" stroke-linejoin="round" fill="none" d="M-13.5,15L3.7,15L3.7,-9C3.7,-12.4,6.4,-15,9.7,-15L13.5,-15" stroke-width="1.33333" ed:parentid="164" id="169" stroke="#696969" ed:tosuperid="163"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,379.72,817.67)" stroke-linejoin="round" fill="none" d="M-13.5,16.7L3.7,16.7L3.7,-10.7C3.7,-14,6.4,-16.7,9.7,-16.7L13.5,-16.7" stroke-width="1.33333" ed:parentid="104" id="177" stroke="#696969" ed:tosuperid="156"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,379.72,862.79)" stroke-linejoin="round" fill="none" d="M3.7,-28.5L3.7,22.5C3.7,25.8,6.4,28.5,9.7,28.5L13.5,28.5" stroke-width="1.33333" ed:parentid="104" id="178" stroke="#696969" ed:tosuperid="164"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,365.72,1517.37)" stroke-linejoin="round" fill="none" d="M-13.5,61.8L3.7,61.8L3.7,-55.8C3.7,-59.1,6.4,-61.8,9.7,-61.8L13.5,-61.8" stroke-width="1.33333" ed:parentid="108" id="225" stroke="#696969" ed:tosuperid="204"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,365.72,1569.98)" stroke-linejoin="round" fill="none" d="M3.7,9.2L3.7,-3.2C3.7,-6.5,6.4,-9.2,9.7,-9.2L13.5,-9.2" stroke-width="1.33333" ed:parentid="108" id="226" stroke="#696969" ed:tosuperid="212"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,682.72,200.85)" stroke-linejoin="round" fill="none" d="M-13.5,7.5L3.7,7.5L3.7,-1.5C3.7,-4.8,6.4,-7.5,9.7,-7.5L13.5,-7.5" stroke-width="1.33333" ed:parentid="445" id="390" stroke="#696969" ed:tosuperid="389"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,682.72,215.9)" stroke-linejoin="round" fill="none" d="M3.7,-7.5L3.7,1.5C3.7,4.8,6.4,7.5,9.7,7.5L13.5,7.5" stroke-width="1.33333" ed:parentid="445" id="392" stroke="#696969" ed:tosuperid="391"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,553.72,65.48)" stroke-linejoin="round" fill="none" d="M-13.5,22.6L3.7,22.6L3.7,-16.6C3.7,-19.9,6.4,-22.6,9.7,-22.6L13.5,-22.6" stroke-width="1.33333" ed:parentid="126" id="394" stroke="#696969" ed:tosuperid="393"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,553.72,80.52)" stroke-linejoin="round" fill="none" d="M3.7,7.5L3.7,-1.5C3.7,-4.8,6.4,-7.5,9.7,-7.5L13.5,-7.5" stroke-width="1.33333" ed:parentid="126" id="396" stroke="#696969" ed:tosuperid="395"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,553.72,110.6)" stroke-linejoin="round" fill="none" d="M3.7,-22.6L3.7,16.6C3.7,19.9,6.4,22.6,9.7,22.6L13.5,22.6" stroke-width="1.33333" ed:parentid="126" id="405" stroke="#696969" ed:tosuperid="397"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,595.72,178.29)" stroke-linejoin="round" fill="none" d="M-13.5,15L3.7,15L3.7,-9C3.7,-12.4,6.4,-15,9.7,-15L13.5,-15" stroke-width="1.33333" ed:parentid="142" id="432" stroke="#696969" ed:tosuperid="411"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,595.72,200.85)" stroke-linejoin="round" fill="none" d="M3.7,-7.5L3.7,1.5C3.7,4.8,6.4,7.5,9.7,7.5L13.5,7.5" stroke-width="1.33333" ed:parentid="142" id="473" stroke="#696969" ed:tosuperid="445"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,522.72,453.83)" stroke-linejoin="round" fill="none" d="M-13.5,30.1L3.7,30.1L3.7,-24.1C3.7,-27.4,6.4,-30.1,9.7,-30.1L13.5,-30.1" stroke-width="1.33333" ed:parentid="122" id="475" stroke="#696969" ed:tosuperid="474"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,724.72,261.02)" stroke-linejoin="round" fill="none" d="M-13.5,7.5L3.7,7.5L3.7,-1.5C3.7,-4.8,6.4,-7.5,9.7,-7.5L13.5,-7.5" stroke-width="1.33333" ed:parentid="145" id="477" stroke="#696969" ed:tosuperid="476"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,724.72,383.75)" stroke-linejoin="round" fill="none" d="M-13.5,70.1L3.7,70.1L3.7,-64.1C3.7,-67.4,6.4,-70.1,9.7,-70.1L13.5,-70.1" stroke-width="1.33333" ed:parentid="144" id="479" stroke="#696969" ed:tosuperid="478"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,724.72,398.79)" stroke-linejoin="round" fill="none" d="M3.7,55L3.7,-49C3.7,-52.4,6.4,-55,9.7,-55L13.5,-55" stroke-width="1.33333" ed:parentid="144" id="481" stroke="#696969" ed:tosuperid="480"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,825.72,343.75)" stroke-linejoin="round" fill="none" d="M-13.5,0L3.7,0C3.7,0,6.4,0,9.7,0L13.5,0" stroke-width="1.33333" ed:parentid="480" id="483" stroke="#696969" ed:tosuperid="482"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,825.72,313.67)" stroke-linejoin="round" fill="none" d="M-13.5,0L3.7,0C3.7,0,6.4,0,9.7,0L13.5,0" stroke-width="1.33333" ed:parentid="478" id="485" stroke="#696969" ed:tosuperid="484"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,724.72,423.83)" stroke-linejoin="round" fill="none" d="M3.7,30L3.7,-24C3.7,-27.3,6.4,-30,9.7,-30L13.5,-30" stroke-width="1.33333" ed:parentid="144" id="487" stroke="#696969" ed:tosuperid="486"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,724.72,523.92)" stroke-linejoin="round" fill="none" d="M3.7,-70.1L3.7,64.1C3.7,67.4,6.4,70.1,9.7,70.1L13.5,70.1" stroke-width="1.33333" ed:parentid="144" id="489" stroke="#696969" ed:tosuperid="488"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,825.72,403.83)" stroke-linejoin="round" fill="none" d="M-13.5,-10L3.7,-10L3.7,4C3.7,7.3,6.4,10,9.7,10L13.5,10" stroke-width="1.33333" ed:parentid="486" id="491" stroke="#696969" ed:tosuperid="490"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,797.72,594)" stroke-linejoin="round" fill="none" d="M-13.5,0L3.7,0C3.7,0,6.4,0,9.7,0L13.5,0" stroke-width="1.33333" ed:parentid="488" id="493" stroke="#696969" ed:tosuperid="492"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,724.72,478.87)" stroke-linejoin="round" fill="none" d="M3.7,-25L3.7,19C3.7,22.4,6.4,25,9.7,25L13.5,25" stroke-width="1.33333" ed:parentid="144" id="495" stroke="#696969" ed:tosuperid="494"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,825.72,533.92)" stroke-linejoin="round" fill="none" d="M-13.5,-30L3.7,-30L3.7,24C3.7,27.3,6.4,30,9.7,30L13.5,30" stroke-width="1.33333" ed:parentid="494" id="497" stroke="#696969" ed:tosuperid="496"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,623.72,646.65)" stroke-linejoin="round" fill="none" d="M-13.5,22.6L3.7,22.6L3.7,-16.6C3.7,-19.9,6.4,-22.6,9.7,-22.6L13.5,-22.6" stroke-width="1.33333" ed:parentid="146" id="503" stroke="#696969" ed:tosuperid="502"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,623.72,661.69)" stroke-linejoin="round" fill="none" d="M3.7,7.5L3.7,-1.5C3.7,-4.8,6.4,-7.5,9.7,-7.5L13.5,-7.5" stroke-width="1.33333" ed:parentid="146" id="505" stroke="#696969" ed:tosuperid="504"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,623.72,676.73)" stroke-linejoin="round" fill="none" d="M3.7,-7.5L3.7,1.5C3.7,4.8,6.4,7.5,9.7,7.5L13.5,7.5" stroke-width="1.33333" ed:parentid="146" id="507" stroke="#696969" ed:tosuperid="506"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,623.72,691.77)" stroke-linejoin="round" fill="none" d="M3.7,-22.6L3.7,16.6C3.7,19.9,6.4,22.6,9.7,22.6L13.5,22.6" stroke-width="1.33333" ed:parentid="146" id="509" stroke="#696969" ed:tosuperid="508"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,724.72,276.06)" stroke-linejoin="round" fill="none" d="M3.7,-7.5L3.7,1.5C3.7,4.8,6.4,7.5,9.7,7.5L13.5,7.5" stroke-width="1.33333" ed:parentid="145" id="511" stroke="#696969" ed:tosuperid="510"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,825.72,253.5)" stroke-linejoin="round" fill="none" d="M-13.5,0L3.7,0C3.7,0,6.4,0,9.7,0L13.5,0" stroke-width="1.33333" ed:parentid="476" id="513" stroke="#696969" ed:tosuperid="512"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,825.72,283.58)" stroke-linejoin="round" fill="none" d="M-13.5,0L3.7,0C3.7,0,6.4,0,9.7,0L13.5,0" stroke-width="1.33333" ed:parentid="510" id="515" stroke="#696969" ed:tosuperid="514"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,379.72,1077.31)" stroke-linejoin="round" fill="none" d="M3.7,69.3L3.7,-63.3C3.7,-66.6,6.4,-69.3,9.7,-69.3L13.5,-69.3" stroke-width="1.33333" ed:parentid="106" id="539" stroke="#696969" ed:tosuperid="538"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,379.72,1092.35)" stroke-linejoin="round" fill="none" d="M3.7,54.3L3.7,-48.3C3.7,-51.6,6.4,-54.3,9.7,-54.3L13.5,-54.3" stroke-width="1.33333" ed:parentid="106" id="541" stroke="#696969" ed:tosuperid="540"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,379.72,1107.4)" stroke-linejoin="round" fill="none" d="M3.7,39.2L3.7,-33.2C3.7,-36.5,6.4,-39.2,9.7,-39.2L13.5,-39.2" stroke-width="1.33333" ed:parentid="106" id="543" stroke="#696969" ed:tosuperid="542"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,379.72,1122.44)" stroke-linejoin="round" fill="none" d="M3.7,24.2L3.7,-18.2C3.7,-21.5,6.4,-24.2,9.7,-24.2L13.5,-24.2" stroke-width="1.33333" ed:parentid="106" id="545" stroke="#696969" ed:tosuperid="544"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,379.72,1137.48)" stroke-linejoin="round" fill="none" d="M3.7,9.1L3.7,-3.1C3.7,-6.5,6.4,-9.1,9.7,-9.1L13.5,-9.1" stroke-width="1.33333" ed:parentid="106" id="547" stroke="#696969" ed:tosuperid="546"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,379.72,1160.04)" stroke-linejoin="round" fill="none" d="M3.7,-13.4L3.7,7.4C3.7,10.7,6.4,13.4,9.7,13.4L13.5,13.4" stroke-width="1.33333" ed:parentid="106" id="549" stroke="#696969" ed:tosuperid="548"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,379.72,1205.17)" stroke-linejoin="round" fill="none" d="M3.7,-58.5L3.7,52.5C3.7,55.9,6.4,58.5,9.7,58.5L13.5,58.5" stroke-width="1.33333" ed:parentid="106" id="551" stroke="#696969" ed:tosuperid="550"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,379.72,1242.77)" stroke-linejoin="round" fill="none" d="M3.7,-96.1L3.7,90.1C3.7,93.5,6.4,96.1,9.7,96.1L13.5,96.1" stroke-width="1.33333" ed:parentid="106" id="553" stroke="#696969" ed:tosuperid="552"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,379.72,1062.27)" stroke-linejoin="round" fill="none" d="M-13.5,84.4L3.7,84.4L3.7,-78.4C3.7,-81.7,6.4,-84.4,9.7,-84.4L13.5,-84.4" stroke-width="1.33333" ed:parentid="106" id="555" stroke="#696969" ed:tosuperid="554"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,522.72,1241.15)" stroke-linejoin="round" fill="none" d="M-13.5,22.6L3.7,22.6L3.7,-16.6C3.7,-19.9,6.4,-22.6,9.7,-22.6L13.5,-22.6" stroke-width="1.33333" ed:parentid="550" id="557" stroke="#696969" ed:tosuperid="556"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,522.72,1256.19)" stroke-linejoin="round" fill="none" d="M3.7,7.5L3.7,-1.5C3.7,-4.8,6.4,-7.5,9.7,-7.5L13.5,-7.5" stroke-width="1.33333" ed:parentid="550" id="559" stroke="#696969" ed:tosuperid="558"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,522.72,1271.23)" stroke-linejoin="round" fill="none" d="M3.7,-7.5L3.7,1.5C3.7,4.8,6.4,7.5,9.7,7.5L13.5,7.5" stroke-width="1.33333" ed:parentid="550" id="561" stroke="#696969" ed:tosuperid="560"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,522.72,1286.27)" stroke-linejoin="round" fill="none" d="M3.7,-22.6L3.7,16.6C3.7,19.9,6.4,22.6,9.7,22.6L13.5,22.6" stroke-width="1.33333" ed:parentid="550" id="563" stroke="#696969" ed:tosuperid="562"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,564.72,1165.94)" stroke-linejoin="round" fill="none" d="M-13.5,7.5L3.7,7.5L3.7,-1.5C3.7,-4.8,6.4,-7.5,9.7,-7.5L13.5,-7.5" stroke-width="1.33333" ed:parentid="548" id="579" stroke="#696969" ed:tosuperid="578"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,564.72,1180.98)" stroke-linejoin="round" fill="none" d="M3.7,-7.5L3.7,1.5C3.7,4.8,6.4,7.5,9.7,7.5L13.5,7.5" stroke-width="1.33333" ed:parentid="548" id="581" stroke="#696969" ed:tosuperid="580"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,550.72,977.92)" stroke-linejoin="round" fill="none" d="M-13.5,0L3.7,0C3.7,0,6.4,0,9.7,0L13.5,0" stroke-width="1.33333" ed:parentid="554" id="585" stroke="#696969" ed:tosuperid="584"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,494.72,1435.54)" stroke-linejoin="round" fill="none" d="M-13.5,20L3.7,20L3.7,-14C3.7,-17.4,6.4,-20,9.7,-20L13.5,-20" stroke-width="1.33333" ed:parentid="204" id="587" stroke="#696969" ed:tosuperid="586"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,494.72,1460.58)" stroke-linejoin="round" fill="none" d="M3.7,-5L3.7,-1C3.7,2.3,6.4,5,9.7,5L13.5,5" stroke-width="1.33333" ed:parentid="204" id="589" stroke="#696969" ed:tosuperid="588"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,494.72,1485.62)" stroke-linejoin="round" fill="none" d="M3.7,-30L3.7,24C3.7,27.4,6.4,30,9.7,30L13.5,30" stroke-width="1.33333" ed:parentid="204" id="591" stroke="#696969" ed:tosuperid="590"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,494.72,1553.27)" stroke-linejoin="round" fill="none" d="M-13.5,7.5L3.7,7.5L3.7,-1.5C3.7,-4.8,6.4,-7.5,9.7,-7.5L13.5,-7.5" stroke-width="1.33333" ed:parentid="212" id="593" stroke="#696969" ed:tosuperid="592"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,494.72,1568.31)" stroke-linejoin="round" fill="none" d="M3.7,-7.5L3.7,1.5C3.7,4.8,6.4,7.5,9.7,7.5L13.5,7.5" stroke-width="1.33333" ed:parentid="212" id="595" stroke="#696969" ed:tosuperid="594"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,365.72,1637.67)" stroke-linejoin="round" fill="none" d="M3.7,-58.5L3.7,52.5C3.7,55.8,6.4,58.5,9.7,58.5L13.5,58.5" stroke-width="1.33333" ed:parentid="108" id="597" stroke="#696969" ed:tosuperid="596"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,494.72,1651.04)" stroke-linejoin="round" fill="none" d="M-13.5,45.1L3.7,45.1L3.7,-39.1C3.7,-42.4,6.4,-45.1,9.7,-45.1L13.5,-45.1" stroke-width="1.33333" ed:parentid="596" id="601" stroke="#696969" ed:tosuperid="600"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,494.72,1666.08)" stroke-linejoin="round" fill="none" d="M3.7,30.1L3.7,-24.1C3.7,-27.4,6.4,-30.1,9.7,-30.1L13.5,-30.1" stroke-width="1.33333" ed:parentid="596" id="603" stroke="#696969" ed:tosuperid="602"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,494.72,1681.12)" stroke-linejoin="round" fill="none" d="M3.7,15L3.7,-9C3.7,-12.4,6.4,-15,9.7,-15L13.5,-15" stroke-width="1.33333" ed:parentid="596" id="605" stroke="#696969" ed:tosuperid="604"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,494.72,1718.73)" stroke-linejoin="round" fill="none" d="M3.7,-22.6L3.7,16.6C3.7,19.9,6.4,22.6,9.7,22.6L13.5,22.6" stroke-width="1.33333" ed:parentid="596" id="607" stroke="#696969" ed:tosuperid="606"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,595.72,1718.73)" stroke-linejoin="round" fill="none" d="M-13.5,22.6L3.7,22.6L3.7,-16.6C3.7,-19.9,6.4,-22.6,9.7,-22.6L13.5,-22.6" stroke-width="1.33333" ed:parentid="606" id="609" stroke="#696969" ed:tosuperid="608"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,595.72,1733.77)" stroke-linejoin="round" fill="none" d="M3.7,7.5L3.7,-1.5C3.7,-4.8,6.4,-7.5,9.7,-7.5L13.5,-7.5" stroke-width="1.33333" ed:parentid="606" id="611" stroke="#696969" ed:tosuperid="610"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,595.72,1748.81)" stroke-linejoin="round" fill="none" d="M3.7,-7.5L3.7,1.5C3.7,4.8,6.4,7.5,9.7,7.5L13.5,7.5" stroke-width="1.33333" ed:parentid="606" id="613" stroke="#696969" ed:tosuperid="612"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,595.72,1763.85)" stroke-linejoin="round" fill="none" d="M3.7,-22.6L3.7,16.6C3.7,19.9,6.4,22.6,9.7,22.6L13.5,22.6" stroke-width="1.33333" ed:parentid="606" id="615" stroke="#696969" ed:tosuperid="614"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,365.72,1854.94)" stroke-linejoin="round" fill="none" d="M-13.5,9.1L3.7,9.1L3.7,-3.1C3.7,-6.5,6.4,-9.1,9.7,-9.1L13.5,-9.1" stroke-width="1.33333" ed:parentid="116" id="617" stroke="#696969" ed:tosuperid="616"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,365.72,1869.98)" stroke-linejoin="round" fill="none" d="M3.7,-5.9L3.7,-0.1C3.7,3.2,6.4,5.9,9.7,5.9L13.5,5.9" stroke-width="1.33333" ed:parentid="116" id="619" stroke="#696969" ed:tosuperid="618"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,365.72,1885.02)" stroke-linejoin="round" fill="none" d="M3.7,-20.9L3.7,14.9C3.7,18.2,6.4,20.9,9.7,20.9L13.5,20.9" stroke-width="1.33333" ed:parentid="116" id="621" stroke="#696969" ed:tosuperid="620"></path>
    <path stroke-linecap="round" transform="matrix(1,0,0,1,161.25,1299.19)" stroke-linejoin="round" fill="none" d="M4.6,-280L32.6,-280L32.6,274C32.6,277.3,35.3,280,38.6,280L67.6,280" stroke-width="1.33333" ed:parentid="101" id="622" stroke="#696969" ed:tosuperid="108"></path>
    <g transform="matrix(1,0,0,1,21.33,951.49)" id="101" ed:topictype="mainidea" ed:layout="map" ed:width="144.5520800352097" ed:height="135.4429300352097">
        <path fill="#365b88" stroke-linejoin="round" d="M4,0L140.6,0C143.2,0,144.6,1.3,144.6,4L144.6,131.4C144.6,134.1,143.2,135.4,140.6,135.4L4,135.4C1.3,135.4,0,134.1,0,131.4L0,4C0,1.3,1.3,0,4,0z" stroke-width="1.33333" stroke="#365b88"></path>
        <g transform="translate(27.95,15.27)">
            <path transform="matrix(1,0,0,1,0,0)" fill="#ffffff" d="M87.3,5L48.3,5L48.3,2.3L48.3,0L46.3,0L43.6,0L41.3,0L41.3,2.3L41.3,5L2.3,5L0,5L0,7.3L0,10L0,12.3L2.3,12.3L7.3,12.3L7.3,56C7.3,60.3,11,63.9,15.3,63.9L41.3,63.9L41.3,72.5L18.5,72.5L16.2,72.5L16.2,74.8L16.2,77.8L16.2,80.1L18.5,80.1L71.9,80.1L74.2,80.1L74.2,77.8L74.2,74.8L74.2,72.5L71.9,72.5L48.6,72.5L48.6,63.9L74.4,63.9C78.7,63.9,82.3,60.3,82.3,56L82.3,12.3L87.3,12.3L89.6,12.3L89.6,10L89.6,7.3L89.6,5L87.3,5z" id="407"></path>
            <path transform="matrix(1,0,0,1,2.59,2.3)" fill="#4c4c4c" d="M85,5L43.7,5L43.7,0L41,0L41,5L0,5L0,7.7L7.3,7.7L7.3,53.5C7.3,56.7,9.8,59.2,13,59.2L41,59.2L41,72.5L16.1,72.5L16.1,75.2L69.5,75.2L69.5,72.5L43.7,72.5L43.7,59.2L71.8,59.2C74.9,59.2,77.4,56.6,77.4,53.5L77.4,7.7L85,7.7L85,5zM75.4,53.5C75.4,55.3,73.8,57,72,57L12.9,57C11,57,9.4,55.4,9.4,53.5L9.4,7.8L75.4,7.8L75.4,53.5z" id="408"></path>
            <path transform="matrix(1,0,0,1,38.63,26.83)" fill="#5ac7d5" d="M19.8,3.4C17.8,1.4,15,0.3,12.3,0L11.6,12.9L0,16.1C0.7,17.6,0.9,18.4,1.5,19.1C2.2,19.9,3.2,20.9,4,21.6C6.1,23.2,8.6,23.9,11.1,23.9C12.8,23.9,14.3,23.7,15.7,23C17,22.3,18.4,21.4,19.5,20.5C20.7,19.4,21.5,18,22,16.7C22.7,15,22.9,13.5,22.9,11.9C23.4,8.6,22.1,5.7,19.8,3.4z" id="409"></path>
            <path transform="matrix(1,0,0,1,34.07,22.52)" fill="#fcd486" d="M12.7,0C10,0,8.2,0.5,6.6,1.1C4.9,1.8,3.6,2.8,2.7,4.1C0.9,6.4,-0.2,9.1,0,12.1C0,13.4,0.2,15,0.7,16.4L12.7,14.1L12.7,0z" id="410"></path>
        </g>
        <text class="st1">
            <tspan style="white-space:pre" x="19" y="117.3">MySQL管理规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,228.89,322.17)" ed:parentid="101" id="102" ed:layout="rightmap" ed:width="137.3333300352097" ed:height="89.33333003520966">
        <path fill="#f6f6f6" stroke-linejoin="round" d="M4,0L133.3,0C136,0,137.3,1.3,137.3,4L137.3,85.3C137.3,88,136,89.3,133.3,89.3L4,89.3C1.3,89.3,0,88,0,85.3L0,4C0,1.3,1.3,0,4,0z" stroke-width="1.33333" stroke="#696969"></path>
        <g transform="translate(46.34,8.27)">
            <path transform="matrix(1,0,0,1,0,0)" fill="#ffffff" d="M44.1,12L43.8,10.5L42.3,10.8L40.9,11.1L39.5,11.4L39.8,12.9C42.1,26.7,42.1,42.1,38.6,43.1C37.9,43.2,37.3,43.4,36.8,43.4C36.2,43.4,35.6,43.3,35.3,43C34.3,42.2,34.2,40.2,34.2,39.3C34.2,39,34.6,22,34.7,16.7C34.9,10.3,33.6,6,31,3.3C28.9,0.8,26.4,0,23.6,0C23.4,0,23.3,0,23.1,0C19.8,0.1,17.1,1.1,15.3,3.1C14.3,4.2,13.7,5.4,13.4,6.5L8.5,6.5C3.9,6.6,0,10.6,0,15.2L0,33.2C0,41.4,6.6,48,14.7,48L15.7,48C22.6,48,28.4,43.1,29.9,36.6C29.9,37,29.7,38.8,29.7,38.9C29.7,39.4,29.4,43.9,32.3,46.3C33.5,47.1,35.1,47.6,36.6,47.6C37.6,47.6,38.8,47.5,39.9,47.2C48.6,44.6,45.2,19.7,44.1,12z" id="399"></path>
            <path transform="matrix(1,0,0,1,14.02,1.13)" fill="#4c4c4c" d="M25.6,44.8C24.6,45.1,23.2,45.2,22.4,45.1C16.1,44.4,16.9,37.5,16.9,37.3C16.9,37.3,17.3,20.2,17.4,14.9C17.6,9.3,16.6,5.5,14.5,3.5C13.3,2.2,11.2,1.6,9,1.5C6.9,1.4,4.3,2.3,3.1,3.8C1.3,6,1.8,6.7,1.7,7.1L0,7.1C0.1,6.4,0.5,4.3,2.1,2.7C3.7,1.1,6.2,0,9.5,0C12,0,14.1,1,15.5,2.6C17.8,4.9,19,9.1,18.8,15C18.7,20.4,18.5,37.5,18.5,37.5C18.5,37.5,17.3,42.9,22.6,43.8C23.3,44,24.3,43.8,25.1,43.5C30.9,41.8,28.2,19,26.8,10.7L28.2,10.4C28.8,13.6,33.5,42.4,25.6,44.8z" id="400"></path>
            <path transform="matrix(1,0,0,1,1.18,8.34)" fill="#4c4c4c" d="M13.3,38.7C6,38.7,0,32.6,0,25.3L0,7.3C0,3.4,3.2,0,7.1,0L20.4,0C24.3,0.1,27.5,3.4,27.5,7.3L27.5,25.3C27.5,32.6,21.5,38.7,14.3,38.7L13.3,38.7z" id="401"></path>
            <path transform="matrix(1,0,0,1,2.65,9.65)" fill="#69d9e2" d="M5.9,0L18.8,0C22,0,24.8,2.7,24.8,6.1L24.8,24C24.8,30.7,19.5,36,12.9,36L11.9,36C5.3,36,0,30.7,0,24L0,6.1C0.1,2.7,2.8,0,5.9,0z" id="402"></path>
            <path transform="matrix(1,0,0,1,8.48,9.65)" fill="#fffdfd" d="M0,0L13.1,0L13.1,10.3C13.1,13.8,10.4,16.7,6.8,16.7C2.8,16.7,0,13.9,0,10.3L0,0z" id="403"></path>
            <path transform="matrix(1,0,0,1,14.08,7.74)" fill="#4c4c4c" d="M1,7.9C0.4,7.9,0,7.4,0,7L0,0.8C0,0.4,0.4,0,0.7,0C1.3,0,1.6,0.4,1.6,0.8L1.6,7C1.7,7.4,1.4,7.9,1,7.9z" id="404"></path>
        </g>
        <text class="st2">
            <tspan style="white-space:pre" x="17" y="78.2">数据库文档规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,228.89,789.67)" ed:parentid="101" id="104" ed:layout="rightmap" ed:width="137.3333300352097" ed:height="89.33333003520966">
        <path fill="#f6f6f6" stroke-linejoin="round" d="M4,0L133.3,0C136,0,137.3,1.3,137.3,4L137.3,85.3C137.3,88,136,89.3,133.3,89.3L4,89.3C1.3,89.3,0,88,0,85.3L0,4C0,1.3,1.3,0,4,0z" stroke-width="1.33333" stroke="#696969"></path>
        <g transform="translate(47.51,8.27)">
            <path transform="matrix(1,0,0,1,2.68,2.69)" fill="#ffffff" d="M38,0.5L38,3L32.5,3L32.5,30.2C35.1,31,37,33.4,37,36.3C37,39.7,34,42.6,30.5,42.6C27.5,42.6,24.9,40.5,24.2,37.7L0,37.7L0,35.3L3.4,35.3C2.4,35.1,1.8,34.4,1.8,33.3L1.8,27.4C1.8,26.4,2.5,25.6,3.5,25.3C2.5,25,1.8,24.3,1.8,23.2L1.8,17.4C1.8,16.4,2.5,15.6,3.5,15.3C2.5,15.2,1.8,14.3,1.8,13.2L1.8,7.4C1.8,6.3,2.8,5.3,3.9,5.3L27.9,5.3C28.9,5.3,29.7,6,29.8,6.8L29.8,2.5L29.8,0L38,0" id="433"></path>
            <path transform="matrix(1,0,0,1,0,0)" fill="#ffffff" d="M40.7,0L32.8,0L30,0L30,2.7L30,5.2L6.8,5.2C4.2,5.2,1.9,7.5,1.9,10.1L1.9,15.9C1.9,16.6,2.1,17.3,2.4,18C2.1,18.7,1.9,19.2,1.9,20.1L1.9,25.9C1.9,26.6,2.1,27.3,2.4,28C2.1,28.5,1.9,29.2,1.9,30.1L1.9,35.2L0,35.2L0,38L0,40.3L0,43L2.8,43L25,43C26.5,46.1,29.6,47.9,33.1,48C39.5,48.2,42.3,43.6,42.4,38.8C42.4,35.5,40.5,32.6,37.9,31L37.9,8.3L40.5,8.3L43.3,8.3L43.3,5.6L43.3,3.1L43.3,0L40.7,0z" id="434"></path>
            <path transform="matrix(1,0,0,1,1.38,1.2)" fill="#4c4c4c" d="M40.5,0L29.8,0L29.8,5.2C29.6,5.2,5.3,5.2,5.3,5.2C3.4,5.2,1.8,6.8,1.8,8.7L1.8,14.5C1.8,14.9,2.2,16.4,2.4,16.8C2.1,17.4,1.8,18.1,1.8,18.7L1.8,24.5C1.8,24.9,2.2,26.4,2.4,26.8C2.1,27.4,1.8,28.1,1.8,28.7L1.8,34.5C1.8,34.8,1.8,35.1,1.8,35.2L0,35.2L0,40.9L1.4,40.9L24.6,40.9C25.8,43.8,28.7,45.6,31.9,45.6C36.2,45.6,39.7,42.5,39.7,38.1C39.7,37.5,40.2,33.1,35.2,30.2L35.2,6.2L39.3,6.2L40.5,6.2L40.5,0z" id="435"></path>
            <path transform="matrix(1,0,0,1,2.95,3.2)" fill="#dddddd" d="M37.9,2.5L37.9,0L29.9,0L29.9,2.5L29.9,34.8L0,34.8L0,37.2L32.5,37.2L32.5,34.8L32.3,2.5L37.9,2.5z" id="436"></path>
            <path transform="matrix(1,0,0,1,4.71,28.01)" fill="#ffe48f" d="M26,10L2.1,10C1,10,0,9,0,7.9L0,2.1C0,1,1,0,2.1,0L26,0C27.2,0,28.2,1,28.2,2.1L28.2,7.9C28.2,9.2,27.3,10,26,10z" id="437"></path>
            <path transform="matrix(1,0,0,1,4.71,18.17)" fill="#304352" d="M26,10L2.1,10C1,10,0,9,0,7.9L0,2.1C0,1,1,0,2.1,0L26,0C27.2,0,28.2,1,28.2,2.1L28.2,7.9C28.2,9.1,27.3,10,26,10z" id="438"></path>
            <path transform="matrix(1,0,0,1,4.71,8.38)" fill="#69d9e2" d="M26,10L2.1,10C1,10,0,9,0,7.9L0,2.1C0,1,1,0,2.1,0L26,0C27.2,0,28.2,1,28.2,2.1L28.2,7.9C28.2,9,27.3,10,26,10z" id="439"></path>
            <path transform="matrix(1,0,0,1,26.82,32.49)" fill="#132c2d" d="M12.8,6.3C12.8,9.9,10,12.7,6.4,12.7C2.9,12.7,0,9.9,0,6.3C0,2.8,2.9,0,6.4,0C10,0,12.8,2.8,12.8,6.3z" id="440"></path>
            <path transform="matrix(1,0,0,1,29.88,35.52)" fill="#dddddd" d="M6.7,3.3C6.7,5.2,5.2,6.6,3.4,6.6C1.5,6.6,0,5.2,0,3.3C0,1.5,1.5,0,3.4,0C5.2,0,6.7,1.5,6.7,3.3z" id="441"></path>
        </g>
        <text class="st3">
            <tspan style="white-space:pre" x="17" class="st4" y="78.2">数</tspan>
            <tspan style="white-space:pre" x="31" class="st4">据库应用环境</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,228.89,1101.96)" ed:parentid="101" id="106" ed:layout="rightmap" ed:width="137.3333300352097" ed:height="89.33333003520966">
        <path fill="#f6f6f6" stroke-linejoin="round" d="M4,0L133.3,0C136,0,137.3,1.3,137.3,4L137.3,85.3C137.3,88,136,89.3,133.3,89.3L4,89.3C1.3,89.3,0,88,0,85.3L0,4C0,1.3,1.3,0,4,0z" stroke-width="1.33333" stroke="#696969"></path>
        <g transform="translate(48.44,8.27)">
            <path transform="matrix(1,0,0,1,0,0)" fill="#ffffff" d="M40.6,39.6L33.3,30.2C35.9,26.9,37.4,23,37.4,18.6C37.2,8.3,28.8,0,18.6,0C8.3,0,0,8.3,0,18.5C0,28.7,8.3,37,18.6,37C20.8,37,23.2,36.6,25.3,35.8L31.8,45.5C32.4,46.3,34.2,48,36.6,48C37.7,48,38.7,47.6,39.7,47C42.3,44.9,41.5,41.3,40.6,39.6z" id="443"></path>
            <path transform="matrix(1,0,0,1,1.37,1.35)" fill="#4c4c4c" d="M38.1,38.8L30.2,28.7C33.1,25.6,34.7,21.5,34.7,17.2C34.6,7.6,26.8,0,17.3,0C7.8,0,0,7.7,0,17.2C0,26.6,7.8,34.3,17.3,34.3C19.8,34.3,22.2,33.8,24.4,32.8L31.4,43.4C31.7,43.7,33.3,45.3,35.3,45.3C36.1,45.3,36.8,45,37.4,44.5C39.4,43,38.7,40.2,38.1,38.8z" id="444"></path>
            <path transform="matrix(1,0,0,1,23.89,25.93)" fill="#69d9e2" d="M0,2.8L10.3,18.1C10.3,18.1,12.5,20.3,14.2,18.9C15.9,17.5,14.5,14.7,14.5,14.7L2.8,0L0,2.8z" id="446"></path>
            <path transform="matrix(1,0,0,1,2.94,2.76)" fill="#69d9e2" d="M0,15.8C0,7.1,7.1,0,15.9,0C24.8,0,31.9,7.1,31.9,15.8C31.9,24.6,24.8,31.7,15.9,31.7C7.1,31.7,0,24.6,0,15.8z" id="447"></path>
            <path transform="matrix(1,0,0,1,6.22,6.07)" fill="#ffffff" d="M0,12.4C0,5.6,5.7,0,12.5,0C19.5,0,25.2,5.6,25.2,12.4C25.2,19.4,19.5,25,12.5,25C5.7,24.9,0,19.3,0,12.4z" id="448"></path>
        </g>
        <text class="st2">
            <tspan style="white-space:pre" x="17" y="78.2">数</tspan>
            <tspan style="white-space:pre" x="31">据库技术规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,228.89,1534.5)" ed:parentid="101" id="108" ed:layout="rightmap" ed:width="123.3333300352097" ed:height="89.33333003520966">
        <path fill="#f6f6f6" stroke-linejoin="round" d="M4,0L119.3,0C122,0,123.3,1.3,123.3,4L123.3,85.3C123.3,88,122,89.3,119.3,89.3L4,89.3C1.3,89.3,0,88,0,85.3L0,4C0,1.3,1.3,0,4,0z" stroke-width="1.33333" stroke="#696969"></path>
        <g transform="translate(38.17,8.27)">
            <path transform="matrix(1,0,0,1,0,0)" fill="#ffffff" d="M24,0C10.8,0,0,10.8,0,24C0,37.2,10.8,48,24,48C37.2,48,48,37.2,48,24C48,10.8,37.2,0,24,0z" id="413"></path>
            <path transform="matrix(1,0,0,1,1.02,1.02)" fill="#4c4c4c" d="M23,1C35.2,1,44.9,10.9,44.9,23C44.9,35.2,35,44.9,23,44.9C10.9,44.9,1,35,1,23C1,10.9,10.8,1,23,1zM23,0C10.3,0,0,10.3,0,23C0,35.7,10.3,46,23,46C35.7,46,46,35.7,46,23C46,10.3,35.7,0,23,0z" id="414"></path>
            <path transform="matrix(1,0,0,1,5.56,5.56)" fill="#ffe48f" d="M18.4,0C8.2,0,0,8.3,0,18.4C0,28.6,8.3,36.9,18.4,36.9C28.6,36.9,36.9,28.6,36.9,18.4C36.9,8.2,28.6,0,18.4,0z" id="415"></path>
            <path transform="matrix(1,0,0,1,15.12,16.35)" fill="#304352" d="M13.6,0L10.1,5.4C9.1,7.1,8.7,7.6,8.2,8.5L6.4,5.4L2.9,0L0,0L5.9,9.1L2,9.1L2,11.1L6.9,11.1L2,11.9L2,13.9L6.9,13.9L6.9,18.2L9.3,18.2L9.3,13.9L14.1,13.9L14.1,11.9L9.3,11.9L14.1,11.1L14.1,9.1L10.2,9.1L16.1,0L13.6,0z" id="416"></path>
        </g>
        <text class="st2">
            <tspan style="white-space:pre" x="17" y="78.2">团队管理规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,228.89,1941.75)" ed:parentid="101" id="114" ed:layout="rightmap" ed:width="123.3333300352097" ed:height="77.33333003520966">
        <path fill="#f6f6f6" stroke-linejoin="round" d="M4,0L119.3,0C122,0,123.3,1.3,123.3,4L123.3,73.3C123.3,76,122,77.3,119.3,77.3L4,77.3C1.3,77.3,0,76,0,73.3L0,4C0,1.3,1.3,0,4,0z" stroke-width="1.33333" stroke="#696969"></path>
        <g transform="translate(38.17,8.27)">
            <path transform="matrix(1,0,0,1,0,0)" fill="#ffffff" d="M43.6,19C42.8,18.4,42,17.9,41.1,17.5C43,16.2,44.3,14,44.4,11.5C44.5,7.3,41.3,3.7,37,3.6C36.9,3.6,36.7,3.6,36.7,3.6C35.3,3.6,34,3.9,32.8,4.6C31.2,1.9,28.3,0.2,25.2,0.1C24.8,0,24.7,0,24.5,0C20.8,0,17.5,2.2,16.1,5.5C14.9,4.6,13.5,4.1,11.9,4.1C11.8,4.1,11.7,4.1,11.7,4.1C7.5,4.1,4.1,7.3,4,11.5C4,14.1,5.2,16.4,7.2,17.8C6.3,18.3,5.5,18.7,4.7,19.3C1.7,21.2,0,24,0,27.1C0,27.3,0,27.3,0,27.4L0,32.5L0,33.7L1.2,33.7L10.5,33.7L10.5,34.8L10.5,36L11.7,36L37.3,36L38.5,36L38.5,34.8L38.5,33.2L46.8,33.2L48,33.2L48,32L48,26.9C48,23.7,46.3,20.8,43.6,19zM29.6,16.8C30,16.6,30.5,16.3,30.8,16C31.2,16.5,31.7,16.9,32.2,17.3C31.9,17.4,31.6,17.5,31.3,17.7C30.8,17.4,30.2,17.1,29.6,16.8zM16.2,17.8C17.1,17.2,17.8,16.5,18.3,15.6C18.7,16,19.2,16.4,19.7,16.7C18.7,17.1,17.7,17.6,16.8,18.2C16.6,18.1,16.4,17.9,16.2,17.8z" id="418"></path>
            <path transform="matrix(1,0,0,1,30.57,5.68)" fill="#f7d77c" d="M6.8,11.6C6.7,11.6,6.6,11.6,6.5,11.6C6.2,11.6,6.1,11.6,5.9,11.6C2.1,11.2,0,8.6,0,5.6C0.1,2.5,2.6,0,5.9,0C7.7,0,9.1,0.7,10.2,1.9C11.3,3.1,11.9,4.5,11.8,6.1C11.6,8.7,9.6,11.2,6.8,11.6z" id="419"></path>
            <path transform="matrix(1,0,0,1,29.96,5.13)" fill="#4c4c4c" d="M6.5,1.2C6.6,1.2,6.6,1.2,6.7,1.2C8.1,1.2,9.5,1.8,10.5,2.9C11.4,3.9,11.9,5.2,11.9,6.6C11.8,9.1,10,11.1,7.6,11.6C7.5,11.6,7.3,11.6,7.2,11.6C7,11.6,6.9,11.6,6.6,11.6C3.2,11.2,1.3,8.8,1.3,6.4C1.3,3.4,3.6,1.2,6.5,1.2zM6.5,0C3,0,0.1,2.8,0,6.2C0,9.4,2.3,12.2,5.5,12.8C5.9,12.8,6.1,12.8,6.5,12.8C6.8,12.8,7.2,12.8,7.4,12.8C10.6,12.3,12.8,9.8,12.9,6.7C13,3.1,10.3,0.1,6.7,0C6.6,0,6.6,0,6.5,0z" id="420"></path>
            <path transform="matrix(1,0,0,1,5.86,6.18)" fill="#f7d77c" d="M6.8,11.6C6.7,11.6,6.6,11.6,6.5,11.6C6.2,11.6,6.1,11.6,5.9,11.6C2.1,11.2,0,8.6,0,5.6C0.1,2.5,2.6,0,5.9,0C9.3,0.1,11.9,2.9,11.8,6.1C11.6,8.8,9.6,11.1,6.8,11.6z" id="421"></path>
            <path transform="matrix(1,0,0,1,5.25,5.63)" fill="#4c4c4c" d="M6.5,1.2C6.6,1.2,6.6,1.2,6.7,1.2C8.1,1.2,9.5,1.8,10.5,2.8C11.4,3.9,11.9,5.2,11.9,6.5C11.8,9.1,10,11.1,7.6,11.6C7.5,11.6,7.3,11.6,7.2,11.6C7,11.6,6.9,11.6,6.6,11.6C3.1,11.1,1.2,8.8,1.2,6.3C1.3,3.4,3.6,1.2,6.5,1.2zM6.5,0C3,0,0.1,2.8,0,6.2C0,9.4,2.3,12.2,5.5,12.8C5.9,12.8,6.1,12.8,6.5,12.8C6.8,12.8,7.2,12.8,7.4,12.8C10.6,12.3,12.8,9.8,12.9,6.7C13,3.1,10.3,0.1,6.7,0C6.6,0,6.6,0,6.5,0z" id="422"></path>
            <path transform="matrix(1,0,0,1,17.13,2.11)" fill="#f7d77c" d="M8.7,14.8C8.6,14.8,8.3,14.8,8.2,14.8C8,14.8,7.8,14.8,7.5,14.8L6.3,14.8C2.7,14.2,0,10.9,0,7.2C0.1,3.2,3.4,0,7.5,0C7.5,0,11.6,0.9,13.1,2.3C14.5,3.8,15.2,5.7,15.1,7.7C14.9,11.3,12.2,14.3,8.7,14.8z" id="423"></path>
            <path transform="matrix(1,0,0,1,16.42,1.21)" fill="#4c4c4c" d="M8.1,1.4C8.2,1.4,8.2,1.4,8.4,1.4C10.3,1.5,12,2.3,13.2,3.6C14.5,5,15.2,6.7,15.1,8.5C15,11.9,12.5,14.5,9.3,15C9.2,15,8.9,15,8.8,15C8.6,15,8.4,15,8.1,15L7,15C3.7,14.4,1.2,11.5,1.2,8C1.4,4.4,4.4,1.4,8.1,1.4zM0,8C0,12,3,16.1,8.1,16.2C12.9,16.2,16.2,12.4,16.3,8.4C16.4,3.9,12.9,0.3,8.4,0C3.6,-0.1,0.2,3.6,0,8z" id="424"></path>
            <path transform="matrix(1,0,0,1,1.87,21.37)" fill="#69d9e2" d="M40.8,0L35,7.1C34.7,4.4,32.9,1.8,30.3,0L29.4,1.3L24.9,6.7L23,9L19.3,4.9L15.7,1C12.3,2,10.8,4.5,10.6,7.2L3.5,0.8C1.3,2.4,0,4.5,0,6.9L0,11.6L10.6,11.6L10.6,13.9L35,13.8L35,11L44.4,11L44.4,6.2C44.4,3.8,43.1,1.6,40.8,0z" id="425"></path>
            <path transform="matrix(1,0,0,1,1.31,17.5)" fill="#4c4c4c" d="M41.6,2.8C39.9,1.3,37.6,0.5,35.1,0.5C33.2,0.5,31.4,1,29.9,1.9C28,0.7,25.7,0,23.2,0C20.4,0,17.8,0.9,15.7,2.5C14.1,1.6,12.3,1.1,10.4,1.1C8.1,1.1,6,1.8,4.2,3.1C1.7,4.6,0,7.1,0,9.9L0,15.3L10.6,15.3L10.6,17.6L36.2,17.6L36.2,14.8L45.6,14.8L45.6,9.7C45.6,9.6,45.6,9.4,45.6,9.4C45.6,6.7,44,4.3,41.6,2.8zM35.1,1.6C37.1,1.6,38.9,2.2,40.4,3.3L35.9,8.8C35.2,6.3,33.5,4.1,31.1,2.6C32.3,2,33.7,1.6,35.1,1.6zM16.8,3.2C18.6,1.9,20.9,1,23.2,1.1C26.8,1.4,29.7,3,29.9,3.2C29.9,3.3,23.5,11.2,23.5,11.2L20.5,7.7L16.5,3.3C16.6,3.3,16.7,3.3,16.8,3.2zM10.7,10L5.1,4C4.7,4.2,4.4,4.5,4.1,4.8L10.6,11.9L10.6,14.1L1.2,14.1C1.2,14.1,0.3,6.6,4,4.6C4.4,4.5,4.7,4.2,5.1,4C6.6,2.9,8.4,2.2,10.4,2.2C11.9,2.2,13.4,2.6,14.7,3.3C14.7,3.3,14.6,3.4,14.5,3.4C12.4,5.2,11,7.4,10.7,10zM44.3,13.5L34.9,13.5L34.9,16.5L11.8,16.5L11.8,11.4C11.8,11.4,11.7,8.9,13.9,6C13.9,6,15.4,4.3,15.5,4.1L23.5,13.1L30.9,4C32.7,5.3,34,6.9,34.5,8.7C34.6,9,34.7,9.3,34.7,9.5C34.8,9.9,34.9,13.5,34.9,13.5L36.2,13.5C36.2,13.5,36.2,10.7,36.1,10.5C36.1,10.5,36.1,10.5,36.1,10.5L36.8,9.5L41.3,4C42.8,5.4,43.8,7.3,44.1,9.5L44.3,10.5L44.3,13.5z" id="430"></path>
        </g>
        <text class="st2">
            <tspan style="white-space:pre" x="17" y="66.2">事件升级规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,228.89,1819.42)" ed:parentid="101" id="116" ed:layout="rightmap" ed:width="123.3333300352097" ed:height="89.33333003520966">
        <path fill="#f6f6f6" stroke-linejoin="round" d="M4,0L119.3,0C122,0,123.3,1.3,123.3,4L123.3,85.3C123.3,88,122,89.3,119.3,89.3L4,89.3C1.3,89.3,0,88,0,85.3L0,4C0,1.3,1.3,0,4,0z" stroke-width="1.33333" stroke="#696969"></path>
        <g transform="translate(38.68,8.27)">
            <path transform="matrix(1,0,0,1,0,0)" fill="#fffdfd" d="M45.7,17.8L25.7,1C25.3,0.4,24.4,0,23.6,0C22.7,0,22,0.3,21.4,0.9L1.3,17.7C0.1,18.6,-0.3,20.1,0.2,21.6C0.7,23,2.1,23.9,3.5,23.9L4.6,23.9L4.6,43.7C4.6,46.1,6.5,48,8.8,48L38.2,48C40.6,48,42.4,46,42.4,43.7L42.4,24L43.5,24C44.9,24,46.3,23,46.8,21.7C47.2,20.2,46.9,18.7,45.7,17.8z" id="465"></path>
            <path transform="matrix(1,0,0,1,1.25,1.24)" fill="#4c4c4c" d="M22.3,1.2C22.6,1.2,22.8,1.3,23,1.5L43.1,18.3C43.8,18.9,43.4,20.2,42.4,20.2L38.8,20.2L38.8,42.5C38.8,43.6,38,44.4,37,44.4L7.6,44.4C6.5,44.4,5.8,43.5,5.8,42.5L5.8,20.3L2.2,20.3C1.3,20.3,0.8,19.1,1.5,18.4L21.6,1.6C21.8,1.3,22.1,1.2,22.3,1.2zM22.3,0C21.8,0,21.2,0.3,20.9,0.5L0.8,17.2C0.1,17.9,-0.2,18.8,0.1,19.8C0.5,20.8,1.3,21.4,2.3,21.4L4.6,21.4L4.6,42.5C4.6,44.2,5.9,45.5,7.6,45.5L37,45.5C38.7,45.5,40,44.2,40,42.5L40,21.5L42.3,21.5C43.3,21.5,44.1,20.9,44.5,19.9C44.9,18.9,44.5,18,43.8,17.3L23.7,0.6C23.4,0.3,22.9,0,22.3,0z" id="466"></path>
            <path transform="matrix(1,0,0,1,14.45,27.72)" fill="#304352" d="M1.1,10L6.9,10C7.5,10,8,10.4,8,11L8,16.9C8,17.3,7.5,17.9,6.9,17.9L1.1,17.9C0.5,17.9,0,17.4,0,16.9L0,11C0.2,10.5,0.6,10,1.1,10zM5.2,1.1L5.2,7C5.2,7.6,5.7,8.1,6.3,8.1L12.1,8.1C12.7,8.1,13.2,7.6,13.2,7L13.2,1.1C13.2,0.5,12.7,0,12.1,0L6.3,0C5.7,0.1,5.2,0.6,5.2,1.1zM10.4,11L10.4,16.9C10.4,17.3,10.8,17.9,11.4,17.9L17.2,17.9C17.7,17.9,18.2,17.4,18.2,16.9L18.2,11C18.2,10.4,17.8,10,17.2,10L11.4,10C10.8,10,10.4,10.5,10.4,11z" id="467"></path>
            <path transform="matrix(1,0,0,1,2.41,2.47)" fill="#69d9e2" d="M20.4,0.3L0.4,17.3C-0.3,17.9,0,19.3,1.1,19.3L41.3,19.3C42.3,19.3,42.7,18,42,17.3L22,0.3C21.4,-0.1,20.8,-0.1,20.4,0.3z" id="468"></path>
        </g>
        <text class="st2">
            <tspan style="white-space:pre" x="17" y="78.2">团队培训规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,393.22,109.58)" ed:parentid="102" id="120" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">业务明细</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,393.22,460.33)" ed:parentid="102" id="122" ed:layout="rightmap" ed:width="116" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L116,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">数据库维护记录</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,494.22,64.46)" ed:parentid="120" id="126" ed:layout="rightmap" ed:width="46" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L46,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">业务</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,567.22,79.5)" ed:parentid="126" id="128" ed:layout="rightmap" ed:width="88" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L88,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">业务责任人</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,494.22,169.75)" ed:parentid="120" id="142" ed:layout="rightmap" ed:width="88" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L88,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">业务数据库</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,637.22,430.25)" ed:parentid="474" id="144" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">二级目录</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,637.22,244.96)" ed:parentid="474" id="145" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">一级目录</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,536.22,645.62)" ed:parentid="122" id="146" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">文档模板</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,393.22,777.42)" ed:parentid="104" id="156" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">环</tspan>
            <tspan style="white-space:pre" x="22">境划分</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,494.22,807.5)" ed:parentid="156" id="158" ed:layout="rightmap" ed:width="73.203125" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L73.2,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">开发DEV</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,494.22,777.42)" ed:parentid="156" id="160" ed:layout="rightmap" ed:width="74.1875" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74.2,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">测试UAT</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,494.22,747.33)" ed:parentid="156" id="162" ed:layout="rightmap" ed:width="73.0625" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L73.1,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">生产PRD</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,494.22,837.58)" ed:parentid="164" id="163" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">实</tspan>
            <tspan style="white-space:pre" x="22">例列表</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,393.22,867.67)" ed:parentid="104" id="164" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">实</tspan>
            <tspan style="white-space:pre" x="22">例列表</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,494.22,897.75)" ed:parentid="164" id="166" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">应</tspan>
            <tspan style="white-space:pre" x="22">用明细</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,494.22,867.67)" ed:parentid="164" id="168" ed:layout="rightmap" ed:width="88" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L88,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">服</tspan>
            <tspan style="white-space:pre" x="22">务器明细</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,379.22,1432)" ed:parentid="108" id="204" ed:layout="rightmap" ed:width="102" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L102,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">工作记录规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,379.22,1537.21)" ed:parentid="108" id="212" ed:layout="rightmap" ed:width="102" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L102,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">开发项目规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,696.22,169.75)" ed:parentid="445" id="389" ed:layout="rightmap" ed:width="88" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L88,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">第一责任人</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,696.22,199.83)" ed:parentid="445" id="391" ed:layout="rightmap" ed:width="88" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L88,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">第二责任人</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,567.22,19.33)" ed:parentid="126" id="393" ed:layout="rightmap" ed:width="46" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L46,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">简介</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,567.22,49.42)" ed:parentid="126" id="395" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">业务架构</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,567.22,109.58)" ed:parentid="126" id="397" ed:layout="rightmap" ed:width="102" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L102,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">业务团队成员</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,609.22,139.67)" ed:parentid="142" id="411" ed:layout="rightmap" ed:width="88" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L88,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">数据库架构</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,609.22,184.79)" ed:parentid="142" id="445" ed:layout="rightmap" ed:width="60" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L60,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">责任人</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,536.22,400.17)" ed:parentid="122" id="474" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">目录规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,738.22,229.92)" ed:parentid="145" id="476" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">命令规则</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,738.22,290.08)" ed:parentid="144" id="478" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st5">
            <tspan style="white-space:pre" x="8" class="st6" y="17.6">命令规则</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,738.22,320.17)" ed:parentid="144" id="480" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">存放内容</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,839.22,320.17)" ed:parentid="480" id="482" ed:layout="rightmap" ed:width="144" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L144,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">事件的输出文档明细</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,839.22,290.08)" ed:parentid="478" id="484" ed:layout="rightmap" ed:width="350.234375" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L350.2,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">yy.mm.dd.N.事件名称_事件类别_${事件简述}${后缀}</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,738.22,370.25)" ed:parentid="144" id="486" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">事件类别</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,738.22,570.42)" ed:parentid="144" id="488" ed:layout="rightmap" ed:width="46" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L46,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">后缀</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,839.22,350.25)" ed:parentid="486" id="490" ed:layout="rightmap" ed:width="514" ed:height="63.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,63.6L514,63.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">产品咨询、语法故障、CPU故障、内存故障、IO故障、磁盘故障、主从故障、锁</tspan>
            <tspan style="white-space:pre" x="8" y="37.6">故障、连接故障、密码破解、数据丢失、安装配置、备份恢复、架构设计、SQL</tspan>
            <tspan style="white-space:pre" x="8" y="57.6">优化、迁移实施、数据库升级、SQL追踪、数据库宕机、非数据库故障</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,811.22,570.42)" ed:parentid="488" id="492" ed:layout="rightmap" ed:width="481.3125" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L481.3,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">迁移【报告/方案/跟进】、实施【报告/方案/跟进】、排查报告、优化建议</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,738.22,480.33)" ed:parentid="144" id="494" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">事件简述</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,839.22,420.33)" ed:parentid="494" id="496" ed:layout="rightmap" ed:width="109.328125" ed:height="143.5833325088024">
        <path stroke-linejoin="round" fill="none" d="M0,143.6L109.3,143.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st5">
            <tspan style="white-space:pre" x="8" class="st7" y="17.6">前提/前情概要 </tspan>
            <tspan style="white-space:pre" x="8" class="st7" y="37.6">需求说明 </tspan>
            <tspan style="white-space:pre" x="8" class="st7" y="57.6">XX方案 </tspan>
            <tspan style="white-space:pre" x="8" class="st7" y="77.6">XX步骤 </tspan>
            <tspan style="white-space:pre" x="8" class="st7" y="97.6">XX计划 </tspan>
            <tspan style="white-space:pre" x="8" class="st7" y="117.6">XX总结 </tspan>
            <tspan style="white-space:pre" x="8" class="st7" y="137.6">后续事项</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,637.22,600.5)" ed:parentid="146" id="502" ed:layout="rightmap" ed:width="102" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L102,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">迁移报告模板</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,637.22,630.58)" ed:parentid="146" id="504" ed:layout="rightmap" ed:width="102" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L102,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">迁移方案模板</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,637.22,660.67)" ed:parentid="146" id="506" ed:layout="rightmap" ed:width="102" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L102,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">排查报告模板</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,637.22,690.75)" ed:parentid="146" id="508" ed:layout="rightmap" ed:width="102" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L102,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">优化建议模板</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,738.22,260)" ed:parentid="145" id="510" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">存放内容</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,839.22,229.92)" ed:parentid="476" id="512" ed:layout="rightmap" ed:width="200" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L200,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">业务类型_业务编号_业务名称</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,839.22,260)" ed:parentid="510" id="514" ed:layout="rightmap" ed:width="200" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L200,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">存放该业务有输出沉淀的事件</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,393.22,984.42)" ed:parentid="106" id="538" ed:layout="rightmap" ed:width="144" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L144,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">数据库安装部署规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,393.22,1014.5)" ed:parentid="106" id="540" ed:layout="rightmap" ed:width="116" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L116,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">数据库备份规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,393.22,1044.58)" ed:parentid="106" id="542" ed:layout="rightmap" ed:width="116" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L116,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">数据库安全规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,393.22,1074.67)" ed:parentid="106" id="544" ed:layout="rightmap" ed:width="116" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L116,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">数据库归档规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,393.22,1104.75)" ed:parentid="106" id="546" ed:layout="rightmap" ed:width="116" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L116,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">数据库监控规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,393.22,1149.87)" ed:parentid="106" id="548" ed:layout="rightmap" ed:width="158" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L158,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">数据库自动化运维规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,393.22,1240.12)" ed:parentid="106" id="550" ed:layout="rightmap" ed:width="116" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L116,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">数据库迁移规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,393.22,1315.33)" ed:parentid="106" id="552" ed:layout="rightmap" ed:width="116" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L116,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">数据库升级规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,393.22,954.33)" ed:parentid="106" id="554" ed:layout="rightmap" ed:width="144" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L144,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">业务数据库对接规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,536.22,1195)" ed:parentid="550" id="556" ed:layout="rightmap" ed:width="102" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L102,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">迁移传输速率</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,536.22,1225.08)" ed:parentid="550" id="558" ed:layout="rightmap" ed:width="116" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L116,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">数据库迁移方案</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,536.22,1255.17)" ed:parentid="550" id="560" ed:layout="rightmap" ed:width="116" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L116,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">数据库迁移案例</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,536.22,1285.25)" ed:parentid="550" id="562" ed:layout="rightmap" ed:width="116" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L116,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">数据库上云规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,678.22,1237.92)" ed:parentid="576" id="577" ed:layout="rightmap" ed:width="97.31999999999999" ed:height="28">
        <path fill="#ffffff" stroke-linejoin="round" d="M14,0L83.3,0C91.1,0,97.3,6.3,97.3,14C97.3,21.7,91.1,28,83.3,28L14,28C6.3,28,0,21.7,0,14C0,6.3,6.3,0,14,0z" stroke="#b9947b"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="12" y="20">同构和异构</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,578.22,1134.83)" ed:parentid="548" id="578" ed:layout="rightmap" ed:width="116" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L116,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">自动化运维工具</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,578.22,1164.92)" ed:parentid="548" id="580" ed:layout="rightmap" ed:width="116" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L116,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">自动化运维平台</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,564.22,954.33)" ed:parentid="554" id="584" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">标签规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,508.22,1371.92)" ed:parentid="204" id="586" ed:layout="rightmap" ed:width="514" ed:height="43.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,43.6L514,43.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">每个同学一周工作记录（以DBA为维度记录的事实，保存在jira中，任务规范命</tspan>
            <tspan style="white-space:pre" x="8" y="37.6">名：DBA_花名_工作记录）</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,508.22,1422)" ed:parentid="204" id="588" ed:layout="rightmap" ed:width="514" ed:height="43.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,43.6L514,43.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">一周按照客户（事件、项目）维度统计（项目类记录在jira中，任务规范命名：</tspan>
            <tspan style="white-space:pre" x="8" y="37.6">DBA_客户名_项目名_跟进记录）</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,508.22,1472.08)" ed:parentid="204" id="590" ed:layout="rightmap" ed:width="514" ed:height="43.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,43.6L514,43.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">内部的规划好的事件进展（记录在jira中，任务规范命名：DBA_内部任务_【具体</tspan>
            <tspan style="white-space:pre" x="8" y="37.6">任务】）</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,508.22,1522.17)" ed:parentid="212" id="592" ed:layout="rightmap" ed:width="429.78125" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L429.8,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">创建对应项目的面版，项目的对应PM制定任务命名、标签等规则；</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,508.22,1552.25)" ed:parentid="212" id="594" ed:layout="rightmap" ed:width="177.78125" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L177.8,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">项目中的报告人为对应PM</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,379.22,1672.58)" ed:parentid="108" id="596" ed:layout="rightmap" ed:width="102" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L102,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">每周例会规范</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,508.22,1582.33)" ed:parentid="596" id="600" ed:layout="rightmap" ed:width="203.6875" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L203.7,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">例会时间：周五下午4点～6点</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,508.22,1612.42)" ed:parentid="596" id="602" ed:layout="rightmap" ed:width="200.1875" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L200.2,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">例会主持：DBA小组成员轮询</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,508.22,1642.5)" ed:parentid="596" id="604" ed:layout="rightmap" ed:width="477.796875" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L477.8,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">会议纪要：主持人负责会议纪要并邮件同步小组成员，最迟周六23:59发出</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,508.22,1717.71)" ed:parentid="596" id="606" ed:layout="rightmap" ed:width="74" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L74,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">周会议程</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,609.22,1672.58)" ed:parentid="606" id="608" ed:layout="rightmap" ed:width="186" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L186,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">小组成员一周工作内容简报</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,609.22,1702.67)" ed:parentid="606" id="610" ed:layout="rightmap" ed:width="214" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L214,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">业务项目进展以及后续计划汇报</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,609.22,1732.75)" ed:parentid="606" id="612" ed:layout="rightmap" ed:width="242" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L242,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">内部开发项目进展以及后续计划汇报</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,609.22,1762.83)" ed:parentid="606" id="614" ed:layout="rightmap" ed:width="144" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L144,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">技术分享和吐槽环节</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,379.22,1822.21)" ed:parentid="116" id="616" ed:layout="rightmap" ed:width="102" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L102,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">新人入职培训</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,379.22,1852.29)" ed:parentid="116" id="618" ed:layout="rightmap" ed:width="102" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L102,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">日常技能培训</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,379.22,1882.37)" ed:parentid="116" id="620" ed:layout="rightmap" ed:width="116" ed:height="23.58333250880241">
        <path stroke-linejoin="round" fill="none" d="M0,23.6L116,23.6" stroke-width="1.33333" stroke="#696969"></path>
        <text class="st2">
            <tspan style="white-space:pre" x="8" y="17.6">新技术学习培训</tspan>
        </text>
    </g>
</svg>
</div>
        <div id="copyright">Created by <a href="https://www.toberoot.com/" target="_blank"
                                          title="edrawsoft">BoobooWei</a></div>
    </div>
</div>
<script>
    eval(atob('dmFyIG11YT13aW5kb3cubmF2aWdhdG9yLnVzZXJBZ2VudDsNCnZhciB1YT0obXVhLmluZGV4T2YoJ3J2OjExJykrbXVhLmluZGV4T2YoJ01TSUUnKSk+PTA7DQpkb2N1bWVudC5nZXRFbGVtZW50QnlJZCgnc3ZnLWNvbnRhaW5lcicpLm9uY29udGV4dG1lbnUgPSBmdW5jdGlvbiAoZXZlbnQpIHsNCiAgICBldmVudC5wcmV2ZW50RGVmYXVsdCgpOw0KfQ0KZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoJ3N2Zy1jb250YWluZXInKS5vbm1vdXNlZG93biA9IGZ1bmN0aW9uIChldmVudCkgew0KICAgIGlmKGV2ZW50LndoaWNoID09Myl7DQogICAgICAgIHRoaXMuc3R5bGUuY3Vyc29yID0gJ3BvaW50ZXInOw0KICAgICAgICB0aGlzLm9ubW91c2Vtb3ZlID0gZnVuY3Rpb24gKGV2KSB7DQogICAgICAgICAgICB0aGlzLnNjcm9sbEJ5KC0oZXYubW92ZW1lbnRYKSwgMCk7DQogICAgICAgICAgICB3aW5kb3cuc2Nyb2xsQnkoMCwtKGV2Lm1vdmVtZW50WSkpDQogICAgICAgIH0NCiAgICAgICAgdGhpcy5vbm1vdXNldXAgPSBmdW5jdGlvbiAoKSB7DQogICAgICAgICAgICB0aGlzLnN0eWxlLmN1cnNvciA9ICBudWxsOw0KICAgICAgICAgICAgdGhpcy5vbm1vdXNldXAgPSBudWxsOw0KICAgICAgICAgICAgdGhpcy5vbm1vdXNlbW92ZSA9IG51bGw7DQogICAgICAgIH0NCiAgICB9DQp9DQpOdW1iZXIucHJvdG90eXBlLnRvc3VpdHN2Zz1mdW5jdGlvbiAoKSB7DQogICAgdmFyIG51bT10aGlzLnZhbHVlT2YoKTsNCiAgICBpZihudW0lMT09PTApew0KICAgICAgICByZXR1cm4gbnVtKzAuNQ0KICAgIH1lbHNlIHJldHVybiBudW07DQp9Ow0KTnVtYmVyLnByb3RvdHlwZS5wbHVzej1mdW5jdGlvbigpIHsNCiAgICB2YXIgbnVtPXRoaXMudmFsdWVPZigpOw0KICAgIHJldHVybiBudW08MTA/JzAnK251bTpudW07DQp9Ow0KZnVuY3Rpb24gcGFyc2VEYXRlKG51bSkgew0KICAgIHZhciBkYXRlID0gbmV3IERhdGUobnVtKTsNCiAgICB2YXIgWSA9IGRhdGUuZ2V0RnVsbFllYXIoKSArICctJzsNCiAgICB2YXIgTSA9IChkYXRlLmdldE1vbnRoKCkrMSkucGx1c3ooKSArICctJzsNCiAgICB2YXIgRCA9IGRhdGUuZ2V0RGF0ZSgpLnBsdXN6KCkgKyAnICc7DQogICAgdmFyIGggPSBkYXRlLmdldEhvdXJzKCkucGx1c3ooKSArICc6JzsNCiAgICB2YXIgbW0gPSBkYXRlLmdldE1pbnV0ZXMoKS5wbHVzeigpICsgJzonOw0KICAgIHZhciBzID0gZGF0ZS5nZXRTZWNvbmRzKCkucGx1c3ooKTsNCiAgICByZXR1cm4gWStNK0QraCttbStzOw0KfQ0KLy8tLXByZWRlZmluZWQNCi8vY29tbWVudC0tDQp2YXIgY29tbWVudHM9ZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgnZz5nW2VkXFw6Y29tbWVudF0nKTsNCmZ1bmN0aW9uIGdldGN3aChwb3B1cCkgew0KICAgIGRvY3VtZW50LmJvZHkuZ2V0RWxlbWVudHNCeVRhZ05hbWUoJ3N2ZycpWzBdLmFwcGVuZENoaWxkKHBvcHVwKTsNCiAgICB2YXIgdz1wb3B1cC5nZXRCb3VuZGluZ0NsaWVudFJlY3QoKS53aWR0aDsNCiAgICB2YXIgaD1wb3B1cC5nZXRCb3VuZGluZ0NsaWVudFJlY3QoKS5oZWlnaHQ7DQogICAgcmV0dXJuIFt3LGhdDQp9DQpmb3IodmFyIGk9MDtpPGNvbW1lbnRzLmxlbmd0aDtpKyspew0KICAgIHZhciBwb3B1cD1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywnZycpOw0KICAgIHZhciBwb3B1cFI9IGRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgdmFyIGhvdmVyPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgdmFyIG9saW5lPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCdmaWxsJywnI2NkY2RmZicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgneCcsJzAnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3knLCcwJyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCdoZWlnaHQnLCcxNicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnd2lkdGgnLCcxNicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbC1vcGFjaXR5JywnMC42Jyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nLGNvbW1lbnRzW2ldLnF1ZXJ5U2VsZWN0b3IoJ3VzZScpLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJykpOw0KICAgIGhvdmVyLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIGNvbW1lbnRzW2ldLmFwcGVuZENoaWxkKGhvdmVyKTsNCiAgICB2YXIgYT1KU09OLnBhcnNlKGNvbW1lbnRzW2ldLmdldEF0dHJpYnV0ZSgnZWQ6Y29tbWVudCcpKTsNCiAgICB2YXIgaGVpZ2h0PTA7DQogICAgdmFyIGNhcnI9W107DQogICAgZm9yKHZhciBqPTA7ajxhLmxlbmd0aDtqKyspew0KICAgICAgICB2YXIgc3RhbXA9TnVtYmVyKGFbal0uRGF0ZSkqMTAwMDsNCiAgICAgICAgdmFyIHRpbWU9cGFyc2VEYXRlKHN0YW1wKTsNCiAgICAgICAgdmFyIG5hbWU9YVtqXS5OYW1lOw0KICAgICAgICB2YXIgbWVzc2FnZT1hW2pdLk1lc3NhZ2U7DQogICAgICAgIHZhciBtZXNzYWdlQXJyPW1lc3NhZ2Uuc3BsaXQoL1xuLyk7DQogICAgICAgIHZhciBvPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdnJyk7DQogICAgICAgIHZhciBuPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIHZhciB0PWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIHZhciBtPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIG4uc2V0QXR0cmlidXRlKCd4Jyw1KTsNCiAgICAgICAgbi5zZXRBdHRyaWJ1dGUoJ3knLDEyKTsNCiAgICAgICAgbi5zZXRBdHRyaWJ1dGUoJ2ZpbGwnLCcjMDA2ZWZmJyk7DQogICAgICAgIG4udGV4dENvbnRlbnQ9bmFtZSsnOiAnOw0KICAgICAgICBuLnNldEF0dHJpYnV0ZSgnZm9udC1zaXplJywnMTInKTsNCiAgICAgICAgdC5zZXRBdHRyaWJ1dGUoJ3gnLDIwMCk7DQogICAgICAgIHQuc2V0QXR0cmlidXRlKCd5JywxMik7DQogICAgICAgIHQuc2V0QXR0cmlidXRlKCdmaWxsJywnIzk2OTY5NicpOw0KICAgICAgICB0LnRleHRDb250ZW50PXRpbWU7DQogICAgICAgIHQuc2V0QXR0cmlidXRlKCdmb250LXNpemUnLCcxMCcpOw0KICAgICAgICBtLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJywndHJhbnNsYXRlKDIwLDI3KScpOw0KICAgICAgICBtLnNldEF0dHJpYnV0ZSgnZm9udC1zaXplJywnMTInKTsNCiAgICAgICAgZm9yKHZhciBrPTA7azxtZXNzYWdlQXJyLmxlbmd0aDtrKyspew0KICAgICAgICAgICAgdmFyIHRzPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0c3BhbicpOw0KICAgICAgICAgICAgdHMuc2V0QXR0cmlidXRlKCd4JywnMCcpOw0KICAgICAgICAgICAgdHMuc2V0QXR0cmlidXRlKCd5JyxrKjE2KTsNCiAgICAgICAgICAgIHRzLnRleHRDb250ZW50PW1lc3NhZ2VBcnJba107DQogICAgICAgICAgICBtLmFwcGVuZENoaWxkKHRzKTsNCiAgICAgICAgfQ0KICAgICAgICBvLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJywndHJhbnNsYXRlKDAsJytoZWlnaHQrJyknKTsNCiAgICAgICAgby5hcHBlbmRDaGlsZChuKTsNCiAgICAgICAgby5hcHBlbmRDaGlsZCh0KTsNCiAgICAgICAgby5hcHBlbmRDaGlsZChtKTsNCiAgICAgICAgY2Fyci5wdXNoKG8pOw0KICAgICAgICBwb3B1cC5hcHBlbmRDaGlsZChvKTsNCiAgICAgICAgaGVpZ2h0PShtZXNzYWdlQXJyLmxlbmd0aCsxKSoxNitoZWlnaHQ7DQogICAgfQ0KICAgIHZhciB3YXJyPWdldGN3aChwb3B1cCk7DQogICAgb2xpbmUuc2V0QXR0cmlidXRlKCd4JywnMCcpOw0KICAgIG9saW5lLnNldEF0dHJpYnV0ZSgneScsJzAnKTsNCiAgICB2YXIgb3c9d2FyclswXSsxMC41Ow0KICAgIHZhciBvaD13YXJyWzFdKzM7DQogICAgb2xpbmUuc2V0QXR0cmlidXRlKCd3aWR0aCcsb3cpOw0KICAgIG9saW5lLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JyxvaCk7DQogICAgb2xpbmUuc2V0QXR0cmlidXRlKCdmaWxsJywnd2hpdGUnKTsNCiAgICBvbGluZS5zZXRBdHRyaWJ1dGUoJ3N0cm9rZScsJyM2NTY1NjUnKTsNCiAgICBwb3B1cC5hcHBlbmRDaGlsZChvbGluZSk7DQogICAgdmFyIGw9Y2Fyci5sZW5ndGg7DQogICAgd2hpbGUobC0tKXsNCiAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQoY2FycltsXSk7DQogICAgfQ0KICAgIHBvcHVwLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdibG9jayc7DQogICAgfTsNCiAgICBwb3B1cC5vbm1vdXNlb3V0PWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5ID0gJ25vbmUnOw0KICAgIH07DQogICAgdmFyIGNzPWNvbW1lbnRzW2ldLnF1ZXJ5U2VsZWN0b3IoJ3VzZScpLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJykubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICB2YXIgcHM9Y29tbWVudHNbaV0ucGFyZW50Tm9kZS5nZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScpOw0KICAgIGlmKHBzLnN1YnN0cigwLDIpID09ICd0cicpew0KICAgICAgICB2YXIgcHBzID0gcHMubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICAgICAgdmFyIHg9cGFyc2VGbG9hdChjc1swXSkrcGFyc2VGbG9hdChwcHNbMF0pOw0KICAgICAgICB2YXIgeT1wYXJzZUZsb2F0KHBwc1sxXSk7DQogICAgICAgIHg9eC50b3N1aXRzdmcoKTsNCiAgICAgICAgeT15LnRvc3VpdHN2ZygpOw0KICAgICAgICB2YXIgdHJzdHIgPSAndHJhbnNsYXRlKCcreCsnLCcreSsnKSc7DQogICAgfQ0KICAgIGVsc2UgaWYocHMuc3Vic3RyKDAsMikgPT0gJ21hJyl7DQogICAgICAgIHZhciBwcHMgPSBwcy5tYXRjaCgvKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVwpJC8pOw0KICAgICAgICB2YXIgbWFBcnIgPSBbcGFyc2VGbG9hdChwcHNbMV0pLHBhcnNlRmxvYXQocHBzWzNdKSxwYXJzZUZsb2F0KHBwc1s1XSkscGFyc2VGbG9hdChwcHNbN10pLHBhcnNlRmxvYXQocHBzWzldKSxwYXJzZUZsb2F0KHBwc1sxMV0pXTsNCiAgICAgICAgaWYobWFBcnJbMV0gPT0gMCl7DQogICAgICAgICAgICB2YXIgeCA9IHBhcnNlRmxvYXQoY3NbMF0pOw0KICAgICAgICAgICAgdmFyIHk9IHBhcnNlRmxvYXQoY3NbMV0pKzE2Ow0KICAgICAgICAgICAgdmFyIHgxID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTEgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHgxPXgxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgeTE9eTEudG9zdWl0c3ZnKCk7DQogICAgICAgICAgICB2YXIgdHJzdHIgPSAgJ3RyYW5zbGF0ZSgnK3gxKycsJyt5MSsnKSc7DQogICAgICAgIH1lbHNlew0KICAgICAgICAgICAgdmFyIHggPSBwYXJzZUZsb2F0KGNzWzBdKSsxNjsNCiAgICAgICAgICAgIHZhciB5ID0gcGFyc2VGbG9hdChjc1sxXSkrMTY7DQogICAgICAgICAgICB2YXIgeDEgPSB4ICogbWFBcnJbMF0gKyB5ICogbWFBcnJbMl0gKyBtYUFycls0XTsNCiAgICAgICAgICAgIHZhciB5MSA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgeCA9IHBhcnNlRmxvYXQoY3NbMF0pKzE2Ow0KICAgICAgICAgICAgeSA9IHBhcnNlRmxvYXQoY3NbMV0pOw0KICAgICAgICAgICAgdmFyIHgyID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTIgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHZhciBmeCA9IHgxPHgyP3gxLnRvc3VpdHN2ZygpOiB4Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciBmeSA9IHkxPnkyP3kxLnRvc3VpdHN2ZygpOiB5Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciBvZmZ5ID0gTWF0aC5hYnMoeTEteTIpOw0KICAgICAgICAgICAgdmFyIHRyc3RyID0gICd0cmFuc2xhdGUoJytmeCsnLCcrZnkrJyknOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JyxvZmZ5LnRvU3RyaW5nKCkpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnd2lkdGgnLCcxNicpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgneScsKC1vZmZ5KS50b1N0cmluZygpKTsNCiAgICAgICAgICAgIHBvcHVwUi5zZXRBdHRyaWJ1dGUoJ2ZpbGwnLCd0cmFuc3BhcmVudCcpOw0KICAgICAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQocG9wdXBSKTsNCiAgICAgICAgfQ0KICAgIH0NCiAgICBwb3B1cC5zZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScsdHJzdHIpOw0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnY29tbWVudCcsJycpOw0KICAgIHBvcHVwLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnZWQ6Y29tbWVudGlkJyxjb21tZW50c1tpXS5wYXJlbnROb2RlLmlkKTsNCiAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCcjc3ZnLWNvbnRhaW5lciA+IHN2ZycpLmFwcGVuZENoaWxkKHBvcHVwKTsNCiAgICBjb21tZW50c1tpXS5vbm1vdXNlb3Zlcj1mdW5jdGlvbiAoKSB7DQogICAgICAgIHZhciBjb21tZW50aWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KICAgICAgICB0aGlzLnF1ZXJ5U2VsZWN0b3IoJ3JlY3QnKS5zdHlsZS5kaXNwbGF5PSdibG9jayc7DQogICAgICAgIGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDpjb21tZW50aWQ9JyIrY29tbWVudGlkKyInXVtjb21tZW50XSIpLnN0eWxlLmRpc3BsYXk9J2Jsb2NrJzsNCiAgICB9Ow0KICAgIGNvbW1lbnRzW2ldLm9ubW91c2VvdXQ9ZnVuY3Rpb24gKCkgew0KICAgICAgICB2YXIgY29tbWVudGlkPXRoaXMucGFyZW50Tm9kZS5pZDsNCi8vICAgICAgICB3aW5kb3cuZ2V0U2VsZWN0aW9uKCkucmVtb3ZlQWxsUmFuZ2VzKCk7DQogICAgICAgIHRoaXMucXVlcnlTZWxlY3RvcigncmVjdCcpLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6Y29tbWVudGlkPSciK2NvbW1lbnRpZCsiJ11bY29tbWVudF0iKS5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICB9DQp9DQovLy0tY29tbWVudA0KLy9ub3RlLS0NCmlmKCF1YSl7DQogICAgdmFyIG5vdGVzPWRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoJ2c+Z1tlZFxcOm5vdGVdJyk7DQogICAgZnVuY3Rpb24gZ2V0d2gocyxwKSB7DQogICAgICAgIHZhciBtYWlucD1kb2N1bWVudC5jcmVhdGVFbGVtZW50KCdkaXYnKTsNCiAgICAgICAgbWFpbnAuc3R5bGUuY3NzVGV4dD1zOw0KICAgICAgICBtYWlucC5zdHlsZS5kaXNwbGF5PSdpbmxpbmUtYmxvY2snOw0KCQltYWlucC5zdHlsZS5tYXhXaWR0aCA9ICc0MDBweCc7DQoJCW1haW5wLnN0eWxlLndvcmRCcmVhayA9ICdicmVhay1hbGwnOw0KICAgICAgICBtYWlucC5pbm5lckhUTUw9cDsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5hcHBlbmRDaGlsZChtYWlucCk7DQogICAgICAgIHZhciB3PW1haW5wLmNsaWVudFdpZHRoOw0KICAgICAgICB2YXIgaD1tYWlucC5jbGllbnRIZWlnaHQ7DQogICAgICAgIGRvY3VtZW50LmJvZHkucmVtb3ZlQ2hpbGQobWFpbnApOw0KICAgICAgICByZXR1cm4gW3csaF0NCiAgICB9DQogICAgZm9yKHZhciBpPTA7aTxub3Rlcy5sZW5ndGg7aSsrKXsNCiAgICAgICAgdmFyIGE9bm90ZXNbaV0uZ2V0QXR0cmlidXRlKCdlZDpub3RlJyk7DQoJCXZhciBub3RlTG9jayA9IG5vdGVzW2ldLmdldEF0dHJpYnV0ZSgnZWQ6bm90ZWxvY2snKTsNCiAgICAgICAgaWYobm90ZUxvY2sgPT0gJ3RydWUnKXsNCiAgICAgICAgICAgIGNvbnRpbnVlOw0KICAgICAgICB9DQogICAgICAgIHZhciBtYWlucD1hLm1hdGNoKC88Ym9keVtePl0qPiguKik8XC9ib2R5Pi8pWzFdOw0KICAgICAgICB2YXIgbWFpbnM9YS5tYXRjaCgvc3R5bGU9IiguKj8pIi8pWzFdOw0KICAgICAgICB2YXIgb3V0PWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdnJyk7DQogICAgICAgIHZhciBvbGluZT1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywncmVjdCcpOw0KICAgICAgICB2YXIgcG9wdXA9ZG9jdW1lbnQuY3JlYXRlRWxlbWVudE5TKCdodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZycsJ2ZvcmVpZ25PYmplY3QnKTsNCiAgICAgICAgdmFyIHBvcHVwUj0gZG9jdW1lbnQuY3JlYXRlRWxlbWVudE5TKCdodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZycsJ3JlY3QnKTsNCiAgICAgICAgdmFyIGhvdmVyPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbCcsJyNjZGNkZmYnKTsNCiAgICAgICAgaG92ZXIuc2V0QXR0cmlidXRlKCd4JywnMCcpOw0KICAgICAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3knLCcwJyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JywnMTYnKTsNCiAgICAgICAgaG92ZXIuc2V0QXR0cmlidXRlKCd3aWR0aCcsJzE2Jyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbC1vcGFjaXR5JywnMC42Jyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyxub3Rlc1tpXS5xdWVyeVNlbGVjdG9yKCd1c2UnKS5nZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScpKTsNCiAgICAgICAgaG92ZXIuc3R5bGUuZGlzcGxheT0nbm9uZSc7DQogICAgICAgIG5vdGVzW2ldLmFwcGVuZENoaWxkKGhvdmVyKTsNCiAgICAgICAgcG9wdXAuc3R5bGUuY3NzVGV4dD1tYWluczsNCiAgICAgICAgcG9wdXAuaW5uZXJIVE1MPW1haW5wOw0KICAgICAgICB2YXIgd2g9Z2V0d2gobWFpbnMsbWFpbnApOw0KICAgICAgICBwb3B1cC5zZXRBdHRyaWJ1dGUoJ3dpZHRoJyx3aFswXSk7DQogICAgICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnaGVpZ2h0Jyx3aFsxXSk7DQogICAgICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJywndHJhbnNsYXRlKDgsNCknKTsNCgkJcG9wdXAuc3R5bGUud29yZEJyZWFrID0gJ2JyZWFrLWFsbCc7DQogICAgICAgIHBvcHVwLnN0eWxlLnRleHRBbGlnbj0nbGVmdCc7DQogICAgICAgIG9saW5lLnNldEF0dHJpYnV0ZSgneCcsJzAnKTsNCiAgICAgICAgb2xpbmUuc2V0QXR0cmlidXRlKCd5JywnMCcpOw0KICAgICAgICBvbGluZS5zZXRBdHRyaWJ1dGUoJ3dpZHRoJyx3aFswXSsxNik7DQogICAgICAgIG9saW5lLnNldEF0dHJpYnV0ZSgnaGVpZ2h0Jyx3aFsxXSs4KTsNCiAgICAgICAgb2xpbmUuc2V0QXR0cmlidXRlKCdzdHJva2UnLCcjYTI3YTAwJyk7DQogICAgICAgIG9saW5lLnNldEF0dHJpYnV0ZSgnZmlsbCcsJyNmZmU3OWQnKTsNCiAgICAgICAgb3V0LmFwcGVuZENoaWxkKG9saW5lKTsNCiAgICAgICAgb3V0LmFwcGVuZENoaWxkKHBvcHVwKTsNCiAgICAgICAgb3V0LnNldEF0dHJpYnV0ZSgnbm90ZScsJycpOw0KICAgICAgICBvdXQuc3R5bGUuZGlzcGxheT0nbm9uZSc7DQogICAgICAgIG91dC5zZXRBdHRyaWJ1dGUoJ2VkOm5vdGVpZCcsbm90ZXNbaV0ucGFyZW50Tm9kZS5pZCk7DQogICAgICAgIG91dC5vbm1vdXNlb3Zlcj1mdW5jdGlvbiAoKSB7DQogICAgICAgICAgICB0aGlzLnN0eWxlLmRpc3BsYXk9J2Jsb2NrJzsNCiAgICAgICAgfTsNCiAgICAgICAgb3V0Lm9ubW91c2VvdXQ9ZnVuY3Rpb24gKCkgew0KLy8gICAgICAgIHdpbmRvdy5nZXRTZWxlY3Rpb24gPyB3aW5kb3cuZ2V0U2VsZWN0aW9uKCkucmVtb3ZlUmFuZ2Uod2luZG93LmdldFNlbGVjdGlvbigpLnJlKTpkb2N1bWVudC5zZWxlY3Rpb24uZW1wdHkoKTsNCg0KICAgICAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICAgICAgfTsNCiAgICAgICAgdmFyIGNzPW5vdGVzW2ldLnF1ZXJ5U2VsZWN0b3IoJ3VzZScpLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJykubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICAgICAgdmFyIHBzPW5vdGVzW2ldLnBhcmVudE5vZGUuZ2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nKTsNCiAgICAgICAgaWYocHMuc3Vic3RyKDAsMikgPT0gJ3RyJyl7DQogICAgICAgICAgICB2YXIgcHBzID0gcHMubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICAgICAgICAgIHZhciB4PXBhcnNlRmxvYXQoY3NbMF0pK3BhcnNlRmxvYXQocHBzWzBdKTsNCiAgICAgICAgICAgIHZhciB5PXBhcnNlRmxvYXQocHBzWzFdKTsNCiAgICAgICAgICAgIHg9eC50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHk9eS50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciB0cnN0ciA9ICd0cmFuc2xhdGUoJyt4KycsJyt5KycpJzsNCiAgICAgICAgfWVsc2UgaWYocHMuc3Vic3RyKDAsMikgPT0gJ21hJyl7DQogICAgICAgICAgICB2YXIgcHBzID0gcHMubWF0Y2goLyhcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylcKSQvKTsNCiAgICAgICAgICAgIHZhciBtYUFyciA9IFtwYXJzZUZsb2F0KHBwc1sxXSkscGFyc2VGbG9hdChwcHNbM10pLHBhcnNlRmxvYXQocHBzWzVdKSxwYXJzZUZsb2F0KHBwc1s3XSkscGFyc2VGbG9hdChwcHNbOV0pLHBhcnNlRmxvYXQocHBzWzExXSldOw0KICAgICAgICAgICAgaWYobWFBcnJbMV0gPT0gMCl7DQogICAgICAgICAgICAgICAgdmFyIHggPSBwYXJzZUZsb2F0KGNzWzBdKTsNCiAgICAgICAgICAgICAgICB2YXIgeSA9IHBhcnNlRmxvYXQoY3NbMV0pKzE2Ow0KICAgICAgICAgICAgICAgIHZhciB4MSA9IHggKiBtYUFyclswXSArIHkgKiBtYUFyclsyXSArIG1hQXJyWzRdOw0KICAgICAgICAgICAgICAgIHZhciB5MSA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgICAgIHgxPXgxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgICAgIHkxPXkxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgICAgIHZhciB0cnN0ciA9ICAndHJhbnNsYXRlKCcreDErJywnK3kxKycpJzsNCiAgICAgICAgICAgIH1lbHNlew0KICAgICAgICAgICAgICAgIHZhciB4ID0gcGFyc2VGbG9hdChjc1swXSkrMTY7DQogICAgICAgICAgICAgICAgdmFyIHkgPSBwYXJzZUZsb2F0KGNzWzFdKSsxNjsNCiAgICAgICAgICAgICAgICB2YXIgeDEgPSB4ICogbWFBcnJbMF0gKyB5ICogbWFBcnJbMl0gKyBtYUFycls0XTsNCiAgICAgICAgICAgICAgICB2YXIgeTEgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgICAgICB4ID0gcGFyc2VGbG9hdChjc1swXSkrMTY7DQogICAgICAgICAgICAgICAgeSA9IHBhcnNlRmxvYXQoY3NbMV0pOw0KICAgICAgICAgICAgICAgIHZhciB4MiA9IHggKiBtYUFyclswXSArIHkgKiBtYUFyclsyXSArIG1hQXJyWzRdOw0KICAgICAgICAgICAgICAgIHZhciB5MiA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgICAgIHZhciBmeCA9IHgxPHgyP3gxLnRvc3VpdHN2ZygpOiB4Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgICAgICB2YXIgZnkgPSB5MT55Mj95MS50b3N1aXRzdmcoKTogeTIudG9zdWl0c3ZnKCk7DQoJCQkJdmFyIG9mZnkgPSBNYXRoLmFicyh5MS15Mik7CQkJCQkJCQkJCSAgDQogICAgICAgICAgICAgICAgdmFyIHRyc3RyID0gICd0cmFuc2xhdGUoJytmeCsnLCcrZnkrJyknOw0KICAgICAgICAgICAgICAgIHBvcHVwUi5zZXRBdHRyaWJ1dGUoJ2hlaWdodCcsb2ZmeS50b1N0cmluZygpKTsNCiAgICAgICAgICAgICAgICBwb3B1cFIuc2V0QXR0cmlidXRlKCd3aWR0aCcsJzE2Jyk7DQogICAgICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgneScsKC1vZmZ5KS50b1N0cmluZygpKTsNCiAgICAgICAgICAgICAgICBwb3B1cFIuc2V0QXR0cmlidXRlKCdmaWxsJywndHJhbnNwYXJlbnQnKTsNCiAgICAgICAgICAgICAgICBwb3B1cC5hcHBlbmRDaGlsZChwb3B1cFIpOw0KICAgICAgICAgICAgfQ0KICAgICAgICB9DQogICAgICAgIG91dC5zZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScsdHJzdHIpOw0KICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCcjc3ZnLWNvbnRhaW5lciA+IHN2ZycpLmFwcGVuZENoaWxkKG91dCk7DQogICAgICAgIG5vdGVzW2ldLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgICAgIHZhciBub3RlaWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KICAgICAgICAgICAgdGhpcy5xdWVyeVNlbGVjdG9yKCdyZWN0Jykuc3R5bGUuZGlzcGxheT0nYmxvY2snOw0KICAgICAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOm5vdGVpZD0nIitub3RlaWQrIiddW25vdGVdIikuc3R5bGUuZGlzcGxheT0nYmxvY2snOw0KICAgICAgICB9Ow0KICAgICAgICBub3Rlc1tpXS5vbm1vdXNlb3V0PWZ1bmN0aW9uICgpIHsNCiAgICAgICAgICAgIHZhciBub3RlaWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KLy8gICAgICAgIHdpbmRvdy5nZXRTZWxlY3Rpb24oKS5yZW1vdmVBbGxSYW5nZXMoKTsNCiAgICAgICAgICAgIHRoaXMucXVlcnlTZWxlY3RvcigncmVjdCcpLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgICAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOm5vdGVpZD0nIitub3RlaWQrIiddW25vdGVdIikuc3R5bGUuZGlzcGxheT0nbm9uZSc7DQogICAgICAgIH0NCiAgICB9DQp9ZWxzZXsNCiAgICBjb25zb2xlLmxvZygn5oqx5q2J77yMSUXmtY/op4jlmajkuI3mlK/mjIFub3Rl6Kej5p6Q77yM6K+35L2/55So5YW25LuW5YaF5qC45rWP6KeI5Zmo44CC6LCi6LCi77yBJykNCn0NCi8vLS1ub3RlDQovL2h5cGVybGluay0tDQp2YXIgbGlua3M9ZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgnZz5nW2VkXFw6aHlwZXJsaW5rXScpOw0KZnVuY3Rpb24gZ2V0bWF4bGVuKGFycixicnIpIHsNCiAgICB2YXIgbD0wOw0KICAgIHZhciBsbD0wOw0KICAgIGZvcih2YXIgaj0wO2o8YXJyLmxlbmd0aDtqKyspew0KICAgICAgICB2YXIgZT1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywndGV4dCcpOw0KICAgICAgICBpZighaXNOYU4obGlua2FycltqXSkpew0KICAgICAgICAgICAgZS50ZXh0Q29udGVudD0nUGFnZS0nK2FycltqXTsNCiAgICAgICAgfWVsc2V7DQogICAgICAgICAgICBlLnRleHRDb250ZW50PWFycltqXTsNCiAgICAgICAgfQ0KICAgICAgICBlLnN0eWxlLmZvbnRTaXplPScxMnB4JzsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0uYXBwZW5kQ2hpbGQoZSk7DQogICAgICAgIHZhciBldz1lLmdldEJCb3goKS53aWR0aDsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0ucmVtb3ZlQ2hpbGQoZSk7DQogICAgICAgIHZhciBoPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIGgudGV4dENvbnRlbnQ9YnJyW2pdOw0KICAgICAgICBoLnN0eWxlLmZvbnRTaXplPScxMnB4JzsNCiAgICAgICAgaC5zdHlsZS5mb250V2VpZ2h0PSdib2xkJzsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0uYXBwZW5kQ2hpbGQoaCk7DQogICAgICAgIHZhciBodz1oLmdldEJCb3goKS53aWR0aDsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0ucmVtb3ZlQ2hpbGQoaCk7DQogICAgICAgIGw9ZXc+aHc/ZXc6aHc7DQogICAgICAgIGxsPWw+bGw/bDpsbDsNCiAgICB9DQogICAgcmV0dXJuIGxsOw0KfQ0KZm9yKHZhciBpPTA7aTxsaW5rcy5sZW5ndGg7aSsrKXsNCiAgICB2YXIgcG9wdXA9ZG9jdW1lbnQuY3JlYXRlRWxlbWVudE5TKCdodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZycsJ2cnKTsNCiAgICB2YXIgcG9wdXBSPSBkb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywncmVjdCcpOw0KICAgIHZhciBob3Zlcj1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywncmVjdCcpOw0KICAgIHZhciBkZXNjYXJyPVtdOw0KICAgIHZhciBsaW5rYXJyPVtdOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbCcsJyNjZGNkZmYnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3gnLCcwJyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCd5JywnMCcpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JywnMTYnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3dpZHRoJywnMTYnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ2ZpbGwtb3BhY2l0eScsJzAuNicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyxsaW5rc1tpXS5xdWVyeVNlbGVjdG9yKCd1c2UnKS5nZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScpKTsNCiAgICBob3Zlci5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICBsaW5rc1tpXS5hcHBlbmRDaGlsZChob3Zlcik7DQogICAgLy8gY29uc29sZS5sb2cobGlua3NbaV0uZ2V0QXR0cmlidXRlKCdlZDpoeXBlcmxpbmsnKSk7DQogICAgdmFyIGE9SlNPTi5wYXJzZShsaW5rc1tpXS5nZXRBdHRyaWJ1dGUoJ2VkOmh5cGVybGluaycpKTsNCiAgICB2YXIgY3M9bGlua3NbaV0ucXVlcnlTZWxlY3RvcigndXNlJykuZ2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nKS5tYXRjaCgvXCgoXFMqfFxTKlxzXFMqKVwpLylbMV0uc3BsaXQoLyB8LC8pOw0KICAgIHZhciBwcz1saW5rc1tpXS5wYXJlbnROb2RlLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyk7DQogICAgaWYocHMuc3Vic3RyKDAsMikgPT0gJ3RyJyl7DQogICAgICAgIHZhciBwcHMgPSBwcy5tYXRjaCgvXCgoXFMqfFxTKlxzXFMqKVwpLylbMV0uc3BsaXQoLyB8LC8pOw0KICAgICAgICB2YXIgeD1wYXJzZUZsb2F0KGNzWzBdKStwYXJzZUZsb2F0KHBwc1swXSk7DQogICAgICAgIHZhciB5PXBhcnNlRmxvYXQocHBzWzFdKTsNCiAgICAgICAgeD14LnRvc3VpdHN2ZygpOw0KICAgICAgICB5PXkudG9zdWl0c3ZnKCk7DQogICAgICAgIHZhciB0cnN0ciA9ICd0cmFuc2xhdGUoJyt4KycsJyt5KycpJzsNCiAgICB9ZWxzZSBpZihwcy5zdWJzdHIoMCwyKSA9PSAnbWEnKXsNCiAgICAgICAgdmFyIHBwcyA9IHBzLm1hdGNoKC8oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pXCkkLyk7DQogICAgICAgIHZhciBtYUFyciA9IFtwYXJzZUZsb2F0KHBwc1sxXSkscGFyc2VGbG9hdChwcHNbM10pLHBhcnNlRmxvYXQocHBzWzVdKSxwYXJzZUZsb2F0KHBwc1s3XSkscGFyc2VGbG9hdChwcHNbOV0pLHBhcnNlRmxvYXQocHBzWzExXSldOw0KICAgICAgICBpZihtYUFyclsxXSA9PSAwKXsNCiAgICAgICAgICAgIHZhciB4ID0gcGFyc2VGbG9hdChjc1swXSk7DQogICAgICAgICAgICB2YXIgeSA9IHBhcnNlRmxvYXQoY3NbMV0pKzE2Ow0KICAgICAgICAgICAgdmFyIHgxID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTEgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHgxPXgxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgeTE9eTEudG9zdWl0c3ZnKCk7DQogICAgICAgICAgICB2YXIgdHJzdHIgPSAgJ3RyYW5zbGF0ZSgnK3gxKycsJyt5MSsnKSc7DQogICAgICAgIH1lbHNlew0KICAgICAgICAgICAgdmFyIHggPSBwYXJzZUZsb2F0KGNzWzBdKSsxNjsNCiAgICAgICAgICAgIHZhciB5ID0gcGFyc2VGbG9hdChjc1sxXSkrMTY7DQogICAgICAgICAgICB2YXIgeDEgPSB4ICogbWFBcnJbMF0gKyB5ICogbWFBcnJbMl0gKyBtYUFycls0XTsNCiAgICAgICAgICAgIHZhciB5MSA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgeCA9IHBhcnNlRmxvYXQoY3NbMF0pKzE2Ow0KICAgICAgICAgICAgeSA9IHBhcnNlRmxvYXQoY3NbMV0pOw0KICAgICAgICAgICAgdmFyIHgyID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTIgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHZhciBmeCA9IHgxPHgyP3gxLnRvc3VpdHN2ZygpOiB4Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciBmeSA9IHkxPnkyP3kxLnRvc3VpdHN2ZygpOiB5Mi50b3N1aXRzdmcoKTsNCgkJCXZhciBvZmZ5ID0gTWF0aC5hYnMoeTEteTIpOwkJCQkJCQkJCQkgIA0KICAgICAgICAgICAgdmFyIHRyc3RyID0gICd0cmFuc2xhdGUoJytmeCsnLCcrZnkrJyknOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JyxvZmZ5LnRvU3RyaW5nKCkpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnd2lkdGgnLCcxNicpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgneScsKC1vZmZ5KS50b1N0cmluZygpKTsNCiAgICAgICAgICAgIHBvcHVwUi5zZXRBdHRyaWJ1dGUoJ2ZpbGwnLCd0cmFuc3BhcmVudCcpOw0KICAgICAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQocG9wdXBSKTsNCiAgICAgICAgfQ0KICAgIH0NCiAgICB2YXIgYWw9YS5sZW5ndGg7DQogICAgZm9yKHZhciBqPTA7ajxhbDtqKyspew0KICAgICAgICBsaW5rYXJyLnB1c2goYVtqXS5saW5rKTsNCiAgICAgICAgZGVzY2Fyci5wdXNoKGFbal0uZGVzYyk7DQogICAgfQ0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyx0cnN0cik7DQogICAgdmFyIG1heD1nZXRtYXhsZW4obGlua2FycixkZXNjYXJyKTsNCiAgICBmb3IodmFyIGs9MDtrPGFsO2srKyl7DQogICAgICAgIHZhciBjPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdhJyk7DQogICAgICAgIHZhciBkPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgICAgIHZhciBlPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIHZhciBmPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIGlmKGlzTmFOKGxpbmthcnJba10pKXsNCiAgICAgICAgICAgIGMuc2V0QXR0cmlidXRlTlMoImh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiwgInhsaW5rIiwgImh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiKTsNCiAgICAgICAgICAgIGMuc2V0QXR0cmlidXRlTlMoImh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiLCAiaHJlZiIsIGxpbmthcnJba10pOw0KICAgICAgICAgICAgYy5zZXRBdHRyaWJ1dGUoJ3RhcmdldCcsJ19ibGFuaycpOw0KICAgICAgICAgICAgZS50ZXh0Q29udGVudD1saW5rYXJyW2tdOw0KICAgICAgICB9ZWxzZXsNCiAgICAgICAgICAgIGUudGV4dENvbnRlbnQ9J1BhZ2UtJytsaW5rYXJyW2tdOw0KICAgICAgICAgICAgYy5zZXRBdHRyaWJ1dGVOUygiaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciLCAieGxpbmsiLCAiaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIpOw0KICAgICAgICAgICAgYy5zZXRBdHRyaWJ1dGVOUygiaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIsICJocmVmIiwgIiMiK2xpbmthcnJba10pOw0KICAgICAgICB9DQogICAgICAgIGQuc2V0QXR0cmlidXRlKCd3aWR0aCcsbWF4KzEwKTsNCiAgICAgICAgZC5zZXRBdHRyaWJ1dGUoJ2hlaWdodCcsJzMzJyk7DQogICAgICAgIGQuc2V0QXR0cmlidXRlKCdzdHJva2UnLCcjOTk5OTk5Jyk7DQogICAgICAgIGQuc2V0QXR0cmlidXRlKCdmaWxsJywnd2hpdGUnKTsNCiAgICAgICAgZC5zZXRBdHRyaWJ1dGUoJ3knLDMzKmspOw0KICAgICAgICBmLnRleHRDb250ZW50PWRlc2NhcnJba107DQogICAgICAgIGYuc3R5bGUuZm9udFNpemU9JzEycHgnOw0KICAgICAgICBmLnN0eWxlLmZvbnRXZWlnaHQ9J2JvbGQnOw0KICAgICAgICBmLnNldEF0dHJpYnV0ZSgneCcsNSk7DQogICAgICAgIGYuc2V0QXR0cmlidXRlKCd5JywzMyprKzEyKTsNCiAgICAgICAgZS5zdHlsZS5mb250U2l6ZT0nMTJweCc7DQogICAgICAgIGUuc2V0QXR0cmlidXRlKCd5JywzMyprKzI4KTsNCiAgICAgICAgZS5zZXRBdHRyaWJ1dGUoJ3gnLDUpOw0KICAgICAgICBjLmFwcGVuZENoaWxkKGQpOw0KICAgICAgICBjLmFwcGVuZENoaWxkKGYpOw0KICAgICAgICBjLmFwcGVuZENoaWxkKGUpOw0KICAgICAgICBjLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgICAgIHRoaXMucXVlcnlTZWxlY3RvcigncmVjdCcpLnN0eWxlLmZpbGw9JyNlMWUxZmYnDQogICAgICAgIH07DQogICAgICAgIGMub25tb3VzZW91dD1mdW5jdGlvbiAoKSB7DQogICAgICAgICAgICB0aGlzLnF1ZXJ5U2VsZWN0b3IoJ3JlY3QnKS5zdHlsZS5maWxsPSd3aGl0ZScNCiAgICAgICAgfTsNCiAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQoYyk7DQogICAgfQ0KICAgIHBvcHVwLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnaHlwZXJsaW5rJywnJyk7DQogICAgcG9wdXAuc2V0QXR0cmlidXRlKCdlZDpsaW5raWQnLGxpbmtzW2ldLnBhcmVudE5vZGUuaWQpOw0KICAgIHBvcHVwLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdibG9jayc7DQogICAgfTsNCiAgICBwb3B1cC5vbmNsaWNrPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICB9Ow0KICAgIHBvcHVwLm9ubW91c2VvdXQ9ZnVuY3Rpb24gKCkgew0KICAgICAgICB0aGlzLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIH07DQogICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcignI3N2Zy1jb250YWluZXIgPiBzdmcnKS5hcHBlbmRDaGlsZChwb3B1cCk7DQogICAgbGlua3NbaV0ub25tb3VzZW92ZXI9ZnVuY3Rpb24gKCkgew0KICAgICAgICB2YXIgbGlua2lkPXRoaXMucGFyZW50Tm9kZS5pZDsNCiAgICAgICAgdGhpcy5xdWVyeVNlbGVjdG9yKCdyZWN0Jykuc3R5bGUuZGlzcGxheT0nYmxvY2snOw0KICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6bGlua2lkPSciK2xpbmtpZCsiJ11baHlwZXJsaW5rXSIpLnN0eWxlLmRpc3BsYXk9J2Jsb2NrJzsNCiAgICB9DQogICAgbGlua3NbaV0ub25tb3VzZW91dD1mdW5jdGlvbiAoKSB7DQogICAgICAgIHZhciBsaW5raWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KICAgICAgICB0aGlzLnF1ZXJ5U2VsZWN0b3IoJ3JlY3QnKS5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOmxpbmtpZD0nIitsaW5raWQrIiddW2h5cGVybGlua10iKS5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICB9DQp9DQovLy0taHlwZXJsaW5rDQovL2luaXRpYWxpemUtLQ0KdmFyIHNoYXBlPWRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoJ2dbZWRcXDp0b2d0b3BpY2lkXScpOw0KdmFyIG1JZD1kb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCdnW2VkXFw6dG9waWN0eXBlXScpOw0KdmFyIGRhdGFUcmVlPXt9Ow0KdmFyIGV4dHJhUmVsYT17fTsNCnZhciBjaGVja0lEPScnOw0KZm9yKHZhciBpPTA7aTxtSWQubGVuZ3RoO2krKyl7DQogICAgdmFyIHR5cGU9bUlkW2ldLmdldEF0dHJpYnV0ZSgnZWQ6dG9waWN0eXBlJyk7DQogICAgdmFyIHNpZD1tSWRbaV0uaWQ7DQogICAgaWYodHlwZSE9PSdjYWxsb3V0Jyl7DQogICAgICAgIGluaXQoc2lkLGRhdGFUcmVlKQ0KICAgIH0NCn0NCmZ1bmN0aW9uIGluaXQoaWQsIG9iaikgew0KICAgIHZhciBjaGlsZHMgPSBkb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCJnW2VkXFw6cGFyZW50aWQ9JyIgKyBpZCArICInXTpub3QoW2VkXFw6dG9waWN0eXBlXSkiKTsNCiAgICB2YXIgY2FsbHMgPSBkb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCJnW2VkXFw6cGFyZW50aWQ9JyIgKyBpZCArICInXVtlZFxcOnRvcGljdHlwZV0iKTsNCiAgICB2YXIgc3VtbWFyeSA9IGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoInBhdGhbZWRcXDpwYXJlbnRpZCo9JyIgKyBpZCArICInXVtlZFxcOnR5cGU9J3N1bW1hcnknXSIpOw0KICAgIHZhciBib3VuZGFyeT0gZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgicGF0aFtlZFxcOnBhcmVudGlkKj0nIiArIGlkICsgIiddW2VkXFw6dHlwZT0nYm91bmRhcnknXSIpOw0KICAgIHZhciByZWxhZnJvbT1kb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCJnW2VkXFw6ZnJvbWlkKj0nIiArIGlkICsgIiddW2VkXFw6dHlwZT0ncmVsYXRpb24nXSIpOw0KICAgIHZhciByZWxhdG89ZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgiZ1tlZFxcOnRvaWQqPSciICsgaWQgKyAiJ11bZWRcXDp0eXBlPSdyZWxhdGlvbiddIik7DQogICAgb2JqWyJtIiArIGlkXSA9IHt9Ow0KICAgIHZhciB0eXBlID0gZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoaWQpLmdldEF0dHJpYnV0ZSgnZWQ6dG9waWN0eXBlJyk7DQogICAgdmFyIGl3PWRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGlkKS5nZXRBdHRyaWJ1dGUoJ2VkOndpZHRoJyk7DQogICAgdmFyIGloPWRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGlkKS5nZXRBdHRyaWJ1dGUoJ2VkOmhlaWdodCcpOw0KICAgIGlmICh0eXBlKSB7DQogICAgICAgIG9ialsibSIgKyBpZF0udHlwZSA9IHR5cGU7DQogICAgfQ0KICAgIGlmKGl3JiZpaCl7DQogICAgICAgIG9ialsibSIgKyBpZF0ud2lkdGggPWl3Ow0KICAgICAgICBvYmpbIm0iICsgaWRdLmhlaWdodCA9aWg7DQogICAgfQ0KICAgIGlmIChyZWxhZnJvbS5sZW5ndGggIT09IDApIHsNCiAgICAgICAgb2JqWyJtIiArIGlkXS5yZWxhZnJvbSA9IHt9Ow0KICAgICAgICBmb3IgKHZhciBpID0gMDsgaSA8IHJlbGFmcm9tLmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgaW5kZXhpZCA9IHJlbGFmcm9tW2ldLmlkOw0KICAgICAgICAgICAgdmFyIHRvaWQgPSBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChpbmRleGlkKS5nZXRBdHRyaWJ1dGUoJ2VkOnRvaWQnKTsNCiAgICAgICAgICAgIGlmIChleHRyYVJlbGFbaW5kZXhpZF0gPT09IHVuZGVmaW5lZCkgew0KICAgICAgICAgICAgICAgIGV4dHJhUmVsYVtpbmRleGlkXSA9IHsNCiAgICAgICAgICAgICAgICAgICAgaWQ6IGluZGV4aWQsDQogICAgICAgICAgICAgICAgICAgIGZyb21pZDogaWQsDQogICAgICAgICAgICAgICAgICAgIHRvaWQ6IHRvaWQsDQogICAgICAgICAgICAgICAgICAgIGlzQzogZmFsc2UNCiAgICAgICAgICAgICAgICB9Ow0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgb2JqWyJtIiArIGlkXS5yZWxhZnJvbVtpbmRleGlkXT17fTsNCiAgICAgICAgICAgIG9ialsibSIgKyBpZF0ucmVsYWZyb20uZGlzcGxheT1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChpZCkuc3R5bGUuZGlzcGxheSAhPT0gJ25vbmUnPydibG9jayc6J25vbmUnOw0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChyZWxhdG8ubGVuZ3RoICE9PSAwKSB7DQogICAgICAgIG9ialsibSIgKyBpZF0ucmVsYXRvID0ge307DQogICAgICAgIGZvciAodmFyIGkgPSAwOyBpIDwgcmVsYXRvLmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgaW5kZXhpZD1yZWxhdG9baV0uaWQ7DQogICAgICAgICAgICB2YXIgZnJvbWlkPWRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGluZGV4aWQpLmdldEF0dHJpYnV0ZSgnZWQ6ZnJvbWlkJyk7DQogICAgICAgICAgICBpZihleHRyYVJlbGFbaW5kZXhpZF0gPT09IHVuZGVmaW5lZCl7DQogICAgICAgICAgICAgICAgZXh0cmFSZWxhW2luZGV4aWRdPXsNCiAgICAgICAgICAgICAgICAgICAgaWQ6aW5kZXhpZCwNCiAgICAgICAgICAgICAgICAgICAgZnJvbWlkOmZyb21pZCwNCiAgICAgICAgICAgICAgICAgICAgdG9pZDppZCwNCiAgICAgICAgICAgICAgICAgICAgaXNDOmZhbHNlDQogICAgICAgICAgICAgICAgfTsNCiAgICAgICAgICAgIH0NCiAgICAgICAgICAgIG9ialsibSIgKyBpZF0ucmVsYXRvW2luZGV4aWRdPXt9Ow0KICAgICAgICAgICAgb2JqWyJtIiArIGlkXS5yZWxhdG8uZGlzcGxheT1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChpZCkuc3R5bGUuZGlzcGxheSAhPT0gJ25vbmUnPydibG9jayc6J25vbmUnOw0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChjaGlsZHMubGVuZ3RoICE9PSAwKSB7DQogICAgICAgIG9ialsibSIgKyBpZF0uY2hpbGQgPSB7fTsNCiAgICAgICAgaWYgKGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDp0b2d0b3BpY2lkPSciICsgaWQgKyAiJ10iKSkgew0KICAgICAgICAgICAgLy8gY29uc29sZS5sb2coZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOnRvZ3RvcGljaWQ9JyIgKyBpZCArICInXSIpLmNoaWxkTm9kZXNbMF0uZ2V0QXR0cmlidXRlKCd4bGluazpocmVmJykpOw0KICAgICAgICAgICAgdmFyIHRvZyA9IGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDp0b2d0b3BpY2lkPSciICsgaWQgKyAiJ10iKS5nZXRFbGVtZW50c0J5VGFnTmFtZSgndXNlJylbMF0uZ2V0QXR0cmlidXRlKCd4bGluazpocmVmJykuc2xpY2UoMSk7DQogICAgICAgICAgICBvYmpbIm0iICsgaWRdLnRvZ3R5cGUgPSB0b2c7DQogICAgICAgIH0NCiAgICAgICAgZm9yICh2YXIgaSA9IDA7IGkgPCBjaGlsZHMubGVuZ3RoOyBpKyspIHsNCiAgICAgICAgICAgIHZhciBjaWQgPSBjaGlsZHNbaV0uaWQ7DQogICAgICAgICAgICBpbml0KGNpZCwgb2JqWyJtIiArIGlkXS5jaGlsZCk7DQogICAgICAgIH0NCiAgICB9DQogICAgaWYgKGNhbGxzLmxlbmd0aCAhPT0gMCkgew0KICAgICAgICBvYmpbIm0iICsgaWRdLmNhbGwgPSB7fTsNCiAgICAgICAgZm9yICh2YXIgaSA9IDA7IGkgPCBjYWxscy5sZW5ndGg7IGkrKykgew0KICAgICAgICAgICAgdmFyIGNpZCA9IGNhbGxzW2ldLmlkOw0KICAgICAgICAgICAgaW5pdChjaWQsIG9ialsibSIgKyBpZF0uY2FsbCk7DQogICAgICAgIH0NCiAgICB9DQogICAgaWYgKGJvdW5kYXJ5Lmxlbmd0aCAhPT0gMCkgew0KICAgICAgICBvYmpbIm0iICsgaWRdLmJvdW5kYXJ5ID0ge307DQogICAgICAgIGZvciAodmFyIGkgPSAwOyBpIDwgYm91bmRhcnkubGVuZ3RoOyBpKyspIHsNCiAgICAgICAgICAgIHZhciBjaWQgPWJvdW5kYXJ5W2ldLmlkOw0KICAgICAgICAgICAgaW5pdChjaWQsIG9ialsibSIgKyBpZF0uYm91bmRhcnkpOw0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChzdW1tYXJ5Lmxlbmd0aCAhPT0gMCkgew0KICAgICAgICBvYmpbIm0iICsgaWRdLnN1bW1hcnkgPSB7fTsNCiAgICAgICAgZm9yICh2YXIgaSA9IDA7IGkgPCBzdW1tYXJ5Lmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgY2lkID0gc3VtbWFyeVtpXS5pZDsNCiAgICAgICAgICAgIGluaXQoY2lkLCBvYmpbIm0iICsgaWRdLnN1bW1hcnkpOw0KICAgICAgICB9DQogICAgfQ0KfQ0KLy8tLWluaXRpYWxpemUNCi8vdG9nZ2xlZGlzcGxheS0tDQp2YXIgY2hhaW5BcnI9W107DQpmdW5jdGlvbiBnZXRjaGFpbihpZCl7DQogICAgY2hhaW5BcnIudW5zaGlmdCgnbScraWQpOw0KICAgIHZhciBwYXJlbnQ9ZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoaWQpLmdldEF0dHJpYnV0ZSgnZWQ6cGFyZW50aWQnKTsNCiAgICBpZighcGFyZW50KXsNCiAgICAgICAgcmV0dXJuOw0KICAgIH0NCglpZihwYXJlbnQubWF0Y2goL1wsLykpew0KICAgICAgICBwYXJlbnQgPSBwYXJlbnQubWF0Y2goL1xkKyg/PVwsKS8pWzBdDQogICAgfQ0KICAgIGdldGNoYWluKHBhcmVudCk7DQp9DQpmdW5jdGlvbiBnZXRvYmooaWQpIHsNCiAgICBjaGFpbkFycj1bXTsNCiAgICBnZXRjaGFpbihpZCk7DQogICAgdmFyIG1haW49Y2hhaW5BcnJbMF07DQogICAgaWYoY2hhaW5BcnIubGVuZ3RoPjEpew0KICAgICAgICB2YXIgb2JqPWRhdGFUcmVlW21haW5dOw0KICAgICAgICAvLyBjb25zb2xlLmxvZyhjaGFpbkFycik7DQogICAgICAgIGZvcih2YXIgaT0xO2k8Y2hhaW5BcnIubGVuZ3RoO2krKykgew0KICAgICAgICAgICAgdmFyIGEgPSBjaGFpbkFycltpXTsNCiAgICAgICAgICAgIGZvcih2YXIgaj0wO2o8T2JqZWN0LmtleXMob2JqKS5sZW5ndGg7aisrKXsNCiAgICAgICAgICAgICAgICB2YXIgY29iaj0gb2JqW09iamVjdC5rZXlzKG9iailbal1dW2FdOw0KICAgICAgICAgICAgICAgIGlmKGNvYmopew0KICAgICAgICAgICAgICAgICAgICBvYmo9Y29iajsNCiAgICAgICAgICAgICAgICAgICAgY29udGludWUNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICAgICAgcmV0dXJuIG9iag0KICAgIH1lbHNlew0KICAgICAgICB2YXIgb2JqPWRhdGFUcmVlW21haW5dOw0KICAgICAgICByZXR1cm4gb2JqDQogICAgfQ0KDQp9DQpmb3IodmFyIGk9MDtpPHNoYXBlLmxlbmd0aDtpKyspew0KICAgIHNoYXBlW2ldLm9uY2xpY2s9ZnVuY3Rpb24gKCkgew0KICAgICAgICB2YXIgaWQ9TnVtYmVyKHRoaXMuZ2V0QXR0cmlidXRlKCdlZDp0b2d0b3BpY2lkJykpOw0KICAgICAgICB2YXIgb2JqPWdldG9iaihpZCk7DQoNCiAgICAgICAgdmFyIHR5cGU9b2JqLnRvZ3R5cGU9PT0nbWludXMnPydwbHVzJzonbWludXMnOw0KICAgICAgICB2YXIgZGlzcGxheT1vYmoudG9ndHlwZT09PSdtaW51cyc/J25vbmUnOidibG9jayc7DQogICAgICAgIHRoaXMuZ2V0RWxlbWVudHNCeVRhZ05hbWUoJ3VzZScpWzBdLnNldEF0dHJpYnV0ZSgneGxpbms6aHJlZicsJyMnK3R5cGUpOw0KICAgICAgICBvYmoudG9ndHlwZT10eXBlOw0KICAgICAgICBjaGVja0lEPW9iajsNCg0KICAgICAgICB1dGQob2JqLGlkLGRpc3BsYXkpOw0KICAgICAgICBleHRyYVJlbGFGaW4oKTsNCiAgICB9DQp9DQpmdW5jdGlvbiB1dGQob2JqLGlkLHNob3csb2MpIHsNCg0KICAgIHZhciBwc2hvdz1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChpZCkuc3R5bGUuZGlzcGxheSE9PSAnbm9uZSc/J2Jsb2NrJzonbm9uZSc7DQogICAgaWYgKG9iai5yZWxhZnJvbSl7DQogICAgICAgIGlmKG9iai5yZWxhZnJvbS5kaXNwbGF5IT09IHBzaG93KXsNCiAgICAgICAgICAgIHZhciByZWxhZnJvbXM9T2JqZWN0LmtleXMob2JqLnJlbGFmcm9tKTsNCiAgICAgICAgICAgIHJlbGFmcm9tcy5zcGxpY2UocmVsYWZyb21zLmluZGV4T2YoJ2Rpc3BsYXknKSwxKTsNCiAgICAgICAgICAgIGZvcih2YXIgaz0wO2s8cmVsYWZyb21zLmxlbmd0aDtrKyspew0KICAgICAgICAgICAgICAgIHZhciBkPXJlbGFmcm9tc1trXTsNCiAgICAgICAgICAgICAgICBleHRyYVJlbGFbZF0uaXNDPXRydWU7DQogICAgICAgICAgICB9DQogICAgICAgICAgICBvYmoucmVsYWZyb20uZGlzcGxheSA9IHBzaG93Ow0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChvYmoucmVsYXRvKXsNCiAgICAgICAgaWYob2JqLnJlbGF0by5kaXNwbGF5IT09IHBzaG93KXsNCiAgICAgICAgICAgIHZhciByZWxhdG9zPU9iamVjdC5rZXlzKG9iai5yZWxhdG8pOw0KICAgICAgICAgICAgcmVsYXRvcy5zcGxpY2UocmVsYXRvcy5pbmRleE9mKCdkaXNwbGF5JyksMSk7DQogICAgICAgICAgICBmb3IodmFyIGs9MDtrPHJlbGF0b3MubGVuZ3RoO2srKyl7DQogICAgICAgICAgICAgICAgdmFyIGQ9cmVsYXRvc1trXTsNCiAgICAgICAgICAgICAgICBleHRyYVJlbGFbZF0uaXNDPXRydWU7DQogICAgICAgICAgICB9DQogICAgICAgICAgICBvYmoucmVsYXRvLmRpc3BsYXkgPSBwc2hvdzsNCiAgICAgICAgfQ0KICAgIH0NCiAgICBpZihvYmouY2FsbCl7DQogICAgICAgIHZhciBjYWxscz1PYmplY3Qua2V5cyhvYmouY2FsbCk7DQogICAgICAgIGlmKGNoZWNrSUQhPT1vYmopew0KICAgICAgICAgICAgZm9yKHZhciBpPTA7aSA8IGNhbGxzLmxlbmd0aDtpKyspew0KICAgICAgICAgICAgICAgIHZhciBhPWNhbGxzW2ldLnNsaWNlKDEpOw0KICAgICAgICAgICAgICAgIHZhciBiPW9iai5jYWxsW2NhbGxzW2ldXTsNCiAgICAgICAgICAgICAgICB2YXIgYz1iLnRvZ3R5cGU7DQogICAgICAgICAgICAgICAgZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoYSkuc3R5bGUuZGlzcGxheT1zaG93Ow0KICAgICAgICAgICAgICAgIGlmIChiLnJlbGFmcm9tJiYhYyl7DQogICAgICAgICAgICAgICAgICAgIGlmKGIucmVsYWZyb20uZGlzcGxheSE9PSBzaG93KXsNCiAgICAgICAgICAgICAgICAgICAgICAgIHZhciByZWxhZnJvbXM9T2JqZWN0LmtleXMoYi5yZWxhZnJvbSk7DQogICAgICAgICAgICAgICAgICAgICAgICByZWxhZnJvbXMuc3BsaWNlKHJlbGFmcm9tcy5pbmRleE9mKCdkaXNwbGF5JyksMSk7DQogICAgICAgICAgICAgICAgICAgICAgICBmb3IodmFyIGs9MDtrPHJlbGFmcm9tcy5sZW5ndGg7aysrKXsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICB2YXIgZD1yZWxhZnJvbXNba107DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgZXh0cmFSZWxhW2RdLmlzQz10cnVlOw0KICAgICAgICAgICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgICAgICAgICAgYi5yZWxhZnJvbS5kaXNwbGF5ID0gc2hvdzsNCiAgICAgICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICBpZiAoYi5yZWxhdG8mJiFjKXsNCiAgICAgICAgICAgICAgICAgICAgaWYoYi5yZWxhdG8uZGlzcGxheSE9PSBzaG93KXsNCiAgICAgICAgICAgICAgICAgICAgICAgIHZhciByZWxhdG9zPU9iamVjdC5rZXlzKGIucmVsYXRvKTsNCiAgICAgICAgICAgICAgICAgICAgICAgIHJlbGF0b3Muc3BsaWNlKHJlbGF0b3MuaW5kZXhPZignZGlzcGxheScpLDEpOw0KICAgICAgICAgICAgICAgICAgICAgICAgZm9yKHZhciBrPTA7azxyZWxhdG9zLmxlbmd0aDtrKyspew0KICAgICAgICAgICAgICAgICAgICAgICAgICAgIHZhciBkPXJlbGF0b3Nba107DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgZXh0cmFSZWxhW2RdLmlzQz10cnVlOw0KICAgICAgICAgICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgICAgICAgICAgYi5yZWxhdG8uZGlzcGxheSA9IHNob3c7DQogICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYoYyl7DQogICAgICAgICAgICAgICAgICAgIGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDp0b2d0b3BpY2lkPSciK2ErIiddIikuc3R5bGUuZGlzcGxheT1zaG93Ow0KICAgICAgICAgICAgICAgICAgICBpZihjPT09J21pbnVzJyl7DQogICAgICAgICAgICAgICAgICAgICAgICB1dGQoYixhLHNob3cpDQogICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgaWYgKChiLmNhbGx8fGIuYm91bmRhcnl8fGIuc3VtbWFyeSkmJmM9PT0ncGx1cycpIHsNCiAgICAgICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdyx0cnVlKQ0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIGlmKGIuY2FsbCYmIWMpIHsNCiAgICAgICAgICAgICAgICAgICAgdXRkKGIsYSxzaG93LHRydWUpDQogICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIGlmIChiLnN1bW1hcnkmJiFjKSB7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYgKGIuYm91bmRhcnkmJiFjKSB7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQoNCiAgICAgICAgICAgIH0NCiAgICAgICAgfQ0KICAgIH0NCiAgICBpZihvYmouc3VtbWFyeSl7DQogICAgICAgIHZhciBzdW1tYXJ5cz1PYmplY3Qua2V5cyhvYmouc3VtbWFyeSk7DQogICAgICAgIGlmKChjaGVja0lEIT09b2JqJiYob2JqLnRvZ3R5cGU9PT0nbWludXMnfHwhb2JqLnRvZ3R5cGUpKXx8Y2hlY2tJRD09PW9iail7DQogICAgICAgICAgICBmb3IodmFyIGk9MDtpPHN1bW1hcnlzLmxlbmd0aDtpKyspew0KICAgICAgICAgICAgICAgIHZhciBhPXN1bW1hcnlzW2ldLnNsaWNlKDEpOw0KCQkJCXZhciBvc3AgPSBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5nZXRBdHRyaWJ1dGUoJ2VkOnBhcmVudGlkJyk7DQogICAgICAgICAgICAgICAgaWYob3NwLm1hdGNoKC9cLC8pKXsNCiAgICAgICAgICAgICAgICAgICAgdmFyIG9zcGEgPSBvc3Auc3BsaXQoJywnKTsNCiAgICAgICAgICAgICAgICAgICAgdmFyIG9zcEw9MDsNCg0KICAgICAgICAgICAgICAgICAgICBmb3IodmFyIGo9MDtqPG9zcGEubGVuZ3RoO2orKyl7DQogICAgICAgICAgICAgICAgICAgICAgICBpZihzaG93ID09ICdub25lJyl7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgaWYoZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQob3NwYVtqXSkuc3R5bGUuZGlzcGxheSAhPSAnbm9uZScpew0KICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBicmVhazsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICB9ZWxzZXsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgb3NwTCsrOw0KICAgICAgICAgICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgICAgIH1lbHNlew0KICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBpZihkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5zdHlsZS5kaXNwbGF5ICE9ICdub25lJyl7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBicmVhazsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgfWVsc2V7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBvc3BMKys7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgICAgIH0NCg0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgICAgIGlmKG9zcEwgIT09IG9zcGEubGVuZ3RoKXsNCiAgICAgICAgICAgICAgICAgICAgICAgIGNvbnRpbnVlOw0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIHZhciBiPW9iai5zdW1tYXJ5W3N1bW1hcnlzW2ldXTsNCiAgICAgICAgICAgICAgICAvLyBjb25zb2xlLmxvZyhhKTsNCiAgICAgICAgICAgICAgICBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5zdHlsZS5kaXNwbGF5PXNob3c7DQovLyAgICAgICAgICAgICAgICBpZihjKXsNCi8vICAgICAgICAgICAgICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6dG9ndG9waWNpZD0nIithKyInXSIpLnN0eWxlLmRpc3BsYXk9c2hvdzsNCi8vICAgICAgICAgICAgICAgICAgICBpZihjPT09J21pbnVzJyl7DQovLyAgICAgICAgICAgICAgICAgICAgICAgIHV0ZChiLHNob3cpDQovLyAgICAgICAgICAgICAgICAgICAgfQ0KLy8gICAgICAgICAgICAgICAgICAgIGlmIChiLmNhbGwmJmM9PT0ncGx1cycpIHsNCi8vICAgICAgICAgICAgICAgICAgICAgICAgdXRkKGIsc2hvdyx0cnVlKQ0KLy8gICAgICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIGlmKGIuY2FsbCYmIWMpIHsNCi8vICAgICAgICAgICAgICAgICAgICB1dGQoYixzaG93LHRydWUpDQovLyAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYoT2JqZWN0LmtleXMoYikubGVuZ3RoIT09MCl7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICB9DQogICAgaWYob2JqLmJvdW5kYXJ5KXsNCiAgICAgICAgdmFyIGJvdW5kYXJ5cz1PYmplY3Qua2V5cyhvYmouYm91bmRhcnkpOw0KICAgICAgICBpZihjaGVja0lEIT09b2JqKXsNCiAgICAgICAgICAgIGZvcih2YXIgaT0wO2k8Ym91bmRhcnlzLmxlbmd0aDtpKyspew0KICAgICAgICAgICAgICAgIHZhciBhPWJvdW5kYXJ5c1tpXS5zbGljZSgxKTsNCiAgICAgICAgICAgICAgICB2YXIgYj1vYmouYm91bmRhcnlbYm91bmRhcnlzW2ldXTsNCiAgICAgICAgICAgICAgICAvLyBjb25zb2xlLmxvZyhhKTsNCiAgICAgICAgICAgICAgICBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5zdHlsZS5kaXNwbGF5PXNob3c7DQovLyAgICAgICAgICAgICAgICBpZihjKXsNCi8vICAgICAgICAgICAgICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6dG9ndG9waWNpZD0nIithKyInXSIpLnN0eWxlLmRpc3BsYXk9c2hvdzsNCi8vICAgICAgICAgICAgICAgICAgICBpZihjPT09J21pbnVzJyl7DQovLyAgICAgICAgICAgICAgICAgICAgICAgIHV0ZChiLHNob3cpDQovLyAgICAgICAgICAgICAgICAgICAgfQ0KLy8gICAgICAgICAgICAgICAgICAgIGlmIChiLmNhbGwmJmM9PT0ncGx1cycpIHsNCi8vICAgICAgICAgICAgICAgICAgICAgICAgdXRkKGIsc2hvdyx0cnVlKQ0KLy8gICAgICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIGlmKGIuY2FsbCYmIWMpIHsNCi8vICAgICAgICAgICAgICAgICAgICB1dGQoYixzaG93LHRydWUpDQovLyAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYoT2JqZWN0LmtleXMoYikubGVuZ3RoIT09MCl7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICB9DQogICAgaWYoIW9jJiZvYmouY2hpbGQpIHsNCiAgICAgICAgdmFyIGNoaWxkcyA9IE9iamVjdC5rZXlzKG9iai5jaGlsZCk7DQogICAgICAgIGZvciAodmFyIGkgPSAwOyBpIDwgY2hpbGRzLmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgYSA9IGNoaWxkc1tpXS5zbGljZSgxKTsNCiAgICAgICAgICAgIHZhciBiID0gb2JqLmNoaWxkW2NoaWxkc1tpXV07DQogICAgICAgICAgICB2YXIgYyA9IGIudG9ndHlwZTsNCiAgICAgICAgICAgIGRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGEpLnN0eWxlLmRpc3BsYXkgPSBzaG93Ow0KCQkJdmFyIHRTUGF0aCA9IGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoInBhdGhbZWRcXDp0b3N1cGVyaWQ9JyIrYSsiJ10iKTsNCiAgICAgICAgICAgIGlmKHRTUGF0aCl7DQogICAgICAgICAgICAgICAgdFNQYXRoLnN0eWxlLmRpc3BsYXkgPSBzaG93Ow0KICAgICAgICAgICAgfQ0KCQkJdmFyIG5vdGVUaXAgPSBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6bm90ZXRvPSciK2ErIiddIik7DQogICAgICAgICAgICBpZihub3RlVGlwKXsNCiAgICAgICAgICAgICAgICBub3RlVGlwLnN0eWxlLmRpc3BsYXkgPSBzaG93Ow0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgaWYgKGIucmVsYWZyb20mJiFjKXsNCiAgICAgICAgICAgICAgICBpZihiLnJlbGFmcm9tLmRpc3BsYXkhPT0gc2hvdyl7DQogICAgICAgICAgICAgICAgICAgIHZhciByZWxhZnJvbXM9T2JqZWN0LmtleXMoYi5yZWxhZnJvbSk7DQogICAgICAgICAgICAgICAgICAgIHJlbGFmcm9tcy5zcGxpY2UocmVsYWZyb21zLmluZGV4T2YoJ2Rpc3BsYXknKSwxKTsNCiAgICAgICAgICAgICAgICAgICAgZm9yKHZhciBrPTA7azxyZWxhZnJvbXMubGVuZ3RoO2srKyl7DQogICAgICAgICAgICAgICAgICAgICAgICB2YXIgZD1yZWxhZnJvbXNba107DQogICAgICAgICAgICAgICAgICAgICAgICBleHRyYVJlbGFbZF0uaXNDPXRydWU7DQogICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgYi5yZWxhZnJvbS5kaXNwbGF5ID0gc2hvdzsNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgICAgICBpZiAoYi5yZWxhdG8mJiFjKXsNCiAgICAgICAgICAgICAgICBpZihiLnJlbGF0by5kaXNwbGF5IT09IHNob3cpew0KICAgICAgICAgICAgICAgICAgICB2YXIgcmVsYXRvcz1PYmplY3Qua2V5cyhiLnJlbGF0byk7DQogICAgICAgICAgICAgICAgICAgIHJlbGF0b3Muc3BsaWNlKHJlbGF0b3MuaW5kZXhPZignZGlzcGxheScpLDEpOw0KICAgICAgICAgICAgICAgICAgICBmb3IodmFyIGs9MDtrPHJlbGF0b3MubGVuZ3RoO2srKyl7DQogICAgICAgICAgICAgICAgICAgICAgICB2YXIgZD1yZWxhdG9zW2tdOw0KICAgICAgICAgICAgICAgICAgICAgICAgZXh0cmFSZWxhW2RdLmlzQz10cnVlOw0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgICAgIGIucmVsYXRvLmRpc3BsYXkgPSBzaG93Ow0KICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgIH0NCiAgICAgICAgICAgIGlmIChjKSB7DQogICAgICAgICAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOnRvZ3RvcGljaWQ9JyIgKyBhICsgIiddIikuc3R5bGUuZGlzcGxheSA9IHNob3c7DQogICAgICAgICAgICAgICAgaWYgKGMgPT09ICdtaW51cycpIHsNCiAgICAgICAgICAgICAgICAgICAgdXRkKGIsYSxzaG93KQ0KICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICBpZiAoKGIuY2FsbHx8Yi5ib3VuZGFyeXx8Yi5zdW1tYXJ5KSYmYz09PSdwbHVzJykgew0KICAgICAgICAgICAgICAgICAgICB1dGQoYixhLHNob3csdHJ1ZSkNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgICAgICBpZiAoYi5jYWxsJiYhYykgew0KICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdyx0cnVlKQ0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgaWYgKGIuc3VtbWFyeSYmIWMpIHsNCiAgICAgICAgICAgICAgICB1dGQoYixhLHNob3cpDQogICAgICAgICAgICB9DQogICAgICAgICAgICBpZiAoYi5ib3VuZGFyeSYmIWMpIHsNCiAgICAgICAgICAgICAgICB1dGQoYixhLHNob3cpDQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICB9DQp9DQoNCmZ1bmN0aW9uIGV4dHJhUmVsYUZpbigpIHsNCiAgICB2YXIgZXh0cmFrZXlzPU9iamVjdC5rZXlzKGV4dHJhUmVsYSk7DQogICAgZm9yKHZhciBpPTA7aTxleHRyYWtleXMubGVuZ3RoO2krKyl7DQogICAgICAgIHZhciBleHRyYU9iaj1leHRyYVJlbGFbZXh0cmFrZXlzW2ldXTsNCiAgICAgICAgaWYoZXh0cmFPYmouaXNDID09PSB0cnVlKXsNCiAgICAgICAgICAgIHZhciBmc2hvdz1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChleHRyYU9iai5mcm9taWQpLnN0eWxlLmRpc3BsYXkgIT09J25vbmUnPyB0cnVlOiBmYWxzZTsNCiAgICAgICAgICAgIHZhciB0c2hvdz1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChleHRyYU9iai50b2lkKS5zdHlsZS5kaXNwbGF5ICE9PSdub25lJz8gdHJ1ZTogZmFsc2U7DQogICAgICAgICAgICBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChleHRyYU9iai5pZCkuc3R5bGUuZGlzcGxheT1mc2hvdyAmJiB0c2hvdz8gJ2Jsb2NrJzogJ25vbmUnOw0KICAgICAgICAgICAgZXh0cmFSZWxhW2V4dHJha2V5c1tpXV0uaXNDID0gZmFsc2U7DQogICAgICAgIH0NCiAgICB9DQp9'))
</script>
</body>
