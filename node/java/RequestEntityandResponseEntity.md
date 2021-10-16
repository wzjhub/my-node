

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