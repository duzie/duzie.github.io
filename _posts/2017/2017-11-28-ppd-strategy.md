---
layout: post
title: 拍拍贷最低投标金额设置
category: ppd
tags: [拍拍贷]
---

拍拍贷系统9月份更新后，最低起投为50元，但后台代码没有更新，还可以设置为20元。

## 很简单的哟

设置页面　[http://invest.ppdai.com/AutoBidManage/StrategyList](http://invest.ppdai.com/AutoBidManage/StrategyList)

页面执行
```
var url = '/AutoBidManage/UserStrategySwitch';
                    var data = {
                        strategyId: 193080, //策略ID
                        isOpen: true,
                        bidLimitAmount: 20　//最低金额
                    }
              
postData(url, data);

```