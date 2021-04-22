# CloudMusic-LevelUp

> 网易云音乐刷歌升级脚本
>
> [项目 GitHub 地址](https://github.com/Secriy/CloudMusic-LevelUp)

## 脚本功能

1. 登录网易云音乐
2. 执行签到，并显示奖励的积分数值
3. 刷音乐播放量，返回具体数值
4. 使用 GitHub Actions 挂脚本

## 使用方式

### 安装依赖

```shell
pip install -r requirements.txt
```

### 执行脚本

脚本使用命令行参数输入变量，其中手机号和密码的 32 位 MD5 值为必填字段，其余均为可选字段。

```shell
# python action.py -h 查看usage
usage: action.py [-h] [-s [SCKEY [SCKEY ...]]] [-t [TG_BOT_TOKEN [TG_BOT_TOKEN ...]]] [-c [TG_CHAT_ID [TG_CHAT_ID ...]]] [-b [BARK_KEY [BARK_KEY ...]]] [-l [PLAYLIST [PLAYLIST ...]]] [-w [WW_ID [WW_ID ...]]] [-a [AGENT_ID [AGENT_ID ...]]]
                 [-e [APP_SECRETS [APP_SECRETS ...]]]
                 phone password

positional arguments:
  phone                 List of Phone Number.
  password              List of MD5 value of the password.

optional arguments:
  -h, --help            show this help message and exit
  -s [SCKEY [SCKEY ...]]
                        The SCKEY of the Server Chan.
  -t [TG_BOT_TOKEN [TG_BOT_TOKEN ...]]
                        The token of your telegram bot.
  -c [TG_CHAT_ID [TG_CHAT_ID ...]]
                        The chat ID of your telegram account.
  -b [BARK_KEY [BARK_KEY ...]]
                        The key of your bark app.
  -l [PLAYLIST [PLAYLIST ...]]
                        Your playlist.
  -w [WW_ID [WW_ID ...]]
                        Your Wecom ID.
  -a [AGENT_ID [AGENT_ID ...]]
                        Your Wecom App-AgentID.
  -e [APP_SECRETS [APP_SECRETS ...]]
                        Your Wecom App-Secrets.
```

密码的 MD5 值计算可以在[MD5 在线加密](https://md5jiami.51240.com/)上进行，取 32 位小写值

![](README/image-20200829112617823.png)

执行结果：

![](README/image-20200830161354842.png)

因为我今天已经刷满了，所以就不累计听歌量了。

### 自定义歌单

鉴于网易云每天推荐的歌单不太够，就增加了自定义歌单的功能，也是使用参数的形式，支持多个歌单输入，例如：

```shell
# 必须添加-l指定参数
python action.py [手机号列表] [32位MD5密码加密值列表] -l 5173689994 4901511925
```

### Server 酱 Turbo 版微信推送

使用 Server 酱 Turbo 版可以绑定微信，将脚本每次的运行结果推送到你的微信上。

使用方法：

1. 访问[Server 酱 Turbo 版官网](https://sct.ftqq.com/)，点击**登入**，使用微信扫码登录

2. 登入成功后，按照网站上的说明选择消息通道，如**方糖服务号**（于 2021 年 4 月停止服务）

3. 点击**SendKey**，找到自己的 SendKey，并复制

4. 执行脚本时带参数`-s`指定 SendKey

   用例：

   ```shell
   python action.py [手机号1],[手机号2] [32位MD5密码加密值1],[32位MD5密码加密值2] -s [SendKey]
   ```

   示例：

   ```shell
   python action.py 10000000000,20000000000 4******************************1,3******************************2 -s SSS111111T111112f3e421 -l 2133132 2311315 2434234
   ```

   ![](README/image-20201113151600263.png)

### Telegram Bot 推送

使用 Telegram 机器人按时推送脚本执行结果。

使用方法：

1. 创建 Telegram 机器人并获取机器人 Token 以及个人账户的 Chat ID

2. 执行脚本时指定参数`-t`和`-c`，分别对应 Token 和 Chat ID

### Bark 推送

使用 Bark App 实现推送（建议 iOS/iPadOS 用户使用）

使用方法：

1. 安装 Bark 移动端程序

2. 复制应用内的示例 URL 并截取其中的 22 位随机字符串

3. 执行脚本时指定参数`-b`，后接上述 22 位字符串

### 企业微信推送

使用方法:

1. 配置企业微信，获取企业 ID、应用 ID、应用 Secret

2. 执行脚本时指定参数` -w`、`-a`、`-e `，分别对应企业 ID、应用 ID 和应用 Secret

## GitHub Actions 部署

### 1. Fork 该仓库

### 2. 创建 Secrets

- 创建 PHONE，填入手机号列表,以`,`分割（必填）

- 创建 PASSWORD，填入 32 位 MD5 密码加密值列表，填写此项可不填 NOMD5_PASSWORD 项（与 NOMD5_PASSWORD 二选其一）

- 创建 NOMD5_PASSWORD，填入密码，会自动转换密码为 MD5 值，填写此项可不填 PASSWORD 项（与 PASSWORD 二选其一）

- 创建 PLAYLIST，填入 歌单 ID，多个歌单 ID 用空格分隔（可选）

- 创建 SCKEY（Server 酱 SendKey，可选）

- 创建 TG_BOT_TOKEN（Telegram 机器人 Token，可选）

- 创建 TG_CHAT_ID（Telegram 账号 Chat ID，可选）

- 创建 BARK_KEY（Bark 设备密钥，可选）

- 创建 WW_ID （企业微信 ID，可选）

- 创建 AGENT_ID （企业微信 App-AgentID，可选）

- 创建 APP_SECRETS （企业微信 App-Secrets，可选）

![](README/image-20201110002853759.png)

### 3. 启用 Action

点击 Actions，选择 **I understand my workflows, go ahead and enable them**

真的是过了很久之后才发现**由于 GitHub Actions 的限制，直接 fork 来的仓库不会自动执行！！！**

必须手动修改项目提交上去，最简单的方法就是修改下图的 README.md 文件（右侧有网页端编辑按钮）。

![image-20201022185210937](README/image-20201022185210937.png)

随便修改什么都行，修改完 commit 就可以了。

之后**每天 0 点**会自动执行一次脚本

![](README/image-20200829120815423.png)

![](README/image-20200829120847583.png)

### 4. 手动执行

GitHub 现在有了手动执行的功能，点击下图 Run workflow 即可。

![](README/image-20201022192517489.png)

### 5. 多次执行（可选）

如果觉得每天刷的听歌量达不到要求，可以尝试每天多次执行的解决方案，修改 _.github/workflows/action.yml_ 内的 _cron_ 值为 **"0 4/16 \* \* \*"** ，即在每天的 0 点和 12 点执行。

## 注意事项

- 手机号列表和密码列表信息必须按顺序一一对应

- 网易云音乐限制每天最多计算 300 首

- 必须手动修改内容，不然不会自动执行！

- 为了方便他人学习研究，脚本保留了网易云音乐完整的表单加密算法

- Server 酱的应用场景取决于个人，请跟据自己的需求选择消息通道并进行配置，如在使用和配置方面有疑问，可以提出 Issue 或直接联系 Server Chan 管理员
