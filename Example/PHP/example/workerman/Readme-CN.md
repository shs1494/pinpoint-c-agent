## 为Workman框架编写插件的步骤

### 1. 在入口添加一个pre请求插件

```  php 
///@hook:example\workerman\HandleRequest::onMessage
class TcpServerPerRequestPlugin extends Candy
{
 public function onBefore(){}
 public function onEnd(&$ret){}
}
```

### 2. 在其他插件上添加common plugins (和 Example/PHP/Plugins 一样)

``` php
///@hook:example\UserManagerment::checkUser example\UserManagerment::register example\UserManagerment::cacheUser
class CommonPlugin extends Candy
{
    public function onBefore(){
        pinpoint_add_clue("stp",PHP_METHOD);
        pinpoint_add_clues(PHP_ARGS,print_r($this->args,true));
    }

    public function onEnd(&$ret){
        pinpoint_add_clues(PHP_RETURN,'xxx');
    }

    public function onException($e){
        pinpoint_add_clue("EXP",$e->getMessage());
    }
}
```