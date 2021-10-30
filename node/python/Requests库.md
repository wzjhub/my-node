

github:https://github.com/wistbean/learn_python3_spider

## 导入 requests 模块

```python
import requests
```

## 一行代码 Get 请求

```python
r = requests.get('https://api.github.com/events')
```

## 一行代码 Post 请求

```python
r = requests.post('https://httpbin.org/post', data = {'key':'value'})
```

## 想要携带请求参数

```python
payload = {'key1': 'value1', 'key2': 'value2'}

r = requests.get('https://httpbin.org/get', params=payload)
```

## 假装自己是浏览器

```python
url = 'https://api.github.com/some/endpoint'

headers = {'user-agent': 'my-app/0.0.1'}

r = requests.get(url, headers=headers)
```

## 获取服务器响应文本内容

```python
import requests

r = requests.get('https://api.github.com/events')

r.text
```

## Post请求一个键里面添加多个值

```python
payload_tuples = [('key1', 'value1'), ('key1', 'value2')]

r1 = requests.post('https://httpbin.org/post', data=payload_tuples)

payload_dict = {'key1': ['value1', 'value2']}

r2 = requests.post('https://httpbin.org/post', data=payload_dict)

print(r1.text)

#{  ...  "form": {    "key1": [      "value1",      "value2"    ]  },  ...}

r1.text == r2.text

#True
```

## 想上传文件？

```python
url = 'https://httpbin.org/post'

files = {'file': open('report.xls', 'rb')}

r = requests.post(url, files=files)

r.text
```























