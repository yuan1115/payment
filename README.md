### 第三方支付扩展包

    目前只有支持支付宝PC网站端支付

#### 安装

* composer require jmx/payment       
* 需要注意安装完之后需要给jmx/payment/src/Api/log.txt文件添加写入权限
    * chmod 777　项目目录/vendor/jmx/payment/src/Api/log.txt                            

#### 文件目录结构

    |- payment                                              
        |- src
            |- Api 　　　　　　#此文件夹需要有写入权限
            |- Charge         
            |- Common           
            |- Config
            |- Charge.php    #入口
        |- autoload.php
        |- composer.json
        |- README.md

#### 示例代码
    <?php
    use alipay\Charge;
    
    class Alipay
    {
        private $pay;
        private $config = array(
            'use_sandbox' => true,
            'app_id'=>'appid',
            'sign_type' =>'RSA2',
            'alipay_public_key' => "支付宝公钥",
            'merchant_private_key' => '应用私钥',
            'notify_url' =>'http://xxx.com/AliPay/notify_url',
            'return_url' =>'http://xxx.com/AliPay/return_url'
        );

       public function pay()
       {
           $paydata = [
                'out_trade_no' => time(),
                'amount'   => 0.01,
                'client_ip' => '',
                'subject'  => '购买套餐',
                'body' =>'购买套餐',
                'qr_mod'=>2
            ];
           try {                                                   
                $this->pay = new Charge;
                $this->pay->run($type,$this->config,$paydata);  
            } catch (PayException $e) {
                echo $e->errorMessage();
                exit;
            }
       } 

       public function notify_url(){
           $this->pay->notify_url();
           //下面写业务代码

       }

       public function return_url(){
           $this->pay->return_url();
           //下面写业务代码
           
       }
    }

##### 参数说明

* $type　string 支付宝支付类型，详细参见Config/Config.php
* $aliconfig  array  支付宝配置参数
    
    字段名 | 解释 | 是否必填 | 可选值 
    :-: | :-: | :-: | :-: 
    app_id | 应用appid| yes|  | 
    merchant\_private_key | 应用对应的私钥| yes||
    alipay\_public_key | 应用对应的支付宝公钥| yes|| 
    sign\_type | 加密方式| no| RSA/RSA2，默认使用RSA2| 
    gatewayUrl | 支付宝的网关| no| 默认https://openapi.alipay.com/gateway.do| 
    charset | 采用的编码| no| utf-8| 
    use\_sandbox | 是否沙箱环境| no | 默认false| 
    notify\_url | 异步通知的地址| no || 
    return\_url | 同步通知的地址| no || 

* $paydata array 交易数据 ([更多参数](https://docs.open.alipay.com/api_1/alipay.trade.page.pay/))
    
    字段名 | 解释 | 是否必填 
    :-: | :-: | :-: 
    out\_trade_no | 订单号| yes|
    amount | 支付金额| yes|
    subject | 订单名称| yes|
    body | 商品描述| no|

#### 异步通知调用
    测试
#### 同步通知调用

