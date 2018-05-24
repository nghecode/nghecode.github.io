---
layout: post
title: "Tạo một ứng dụng REST API bằng ngôn ngữ lập trình Go"
date: 2018-05-24 20:47:10 --0600
tags: [go, rest api]
---

Go là một ngôn ngữ lập trình được Google giới thiệu vào năm 2009. Rất nhanh sau đó nó đã được phổ biến ra toàn thế giới vì sự mạnh mẽ và tiện dụng của nó trong việc phát triển các sản phẩm phần mềm.Trong khuôn khổ bài viết này mình muốn giới thiệu đến các bạn một ứng dụng Rest API nhỏ viết bằng Golang.

** Nội dung: **

* Thêm xoá sửa và truy vấn dữ liệu từ
* Làm việc với cơ sở dữ liệu Postgresql
* Quản lý đăng ký người dùng

** Cài đặt **

Việc cài đặt ngôn ngữ Golang tương đối đơn giản nên tôi sẽ không đề cập trong bài viết này, các bạn có thể tham khảo thêm tại trang chủ [Golang](https://golang.org/)

1.  Từ thư mục làm việc của Go bạn tạo một thư mục mới, thư mục này dùng để chưa mã nguồn của dự án mới.
    {% highlight shell %}
    cd $GOPATH/src/github.com/<tên user github>/contact-api
    {% endhighlight %}

2.  Cài đặt thêm các thư viện cần thiết, ở đây tôi không sử dụng 1 framework cụ thể nào mà chỉ dùng các thư viện đơn lẻ, bao gồm:

* gorilla: dùng cho việc quản lý routing
* jwt-go: dùng cho việc quản lý authentication
* sqlx: dùng để làm việc với Postgresql

  {% highlight shell %}
  go get "github.com/jmoiron/sqlx"
  go get "github.com/gorilla/mux"
  go get "github.com/gorilla/context"
  go get "github.com/gorilla/handlers"
  go get "github.com/dgrijalva/jwt-go"
  {% endhighlight %}

** Viết những dòng mã đầu tiên **

Từ thư mục làm việc, tạo tập tin `main.go`
{% highlight shell %}
touch main.go
{% endhighlight %}