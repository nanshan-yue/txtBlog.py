# 1.set hostname

```
sudo vim /etc/hosts

127.0.0.1 yoursite.mac
```

# 2.set txtBlog.py/index.py
```
vim index.py

if __name__ == '__main__':
        app.debug = True # 设置调试模式，生产模式的时候要关掉debug
        #app.run(host="blog.hello.com",port=8000)
        app.run(host="127.0.0.1",port=yourport) #default, private
        #app.run(host="0.0.0.0",port=getConf("system", "port")) #public
```

# 3.start txtBlog.py
```
python index.py
```


# 4.open Apache's proxy reverse proxy module
```
find /Applications/XAMPP |grep -i httpd.conf
```
 open /Applications/XAMPP/xamppfiles/etc/httpd.conf
```
vim /Applications/XAMPP/xamppfiles/etc/httpd.conf
```
Find the following modules in the httpd.conf file and remove the # in front of the module
```
LoadModule proxy_http_module libexec/apache2/mod_proxy_http.so
LoadModule proxy_module libexec/apache2/mod_proxy.so
LoadModule ssl_module libexec/apache2/mod_ssl.so
```
# 5.modify httpd-vhosts.conf
```
sudo vim  /Applications/XAMPP/etc/extra/httpd-vhosts.conf
```
```
<VirtualHost *:80>
    ServerName yoursite.mac
    ProxyRequests Off
    ProxyPreserveHost On
    <Proxy />
        Order deny,allow
        Allow from all
    </Proxy>
    ProxyPass /  http://127.0.0.1:yourport/
    ProxyPassReverse /  http://127.0.0.1:yourport/
</VirtualHost>
```
> `http://127.0.0.1:yourport/` Must end with /, or my be error

## tips:
Every time you modify your httpd-vhosts.conf, you should stop and restart Apache in the terminal.
```
sudo /Applications/XAMPP/xamppfiles/xampp stopapache
sudo /Applications/XAMPP/xamppfiles/xampp startapache
```

# 6.Enable and test

Now you can point your browser to http://yoursite.mac/
else, you can point your browser to http://127.0.0.1:yourport/

- https://www.zhihu.com/question/438083442/answer/1675598574
- https://blog.csdn.net/greenqingqingws/article/details/79004577