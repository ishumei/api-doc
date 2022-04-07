# QPS限制-指导方案

指导方案—用于指导一个组织对于一个服务的QPS如何在合理的时间内达到最佳；

目录

- [QPS限制-指导方案](#qps限制-指导方案)
	- [一、文档说明](#一文档说明)
		- [**背景资料**](#背景资料)
	- [二、指导方案](#二指导方案)
		- [QPS限制说明](#qps限制说明)
			- [集群限制](#集群限制)
	- [三、demo](#三demo)

## 一、文档说明

用于指导一个组织对于一个服务的流量控制

### **背景资料**

文档背景：主接口对请求服务进行流量限制，当请求流量超出最大QPS时请求失败，返回1901。

痛点分析：主接口只能处理均匀分布的QPS。

## 二、指导方案

### QPS限制说明

介绍：一个公司的请求在一秒内均匀分布则能够达到QPS峰值，或在10ms内小于 MAXQPS * 0.3 ,则能通过；
例如一个公司的最大QPS为 1000;

| 时间/ms | QPS         | 请求能否通过 | 备注             |
| ------- | ----------- | ------------ | ---------------- |
| 10      | QPS <= 300  | 1100         | 1100：成功通过   |
| 1000    | QPS <= 1000 | 1100         |                  |
| 10      | QPS > 300   | 1901         | 1901：请求数超限 |
| 1000    | QPS > 1000  | 1901         |                  |

注：时间表示跨度，10 代表 10ms内；

#### 集群限制

介绍：除去公司限制以外，数美对各地区集群也做了相应限制
注意：一般来说集群QPS上限高，无需担心因集群QPS而导致组织公司QPS受影响。

##  三、demo

```go
package main

import (
	"bytes"
	"encoding/json"
	"fmt"
	"io/ioutil"
	"net/http"
	"sync"
	"time"
)

var (
	MaxQPS = {MaxQPS}
	AverageQPS = MaxQPS / 10
)

// 历史数据QPS限制demo，实时数据直接转发至数美由数美提供QPS限制
// 一秒内均匀打进
func main(){
	// 文本服务url与必带参数
	url := "http://api-text-bj.fengkongcloud.com/text/v4"
	accessKey := "{ACCESS_KEY}"
	text := "{TEXT}"
	uid := "{UID}"

	// 历史数据,此处只显示两条
	payload01 := map[string]interface{}{
		"accessKey": accessKey,
		"appId":     "default",
		"eventId":   "default",
		"type":      "ALL",
		"data": map[string]string{
			"text":    text,
			"tokenId": uid,
		},
	}
	payload02 := map[string]interface{}{
		"accessKey": accessKey,
		"appId":     "default",
		"eventId":   "default",
		"type":      "ALL",
		"data": map[string]string{
			"text":    text,
			"tokenId": uid,
		},
	}
	contents := struct{
		payloads []map[string]interface{}
	}{}
	contents.payloads = append(contents.payloads, payload01)
	contents.payloads = append(contents.payloads, payload02)

	var count int
	wg := &sync.WaitGroup{}
	ts := time.Now().UnixNano() / (1000 * 1000)
	for i := 0;i < len(contents.payloads);i++ {
		b,_ := json.Marshal(contents.payloads[i])
		wg.Add(1)
		go func(url string, data []byte,group *sync.WaitGroup) {
			doPost(url,b)
			group.Done()
		}(url,b,wg)
		count++
		now := time.Now().UnixNano() / (1000 * 1000)
		// 每发送 MaxQPS/10 等待100毫秒
		if count>=AverageQPS{
            count = 0
			time.Sleep(time.Millisecond * 100)
		}
	}
	wg.Wait()
}

func doPost(url string, content []byte){
	resp, _ := http.Post(url,"application/json", bytes.NewBuffer(content))
	var data map[string]interface{}
	if resp != nil {
		respBytes, _ := ioutil.ReadAll(resp.Body)
		_ = json.Unmarshal(respBytes,&data)
		fmt.Println(data)
	}
}
```

