# Actor Speech
Actor Speech 是 Actor 用来交流的语言，它们的交流方式，非常类似人类世界的信件。
## ChatLog
**ChatLogItem** 主要由三部分构成：
+ 1、**信件标识**：包含三个ID，
+ 2、**发信 (sendLetter)**：包括了信件的发出方、意图和内容。
+ 3、**回信 (replyLetter)**：包括了信件的处理结果和流转过程。
``` JSON
"ChatLogItem":{
  "globalId": "GUID",
  "parentId": null,
  "selfId": "GUID",
  "sendLetter": {},
  "replyLetter": {},
}
```
## :envelope_with_arrow:发信 (sendLetter)
由信封 (`envelope`) 和信件 (`letter`) 两部分组成。
### 信封 (envelope)
+ **postedAt**：信件的发送时间，由发送信件的邮局填写。
+ **purpose**：信件的意图，根据邮局提供的可选意图填写。可自定义，但存在风险：包括并不限于邮局拒绝处理。
  + **ACT**：**执行**，希望收信人**执行**一个动作或流程。这是最**通用**的意图。
  + **GET**：**获取**，表示“我需要你为我提供某物”。为明确获取范围，可细分为：
    + **GET-F (Full)**:期望获取一个完整的实体。
      >"请将档案柜中的‘张三’档案袋，拿给我看看"
    + **GET-P (Partial)**:期望只获取实体的部分信息。
      >"请将档案柜中的‘张三’档案袋的个人信息页，拿给我看看。档案袋中的其他部分不用拿给我。"
  + **PUT**: **给予**，表示“我给你一个实体/实体的一部分”。为精确表达意图，可细分为：
    + **PUT-C (Create)**: 期望**创建**一个新实体。仅当目标不存在时执行操作。
      >“我将一份员工‘张三’的档案袋交给你，请你放入档案柜。如果柜子里已经有一个‘张三’的档案袋，什么也不用做。”
    + **PUT-U (Update)**: 期望**更新**一个已存在的实体。仅当目标存在时执行操作。
      >“我将一份员工‘张三’的档案袋交给你，请你替换掉档案柜中的‘张三’档案袋。如果柜子没有‘张三的档案袋’，什么也不用做。”
    + **PUT-S (Set)**: 期望确保某个实体**存在**，并且其内容与`letter`所描述的完全一致。
      >“我将一份员工‘张三’的档案袋交给你，请你确保档案柜中‘张三’的档案就是我给你这份。如果之前没有，就把这份放进去；如果之前有，就把旧的拿出来，把这份新的放进去。”
    + **PUT-P (Patch)**: 期望**修改**实体的**一部分**。
      >“我将一页属于一份‘张三’档案袋的个人信息页给你，请你替换掉档案柜中‘张三’档案袋里的旧的个人信息页。如果柜子里根本没有‘张三’的档案，什么也不用做。

      >“请将档案柜中‘张三’档案袋中的个人信息页拿出来扔掉，如果柜子里根本没有‘张三’的档案，什么也不用做。
  + **DEL**: **丢弃**，希望收信人将某物**丢弃**。
      >“档案柜里的‘张三’档案袋不需要了，请你把整个档案袋从档案柜中移除并销毁。”
  + **NOTIFY**：**通知**，一种单向通信机制，用于告知收信人一个事件或状态，而不期望或要求其对通知内容本身进行业务处理。
    + **NOTIFY-S (Silent)**：**静默通知**，发送后不期待任何形式的响应。
    + **NOTIFY-A (Acknowledged)**：**回执通知**，要求返回一个送达回执。
+ **sendLetterType**：发信的 sendLetter.letter 数据格式（如 json, xml）。
+ **replyLetterType** 期望回信的 replyLetter.letter 数据格式（如 json, xml）。
+ **sender**：**发信人**。
  + **actor**：**发信主体**。发信主体的唯一标识。其值根据认证状态和方式而定：
    + **actorType**:**主体类型**。表明发信主体的类别，例如：NaturalPerson（自然人）、LegalPerson（法人）、Bot（机器人）。
    + **actorId**:**主体标识**
        >**匿名**："anonymity",
        >**通过 `Token` 认证的主体**："token:A35D8X3E5A8CX3XD5F8A3GS8",
        >**通过 `JWT` 认证的主体**："JWT:fxcgaeff65F89GR5df21f5e==", 
        >**通过 actorId 本身**："actorId:af62F6+DS165SDF4==", 
  + **agent**：**信使**，是谁帮你投递的信件。
    + **actorType**:**信使类型**。表明代理主体的类别，例如：NaturalPerson（自然人）、LegalPerson（法人）、Bot（机器人）。
    + **actorId**:**信使标识**
        >**匿名**："anonymity",
        >**浏览器**:"fingerprint:browser_chrome_edge",
        >**通过 `Token` 认证的主体**："token:A35D8X3E5A8CX3XD5F8A3GS8",
        >**通过 `JWT` 认证的主体**："JWT:fxcgaeff65F89GR5df21f5e==", 
        >**通过 actorId 本身**："actorId:af62F6+DS165SDF4==", 
+ **receiver**：**收信人**。
  + **endpoint**：**小区**。收件人的小区。
    >"https://actorcenter.lnsaw.com:443"
  + **address**：**地址**。收件人的楼号、单元、楼层、门牌：反正是送到地方了，至于是不是串门的人拆了信件。那就不知道了
    >["/api/auth"]
  + **actor**:**收信主体**。除了直接上门，也可以根据收件人的信息配送，但是要小心重名的。
    + **actorType**:**主体**。表明收信主体的类别，例如：NaturalPerson（自然人）、LegalPerson（法人）、Bot（机器人）。
    + **actorId**:**主体标识**
        >**所有人**："all", //什么？寄给小区的所有人？你确定嘛？你的 purpose 是 ACT ，不是 NOTIFY ！嗨呀！！！我回来了，这个小区的保安，不让我这样干。你重发一个吧，我走了。
        >**通过 `Token` 认证的主体**："token:A35D8X3E5A8CX3XD5F8A3GS8",
        >**通过 `JWT` 认证的主体**："JWT:fxcgaeff65F89GR5df21f5e==", 
        >**通过 actorId 本身**："actorId:af62F6+DS165SDF4==", 
  + **【重要约束】**：`actor` 与 `route` 数组互斥，不可同时使用。
+ **intent**：**具体意图**，写这个信的详细目的有那些呢?
  >["NaturalPerson.Login"]
  >["NaturalPerson.Login","NaturalPerson.Info"]
  >["Order.Place.Prepare","Inventory.Lock","Payment.Create"]
### 信件 (letter)
这里装着实现 `intent` 所需的所有具体内容。它的结构和含义，完全由通信双方根据约定的 `intent` 来理解。
### 聊天记录 (ChatLog) 
IEnumerable<ChatLogItem>，可能包含了和该次的信件有关的过往信件。

## :inbox_tray:回信 (reply)
由回执 (`receipt`) 、信件 (`letter`)、(`internal`) 和信件 (`journey`) 两部分组成。
### 回执 (receipt)
+ **replyAt**: **回信时间**。收件人回信的时间。
+ **postStatus**:**邮局状态标识**。描述送信的状态：
    + `recipient_not_found`：查无此人。地址或收信主体不存在。
    + `delivery_timeout`：投递超时。在约定时间内未找到收信人，邮差放弃寻找。
    + `delivery_success`：投递成功。信件已成功送达至收信人。
    + `recipient_rejected`：收件人拒收。信件送达，但收信人拒绝接收。
    + `recipient_silent`：收件人已读不回。信件已送达并被接收，但收信人选择不予以回复。
+ **postCode**:**邮局状态码**。和送信状态对应的编码，一般是数字。
### 信件 (letter)
这里装着实现 `intent` 所需的所有具体内容。它的结构和含义，完全由通信双方根据约定的 `intent` 来理解。一般来说包含以下字段：
+ **code**：**回复码**。一个预定义的代码，用于让发信人快速、明确地知晓处理结果（例如："success", "unauthorized", "insufficient_balance"）。
+ **message**：**简短消息**。一段简短的人类可读文本，用于补充说明处理情况（例如："登录成功", "余额不足"）。
+ **data**：**具体回复**。承载本次 `intent` 的完整响应数据，其结构由具体的 `intent` 约定。
### 笔记 (notes)
收信人为了处理发信人的 `intent` ，记录下来的每一个步骤以及结果，它的结构和含义有 `intent` 约定
### 聊天记录 (ChatLog) 
IEnumerable<ChatLogItem>，可能包含了和该次的信件有关的信件。通常是收信人为了解决发信人的 `intent` ,给其他 `Actor` 发出的信件。


***
其实 the-attic 的 [InstructionStruct.md] 才是真的