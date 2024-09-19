# QPS Limitation - Guidance Plan

Guidance Plan—Used to guide an organization on how to achieve the best QPS for a service within a reasonable time;

Table of Contents

- [QPS Limitation - Guidance Plan](#qps-limitation-guidance-plan)
	- [1. Document Description](#1-document-description)
		- [Background Information](#background-information)
	- [2. Guidance Plan](#2-guidance-plan)
		- [QPS Limitation Description](#qps-limitation-description)
			- [Cluster Limitation](#cluster-limitation)
	- [3. Demo](#3-demo)

## 1. Document Description

Used to guide an organization on traffic control for a service

### Background Information

Document Background: The main interface limits the request service traffic. When the request traffic exceeds the maximum QPS, the request fails and returns 1901.

Pain Point Analysis: The main interface can only handle evenly distributed QPS.

## 2. Guidance Plan

### QPS Limitation Description

Introduction: A company's requests evenly distributed within one second can reach the QPS peak, or within 10ms less than MAXQPS * 0.3, then it can pass;
For example, a company's maximum QPS is 1000;

| Time/ms | QPS         | Can Request Pass | Remarks          |
| ------- | ----------- | ---------------- | ---------------- |
| 10      | QPS <= 300  | 1100             | 1100: Passed     |
| 1000    | QPS <= 1000 | 1100             |                  |
| 10      | QPS > 300   | 1901             | 1901: Request Exceeded |
| 1000    | QPS > 1000  | 1901             |                  |

Note: Time represents the span, 10 represents within 10ms;

#### Cluster Limitation

Introduction: Besides company limitations, Shumei also imposes corresponding limitations on clusters in various regions.
Note: Generally, the cluster QPS upper limit is high, so there is no need to worry about the organization's company QPS being affected by the cluster QPS.

## 3. Demo

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

// Historical data QPS limitation demo, real-time data is directly forwarded to Shumei for QPS limitation
// Evenly distributed within one second
func main(){
	// Text service URL and required parameters
	url := "http://api-text-bj.fengkongcloud.com/text/v4"
	accessKey := "{ACCESS_KEY}"
	text := "{TEXT}"
	uid := "{UID}"

	// Historical data, only two entries are shown here
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
		// Wait 100 milliseconds for every MaxQPS/10 sent
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