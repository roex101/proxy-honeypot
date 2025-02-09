# 关于
📮Roex101@126.com
Admin：@Roex101

个人博客：https://blog.0102019.xyz/

名下所有的频道

❶主频道：[t.me/Roex10](https://t.me/Roex10)
❷HTTP/HTTPS代理蜜罐监测：[t.me/Roex101_Proxy_honeypot](https://t.me/Roex101_Proxy_honeypot)

不做任何违法行为，仅个人学习
持续更新中…
# proxy-honeypot部署教程
一、环境准备

Debian11+ / Ubuntu 20.04+ 系统
```
sudo apt update && sudo apt upgrade -y
sudo apt install -y golang certbot
```
二、SSL证书申请
 申请证书（需先配置域名DNS解析）
```sudo certbot certonly --standalone -d yourdomain.com```

 复制证书到标准位置
```
sudo mkdir -p /etc/ssl/{certs,private}
sudo cp /etc/letsencrypt/live/yourdomain.com/fullchain.pem /etc/ssl/certs/
sudo cp /etc/letsencrypt/live/yourdomain.com/privkey.pem /etc/ssl/private/
```

 设置权限
```
sudo chmod 600 /etc/ssl/private/privkey.pem
```
三、Telegram配置
通过 @BotFather 创建机器人并获取 TG_BOT_TOKEN

创建频道并获取 TG_CHANNEL_ID（需通过 getUpdates API 获取）
四、编译部署
 1. 保存代码为 honeypot.go
 2. 设置环境变量
```
export TG_BOT_TOKEN="你的机器人Token"
export TG_CHANNEL_ID="你的频道ID"
```
 3. 编译程序
```
go build -o honeypot honeypot.go
```
 4. 设置特权端口绑定
```
sudo setcap 'cap_net_bind_service=+ep' ./honeypot
```
 5. 启动服务
```
./honeypot
```
五、验证测试

 HTTP代理测试
```curl -x http://yourdomain.com:80 http://httpbin.org/ip```

 HTTPS代理测试
```curl -x http://yourdomain.com:80 https://httpbin.org/ip```

 查看日志
```tail -f /var/log/honeypot.log```
六、系统服务配置「可选」
/etc/systemd/system/honeypot.service
```
[Unit]
Description=Advanced Proxy Honeypot
After=network.target

[Service]
Environment="TG_BOT_TOKEN=your_token"
Environment="TG_CHANNEL_ID=your_channel"
ExecStart=/path/to/honeypot
Restart=always
User=www-data

[Install]
WantedBy=multi-user.target
```
#功能特性
完整代理支持：

HTTP (CONNECT/GET/POST)

HTTPS 隧道代理

请求头完整转发

Keep-Alive 连接

监控功能：

实时Telegram通知

本地日志记录

客户端IP追踪

反检测机制：

合法SSL证书

随机响应延迟

混合端口监听

服务伪装（Nginx Server头）
