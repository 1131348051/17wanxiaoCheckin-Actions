# 🎨17wanxiaoCheckin



⚡2020.11.15：本项目已更新，使用本项目，你不需要抓包就可以使用（理论上大概......）



<u>中南林业科技大学</u> 测试可用，欢迎大家 fork 测试，如果可用的话，可以开 [issue](https://github.com/ReaJason/17wanxiaoCheckin-Actions/issues) 让更多人知道

感谢 [@zhongbr](https://github.com/zhongbr) 的完美校园逆向登录分析代码的分享：[完美校园模拟登录](https://github.com/zhongbr/wanmei_campus)

之前的抓包部署教程请前往：[完美校园抓包打卡](https://reajason.top/2020/06/28/17wanxiaoCheckin/)



#### 一、功能介绍

1. 完美校园模拟登录获取token
2. 自动获取上次提交的打卡数据
3. 每天早上六点自动打卡（有 10+ 分钟延迟）
4. 微信推送打卡消息

#### 二、打卡数据

细心的你应该会发现，自从第一次打卡之后，每次进去信息基本自动填写好了，我抓取的就是这个接口，

这样子也相当于大家不用抓包了，如果你进入完美校园健康打卡界面，它没有自动填写信息，可能本项目也就不起作用了。

```python
def get_post_json(self, token):
        jsons = {"businessType": "epmpics",
                 "jsonData": {"templateid": "pneumonia", "token": token},
                 "method": "userComeApp"}
        try:
            res = requests.post(url="https://reportedh5.17wanxiao.com/sass/api/epmpics", json=jsons)
        except:
            return None
        data = json.loads(res.json()['data'])
        post_dict = {
            "areaStr": data['areaStr'],
            "deptStr": data['deptStr'],
            "deptid": data['deptStr']['deptid'],
            "customerid": data['customerid'],
            "userid": data['userid'],
            "username": data['username'],
            "stuNo": data['stuNo'],
            "phonenum": data['phonenum'],
            "templateid": data['templateid'],
            "updatainfo": [{"propertyname": i["propertyname"], "value": i["value"]} for i in
                           data['cusTemplateRelations']],
            "checkbox": [{"description": i["decription"], "value": i["value"]} for i in
                         data['cusTemplateRelations']],
        }
        # print(json.dumps(post_dict, sort_keys=True, indent=4, ensure_ascii=False))
        logging.info('获取完美校园打卡post参数成功')
        return post_dict
```



#### 三、使用方法

1. 点击右上角的 fork，fork 本项目
2. 设置三个 secrets 字段：USERNAME、PASSWORD、SCKEY
3. 开启 Actions，修改 README.md 测试一次



