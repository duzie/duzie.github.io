---
layout: post
title: 拍拍贷付费策略优选
category: ppd
tags: [拍拍贷]
---

定时查找花钱策略，90天逾期率比较低的且有空位，抢了它。

## 定时抢

### 打开花钱策略页面

　[http://invstrat.ppdai.com/strategyList](http://invstrat.ppdai.com/strategyList)

### 页面执行
```
_ = document.createElement("script"); _.src = "https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"; document.body.appendChild(_); _.onload = function () { main() };

function main(){
    setInterval(query,10000);
}

function query(){
    $.post('http://strategy.ppdai.com/forward/forwardreq',
        {'Target':'strategyList-getStrategyList','RequestBody':'{"pageIndex":1,"pageSize":10,"rangeList":[],"nickName":"","sort":4}'},
        function(data){
            data = $.parseJSON(data);
            var list = data.data.strategyList;
            for(i = 0; i < list.length; i++){
                if(list[i].funding < 200){
                    buy(list[i].strategyId);
                }
            }
        }
    );
}

function buy(id){
    $.ajax({
        type: "POST",
        url: "http://strategy.ppdai.com/forward/forwardreq",
        data: {'Target':'strategydetail-buyStrategy','RequestBody':'{"bidAmount":"50","strategyId":'+ id +'}'},
        xhrFields: {
            withCredentials: true // 带上cookies
        },
        success:function(data){
            console.log(data);
        }
    });
}

```

当然也可以对准目标抢

## 传对应策略ID

```
var id = 751; //快投2号
_ = document.createElement("script"); _.src = "https://cdn.bootcss.com/jquery/3.2.1/jquery.min.js"; document.body.appendChild(_); _.onload = function () { main() };

function buy(){
    $.ajax({
        type: "POST",
        url: "http://strategy.ppdai.com/forward/forwardreq",
        data: {'Target':'strategydetail-buyStrategy','RequestBody':'{"bidAmount":"50","strategyId":'+ id +'}'},
        xhrFields: {
            withCredentials: true // 带上cookies
        },
        success:function(data){
            console.log(data);
        }
    });
}

setInterval(buy,10000);

```
