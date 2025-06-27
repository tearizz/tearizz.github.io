# REST API in Go


<!--more-->
访问已有的对外开放的RestfulApi，包括GET，PUT，POST等方式，文件的内容以Json为主
## 1. 构建url
```go
u := url.URL{
    Scheme: "https",
    Host:   "localhost:8080",
}

uri := u.JoinPath("v1", "path1", "path2", "path3")

==> uri.String() ==> "https://localhost:8080/v1/path1/path2/path3"
```

## 2. 构建client
```go
client := &http.Client{
    Timeout: 100*time.Second,
    Transport: &http.Transport{
        TLSClientConfig: &tls.Config{
            InsecureSkipVerify: true
        }
    }
}
```
- &http.Client，&http.Transport，&tls.Config 这里注意都应该是指针 
- InsecureSkipVerify: true 的作用是跳过tls检查，与curl -k 相同

## 3. 封装请求数据
目前的应用中，数据都是以json的形式上传，故此处仅展示封装json数据

（1）传入结构体
    在数据需要用户手动传入时，可以使用将数据定义为结构体的方式，方便用户传入
```go
data := DataStruct{
    Filed1: xxxx,
    Filed2: xxxx,
    Filed3: xxxx,
} 

requestData := new(bytes.Buffer)
json.NewEncoder(requestData).Encode(data)
```
- requestData 用来承接json encode后的data


（2）传入map数据
```go
data := make(map[string]interface{})
data["field1"] = "f1Value"
data["field2"] = "f2Value"

requestData := new(bytes.Buffer)
json.NewEncoder(requestData).Encode(data)
```

## 4. 发送请求
在这里就定义请求“GET”、“PUT”或“POST” , 还要定义所需的请求头，最后发送请求
```go
request,err := http.NewRequest("GET",uri,nil)
request,err := http.NewRequest("POST",uri,requestData)
request,err := http.NewRequest("PUT",uri,requestData)

request.Header.Add("Content-Type","application/json")
request.Header.Add("Cookie","token="+token)

response,err := client.Do(request)
defer response.Body.Close()
```
- http.NewRequest 生成请求
- request.Header.Add(k, v) 添加请求头
- client.Do(request) 发送请求
## 5. 解析返回值
(1) 结构体承接返回值， 通过实现定义好的结构体承接全部的返回值
```go
body,err := io.ReadAll(response.Body)

var r Response
err = json.Unmarshal(body,&r)
```
- io.ReadAll 读取数据
- Response 结构体承接返回的数据
- json.Unmarshal(body, &r) 解析返回数据，需注意入参应为指针 &r


（2）使用map承接返回值，可以省略定义结构体，直接拿到想要的值
```go
body.err := io.ReadAll(response.Body)

reslt := make(map[string]interface{})
err = json.Unmarshal(body,&result)

_ = result["field1"].(string) // single json value
_ = result["field2"].(bool)
_ = result["field3"].(map[string]interface{})["field31"].(string) // multipl json nest
```


> 创作于 2024-09-01
> 
> 第一次修改于 2024-09-06


---

> 作者: tearizz  
> URL: http://localhost:1313/posts/319de9c/  

