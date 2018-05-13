---
layout: post
title: "Quản lý nhiều tài khoản github trên máy tính"
date: 2018-05-13 20:47:10 --0600
categories: git
---
Đây là giải pháp cho việc sử dụng nhiều tài khoản github trên cùng một máy tính.

**Bước 1:** Tạo key ssh cho tài khoản thứ 2, dùng lệnh ssh-keygen
{% highlight shell %}
ssh-keygen -t rsa -C "<địa chỉ email của tài khoản>"
{% endhighlight %}

Chú ý khi cung cấp thông tin đường dẫn cho tập tin mới: tên mới phải khác tên đã được tạo cho các tài khoản trước đó nếu không tập tin cũ sẽ bị ghi đè. Tên file mới có thể đặt theo dạng `~/.ssh/id_rsa_<tên tài khoản>`, ví dụ `~/.ssh/id_rsa_nghecode`

**Bước 2:** Thêm key vừa tạo vào tài khoản github:

Đăng nhập vào tài khoản github, vào mục `Settings > SHH and GPG Keys` chọn nút `New SSH Key`

Nhập nội dung bất kỳ ở mục `Title` ví dụ như `macbook-key`

Copy nội dung của file `~/.shh/id_rsa_<tên tài khoản>.pub` và paste vào mục key, có thể dùng lệnh bên dưới để copy nội dung của file public.
{% highlight shell %}
pbcopy < ~/.ssh/id_rsa_nghecode.pub
{% endhighlight %}
Tiếp theo, thêm key vừa tạo vào ssh bằng lệnh `ssh-add`, ví dụ: `ssh-add ~/.ssh/id_rsa_nghecode`

**Bước 3:** Tạo file config:

Để giúp git có thể nhận biết được chúng ta sử dụng tài khoản nào để làm việc với github thì ta phải tạo file config. Bạn có thể dùng `vim` hoặc bất kỳ công cụ nào để chỉnh sửa nội dung của file config
{% highlight shell %}
vim ~/.ssh/config
{% endhighlight %}
Thêm đoạn cấu hình như sau:
{% highlight shell %}
# Default account
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa
{% endhighlight %}
Đây là cấu hình mặc định cho tài khoản hiện hành. Tiếp theo chúng ta thêm một đoạn cấu hình tương tự cho tài khoản mới. Chúng ta có thể tự đặt lại thông tin của trường `Host`
{% highlight shell %}
# Account nghecode
Host nghecode.vn
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_rsa_nghecode
{% endhighlight %}
Sau khi nhập xong thì lưu thông tin lại và thoát khỏi Vim bằng lệnh `:wq`

**Bước 4:** Thử nghiệm và tận hưởng thành quả :)

Lưu ý khi thêm thông tin repository đối với tài khoản thứ hai chúng ta vừa thêm vào, chúng ta phải dùng thông tin tại trường `Host` trong file config thay cho giá trị mặc định là github.com

Mặc đinh:
{% highlight shell %}
git remote add origin git@github.com:<tên tài khoản>/nghecode.github.io.git
{% endhighlight %}

Tài khoản thứ 2:
{% highlight shell %}
git remote add origin git@nghecode.com:<tên tài khoản>/nghecode.github.io.git
{% endhighlight %}
