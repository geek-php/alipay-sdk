# alipay-sdk-php 命名空间版

![build=passing][ico-build]
[![Latest Version on Packagist][ico-version]][link-packagist]
[![Software License][ico-license]](LICENSE.md)
[![composer][ico-composer]][link-packagist]
[![Total Downloads][ico-downloads]][link-downloads]

alipay-sdk-PHP 由于没有使用PSR-4规范，默认需要引入AopSdk.php，这样默认是初始就加载了的。
另外默认带了lotusphp框架，这个就有点臃肿了。

本SDK在官网的基础上进行精简封装：

- 增加了命名空间
- 使用PSR-2代码规范格式化
- 去除lotusphp框架
- 去除无用代码


目前Request仅保留了`AlipayTrade`系列。
官方SDK获取：https://docs.open.alipay.com/54/103419，或者
http://aopsdkdownload.cn-hangzhou.alipay-pub.aliyun-inc.com/demo/alipay.trade.wap.pay-PHP-UTF-8.zip

## 安装
推荐使用`composer`安装：
```
composer require yjc/alipay-sdk
```

或者在composer.json里追加：
```
"require": {
	"yjc/alipay-sdk": "1.0"
},
```

## 使用示例
首先需要注册支付宝商家版。然后进行配置。

``` php
<?php
/**
 * Created by PhpStorm.
 * User: YJC
 * Date: 2018/10/29 029
 * Time: 22:32
 * @see https://docs.open.alipay.com/54/106370/
 */

$aop = new \Yjc\Alipay\Aop\AopClient();
$aop->gatewayUrl = "https://openapi.alipay.com/gateway.do";
$aop->appId = "app_id";
$aop->rsaPrivateKey = '请填写开发者私钥去头去尾去回车，一行字符串';
$aop->format = "json";
$aop->charset = "UTF-8";
$aop->signType = "RSA2";
$aop->alipayrsaPublicKey = '请填写支付宝公钥，一行字符串';
//实例化具体API对应的request类,类名称和接口名称对应,当前调用接口名称：alipay.trade.app.pay
$request = new \Yjc\Alipay\Aop\Request\AlipayTradeAppPayRequest();
//SDK已经封装掉了公共参数，这里只需要传入业务参数
$bizcontent = "{\"body\":\"我是测试数据\","
    . "\"subject\": \"App支付测试\","
    . "\"out_trade_no\": \"20170125test01\","
    . "\"timeout_express\": \"30m\","
    . "\"total_amount\": \"0.01\","
    . "\"product_code\":\"QUICK_MSECURITY_PAY\""
    . "}";
$request->setNotifyUrl("商户外网可以访问的异步地址");
$request->setBizContent($bizcontent);
//这里和普通的接口调用不同，使用的是sdkExecute
$response = $aop->sdkExecute($request);
//htmlspecialchars是为了输出到页面时防止被浏览器将关键参数html转义，实际打印到日志以及http传输不会有这个问题
echo htmlspecialchars($response);//就是orderString 可以直接给客户端请求，无需再做处理。

```


[ico-build]: https://img.shields.io/badge/build-passing-brightgreen.svg?maxAge=2592000
[ico-version]: https://img.shields.io/packagist/v/yjc/alipay-sdk.svg?style=flat-square
[ico-license]: https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square
[ico-downloads]: https://img.shields.io/packagist/dt/yjc/alipay-sdk.svg?style=flat-square
[ico-composer]: https://img.shields.io/badge/composer-yjc/alipay-sdk.svg?maxAge=2592000

[link-packagist]: https://packagist.org/packages/yjc/alipay-sdk
[link-downloads]: https://packagist.org/packages/yjc/alipay-sdk
[link-author]: https://github.com/52fhy

