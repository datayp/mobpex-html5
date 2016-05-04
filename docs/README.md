
# Mobpex html5

### 简介

​        WAP站点商户、微信公众号商户可通过HTML5 SDK 接入支付宝WAP支付、银联WAP支付、易宝WAP支付、微信公众号支付等。

### 安装

下载HTML5 SDK,wapPay.html为默认收银台页面，用户可改造此收银台页面。

### 初始化收银台

设置initParam

    {//初始化WAP收银台页面参数
    	"orderInfo" : {                           // 订单信息
    		"orderNo" : "199810112011",           //商户订单号
    		"productName" : "商品名称",            //商品名称
    		"productDescription" : "商品描述",     //商品描述
    		"amount" : "0.01",                    //商品金额
    		"payee" : "湖南易宝天创数据服务有限公司",  //收款方 
    		"submitPayUrl" : "../demo/submitOrder"//提交支付请求url
    		//此url为商户url，商户后台在收到此地址的请求后调用server SDK获取支付凭证并作为响应返回。        
    	},
    	"channel" : [ 
    		{"payChannel":"ALIPAY","payType":"WAP"}, 
    		{"payChannel":"YEEPAY","payType":"WAP"}, 
    		{"payChannel":"UPACP","payType":"WAP"}, 
    		{"payChannel":"WECHAT","payType":"PUB"} 
    	]
    	//收银台可选支付渠道列表，payChannel和payType取值范围为APP可用支付渠道列表接口列出的channelCode和type组合
    	,"merchantUrl" : "https://www.mysite.com/myOrder"  //用户点击收银台返回按钮后跳转的地址
    }








#### 商户后台处理支付请求	

​        用户点击收银台某支付渠道进行支付后，会触发调用本SDK中your-js.js的submitPay方法，商户可改造此方法附带更多参数或者改变参数格式，默认的submitPay方法将会组织订单号、支付渠道、支付类型几个最基本参数POST到submitPayUrl地址，商户后台系统在接收到此地址的请求后先处理自身业务逻辑，之后需要调用Mobpex server SDK预支付请求接口获取支付凭证并作为响应返回给调用者,如果用户选择的是公众号支付则在发起预支付请求时需要传入扩展参数openid。



#### 显示更友好的错误信息

​        商户可以重写your-js.js里的errorCallBack方法，实现更友好的错误提醒。默认的在遇到错误的时候，收银台会通过alert提示错误信息。 微信支付在支付成功后会回调successCallBack方法，但支付请求的状态更新请以后台通知为准，其他支付渠道支付成功后会跳转到预支付请求中指定的returnUrl。



#### 实现更友好的等待状态

​        商户可以重写your-js.js里的showWait方法和endWait方法，实现更友好的等待状态显示。默认的在用户点击相应支付渠道后，为防止重复提交，startWait()方法会禁用掉各渠道的点击按钮。



