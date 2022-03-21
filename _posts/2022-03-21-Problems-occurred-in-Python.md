---
layout:     post
title:      Problem occurred in Python
subtitle:   Python
date:       2022-03-21
author:     UPOJZSB
header-img: img/post-bg-random.jpg
catalog: true
tags:
    - Problem
    - Python
    - Python3
    - requests
---

# requests

## ValueError: check_hostname requires server_hostname

```python
Traceback (most recent call last):
  File "D:/Python_Proj/anti-touch-fish/main.py", line 21, in <module>
    main()
  File "D:/Python_Proj/anti-touch-fish/main.py", line 17, in main
    r = requests.get(news_url, proxies=proxy)
  File "C:\Users\lzhao\AppData\Roaming\Python\Python38\site-packages\requests\api.py", line 76, in get
    return request('get', url, params=params, **kwargs)
  File "C:\Users\lzhao\AppData\Roaming\Python\Python38\site-packages\requests\api.py", line 61, in request
    return session.request(method=method, url=url, **kwargs)
  File "C:\Users\lzhao\AppData\Roaming\Python\Python38\site-packages\requests\sessions.py", line 542, in request
    resp = self.send(prep, **send_kwargs)
  File "C:\Users\lzhao\AppData\Roaming\Python\Python38\site-packages\requests\sessions.py", line 655, in send
    r = adapter.send(request, **kwargs)
  File "C:\Users\lzhao\AppData\Roaming\Python\Python38\site-packages\requests\adapters.py", line 439, in send
    resp = conn.urlopen(
  File "D:\Program Files\Python38\lib\site-packages\urllib3\connectionpool.py", line 696, in urlopen
    self._prepare_proxy(conn)
  File "D:\Program Files\Python38\lib\site-packages\urllib3\connectionpool.py", line 964, in _prepare_proxy
    conn.connect()
  File "D:\Program Files\Python38\lib\site-packages\urllib3\connection.py", line 359, in connect
    conn = self._connect_tls_proxy(hostname, conn)
  File "D:\Program Files\Python38\lib\site-packages\urllib3\connection.py", line 500, in _connect_tls_proxy
    return ssl_wrap_socket(
  File "D:\Program Files\Python38\lib\site-packages\urllib3\util\ssl_.py", line 453, in ssl_wrap_socket
    ssl_sock = _ssl_wrap_socket_impl(sock, context, tls_in_tls)
  File "D:\Program Files\Python38\lib\site-packages\urllib3\util\ssl_.py", line 495, in _ssl_wrap_socket_impl
    return ssl_context.wrap_socket(sock)
  File "D:\Program Files\Python38\lib\ssl.py", line 500, in wrap_socket
    return self.sslsocket_class._create(
  File "D:\Program Files\Python38\lib\ssl.py", line 997, in _create
    raise ValueError("check_hostname requires server_hostname")
ValueError: check_hostname requires server_hostname
```

## Solution

The problem is related to proxy. Because new version of urllib3 has modified the scheme of proxy it returned from the old scheme:

```python
proxy = urllib.request.getproxies()
# proxy = {
# 'http' : 'http_proxy_ip:port',
# 'https' : 'https_proxy_ip:port'
#}
```
to the new one:

```python
proxy = urllib.request.getproxies()
# proxy = {
# 'http' : 'http://http_proxy_ip:port',
# 'https' : 'https://https_proxy_ip:port'
#}
```

So, modify the scheme to the previous one should work well.

*Updated at 21 Mar, 2022*
  
## Reference
[Reference 1](https://stackoverflow.com/questions/66642705/why-requests-raise-this-exception-check-hostname-requires-server-hostname)

[Reference 2](https://github.com/conda/conda/issues/10590)
