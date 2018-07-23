# 图涵APP开发文档

PhotoSaying Development Document(Client-Server Interface Doc)

> 文档创建于2018年7月22日 下午12:10

> 最后修改于2018年7月23日 下午18:20

*Created by xht, Modified by swx*

## 数据库

Sql Formatted:


```{sql}
create table ps_auth
(
	user_ID varchar(32) not null,
	passwd varchar(255) not null
)
;

create table ps_comment
(
	id int auto_increment
		primary key,
	user_ID varchar(32) not null,
	text varchar(255) not null,
	photosay_ID int not null,
	likes int default '0' not null,
	pubulish_time datetime default CURRENT_TIMESTAMP not null,
	comment_id int null
)
;

create table ps_photosay
(
	id int auto_increment
		primary key,
	user_ID varchar(32) not null,
	img_src varchar(255) not null,
	text varchar(255) not null,
	video varchar(35) not null,
	likes int default '0' not null,
	style varchar(255) not null,
	location varchar(255)    null,
	weather  varchar(255)    null
)
;

create table ps_text_pool
(
	id int auto_increment
		primary key,
	style varchar(255) not null,
	text varchar(255) not null,
	created_time datetime default CURRENT_TIMESTAMP not null,
	subject varchar(255) default '' not null
)
;

create table ps_user
(
	id varchar(32) not null
		primary key,
	user_name varchar(20) default '' not null,
	nick_name varchar(20) default '' not null,
	type int default '2' not null comment '0:管理员,1：VIP 2：普通用户',
	gender int default '0' not null comment '0:未知性别,1:男，2：女',
	phone varchar(13) default '' not null,
	email varchar(25) default '' not null,
	avatar varchar(50) default '' not null,
	bio varchar(30) default '' not null,
	created_time datetime default CURRENT_TIMESTAMP not null,
	updated_time datetime default CURRENT_TIMESTAMP not null,
	constraint user_name
		unique (user_name)
)
;

create table ps_user_log
(
	id int auto_increment
		primary key,
	user_ID varchar(32) not null,
	photo_message varchar(35) null,
	IP varchar(15) null,
	text varchar(35) not null,
	style varchar(255) null,
	created_time datetime default CURRENT_TIMESTAMP not null
)
;

create table ps_user_text
(
	id int auto_increment
		primary key,
	user_ID varchar(32) not null,
	text varchar(255) default '' not null,
	created_time datetime default CURRENT_TIMESTAMP not null,
	style varchar(255) not null
);
```


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
|message    |string   |结果信息   |
|token  |string | 请求成功返回的Token，用于用户后续请求需要认证的api接口                     |

##### 接口示例
地址：[http://photosaying.xht97.cn/api/v1/login](http://photosaying.xht97.cn/api/v1/login)

```{json}
{
    "statue": ture,
    "errorMsg": 0,
    "message": "用户登录成功",
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
> [https://photosaying.xht97.cn/api/v1/user/registerAuth](https://photosaying.xht97.cn/api/v1/user/registerAuth)

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
> [https://photosaying.xht97.cn/api/v1/user/info](https://photosaying.xht97.cn/api/v1/user/info)

##### 支持格式
> JSON

##### HTTP请求方式
> GET

##### 请求参数
|参数|必选|类型|说明|
|:-----  |:-------|:-----|-----        |
|user_ID   |ture    |string   |请求的用户唯一ID  |

##### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|status   |boolean    |返回结果状态，true为成功，false为失败   |
|errorMsg   |int   |错误信息代号         |
|message    |string   |结果信息   |
|content   |string(json)      |请求的用户的信息，格式为JSON，k-v对应       |


### 二.图涵内容核心业务

#### 1.语料库操作

根据某个图片主题内容与语料风格从语料库中获取相应的语料

##### URL
> [https://photosaying.xht97.cn/api/v1/core/fetchText](https://photosaying.xht97.cn/api/v1/fetchText)

##### 支持格式
> JSON

##### HTTP请求方式
> GET

##### 请求参数
|参数|必选|类型|说明|
|:-----  |:-------|:-----|-----        |
|subject    |true    |string   |图片主题内容  |
|type      |optional    |string      |语料风格        |

##### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|status   |boolean    |返回结果状态，true为成功，false为失败   |
|errorMsg   |int   |错误信息代号         |
|message    |string   |结果信息   |

用户推送自定义语料至自己的语料库（放入公开语料库要经过审核）

*TODO*

#### 2.用户处理图片使用记录

用户处理一张图片完成后，无论用户是否上传至社区均进行一次用户使用记录上传记录

##### URL
> [https://photosaying.xht97.cn/api/v1/core/userLog](https://photosaying.xht97.cn/api/v1/core/userLog)

##### 支持格式
> JSON

##### HTTP请求方式
> POST

##### 请求参数
|参数|必选|类型|说明|
|:-----  |:-------|:-----|-----        |
|token    |true    |string   |用户登录获得到的token  |
|photo_message      |true    |string      |图片配字      |
|text          |true      |string      |图片配字         |
|style          |true     |string      |文字风格         |

##### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|status   |boolean    |返回结果状态，true为成功，false为失败   |
|errorMsg   |int   |错误信息代号         |
|message    |string   |结果信息   |


#### 3.社区图涵相关操作

用户上传图涵至公开社区

##### URL
> [https://photosaying.xht97.cn/api/v1/core/upload](https://photosaying.xht97.cn/api/v1/core/upload)

##### 支持格式
> JSON

##### HTTP请求方式
> POST

##### 请求参数
|参数|必选|类型|说明|
|:-----  |:-------|:-----|-----        |
|token    |true    |string   |用户登录获得到的token  |
|img_src      |true    |string      |图片URL      |
|text          |true      |string      |图片配字         |
|style          |true     |string      |图片配字的风格         |
|location          |false     |string      |用户发送图涵的位置       |
|weather          |false     |string      |发送图涵地点的当时天气    |

##### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|status   |boolean    |返回结果状态，true为成功，false为失败   |
|errorMsg   |int   |错误信息代号         |
|message    |string   |结果信息   |

用户删除自己发表在公开社区的图涵内容


#### 4.社区图涵点赞

用户对于社区中的某条图涵点赞

##### URL
> [https://photosaying.xht97.cn/api/v1/core/like](https://photosaying.xht97.cn/api/v1/core/like)

##### 支持格式
> JSON

##### HTTP请求方式
> POST

##### 请求参数
|参数|必选|类型|说明|
|:-----  |:-------|:-----|-----        |
|token    |true    |string   |用户登录获得到的token  |
|photosay_ID      |true    |string      |图涵ID      |

##### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|status   |boolean    |返回结果状态，true为成功，false为失败   |
|errorMsg   |int   |错误信息代号         |
|message    |string   |结果信息   |

用户取消对于社区的某条图涵的点赞


#### 5.社区图涵评论

用户对于社区的图涵评论或者对于评论的回复

##### URL
> [https://photosaying.xht97.cn/api/v1/core/comment](https://photosaying.xht97.cn/api/v1/core/comment)

##### 支持格式
> JSON

##### HTTP请求方式
> POST

##### 请求参数
|参数|必选|类型|说明|
|:-----  |:-------|:-----|-----        |
|token    |true    |string   |用户登录获得到的token  |
|photosay_ID      |true    |string      |图涵ID      |
|comment_ID     |false   |string    |如果这条回复内容是回复另一个人的内容，这在这个参数中带上被回复的内容的ID    |
|content       |true    |string     |用户回复内容      |

##### 返回字段
|返回字段|字段类型|说明                              |
|:-----   |:------|:-----------------------------   |
|status   |boolean    |返回结果状态，true为成功，false为失败   |
|errorMsg   |int   |错误信息代号         |
|message    |string   |结果信息   |

用户删除自己的评论或回复


### 三.第三方API文档

###### SM.MS图床服务

[https://sm.ms/doc/](https://sm.ms/doc/)

###### 腾讯优图图片识别

[http://open.youtu.qq.com/#/develop/api-image-tag](http://open.youtu.qq.com/#/develop/api-image-tag)

###### 云片短信验证码业务

[https://www.yunpian.com/doc/zh_CN/introduction/format.html](https://www.yunpian.com/doc/zh_CN/introduction/format.html)

###### 和风实时天气服务

[https://www.heweather.com/documents/api/s6/weather-now](https://www.heweather.com/documents/api/s6/weather-now)


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