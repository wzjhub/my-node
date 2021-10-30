

```python
import urllib.request
url = "https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fpan.iqiyi.com%2Fext%2Fpaopao%2F%3Ftoken%3DeJxjYGBgmGrnfZoBDEyZARQ0AiI.jpg&refer=http%3A%2F%2Fpan.iqiyi.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1636981386&t=1324a9cd7ee71ddf1c7adb03f0051d77"
urllib.request.urlretrieve(url, 'lisa.jpg')
```

