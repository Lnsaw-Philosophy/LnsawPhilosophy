# Lnsaw API 介绍
## 什么是 Lnsaw API？
Lnsaw API 是一种基于 “**现实与指令思想**” 构建的全新通信范式。它旨在解决传统 RESTful API 在设计语义模糊、交互效率低下、可观测性复杂等方面的根本性问题。
其核心是 **InstructionStruct**（指令结构体），一个自包含的、结构化的交互单元，旨在统一描述从人到机器、从软件到硬件的一切协作。

## 核心设计思想
+ 1.**现实思想**：将交互中的所有参与者（人、组织、软件、设备）统一抽象为 Actor（行为主体）。
+ 2.**指令思想**：将所有交互抽象为 InstructionStruct 的传递。一个指令清晰表达了 “谁” 想让 “谁” 完成 “什么” 任务。

## 核心组件：InstructionStruct
一个 InstructionStruct 就像一封装订好的信件，包含完整的寄送和回复信息。
```JSON
{
  "globalId": "指令全局唯一ID",
  "parentId": "父指令ID（用于追踪）",
  "selfId": "本指令ID",
  "dispatch": { // 发信部分
    "envelope": { // 信封：路由与元数据
      "postedAt": "发送时间",
      "instructionType": "指令类型 (DO/GET/PUT/DEL/NOTIFY)",
      "sender": { "actor": "发送者身份" },
      "receiver": { "endpoint": "接收方地址", "route": ["/api/path"] },
      "intent": ["业务意图.动作"] // 如：["NaturalPerson.Login"]
    },
    "letter": [ // 信文：业务参数
      {"key": "value"}
    ],
    "journey": [] // 溯源链：记录本指令的触发上下文
  },
  "reply": { // 回信部分
    "receipt": { // 回执：协议状态
      "replyAt": "回复时间",
      "protocolStatus": "completed"
    },
    "letter": [ // 信文：业务结果
      {"code": 200, "data": {}, "message": "success"}
    ],
    "journey": [] // 溯源链：记录了完整的子指令
  }
}
```
## 示例：用户登录
让我们用一个用户登录的场景，对比传统 RESTful 与 Lnsaw API 的区别。

### 传统 RESTful 方式
#### 请求
```HTTP
POST /api/v1/token HTTP/1.1
Content-Type: application/json

{
  "username": "zhangsan",
  "password": "encrypted_hash"
}
```
#### 响应
```HTTP
HTTP/1.1 200 OK
{
  "access_token": "eyJhbGciOiJ...",
  "expires_in": 3600
}
```
#### 问题
+ **语义模糊**：我们是在创建(POST)一个叫token的资源吗？还是在进行身份验证？
+ **职责不清**：登录逻辑与 token 管理耦合。
+ **可观测性弱**：需要额外链路追踪工具来关联日志。

### Lnsaw API 方式

#### 请求 InstructionStruct
```JSON
{
  "globalId": "2024-05-21T08:00:00Z_abc123",
  "parentId": null,
  "selfId": "2024-05-21T08:00:00Z_def456",
  "dispatch": {
    "envelope": {
      "postedAt": "2024-05-21T08:00:00Z",
      "instructionType": "DO",
      "dispatchType": "json",
      "replyType": "json",
      "sender": {
        "actorType": "NaturalPerson",
        "actor": "anonymity",
        "agent": "bot:browser_chrome_edge"
      },
      "receiver": {
        "endpoint": "https://actorcenter.lnsaw.com:443",
        "route": ["/api/auth"]
      },
      "intent": ["NaturalPerson.Login"]
    },
    "letter": [
      {
        "username": "zhangsan",
        "password": "encrypted_hash"
      }
    ],
    "journey": []
  }
}
```
#### 响应 InstructionStruct
```JSON
{
  "globalId": "2024-05-21T08:00:00Z_abc123",
  "parentId": null,
  "selfId": "2024-05-21T08:00:00Z_def456",
  "dispatch": {...}, // 原样返回发信部分
  "reply": {
    "receipt": {
      "replyAt": "2024-05-21T08:00:00.120Z",
      "protocolStatus": "completed",
      "protocolCode": "200"
    },
    "letter": [
      {
        "code": 200,
        "message": "登录成功",
        "data": {
            "authorization": "token:lnsaw_at_xyz789...",      // 凭证本身，包含类型前缀
            "authorizationType": "token:Bearer",              // 明确的使用方式
            "expiresIn": 3600,                                // 生命周期
            "actorSummary": {                                 // 安全的身份摘要
                "actorType": "NaturalPerson",                   // 主体类型
                "displayName": "张三",                          // 显示名称
                "avatar": "https://...",                        // 形象标识
                "roleGroup": "Administrators"                   // 权限组
            }
        }
      }
    ],
    "internal": [ // 内部处理笔记
      {"step": "密码验证", "status": "passed"},
      {"step": "生成会话", "status": "success"}
    ],
    "journey": [] // 本次登录未调用其他Bot，故为空
  }
}
```
#### Lnsaw 登录方式的优势
+ **1.语义清晰**：intent: ["NaturalPerson.Login"] 明确表达了业务意图就是“自然人登录”，无需猜测。
+ **2.职责单一**：认证服务 (/api/auth) 只负责认证，返回的是完整的用户会话上下文，而非一个抽象的 token。
+ **3.内生可观测性**：整个交互过程自包含。如果登录过程中需要调用用户信息服务或风控服务，这些子调用会完整记录在 reply.journey 中，形成一个追踪树。
+ **4.强大的扩展性**：如果需要双因子认证，只需在 intent 数组中增加 ["2FA.Verify"] 即可，接收方会按顺序处理。

## 总结
Lnsaw API 通过引入 InstructionStruct 这一核心抽象，将分布式系统的交互从基于“资源”的视角，转变为基于“意图”和“协作”的视角。它不仅在性能上通过批量请求等方式带来提升，更重要的是，它在设计哲学层面提供了一种更自然、更清晰、更易于理解和维护的方式来构建复杂的协作系统。

这只是 Lnsaw 思想的初步应用。当与 Actor 模型结合时，它将能构建出真正智能、自主协作的数字生态。

***
*如果您想了解更多，请探索 [**Lnsaw 思想说明**] 和 [**InstructionStruct 完整规范**]。*
*关于 [**Actor**]，请探索 [**Lnsaw Actor**]。*




