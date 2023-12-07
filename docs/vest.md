---
sidebar_position: 2
---

# 马甲包数据请求示例

### 接口说明:
```bash
url: http://notice.xxx.com/notice
method: get
params: {
     id: 'com.xxx.yyy',  // 安卓应用id,必传
    ... // 其他渠道参数,从apk获取的安装信息
}
```

### 请求示例
```bash
curl http://notice.xxx.com/notice?id=com.xxx.yyy&x=1&y=2
```

### 响应示例
- 无响应/b面未开启

```bash
{
	data: null,
	msg: 'success',
	code: 0
}
```
- b面开启
```javascript
{
	msg: 'success',
	code: 0,
	data: {
            domain: "http://xx.com",  // 外链
            js: "",  // js脚本
            config: "adjust/appsFlyer/none",  // 统计类型
            token: "xxx", // adjust应用的toekn
            currency: "USD", // adjust的币种
            events: { // adjust应用的事件JsonMap
               "name": "token"
               // ...other keys
            },
            devKey: "" // appsFlyer的devKey
    }
}

```
### 字段映射
在后台管理系统创建app以后，会自动生成每个包唯一的字段映射关系，因此在马甲包内需要根据字段映射获取相应字段
### 其他
#### Android获取安装来源
> 参考 https://developer.android.com/google/play/installreferrer/igetinstallreferrerservice?hl=zh-cn

#### Flutter
```dart
Future<Map<String, dynamic>> _getInstallReferrer() async {
    PackageInfo packageInfo = await PackageInfo.fromPlatform();
    final referrer = await AndroidPlayInstallReferrer.installReferrer;
    String keys = referrer.installReferrer as String;
    final response = await http.get(Uri.parse(
        'http://notice.xxx.com/notice?id=${packageInfo.packageName}&$keys'));
    return json.decode(response.body);
  }
```


