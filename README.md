
# Zeep Sport Robot - 一个基于 Zeep 平台的开源全自动刷步程序

---

***Zeep Sport Robot*** 是一个能够通过数据包来修改 Zeep 平台运动步数的程序，它与其他程序不同的就是它可通过 GitHub Actions 实现定时任务在指定的时间点自动运行以进行运动步数的修改，从而解放四肢， 足不出户即可拥有异于常人的运动步数，除了能够刷取 Zeep 平台的步数以外，还可对能够接入到 Zeep 平台的第三方平台的运动步数进行修改，如：微信、支付宝等

---

## 声明

本程序***仅供学习交流***使用，本人对程序的使用与传播***无任何掌控能力***，由于使用本程序所导致的一切后果***由使用者自行承担***！！！此程序***不会记录您的任何信息，也不会盗取您的账号信息***，所有的代码都是开放的，您可以对此代码进行分析以查验此程序是否会盗取您的账号信息

---

## 原理

此程序通过模拟设备端发送的步数更新数据包，使得 Zeep 平台的当日运动步数被修改，并且同步这个步数到已接入的第三方平台上，但是这些操作是不可逆的 (其对于运动步数的修改也只能向增大的方向修改，而不能向减小的方向修改)

## 使用

### 准备工作

#### 注册 Zeep / Zeep Life 账号

注册一个 Zeep / Zeep Life 账号，必须绑定一个手机号或邮箱并且设定密码，所以建议注册时使用邮箱注册

#### 接入第三方平台

在 Zeep / Zeep Life APP里操作

### 配置程序运行数据 (必选步骤)

**以下操作最好不要在打开浏览器的翻译时完成，便于按照原来的选项英文名称找到正确的选项**

 1. 在自己的 GitHub 账号上新建一个私人存储库(安全性较高)，然后把这个仓库里的所有文件全部复制到刚刚创建的存储库里
 2. 然后在顶部横栏中点击 "Settings"(译名"设置") 选项进入这个存储库的设置页面，再在侧边栏(左)中找到 "Security"(译名"安全") 的分类中的 "Secrets and variables"(译名"密钥与变量") 选项卡，展开此选项卡后点击 "Actions"(译名"活动") 选项
 3. 此时在右侧页面点击 "Secrets"(译名"密钥") 并在下方的 "Repository secrets"(译名"存储库密钥") 部分点击 "New repository secret"(译名"新建存储库密钥") 按钮进入存储库密钥创建界面
 4. 在 "Name"(译名"名称") 部分的输入框输入 "ACCOUNT"(不用输引号) ，然后在 "Secret"(译名"密钥") 部分的输入框输入你的 Zeep 账号(绑定的手机号或邮箱)，然后点击下方的 "Add Secret"(译名"添加密钥") 按钮
 5. 随后会回到刚刚的界面，但 "Repository secrets"(译名"存储库密钥") 部分的列表中会多出刚刚提交的名为 "ACCOUNT" 的项目
 6. 然后什么都别动，再次点击 "New repository secrets"(译名"新建存储库密钥") 按钮进入存储库密钥创建界面
 7. 在 "Name"(译名"名称") 部分的输入框输入 "PASSWORD"(不用输引号) ，然后在 "Secret"(译名"密钥") 部分的输入框输入你的 Zeep 账号使用的密码，然后点击下方的 "Add Secret"(译名"添加密钥") 按钮
 8. 随后又会回到刚刚的页面，但 "Repository secrets"(译名"存储库密钥") 部分的列表中又会多出名为 "PASSWORD" 的项目，此时就已经成功的将程序的运行所需数据全部添加进去了

### 自定义设置 (可选步骤)

这个程序包含了许多可以自定义的功能，下面简要做一些修改的说明 (若没有专业知识，就不建议修改设置了，因为修改设置可能会导致的问题如果没有这方面的知识可能会导致程序直接出错，而 GitHub 会在 GitHub Actions 出错的时候给你发邮件，要是改错可能就变成邮件轰炸机了)

#### 1. 刷取步数及其模式的更改

可在 `main.py` 文件的第 108、109 行将随机步数的最小值 `25000` 与最大值 `55000` 替换为需要设置的指定步数范围 (默认在 25000 ~ 55000 之间随机刷取步数)

若不希望随机刷取步数，可将 main.py 的第 111 行中的 `None` 替换为需要设置的指定步数值 (不建议超过98800，这是微信运动的上限值，如果超出可能会被限制使用微信运动)

#### 2. 程序的自动运行时间

可在 `.github/workflows/main.yml` 文件的第 6 行将 `- cron: '0 4 * * *'` 修改为其它的 corn 时间表达式来修改程序运行的时间 (默认设定在每天的 12:00 运行)

下面是 GitHub Actions 的 cron 表达式的编写格式和编写方法：

格式：````- cron:  '分 时 日 月 星期'````

各字段的设定：
|字段|取值范围|描述|
|--|--|--|
|分 (min)|0 ≤ min ≤ 59|一个小时中的第 min 分钟|
|时 (hour)|0 ≤ hour ≤ 23|一天中的第 hour 小时|
|日 (DOM)|1 ≤ DOM ≤ 31|一个月中的第 DOM 天|
|月 (month)|1 ≤ month ≤ 12|一年中的第 month 月|
|星期 (DOW)|0 ≤ DOW ≤ 6|一周中的第 DOW-1 天 ( 以星期日为一周中的第1天，此时 DOW=0 )|

可以在这五个字段中的任意字段中使用运算符：

|符号|名称|描述|表达式|运行效果|
|--|--|--|--|--|
|*|任意值|无论此字段的值为何，都视为满足条件|a * * b *|在每年的第 b 月的每一天的每一个小时的第 a 分钟运行|
|,|数值列表分隔符|此字段的值只要是列出的任意值之一，就视为满足条件|a,b,c d,e * * *|在每一天的第 a、b、c 小时的第 d、e 分中运行|
|-|区间|此字段的值只要在指定范围内，就视为满足条件|a 4-8 * * *|在每一天的第 4、5、6、7、8 小时的第 a 分钟运行|
|/|步进|此字段的值从指定起始值开始，只要满足步长规则，就视为满足条件|20/15 * * * *|在每一天的每一个小时里，从这一个小时的第 20 分钟开始到这一个小时结束的时间里，每隔 15 分钟运行一次(即在第 20、35、50 分钟运行)|

> 注意:
> -   以上时间均为UTC标准时间，不是北京时间
> -   只要当前的UTC时间符合以上条件，GitHub Actions 就会运行
> -   自动运行的每两次 GitHub Actions 之间至少要有5分钟的间隔，如果设定了每1分钟运行一次，则实际应为每5分钟运行一次

官方文档：[GitHub Actions 定时触发 - corn 表达式 ( GitHub Docs  )](https://docs.github.com/cn/enterprise-server@2.22/actions/learn-github-actions/events-that-trigger-workflows#scheduled-events)

#### 3.多账户同时刷取 (专业性)

**注意：此选项涉及的修改可能需要具备专业知识的人进行修改，否则可能会导致稳定性或安全性问题**

在 `main.py` 第 110 行处的 `account` 列表内可填入多个账户，但配置的其它账户应如单个账户的配置操作一样，将账户密码信息保存在 GitHub Repository Secrets 中，配置后再对 `.github/workflows/main.yml` 文件与 `main.py` 文件进行对应的修改

不同的账户可使用不同的步数选取方案(如：A账户使用随机刷取方案，但B账户使用定值刷取方案)，但目前不同账户的随机步数仅可使用同一个随机数取值范围 (定值刷取方案可使用不同值)

---

对于代码的修改并不局限于以上所提及的范围，更多玩法大家可以自行对代码进行分析从而实现

本程序与本文档目前还有许多不足之处，后续会慢慢完善，请各位诸多包容，谢谢！
