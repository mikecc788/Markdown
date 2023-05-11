## AirSmart API 接口文档  

###服务器ip

- http://159.75.204.128:5000/  

### 1 上传吹气数据 

####1.1 接口名称

- AirFlowRecord/Add

#### 1.2 请求方式

**POST**  

#### 1.3 请求参数

#####1.3.1 Header 参数

| 参数名          | 必选   | 类型/参数值           | 说明     |
| ------------ | ---- | ---------------- | ------ |
| Content-Type | 是    | application/json | 请求参数类型 |

#####1.3.2 Body 参数

| 参数名         | 必选   | 类型     | 限制条件            | 说明           |
| ----------- | ---- | ------ | --------------- | ------------ |
| deviceid    | 是    | string | 1 < length < 50 | 设备identifier |
| userid      | 是    | string | 1 < length < 50 |              |
| symptom     | 是    | string | length = 6      | 症状数组         |
| remakes     | 是    | string | length <255     | 备注           |
| fvcValue    | 是    | Double |                 |              |
| currentTime | 是    | string |                 | 当前测试时间       |
| pefValue    | 是    | Double |                 |              |
|             |      |        |                 |              |
|             |      |        |                 |              |

**注意事项**: 

**需要调用到的其他接口**:  

| 接口名称  | 接口地址                                   | 用途说明 |
| ----- | -------------------------------------- | ---- |
| 获取验证码 | `{apiAddress}/api/common/getCheckCode` |      |

​    

####1.4返回示例

```json
{
  	Data = "<null>"; // 返回对象 json
    Msg = "Incorrect datetime value: '2023' for column 'test_time' at row 1";// 提示信息
    Success = 0; // 返回内容 0 失败 1成功
}
```



### 2 获取吹气数据

#### 2.1 接口名称

- AirFlowRecord/Add

#### 2.2 请求方式

**POST**  

#### 2.3 请求参数

##### 2.3.1 Header 参数

| 参数名          | 必选   | 类型/参数值           | 说明     |
| ------------ | ---- | ---------------- | ------ |
| Content-Type | 是    | application/json | 请求参数类型 |

##### 2.3.2 Body 参数

| 参数名       | 必选   | 类型     | 限制条件            | 说明   |
| --------- | ---- | ------ | --------------- | ---- |
| deviceid  | 是    | string | 1 < length < 50 |      |
| userid    | 是    | string | 1 < length < 50 |      |
| symptom   | 是    | string | length = 6      |      |
| remakes   | 是    | string |                 |      |
| testvalue | 是    | string |                 |      |
| testtime  | 是    | string |                 |      |

**注意事项**: 

**需要调用到的其他接口**:  

| 接口名称  | 接口地址                                   | 用途说明 |
| ----- | -------------------------------------- | ---- |
| 获取验证码 | `{apiAddress}/api/common/getCheckCode` |      |

​    

#### 2.4返回示例

```json
{
  	Data = "<null>"; // 状态码
    Msg = "Incorrect datetime value: '2023' for column 'test_time' at row 1";// 提示信息
    Success = 0; // 返回内容
}
```



