### 前言

---

本文档目的是定义通用对外服务接口，以便快速集成大客户自有的系统，从而打通大客户与商家物流门户网站的数据流，实现整体物流供应链的一体化，进而达到与客户双赢。

### 对接流程

---

* 业务部与客户确认业务需求后，物流项目组安排人员与客户技术联系，沟通对接的相关技术问题。
* 客户需要按照此文档进行商家物流系统对接，对接完成后联系项目技术人员进行验证。
* 验证通过后会邮件通知客户技术接口人给予正式的对接地址和授权码后系统即可上线使用。

### 接口规范说明

---

通用对外服务接口统一使用 UTF-8 编码的 XML 报文，接口通信协议支持 WEBSERVICE协议：当使用 WEBSERVICE 接口时，报文通过方法参数传入（两个参数分别为 xml 报文及校验码）。

以下定义了通用对外服务接口报文需要遵循的格式与规则：

**请求报文规则**

* 请求报文规则
  * Head 元素预先定义了"客户代码";
  * "密钥"统由物流系统统一分配；


```
   请求报文:
     <request lang="zh-CN">
        <head>客户代码</head>
        <hody>请求数据 XML</body>
     </request>
```

**响应报文规则**

* 响应报文规则
  * head 元素值为 OK 或 ERR；OK 代表交易正常，ERR 代表发生系统或业务异常;
  * head 元素值为 OK 时返回 body 元素，为 ERR 时返回 error 元素；body 与 error 元素不会同时存在；
  * error 元素中内容为错误描述。


```
  响应报文:
     <response>
        <head>OK</head>
        <body>正常响应数据 XML</body>
     </response>

     <response>
        <head>ERR</head>
        <error>错误详细信息</error>
     </response>
 
```

**verifyCode 的生成规则**

* 接入系统前，系统管理员会为每个接入客户分配一个“密钥”;
* 先把 xml 与  密钥  前后连接;
* 把连接后的字符串做 MD5 编码;
* 把 MD5 编码后的数据进行 Base64 编码，字符编码为\(UTF-8\)，此时编码后的字符串即为 verifyCode。

### 服务接口

---

**批量订单**

功能描述   
* <font color=red>客户订单号, 不能为空</font>
* 下单接口根据客户需要，可提供以下功能：
    * 客户系统向物流系统下发订单;
    * 为订单分配追踪号。

**接口信息定义**

| 协议类型 | WebService SOAP |
| --- | --- |
| 接口 | <font color=blue>String batchOrderService(String xml, String verifyCode);</font>  |

**数据元素定义**

| 字段名 | 类型 | 是否必填 | 默认值 | 描述 |
| --- | --- | --- | --- | --- |
| <font color=blue>数据元素<请求></font> | <font color=blue>order</font> | | | |
| trackingNumber | String(30) |   |   | 不用传值  |
| customerReferenceNumber | String(30) | 是  |   | 客户订单号  |
| baseChannelCode | String(30) | 是  |   | 运输方式代码  |
| baseCountryCode | String(20) | 是  |   | 国家二字简码  |
| insuranceSign | int(6) |   | 0  | 是否购买保险,0,否,1,是  |
| insuranceAmount | decimal(10,3) |   |   | 保险金额  |
| goodsType | String(30) |   |  普货 | 敏感货品类型  |
| parcelType | String(30) |   |  Gift | 申报包裹类型  |
| currency | String(10) |   | USD  |  币种 |
| returnSign | int(1) |   | 0  |  是否需要退件,0,否 |
| remark | String(100) |   |   | 订单备注  |
| orderStatus | int(2) | 是  |  2 | 订单状态: 2, 预报  |
| cargoTotalQuantity | int(11) | | | 总数量 |
| cargoTotalWeight | decimal(10,3) | | | 总重量 |
| cargoTotalValue | decimal(10,3) | | | 总申报价值 |
| totalPieces | int(11) | | | 总箱数, 一票多件 |
|   |   |   |   |   |
| shipperCompany | String(60) | | | 寄件方公司名称 |
| shipperContact | String(60) | | | 寄件方联系人|
| shipperTel | String(60) | | | 寄件方联系电话|
| shipperMobile | String(60) | | | 寄件方手机|
| shipperEmail | String(100) | | | 寄件方邮箱|
| shipperAddress | String(200) | | | 寄件方详细地址|
| shipperCountry | String(30) | | CN| 寄件方国家二字简码|
| shipperProvince | String(30) | | | 寄件方所在省|
| shipperCity | String(30) | | | 寄件方所属城市|
| shipperPostCode | String(30) | | | 寄件方邮编|
| | | | | |
| deliveryCompany | String(60) | | | 收件人公司名|
| deliveryContact | String(60) | | | 收件人名|
| deliveryTel | String(60) | | | 收件人联系电话|
| deliveryMobile | String(60) | | | 收件人手机|
| deliveryEmail | String(100) | | | 收件人邮箱|
| deliveryAddress | String(200) | | | 收件人详细地址|
| deliveryCountry | String(10) | | | 目的国家简码|
| deliveryProvince | String(30) | | | 收件人所在省|
| deliveryCity | String(30) | | | 收件人所属城市|
| deliveryPostCode | String(30) | | | 收件人邮编|
| | | | | |
| <font color=blue>数据元素<请求></font> | <font color=blue>orderDeclarationInfo</font> | | | |
| pdNameEn | String(100) | 是 | | 申报名称(英文)|
| pdNameCn | String(100) |  | | 申报名称(中文)|
| pdWeight | decimal(10,3) | 是 | | 申报重量(kg)|
| pdQuantity | int(11)) | 是| | 申报数量 |
| pdValue | decimal(10,3) | 是 | | 申报价值|
| pdHscode | String(30) | | | 海关编码|
| pdSku | String(60) | | | SKU,产品编码|
| pdUrl | String(200) | | | 申报品URL|
| pdRemark | String(200) | | | 备注|
| | | | | |
| <font color=blue>数据元素<响应></font> | <font color=blue>orderResponse</font> | | | |
| customerReferenceNumber | String(60) |  | | 客户订单号|
| orderNumber | String(60) |  | | 物流系统内部订单号|
| trackingNumber | String(60) |  | | 跟踪号|
| status | String(10) |   |   | 状态:success, error |
| errorMsg| String(200) |   |   | 错误信息 |

**应用场景 XML 实例**
* 场景描述：客户下单   

```
请求 XML：
<request>
    <head>80010124</head>
    <body>
        <order>
            <baseChannelCode>SZXBR</baseChannelCode>
            <baseCountryCode>NZ</baseCountryCode>
            <buyerEmail>666@qq.com</buyerEmail>
            <cargoTotalQuantity>1</cargoTotalQuantity>
            <cargoTotalValue>0.23</cargoTotalValue>
            <cargoTotalWeight>1.5</cargoTotalWeight>
            <customerReferenceNumber>Test1477558130011</customerReferenceNumber>
            <deliveryAddress>奈良市学園南3丁目13-1737</deliveryAddress>
            <deliveryCity>Santiago,</deliveryCity>
            <deliveryCompany>Melissa Davis</deliveryCompany>
            <deliveryContact>Melissa Davis</deliveryContact>
            <deliveryProvince>奈良県</deliveryProvince>
            <deliveryCountry>NE</deliveryCountry>
            <deliveryPostCode>742492804</deliveryPostCode>
            <deliveryTel>414-672-9870</deliveryTel>
            <goodsType>Gift</goodsType>
            <insuranceAmount>0.0</insuranceAmount>
            <orderDeclarationInfo>
                <pdHscode>xxx</pdHscode>
                <pdNameCn>礼物1</pdNameCn>
                <pdNameEn>gift1</pdNameEn>
                <pdQuantity>1</pdQuantity>
                <pdRemark></pdRemark>
                <pdSku>yyy</pdSku>
                <pdUrl></pdUrl>
                <pdValue>12.3</pdValue>
                <pdWeight>1.1</pdWeight>
            </orderDeclarationInfo>
            <orderDeclarationInfo>
                <pdHscode>xxx</pdHscode>
                <pdNameCn>礼物2</pdNameCn>
                <pdNameEn>gift2</pdNameEn>
                <pdQuantity>1</pdQuantity>
                <pdRemark></pdRemark>
                <pdSku>yyy</pdSku>
                <pdUrl></pdUrl>
                <pdValue>12.3</pdValue>
                <pdWeight>1.1</pdWeight>
            </orderDeclarationInfo>
            <orderDeclarationInfo>
                <pdHscode>xxx</pdHscode>
                <pdNameCn>礼物3</pdNameCn>
                <pdNameEn>gift3</pdNameEn>
                <pdQuantity>1</pdQuantity>
                <pdRemark></pdRemark>
                <pdSku>yyy</pdSku>
                <pdUrl></pdUrl>
                <pdValue>12.3</pdValue>
                <pdWeight>1.1</pdWeight>
            </orderDeclarationInfo>
            <returnSign>0</returnSign>
            <totalPieces>1</totalPieces>
        </order>
        <order> ... </order>
        <order> ... </order>
 
    </body>
</request>

```

```
订单处理成功：
<response>
	<head>OK</head>
	<body>
		<orderResponse>
			<customerReferenceNumber>Test1477562333781</customerReferenceNumber>
			<orderNumber>33216102700010</orderNumber>
			<trackingNumber>RS278838702CN</trackingNumber>
			<status>success</status>
		</orderResponse>
		<orderResponse> ... </orderResponse>
		<orderResponse> ... </orderResponse>
	</body>
</response>

```

---

**修改订单状态**

功能描述 
*  该接口用于订单预报、取消预报 和 删除操作

**接口信息定义**

| 协议类型 | WebService SOAP |
| --- | --- |
| 接口 | <font color=blue>String changeOrderStatusService(String xml, String verifyCode);</font>  |

**数据元素定义**

| 字段名 | 类型 | 是否必填 | 默认值 | 描述 |
| --- | --- | --- | --- | --- |
| <font color=blue>数据元素<请求></font> | <font color=blue>changeOrderStatus</font> | | | |
| trackingNumber | String(30) | 是  |   | 服务商单号 |
| customerReferenceNumber | String(30) | 是  |   | 客户订单号  |
| orderNumber | String(30) | 是  |   | 物流系统内部单号 |
| orderStatus | int(2) | 是  | 2 | 状态标识 0=新建;1=确认;2=预报;-1=删除  |
| | | | | |
| <font color=blue>数据元素<响应></font> | <font color=blue>orderResponse</font> | | | |
| customerReferenceNumber | String(60) |  | | 客户订单号|
| orderNumber | String(60) |  | | 物流系统内部订单号|
| trackingNumber | String(60) |  | | 跟踪号|

**应用场景 XML 实例**

* 场景描述：修改订单状态

```

请求 XML：

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>

<request>  
    <head>80010124</head>
    <body>
         <changeOrderStatus 
              customerReferenceNumber="Test1477646286292"    
              orderNumber="33216102800004" 
              orderStatus="0" 
              trackingNumber="RS278838778CN"
         />
    </body>
</request>

```


```

订单处理成功：  

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>

<response>
     <head>OK</head>
     <body>
          <orderResponse>
              <customerReferenceNumber>Test1477646286292</customerReferenceNumber>           
              <orderNumber>33216102800004</orderNumber>    
              <trackingNumber>RS278838778CN</trackingNumber>
          </orderResponse>
      </body>
 </response>

```

---



**获取渠道信息**



功能描述

* 该接口用于获取渠道名称,编码等信息



**接口信息定义**

| 协议类型 | WebService SOAP |
| --- | --- |
| 接口 | <font color=blue>String channelService(String xml, String verifyCode);</font>  |


**数据元素定义**


| 字段名 | 类型 | 是否必填 | 默认值 | 描述 |
| --- | --- | --- | --- | --- |
| <font color=blue>数据元素<响应></font> | <font color=blue>baseChannelResponseDto</font> | | | |
| channelNameCn    | String(60) | 是  |   | 渠道中文名称 |
| channelNameEn    | String(60) | 是  |   | 渠道英文名称 |
| channelCode      |            | 是  |   | 渠道编码    |
| channelLabelType |            | 是  |   | P 平邮,R 挂号 |

**应用场景 XML 实例**

* 场景描述：下单用的渠道编码

```

请求 XML：

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<request>
    <body/>
    <head>80010124</head>
</request>

```


```

订单处理成功：  

<?xml version="1.0" encoding="UTF-8" standalone="yes"?>

<response>
     <head>OK</head>
     <body>
         <baseChannelResponseDto>
             <baseChannelDto>
                <channelCode>SZXBR</channelCode>
                <channelLabelType>R</channelLabelType>
                <channelNameCn>深圳邮政小包挂号</channelNameCn>
                <channelNameEn>Shenzhen Air Mail Registered</channelNameEn>
             </baseChannelDto>
             ......
             <baseChannelDto>
                 ...
             </baseChannelDto>             
         </baseChannelResponseDto>
     </body>
 </response>

```




