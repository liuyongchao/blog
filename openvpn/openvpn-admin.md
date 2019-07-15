# openvpn配置

## opevpn解决可以链接服务端无法连接内网机器

```bash
#添加tun0指定openvpn服务器ip10.8.0.1
ip addr add dev tun0 10.1.0.1/24 broadcast 10.1.0.255
#添加一条SNAT规则，让源地址是10.8.0.0/24网段的地址进来转换成vpn服务器的内网地址(192.168.66.253)，这样vpn客户端访问公司内网服务器的时候伪装成vpn服务器去访问
iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -j SNAT --to-source 192.168.66.253 
service iptables save
service iptables start
chkconfig --level 35 iptables on
```