RouterOS脚本文件 rsc

ChinaRoute.rsc，0.5MB

WorldRoute.rsc，1.2MB

IPv4-CIDR.rsc，2.3MB

与上述值偏差过大，属异常。

2022/6/25起，只提供 IPv4-CIDR.rsc，覆盖整个IPv4约99.88%。


更新脚本

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
