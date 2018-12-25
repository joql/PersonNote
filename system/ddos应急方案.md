1. dns多线路解析，例：肉鸡多为国外，国外线路可解析到别的ip，基本可以抵挡大部分ddos

2. cdn，防止真是ip泄露

3. 允许单ip的最大连接数为30

   ```
   
   ```

4. [nginx 防御cc](http://www.cnblogs.com/wpjamer/p/9030259.html)

   不建议使用，国内ip大量内网话，很容易出现多个用户共享ip

   ```
   http {
       limit_req_zone $binary_remote_addr zone=one:10m rate=1r/s;
       server {
           #限制每ip每秒不超过20个请求，漏桶数burst为5
           #brust的意思就是，如果第1秒、2,3,4秒请求为19个，
           #第5秒的请求为25个是被允许的。
           #但是如果你第1秒就25个请求，第2秒超过20的请求返回503错误。
           #nodelay，如果不设置该选项，严格使用平均速率限制请求数，
           #第1秒25个请求时，5个请求放到第2秒执行，
           #设置nodelay，25个请求将在第1秒执行。
           limit_req   zone=one  burst=1 nodelay;
       }
   }
   ```

5. 