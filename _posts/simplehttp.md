
一行代码就能跑起来一个网页server，对于一个只接触过Flask的还是很震惊的。代码也不长，看了看其实现过程。其实SimpleHTTPServer继承于BaseHTTPServer，我觉得神奇的地方（把server搭起来）其实都是BaseHTTPServer实现的，SimpleHTTPServer做了一些get request和head request的功能。我理解的get request在代码中指获取某个linux path下的文件，把它转换为www的path；head request大概指头部信息的代码部分。
```
python -m SimpleHTTPServer 8000
```
