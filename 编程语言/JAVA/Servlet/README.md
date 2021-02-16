## SERVLET 笔记



### 允许跨域请求



```java
/* 允许跨域的主机地址 */
response.setHeader("Access-Control-Allow-Origin", "*");  
/* 允许跨域的请求方法GET, POST, HEAD 等 */
response.setHeader("Access-Control-Allow-Methods", "*");  
/* 重新预检验跨域的缓存时间 (s) */
response.setHeader("Access-Control-Max-Age", "3600");  
/* 允许跨域的请求头 */
response.setHeader("Access-Control-Allow-Headers", "*");  
/* 是否携带cookie */
response.setHeader("Access-Control-Allow-Credentials", "true");  
```

