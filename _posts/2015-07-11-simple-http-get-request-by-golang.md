---
layout: post
title: Пример простого HTTP GET запроса на Go
redirect_to: "https://ypakala.com"
date: 2015-07-11 23:13
tags:
- go
---

![Go: Пример простого HTTP GET запроса](https://raw.githubusercontent.com/wcoder/blog/0d4789ac028877ebbcccef0815fe5765051474d2/go-http-get/golang.png)

#### Начало

Создаем файл `http-get.go`, пишем туда следующий код:

``` go
package main

import (
	"os"
	"io/ioutil"
	"log"
	"net/http"
)

func main() {

	resp, err := http.Get(os.Args[1])
	if err != nil {
		log.Fatal(err)
	}

	defer resp.Body.Close()

	body, err := ioutil.ReadAll(resp.Body)
	if err != nil {
		log.Fatal(err)
	}

	_, err = os.Stdout.Write(body)
	if err != nil {
		log.Fatal(err)
	}
}
```

Компилируем:

``` cmd
go build http-get.go
```

Выполняем:

* для Linux `./http-get http://google.com`
* для windows `http-get.exe http://google.com`

Запись результата в файл:

``` cmd
./http-get http://google.com > index.html
```

#### Пример

![Результат](https://raw.githubusercontent.com/wcoder/blog/0d4789ac028877ebbcccef0815fe5765051474d2/go-http-get/result.PNG)
