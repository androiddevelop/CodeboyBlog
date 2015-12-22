---
layout: post
title: 'Bootstrap输入框建议库 autosuggest.js'
date: '2015-12-22'
header-img: "img/post-bg-web.jpg"
tags:
     - web
author: 'Codeboy'
---

轻量级输入提示控件auto suggest.github地址:[https://github.com/androiddevelop/autosuggest.js](https://github.com/androiddevelop/autosuggest.js)。首先看一下下面例子:

<iframe src="https://example.codeboy.me/autosuggest/index.html" width="100%" height="200px" frameborder="0" scrolling="no"> </iframe>

# autosuggest.js([English Introduction](#english))

适用于Bootstrap的轻量级Ajax输入提示控件([Demo](https://example.codeboy.me/autosuggest/index.html))

在[bootcomplete.js](https://github.com/getwebhelp/bootcomplete.js)的基础上改进，改动如下:

1. **文本框是去焦点时自动隐藏输入提示组件**
2. **增加最大建议数目限制**
3. **增加键盘方向键选择**

### 依赖

- [jQuery](https://jquery.com/download/)(>=1.0)
- [Bootstrap](http://getbootstrap.com/getting-started/)(>=3.0)


### 基本使用

	$('#input').autosuggest({url:'/search.php'});
	
### json数据格式(必须)

	[ 
	  {
       "id" : someId, 
       "label" : "some label name"
      }
	]

> 如果服务端返回数据非此种格式，请修改。
>

### 参数

#### url: 

提交url,注意跨域问题

#### method(非必须):

请求方式(get, post), 默认get

#### queryParamName(非必须)

传递当前输入框的值时的参数名称,默认query,即如果是get方式并保持该值为默认值,则请求url为xxx.com?query=input_value,如果设置此值为search,则url为xxx.com?search=input_value
 
#### wrapperClass(非必须):

包围输入框外层div的css样式

#### menuClass(非必须):

自动补全菜单的css样式，如果需要自定义请提供

### minLength(非必须):

发起请求的最小长度，只有>= 此长度时才会出现建议框,默认最小长度为2

#### maxNum(非必须):

最大建议数目,默认最多给出10个建议提示

#### extra: 

除了queryParam之外的其他参数. 使用: 

	 "key1" : "value1",
	 "key2" : "value2"
	   
### 例子
    
      $("#test").autosuggest({
                url: 'city.json',
                minLength: 1,
                maxNum: 3,
                queryParamName: 'search'
                method: 'post',
                queryParamName: 'search',
                extra: {
                    "key1": "value1",
                    "key2": "value2"
                }
            });


# <span id="english">English Introduction </span>

Lightweight AJAX word suggest for Bootstrap ([Demo](https://example.codeboy.me/autosuggest/index.html))

a branch of [auto-suggest.js](https://github.com/getwebhelp/auto-suggest.js)，the changes are as follows:

1. **close suggestions menu when the input field loses focus.**
2. **add max suggestions number control.**
3. **add keyboard selection operation**

### Require

- [jQuery](https://jquery.com/download/)(>=1.0)
- [Bootstrap](http://getbootstrap.com/getting-started/)(>=3.0)


### Basic Usage

	$('#input').autosuggest({url:'/search.php'});
	
### JSON Response Object

	[ 
	  {
       "id" : someId, 
       "label" : "some label name"
      }
	]


### Options

#### url: 

The url to submit query

#### method:

Request method (get, post), Default get

####  queryParamName:

The name  of query parameter name with the input filed value. Default 'query', if the request method is get and use default value, then the request url is xxx.com?query=input_filed_value,and if you set queryParamName to 'search',则url为xxx.com?search=input_filed_value
 
#### wrapperClass:

CSS Class used for the element wrapper

#### menuClass:

CSS Class used for the suggestions menu

#### minLength:

Minimum string length before sending query request, Default 2

#### maxNum:

Max suggestions num , Default: 10

#### extra:
	
Send extra data parameters with request. Usage: 
	
	 "key1" : "value1",
	 "key2" : "value2"
	 
	
### Example

    $("#test").autosuggest({
                    url: 'city.json',
                    minLength: 1,
                    maxNum: 3,
                    method: 'post',
                    queryParamName: 'search'
                    extra: {
                        "key1": "value1",
                        "key2": "value2"
                    }
                }); 



> 如有任何知识产权、版权问题或理论错误，还请指正。
>
> 转载请注明原作者及以上信息。
