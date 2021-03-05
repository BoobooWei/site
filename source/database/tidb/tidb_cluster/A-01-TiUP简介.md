---
title: TiUP 简介
---

# 参考文档

* [TiUP & TiOps](https://book.tidb.io/session2/chapter1/tiup-tiops.html)


### 思维导图

  <head>
    <meta charset="utf-8"/>
    <meta http-equiv="X-UA-Compatible" content="IE=edge"/>
    <title>TiUP 简介</title>
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
      <div id="main-content">
        <div id="svg-container"><svg xmlns:ed="https://www.edrawsoft.cn/xml/2017/SVGExtensions/" xmlns:xlink="http://www.w3.org/1999/xlink" ed:hSpacing="30" xmlns:ev="http://www.w3.org/2001/xml-events" xmlns="http://www.w3.org/2000/svg" width="1475" height="2225" id="page0" ed:name="页面-1" ed:vSpacing="30" viewBox="0 0 1475 2225" preserveAspectRadio="xMinYMin meet">
    <style type="text/css"><![CDATA[
g[ed\:togtopicid],g[ed\:hyperlink],g[ed\:comment],g[ed\:note] {cursor:pointer;}
g[id] {-moz-user-select: none;-ms-user-select: none;user-select: none;}
svg text::selection,svg tspan::selection{background-color: #4285f4;color: #ffffff;fill: #ffffff;}
.st6 {fill:#303030;font-family:STSong;font-size:9pt}
.st5 {fill:#ffffff;font-family:STSong;font-size:11.25pt}
.st4 {fill:#ffffff;font-family:STSong;font-size:14.25pt}
]]></style>
    <defs/>
    <rect x="0" fill="#ffffff" width="1475" y="0" height="2225"/>
    <path transform="matrix(1,0,0,1,147.5,461.5)" stroke="#595959" fill="none" d="M0.2,423.8L28.2,423.8L28.2,-423.8L63.2,-423.8" stroke-linecap="round" ed:parentid="101" ed:tosuperid="102" stroke-width="2" id="103" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,147.5,496.75)" stroke="#595959" fill="none" d="M0.2,388.5L28.2,388.5L28.2,-388.5L63.2,-388.5" stroke-linecap="round" ed:parentid="101" ed:tosuperid="104" stroke-width="2" id="105" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,308.17,805.13)" stroke="#595959" fill="none" d="M-13.5,150.6L3.7,150.6L3.7,-150.6L13.5,-150.6" stroke-linecap="round" ed:parentid="320" ed:tosuperid="106" id="107" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,368.17,106.63)" stroke="#595959" fill="none" d="M-13.5,1.6L3.7,1.6L3.7,-1.6L13.5,-1.6" stroke-linecap="round" ed:parentid="104" ed:tosuperid="108" id="109" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,308.17,36.13)" stroke="#595959" fill="none" d="M-13.5,1.6L3.7,1.6L3.7,-1.6L13.5,-1.6" stroke-linecap="round" ed:parentid="102" ed:tosuperid="110" id="111" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,308.17,47.13)" stroke="#595959" fill="none" d="M3.7,-9.4L3.7,9.4L13.5,9.4" stroke-linecap="round" ed:parentid="102" ed:tosuperid="112" id="113" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,368.17,117.63)" stroke="#595959" fill="none" d="M3.7,-9.4L3.7,9.4L13.5,9.4" stroke-linecap="round" ed:parentid="104" ed:tosuperid="114" id="115" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,401.17,415)" stroke="#595959" fill="none" d="M-13.5,239.5L3.7,239.5L3.7,-239.5L13.5,-239.5" stroke-linecap="round" ed:parentid="106" ed:tosuperid="116" id="117" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,795.31,385.5)" stroke="#595959" fill="none" d="M-13.5,22L3.7,22L3.7,-22L13.5,-22" stroke-linecap="round" ed:parentid="166" ed:tosuperid="118" id="119" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,401.17,751)" stroke="#595959" fill="none" d="M3.7,-96.5L3.7,96.5L13.5,96.5" stroke-linecap="round" ed:parentid="106" ed:tosuperid="124" id="125" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,544.95,567)" stroke="#595959" fill="none" d="M-13.5,5.5L3.7,5.5L3.7,-5.5L13.5,-5.5" stroke-linecap="round" ed:parentid="198" ed:tosuperid="126" id="127" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,544.95,578)" stroke="#595959" fill="none" d="M3.7,-5.5L3.7,5.5L13.5,5.5" stroke-linecap="round" ed:parentid="198" ed:tosuperid="128" id="129" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,895.84,341.5)" stroke="#595959" fill="none" d="M3.7,22L3.7,-22L13.5,-22" stroke-linecap="round" ed:parentid="118" ed:tosuperid="130" id="131" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,895.84,330.5)" stroke="#595959" fill="none" d="M-13.5,33L3.7,33L3.7,-33L13.5,-33" stroke-linecap="round" ed:parentid="118" ed:tosuperid="132" id="133" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,895.84,363.5)" stroke="#595959" fill="none" d="M3.7,0L13.5,0" stroke-linecap="round" ed:parentid="118" ed:tosuperid="134" id="135" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,895.84,352.5)" stroke="#595959" fill="none" d="M3.7,11L3.7,-11L13.5,-11" stroke-linecap="round" ed:parentid="118" ed:tosuperid="136" id="137" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,895.84,374.5)" stroke="#595959" fill="none" d="M3.7,-11L3.7,11L13.5,11" stroke-linecap="round" ed:parentid="118" ed:tosuperid="138" id="139" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,895.84,385.5)" stroke="#595959" fill="none" d="M3.7,-22L3.7,22L13.5,22" stroke-linecap="round" ed:parentid="118" ed:tosuperid="140" id="141" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,895.84,396.5)" stroke="#595959" fill="none" d="M3.7,-33L3.7,33L13.5,33" stroke-linecap="round" ed:parentid="118" ed:tosuperid="142" id="143" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,795.31,446)" stroke="#595959" fill="none" d="M3.7,-38.5L3.7,38.5L13.5,38.5" stroke-linecap="round" ed:parentid="166" ed:tosuperid="144" id="145" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,902.02,468)" stroke="#595959" fill="none" d="M-13.5,16.5L3.7,16.5L3.7,-16.5L13.5,-16.5" stroke-linecap="round" ed:parentid="144" ed:tosuperid="146" id="147" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,902.02,490)" stroke="#595959" fill="none" d="M3.7,-5.5L3.7,5.5L13.5,5.5" stroke-linecap="round" ed:parentid="144" ed:tosuperid="148" id="149" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,902.02,479)" stroke="#595959" fill="none" d="M3.7,5.5L3.7,-5.5L13.5,-5.5" stroke-linecap="round" ed:parentid="144" ed:tosuperid="150" id="151" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,902.02,501)" stroke="#595959" fill="none" d="M3.7,-16.5L3.7,16.5L13.5,16.5" stroke-linecap="round" ed:parentid="144" ed:tosuperid="152" id="153" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,401.17,511.5)" stroke="#595959" fill="none" d="M3.7,143L3.7,-143L13.5,-143" stroke-linecap="round" ed:parentid="106" ed:tosuperid="154" id="155" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,494.17,283)" stroke="#595959" fill="none" d="M-13.5,85.5L3.7,85.5L3.7,-85.5L13.5,-85.5" stroke-linecap="round" ed:parentid="154" ed:tosuperid="156" id="157" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,558.73,197.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="156" ed:tosuperid="158" id="159" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,494.17,308)" stroke="#595959" fill="none" d="M3.7,60.5L3.7,-60.5L13.5,-60.5" stroke-linecap="round" ed:parentid="154" ed:tosuperid="160" id="161" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,561.88,247.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="160" ed:tosuperid="162" id="163" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,494.17,388)" stroke="#595959" fill="none" d="M3.7,-19.5L3.7,19.5L13.5,19.5" stroke-linecap="round" ed:parentid="154" ed:tosuperid="164" id="165" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,654.31,407.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="164" ed:tosuperid="166" id="167" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,494.17,454)" stroke="#595959" fill="none" d="M3.7,-85.5L3.7,85.5L13.5,85.5" stroke-linecap="round" ed:parentid="154" ed:tosuperid="168" id="169" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,559.63,539.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="168" ed:tosuperid="170" id="171" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,955.53,297.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="132" ed:tosuperid="172" id="173" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,971.22,319.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="130" ed:tosuperid="174" id="175" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,974.55,341.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="136" ed:tosuperid="176" id="177" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,983.8,363.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="134" ed:tosuperid="178" id="179" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,969.52,385.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="138" ed:tosuperid="180" id="181" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,966.64,407.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="140" ed:tosuperid="182" id="183" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,961.94,429.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="142" ed:tosuperid="184" id="185" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,714.88,233.5)" stroke="#595959" fill="none" d="M-13.5,14L3.7,14L3.7,-14L13.5,-14" stroke-linecap="round" ed:parentid="162" ed:tosuperid="186" id="187" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,798.72,219.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="186" ed:tosuperid="188" id="189" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,714.88,244.5)" stroke="#595959" fill="none" d="M3.7,3L3.7,-3L13.5,-3" stroke-linecap="round" ed:parentid="162" ed:tosuperid="190" id="191" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,805.8,241.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="190" ed:tosuperid="192" id="193" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,714.88,258.5)" stroke="#595959" fill="none" d="M3.7,-11L3.7,11L13.5,11" stroke-linecap="round" ed:parentid="162" ed:tosuperid="194" id="195" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,782.95,272.5)" stroke="#595959" fill="none" d="M-13.5,-3L3.7,-3L3.7,3L13.5,3" stroke-linecap="round" ed:parentid="194" ed:tosuperid="196" id="197" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,475.95,710)" stroke="#595959" fill="none" d="M-13.5,137.5L3.7,137.5L3.7,-137.5L13.5,-137.5" stroke-linecap="round" ed:parentid="124" ed:tosuperid="198" id="199" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,475.95,858.5)" stroke="#595959" fill="none" d="M3.7,-11L3.7,11L13.5,11" stroke-linecap="round" ed:parentid="124" ed:tosuperid="200" id="201" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,544.95,754)" stroke="#595959" fill="none" d="M-13.5,115.5L3.7,115.5L3.7,-115.5L13.5,-115.5" stroke-linecap="round" ed:parentid="200" ed:tosuperid="202" id="203" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,862.95,605.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="206" ed:tosuperid="204" id="205" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,661.95,622)" stroke="#595959" fill="none" d="M-13.5,16.5L3.7,16.5L3.7,-16.5L13.5,-16.5" stroke-linecap="round" ed:parentid="202" ed:tosuperid="206" id="207" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,886.95,627.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="210" ed:tosuperid="208" id="209" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,661.95,633)" stroke="#595959" fill="none" d="M3.7,5.5L3.7,-5.5L13.5,-5.5" stroke-linecap="round" ed:parentid="202" ed:tosuperid="210" id="211" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,661.95,644)" stroke="#595959" fill="none" d="M3.7,-5.5L3.7,5.5L13.5,5.5" stroke-linecap="round" ed:parentid="202" ed:tosuperid="214" id="215" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,862.95,649.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="214" ed:tosuperid="216" id="217" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,661.95,655)" stroke="#595959" fill="none" d="M3.7,-16.5L3.7,16.5L13.5,16.5" stroke-linecap="round" ed:parentid="202" ed:tosuperid="218" id="219" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,944.77,671.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="218" ed:tosuperid="220" id="221" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,544.95,792.5)" stroke="#595959" fill="none" d="M3.7,77L3.7,-77L13.5,-77" stroke-linecap="round" ed:parentid="200" ed:tosuperid="222" id="223" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,637.95,704.5)" stroke="#595959" fill="none" d="M-13.5,11L3.7,11L3.7,-11L13.5,-11" stroke-linecap="round" ed:parentid="222" ed:tosuperid="224" id="225" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,865.03,693.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="224" ed:tosuperid="226" id="227" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,637.95,715.5)" stroke="#595959" fill="none" d="M3.7,0L13.5,0" stroke-linecap="round" ed:parentid="222" ed:tosuperid="228" id="229" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,865.78,715.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="228" ed:tosuperid="230" id="231" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,637.95,726.5)" stroke="#595959" fill="none" d="M3.7,-11L3.7,11L13.5,11" stroke-linecap="round" ed:parentid="222" ed:tosuperid="232" id="233" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,863.38,737.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="232" ed:tosuperid="234" id="235" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,544.95,825.5)" stroke="#595959" fill="none" d="M3.7,44L3.7,-44L13.5,-44" stroke-linecap="round" ed:parentid="200" ed:tosuperid="236" id="237" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,637.95,770.5)" stroke="#595959" fill="none" d="M-13.5,11L3.7,11L3.7,-11L13.5,-11" stroke-linecap="round" ed:parentid="236" ed:tosuperid="238" id="239" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,814.95,759.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="238" ed:tosuperid="240" id="241" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,637.95,781.5)" stroke="#595959" fill="none" d="M3.7,0L13.5,0" stroke-linecap="round" ed:parentid="236" ed:tosuperid="242" id="243" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,830.7,781.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="242" ed:tosuperid="244" id="245" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,637.95,792.5)" stroke="#595959" fill="none" d="M3.7,-11L3.7,11L13.5,11" stroke-linecap="round" ed:parentid="236" ed:tosuperid="246" id="247" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,798.78,803.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="246" ed:tosuperid="248" id="249" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,544.95,853)" stroke="#595959" fill="none" d="M3.7,16.5L3.7,-16.5L13.5,-16.5" stroke-linecap="round" ed:parentid="200" ed:tosuperid="250" id="251" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,637.95,831)" stroke="#595959" fill="none" d="M-13.5,5.5L3.7,5.5L3.7,-5.5L13.5,-5.5" stroke-linecap="round" ed:parentid="250" ed:tosuperid="252" id="253" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,806.94,825.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="252" ed:tosuperid="254" id="255" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,637.95,842)" stroke="#595959" fill="none" d="M3.7,-5.5L3.7,5.5L13.5,5.5" stroke-linecap="round" ed:parentid="250" ed:tosuperid="256" id="257" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,783.34,847.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="256" ed:tosuperid="258" id="259" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,544.95,908)" stroke="#595959" fill="none" d="M3.7,-38.5L3.7,38.5L13.5,38.5" stroke-linecap="round" ed:parentid="200" ed:tosuperid="260" id="261" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,685.95,946.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="260" ed:tosuperid="262" id="263" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,836.17,946.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="262" ed:tosuperid="264" id="265" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,992.89,908)" stroke="#595959" fill="none" d="M-13.5,38.5L3.7,38.5L3.7,-38.5L13.5,-38.5" stroke-linecap="round" ed:parentid="264" ed:tosuperid="266" id="267" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,992.89,919)" stroke="#595959" fill="none" d="M3.7,27.5L3.7,-27.5L13.5,-27.5" stroke-linecap="round" ed:parentid="264" ed:tosuperid="268" id="269" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,992.89,930)" stroke="#595959" fill="none" d="M3.7,16.5L3.7,-16.5L13.5,-16.5" stroke-linecap="round" ed:parentid="264" ed:tosuperid="270" id="271" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,992.89,941)" stroke="#595959" fill="none" d="M3.7,5.5L3.7,-5.5L13.5,-5.5" stroke-linecap="round" ed:parentid="264" ed:tosuperid="272" id="273" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,992.89,952)" stroke="#595959" fill="none" d="M3.7,-5.5L3.7,5.5L13.5,5.5" stroke-linecap="round" ed:parentid="264" ed:tosuperid="274" id="275" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,992.89,963)" stroke="#595959" fill="none" d="M3.7,-16.5L3.7,16.5L13.5,16.5" stroke-linecap="round" ed:parentid="264" ed:tosuperid="276" id="277" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,992.89,974)" stroke="#595959" fill="none" d="M3.7,-27.5L3.7,27.5L13.5,27.5" stroke-linecap="round" ed:parentid="264" ed:tosuperid="282" id="283" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,992.89,985)" stroke="#595959" fill="none" d="M3.7,-38.5L3.7,38.5L13.5,38.5" stroke-linecap="round" ed:parentid="264" ed:tosuperid="286" id="287" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,544.95,963)" stroke="#595959" fill="none" d="M3.7,-93.5L3.7,93.5L13.5,93.5" stroke-linecap="round" ed:parentid="200" ed:tosuperid="288" id="289" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,661.95,1051)" stroke="#595959" fill="none" d="M-13.5,5.5L3.7,5.5L3.7,-5.5L13.5,-5.5" stroke-linecap="round" ed:parentid="288" ed:tosuperid="290" id="291" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,909,1045.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="290" ed:tosuperid="292" id="293" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,661.95,1062)" stroke="#595959" fill="none" d="M3.7,-5.5L3.7,5.5L13.5,5.5" stroke-linecap="round" ed:parentid="288" ed:tosuperid="294" id="295" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,802.95,1067.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="294" ed:tosuperid="296" id="297" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,544.95,990.5)" stroke="#595959" fill="none" d="M3.7,-121L3.7,121L13.5,121" stroke-linecap="round" ed:parentid="200" ed:tosuperid="298" id="299" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,637.95,1100.5)" stroke="#595959" fill="none" d="M-13.5,11L3.7,11L3.7,-11L13.5,-11" stroke-linecap="round" ed:parentid="298" ed:tosuperid="300" id="301" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,806.94,1089.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="300" ed:tosuperid="302" id="303" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,637.95,1111.5)" stroke="#595959" fill="none" d="M3.7,0L13.5,0" stroke-linecap="round" ed:parentid="298" ed:tosuperid="304" id="305" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,797.78,1111.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="304" ed:tosuperid="306" id="307" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,637.95,1122.5)" stroke="#595959" fill="none" d="M3.7,-11L3.7,11L13.5,11" stroke-linecap="round" ed:parentid="298" ed:tosuperid="308" id="309" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,814.95,1133.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="308" ed:tosuperid="310" id="311" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,308.17,1204.63)" stroke="#595959" fill="none" d="M3.7,-248.9L3.7,248.9L13.5,248.9" stroke-linecap="round" ed:parentid="320" ed:tosuperid="312" id="313" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,470.17,175.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="116" ed:tosuperid="314" id="315" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,407.41,1376.5)" stroke="#595959" fill="none" d="M-13.5,77L3.7,77L3.7,-77L13.5,-77" stroke-linecap="round" ed:parentid="312" ed:tosuperid="316" id="317" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,407.41,1531)" stroke="#595959" fill="none" d="M3.7,-77.5L3.7,77.5L13.5,77.5" stroke-linecap="round" ed:parentid="312" ed:tosuperid="318" id="319" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,147.5,920.5)" stroke="#595959" fill="none" d="M0.2,-35.3L28.2,-35.3L28.2,35.3L63.2,35.3" stroke-linecap="round" ed:parentid="101" ed:tosuperid="320" stroke-width="2" id="321" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,500.41,1266)" stroke="#595959" fill="none" d="M-13.5,33.5L3.7,33.5L3.7,-33.5L13.5,-33.5" stroke-linecap="round" ed:parentid="316" ed:tosuperid="322" id="323" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,795.88,1177.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="380" ed:tosuperid="324" id="325" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,726.88,1216)" stroke="#595959" fill="none" d="M3.7,16.5L3.7,-16.5L13.5,-16.5" stroke-linecap="round" ed:parentid="322" ed:tosuperid="326" id="327" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,795.88,1199.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="326" ed:tosuperid="328" id="329" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,204.21,1950.5)" stroke="#595959" fill="none" d="M0,-19.5L0,19.5" stroke-linecap="round" ed:parentid="331" ed:tosuperid="334" id="335" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,204.21,2003.5)" stroke="#595959" fill="none" d="M0,-19.5L0,19.5" stroke-linecap="round" ed:parentid="331" ed:tosuperid="336" id="337" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,204.21,2056.5)" stroke="#595959" fill="none" d="M0,-19.5L0,19.5" stroke-linecap="round" ed:parentid="331" ed:tosuperid="338" id="339" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,299.95,1977)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="334" ed:tosuperid="342" id="343" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,306.98,2030)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="336" ed:tosuperid="344" id="345" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,275.21,2083)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="338" ed:tosuperid="346" id="347" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,204.21,2109.5)" stroke="#595959" fill="none" d="M0,-19.5L0,19.5" stroke-linecap="round" ed:parentid="331" ed:tosuperid="348" id="349" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,281.21,2136)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="348" ed:tosuperid="350" id="351" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,204.21,2162.5)" stroke="#595959" fill="none" d="M0,-19.5L0,19.5" stroke-linecap="round" ed:parentid="331" ed:tosuperid="352" id="353" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,251.21,2189)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="352" ed:tosuperid="354" id="355" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,500.41,1343.5)" stroke="#595959" fill="none" d="M3.7,-44L3.7,44L13.5,44" stroke-linecap="round" ed:parentid="316" ed:tosuperid="356" id="357" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,1006.44,1221.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="362" ed:tosuperid="358" id="359" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,795.88,1243.5)" stroke="#595959" fill="none" d="M-13.5,22L3.7,22L3.7,-22L13.5,-22" stroke-linecap="round" ed:parentid="386" ed:tosuperid="362" id="363" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,795.88,1254.5)" stroke="#595959" fill="none" d="M3.7,11L3.7,-11L13.5,-11" stroke-linecap="round" ed:parentid="386" ed:tosuperid="364" id="365" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,958.63,1243.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="364" ed:tosuperid="366" id="367" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,795.88,1265.5)" stroke="#595959" fill="none" d="M3.7,0L13.5,0" stroke-linecap="round" ed:parentid="386" ed:tosuperid="368" id="369" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,964.63,1265.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="368" ed:tosuperid="370" id="371" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,795.88,1276.5)" stroke="#595959" fill="none" d="M3.7,-11L3.7,11L13.5,11" stroke-linecap="round" ed:parentid="386" ed:tosuperid="372" id="373" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,1142.06,1287.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="372" ed:tosuperid="374" id="375" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,795.88,1287.5)" stroke="#595959" fill="none" d="M3.7,-22L3.7,22L13.5,22" stroke-linecap="round" ed:parentid="386" ed:tosuperid="376" id="377" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,1085.66,1309.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="376" ed:tosuperid="378" id="379" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,726.88,1205)" stroke="#595959" fill="none" d="M3.7,27.5L3.7,-27.5L13.5,-27.5" stroke-linecap="round" ed:parentid="322" ed:tosuperid="380" id="381" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,702.88,1365.5)" stroke="#595959" fill="none" d="M-13.5,22L3.7,22L3.7,-22L13.5,-22" stroke-linecap="round" ed:parentid="356" ed:tosuperid="382" id="383" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,726.88,1194)" stroke="#595959" fill="none" d="M-13.5,38.5L3.7,38.5L3.7,-38.5L13.5,-38.5" stroke-linecap="round" ed:parentid="322" ed:tosuperid="384" id="385" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,726.88,1249)" stroke="#595959" fill="none" d="M3.7,-16.5L3.7,16.5L13.5,16.5" stroke-linecap="round" ed:parentid="322" ed:tosuperid="386" id="387" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,702.88,1382)" stroke="#595959" fill="none" d="M3.7,5.5L3.7,-5.5L13.5,-5.5" stroke-linecap="round" ed:parentid="356" ed:tosuperid="388" id="389" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,843.88,1371)" stroke="#595959" fill="none" d="M-13.5,5.5L3.7,5.5L3.7,-5.5L13.5,-5.5" stroke-linecap="round" ed:parentid="388" ed:tosuperid="390" id="391" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,702.88,1407)" stroke="#595959" fill="none" d="M3.7,-19.5L3.7,19.5L13.5,19.5" stroke-linecap="round" ed:parentid="356" ed:tosuperid="392" id="393" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,843.88,1382)" stroke="#595959" fill="none" d="M3.7,-5.5L3.7,5.5L13.5,5.5" stroke-linecap="round" ed:parentid="388" ed:tosuperid="394" id="395" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,1125.28,1387.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="394" ed:tosuperid="396" id="397" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,831.88,1421)" stroke="#595959" fill="none" d="M-13.5,5.5L3.7,5.5L3.7,-5.5L13.5,-5.5" stroke-linecap="round" ed:parentid="392" ed:tosuperid="398" id="399" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,926.23,1418.5)" stroke="#595959" fill="none" d="M-13.5,-3L3.7,-3L3.7,3L13.5,3" stroke-linecap="round" ed:parentid="398" ed:tosuperid="400" id="401" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,960.81,1443.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="404" ed:tosuperid="402" id="403" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,831.88,1435)" stroke="#595959" fill="none" d="M3.7,-8.5L3.7,8.5L13.5,8.5" stroke-linecap="round" ed:parentid="392" ed:tosuperid="404" id="405" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,500.41,1537)" stroke="#595959" fill="none" d="M-13.5,71.5L3.7,71.5L3.7,-71.5L13.5,-71.5" stroke-linecap="round" ed:parentid="318" ed:tosuperid="407" id="408" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,631.48,1465.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="407" ed:tosuperid="409" id="410" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,500.41,1553.5)" stroke="#595959" fill="none" d="M3.7,55L3.7,-55L13.5,-55" stroke-linecap="round" ed:parentid="318" ed:tosuperid="415" id="416" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,593.41,1493)" stroke="#595959" fill="none" d="M-13.5,5.5L3.7,5.5L3.7,-5.5L13.5,-5.5" stroke-linecap="round" ed:parentid="415" ed:tosuperid="417" id="418" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,593.41,1504)" stroke="#595959" fill="none" d="M3.7,-5.5L3.7,5.5L13.5,5.5" stroke-linecap="round" ed:parentid="415" ed:tosuperid="419" id="420" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,500.41,1570)" stroke="#595959" fill="none" d="M3.7,38.5L3.7,-38.5L13.5,-38.5" stroke-linecap="round" ed:parentid="318" ed:tosuperid="421" id="422" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,617.41,1531.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="421" ed:tosuperid="423" id="424" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,500.41,1581)" stroke="#595959" fill="none" d="M3.7,27.5L3.7,-27.5L13.5,-27.5" stroke-linecap="round" ed:parentid="318" ed:tosuperid="425" id="426" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,78.06,1120.57)" stroke="#595959" fill="none" d="M2.9,6.9L-2.9,-6.9" stroke-linecap="round" ed:parentid="437" ed:tosuperid="441" id="430" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,101.28,1116.74)" stroke="#595959" fill="none" d="M-0.5,7.5L0.5,-7.5" stroke-linecap="round" ed:parentid="437" ed:tosuperid="450" id="451" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,123.8,1123.6)" stroke="#595959" fill="none" d="M-3.8,6.5L3.8,-6.5" stroke-linecap="round" ed:parentid="437" ed:tosuperid="452" id="453" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,140.95,1139.72)" stroke="#595959" fill="none" d="M-6.2,4.2L6.2,-4.2" stroke-linecap="round" ed:parentid="437" ed:tosuperid="454" id="455" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,149.19,1161.77)" stroke="#595959" fill="none" d="M-7.4,1L7.4,-1" stroke-linecap="round" ed:parentid="437" ed:tosuperid="456" id="457" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,146.81,1185.19)" stroke="#595959" fill="none" d="M-7.1,-2.4L7.1,2.4" stroke-linecap="round" ed:parentid="437" ed:tosuperid="458" id="459" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,134.3,1205.13)" stroke="#595959" fill="none" d="M-5.3,-5.3L5.3,5.3" stroke-linecap="round" ed:parentid="437" ed:tosuperid="460" id="461" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,593.41,1553.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="425" ed:tosuperid="462" id="463" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,500.41,1592)" stroke="#595959" fill="none" d="M3.7,16.5L3.7,-16.5L13.5,-16.5" stroke-linecap="round" ed:parentid="318" ed:tosuperid="464" id="465" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,617.41,1575.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="464" ed:tosuperid="466" id="467" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,500.41,1608.5)" stroke="#595959" fill="none" d="M3.7,0L13.5,0" stroke-linecap="round" ed:parentid="318" ed:tosuperid="468" id="469" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,569.41,1603)" stroke="#595959" fill="none" d="M-13.5,5.5L3.7,5.5L3.7,-5.5L13.5,-5.5" stroke-linecap="round" ed:parentid="468" ed:tosuperid="470" id="471" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,772.73,1619.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="474" ed:tosuperid="472" id="473" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,569.41,1614)" stroke="#595959" fill="none" d="M3.7,-5.5L3.7,5.5L13.5,5.5" stroke-linecap="round" ed:parentid="468" ed:tosuperid="474" id="475" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,500.41,1625)" stroke="#595959" fill="none" d="M3.7,-16.5L3.7,16.5L13.5,16.5" stroke-linecap="round" ed:parentid="318" ed:tosuperid="476" id="477" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,569.41,1641.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="476" ed:tosuperid="478" id="479" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,500.41,1636)" stroke="#595959" fill="none" d="M3.7,-27.5L3.7,27.5L13.5,27.5" stroke-linecap="round" ed:parentid="318" ed:tosuperid="480" id="481" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,569.41,1663.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="480" ed:tosuperid="482" id="483" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,500.41,1658)" stroke="#595959" fill="none" d="M3.7,-49.5L3.7,49.5L13.5,49.5" stroke-linecap="round" ed:parentid="318" ed:tosuperid="484" id="485" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,690.17,1685.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="492" ed:tosuperid="486" id="487" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,669.48,1713)" stroke="#595959" fill="none" d="M-13.5,5.5L3.7,5.5L3.7,-5.5L13.5,-5.5" stroke-linecap="round" ed:parentid="494" ed:tosuperid="488" id="489" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,669.48,1724)" stroke="#595959" fill="none" d="M3.7,-5.5L3.7,5.5L13.5,5.5" stroke-linecap="round" ed:parentid="494" ed:tosuperid="490" id="491" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,593.41,1696.5)" stroke="#595959" fill="none" d="M-13.5,11L3.7,11L3.7,-11L13.5,-11" stroke-linecap="round" ed:parentid="484" ed:tosuperid="492" id="493" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,593.41,1713)" stroke="#595959" fill="none" d="M3.7,-5.5L3.7,5.5L13.5,5.5" stroke-linecap="round" ed:parentid="484" ed:tosuperid="494" id="495" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,500.41,1680)" stroke="#595959" fill="none" d="M3.7,-71.5L3.7,71.5L13.5,71.5" stroke-linecap="round" ed:parentid="318" ed:tosuperid="500" id="501" stroke-linejoin="round"/>
    <path transform="matrix(1,0,0,1,569.41,1751.5)" stroke="#595959" fill="none" d="M-13.5,0L3.7,0L13.5,0" stroke-linecap="round" ed:parentid="500" ed:tosuperid="502" id="503" stroke-linejoin="round"/>
    <g transform="matrix(1,0,0,1,21,861.75)" ed:height="47" ed:layout="rightmap" id="101" ed:topictype="mainidea" ed:width="126.671875">
        <path fill="#45a7aa" stroke="#45a7aa" d="M4,0L122.7,0C125.4,0,126.7,1.3,126.7,4L126.7,43C126.7,45.7,125.4,47,122.7,47L4,47C1.3,47,0,45.7,0,43L0,4C0,1.3,1.3,0,4,0z" stroke-linejoin="round"/>
        <text class="st4">
            <tspan x="21" y="29" style="white-space:pre">TiUP 简介</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,210.67,23.25)" ed:height="29" ed:parentid="101" id="102" ed:width="84">
        <path fill="#e0a7d3" stroke="#e0a7d3" d="M4,0L80,0C82.7,0,84,1.3,84,4L84,25C84,27.7,82.7,29,80,29L4,29C1.3,29,0,27.7,0,25L0,4C0,1.3,1.3,0,4,0z" stroke-linejoin="round"/>
        <text class="st5">
            <tspan x="18" y="19" style="white-space:pre">是什么</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,210.67,93.75)" ed:height="29" ed:parentid="101" id="104" ed:width="144">
        <path fill="#70bdf2" stroke="#70bdf2" d="M4,0L140,0C142.7,0,144,1.3,144,4L144,25C144,27.7,142.7,29,140,29L4,29C1.3,29,0,27.7,0,25L0,4C0,1.3,1.3,0,4,0z" stroke-linejoin="round"/>
        <text class="st5">
            <tspan x="18" y="19" style="white-space:pre">解决了什么问题</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,321.67,639)" ed:height="15.5" ed:parentid="320" id="106" ed:width="66">
        <path stroke="#595959" fill="none" d="M0,15.5L66,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">命令介绍</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,381.67,89.5)" ed:height="15.5" ed:parentid="104" id="108" ed:width="363.234375">
        <path stroke="#595959" fill="none" d="M0,15.5L363.2,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">在早期的 TiDB 生态中，没有专门的包管理工具，运维管理难度大</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,321.67,19)" ed:height="15.5" ed:parentid="102" id="110" ed:width="249.9375">
        <path stroke="#595959" fill="none" d="M0,15.5L249.9,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">TiDB 4.0 的生态系统里，TiUP 作为新的工具</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,321.67,41)" ed:height="15.5" ed:parentid="102" id="112" ed:width="461.296875">
        <path stroke="#595959" fill="none" d="M0,15.5L461.3,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">承担着包管理器的角色，管理着 TiDB 生态下众多的组件（例如 TiDB、PD、TiKV）</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,381.67,111.5)" ed:height="15.5" ed:parentid="104" id="114" ed:width="345.734375">
        <path stroke="#595959" fill="none" d="M0,15.5L345.7,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">像 Prometheus 等第三方监控报表工具甚至需要额外的特殊管理</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,414.67,160)" ed:height="15.5" ed:parentid="106" id="116" ed:width="42">
        <path stroke="#595959" fill="none" d="M0,15.5L42,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">安装</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,808.81,348)" ed:height="15.5" ed:parentid="166" id="118" ed:width="73.53125">
        <path stroke="#595959" fill="none" d="M0,15.5L73.5,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">Commands</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,414.67,832)" ed:height="15.5" ed:parentid="106" id="124" ed:width="47.78125">
        <path stroke="#595959" fill="none" d="M0,15.5L47.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">Usage</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,558.45,546)" ed:height="15.5" ed:parentid="198" id="126" ed:width="175.078125">
        <path stroke="#595959" fill="none" d="M0,15.5L175.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup [flags] &lt;command> [args...]</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,558.45,568)" ed:height="15.5" ed:parentid="198" id="128" ed:width="181.25">
        <path stroke="#595959" fill="none" d="M0,15.5L181.3,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup [flags] &lt;component> [args...]</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,909.34,304)" ed:height="15.5" ed:parentid="118" id="130" ed:width="48.375">
        <path stroke="#595959" fill="none" d="M0,15.5L48.4,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">install</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,909.34,282)" ed:height="15.5" ed:parentid="118" id="132" ed:width="32.6875">
        <path stroke="#595959" fill="none" d="M0,15.5L32.7,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">list</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,909.34,348)" ed:height="15.5" ed:parentid="118" id="134" ed:width="60.953125">
        <path stroke="#595959" fill="none" d="M0,15.5L61,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">uninstall</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,909.34,326)" ed:height="15.5" ed:parentid="118" id="136" ed:width="51.703125">
        <path stroke="#595959" fill="none" d="M0,15.5L51.7,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">update</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,909.34,370)" ed:height="15.5" ed:parentid="118" id="138" ed:width="46.671875">
        <path stroke="#595959" fill="none" d="M0,15.5L46.7,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">status</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,909.34,392)" ed:height="15.5" ed:parentid="118" id="140" ed:width="43.796875">
        <path stroke="#595959" fill="none" d="M0,15.5L43.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">clean</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,909.34,414)" ed:height="15.5" ed:parentid="118" id="142" ed:width="39.09375">
        <path stroke="#595959" fill="none" d="M0,15.5L39.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">help</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,808.81,469)" ed:height="15.5" ed:parentid="166" id="144" ed:width="79.703125">
        <path stroke="#595959" fill="none" d="M0,15.5L79.7,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">Components</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,915.52,436)" ed:height="15.5" ed:parentid="144" id="146" ed:width="73.46875">
        <path stroke="#595959" fill="none" d="M0,15.5L73.5,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">playground</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,915.52,480)" ed:height="15.5" ed:parentid="144" id="148" ed:width="57.828125">
        <path stroke="#595959" fill="none" d="M0,15.5L57.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">package</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,915.52,458)" ed:height="15.5" ed:parentid="144" id="150" ed:width="50.09375">
        <path stroke="#595959" fill="none" d="M0,15.5L50.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">cluster</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,915.52,502)" ed:height="15.5" ed:parentid="144" id="152" ed:width="54.140625">
        <path stroke="#595959" fill="none" d="M0,15.5L54.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">mirrors</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,414.67,353)" ed:height="15.5" ed:parentid="106" id="154" ed:width="66">
        <path stroke="#595959" fill="none" d="M0,15.5L66,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">命令组成</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,507.67,182)" ed:height="15.5" ed:parentid="154" id="156" ed:width="37.5625">
        <path stroke="#595959" fill="none" d="M0,15.5L37.6,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,572.23,182)" ed:height="15.5" ed:parentid="156" id="158" ed:width="82.84375">
        <path stroke="#595959" fill="none" d="M0,15.5L82.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">TiUP 程序名</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,507.67,232)" ed:height="15.5" ed:parentid="154" id="160" ed:width="40.703125">
        <path stroke="#595959" fill="none" d="M0,15.5L40.7,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">flags</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,575.38,232)" ed:height="15.5" ed:parentid="160" id="162" ed:width="126">
        <path stroke="#595959" fill="none" d="M0,15.5L126,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">全局通用选项，可选</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,507.67,392)" ed:height="15.5" ed:parentid="154" id="164" ed:width="133.140625">
        <path stroke="#595959" fill="none" d="M0,15.5L133.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">command / component</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,667.81,392)" ed:height="15.5" ed:parentid="164" id="166" ed:width="114">
        <path stroke="#595959" fill="none" d="M0,15.5L114,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">运行的命令或组件</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,507.67,524)" ed:height="15.5" ed:parentid="154" id="168" ed:width="38.453125">
        <path stroke="#595959" fill="none" d="M0,15.5L38.5,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">args</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,573.13,524)" ed:height="15.5" ed:parentid="168" id="170" ed:width="174">
        <path stroke="#595959" fill="none" d="M0,15.5L174,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">命令或组件的专有参数，可选</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,969.03,282)" ed:height="15.5" ed:parentid="132" id="172" ed:width="402">
        <path stroke="#595959" fill="none" d="M0,15.5L402,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">查询组件列表，知道有哪些组件可以安装，以及这些组件有哪些版本可选</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,984.72,304)" ed:height="15.5" ed:parentid="130" id="174" ed:width="150">
        <path stroke="#595959" fill="none" d="M0,15.5L150,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">安装某个组件的特定版本</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,988.05,326)" ed:height="15.5" ed:parentid="136" id="176" ed:width="162">
        <path stroke="#595959" fill="none" d="M0,15.5L162,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">升级某个组件到最新的版本</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,997.3,348)" ed:height="15.5" ed:parentid="134" id="178" ed:width="66">
        <path stroke="#595959" fill="none" d="M0,15.5L66,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">卸载组件</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,983.02,370)" ed:height="15.5" ed:parentid="138" id="180" ed:width="114">
        <path stroke="#595959" fill="none" d="M0,15.5L114,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">查看组件运行状态</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,980.14,392)" ed:height="15.5" ed:parentid="140" id="182" ed:width="90">
        <path stroke="#595959" fill="none" d="M0,15.5L90,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">清理组件实例</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,975.44,414)" ed:height="15.5" ed:parentid="142" id="184" ed:width="306">
        <path stroke="#595959" fill="none" d="M0,15.5L306,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">打印帮助信息，后面跟命令则是打印该命令的使用方法</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,728.38,204)" ed:height="15.5" ed:parentid="162" id="186" ed:width="56.84375">
        <path stroke="#595959" fill="none" d="M0,15.5L56.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">--binary</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,812.22,204)" ed:height="15.5" ed:parentid="186" id="188" ed:width="210">
        <path stroke="#595959" fill="none" d="M0,15.5L210,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">打印某个组件的可执行程序文件路径</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,728.38,226)" ed:height="15.5" ed:parentid="162" id="190" ed:width="63.921875">
        <path stroke="#595959" fill="none" d="M0,15.5L63.9,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">--binpath</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,819.3,226)" ed:height="15.5" ed:parentid="190" id="192" ed:width="402">
        <path stroke="#595959" fill="none" d="M0,15.5L402,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">指定要运行组件的可执行程序文件路径，这样可以不使用组件的安装路径</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,728.38,254)" ed:height="15.5" ed:parentid="162" id="194" ed:width="41.078125">
        <path stroke="#595959" fill="none" d="M0,15.5L41.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">--tag</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,796.45,248)" ed:height="27.5" ed:parentid="194" id="196" ed:width="514">
        <path stroke="#595959" fill="none" d="M0,27.5L514,27.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">指定组件运行实例的 tag 名称，该名称可以认为是该实例的 ID，如果不指定，则会自动生成随</tspan>
            <tspan x="8" y="22.6" style="white-space:pre">机的 tag 名称</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,489.45,557)" ed:height="15.5" ed:parentid="124" id="198" ed:width="42">
        <path stroke="#595959" fill="none" d="M0,15.5L42,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">语法</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,489.45,854)" ed:height="15.5" ed:parentid="124" id="200" ed:width="42">
        <path stroke="#595959" fill="none" d="M0,15.5L42,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">常用</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,558.45,623)" ed:height="15.5" ed:parentid="200" id="202" ed:width="90">
        <path stroke="#595959" fill="none" d="M0,15.5L90,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">查询组件列表</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,876.45,590)" ed:height="15.5" ed:parentid="206" id="204" ed:width="55.25">
        <path stroke="#595959" fill="none" d="M0,15.5L55.3,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup list</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,675.45,590)" ed:height="15.5" ed:parentid="202" id="206" ed:width="174">
        <path stroke="#595959" fill="none" d="M0,15.5L174,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">查看当前有哪些组件可以安装</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,900.45,612)" ed:height="15.5" ed:parentid="210" id="208" ed:width="128.90625">
        <path stroke="#595959" fill="none" d="M0,15.5L128.9,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup list &lt;component></tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,675.45,612)" ed:height="15.5" ed:parentid="202" id="210" ed:width="198">
        <path stroke="#595959" fill="none" d="M0,15.5L198,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">查看某个组件有哪些版本可以安装</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,675.45,634)" ed:height="15.5" ed:parentid="202" id="214" ed:width="174">
        <path stroke="#595959" fill="none" d="M0,15.5L174,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">查看当前已经安装的所有组件</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,876.45,634)" ed:height="15.5" ed:parentid="214" id="216" ed:width="107.4375">
        <path stroke="#595959" fill="none" d="M0,15.5L107.4,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup list --installed</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,675.45,656)" ed:height="15.5" ed:parentid="202" id="218" ed:width="255.8125">
        <path stroke="#595959" fill="none" d="M0,15.5L255.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">从服务器获取 TiKV 所有可安装版本组件列表</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,958.27,656)" ed:height="15.5" ed:parentid="218" id="220" ed:width="121.15625">
        <path stroke="#595959" fill="none" d="M0,15.5L121.2,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup list tikv --refresh</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,558.45,700)" ed:height="15.5" ed:parentid="200" id="222" ed:width="66">
        <path stroke="#595959" fill="none" d="M0,15.5L66,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">安装组件</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,651.45,678)" ed:height="15.5" ed:parentid="222" id="224" ed:width="200.078125">
        <path stroke="#595959" fill="none" d="M0,15.5L200.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">使用 TiUP 安装最新稳定版的 TiDB</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,878.53,678)" ed:height="15.5" ed:parentid="224" id="226" ed:width="93.390625">
        <path stroke="#595959" fill="none" d="M0,15.5L93.4,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup install tidb</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,651.45,700)" ed:height="15.5" ed:parentid="222" id="228" ed:width="200.828125">
        <path stroke="#595959" fill="none" d="M0,15.5L200.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">使用 TiUP 安装 nightly 版本的TiDB</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,879.28,700)" ed:height="15.5" ed:parentid="228" id="230" ed:width="129.859375">
        <path stroke="#595959" fill="none" d="M0,15.5L129.9,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup install tidb:nightly</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,651.45,722)" ed:height="15.5" ed:parentid="222" id="232" ed:width="198.421875">
        <path stroke="#595959" fill="none" d="M0,15.5L198.4,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">使用 TiUP 安装 v3.0.6 版本的 TiKV</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,876.88,722)" ed:height="15.5" ed:parentid="232" id="234" ed:width="122.609375">
        <path stroke="#595959" fill="none" d="M0,15.5L122.6,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup install tikv:v3.0.6</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,558.45,766)" ed:height="15.5" ed:parentid="200" id="236" ed:width="66">
        <path stroke="#595959" fill="none" d="M0,15.5L66,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">升级组件</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,651.45,744)" ed:height="15.5" ed:parentid="236" id="238" ed:width="150">
        <path stroke="#595959" fill="none" d="M0,15.5L150,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">升级所有组件至最新版本</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,828.45,744)" ed:height="15.5" ed:parentid="238" id="240" ed:width="97.328125">
        <path stroke="#595959" fill="none" d="M0,15.5L97.3,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup update --all</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,651.45,766)" ed:height="15.5" ed:parentid="236" id="242" ed:width="165.75">
        <path stroke="#595959" fill="none" d="M0,15.5L165.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">升级所有组件至 nightly 版本</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,844.2,766)" ed:height="15.5" ed:parentid="242" id="244" ed:width="141.671875">
        <path stroke="#595959" fill="none" d="M0,15.5L141.7,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup update --all --nightly</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,651.45,788)" ed:height="15.5" ed:parentid="236" id="246" ed:width="133.828125">
        <path stroke="#595959" fill="none" d="M0,15.5L133.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">升级 TiUP 至最新版本</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,812.28,788)" ed:height="15.5" ed:parentid="246" id="248" ed:width="102.390625">
        <path stroke="#595959" fill="none" d="M0,15.5L102.4,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup update --self</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,558.45,821)" ed:height="15.5" ed:parentid="200" id="250" ed:width="66">
        <path stroke="#595959" fill="none" d="M0,15.5L66,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">运行组件</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,651.45,810)" ed:height="15.5" ed:parentid="250" id="252" ed:width="141.984375">
        <path stroke="#595959" fill="none" d="M0,15.5L142,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">运行 v3.0.8 版本的 TiDB</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,820.44,810)" ed:height="15.5" ed:parentid="252" id="254" ed:width="90.390625">
        <path stroke="#595959" fill="none" d="M0,15.5L90.4,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup tidb:v3.0.8</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,651.45,832)" ed:height="15.5" ed:parentid="250" id="256" ed:width="118.390625">
        <path stroke="#595959" fill="none" d="M0,15.5L118.4,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">指定 tag 运行 TiKV</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,796.84,832)" ed:height="15.5" ed:parentid="256" id="258" ed:width="147.53125">
        <path stroke="#595959" fill="none" d="M0,15.5L147.5,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup --tag=experiment tikv</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,558.45,931)" ed:height="15.5" ed:parentid="200" id="260" ed:width="114">
        <path stroke="#595959" fill="none" d="M0,15.5L114,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">查询组件运行状态</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,699.45,931)" ed:height="15.5" ed:parentid="260" id="262" ed:width="123.21875">
        <path stroke="#595959" fill="none" d="M0,15.5L123.2,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">查看 TiDB 运行状态</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,849.67,931)" ed:height="15.5" ed:parentid="262" id="264" ed:width="129.71875">
        <path stroke="#595959" fill="none" d="M0,15.5L129.7,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup status experiment </tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,1006.39,854)" ed:height="15.5" ed:parentid="264" id="266" ed:width="135.109375">
        <path stroke="#595959" fill="none" d="M0,15.5L135.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">Name: 实例的 tag 名称</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,1006.39,876)" ed:height="15.5" ed:parentid="264" id="268" ed:width="164.953125">
        <path stroke="#595959" fill="none" d="M0,15.5L165,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">Component: 实例的组件名称</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,1006.39,898)" ed:height="15.5" ed:parentid="264" id="270" ed:width="144.34375">
        <path stroke="#595959" fill="none" d="M0,15.5L144.3,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">PID: 实例运行的进程 ID</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,1006.39,920)" ed:height="15.5" ed:parentid="264" id="272" ed:width="365.59375">
        <path stroke="#595959" fill="none" d="M0,15.5L365.6,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">Status: 实例状态，RUNNING 表示正在运行，TERM 表示已经终止</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,1006.39,942)" ed:height="15.5" ed:parentid="264" id="274" ed:width="174.140625">
        <path stroke="#595959" fill="none" d="M0,15.5L174.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">Created Time: 实例的启动时间</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,1006.39,964)" ed:height="15.5" ed:parentid="264" id="276" ed:width="266.90625">
        <path stroke="#595959" fill="none" d="M0,15.5L266.9,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">Directory: 实例的工作目录，可以通过 --tag 指定</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,1006.39,986)" ed:height="15.5" ed:parentid="264" id="282" ed:width="260.90625">
        <path stroke="#595959" fill="none" d="M0,15.5L260.9,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">Binary: 实例的可执行程序，可以通过 --binpath</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,1006.39,1008)" ed:height="15.5" ed:parentid="264" id="286" ed:width="130.125">
        <path stroke="#595959" fill="none" d="M0,15.5L130.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">Args: 实例的运行参数</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,558.45,1041)" ed:height="15.5" ed:parentid="200" id="288" ed:width="90">
        <path stroke="#595959" fill="none" d="M0,15.5L90,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">清理组件实例</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,675.45,1030)" ed:height="15.5" ed:parentid="288" id="290" ed:width="220.046875">
        <path stroke="#595959" fill="none" d="M0,15.5L220,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">清理 tag 名称为 experiment 的组件实例</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,922.5,1030)" ed:height="15.5" ed:parentid="290" id="292" ed:width="123.84375">
        <path stroke="#595959" fill="none" d="M0,15.5L123.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup clean experiment</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,675.45,1052)" ed:height="15.5" ed:parentid="288" id="294" ed:width="114">
        <path stroke="#595959" fill="none" d="M0,15.5L114,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">清理所有组件实例</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,816.45,1052)" ed:height="15.5" ed:parentid="294" id="296" ed:width="89.421875">
        <path stroke="#595959" fill="none" d="M0,15.5L89.4,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup clean --all</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,558.45,1096)" ed:height="15.5" ed:parentid="200" id="298" ed:width="66">
        <path stroke="#595959" fill="none" d="M0,15.5L66,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">卸载组件</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,651.45,1074)" ed:height="15.5" ed:parentid="298" id="300" ed:width="141.984375">
        <path stroke="#595959" fill="none" d="M0,15.5L142,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">卸载 v3.0.8 版本的 TiDB</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,820.44,1074)" ed:height="15.5" ed:parentid="300" id="302" ed:width="136.34375">
        <path stroke="#595959" fill="none" d="M0,15.5L136.3,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup uninstall tidb:v3.0.8</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,651.45,1096)" ed:height="15.5" ed:parentid="298" id="304" ed:width="132.828125">
        <path stroke="#595959" fill="none" d="M0,15.5L132.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">卸载所有版本的 TiKV</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,811.28,1096)" ed:height="15.5" ed:parentid="304" id="306" ed:width="127.875">
        <path stroke="#595959" fill="none" d="M0,15.5L127.9,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup uninstall tikv --all</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,651.45,1118)" ed:height="15.5" ed:parentid="298" id="308" ed:width="150">
        <path stroke="#595959" fill="none" d="M0,15.5L150,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">卸载所有已经安装的组件</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,828.45,1118)" ed:height="15.5" ed:parentid="308" id="310" ed:width="106.578125">
        <path stroke="#595959" fill="none" d="M0,15.5L106.6,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup uninstall --all</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,321.67,1438)" ed:height="15.5" ed:parentid="320" id="312" ed:width="72.234375">
        <path stroke="#595959" fill="none" d="M0,15.5L72.2,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">部署 TiDB</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,483.67,160)" ed:height="15.5" ed:parentid="116" id="314" ed:width="415.421875">
        <path stroke="#595959" fill="none" d="M0,15.5L415.4,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">curl --proto '=https' --tlsv1.2 -sSf https://tiup-mirrors.pingcap.com/install.sh | sh</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,420.91,1284)" ed:height="15.5" ed:parentid="312" id="316" ed:width="66">
        <path stroke="#595959" fill="none" d="M0,15.5L66,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">测试环境</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,420.91,1593)" ed:height="15.5" ed:parentid="312" id="318" ed:width="66">
        <path stroke="#595959" fill="none" d="M0,15.5L66,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">生产集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,210.67,941.25)" ed:height="29" ed:parentid="101" id="320" ed:width="84">
        <path fill="#e38d76" stroke="#e38d76" d="M4,0L80,0C82.7,0,84,1.3,84,4L84,25C84,27.7,82.7,29,80,29L4,29C1.3,29,0,27.7,0,25L0,4C0,1.3,1.3,0,4,0z" stroke-linejoin="round"/>
        <text class="st5">
            <tspan x="18" y="19" style="white-space:pre">怎么用</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,513.91,1217)" ed:height="15.5" ed:parentid="316" id="322" ed:width="199.46875">
        <path stroke="#595959" fill="none" d="M0,15.5L199.5,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">通过 playground 组件启动本地集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,809.38,1162)" ed:height="15.5" ed:parentid="380" id="324" ed:width="129.40625">
        <path stroke="#595959" fill="none" d="M0,15.5L129.4,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup install playground</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,740.38,1184)" ed:height="15.5" ed:parentid="322" id="326" ed:width="42">
        <path stroke="#595959" fill="none" d="M0,15.5L42,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">启动</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,809.38,1184)" ed:height="15.5" ed:parentid="326" id="328" ed:width="96.03125">
        <path stroke="#595959" fill="none" d="M0,15.5L96,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup playground</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,90.03,1902)" ed:height="29" ed:layout="map" id="331" ed:topictype="floating" ed:width="228.375">
        <path fill="#50bec1" stroke="#50bec1" d="M4,0L224.4,0C227.1,0,228.4,1.3,228.4,4L228.4,25C228.4,27.7,227.1,29,224.4,29L4,29C1.3,29,0,27.7,0,25L0,4C0,1.3,1.3,0,4,0z" stroke-linejoin="round"/>
        <text class="st5">
            <tspan x="18" y="19" style="white-space:pre">快速启动一个playground集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,121.98,1970)" ed:height="14" ed:parentid="331" ed:layout="rightmap" id="334" ed:width="164.46875">
        <text class="st6">
            <tspan x="9" y="11" style="white-space:pre">查找 playground 的最新版本</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,114.95,2023)" ed:height="14" ed:parentid="331" ed:layout="rightmap" id="336" ed:width="178.53125">
        <text class="st6">
            <tspan x="9" y="11" style="white-space:pre">使用各组件的最新 release 版本</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,146.71,2076)" ed:height="14" ed:parentid="331" ed:layout="rightmap" id="338" ed:width="115">
        <text class="st6">
            <tspan x="9" y="11" style="white-space:pre">使用默认组件个数</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,313.45,1970)" ed:height="14" ed:parentid="334" ed:layout="rightmap" id="342" ed:width="127.40625">
        <text class="st6">
            <tspan x="9" y="11" style="white-space:pre">tiup playground:v0.0.6</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,320.48,2023)" ed:height="14" ed:parentid="336" ed:layout="rightmap" id="344" ed:width="158.15625">
        <text class="st6">
            <tspan x="9" y="11" style="white-space:pre">tiup playground:v0.0.6 v4.0.0</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,288.71,2076)" ed:height="14" ed:parentid="338" ed:layout="rightmap" id="346" ed:width="328.921875">
        <text class="st6">
            <tspan x="9" y="11" style="white-space:pre">启动由 1 个 TiDB、1 个 TiKV 和 1 个 PD 构成的最小化集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,140.71,2129)" ed:height="14" ed:parentid="331" ed:layout="rightmap" id="348" ed:width="127">
        <text class="st6">
            <tspan x="9" y="11" style="white-space:pre">依次启动完各个组件</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,294.71,2123)" ed:height="26" ed:parentid="348" ed:layout="rightmap" id="350" ed:width="515">
        <text class="st6">
            <tspan x="9" y="11" style="white-space:pre">调用 TiUP 命令来启动 TiDB/PD/TiKV 组件，譬如调用 tiup tidb:v4.0.0 来启动 TiDB 实例，当</tspan>
            <tspan x="9" y="23" style="white-space:pre">然，在真正执行时它还会额外指定一些参数</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,170.71,2182)" ed:height="14" ed:parentid="331" ed:layout="rightmap" id="352" ed:width="67">
        <text class="st6">
            <tspan x="9" y="11" style="white-space:pre">启动成功</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,264.71,2176)" ed:height="26" ed:parentid="352" ed:layout="rightmap" id="354" ed:width="515">
        <text class="st6">
            <tspan x="9" y="11" style="white-space:pre">在依次启动完各个组件后，playground 会告诉你启动成功，并告诉你一些有用的信息，譬如如</tspan>
            <tspan x="9" y="23" style="white-space:pre">何通过 MySQL 客户端连接集群、如何访问 dashboard</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,513.91,1372)" ed:height="15.5" ed:parentid="316" id="356" ed:width="175.46875">
        <path stroke="#595959" fill="none" d="M0,15.5L175.5,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">通过 playground 搭建测试集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,1019.94,1206)" ed:height="15.5" ed:parentid="362" id="358" ed:width="180.375">
        <path stroke="#595959" fill="none" d="M0,15.5L180.4,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup --tag=my-cluster playground</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,809.38,1206)" ed:height="15.5" ed:parentid="386" id="362" ed:width="183.5625">
        <path stroke="#595959" fill="none" d="M0,15.5L183.6,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">通过指定 tag 名称的方法来启动</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,809.38,1228)" ed:height="15.5" ed:parentid="386" id="364" ed:width="135.75">
        <path stroke="#595959" fill="none" d="M0,15.5L135.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">启动 v3.0.9 版本的集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,972.13,1228)" ed:height="15.5" ed:parentid="364" id="366" ed:width="126.78125">
        <path stroke="#595959" fill="none" d="M0,15.5L126.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup playground v3.0.9</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,809.38,1250)" ed:height="15.5" ed:parentid="386" id="368" ed:width="141.75">
        <path stroke="#595959" fill="none" d="M0,15.5L141.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">启动 nightly 版本的集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,978.13,1250)" ed:height="15.5" ed:parentid="368" id="370" ed:width="132.875">
        <path stroke="#595959" fill="none" d="M0,15.5L132.9,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup playground nightly</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,809.38,1272)" ed:height="15.5" ed:parentid="386" id="372" ed:width="319.1875">
        <path stroke="#595959" fill="none" d="M0,15.5L319.2,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">指定 TiKV 组件的个数为 3 个，同时启动 Prometheus 监控</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,1155.56,1272)" ed:height="15.5" ed:parentid="372" id="374" ed:width="181.328125">
        <path stroke="#595959" fill="none" d="M0,15.5L181.3,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup playground --kv=3 --monitor</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,809.38,1294)" ed:height="15.5" ed:parentid="386" id="376" ed:width="262.78125">
        <path stroke="#595959" fill="none" d="M0,15.5L262.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">各组件使用本机的对外 IP 地址 x.x.x.x 提供服务</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,1099.16,1294)" ed:height="15.5" ed:parentid="376" id="378" ed:width="160.046875">
        <path stroke="#595959" fill="none" d="M0,15.5L160,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup playground --host x.x.x.x</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,740.38,1162)" ed:height="15.5" ed:parentid="322" id="380" ed:width="42">
        <path stroke="#595959" fill="none" d="M0,15.5L42,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">安装</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,716.38,1316)" ed:height="27.5" ed:parentid="356" id="382" ed:width="514">
        <path stroke="#595959" fill="none" d="M0,27.5L514,27.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">作为一个分布式系统，最基础的 TiDB 测试集群通常由 2 个 TiDB 组件、3 个 TiKV 组件和 3 个</tspan>
            <tspan x="8" y="22.6" style="white-space:pre">PD组件来构成</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,740.38,1140)" ed:height="15.5" ed:parentid="322" id="384" ed:width="351.921875">
        <path stroke="#595959" fill="none" d="M0,15.5L351.9,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">快速启动由 1 个 TiDB、1 个 TiKV 和 1 个 PD 构成的最小化集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,740.38,1250)" ed:height="15.5" ed:parentid="322" id="386" ed:width="42">
        <path stroke="#595959" fill="none" d="M0,15.5L42,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">拓展</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,716.38,1361)" ed:height="15.5" ed:parentid="356" id="388" ed:width="114">
        <path stroke="#595959" fill="none" d="M0,15.5L114,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">部署基础测试集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,857.38,1350)" ed:height="15.5" ed:parentid="388" id="390" ed:width="204.46875">
        <path stroke="#595959" fill="none" d="M0,15.5L204.5,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup playground --db=2 --kv=3 --pd=3</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,716.38,1411)" ed:height="15.5" ed:parentid="356" id="392" ed:width="102">
        <path stroke="#595959" fill="none" d="M0,15.5L102,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">连接到测试集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,857.38,1372)" ed:height="15.5" ed:parentid="388" id="394" ed:width="254.40625">
        <path stroke="#595959" fill="none" d="M0,15.5L254.4,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup playground --db=2 --kv=3 --pd=3 --monitor</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,1138.78,1372)" ed:height="15.5" ed:parentid="394" id="396" ed:width="90">
        <path stroke="#595959" fill="none" d="M0,15.5L90,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">带监控功能的</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,845.38,1400)" ed:height="15.5" ed:parentid="392" id="398" ed:width="67.359375">
        <path stroke="#595959" fill="none" d="M0,15.5L67.4,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup client</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,939.73,1394)" ed:height="27.5" ed:parentid="398" id="400" ed:width="514">
        <path stroke="#595959" fill="none" d="M0,27.5L514,27.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">自动探测到当前启动了哪些集群，并展示一个类似 DOS 图形化界面的列表，让你选择连接哪</tspan>
            <tspan x="8" y="22.6" style="white-space:pre">个集群。</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,974.31,1428)" ed:height="15.5" ed:parentid="404" id="402" ed:width="195.5625">
        <path stroke="#595959" fill="none" d="M0,15.5L195.6,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">指定 tag 名称来连接到特定的集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,845.38,1428)" ed:height="15.5" ed:parentid="392" id="404" ed:width="101.9375">
        <path stroke="#595959" fill="none" d="M0,15.5L101.9,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup client &lt;tag></tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,513.91,1450)" ed:height="15.5" ed:parentid="318" id="407" ed:width="104.078125">
        <path stroke="#595959" fill="none" d="M0,15.5L104.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">安装 cluster 组件</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,644.98,1450)" ed:height="15.5" ed:parentid="407" id="409" ed:width="106.03125">
        <path stroke="#595959" fill="none" d="M0,15.5L106,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup install cluster</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,513.91,1483)" ed:height="15.5" ed:parentid="318" id="415" ed:width="66">
        <path stroke="#595959" fill="none" d="M0,15.5L66,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">部署集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,606.91,1472)" ed:height="15.5" ed:parentid="415" id="417" ed:width="363.75">
        <path stroke="#595959" fill="none" d="M0,15.5L363.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup cluster deploy &lt;cluster-name> &lt;version> &lt;topology.yaml> [flags]</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,606.91,1494)" ed:height="15.5" ed:parentid="415" id="419" ed:width="309.09375">
        <path stroke="#595959" fill="none" d="M0,15.5L309.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup cluster deploy prod-cluster v3.0.12 /tmp/topology.yaml</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,513.91,1516)" ed:height="15.5" ed:parentid="318" id="421" ed:width="90">
        <path stroke="#595959" fill="none" d="M0,15.5L90,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">查看集群列表</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,630.91,1516)" ed:height="15.5" ed:parentid="421" id="423" ed:width="90.34375">
        <path stroke="#595959" fill="none" d="M0,15.5L90.3,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup cluster list</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,513.91,1538)" ed:height="15.5" ed:parentid="318" id="425" ed:width="66">
        <path stroke="#595959" fill="none" d="M0,15.5L66,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">启动集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,53.61,1124.13)" ed:height="88.5" ed:layout="map" id="437" ed:topictype="floating" ed:width="88.5">
        <path fill="#50bec1" stroke="#50bec1" d="M0,44.3C0,19.8,19.8,0,44.3,0C68.7,0,88.5,19.8,88.5,44.3C88.5,68.7,68.7,88.5,44.3,88.5C19.8,88.5,0,68.7,0,44.3z" stroke-linejoin="round"/>
        <text class="st5">
            <tspan x="13" y="48.8" style="white-space:pre">生产集群</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,53.75,1083.82)" ed:height="31" ed:parentid="437" id="441" ed:width="31">
        <path fill="#ffffff" stroke="#595959" d="M0,15.5C0,6.9,6.9,0,15.5,0C24.1,0,31,6.9,31,15.5C31,24.1,24.1,31,15.5,31C6.9,31,0,24.1,0,15.5z" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="2" y="18.5" style="white-space:pre">部署</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,87.3,1078.29)" ed:height="31" ed:parentid="437" id="450" ed:width="31">
        <path fill="#ffffff" stroke="#595959" d="M0,15.5C0,6.9,6.9,0,15.5,0C24.1,0,31,6.9,31,15.5C31,24.1,24.1,31,15.5,31C6.9,31,0,24.1,0,15.5z" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="2" y="18.5" style="white-space:pre">启动</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,119.83,1088.2)" ed:height="31" ed:parentid="437" id="452" ed:width="31">
        <path fill="#ffffff" stroke="#595959" d="M0,15.5C0,6.9,6.9,0,15.5,0C24.1,0,31,6.9,31,15.5C31,24.1,24.1,31,15.5,31C6.9,31,0,24.1,0,15.5z" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="2" y="18.5" style="white-space:pre">停止</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,144.6,1111.48)" ed:height="31" ed:parentid="437" id="454" ed:width="31">
        <path fill="#ffffff" stroke="#595959" d="M0,15.5C0,6.9,6.9,0,15.5,0C24.1,0,31,6.9,31,15.5C31,24.1,24.1,31,15.5,31C6.9,31,0,24.1,0,15.5z" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="2" y="18.5" style="white-space:pre">重启</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,156.5,1143.34)" ed:height="31" ed:parentid="437" id="456" ed:width="31">
        <path fill="#ffffff" stroke="#595959" d="M0,15.5C0,6.9,6.9,0,15.5,0C24.1,0,31,6.9,31,15.5C31,24.1,24.1,31,15.5,31C6.9,31,0,24.1,0,15.5z" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="2" y="18.5" style="white-space:pre">缩容</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,153.06,1177.16)" ed:height="31" ed:parentid="437" id="458" ed:width="31">
        <path fill="#ffffff" stroke="#595959" d="M0,15.5C0,6.9,6.9,0,15.5,0C24.1,0,31,6.9,31,15.5C31,24.1,24.1,31,15.5,31C6.9,31,0,24.1,0,15.5z" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="2" y="18.5" style="white-space:pre">扩容</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,134.99,1205.96)" ed:height="31" ed:parentid="437" id="460" ed:width="31">
        <path fill="#ffffff" stroke="#595959" d="M0,15.5C0,6.9,6.9,0,15.5,0C24.1,0,31,6.9,31,15.5C31,24.1,24.1,31,15.5,31C6.9,31,0,24.1,0,15.5z" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="2" y="18.5" style="white-space:pre">升级</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,606.91,1538)" ed:height="15.5" ed:parentid="425" id="462" ed:width="159.8125">
        <path stroke="#595959" fill="none" d="M0,15.5L159.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup cluster start prod-cluster</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,513.91,1560)" ed:height="15.5" ed:parentid="318" id="464" ed:width="90">
        <path stroke="#595959" fill="none" d="M0,15.5L90,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">查看集群状态</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,630.91,1560)" ed:height="15.5" ed:parentid="464" id="466" ed:width="171.75">
        <path stroke="#595959" fill="none" d="M0,15.5L171.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup cluster display prod-cluster</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,513.91,1593)" ed:height="15.5" ed:parentid="318" id="468" ed:width="42">
        <path stroke="#595959" fill="none" d="M0,15.5L42,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">缩容</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,582.91,1582)" ed:height="15.5" ed:parentid="468" id="470" ed:width="226.890625">
        <path stroke="#595959" fill="none" d="M0,15.5L226.9,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup cluster scale-in &lt;cluster-name> [flags]</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,786.23,1604)" ed:height="15.5" ed:parentid="474" id="472" ed:width="282.796875">
        <path stroke="#595959" fill="none" d="M0,15.5L282.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup cluster scale-in prod-cluster -N 172.16.5.140:20160</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,582.91,1604)" ed:height="15.5" ed:parentid="468" id="474" ed:width="176.328125">
        <path stroke="#595959" fill="none" d="M0,15.5L176.3,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">将 172.16.5.140 上的 TiKV 干掉</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,513.91,1626)" ed:height="15.5" ed:parentid="318" id="476" ed:width="42">
        <path stroke="#595959" fill="none" d="M0,15.5L42,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">扩容</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,582.91,1626)" ed:height="15.5" ed:parentid="476" id="478" ed:width="298.671875">
        <path stroke="#595959" fill="none" d="M0,15.5L298.7,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">cluster scale-out &lt;cluster-name> &lt;topology.yaml> [flags]</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,513.91,1648)" ed:height="15.5" ed:parentid="318" id="480" ed:width="42">
        <path stroke="#595959" fill="none" d="M0,15.5L42,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">升级</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,582.91,1648)" ed:height="15.5" ed:parentid="480" id="482" ed:width="221.046875">
        <path stroke="#595959" fill="none" d="M0,15.5L221,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup cluster upgrade prod-cluster v4.0.0-rc</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,513.91,1692)" ed:height="15.5" ed:parentid="318" id="484" ed:width="66">
        <path stroke="#595959" fill="none" d="M0,15.5L66,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">更新配置</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,703.67,1670)" ed:height="15.5" ed:parentid="492" id="486" ed:width="189.15625">
        <path stroke="#595959" fill="none" d="M0,15.5L189.2,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup cluster edit-config prod-cluster</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,682.98,1692)" ed:height="15.5" ed:parentid="494" id="488" ed:width="168.46875">
        <path stroke="#595959" fill="none" d="M0,15.5L168.5,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup cluster reload prod-cluster</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,682.98,1714)" ed:height="15.5" ed:parentid="494" id="490" ed:width="205.171875">
        <path stroke="#595959" fill="none" d="M0,15.5L205.2,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">tiup cluster reload prod-cluster -R tidb</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,606.91,1670)" ed:height="15.5" ed:parentid="484" id="492" ed:width="69.765625">
        <path stroke="#595959" fill="none" d="M0,15.5L69.8,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">edit-config</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,606.91,1703)" ed:height="15.5" ed:parentid="484" id="494" ed:width="49.078125">
        <path stroke="#595959" fill="none" d="M0,15.5L49.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">reload</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,513.91,1736)" ed:height="15.5" ed:parentid="318" id="500" ed:width="42">
        <path stroke="#595959" fill="none" d="M0,15.5L42,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">其他</tspan>
        </text>
    </g>
    <g transform="matrix(1,0,0,1,582.91,1736)" ed:height="15.5" ed:parentid="500" id="502" ed:width="111.09375">
        <path stroke="#595959" fill="none" d="M0,15.5L111.1,15.5" stroke-linejoin="round"/>
        <text class="st6">
            <tspan x="8" y="10.7" style="white-space:pre">通过help帮助获取</tspan>
        </text>
    </g>
</svg>
</div>
        <div id="copyright">Created With  <a href="https://www.toberoot.com/" target="_blank" title="edrawsoft">BoobooWei</a></div>
      </div>
    </div>
    <script>eval(atob('dmFyIG11YT13aW5kb3cubmF2aWdhdG9yLnVzZXJBZ2VudDsNCnZhciB1YT0obXVhLmluZGV4T2YoJ3J2OjExJykrbXVhLmluZGV4T2YoJ01TSUUnKSk+PTA7DQpkb2N1bWVudC5nZXRFbGVtZW50QnlJZCgnc3ZnLWNvbnRhaW5lcicpLm9uY29udGV4dG1lbnUgPSBmdW5jdGlvbiAoZXZlbnQpIHsNCiAgICBldmVudC5wcmV2ZW50RGVmYXVsdCgpOw0KfQ0KZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoJ3N2Zy1jb250YWluZXInKS5vbm1vdXNlZG93biA9IGZ1bmN0aW9uIChldmVudCkgew0KICAgIGlmKGV2ZW50LndoaWNoID09Myl7DQogICAgICAgIHRoaXMuc3R5bGUuY3Vyc29yID0gJ3BvaW50ZXInOw0KICAgICAgICB0aGlzLm9ubW91c2Vtb3ZlID0gZnVuY3Rpb24gKGV2KSB7DQogICAgICAgICAgICB0aGlzLnNjcm9sbEJ5KC0oZXYubW92ZW1lbnRYKSwgMCk7DQogICAgICAgICAgICB3aW5kb3cuc2Nyb2xsQnkoMCwtKGV2Lm1vdmVtZW50WSkpDQogICAgICAgIH0NCiAgICAgICAgdGhpcy5vbm1vdXNldXAgPSBmdW5jdGlvbiAoKSB7DQogICAgICAgICAgICB0aGlzLnN0eWxlLmN1cnNvciA9ICBudWxsOw0KICAgICAgICAgICAgdGhpcy5vbm1vdXNldXAgPSBudWxsOw0KICAgICAgICAgICAgdGhpcy5vbm1vdXNlbW92ZSA9IG51bGw7DQogICAgICAgIH0NCiAgICB9DQp9DQpOdW1iZXIucHJvdG90eXBlLnRvc3VpdHN2Zz1mdW5jdGlvbiAoKSB7DQogICAgdmFyIG51bT10aGlzLnZhbHVlT2YoKTsNCiAgICBpZihudW0lMT09PTApew0KICAgICAgICByZXR1cm4gbnVtKzAuNQ0KICAgIH1lbHNlIHJldHVybiBudW07DQp9Ow0KTnVtYmVyLnByb3RvdHlwZS5wbHVzej1mdW5jdGlvbigpIHsNCiAgICB2YXIgbnVtPXRoaXMudmFsdWVPZigpOw0KICAgIHJldHVybiBudW08MTA/JzAnK251bTpudW07DQp9Ow0KZnVuY3Rpb24gcGFyc2VEYXRlKG51bSkgew0KICAgIHZhciBkYXRlID0gbmV3IERhdGUobnVtKTsNCiAgICB2YXIgWSA9IGRhdGUuZ2V0RnVsbFllYXIoKSArICctJzsNCiAgICB2YXIgTSA9IChkYXRlLmdldE1vbnRoKCkrMSkucGx1c3ooKSArICctJzsNCiAgICB2YXIgRCA9IGRhdGUuZ2V0RGF0ZSgpLnBsdXN6KCkgKyAnICc7DQogICAgdmFyIGggPSBkYXRlLmdldEhvdXJzKCkucGx1c3ooKSArICc6JzsNCiAgICB2YXIgbW0gPSBkYXRlLmdldE1pbnV0ZXMoKS5wbHVzeigpICsgJzonOw0KICAgIHZhciBzID0gZGF0ZS5nZXRTZWNvbmRzKCkucGx1c3ooKTsNCiAgICByZXR1cm4gWStNK0QraCttbStzOw0KfQ0KLy8tLXByZWRlZmluZWQNCi8vY29tbWVudC0tDQp2YXIgY29tbWVudHM9ZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgnZz5nW2VkXFw6Y29tbWVudF0nKTsNCmZ1bmN0aW9uIGdldGN3aChwb3B1cCkgew0KICAgIGRvY3VtZW50LmJvZHkuZ2V0RWxlbWVudHNCeVRhZ05hbWUoJ3N2ZycpWzBdLmFwcGVuZENoaWxkKHBvcHVwKTsNCiAgICB2YXIgdz1wb3B1cC5nZXRCb3VuZGluZ0NsaWVudFJlY3QoKS53aWR0aDsNCiAgICB2YXIgaD1wb3B1cC5nZXRCb3VuZGluZ0NsaWVudFJlY3QoKS5oZWlnaHQ7DQogICAgcmV0dXJuIFt3LGhdDQp9DQpmb3IodmFyIGk9MDtpPGNvbW1lbnRzLmxlbmd0aDtpKyspew0KICAgIHZhciBwb3B1cD1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywnZycpOw0KICAgIHZhciBwb3B1cFI9IGRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgdmFyIGhvdmVyPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgdmFyIG9saW5lPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCdmaWxsJywnI2NkY2RmZicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgneCcsJzAnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3knLCcwJyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCdoZWlnaHQnLCcxNicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnd2lkdGgnLCcxNicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbC1vcGFjaXR5JywnMC42Jyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nLGNvbW1lbnRzW2ldLnF1ZXJ5U2VsZWN0b3IoJ3VzZScpLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJykpOw0KICAgIGhvdmVyLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIGNvbW1lbnRzW2ldLmFwcGVuZENoaWxkKGhvdmVyKTsNCiAgICB2YXIgYT1KU09OLnBhcnNlKGNvbW1lbnRzW2ldLmdldEF0dHJpYnV0ZSgnZWQ6Y29tbWVudCcpKTsNCiAgICB2YXIgaGVpZ2h0PTA7DQogICAgdmFyIGNhcnI9W107DQogICAgZm9yKHZhciBqPTA7ajxhLmxlbmd0aDtqKyspew0KICAgICAgICB2YXIgc3RhbXA9TnVtYmVyKGFbal0uRGF0ZSkqMTAwMDsNCiAgICAgICAgdmFyIHRpbWU9cGFyc2VEYXRlKHN0YW1wKTsNCiAgICAgICAgdmFyIG5hbWU9YVtqXS5OYW1lOw0KICAgICAgICB2YXIgbWVzc2FnZT1hW2pdLk1lc3NhZ2U7DQogICAgICAgIHZhciBtZXNzYWdlQXJyPW1lc3NhZ2Uuc3BsaXQoL1xuLyk7DQogICAgICAgIHZhciBvPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdnJyk7DQogICAgICAgIHZhciBuPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIHZhciB0PWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIHZhciBtPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIG4uc2V0QXR0cmlidXRlKCd4Jyw1KTsNCiAgICAgICAgbi5zZXRBdHRyaWJ1dGUoJ3knLDEyKTsNCiAgICAgICAgbi5zZXRBdHRyaWJ1dGUoJ2ZpbGwnLCcjMDA2ZWZmJyk7DQogICAgICAgIG4udGV4dENvbnRlbnQ9bmFtZSsnOiAnOw0KICAgICAgICBuLnNldEF0dHJpYnV0ZSgnZm9udC1zaXplJywnMTInKTsNCiAgICAgICAgdC5zZXRBdHRyaWJ1dGUoJ3gnLDIwMCk7DQogICAgICAgIHQuc2V0QXR0cmlidXRlKCd5JywxMik7DQogICAgICAgIHQuc2V0QXR0cmlidXRlKCdmaWxsJywnIzk2OTY5NicpOw0KICAgICAgICB0LnRleHRDb250ZW50PXRpbWU7DQogICAgICAgIHQuc2V0QXR0cmlidXRlKCdmb250LXNpemUnLCcxMCcpOw0KICAgICAgICBtLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJywndHJhbnNsYXRlKDIwLDI3KScpOw0KICAgICAgICBtLnNldEF0dHJpYnV0ZSgnZm9udC1zaXplJywnMTInKTsNCiAgICAgICAgZm9yKHZhciBrPTA7azxtZXNzYWdlQXJyLmxlbmd0aDtrKyspew0KICAgICAgICAgICAgdmFyIHRzPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0c3BhbicpOw0KICAgICAgICAgICAgdHMuc2V0QXR0cmlidXRlKCd4JywnMCcpOw0KICAgICAgICAgICAgdHMuc2V0QXR0cmlidXRlKCd5JyxrKjE2KTsNCiAgICAgICAgICAgIHRzLnRleHRDb250ZW50PW1lc3NhZ2VBcnJba107DQogICAgICAgICAgICBtLmFwcGVuZENoaWxkKHRzKTsNCiAgICAgICAgfQ0KICAgICAgICBvLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJywndHJhbnNsYXRlKDAsJytoZWlnaHQrJyknKTsNCiAgICAgICAgby5hcHBlbmRDaGlsZChuKTsNCiAgICAgICAgby5hcHBlbmRDaGlsZCh0KTsNCiAgICAgICAgby5hcHBlbmRDaGlsZChtKTsNCiAgICAgICAgY2Fyci5wdXNoKG8pOw0KICAgICAgICBwb3B1cC5hcHBlbmRDaGlsZChvKTsNCiAgICAgICAgaGVpZ2h0PShtZXNzYWdlQXJyLmxlbmd0aCsxKSoxNitoZWlnaHQ7DQogICAgfQ0KICAgIHZhciB3YXJyPWdldGN3aChwb3B1cCk7DQogICAgb2xpbmUuc2V0QXR0cmlidXRlKCd4JywnMCcpOw0KICAgIG9saW5lLnNldEF0dHJpYnV0ZSgneScsJzAnKTsNCiAgICB2YXIgb3c9d2FyclswXSsxMC41Ow0KICAgIHZhciBvaD13YXJyWzFdKzM7DQogICAgb2xpbmUuc2V0QXR0cmlidXRlKCd3aWR0aCcsb3cpOw0KICAgIG9saW5lLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JyxvaCk7DQogICAgb2xpbmUuc2V0QXR0cmlidXRlKCdmaWxsJywnd2hpdGUnKTsNCiAgICBvbGluZS5zZXRBdHRyaWJ1dGUoJ3N0cm9rZScsJyM2NTY1NjUnKTsNCiAgICBwb3B1cC5hcHBlbmRDaGlsZChvbGluZSk7DQogICAgdmFyIGw9Y2Fyci5sZW5ndGg7DQogICAgd2hpbGUobC0tKXsNCiAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQoY2FycltsXSk7DQogICAgfQ0KICAgIHBvcHVwLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdibG9jayc7DQogICAgfTsNCiAgICBwb3B1cC5vbm1vdXNlb3V0PWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5ID0gJ25vbmUnOw0KICAgIH07DQogICAgdmFyIGNzPWNvbW1lbnRzW2ldLnF1ZXJ5U2VsZWN0b3IoJ3VzZScpLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJykubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICB2YXIgcHM9Y29tbWVudHNbaV0ucGFyZW50Tm9kZS5nZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScpOw0KICAgIGlmKHBzLnN1YnN0cigwLDIpID09ICd0cicpew0KICAgICAgICB2YXIgcHBzID0gcHMubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICAgICAgdmFyIHg9cGFyc2VGbG9hdChjc1swXSkrcGFyc2VGbG9hdChwcHNbMF0pOw0KICAgICAgICB2YXIgeT1wYXJzZUZsb2F0KHBwc1sxXSk7DQogICAgICAgIHg9eC50b3N1aXRzdmcoKTsNCiAgICAgICAgeT15LnRvc3VpdHN2ZygpOw0KICAgICAgICB2YXIgdHJzdHIgPSAndHJhbnNsYXRlKCcreCsnLCcreSsnKSc7DQogICAgfQ0KICAgIGVsc2UgaWYocHMuc3Vic3RyKDAsMikgPT0gJ21hJyl7DQogICAgICAgIHZhciBwcHMgPSBwcy5tYXRjaCgvKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVtcLCBdKFwtP1xkKyhcLlxkKyk/KVwpJC8pOw0KICAgICAgICB2YXIgbWFBcnIgPSBbcGFyc2VGbG9hdChwcHNbMV0pLHBhcnNlRmxvYXQocHBzWzNdKSxwYXJzZUZsb2F0KHBwc1s1XSkscGFyc2VGbG9hdChwcHNbN10pLHBhcnNlRmxvYXQocHBzWzldKSxwYXJzZUZsb2F0KHBwc1sxMV0pXTsNCiAgICAgICAgaWYobWFBcnJbMV0gPT0gMCl7DQogICAgICAgICAgICB2YXIgeCA9IHBhcnNlRmxvYXQoY3NbMF0pOw0KICAgICAgICAgICAgdmFyIHk9IHBhcnNlRmxvYXQoY3NbMV0pKzE2Ow0KICAgICAgICAgICAgdmFyIHgxID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTEgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHgxPXgxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgeTE9eTEudG9zdWl0c3ZnKCk7DQogICAgICAgICAgICB2YXIgdHJzdHIgPSAgJ3RyYW5zbGF0ZSgnK3gxKycsJyt5MSsnKSc7DQogICAgICAgIH1lbHNlew0KICAgICAgICAgICAgdmFyIHggPSBwYXJzZUZsb2F0KGNzWzBdKSsxNjsNCiAgICAgICAgICAgIHZhciB5ID0gcGFyc2VGbG9hdChjc1sxXSkrMTY7DQogICAgICAgICAgICB2YXIgeDEgPSB4ICogbWFBcnJbMF0gKyB5ICogbWFBcnJbMl0gKyBtYUFycls0XTsNCiAgICAgICAgICAgIHZhciB5MSA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgeCA9IHBhcnNlRmxvYXQoY3NbMF0pKzE2Ow0KICAgICAgICAgICAgeSA9IHBhcnNlRmxvYXQoY3NbMV0pOw0KICAgICAgICAgICAgdmFyIHgyID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTIgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHZhciBmeCA9IHgxPHgyP3gxLnRvc3VpdHN2ZygpOiB4Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciBmeSA9IHkxPnkyP3kxLnRvc3VpdHN2ZygpOiB5Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciBvZmZ5ID0gTWF0aC5hYnMoeTEteTIpOw0KICAgICAgICAgICAgdmFyIHRyc3RyID0gICd0cmFuc2xhdGUoJytmeCsnLCcrZnkrJyknOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JyxvZmZ5LnRvU3RyaW5nKCkpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnd2lkdGgnLCcxNicpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgneScsKC1vZmZ5KS50b1N0cmluZygpKTsNCiAgICAgICAgICAgIHBvcHVwUi5zZXRBdHRyaWJ1dGUoJ2ZpbGwnLCd0cmFuc3BhcmVudCcpOw0KICAgICAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQocG9wdXBSKTsNCiAgICAgICAgfQ0KICAgIH0NCiAgICBwb3B1cC5zZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScsdHJzdHIpOw0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnY29tbWVudCcsJycpOw0KICAgIHBvcHVwLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnZWQ6Y29tbWVudGlkJyxjb21tZW50c1tpXS5wYXJlbnROb2RlLmlkKTsNCiAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCcjc3ZnLWNvbnRhaW5lciA+IHN2ZycpLmFwcGVuZENoaWxkKHBvcHVwKTsNCiAgICBjb21tZW50c1tpXS5vbm1vdXNlb3Zlcj1mdW5jdGlvbiAoKSB7DQogICAgICAgIHZhciBjb21tZW50aWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KICAgICAgICB0aGlzLnF1ZXJ5U2VsZWN0b3IoJ3JlY3QnKS5zdHlsZS5kaXNwbGF5PSdibG9jayc7DQogICAgICAgIGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDpjb21tZW50aWQ9JyIrY29tbWVudGlkKyInXVtjb21tZW50XSIpLnN0eWxlLmRpc3BsYXk9J2Jsb2NrJzsNCiAgICB9Ow0KICAgIGNvbW1lbnRzW2ldLm9ubW91c2VvdXQ9ZnVuY3Rpb24gKCkgew0KICAgICAgICB2YXIgY29tbWVudGlkPXRoaXMucGFyZW50Tm9kZS5pZDsNCi8vICAgICAgICB3aW5kb3cuZ2V0U2VsZWN0aW9uKCkucmVtb3ZlQWxsUmFuZ2VzKCk7DQogICAgICAgIHRoaXMucXVlcnlTZWxlY3RvcigncmVjdCcpLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6Y29tbWVudGlkPSciK2NvbW1lbnRpZCsiJ11bY29tbWVudF0iKS5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICB9DQp9DQovLy0tY29tbWVudA0KLy9ub3RlLS0NCmlmKCF1YSl7DQogICAgdmFyIG5vdGVzPWRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoJ2c+Z1tlZFxcOm5vdGVdJyk7DQogICAgZnVuY3Rpb24gZ2V0d2gocyxwKSB7DQogICAgICAgIHZhciBtYWlucD1kb2N1bWVudC5jcmVhdGVFbGVtZW50KCdkaXYnKTsNCiAgICAgICAgbWFpbnAuc3R5bGUuY3NzVGV4dD1zOw0KICAgICAgICBtYWlucC5zdHlsZS5kaXNwbGF5PSdpbmxpbmUtYmxvY2snOw0KCQltYWlucC5zdHlsZS5tYXhXaWR0aCA9ICc0MDBweCc7DQoJCW1haW5wLnN0eWxlLndvcmRCcmVhayA9ICdicmVhay1hbGwnOw0KICAgICAgICBtYWlucC5pbm5lckhUTUw9cDsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5hcHBlbmRDaGlsZChtYWlucCk7DQogICAgICAgIHZhciB3PW1haW5wLmNsaWVudFdpZHRoOw0KICAgICAgICB2YXIgaD1tYWlucC5jbGllbnRIZWlnaHQ7DQogICAgICAgIGRvY3VtZW50LmJvZHkucmVtb3ZlQ2hpbGQobWFpbnApOw0KICAgICAgICByZXR1cm4gW3csaF0NCiAgICB9DQogICAgZm9yKHZhciBpPTA7aTxub3Rlcy5sZW5ndGg7aSsrKXsNCiAgICAgICAgdmFyIGE9bm90ZXNbaV0uZ2V0QXR0cmlidXRlKCdlZDpub3RlJyk7DQoJCXZhciBub3RlTG9jayA9IG5vdGVzW2ldLmdldEF0dHJpYnV0ZSgnZWQ6bm90ZWxvY2snKTsNCiAgICAgICAgaWYobm90ZUxvY2sgPT0gJ3RydWUnKXsNCiAgICAgICAgICAgIGNvbnRpbnVlOw0KICAgICAgICB9DQogICAgICAgIHZhciBtYWlucD1hLm1hdGNoKC88Ym9keVtePl0qPiguKik8XC9ib2R5Pi8pWzFdOw0KICAgICAgICB2YXIgbWFpbnM9YS5tYXRjaCgvc3R5bGU9IiguKj8pIi8pWzFdOw0KICAgICAgICB2YXIgb3V0PWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdnJyk7DQogICAgICAgIHZhciBvbGluZT1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywncmVjdCcpOw0KICAgICAgICB2YXIgcG9wdXA9ZG9jdW1lbnQuY3JlYXRlRWxlbWVudE5TKCdodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZycsJ2ZvcmVpZ25PYmplY3QnKTsNCiAgICAgICAgdmFyIHBvcHVwUj0gZG9jdW1lbnQuY3JlYXRlRWxlbWVudE5TKCdodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZycsJ3JlY3QnKTsNCiAgICAgICAgdmFyIGhvdmVyPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbCcsJyNjZGNkZmYnKTsNCiAgICAgICAgaG92ZXIuc2V0QXR0cmlidXRlKCd4JywnMCcpOw0KICAgICAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3knLCcwJyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JywnMTYnKTsNCiAgICAgICAgaG92ZXIuc2V0QXR0cmlidXRlKCd3aWR0aCcsJzE2Jyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbC1vcGFjaXR5JywnMC42Jyk7DQogICAgICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyxub3Rlc1tpXS5xdWVyeVNlbGVjdG9yKCd1c2UnKS5nZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScpKTsNCiAgICAgICAgaG92ZXIuc3R5bGUuZGlzcGxheT0nbm9uZSc7DQogICAgICAgIG5vdGVzW2ldLmFwcGVuZENoaWxkKGhvdmVyKTsNCiAgICAgICAgcG9wdXAuc3R5bGUuY3NzVGV4dD1tYWluczsNCiAgICAgICAgcG9wdXAuaW5uZXJIVE1MPW1haW5wOw0KICAgICAgICB2YXIgd2g9Z2V0d2gobWFpbnMsbWFpbnApOw0KICAgICAgICBwb3B1cC5zZXRBdHRyaWJ1dGUoJ3dpZHRoJyx3aFswXSk7DQogICAgICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnaGVpZ2h0Jyx3aFsxXSk7DQogICAgICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJywndHJhbnNsYXRlKDgsNCknKTsNCgkJcG9wdXAuc3R5bGUud29yZEJyZWFrID0gJ2JyZWFrLWFsbCc7DQogICAgICAgIHBvcHVwLnN0eWxlLnRleHRBbGlnbj0nbGVmdCc7DQogICAgICAgIG9saW5lLnNldEF0dHJpYnV0ZSgneCcsJzAnKTsNCiAgICAgICAgb2xpbmUuc2V0QXR0cmlidXRlKCd5JywnMCcpOw0KICAgICAgICBvbGluZS5zZXRBdHRyaWJ1dGUoJ3dpZHRoJyx3aFswXSsxNik7DQogICAgICAgIG9saW5lLnNldEF0dHJpYnV0ZSgnaGVpZ2h0Jyx3aFsxXSs4KTsNCiAgICAgICAgb2xpbmUuc2V0QXR0cmlidXRlKCdzdHJva2UnLCcjYTI3YTAwJyk7DQogICAgICAgIG9saW5lLnNldEF0dHJpYnV0ZSgnZmlsbCcsJyNmZmU3OWQnKTsNCiAgICAgICAgb3V0LmFwcGVuZENoaWxkKG9saW5lKTsNCiAgICAgICAgb3V0LmFwcGVuZENoaWxkKHBvcHVwKTsNCiAgICAgICAgb3V0LnNldEF0dHJpYnV0ZSgnbm90ZScsJycpOw0KICAgICAgICBvdXQuc3R5bGUuZGlzcGxheT0nbm9uZSc7DQogICAgICAgIG91dC5zZXRBdHRyaWJ1dGUoJ2VkOm5vdGVpZCcsbm90ZXNbaV0ucGFyZW50Tm9kZS5pZCk7DQogICAgICAgIG91dC5vbm1vdXNlb3Zlcj1mdW5jdGlvbiAoKSB7DQogICAgICAgICAgICB0aGlzLnN0eWxlLmRpc3BsYXk9J2Jsb2NrJzsNCiAgICAgICAgfTsNCiAgICAgICAgb3V0Lm9ubW91c2VvdXQ9ZnVuY3Rpb24gKCkgew0KLy8gICAgICAgIHdpbmRvdy5nZXRTZWxlY3Rpb24gPyB3aW5kb3cuZ2V0U2VsZWN0aW9uKCkucmVtb3ZlUmFuZ2Uod2luZG93LmdldFNlbGVjdGlvbigpLnJlKTpkb2N1bWVudC5zZWxlY3Rpb24uZW1wdHkoKTsNCg0KICAgICAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICAgICAgfTsNCiAgICAgICAgdmFyIGNzPW5vdGVzW2ldLnF1ZXJ5U2VsZWN0b3IoJ3VzZScpLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJykubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICAgICAgdmFyIHBzPW5vdGVzW2ldLnBhcmVudE5vZGUuZ2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nKTsNCiAgICAgICAgaWYocHMuc3Vic3RyKDAsMikgPT0gJ3RyJyl7DQogICAgICAgICAgICB2YXIgcHBzID0gcHMubWF0Y2goL1woKFxTKnxcUypcc1xTKilcKS8pWzFdLnNwbGl0KC8gfCwvKTsNCiAgICAgICAgICAgIHZhciB4PXBhcnNlRmxvYXQoY3NbMF0pK3BhcnNlRmxvYXQocHBzWzBdKTsNCiAgICAgICAgICAgIHZhciB5PXBhcnNlRmxvYXQocHBzWzFdKTsNCiAgICAgICAgICAgIHg9eC50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHk9eS50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciB0cnN0ciA9ICd0cmFuc2xhdGUoJyt4KycsJyt5KycpJzsNCiAgICAgICAgfWVsc2UgaWYocHMuc3Vic3RyKDAsMikgPT0gJ21hJyl7DQogICAgICAgICAgICB2YXIgcHBzID0gcHMubWF0Y2goLyhcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylbXCwgXShcLT9cZCsoXC5cZCspPylcKSQvKTsNCiAgICAgICAgICAgIHZhciBtYUFyciA9IFtwYXJzZUZsb2F0KHBwc1sxXSkscGFyc2VGbG9hdChwcHNbM10pLHBhcnNlRmxvYXQocHBzWzVdKSxwYXJzZUZsb2F0KHBwc1s3XSkscGFyc2VGbG9hdChwcHNbOV0pLHBhcnNlRmxvYXQocHBzWzExXSldOw0KICAgICAgICAgICAgaWYobWFBcnJbMV0gPT0gMCl7DQogICAgICAgICAgICAgICAgdmFyIHggPSBwYXJzZUZsb2F0KGNzWzBdKTsNCiAgICAgICAgICAgICAgICB2YXIgeSA9IHBhcnNlRmxvYXQoY3NbMV0pKzE2Ow0KICAgICAgICAgICAgICAgIHZhciB4MSA9IHggKiBtYUFyclswXSArIHkgKiBtYUFyclsyXSArIG1hQXJyWzRdOw0KICAgICAgICAgICAgICAgIHZhciB5MSA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgICAgIHgxPXgxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgICAgIHkxPXkxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgICAgIHZhciB0cnN0ciA9ICAndHJhbnNsYXRlKCcreDErJywnK3kxKycpJzsNCiAgICAgICAgICAgIH1lbHNlew0KICAgICAgICAgICAgICAgIHZhciB4ID0gcGFyc2VGbG9hdChjc1swXSkrMTY7DQogICAgICAgICAgICAgICAgdmFyIHkgPSBwYXJzZUZsb2F0KGNzWzFdKSsxNjsNCiAgICAgICAgICAgICAgICB2YXIgeDEgPSB4ICogbWFBcnJbMF0gKyB5ICogbWFBcnJbMl0gKyBtYUFycls0XTsNCiAgICAgICAgICAgICAgICB2YXIgeTEgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgICAgICB4ID0gcGFyc2VGbG9hdChjc1swXSkrMTY7DQogICAgICAgICAgICAgICAgeSA9IHBhcnNlRmxvYXQoY3NbMV0pOw0KICAgICAgICAgICAgICAgIHZhciB4MiA9IHggKiBtYUFyclswXSArIHkgKiBtYUFyclsyXSArIG1hQXJyWzRdOw0KICAgICAgICAgICAgICAgIHZhciB5MiA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgICAgIHZhciBmeCA9IHgxPHgyP3gxLnRvc3VpdHN2ZygpOiB4Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgICAgICB2YXIgZnkgPSB5MT55Mj95MS50b3N1aXRzdmcoKTogeTIudG9zdWl0c3ZnKCk7DQoJCQkJdmFyIG9mZnkgPSBNYXRoLmFicyh5MS15Mik7CQkJCQkJCQkJCSAgDQogICAgICAgICAgICAgICAgdmFyIHRyc3RyID0gICd0cmFuc2xhdGUoJytmeCsnLCcrZnkrJyknOw0KICAgICAgICAgICAgICAgIHBvcHVwUi5zZXRBdHRyaWJ1dGUoJ2hlaWdodCcsb2ZmeS50b1N0cmluZygpKTsNCiAgICAgICAgICAgICAgICBwb3B1cFIuc2V0QXR0cmlidXRlKCd3aWR0aCcsJzE2Jyk7DQogICAgICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgneScsKC1vZmZ5KS50b1N0cmluZygpKTsNCiAgICAgICAgICAgICAgICBwb3B1cFIuc2V0QXR0cmlidXRlKCdmaWxsJywndHJhbnNwYXJlbnQnKTsNCiAgICAgICAgICAgICAgICBwb3B1cC5hcHBlbmRDaGlsZChwb3B1cFIpOw0KICAgICAgICAgICAgfQ0KICAgICAgICB9DQogICAgICAgIG91dC5zZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScsdHJzdHIpOw0KICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCcjc3ZnLWNvbnRhaW5lciA+IHN2ZycpLmFwcGVuZENoaWxkKG91dCk7DQogICAgICAgIG5vdGVzW2ldLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgICAgIHZhciBub3RlaWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KICAgICAgICAgICAgdGhpcy5xdWVyeVNlbGVjdG9yKCdyZWN0Jykuc3R5bGUuZGlzcGxheT0nYmxvY2snOw0KICAgICAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOm5vdGVpZD0nIitub3RlaWQrIiddW25vdGVdIikuc3R5bGUuZGlzcGxheT0nYmxvY2snOw0KICAgICAgICB9Ow0KICAgICAgICBub3Rlc1tpXS5vbm1vdXNlb3V0PWZ1bmN0aW9uICgpIHsNCiAgICAgICAgICAgIHZhciBub3RlaWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KLy8gICAgICAgIHdpbmRvdy5nZXRTZWxlY3Rpb24oKS5yZW1vdmVBbGxSYW5nZXMoKTsNCiAgICAgICAgICAgIHRoaXMucXVlcnlTZWxlY3RvcigncmVjdCcpLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgICAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOm5vdGVpZD0nIitub3RlaWQrIiddW25vdGVdIikuc3R5bGUuZGlzcGxheT0nbm9uZSc7DQogICAgICAgIH0NCiAgICB9DQp9ZWxzZXsNCiAgICBjb25zb2xlLmxvZygn5oqx5q2J77yMSUXmtY/op4jlmajkuI3mlK/mjIFub3Rl6Kej5p6Q77yM6K+35L2/55So5YW25LuW5YaF5qC45rWP6KeI5Zmo44CC6LCi6LCi77yBJykNCn0NCi8vLS1ub3RlDQovL2h5cGVybGluay0tDQp2YXIgbGlua3M9ZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgnZz5nW2VkXFw6aHlwZXJsaW5rXScpOw0KZnVuY3Rpb24gZ2V0bWF4bGVuKGFycixicnIpIHsNCiAgICB2YXIgbD0wOw0KICAgIHZhciBsbD0wOw0KICAgIGZvcih2YXIgaj0wO2o8YXJyLmxlbmd0aDtqKyspew0KICAgICAgICB2YXIgZT1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywndGV4dCcpOw0KICAgICAgICBpZighaXNOYU4obGlua2FycltqXSkpew0KICAgICAgICAgICAgZS50ZXh0Q29udGVudD0nUGFnZS0nK2FycltqXTsNCiAgICAgICAgfWVsc2V7DQogICAgICAgICAgICBlLnRleHRDb250ZW50PWFycltqXTsNCiAgICAgICAgfQ0KICAgICAgICBlLnN0eWxlLmZvbnRTaXplPScxMnB4JzsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0uYXBwZW5kQ2hpbGQoZSk7DQogICAgICAgIHZhciBldz1lLmdldEJCb3goKS53aWR0aDsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0ucmVtb3ZlQ2hpbGQoZSk7DQogICAgICAgIHZhciBoPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIGgudGV4dENvbnRlbnQ9YnJyW2pdOw0KICAgICAgICBoLnN0eWxlLmZvbnRTaXplPScxMnB4JzsNCiAgICAgICAgaC5zdHlsZS5mb250V2VpZ2h0PSdib2xkJzsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0uYXBwZW5kQ2hpbGQoaCk7DQogICAgICAgIHZhciBodz1oLmdldEJCb3goKS53aWR0aDsNCiAgICAgICAgZG9jdW1lbnQuYm9keS5nZXRFbGVtZW50c0J5VGFnTmFtZSgnc3ZnJylbMF0ucmVtb3ZlQ2hpbGQoaCk7DQogICAgICAgIGw9ZXc+aHc/ZXc6aHc7DQogICAgICAgIGxsPWw+bGw/bDpsbDsNCiAgICB9DQogICAgcmV0dXJuIGxsOw0KfQ0KZm9yKHZhciBpPTA7aTxsaW5rcy5sZW5ndGg7aSsrKXsNCiAgICB2YXIgcG9wdXA9ZG9jdW1lbnQuY3JlYXRlRWxlbWVudE5TKCdodHRwOi8vd3d3LnczLm9yZy8yMDAwL3N2ZycsJ2cnKTsNCiAgICB2YXIgcG9wdXBSPSBkb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywncmVjdCcpOw0KICAgIHZhciBob3Zlcj1kb2N1bWVudC5jcmVhdGVFbGVtZW50TlMoJ2h0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnJywncmVjdCcpOw0KICAgIHZhciBkZXNjYXJyPVtdOw0KICAgIHZhciBsaW5rYXJyPVtdOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnZmlsbCcsJyNjZGNkZmYnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3gnLCcwJyk7DQogICAgaG92ZXIuc2V0QXR0cmlidXRlKCd5JywnMCcpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JywnMTYnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ3dpZHRoJywnMTYnKTsNCiAgICBob3Zlci5zZXRBdHRyaWJ1dGUoJ2ZpbGwtb3BhY2l0eScsJzAuNicpOw0KICAgIGhvdmVyLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyxsaW5rc1tpXS5xdWVyeVNlbGVjdG9yKCd1c2UnKS5nZXRBdHRyaWJ1dGUoJ3RyYW5zZm9ybScpKTsNCiAgICBob3Zlci5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICBsaW5rc1tpXS5hcHBlbmRDaGlsZChob3Zlcik7DQogICAgLy8gY29uc29sZS5sb2cobGlua3NbaV0uZ2V0QXR0cmlidXRlKCdlZDpoeXBlcmxpbmsnKSk7DQogICAgdmFyIGE9SlNPTi5wYXJzZShsaW5rc1tpXS5nZXRBdHRyaWJ1dGUoJ2VkOmh5cGVybGluaycpKTsNCiAgICB2YXIgY3M9bGlua3NbaV0ucXVlcnlTZWxlY3RvcigndXNlJykuZ2V0QXR0cmlidXRlKCd0cmFuc2Zvcm0nKS5tYXRjaCgvXCgoXFMqfFxTKlxzXFMqKVwpLylbMV0uc3BsaXQoLyB8LC8pOw0KICAgIHZhciBwcz1saW5rc1tpXS5wYXJlbnROb2RlLmdldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyk7DQogICAgaWYocHMuc3Vic3RyKDAsMikgPT0gJ3RyJyl7DQogICAgICAgIHZhciBwcHMgPSBwcy5tYXRjaCgvXCgoXFMqfFxTKlxzXFMqKVwpLylbMV0uc3BsaXQoLyB8LC8pOw0KICAgICAgICB2YXIgeD1wYXJzZUZsb2F0KGNzWzBdKStwYXJzZUZsb2F0KHBwc1swXSk7DQogICAgICAgIHZhciB5PXBhcnNlRmxvYXQocHBzWzFdKTsNCiAgICAgICAgeD14LnRvc3VpdHN2ZygpOw0KICAgICAgICB5PXkudG9zdWl0c3ZnKCk7DQogICAgICAgIHZhciB0cnN0ciA9ICd0cmFuc2xhdGUoJyt4KycsJyt5KycpJzsNCiAgICB9ZWxzZSBpZihwcy5zdWJzdHIoMCwyKSA9PSAnbWEnKXsNCiAgICAgICAgdmFyIHBwcyA9IHBzLm1hdGNoKC8oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pW1wsIF0oXC0/XGQrKFwuXGQrKT8pXCkkLyk7DQogICAgICAgIHZhciBtYUFyciA9IFtwYXJzZUZsb2F0KHBwc1sxXSkscGFyc2VGbG9hdChwcHNbM10pLHBhcnNlRmxvYXQocHBzWzVdKSxwYXJzZUZsb2F0KHBwc1s3XSkscGFyc2VGbG9hdChwcHNbOV0pLHBhcnNlRmxvYXQocHBzWzExXSldOw0KICAgICAgICBpZihtYUFyclsxXSA9PSAwKXsNCiAgICAgICAgICAgIHZhciB4ID0gcGFyc2VGbG9hdChjc1swXSk7DQogICAgICAgICAgICB2YXIgeSA9IHBhcnNlRmxvYXQoY3NbMV0pKzE2Ow0KICAgICAgICAgICAgdmFyIHgxID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTEgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHgxPXgxLnRvc3VpdHN2ZygpOw0KICAgICAgICAgICAgeTE9eTEudG9zdWl0c3ZnKCk7DQogICAgICAgICAgICB2YXIgdHJzdHIgPSAgJ3RyYW5zbGF0ZSgnK3gxKycsJyt5MSsnKSc7DQogICAgICAgIH1lbHNlew0KICAgICAgICAgICAgdmFyIHggPSBwYXJzZUZsb2F0KGNzWzBdKSsxNjsNCiAgICAgICAgICAgIHZhciB5ID0gcGFyc2VGbG9hdChjc1sxXSkrMTY7DQogICAgICAgICAgICB2YXIgeDEgPSB4ICogbWFBcnJbMF0gKyB5ICogbWFBcnJbMl0gKyBtYUFycls0XTsNCiAgICAgICAgICAgIHZhciB5MSA9IHggKiBtYUFyclsxXSArIHkgKiBtYUFyclszXSArIG1hQXJyWzVdOw0KICAgICAgICAgICAgeCA9IHBhcnNlRmxvYXQoY3NbMF0pKzE2Ow0KICAgICAgICAgICAgeSA9IHBhcnNlRmxvYXQoY3NbMV0pOw0KICAgICAgICAgICAgdmFyIHgyID0geCAqIG1hQXJyWzBdICsgeSAqIG1hQXJyWzJdICsgbWFBcnJbNF07DQogICAgICAgICAgICB2YXIgeTIgPSB4ICogbWFBcnJbMV0gKyB5ICogbWFBcnJbM10gKyBtYUFycls1XTsNCiAgICAgICAgICAgIHZhciBmeCA9IHgxPHgyP3gxLnRvc3VpdHN2ZygpOiB4Mi50b3N1aXRzdmcoKTsNCiAgICAgICAgICAgIHZhciBmeSA9IHkxPnkyP3kxLnRvc3VpdHN2ZygpOiB5Mi50b3N1aXRzdmcoKTsNCgkJCXZhciBvZmZ5ID0gTWF0aC5hYnMoeTEteTIpOwkJCQkJCQkJCQkgIA0KICAgICAgICAgICAgdmFyIHRyc3RyID0gICd0cmFuc2xhdGUoJytmeCsnLCcrZnkrJyknOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnaGVpZ2h0JyxvZmZ5LnRvU3RyaW5nKCkpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgnd2lkdGgnLCcxNicpOw0KICAgICAgICAgICAgcG9wdXBSLnNldEF0dHJpYnV0ZSgneScsKC1vZmZ5KS50b1N0cmluZygpKTsNCiAgICAgICAgICAgIHBvcHVwUi5zZXRBdHRyaWJ1dGUoJ2ZpbGwnLCd0cmFuc3BhcmVudCcpOw0KICAgICAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQocG9wdXBSKTsNCiAgICAgICAgfQ0KICAgIH0NCiAgICB2YXIgYWw9YS5sZW5ndGg7DQogICAgZm9yKHZhciBqPTA7ajxhbDtqKyspew0KICAgICAgICBsaW5rYXJyLnB1c2goYVtqXS5saW5rKTsNCiAgICAgICAgZGVzY2Fyci5wdXNoKGFbal0uZGVzYyk7DQogICAgfQ0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgndHJhbnNmb3JtJyx0cnN0cik7DQogICAgdmFyIG1heD1nZXRtYXhsZW4obGlua2FycixkZXNjYXJyKTsNCiAgICBmb3IodmFyIGs9MDtrPGFsO2srKyl7DQogICAgICAgIHZhciBjPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdhJyk7DQogICAgICAgIHZhciBkPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCdyZWN0Jyk7DQogICAgICAgIHZhciBlPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIHZhciBmPWRvY3VtZW50LmNyZWF0ZUVsZW1lbnROUygnaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmcnLCd0ZXh0Jyk7DQogICAgICAgIGlmKGlzTmFOKGxpbmthcnJba10pKXsNCiAgICAgICAgICAgIGMuc2V0QXR0cmlidXRlTlMoImh0dHA6Ly93d3cudzMub3JnLzIwMDAvc3ZnIiwgInhsaW5rIiwgImh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiKTsNCiAgICAgICAgICAgIGMuc2V0QXR0cmlidXRlTlMoImh0dHA6Ly93d3cudzMub3JnLzE5OTkveGxpbmsiLCAiaHJlZiIsIGxpbmthcnJba10pOw0KICAgICAgICAgICAgYy5zZXRBdHRyaWJ1dGUoJ3RhcmdldCcsJ19ibGFuaycpOw0KICAgICAgICAgICAgZS50ZXh0Q29udGVudD1saW5rYXJyW2tdOw0KICAgICAgICB9ZWxzZXsNCiAgICAgICAgICAgIGUudGV4dENvbnRlbnQ9J1BhZ2UtJytsaW5rYXJyW2tdOw0KICAgICAgICAgICAgYy5zZXRBdHRyaWJ1dGVOUygiaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciLCAieGxpbmsiLCAiaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIpOw0KICAgICAgICAgICAgYy5zZXRBdHRyaWJ1dGVOUygiaHR0cDovL3d3dy53My5vcmcvMTk5OS94bGluayIsICJocmVmIiwgIiMiK2xpbmthcnJba10pOw0KICAgICAgICB9DQogICAgICAgIGQuc2V0QXR0cmlidXRlKCd3aWR0aCcsbWF4KzEwKTsNCiAgICAgICAgZC5zZXRBdHRyaWJ1dGUoJ2hlaWdodCcsJzMzJyk7DQogICAgICAgIGQuc2V0QXR0cmlidXRlKCdzdHJva2UnLCcjOTk5OTk5Jyk7DQogICAgICAgIGQuc2V0QXR0cmlidXRlKCdmaWxsJywnd2hpdGUnKTsNCiAgICAgICAgZC5zZXRBdHRyaWJ1dGUoJ3knLDMzKmspOw0KICAgICAgICBmLnRleHRDb250ZW50PWRlc2NhcnJba107DQogICAgICAgIGYuc3R5bGUuZm9udFNpemU9JzEycHgnOw0KICAgICAgICBmLnN0eWxlLmZvbnRXZWlnaHQ9J2JvbGQnOw0KICAgICAgICBmLnNldEF0dHJpYnV0ZSgneCcsNSk7DQogICAgICAgIGYuc2V0QXR0cmlidXRlKCd5JywzMyprKzEyKTsNCiAgICAgICAgZS5zdHlsZS5mb250U2l6ZT0nMTJweCc7DQogICAgICAgIGUuc2V0QXR0cmlidXRlKCd5JywzMyprKzI4KTsNCiAgICAgICAgZS5zZXRBdHRyaWJ1dGUoJ3gnLDUpOw0KICAgICAgICBjLmFwcGVuZENoaWxkKGQpOw0KICAgICAgICBjLmFwcGVuZENoaWxkKGYpOw0KICAgICAgICBjLmFwcGVuZENoaWxkKGUpOw0KICAgICAgICBjLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgICAgIHRoaXMucXVlcnlTZWxlY3RvcigncmVjdCcpLnN0eWxlLmZpbGw9JyNlMWUxZmYnDQogICAgICAgIH07DQogICAgICAgIGMub25tb3VzZW91dD1mdW5jdGlvbiAoKSB7DQogICAgICAgICAgICB0aGlzLnF1ZXJ5U2VsZWN0b3IoJ3JlY3QnKS5zdHlsZS5maWxsPSd3aGl0ZScNCiAgICAgICAgfTsNCiAgICAgICAgcG9wdXAuYXBwZW5kQ2hpbGQoYyk7DQogICAgfQ0KICAgIHBvcHVwLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIHBvcHVwLnNldEF0dHJpYnV0ZSgnaHlwZXJsaW5rJywnJyk7DQogICAgcG9wdXAuc2V0QXR0cmlidXRlKCdlZDpsaW5raWQnLGxpbmtzW2ldLnBhcmVudE5vZGUuaWQpOw0KICAgIHBvcHVwLm9ubW91c2VvdmVyPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdibG9jayc7DQogICAgfTsNCiAgICBwb3B1cC5vbmNsaWNrPWZ1bmN0aW9uICgpIHsNCiAgICAgICAgdGhpcy5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICB9Ow0KICAgIHBvcHVwLm9ubW91c2VvdXQ9ZnVuY3Rpb24gKCkgew0KICAgICAgICB0aGlzLnN0eWxlLmRpc3BsYXk9J25vbmUnOw0KICAgIH07DQogICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcignI3N2Zy1jb250YWluZXIgPiBzdmcnKS5hcHBlbmRDaGlsZChwb3B1cCk7DQogICAgbGlua3NbaV0ub25tb3VzZW92ZXI9ZnVuY3Rpb24gKCkgew0KICAgICAgICB2YXIgbGlua2lkPXRoaXMucGFyZW50Tm9kZS5pZDsNCiAgICAgICAgdGhpcy5xdWVyeVNlbGVjdG9yKCdyZWN0Jykuc3R5bGUuZGlzcGxheT0nYmxvY2snOw0KICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6bGlua2lkPSciK2xpbmtpZCsiJ11baHlwZXJsaW5rXSIpLnN0eWxlLmRpc3BsYXk9J2Jsb2NrJzsNCiAgICB9DQogICAgbGlua3NbaV0ub25tb3VzZW91dD1mdW5jdGlvbiAoKSB7DQogICAgICAgIHZhciBsaW5raWQ9dGhpcy5wYXJlbnROb2RlLmlkOw0KICAgICAgICB0aGlzLnF1ZXJ5U2VsZWN0b3IoJ3JlY3QnKS5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOmxpbmtpZD0nIitsaW5raWQrIiddW2h5cGVybGlua10iKS5zdHlsZS5kaXNwbGF5PSdub25lJzsNCiAgICB9DQp9DQovLy0taHlwZXJsaW5rDQovL2luaXRpYWxpemUtLQ0KdmFyIHNoYXBlPWRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoJ2dbZWRcXDp0b2d0b3BpY2lkXScpOw0KdmFyIG1JZD1kb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCdnW2VkXFw6dG9waWN0eXBlXScpOw0KdmFyIGRhdGFUcmVlPXt9Ow0KdmFyIGV4dHJhUmVsYT17fTsNCnZhciBjaGVja0lEPScnOw0KZm9yKHZhciBpPTA7aTxtSWQubGVuZ3RoO2krKyl7DQogICAgdmFyIHR5cGU9bUlkW2ldLmdldEF0dHJpYnV0ZSgnZWQ6dG9waWN0eXBlJyk7DQogICAgdmFyIHNpZD1tSWRbaV0uaWQ7DQogICAgaWYodHlwZSE9PSdjYWxsb3V0Jyl7DQogICAgICAgIGluaXQoc2lkLGRhdGFUcmVlKQ0KICAgIH0NCn0NCmZ1bmN0aW9uIGluaXQoaWQsIG9iaikgew0KICAgIHZhciBjaGlsZHMgPSBkb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCJnW2VkXFw6cGFyZW50aWQ9JyIgKyBpZCArICInXTpub3QoW2VkXFw6dG9waWN0eXBlXSkiKTsNCiAgICB2YXIgY2FsbHMgPSBkb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCJnW2VkXFw6cGFyZW50aWQ9JyIgKyBpZCArICInXVtlZFxcOnRvcGljdHlwZV0iKTsNCiAgICB2YXIgc3VtbWFyeSA9IGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3JBbGwoInBhdGhbZWRcXDpwYXJlbnRpZCo9JyIgKyBpZCArICInXVtlZFxcOnR5cGU9J3N1bW1hcnknXSIpOw0KICAgIHZhciBib3VuZGFyeT0gZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgicGF0aFtlZFxcOnBhcmVudGlkKj0nIiArIGlkICsgIiddW2VkXFw6dHlwZT0nYm91bmRhcnknXSIpOw0KICAgIHZhciByZWxhZnJvbT1kb2N1bWVudC5xdWVyeVNlbGVjdG9yQWxsKCJnW2VkXFw6ZnJvbWlkKj0nIiArIGlkICsgIiddW2VkXFw6dHlwZT0ncmVsYXRpb24nXSIpOw0KICAgIHZhciByZWxhdG89ZG9jdW1lbnQucXVlcnlTZWxlY3RvckFsbCgiZ1tlZFxcOnRvaWQqPSciICsgaWQgKyAiJ11bZWRcXDp0eXBlPSdyZWxhdGlvbiddIik7DQogICAgb2JqWyJtIiArIGlkXSA9IHt9Ow0KICAgIHZhciB0eXBlID0gZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoaWQpLmdldEF0dHJpYnV0ZSgnZWQ6dG9waWN0eXBlJyk7DQogICAgdmFyIGl3PWRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGlkKS5nZXRBdHRyaWJ1dGUoJ2VkOndpZHRoJyk7DQogICAgdmFyIGloPWRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGlkKS5nZXRBdHRyaWJ1dGUoJ2VkOmhlaWdodCcpOw0KICAgIGlmICh0eXBlKSB7DQogICAgICAgIG9ialsibSIgKyBpZF0udHlwZSA9IHR5cGU7DQogICAgfQ0KICAgIGlmKGl3JiZpaCl7DQogICAgICAgIG9ialsibSIgKyBpZF0ud2lkdGggPWl3Ow0KICAgICAgICBvYmpbIm0iICsgaWRdLmhlaWdodCA9aWg7DQogICAgfQ0KICAgIGlmIChyZWxhZnJvbS5sZW5ndGggIT09IDApIHsNCiAgICAgICAgb2JqWyJtIiArIGlkXS5yZWxhZnJvbSA9IHt9Ow0KICAgICAgICBmb3IgKHZhciBpID0gMDsgaSA8IHJlbGFmcm9tLmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgaW5kZXhpZCA9IHJlbGFmcm9tW2ldLmlkOw0KICAgICAgICAgICAgdmFyIHRvaWQgPSBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChpbmRleGlkKS5nZXRBdHRyaWJ1dGUoJ2VkOnRvaWQnKTsNCiAgICAgICAgICAgIGlmIChleHRyYVJlbGFbaW5kZXhpZF0gPT09IHVuZGVmaW5lZCkgew0KICAgICAgICAgICAgICAgIGV4dHJhUmVsYVtpbmRleGlkXSA9IHsNCiAgICAgICAgICAgICAgICAgICAgaWQ6IGluZGV4aWQsDQogICAgICAgICAgICAgICAgICAgIGZyb21pZDogaWQsDQogICAgICAgICAgICAgICAgICAgIHRvaWQ6IHRvaWQsDQogICAgICAgICAgICAgICAgICAgIGlzQzogZmFsc2UNCiAgICAgICAgICAgICAgICB9Ow0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgb2JqWyJtIiArIGlkXS5yZWxhZnJvbVtpbmRleGlkXT17fTsNCiAgICAgICAgICAgIG9ialsibSIgKyBpZF0ucmVsYWZyb20uZGlzcGxheT1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChpZCkuc3R5bGUuZGlzcGxheSAhPT0gJ25vbmUnPydibG9jayc6J25vbmUnOw0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChyZWxhdG8ubGVuZ3RoICE9PSAwKSB7DQogICAgICAgIG9ialsibSIgKyBpZF0ucmVsYXRvID0ge307DQogICAgICAgIGZvciAodmFyIGkgPSAwOyBpIDwgcmVsYXRvLmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgaW5kZXhpZD1yZWxhdG9baV0uaWQ7DQogICAgICAgICAgICB2YXIgZnJvbWlkPWRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGluZGV4aWQpLmdldEF0dHJpYnV0ZSgnZWQ6ZnJvbWlkJyk7DQogICAgICAgICAgICBpZihleHRyYVJlbGFbaW5kZXhpZF0gPT09IHVuZGVmaW5lZCl7DQogICAgICAgICAgICAgICAgZXh0cmFSZWxhW2luZGV4aWRdPXsNCiAgICAgICAgICAgICAgICAgICAgaWQ6aW5kZXhpZCwNCiAgICAgICAgICAgICAgICAgICAgZnJvbWlkOmZyb21pZCwNCiAgICAgICAgICAgICAgICAgICAgdG9pZDppZCwNCiAgICAgICAgICAgICAgICAgICAgaXNDOmZhbHNlDQogICAgICAgICAgICAgICAgfTsNCiAgICAgICAgICAgIH0NCiAgICAgICAgICAgIG9ialsibSIgKyBpZF0ucmVsYXRvW2luZGV4aWRdPXt9Ow0KICAgICAgICAgICAgb2JqWyJtIiArIGlkXS5yZWxhdG8uZGlzcGxheT1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChpZCkuc3R5bGUuZGlzcGxheSAhPT0gJ25vbmUnPydibG9jayc6J25vbmUnOw0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChjaGlsZHMubGVuZ3RoICE9PSAwKSB7DQogICAgICAgIG9ialsibSIgKyBpZF0uY2hpbGQgPSB7fTsNCiAgICAgICAgaWYgKGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDp0b2d0b3BpY2lkPSciICsgaWQgKyAiJ10iKSkgew0KICAgICAgICAgICAgLy8gY29uc29sZS5sb2coZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOnRvZ3RvcGljaWQ9JyIgKyBpZCArICInXSIpLmNoaWxkTm9kZXNbMF0uZ2V0QXR0cmlidXRlKCd4bGluazpocmVmJykpOw0KICAgICAgICAgICAgdmFyIHRvZyA9IGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDp0b2d0b3BpY2lkPSciICsgaWQgKyAiJ10iKS5nZXRFbGVtZW50c0J5VGFnTmFtZSgndXNlJylbMF0uZ2V0QXR0cmlidXRlKCd4bGluazpocmVmJykuc2xpY2UoMSk7DQogICAgICAgICAgICBvYmpbIm0iICsgaWRdLnRvZ3R5cGUgPSB0b2c7DQogICAgICAgIH0NCiAgICAgICAgZm9yICh2YXIgaSA9IDA7IGkgPCBjaGlsZHMubGVuZ3RoOyBpKyspIHsNCiAgICAgICAgICAgIHZhciBjaWQgPSBjaGlsZHNbaV0uaWQ7DQogICAgICAgICAgICBpbml0KGNpZCwgb2JqWyJtIiArIGlkXS5jaGlsZCk7DQogICAgICAgIH0NCiAgICB9DQogICAgaWYgKGNhbGxzLmxlbmd0aCAhPT0gMCkgew0KICAgICAgICBvYmpbIm0iICsgaWRdLmNhbGwgPSB7fTsNCiAgICAgICAgZm9yICh2YXIgaSA9IDA7IGkgPCBjYWxscy5sZW5ndGg7IGkrKykgew0KICAgICAgICAgICAgdmFyIGNpZCA9IGNhbGxzW2ldLmlkOw0KICAgICAgICAgICAgaW5pdChjaWQsIG9ialsibSIgKyBpZF0uY2FsbCk7DQogICAgICAgIH0NCiAgICB9DQogICAgaWYgKGJvdW5kYXJ5Lmxlbmd0aCAhPT0gMCkgew0KICAgICAgICBvYmpbIm0iICsgaWRdLmJvdW5kYXJ5ID0ge307DQogICAgICAgIGZvciAodmFyIGkgPSAwOyBpIDwgYm91bmRhcnkubGVuZ3RoOyBpKyspIHsNCiAgICAgICAgICAgIHZhciBjaWQgPWJvdW5kYXJ5W2ldLmlkOw0KICAgICAgICAgICAgaW5pdChjaWQsIG9ialsibSIgKyBpZF0uYm91bmRhcnkpOw0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChzdW1tYXJ5Lmxlbmd0aCAhPT0gMCkgew0KICAgICAgICBvYmpbIm0iICsgaWRdLnN1bW1hcnkgPSB7fTsNCiAgICAgICAgZm9yICh2YXIgaSA9IDA7IGkgPCBzdW1tYXJ5Lmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgY2lkID0gc3VtbWFyeVtpXS5pZDsNCiAgICAgICAgICAgIGluaXQoY2lkLCBvYmpbIm0iICsgaWRdLnN1bW1hcnkpOw0KICAgICAgICB9DQogICAgfQ0KfQ0KLy8tLWluaXRpYWxpemUNCi8vdG9nZ2xlZGlzcGxheS0tDQp2YXIgY2hhaW5BcnI9W107DQpmdW5jdGlvbiBnZXRjaGFpbihpZCl7DQogICAgY2hhaW5BcnIudW5zaGlmdCgnbScraWQpOw0KICAgIHZhciBwYXJlbnQ9ZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoaWQpLmdldEF0dHJpYnV0ZSgnZWQ6cGFyZW50aWQnKTsNCiAgICBpZighcGFyZW50KXsNCiAgICAgICAgcmV0dXJuOw0KICAgIH0NCglpZihwYXJlbnQubWF0Y2goL1wsLykpew0KICAgICAgICBwYXJlbnQgPSBwYXJlbnQubWF0Y2goL1xkKyg/PVwsKS8pWzBdDQogICAgfQ0KICAgIGdldGNoYWluKHBhcmVudCk7DQp9DQpmdW5jdGlvbiBnZXRvYmooaWQpIHsNCiAgICBjaGFpbkFycj1bXTsNCiAgICBnZXRjaGFpbihpZCk7DQogICAgdmFyIG1haW49Y2hhaW5BcnJbMF07DQogICAgaWYoY2hhaW5BcnIubGVuZ3RoPjEpew0KICAgICAgICB2YXIgb2JqPWRhdGFUcmVlW21haW5dOw0KICAgICAgICAvLyBjb25zb2xlLmxvZyhjaGFpbkFycik7DQogICAgICAgIGZvcih2YXIgaT0xO2k8Y2hhaW5BcnIubGVuZ3RoO2krKykgew0KICAgICAgICAgICAgdmFyIGEgPSBjaGFpbkFycltpXTsNCiAgICAgICAgICAgIGZvcih2YXIgaj0wO2o8T2JqZWN0LmtleXMob2JqKS5sZW5ndGg7aisrKXsNCiAgICAgICAgICAgICAgICB2YXIgY29iaj0gb2JqW09iamVjdC5rZXlzKG9iailbal1dW2FdOw0KICAgICAgICAgICAgICAgIGlmKGNvYmopew0KICAgICAgICAgICAgICAgICAgICBvYmo9Y29iajsNCiAgICAgICAgICAgICAgICAgICAgY29udGludWUNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICAgICAgcmV0dXJuIG9iag0KICAgIH1lbHNlew0KICAgICAgICB2YXIgb2JqPWRhdGFUcmVlW21haW5dOw0KICAgICAgICByZXR1cm4gb2JqDQogICAgfQ0KDQp9DQpmb3IodmFyIGk9MDtpPHNoYXBlLmxlbmd0aDtpKyspew0KICAgIHNoYXBlW2ldLm9uY2xpY2s9ZnVuY3Rpb24gKCkgew0KICAgICAgICB2YXIgaWQ9TnVtYmVyKHRoaXMuZ2V0QXR0cmlidXRlKCdlZDp0b2d0b3BpY2lkJykpOw0KICAgICAgICB2YXIgb2JqPWdldG9iaihpZCk7DQoNCiAgICAgICAgdmFyIHR5cGU9b2JqLnRvZ3R5cGU9PT0nbWludXMnPydwbHVzJzonbWludXMnOw0KICAgICAgICB2YXIgZGlzcGxheT1vYmoudG9ndHlwZT09PSdtaW51cyc/J25vbmUnOidibG9jayc7DQogICAgICAgIHRoaXMuZ2V0RWxlbWVudHNCeVRhZ05hbWUoJ3VzZScpWzBdLnNldEF0dHJpYnV0ZSgneGxpbms6aHJlZicsJyMnK3R5cGUpOw0KICAgICAgICBvYmoudG9ndHlwZT10eXBlOw0KICAgICAgICBjaGVja0lEPW9iajsNCg0KICAgICAgICB1dGQob2JqLGlkLGRpc3BsYXkpOw0KICAgICAgICBleHRyYVJlbGFGaW4oKTsNCiAgICB9DQp9DQpmdW5jdGlvbiB1dGQob2JqLGlkLHNob3csb2MpIHsNCg0KICAgIHZhciBwc2hvdz1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChpZCkuc3R5bGUuZGlzcGxheSE9PSAnbm9uZSc/J2Jsb2NrJzonbm9uZSc7DQogICAgaWYgKG9iai5yZWxhZnJvbSl7DQogICAgICAgIGlmKG9iai5yZWxhZnJvbS5kaXNwbGF5IT09IHBzaG93KXsNCiAgICAgICAgICAgIHZhciByZWxhZnJvbXM9T2JqZWN0LmtleXMob2JqLnJlbGFmcm9tKTsNCiAgICAgICAgICAgIHJlbGFmcm9tcy5zcGxpY2UocmVsYWZyb21zLmluZGV4T2YoJ2Rpc3BsYXknKSwxKTsNCiAgICAgICAgICAgIGZvcih2YXIgaz0wO2s8cmVsYWZyb21zLmxlbmd0aDtrKyspew0KICAgICAgICAgICAgICAgIHZhciBkPXJlbGFmcm9tc1trXTsNCiAgICAgICAgICAgICAgICBleHRyYVJlbGFbZF0uaXNDPXRydWU7DQogICAgICAgICAgICB9DQogICAgICAgICAgICBvYmoucmVsYWZyb20uZGlzcGxheSA9IHBzaG93Ow0KICAgICAgICB9DQogICAgfQ0KICAgIGlmIChvYmoucmVsYXRvKXsNCiAgICAgICAgaWYob2JqLnJlbGF0by5kaXNwbGF5IT09IHBzaG93KXsNCiAgICAgICAgICAgIHZhciByZWxhdG9zPU9iamVjdC5rZXlzKG9iai5yZWxhdG8pOw0KICAgICAgICAgICAgcmVsYXRvcy5zcGxpY2UocmVsYXRvcy5pbmRleE9mKCdkaXNwbGF5JyksMSk7DQogICAgICAgICAgICBmb3IodmFyIGs9MDtrPHJlbGF0b3MubGVuZ3RoO2srKyl7DQogICAgICAgICAgICAgICAgdmFyIGQ9cmVsYXRvc1trXTsNCiAgICAgICAgICAgICAgICBleHRyYVJlbGFbZF0uaXNDPXRydWU7DQogICAgICAgICAgICB9DQogICAgICAgICAgICBvYmoucmVsYXRvLmRpc3BsYXkgPSBwc2hvdzsNCiAgICAgICAgfQ0KICAgIH0NCiAgICBpZihvYmouY2FsbCl7DQogICAgICAgIHZhciBjYWxscz1PYmplY3Qua2V5cyhvYmouY2FsbCk7DQogICAgICAgIGlmKGNoZWNrSUQhPT1vYmopew0KICAgICAgICAgICAgZm9yKHZhciBpPTA7aSA8IGNhbGxzLmxlbmd0aDtpKyspew0KICAgICAgICAgICAgICAgIHZhciBhPWNhbGxzW2ldLnNsaWNlKDEpOw0KICAgICAgICAgICAgICAgIHZhciBiPW9iai5jYWxsW2NhbGxzW2ldXTsNCiAgICAgICAgICAgICAgICB2YXIgYz1iLnRvZ3R5cGU7DQogICAgICAgICAgICAgICAgZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQoYSkuc3R5bGUuZGlzcGxheT1zaG93Ow0KICAgICAgICAgICAgICAgIGlmIChiLnJlbGFmcm9tJiYhYyl7DQogICAgICAgICAgICAgICAgICAgIGlmKGIucmVsYWZyb20uZGlzcGxheSE9PSBzaG93KXsNCiAgICAgICAgICAgICAgICAgICAgICAgIHZhciByZWxhZnJvbXM9T2JqZWN0LmtleXMoYi5yZWxhZnJvbSk7DQogICAgICAgICAgICAgICAgICAgICAgICByZWxhZnJvbXMuc3BsaWNlKHJlbGFmcm9tcy5pbmRleE9mKCdkaXNwbGF5JyksMSk7DQogICAgICAgICAgICAgICAgICAgICAgICBmb3IodmFyIGs9MDtrPHJlbGFmcm9tcy5sZW5ndGg7aysrKXsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICB2YXIgZD1yZWxhZnJvbXNba107DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgZXh0cmFSZWxhW2RdLmlzQz10cnVlOw0KICAgICAgICAgICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgICAgICAgICAgYi5yZWxhZnJvbS5kaXNwbGF5ID0gc2hvdzsNCiAgICAgICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICBpZiAoYi5yZWxhdG8mJiFjKXsNCiAgICAgICAgICAgICAgICAgICAgaWYoYi5yZWxhdG8uZGlzcGxheSE9PSBzaG93KXsNCiAgICAgICAgICAgICAgICAgICAgICAgIHZhciByZWxhdG9zPU9iamVjdC5rZXlzKGIucmVsYXRvKTsNCiAgICAgICAgICAgICAgICAgICAgICAgIHJlbGF0b3Muc3BsaWNlKHJlbGF0b3MuaW5kZXhPZignZGlzcGxheScpLDEpOw0KICAgICAgICAgICAgICAgICAgICAgICAgZm9yKHZhciBrPTA7azxyZWxhdG9zLmxlbmd0aDtrKyspew0KICAgICAgICAgICAgICAgICAgICAgICAgICAgIHZhciBkPXJlbGF0b3Nba107DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgZXh0cmFSZWxhW2RdLmlzQz10cnVlOw0KICAgICAgICAgICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgICAgICAgICAgYi5yZWxhdG8uZGlzcGxheSA9IHNob3c7DQogICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYoYyl7DQogICAgICAgICAgICAgICAgICAgIGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoImdbZWRcXDp0b2d0b3BpY2lkPSciK2ErIiddIikuc3R5bGUuZGlzcGxheT1zaG93Ow0KICAgICAgICAgICAgICAgICAgICBpZihjPT09J21pbnVzJyl7DQogICAgICAgICAgICAgICAgICAgICAgICB1dGQoYixhLHNob3cpDQogICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgaWYgKChiLmNhbGx8fGIuYm91bmRhcnl8fGIuc3VtbWFyeSkmJmM9PT0ncGx1cycpIHsNCiAgICAgICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdyx0cnVlKQ0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIGlmKGIuY2FsbCYmIWMpIHsNCiAgICAgICAgICAgICAgICAgICAgdXRkKGIsYSxzaG93LHRydWUpDQogICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIGlmIChiLnN1bW1hcnkmJiFjKSB7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYgKGIuYm91bmRhcnkmJiFjKSB7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQoNCiAgICAgICAgICAgIH0NCiAgICAgICAgfQ0KICAgIH0NCiAgICBpZihvYmouc3VtbWFyeSl7DQogICAgICAgIHZhciBzdW1tYXJ5cz1PYmplY3Qua2V5cyhvYmouc3VtbWFyeSk7DQogICAgICAgIGlmKChjaGVja0lEIT09b2JqJiYob2JqLnRvZ3R5cGU9PT0nbWludXMnfHwhb2JqLnRvZ3R5cGUpKXx8Y2hlY2tJRD09PW9iail7DQogICAgICAgICAgICBmb3IodmFyIGk9MDtpPHN1bW1hcnlzLmxlbmd0aDtpKyspew0KICAgICAgICAgICAgICAgIHZhciBhPXN1bW1hcnlzW2ldLnNsaWNlKDEpOw0KCQkJCXZhciBvc3AgPSBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5nZXRBdHRyaWJ1dGUoJ2VkOnBhcmVudGlkJyk7DQogICAgICAgICAgICAgICAgaWYob3NwLm1hdGNoKC9cLC8pKXsNCiAgICAgICAgICAgICAgICAgICAgdmFyIG9zcGEgPSBvc3Auc3BsaXQoJywnKTsNCiAgICAgICAgICAgICAgICAgICAgdmFyIG9zcEw9MDsNCg0KICAgICAgICAgICAgICAgICAgICBmb3IodmFyIGo9MDtqPG9zcGEubGVuZ3RoO2orKyl7DQogICAgICAgICAgICAgICAgICAgICAgICBpZihzaG93ID09ICdub25lJyl7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgaWYoZG9jdW1lbnQuZ2V0RWxlbWVudEJ5SWQob3NwYVtqXSkuc3R5bGUuZGlzcGxheSAhPSAnbm9uZScpew0KICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBicmVhazsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICB9ZWxzZXsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgb3NwTCsrOw0KICAgICAgICAgICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgICAgIH1lbHNlew0KICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBpZihkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5zdHlsZS5kaXNwbGF5ICE9ICdub25lJyl7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBicmVhazsNCiAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgfWVsc2V7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgICBvc3BMKys7DQogICAgICAgICAgICAgICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgICAgIH0NCg0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgICAgIGlmKG9zcEwgIT09IG9zcGEubGVuZ3RoKXsNCiAgICAgICAgICAgICAgICAgICAgICAgIGNvbnRpbnVlOw0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgfQ0KICAgICAgICAgICAgICAgIHZhciBiPW9iai5zdW1tYXJ5W3N1bW1hcnlzW2ldXTsNCiAgICAgICAgICAgICAgICAvLyBjb25zb2xlLmxvZyhhKTsNCiAgICAgICAgICAgICAgICBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5zdHlsZS5kaXNwbGF5PXNob3c7DQovLyAgICAgICAgICAgICAgICBpZihjKXsNCi8vICAgICAgICAgICAgICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6dG9ndG9waWNpZD0nIithKyInXSIpLnN0eWxlLmRpc3BsYXk9c2hvdzsNCi8vICAgICAgICAgICAgICAgICAgICBpZihjPT09J21pbnVzJyl7DQovLyAgICAgICAgICAgICAgICAgICAgICAgIHV0ZChiLHNob3cpDQovLyAgICAgICAgICAgICAgICAgICAgfQ0KLy8gICAgICAgICAgICAgICAgICAgIGlmIChiLmNhbGwmJmM9PT0ncGx1cycpIHsNCi8vICAgICAgICAgICAgICAgICAgICAgICAgdXRkKGIsc2hvdyx0cnVlKQ0KLy8gICAgICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIGlmKGIuY2FsbCYmIWMpIHsNCi8vICAgICAgICAgICAgICAgICAgICB1dGQoYixzaG93LHRydWUpDQovLyAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYoT2JqZWN0LmtleXMoYikubGVuZ3RoIT09MCl7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICB9DQogICAgaWYob2JqLmJvdW5kYXJ5KXsNCiAgICAgICAgdmFyIGJvdW5kYXJ5cz1PYmplY3Qua2V5cyhvYmouYm91bmRhcnkpOw0KICAgICAgICBpZihjaGVja0lEIT09b2JqKXsNCiAgICAgICAgICAgIGZvcih2YXIgaT0wO2k8Ym91bmRhcnlzLmxlbmd0aDtpKyspew0KICAgICAgICAgICAgICAgIHZhciBhPWJvdW5kYXJ5c1tpXS5zbGljZSgxKTsNCiAgICAgICAgICAgICAgICB2YXIgYj1vYmouYm91bmRhcnlbYm91bmRhcnlzW2ldXTsNCiAgICAgICAgICAgICAgICAvLyBjb25zb2xlLmxvZyhhKTsNCiAgICAgICAgICAgICAgICBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChhKS5zdHlsZS5kaXNwbGF5PXNob3c7DQovLyAgICAgICAgICAgICAgICBpZihjKXsNCi8vICAgICAgICAgICAgICAgICAgICBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6dG9ndG9waWNpZD0nIithKyInXSIpLnN0eWxlLmRpc3BsYXk9c2hvdzsNCi8vICAgICAgICAgICAgICAgICAgICBpZihjPT09J21pbnVzJyl7DQovLyAgICAgICAgICAgICAgICAgICAgICAgIHV0ZChiLHNob3cpDQovLyAgICAgICAgICAgICAgICAgICAgfQ0KLy8gICAgICAgICAgICAgICAgICAgIGlmIChiLmNhbGwmJmM9PT0ncGx1cycpIHsNCi8vICAgICAgICAgICAgICAgICAgICAgICAgdXRkKGIsc2hvdyx0cnVlKQ0KLy8gICAgICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIH0NCi8vICAgICAgICAgICAgICAgIGlmKGIuY2FsbCYmIWMpIHsNCi8vICAgICAgICAgICAgICAgICAgICB1dGQoYixzaG93LHRydWUpDQovLyAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgaWYoT2JqZWN0LmtleXMoYikubGVuZ3RoIT09MCl7DQogICAgICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdykNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICB9DQogICAgaWYoIW9jJiZvYmouY2hpbGQpIHsNCiAgICAgICAgdmFyIGNoaWxkcyA9IE9iamVjdC5rZXlzKG9iai5jaGlsZCk7DQogICAgICAgIGZvciAodmFyIGkgPSAwOyBpIDwgY2hpbGRzLmxlbmd0aDsgaSsrKSB7DQogICAgICAgICAgICB2YXIgYSA9IGNoaWxkc1tpXS5zbGljZSgxKTsNCiAgICAgICAgICAgIHZhciBiID0gb2JqLmNoaWxkW2NoaWxkc1tpXV07DQogICAgICAgICAgICB2YXIgYyA9IGIudG9ndHlwZTsNCiAgICAgICAgICAgIGRvY3VtZW50LmdldEVsZW1lbnRCeUlkKGEpLnN0eWxlLmRpc3BsYXkgPSBzaG93Ow0KCQkJdmFyIHRTUGF0aCA9IGRvY3VtZW50LnF1ZXJ5U2VsZWN0b3IoInBhdGhbZWRcXDp0b3N1cGVyaWQ9JyIrYSsiJ10iKTsNCiAgICAgICAgICAgIGlmKHRTUGF0aCl7DQogICAgICAgICAgICAgICAgdFNQYXRoLnN0eWxlLmRpc3BsYXkgPSBzaG93Ow0KICAgICAgICAgICAgfQ0KCQkJdmFyIG5vdGVUaXAgPSBkb2N1bWVudC5xdWVyeVNlbGVjdG9yKCJnW2VkXFw6bm90ZXRvPSciK2ErIiddIik7DQogICAgICAgICAgICBpZihub3RlVGlwKXsNCiAgICAgICAgICAgICAgICBub3RlVGlwLnN0eWxlLmRpc3BsYXkgPSBzaG93Ow0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgaWYgKGIucmVsYWZyb20mJiFjKXsNCiAgICAgICAgICAgICAgICBpZihiLnJlbGFmcm9tLmRpc3BsYXkhPT0gc2hvdyl7DQogICAgICAgICAgICAgICAgICAgIHZhciByZWxhZnJvbXM9T2JqZWN0LmtleXMoYi5yZWxhZnJvbSk7DQogICAgICAgICAgICAgICAgICAgIHJlbGFmcm9tcy5zcGxpY2UocmVsYWZyb21zLmluZGV4T2YoJ2Rpc3BsYXknKSwxKTsNCiAgICAgICAgICAgICAgICAgICAgZm9yKHZhciBrPTA7azxyZWxhZnJvbXMubGVuZ3RoO2srKyl7DQogICAgICAgICAgICAgICAgICAgICAgICB2YXIgZD1yZWxhZnJvbXNba107DQogICAgICAgICAgICAgICAgICAgICAgICBleHRyYVJlbGFbZF0uaXNDPXRydWU7DQogICAgICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICAgICAgYi5yZWxhZnJvbS5kaXNwbGF5ID0gc2hvdzsNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgICAgICBpZiAoYi5yZWxhdG8mJiFjKXsNCiAgICAgICAgICAgICAgICBpZihiLnJlbGF0by5kaXNwbGF5IT09IHNob3cpew0KICAgICAgICAgICAgICAgICAgICB2YXIgcmVsYXRvcz1PYmplY3Qua2V5cyhiLnJlbGF0byk7DQogICAgICAgICAgICAgICAgICAgIHJlbGF0b3Muc3BsaWNlKHJlbGF0b3MuaW5kZXhPZignZGlzcGxheScpLDEpOw0KICAgICAgICAgICAgICAgICAgICBmb3IodmFyIGs9MDtrPHJlbGF0b3MubGVuZ3RoO2srKyl7DQogICAgICAgICAgICAgICAgICAgICAgICB2YXIgZD1yZWxhdG9zW2tdOw0KICAgICAgICAgICAgICAgICAgICAgICAgZXh0cmFSZWxhW2RdLmlzQz10cnVlOw0KICAgICAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICAgICAgICAgIGIucmVsYXRvLmRpc3BsYXkgPSBzaG93Ow0KICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgIH0NCiAgICAgICAgICAgIGlmIChjKSB7DQogICAgICAgICAgICAgICAgZG9jdW1lbnQucXVlcnlTZWxlY3RvcigiZ1tlZFxcOnRvZ3RvcGljaWQ9JyIgKyBhICsgIiddIikuc3R5bGUuZGlzcGxheSA9IHNob3c7DQogICAgICAgICAgICAgICAgaWYgKGMgPT09ICdtaW51cycpIHsNCiAgICAgICAgICAgICAgICAgICAgdXRkKGIsYSxzaG93KQ0KICAgICAgICAgICAgICAgIH0NCiAgICAgICAgICAgICAgICBpZiAoKGIuY2FsbHx8Yi5ib3VuZGFyeXx8Yi5zdW1tYXJ5KSYmYz09PSdwbHVzJykgew0KICAgICAgICAgICAgICAgICAgICB1dGQoYixhLHNob3csdHJ1ZSkNCiAgICAgICAgICAgICAgICB9DQogICAgICAgICAgICB9DQogICAgICAgICAgICBpZiAoYi5jYWxsJiYhYykgew0KICAgICAgICAgICAgICAgIHV0ZChiLGEsc2hvdyx0cnVlKQ0KICAgICAgICAgICAgfQ0KICAgICAgICAgICAgaWYgKGIuc3VtbWFyeSYmIWMpIHsNCiAgICAgICAgICAgICAgICB1dGQoYixhLHNob3cpDQogICAgICAgICAgICB9DQogICAgICAgICAgICBpZiAoYi5ib3VuZGFyeSYmIWMpIHsNCiAgICAgICAgICAgICAgICB1dGQoYixhLHNob3cpDQogICAgICAgICAgICB9DQogICAgICAgIH0NCiAgICB9DQp9DQoNCmZ1bmN0aW9uIGV4dHJhUmVsYUZpbigpIHsNCiAgICB2YXIgZXh0cmFrZXlzPU9iamVjdC5rZXlzKGV4dHJhUmVsYSk7DQogICAgZm9yKHZhciBpPTA7aTxleHRyYWtleXMubGVuZ3RoO2krKyl7DQogICAgICAgIHZhciBleHRyYU9iaj1leHRyYVJlbGFbZXh0cmFrZXlzW2ldXTsNCiAgICAgICAgaWYoZXh0cmFPYmouaXNDID09PSB0cnVlKXsNCiAgICAgICAgICAgIHZhciBmc2hvdz1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChleHRyYU9iai5mcm9taWQpLnN0eWxlLmRpc3BsYXkgIT09J25vbmUnPyB0cnVlOiBmYWxzZTsNCiAgICAgICAgICAgIHZhciB0c2hvdz1kb2N1bWVudC5nZXRFbGVtZW50QnlJZChleHRyYU9iai50b2lkKS5zdHlsZS5kaXNwbGF5ICE9PSdub25lJz8gdHJ1ZTogZmFsc2U7DQogICAgICAgICAgICBkb2N1bWVudC5nZXRFbGVtZW50QnlJZChleHRyYU9iai5pZCkuc3R5bGUuZGlzcGxheT1mc2hvdyAmJiB0c2hvdz8gJ2Jsb2NrJzogJ25vbmUnOw0KICAgICAgICAgICAgZXh0cmFSZWxhW2V4dHJha2V5c1tpXV0uaXNDID0gZmFsc2U7DQogICAgICAgIH0NCiAgICB9DQp9'))</script>
  </body>




