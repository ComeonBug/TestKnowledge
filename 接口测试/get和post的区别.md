一般问这个问题是指http请求中get和post请求的区别

通过curl命令来模拟这俩请求，得出的结论是：

1）最明显就是请求头里的request-method不同

2）post请求可以加body，而body里可以放form-data、json、xml等各种数据类型的数据,返回的response header里有Content-Length和content-type这2个字段

3）行业规范，比如restful api中建议：在状态变化获取资源的请求用get，而对资源进行写入、更新操作时用post请求

而很多人说的get的请求参数在param里通过&连接的说法不准确，因为post也是可以在url里写param参数的，最常见的是我们现在都用的企微的接口，企微的接口中post接口中都在url后面拼接里token的参数。还有一点说get请求有最大长度而post没有的说法也是不准确的，get的请求URL长度其实是受限于浏览器的问题，是浏览器url的长度限制，如果是在是用curl命令的话可以看到get也可以发送超过1024个字节的url

`curl -X GET "https://httpbin.org/get" -H "accept: application/json"`

`curl -X POST "https://httpbin.org/redirect-to" -H "accept: text/html" -H "Content-Type: application/x-www-form-urlencoded" -d "url=url1&status_code=200"`