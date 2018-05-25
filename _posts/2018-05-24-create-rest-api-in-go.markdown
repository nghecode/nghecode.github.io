---
layout: post
title: "Tạo một ứng dụng REST API bằng ngôn ngữ lập trình Go"
date: 2018-05-24 20:47:10 --0600
tags: [go, rest api]
---

Go là một ngôn ngữ lập trình được Google giới thiệu vào năm 2009. Rất nhanh sau đó nó đã được phổ biến ra toàn thế giới vì sự mạnh mẽ và tiện dụng của nó trong việc phát triển các sản phẩm phần mềm.Trong khuôn khổ bài viết này mình muốn giới thiệu đến các bạn một ứng dụng Rest API nhỏ viết bằng Golang.

**Nội dung:**

* Thêm xoá sửa và truy vấn dữ liệu từ
* Làm việc với cơ sở dữ liệu Postgresql
* Quản lý đăng ký người dùng

**Cài đặt**

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

**Viết những dòng mã đầu tiên**

Từ thư mục làm việc, tạo tập tin `main.go`
{% highlight shell %}
touch main.go
{% endhighlight %}

Đầu tiên chúng ta tạo một hàm main, khi chạy ứng dụng hàm này sẽ được thực thi đầu tiên. Chung ta tạo một hàm khi chạy sẽ hiển thị lên trang chủ cụm từ `Hello, <tên bạn>`

```go
package main

import (
  "fmt"
  "html"
  "log"
  "net/http"
)

func main() {
  http.HandleFunc("/", func(w http.ResponseWriter, r *http.Request) {
        fmt.Fprintf(w, "Hello, %q", html.EscapeString(r.URL.Path))
      })

  log.Fatal(http.ListenAndServe(":3001", nil))

}
```
> Bạn nên dùng Visual Code và cài đặt thêm các gói mở rộng để viết mã các chương trình bằng Golang. Bạn sẽ không phải khai báo các thư viện bằng lệnh `import`, VSCode sẽ tự đông làm giúp bạn khi có khai báo mới.

Sau khi lưu lại các thay đổi bạn có thể build và chạy thử chương trình bằng câu lệnh sau:
```shell
go build

./contact-api
```
Sau khi chạy xong bạn mở trình duyệt web và truy cập vào địa chỉ `http://localhost:3001` để kiểm tra chương trình.

Đến đây cơ bản bạn đã làm cho ứng dụng hoạt động được nhưng làm cách nào để ứng dụng có thể hiển thị tên bạn trong câu `Heello <tên bạn>` hay làm cách nào chúng ta có thể thay đổi các thông số để có thể truy vấn dữ liệu từ API ... mới các bạn tiếp tục theo dỏi phần tiếp theo.
