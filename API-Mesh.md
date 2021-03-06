# Version：0.0.1
# CreateDate：2019/04/16

	#2019/04/16
	1. 主题词树：添加sid,去掉ename
	#2019/04/28
	1. 修改自动提示返回类型，由Json改为List
	2. 文本词查询返回值增加检索词

# 主题词接口设计
1. 主题词详情
2. 自动提示
3. 文本词查询
4. 主题词导航树

## 主要参数简介
>请求方式：POST  
>请求体：JSON

检索词: query  
起始ID号：start  
每页条数： size  

## 主题词详情
> 通过主题词ID号，获取单条主题词的详细信息  
> URL："http://127.0.0.1:8000/api/mesh/details/"  
> 传入的Json参数    

    {
    	"mid":"D008168"，
    	"user_info":{
    		"phone":"13233543654"，
    		"key":"ee38f3401083dadc1c8ebbcba9bee77b"，
    		"ip":"1.1.1.12"
    	}
    }
> 返回的Json数据

    {
    	"status":"1100", //成功1100
    	“error”:"不是正确的ID号格式"，//失败才会有该参数
    	"details":{
    			"uid":"D008168",
    			.......
    		}
    }

## 自动提示
> 根据用户输入字母，自动提示用户可能想输入的词，也称自动补全  
> URL："http://127.0.0.1:8000/api/mesh/suggests/"
> 传入的Json参数  

    {
    	"query":"lun",
    	"size": 10, //可选参数， 默认返回前十条
    	"user_info":{
    		"phone":"13233543654"，
    		"key":"ee38f3401083dadc1c8ebbcba9bee77b"，
    		"ip":"1.1.1.12"
    	}
    }

> 返回的Json数据

    {
    	"status":"1100", //成功1100
    	"suggestions":{
    		'Lung Diseases':{
              		"ename" : "Lung Diseases",
              		"cname" : "肺疾病"
    		},
    		"Lung Abscess":{
    	              "ename" : "Lung Abscess",
    	              "cname" : "肺脓肿"
    		}
    	} //返回的为列表形式的建议词
    }

## 文本词查询
> 根据用户输入检索词，查询相应的主题词，并返回检索结果  
> URL："http://127.0.0.1:8000/api/mesh/results/"
> 传入的Json参数  

    {
    	"query":"lung",
    	"start":0, //可选参数， 默认从第一条开始
    	"size":10 //可选参数， 默认返回前十条
    }

> 返回的Json数据

    {
    	"status":"1100", //成功1100
    	"total":22, //查询结果总数
    	"error":"您输入的检索词lnug未找到结果，已为您展示Lung的查询结果"，//错误信息，如果由
    	"results":[{
    			'cname': '肺',
    			'uid': 'D008168',
    			'ename': 'Lung'
    		}, {
    			'cname': '肺移植',
    			'uid': 'D016040',
    			'ename': 'Lung Transplantation'
    		}], //查询结果，由Json对象组成的列表
    	"exact": {
    	        "uid": "D008168",
    	        "ename": "Lung",
    	        "cname": "肺"
    		}
    }

## 主题词导航树
> 逐级返回导航树
> URL："http://127.0.0.1:8000/api/mesh/trees/"
> 传入的Json参数  

	{
		"fid":-1, //初始界面为-1
		"user_info":{
			"phone":"13233543654"，
			"key":"ee38f3401083dadc1c8ebbcba9bee77b"，
			"ip":"1.1.1.12"
		}
	}

>返回的Json参数

	{
		"status":"1100", //成功1100
		"tree":[
			{
			"sid": "A",
			"uid": null,
			"hasChild": 1,
			"cname": "疾病类"
			},
			{
			"sid": "A",
			"uid": null,
			"hasChild": 1,
			"cname": "现象与过程类"
			},
			....................
		]
	}