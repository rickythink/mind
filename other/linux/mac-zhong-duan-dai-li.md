# Mac终端代理

```bash
$ vim ~/.zshrc
添加配置，设置/清除代理
alias proxy_on='export ALL_PROXY=socks5://127.0.0.1:1080'
alias proxy_off='unset ALL_PROXY'
生效
$ source ~/.zshrc
检查
$ curl -i http://ip.cn 或者
$ curl -i cip.cc
```
