# 服务器后端API接口定义规范

* URL: <http://49.235.190.178:10010>
* Method: POST
* ContentType: application/x-www-form-urlencoded

> 如何发送消息

```javascript
  const io = require('socket.io-client');
  const socket = io('http://localhost:10020', {
    reconnectionDelayMax: 100000
  });
  // 订阅聊天室消息，这样就能接收到目标聊天室所有用户发送的消息
  socket.emit('subscribe', {roomID: 1});
  // 接受消息，根据返回的消息决定展示与否，如何展示
  socket.on('serverToClient', (message) => {
    /**
     * 返回的 message 对象包括发送的所有内容
     * roomID：聊天室ID
     * userID：用户ID
     * content：聊天内容
     * ------------------------------------
     * 下面这两个字段只有用户消息发送失败才会有
     * code：状态码（失败3001）
     * message：提示信息
     */
  });
  // 发送一条消息，用户输入发送消息时执行这段代码
  socket.emit('clientToServer', {
    roomID: 123,
    userID: 123,
    content: '今天天气真好',
  });
  // 退出前取消订阅
  socket.emit('unsubscribe', {roomID: 1});
  // 用户退出聊天室的时候应该断开连接
  socket.disconnect();
```

> 登陆: /api/login

| 请求参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| userID | string | '13978278212' | 用户账号/ID |
| password | string | '123456abc' | 密码 |

| 响应参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| code | number | 200/1001  | 状态码（成功/失败） |
| message | string | 'login success!' | 提示信息 |

> 注册: /api/logon

| 请求参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| userID | string | '13978278212' | 用户账号/ID |
| password | string | '123456abc' | 密码 |

| 响应参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| code | number | 200/1002  | 状态码（成功/失败） |
| message | string | 'logon success!' | 提示信息 |

> 修改密码: /api/modify/password

| 请求参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| userID | string | '13978278212' | 用户账号/ID |
| passwordOld | string | '123456abc' | 原密码 |
| passwordNew | string | '654321cba' | 新密码 |

| 响应参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| code | number | 200/1003  | 状态码（成功/失败） |
| message | string | 'modify password success!' | 提示信息 |

> 修改用户昵称: /api/modify/user_name

| 请求参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| userID | string | '13978278212' | 用户账号/ID |
| userName | string | '特工007' | 用户新昵称 |

| 响应参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| code | number | 200/1004  | 状态码（成功/失败） |
| message | string | 'modify userName success!' | 提示信息 |

> 创建聊天室: /api/create/chat_room

| 请求参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| roomName | string | '一起来聊天!' | 聊天室名称 |
| className | string | '美食' | 聊天室分类 |
| userID | string | '13978278212' | 用户账号/ID |
| capacity | number | 50 | 聊天室人数上限 |

| 响应参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| code | number | 200/2001  | 状态码（成功/失败） |
| message | string | 'create chatRoom success!' | 提示信息 |

> 通过ID获取聊天室信息: /api/get/chat_room/by_id

| 请求参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| roomID | number | 1 | 聊天室ID |

| 响应参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| code | number | 200/2002  | 状态码（成功/失败） |
| message | string | 'get chatRoom success!' | 提示信息 |
| chat_room | object | 略 | 聊天室信息（对象） |

> 通过分类获取聊天室信息: /api/get/chat_room/by_class

| 请求参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| className | string | '美食' | 聊天室分类 |

| 响应参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| code | number | 200/2003  | 状态码（成功/失败） |
| message | string | 'get chatRoom success!' | 提示信息 |
| chat_rooms | array\<object\> | 略 | 聊天室信息（对象数组）|

> 获取聊天历史: /api/get/chat_history

| 请求参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| roomID | number | 1 | 聊天室ID |
| dataNum | number | 1 | 返回的聊天消息数量 |

| 响应参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| code | number | 200/3002  | 状态码（成功/失败） |
| message | string | 'get chat history success!' | 提示信息 |
| chat_history | array\<object\> | 略 | 聊天内容（对象数组）|

> 获取聊天室活跃用户数量: /api/get/chat_room/users

| 请求参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| roomID | number | 1 | 聊天室ID |

| 响应参数名 | 类型 | 例子 | 说明 |
| ---- | ---- | ---- | ---- |
| code | number | 200/3003  | 状态码（成功/失败） |
| message | string | 'get chatRoom's users success!' | 提示信息 |
| number | number | 20 | 聊天室用户数 |
