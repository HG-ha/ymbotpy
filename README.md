ymbotpy
=======

**ymbotpy** 是基于 机器人开放平台API <https://bot.q.qq.com/wiki/develop/api/> 实现的机器人框架，目的提供一个易使用、开发效率高的开发框架，同时支持Webhook和WebSocket，非Tencent官方SDK，但用法完全相同。


特性
----

- 兼容 Python 3.7+
- 简单、易用、快速集成，过滤垃圾消息以及消息鉴权
- 同时支持Webhook和WebSocket，可无缝替换websocket版本qq-botpy

安装
-----

``` bash
pip install ymbotpy
```
``注意: Webhook由于通信方式的变化, 不再需要在本地注册监听事件，只需要编写具体的事件代码即可，事件开关在平台控制``

Webhook 快速上手
--------
1.  频道内@自动回复
    ``` python
    import ymbotpy
    from ymbotpy.message import Message

    class MyClient(ymbotpy.WebHookClient):
        async def on_at_message_create(self, message: Message):
            """频道内@"""
            await message.reply(content=f"机器人{self.robot.name}收到你的@消息了: {message.content}")

    if __name__ == "__main__":
        client = MyClient()
        # 启用沙盒模式
        # client = MyClient(is_sandbox=True)
        client.run(appid='botid',secret='secert',port=8080,system_log=True)
    ```

2.  在开发平台应配置的回调地址为：example.com/qbot/webhook，您可以通过以下方式来修改回调URL自定义webhook的url
    ``` python
    client.run(appid=BOT_APPID,secret=BOT_SECRET,port=8080,system_log=True,hook_route='/your_url')
    ```

3.  配置SSL以直接实现域名直接访问

    ```python
    client.run(appid=BOT_APPID,secret=BOT_SECRET,port=443,system_log=True,
        ssl_keyfile='ssl/private.key',ssl_certfile='ssl/public.crt')
    ```

WebSocket 快速上手（更多用法请参考官方文档）
--------
1.  频道内@自动回复
    ``` python
    import ymbotpy
    from ymbotpy.message import Message

    class MyClient(ymbotpy.Client):
        async def on_at_message_create(self, message: Message):
            """频道内@"""
            await message.reply(content=f"机器人{self.robot.name}收到你的@消息了: {message.content}")

    if __name__ == "__main__":
        intents = ymbotpy.Intents(public_guild_messages=True) 
        client = MyClient(intents=intents)
        # 启用沙盒模式
        # client = MyClient(intents=intents, is_sandbox=True)
        client.run(appid='botid', secret='secert')
    ```

更改日志存储位置
--------
-  将日志存放到当前路径下的logs目录中，确保代码添加在main.py主程序顶部
    ``` python
    import os
    from ymbotpy.logging import configure_logging, DEFAULT_FILE_HANDLER
    # 创建自定义handler配置，指定文件夹路径
    custom_handler = DEFAULT_FILE_HANDLER.copy()
    # 确保logs目录存在
    if not os.path.exists("logs"):
        os.makedirs("logs")
    custom_handler["filename"] = os.path.join("logs", "%(name)s.log")
    
    # 配置日志
    configure_logging(ext_handlers=custom_handler)

    # 其他代码
    import ymbotpy
    from ymbotpy.message import Message

    class MyClient(ymbotpy.WebHookClient):
        async def on_at_message_create(self, message: Message):
            """频道内@"""
            await message.reply(content=f"机器人{self.robot.name}收到你的@消息了: {message.content}")

    if __name__ == "__main__":
        client = MyClient()
        client.run(appid='botid',secret='secert',port=8080,system_log=True)
    
    ```

文档
----

有关此 SDK 更多用法，请查阅qq-botpy <https://github.com/tencent-connect/botpy>

### 欢迎访问镜芯科技
1.  天狼星框架：<https://www.siriusbot.cn/>
2.  镜芯API：<https://api2.wer.plus/>
3.  林枫云_站长首选云服务器：<https://www.dkdun.cn/>
