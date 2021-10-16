

```

```

```java
@RequestMapping("test")
public ResponseEntity<String> handleRequest (RequestEntity<String> requestEntity) {
    System.out.println("request body : " + requestEntity.getBody());
    HttpHeaders headers = requestEntity.getHeaders();
    System.out.println("request headers : " + headers);
    HttpMethod method = requestEntity.getMethod();
    System.out.println("request method : " + method);
    System.out.println("request url: " + requestEntity.getUrl());

    ResponseEntity<String> responseEntity = new ResponseEntity<>("my response body",
            HttpStatus.OK);
    return responseEntity;
}
```

```java
 HttpClient client = new HttpClient();
        PostMethod post = new PostMethod("http://localhost:8080/test");
        NameValuePair message = new NameValuePair("json", "123");
        NameValuePair message1 = new NameValuePair("123", "123");
        post.setRequestBody(new NameValuePair[]{message, message1});
/*        JSONObject jsonObject = new JSONObject();
        jsonObject.put("url","http:xxx");
        String  toJson = jsonObject.toString();
        RequestEntity se = new StringRequestEntity(toJson ,"application/json" ,"UTF-8");
        post.setRequestEntity(se);*/
        client.executeMethod(post);
        String result = post.getResponseBodyAsString();
```

