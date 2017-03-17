**获取面单**

功能描述

* 该接口用于获取面单


**接口信息定义**

| 协议类型 | WebService SOAP |
| --- | --- |
| 接口 | <font color=blue>String printLabelService(String xml, String verifyCode);</font>  |

**数据元素定义**

| 字段名 | 类型 | 是否必填 | 默认值 | 描述 |
| --- | --- | --- | --- | --- |
| <font color=blue>数据元素<请求></font> | <font color=blue>label</font> | | | |
| labelType | String(30) | 是  | pdf | 面单类型 |
| paperSize | String(30) | 是  |  100X100 | 面单规格  |
| trackingNumber | String(30) | 是  |   | 面单追踪号 |
| | | | | |
| <font color=blue>数据元素<响应></font> | <font color=blue>labelResponse</font> | | | |
| trackingNumber | String(30) | 是  |   | 面单追踪号 |
| pdfUrl | String(60) |  | | 面单pdf路径 |
| status | boolean |  | | 获取面单状态|
| error | String(60) |  | | 获取面单错误信息|


**应用场景 XML 实例**

* 场景描述：通过追踪号获取面单

```

 
请求 XML：

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>

<request>

    <head>80010124</head>

    <body>
        <label>
            <labelType>pdf</labelType>
            <paperSize>100x100</paperSize>
            <trackingNumber>RLXXXXXXXXXX1</trackingNumber>
            <trackingNumber>RLXXXXXXXXXX2</trackingNumber>
        </label>
    </body>

</request>


```


```


订单处理成功：  


<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<response>
     <head>OK</head>
     <body>
          <labelResponse>
               <data>                   
                  <pdfUrl>http://xxx.xxxxx.com/ParcelPrintAction?snos=aba8b486</pdfUrl>
                   <status>true</status>
                   <trackingNumber>RLXXXXXXXXXX1</trackingNumber>
               </data>
               <data>
                    <pdfUrl>http://xxx.xxxxx.com/ParcelPrintAction?snos=c7c3f4fe</pdfUrl>
                    <status>true</status>
                    <trackingNumber>RLXXXXXXXXXX2</trackingNumber>
               </data>
          </labelResponse>
      </body> 
</response>

```