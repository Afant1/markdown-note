# curl命令总结
## 使用socks代理

    curl --socks5 202.113.41.443 http://www.baidu.com

## curl处理cookie
`接收cookie,将cookie存储到文件cookies`

    curl -c /tmp/cookies http://www.baidu.com

`发送cookie`

    curl -b "key1=value1;key2=value2;" http://www.baidu.com
## curl发送数据
`get方式`

    curl -G -d "key1=value1;key2=value2" http://www.baidu.com

`post方式`

    curl -d "key1=value1;key2=value2" http://www.baidu.com
`提交表单`

    curl -F file=@/tmp/me.txt http://www.baidu.com
## 设置请求头
`设置ua`

    curl -A "Mozilla/5.0 Firefox/21.0" http://www.baidu.com
`设置referer`

    curl -e "http://google.com" http://baidu.com
`设置请求头`

    curl -H "Connection:keep-alve \n User-Agent:Mozilla/5.0 Firefox/21.0" http://www.baidu.com
`只返回请求头部`

    curl -I http://www.baidu.com
` 保存响应头到指定文件`

    curl -o /tmp/response http://www.baidu.com
## 用户名、密码认证

    curl -u user:pass http://www.baidu.com