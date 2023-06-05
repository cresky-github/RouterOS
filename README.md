
### 折腾 fake-ip 分流，以下内容已经变得无意义。



RouterOS脚本文件 rsc
- ChinaRoute.rsc，0.5MB
- WorldRoute.rsc，1.2MB
- IPv4-CIDR.rsc，2.3MB

与上述值偏差过大，属异常。

2022/06/25起，只提供 IPv4-CIDR.rsc，覆盖整个IPv4约99.88%。

2022/09/11起，IPv4-Route.rsc 把WorldRoute写入到路由表。导入前，根据实际情况修改 网关和路由表名。

2022/10/31起，在主PPPoE的拨号脚本加入以下代码：
```
:global "BYPASS.GATEWAY" 10.0.255.254
:global "BYPASS.DISTANCE" 64
:global "BYPASS.TABLE" "World"
```
后根据个人实际情况修改。也可直接修改脚本。
IPv4-Route.rsc会读取，并使用。

![image](https://user-images.githubusercontent.com/52242290/198998946-0e848561-83f9-43e6-8320-3f9ce7b3c96d.png)

```
/system logging disable 0
/system logging disable 5
/import file=IPv4-CIDR.rsc
/import file=IPv4-Route.rsc
/system logging enable 0
/system logging enable 5
```

## 更新脚本
```
:log info "start download IPv4-CIDR.rsc ..."
/tool fetch http-method=get url=https://raw.githubusercontent.com/cresky-github/RouterOS/main/IPv4-CIDR.rsc
:log info "IPv4-CIDR.rsc downloaded."
/file {
  :local addrFile	
  :local fileSize	
  :set addrFile [find where name="IPv4-CIDR.rsc"]	
  :set fileSize [get $addrFile size]	
  :if ($fileSize > 2000000) do={	
    /import file=IPv4-CIDR.rsc		
    :log info "IPv4-CIDR updated!"		
  }	
}
```
