# RestfulAPI


&lt;!--more--&gt;
访问已有的对外开放的RestfulApi，包括GET，PUT，POST等方式，文件的内容以Json为主
## 1. 构建url
```go
u := url.URL{
    Scheme: &#34;https&#34;,
    Host:   &#34;localhost:8080&#34;,
}

uri := u.JoinPath(&#34;v1&#34;, &#34;path1&#34;, &#34;path2&#34;, &#34;path3&#34;)

==&gt; uri.String() ==&gt; &#34;https://localhost:8080/v1/path1/path2/path3&#34;
```

## 2. 构建client
```go
client := &amp;http.Client{
    Timeout: 100*time.Second,
    Transport: &amp;http.Transport{
        TLSClientConfig: &amp;tls.Config{
            InsecureSkipVerify: true
        }
    }
}
```
- &amp;http.Client，&amp;http.Transport，&amp;tls.Config 这里注意都应该是指针 
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
data[&#34;field1&#34;] = &#34;f1Value&#34;
data[&#34;field2&#34;] = &#34;f2Value&#34;

requestData := new(bytes.Buffer)
json.NewEncoder(requestData).Encode(data)
```

## 4. 发送请求
在这里就定义请求“GET”、“PUT”或“POST” , 还要定义所需的请求头，最后发送请求
```go
request,err := http.NewRequest(&#34;GET&#34;,uri,nil)
request,err := http.NewRequest(&#34;POST&#34;,uri,requestData)
request,err := http.NewRequest(&#34;PUT&#34;,uri,requestData)

request.Header.Add(&#34;Content-Type&#34;,&#34;application/json&#34;)
request.Header.Add(&#34;Cookie&#34;,&#34;token=&#34;&#43;token)

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
err = json.Unmarshal(body,&amp;r)
```
- io.ReadAll 读取数据
- Response 结构体承接返回的数据
- json.Unmarshal(body, &amp;r) 解析返回数据，需注意入参应为指针 &amp;r


（2）使用map承接返回值，可以省略定义结构体，直接拿到想要的值
```go
body.err := io.ReadAll(response.Body)

reslt := make(map[string]interface{})
err = json.Unmarshal(body,&amp;result)

_ = result[&#34;field1&#34;].(string) // single json value
_ = result[&#34;field2&#34;].(bool)
_ = result[&#34;field3&#34;].(map[string]interface{})[&#34;field31&#34;].(string) // multipl json nest
```


&gt; 创作于 2024-09-01
&gt; 
&gt; 第一次修改于 2024-09-06


---

> 作者:   
> URL: http://localhost:1313/posts/319de9c/  

