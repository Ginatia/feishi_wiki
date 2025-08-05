
**scrapy=2.13.3**

## 创建目录

```powershell
scrapy startproject project_name
```
## 创建Spider类

```python
from pathlib import Path

import scrapy
from scrapy.http import TextResponse

class Spider(scrapy.Spider):
    name = "s0"

    async def start(self):
        urls = [
        "https://quotes.toscrape.com/page/1/",
        ]

        for url in urls:
            yield scrapy.Request(url=url,callback=self.parse)

    def parse(self,response:TextResponse):
        page = response.url.split("/")[-2]
        filename = f"quotes-{page}.html"
        Path(filename).write_bytes(response.body)
        self.log(f"Saved file {filename}")
```

name： 标识 Spider。在同一个项目内，该名称必须是唯一的。

start： 必须是一个异步生成器，用于生成请求（以及可选的条目），以便开始爬虫。

parse： 该方法将被调用来处理每个请求下载的响应。

## 运行Spider

进入项目顶层

```powershell
scrapy crawl s0
```

## 存储抓取的数据

-O 选项覆盖文件

```powershell
scrapy crawl s1 -O s1.json
```

-o 选项附加到文件

此时选用 Json Line 格式，否则Json格式会因为附加内容使得文件内容无效。
Json Line 格式每一行都是单独的 Json对象。
最终可以使用 JQ 工具进行转换。

```powershell
scrapy crawl s2 -o s2.jsonl
```

## 跟踪链接

```html
<ul class="pager">
    <li class="next">
        <a href="/page/2/">Next <span aria-hidden="true">&rarr;</span></a>
    </li>
</ul>
```

Next 所在的链接，可以用css选择器定位。

```python
response.css("ul.pager li.next a::attr(href)").get()
```

```python
from pathlib import Path

import scrapy
from scrapy.http import TextResponse

class Spider(scrapy.Spider):
    name = "s1"

    async def start(self):
        urls = [
        "https://quotes.toscrape.com/page/1/",
        "https://quotes.toscrape.com/page/2/",
        ]

        for url in urls:
            yield scrapy.Request(url=url,callback=self.parse)

    def parse(self,response:TextResponse):
        for quote in response.css("div.quote"):
            yield {
                "text": quote.css("span.text::text").get(),
                "author": quote.css("small.author::text").get(),
                "tags": quote.css("div.tags a.tag::text").getall(),
            }

        next_page = response.css("ul.pager li.next a::attr(href)").get()
        if next_page is not None:
            next_page = response.urljoin(next_page)
            yield scrapy.Request(next_page,callback=self.parse)

```

## 创建 Request 对象的快捷方式

```python
from pathlib import Path

import scrapy
from scrapy.http import TextResponse

class Spider(scrapy.Spider):
    name = "s2"

    async def start(self):
        urls = [
        "https://quotes.toscrape.com/page/1/",
        "https://quotes.toscrape.com/page/2/",
        ]

        for url in urls:
            yield scrapy.Request(url=url,callback=self.parse)

    def parse(self,response:TextResponse):
        for quote in response.css("div.quote"):
            yield {
                "text": quote.css("span.text::text").get(),
                "author": quote.css("small.author::text").get(),
                "tags": quote.css("div.tags a.tag::text").getall(),
            }

        next_page = response.css("ul.pager li.next a::attr(href)").get()
        if next_page is not None:
            yield response.follow(next_page,callback=self.parse)
```






