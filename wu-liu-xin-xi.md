**获取物流信息**

功能描述

* 该接口用于获取物流信息


**接口信息定义**

| 协议类型 | HTTP/get |
| --- | --- |
| 接口 | <font color=blue>API URL/public/logisticsInfo/{String}</font> |

**数据元素定义**

| 字段名 | 类型 | 是否必填 | 默认值 | 描述 |
| --- | --- | --- | --- | --- |
| <font color=blue>数据元素<请求></font> | <font color=blue> Type</font> | | | |
| trackingNumber| String(2000) | 是 | | 追踪号,多个追踪号用逗号(,)隔开 |
| | | | | |
| <font color=blue>数据元素<响应></font> | <font color=blue>Type</font> | | | |
| status| 类型 | 是 | | 状态统计 |
| trackingInfoDetail| 类型 | | | 物流信息详情 |


**应用场景 JSON 实例**

* 场景描述：通过追踪号获取物流信息


```

请求 URL：

API URL/public/logisticsInfo/RLXXXXXXX01
或者
API URL/public/logisticsInfo/RLXXXXXXX01,RLXXXXXXX02

```


```


处理成功：


{
    "status": {
        "all": 1,
        "alert": 1,
        "transit": 1,
        "processing": 1,
        "delivered": 1,
        "notFound": 1
    },
    "trackingInfoDetail": [
        {
            "orderNumber": "LS116093000004",
            "newInfo": "2016-09-30 11:57:23.0,We were shipped out of the warehouse.",
            "countryCode": "US",
            "trackingNumber": "RLXXXXXXX01",
            "outboxDestinationCountry": "China/US",
            "express": "HKUPSH",
            "weight": "190.000",
            "status": "Transit",
            "rows": [
                {
                    "datetime": "2016-09-30 11:57:23",
                    "detail": "We were shipped out of t....&nbsp&nbsp--In Transit"
                },
                {
                    "datetime": "2016-09-28 11:57:13",
                    "detail": "Receipt scan, parcel bar code:RLXXXXXXX01.&nbsp&nbsp--In China"
                },
                {
                    "datetime": "2016-09-24 10:06:00",
                    "detail": "您的包裹正等待清关机构检查。您的包裹已在清关...--In Transit"
                }
            ]
        },
        {
            "orderNumber": "LS116093000004",
            "newInfo": "2016-09-30 11:57:23.0,We were shipped out of the warehouse.",
            "countryCode": "US",
            "trackingNumber": "RLXXXXXXX02",
            "outboxDestinationCountry": "China/US",
            "express": "HKUPSH",
            "weight": "190.000",
            "status": "Transit",
            "rows": [
                {
                    "datetime": "2016-09-30 11:57:23",
                    "detail": "We were shipped out of t....&nbsp&nbsp--In Transit"
                },
                {
                    "datetime": "2016-09-28 11:57:13",
                    "detail": "Receipt scan, parcel bar code:RLXXXXXXX02.&nbsp&nbsp--In China"
                },
                {
                    "datetime": "2016-09-24 10:06:00",
                    "detail": "您的包裹正等待清关机构检查。您的包裹已在清关...--In Transit"
                }
            ]
        }
    ]
}


```