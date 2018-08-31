```
#创建一个项目
scrapy startproject quotetutorial
#创建spider并指定域名
cd quotetutorial
scrapy genspider quotes quotes.toscrape.com
#定义字段items.py
class QuoteItem(scrapy.Item):
    # define the fields for your item here like:
    # name = scrapy.Field()
    text = scrapy.Field()
    author = scrapy.Field()
    tags = scrapy.Field()
#撰写程序quotes.py   
# -*- coding: utf-8 -*-
import scrapy
from quotetutorial.items import QuoteItem

class QuotesSpider(scrapy.Spider):
    name = 'quotes'
    allowed_domains = ['quotes.toscrape.com']
    start_urls = ['http://quotes.toscrape.com/']

    def parse(self, response):
        quotes = response.xpath("//div[@class='quote']")
        for quote in quotes:
            item = QuoteItem()
            text = quote.xpath("./span/text()").extract_first()
            author = quote.xpath(".//small/text()").extract_first()
            tags = quote.xpath("./div[@class='tags']/a[@class='tag']/text()").extract()
            item['text'] = text
            item['author'] = author
            item['tags'] = tags
            yield item
            next = response.xpath("//li[@class='next']/a/@href").extract_first()
            url = response.urljoin(next)
            yield scrapy.Request(url = url,callback = self.parse)
#运行爬虫项目
scrapy crawl quotes
scrapy crawl quotes -o quotes.json
scrapy crawl quotes -o quotes.jl
scrapy crawl quotes -o quotes.csv
scrapy crawl quotes -o quotes.xml
#远程ftp
scrapy crawl quotes -o ftp://user:pass@ftp.example.com/path/quotes.csv
```
