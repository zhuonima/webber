# Webber
一个轻量级爬虫框架

## Get Started

``` go
package main

import (
	"strings"
	"github.com/tzxyz/webber"
)

func main() {
	webber.New().
		Name("妹子图").
		StartUrls("http://www.meizitu.com/a/more_1.html").
		Processor(func(response *webber.Response) *webber.Result {
		// 列表页
		if strings.HasPrefix(response.GetUrl(), "http://www.meizitu.com/a/more_") {
			links := response.Html().Xpath("//h3[@class = 'tit']/a/@href")
			return webber.NewResult().PushUrls(links...)
		}
		// 详情页
		return webber.NewResult().
			PushItem("images", response.Html().Xpath("//div[@id='picture']/p/img/@src")).
			PushItem("title", response.Html().Xpath("//div[@class='metaRight']/h2/a/text()"))
	}).Start()
}

```

## LICENSE
[MIT](LICENSE)