# 图涵APP文档

PhotoSaying Client-Server Interface Document

> 文档编写于2018年7月22日 下午12:10

> 最后修改于2018年7月22日 下午14:50

## 数据库

### User表






## 网页端

### 首页

URL [https://photosaying.xht97.cn](https://photosaying.xht97.cn)

###### 支持格式
> HTML

###### HTTP请求方式
> GET

### 用户主页

URL [https://photosaying.xht97.cn/userHome](https://photosaying.xht97.cn/userHome)

###### 支持格式
> HTML

###### HTTP请求方式
> GET

### 社区主页

URL [https://photosaying.xht97.cn/community](https://photosaying.xht97.cn/community)

###### 支持格式
> HTML

###### HTTP请求方式
> GET

## 客户端Restful API

> 目前为API V1版本，API开头均为*https://photosaying.xht97.cn/api/v1/*

### 一.用户相关逻辑

#### 1.用户登录接口

##### 接口功能
> 根据用户设置的账号和密码验证执行登录操作

##### URL
> [https://photosaying.xht97.cn/api/v1/user/login](https://photosaying.xht97.cn/api/v1/user/login)

##### 支持格式
> JSON

##### HTTP请求方式
> POST

##### 请求参数
|参数|必选|类型|说明|
|:-----  |:-------|:-----|-----        |
|name    |ture    |string|用户名，可以为手机号邮箱或者唯一的用户昵称  |
|passwd    |true    |string   |用户输入的的密码     |

##### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|status   |boolean    |返回结果状态，true为成功，false为失败   |
|errorMsg   |int   |错误信息代号         |
|token  |string | 请求成功返回的Token，用于用户后续请求需要认证的api接口                     |

##### 接口示例
地址：[http://photosaying.xht97.cn/api/v1/login](http://photosaying.xht97.cn/api/v1/login)

```{json}
{
    "statue": ture,
    "errorMsg": 0,
    "token": "34wdsfasr123easdaqerw"
}
```

#### 2.用户注册接口

##### 接口功能
> 根据用户设置的账号和密码验证执行登录操作

用户输入信息申请验证码
##### URL
> [https://photosaying.xht97.cn/api/v1/user/register](https://photosaying.xht97.cn/api/v1/user/register)

##### 支持格式
> JSON

##### HTTP请求方式
> POST

##### 请求参数
|参数|必选|类型|说明|
|:-----  |:-------|:-----|-----        |
|name    |ture    |string|用户名，可以为手机号或者邮箱  |
|type      |true  |int        |用户注册时输入的是邮箱或者时手机号，手机号为1，邮箱为2  |
|passwd    |true  |string   |用户输入的的密码     |

##### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|status   |boolean    |返回结果状态，true为成功，false为失败   |
|errorMsg   |int   |错误信息代号         |
|message    |string   |结果信息   |

###### 接口示例
地址：[http://photosaying.xht97.cn/api/v1/register](http://photosaying.xht97.cn/api/v1/register)

```{json}
{
    "statue": ture,
    "errorMsg": 0,
    "message": "验证码发送成功"
}
```

用户获得验证码后，输入验证码验证合法性
##### URL
> [https://photosaying.xht97.cn/api/v1/user/register-auth](https://photosaying.xht97.cn/api/v1/user/register-auth)

##### 支持格式
> JSON

##### HTTP请求方式
> POST

##### 请求参数
|参数|必选|类型|说明|
|:-----  |:-------|:-----|-----        |
|name    |ture    |string|用户名，可以为手机号或者邮箱  |
|vericode      |true  |string        |用户获得到的验证码   |

##### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|status   |boolean    |返回结果状态，true为成功，false为失败   |
|errorMsg   |int   |错误信息代号         |
|message    |string   |结果信息   |

### 3.用户更新个人信息接口

##### URL
> [https://photosaying.xht97.cn/api/v1/user/update](https://photosaying.xht97.cn/api/v1/user/update)

##### 支持格式
> JSON

##### HTTP请求方式
> POST

##### 请求参数
|参数|必选|类型|说明|
|:-----  |:-------|:-----|-----        |
|name    |ture    |string   |用户名，可以为手机号或者邮箱  |
|content      |true  |string(json)      |用户需要修改的个人信息，格式为JSON，k-v对应   |

##### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|status   |boolean    |返回结果状态，true为成功，false为失败   |
|errorMsg   |int   |错误信息代号         |
|message    |string   |结果信息   |

### 4.请求用户信息

##### URL
> [https://photosaying.xht97.cn/api/v1/user/{id}](https://photosaying.xht97.cn/api/v1/user/{id})

##### 支持格式
> JSON

##### HTTP请求方式
> GET

##### 请求参数
|参数|必选|类型|说明|
|:-----  |:-------|:-----|-----        |
|id    |ture    |string   |请求的用户唯一ID  |

##### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|status   |boolean    |返回结果状态，true为成功，false为失败   |
|content   |string(json)      |请求的用户的信息，格式为JSON，k-v对应       |
|errorMsg   |int   |错误信息代号         |
|message    |string   |结果信息   |


### 二.图片处理业务


### 三.社区业务



## 请求码对应状态详细信息

|服务器状态码| 名称| 对应详细信息|
|:----|:----|:-----|
|200 |OK   |请求成功   |
|301   |Move Permanently   |资源位置被转移，使用心得URI访问资源   |
|400   |Bad Request   |一般性错误返回   |
|403   |Forbidden   |访问的资源没有权限   |
|404   |Not Found   |访问的资源不存在   |

|错误码  |对应详细信息 |
|:----  |:----      |
|0     |没有错误    |
|1      |用户名不存在     |
|2      |验证码校验错误     |
|      |     |
|      |     |
|      |     |
|      |     |
|      |     |
|      |     |