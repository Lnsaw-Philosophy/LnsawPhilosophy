# Lnsaw Api 的本质：一切交互皆为指令
| **Actor意图** | **指令** | **API** |
| - | - | - |
| **要** | 把那个东西给我看看 | `GET`|
| **给** | 这个东西你拿着 | `PUT` `PATCH`|  
| **不要了** | 把这个东西丢掉 | `DLETE` |
| **去做** | 去帮我寄一封信 | `POST` |

## 关于“给”的进一步说明：
+ PUT 像是“给你一个全新的盒子”（完整替换）。
+ PATCH 像是“给你一张便签，贴到原来的盒子上”（部分修改）。
+ 它们都是“给”这一核心意图在不同场景下的实现。在理想情况下，我们或许只需要一个“给”指令，但由于历史和技术原因，它们被分成了两种。

这种视角让我们穿透HTTP方法的表象，直接关注交互的最终目的。

# 指令体
>GET（要）, GIVE（给）, DEL（不要）, DO（执行）
```json
{
  "InstructionId": "DateTime.Utc +_+ GUID", //由agent自动生成
  "meta": {
    "method": "DO", // 核心指令： GET（要）, GIVE（给）, DEL（不要）, DO（执行）
    "intent": "NaturalPerson.Login",//写这封信的目的是什么？
    "origin": { // 发件人
        "actorType":"NaturalPerson",
        "actor": "anonymity", // 发件人，登录后才有发件人：token value/session value/jwt value，或者其他的actor标识
        //"actor":"token:A35D8X3E5A8CX3XD5F8A3GS8",
        //"actor":"JWT:fxcgaeff65F89GR5df21f5e", 
        "agent": "bot:browser_chrome_edge"     // 谁帮你发的？
    },
    "destination": { // 收件人
        "endpoint":"https://actorcenter.lnsaw.com:443", //收件人地址
        "route":"/api/auth",//收件人姓名
    },
    "sentAt": "2025-11-21T10:40:00.000Z",
  },
  "payload": {
    "username": "zhangsan",
    "password": "encrypted_hash"
  },
  "journey": { 
    [
        "meta": {
        "method": "DO", // 核心指令： GET（要）, GIVE（给）, DEL（不要）, DO（执行）
        "intent": "NaturalPerson.Login",//写这封信的目的是什么？
        "origin": { // 发件人
            "actorType":"NaturalPerson",
            "actor": "anonymity", // 发件人，登录后才有发件人：token value/session value/jwt value，或者其他的actor标识
            //"actor":"token:A35D8X3E5A8CX3XD5F8A3GS8",
            //"actor":"JWT:fxcgaeff65F89GR5df21f5e", 
            "agent": "bot:browser_chrome_edge"     // 谁帮你发的？
        },
        "destination": { // 收件人
            "endpoint":"https://actorcenter.lnsaw.com:443", //收件人地址
            "route":"/api/auth",//收件人姓名
        },
        "sentAt": "2025-11-21T10:40:00.000Z",
        },
        "payload": {
            "username": "zhangsan",
            "password": "encrypted_hash"
        },
    ]
  }
}
```

# 登录

这里我们用一个简单的登录的例子

当用户输入账号密码，点击登录，页面跳转到首页。整个过程中发生了什么？

+ 1、浏览器Bot作为自然人的代理，发出一个:[POST]https://actorcenter.lnsaw.com/api/auth/login
+ 2、Controller Bot auth 分配该指令给 login 业务员，login 业务员将对拿到的指令体进行分析，判断指令体是否有效，然后做出决定，是走直连通道，还是走分布式通道。
+ 3、如果走分布式通道，那么 login 业务员 会在原先的指令体上，增加自己的信息

判断Method和intent是否匹配


# 商品管理

