# Xây dựng "Hệ thống theo dõi Server" bằng Flask
### Mục lục
1. <a href="https://github.com/hoanvu/basic_flask_tutorial#mở-đầu">Giới thiệu</a>
2. <a href="https://github.com/hoanvu/basic_flask_tutorial/blob/master/1.%20C%C3%A0i%20%C4%91%E1%BA%B7t%20v%C3%A0%20chu%E1%BA%A9n%20b%E1%BB%8B%20m%C3%B4i%20tr%C6%B0%E1%BB%9Dng.md">Cài đặt và chuẩn bị môi trường</a>
3. <a href="https://github.com/hoanvu/basic_flask_tutorial/blob/master/2.%20X%C3%A2y%20d%E1%BB%B1ng%20b%E1%BB%99%20khung%20cho%20m%E1%BB%99t%20%E1%BB%A9ng%20d%E1%BB%A5ng%20Flask.md">Xây dựng bộ khung cho một ứng dụng Flask</a>
4. <a href="https://github.com/hoanvu/basic_flask_tutorial/blob/master/3.%20Template%20v%C3%A0%20Database.md">Template và Database</a>

### Mở đầu
Ban đầu mình định viết một bài tutorial theo hướng hàn lâm, tức là chỉ đơn thuần giải thích các lí thuyết của Flask và lấy một ví dụ nhàm chán nào đó để cho người đọc dễ hiểu lí thuyết đó. Tên của bài viết ban đầu mình đặt là "Giới thiệu cơ bản về Flask". Nhưng sau đó lại cảm thấy nếu làm thế thì lại đang đi vào lối mòn của trường lớp, nên đã quyết định viết một bài giới thiệu về Flask bằng cách hướng dẫn xây dựng một Hệ thống theo dõi Server đơn giản sử dụng web framework này. 
 
Hệ thống theo dõi Server này thực ra là một mini project mình được giao để làm gần đây, và mình nghĩ việc viết lại những gì mình đã học và làm là cách tốt nhất để hệ thống hóa kiến thức của bản thân cũng như giúp đỡ những bạn mới tìm hiểu có thể bắt đầu một cách nhanh nhất. Các bạn có thể <a href="https://github.com/hoanvu/python_projects/tree/master/web/server_monitoring" target="_blank">truy cập source code của project này tại đây</a>.

> <strong>Lưu ý:</strong> Do kiến thức của mình vẫn còn rất hạn hẹp nên không thể tránh khỏi những lỗi và thiếu sót. Nếu bạn đọc và tìm được lỗi nào đó thì rất mong bạn có thể Fork project này và contribute vào đó. Tất cả đóng góp đều rất được welcome và tôn trọng.

### Giới thiệu về Flask
Flask là một microframework của Python, rất thích hợp với những người mới bắt đầu tìm hiểu viết web app bằng ngôn ngữ này. Tại sao lại gọi đó là một "micro" framework? Trang chủ của Flask nói rằng: "micro" ở đây không có nghĩa là toàn bộ code của ứng dụng mà bạn viết phải nằm gọn trong một file Python duy nhất, "micro" cũng không có nghĩa là Flask thiếu một vài tính năng nào đó. 

<strong>"Micro" nghĩa là phần core của Flask rất gọn nhẹ, nhưng lại có khả năng mở rộng tốt</strong>. Flask sẽ không ép buộc bạn dùng cái gì trong quá trình phát triển. Ví dụ bạn có thể tùy ý chọn database nào mà bạn thích và quen dùng. Thứ duy nhất mà Flask chọn để sử dụng mặc định cho bạn là Template Engine, tuy nhiên bạn vẫn có thể dùng một template engine khác một cách dễ dàng

### Giới thiệu "Hệ thống theo dõi server"
Do mình sẽ dùng project này để mô tả và giải thích các lí thuyết của Flask, và qua đó kết hợp để xây dựng một web app hoàn chỉnh, việc giới thiệu sơ qua về project sẽ giúp bạn có một cái nhìn tổng quan về những gì chúng ta sẽ tìm hiểu và những gì Flask có thể làm được.

> <strong>Lưu ý:</strong> do tutorial này chỉ nói về Flask, nên mình sẽ chỉ giải thích những gì liên quan tới Flask. Những phần không liên quan như: làm thế nào để check trạng thái server hay cách refresh page để cập nhật trạng thái server sẽ không được đề cập đến. Source code liên quan tới các phần đó các bạn có thể xem ở link phía trên mục Mở đầu.

- <strong>Mục đích:</strong> Hệ thống này sẽ check một danh sách các server có trên hệ thống 5 giây một lần và hiển thị trạng thái tương ứng (Online hay Offline)
- <strong>Thành phần:</strong> Ngoài Flask, chúng ta sẽ sử dụng các framework & thư viện sau:
	+ <em>jQuery</em>: tự động refresh trạng thái của server
	+ <em>Bootstrap</em>: giao diện cho frontend
	+ <em>SQLite 3</em>: database chứa thông tin server và user quản trị
- <strong>Tính năng</strong>: các tính năng cơ bản của hệ thống:
	+ Xem danh sách server và trạng thái tương ứng
	+ Thêm server để theo dõi
	+ Thêm user quản trị (*)
	+ Xóa server đang theo dõi khỏi hệ thống (*)
	+ Xóa user (*)
	+ Gửi email khi phát hiện server down

> (*): những tính năng chỉ available khi user đã đăng nhập

- <strong>Cấu trúc cây thư mục của project</strong>: thường thì cấu trúc thư mục của project sẽ khác phụ thuộc vào thói quen của mỗi developer nên sẽ không có một cấu trúc chuẩn nào. Tuy nhiên, trong Flask sẽ có 2 thư mục bắt buộc bạn phải có là <em>templates</em> và <em>static</em>. Lí do tại sao mình sẽ giải thích trong các phần tới. Dưới đây là cấu trúc thư mục mà mình dùng để viết project này. Bạn không cần phải tạo tất cả các file này cùng một lúc ngay bây giờ, mình sẽ hướng dẫn bạn khi nào cần tạo và chức năng của mỗi file/folder là gì khi cần.

```
server_monitoring\
	db\
		servers.db
	static\
		main.js
		style.css
		bootstrap.min.css
		jquery.min.js
	templates\
		layout.html
		index.html
		addUser.html
		login.html
	config.cfg
	schema.sql
	server.py
```
Đây là một screenshot của project mà chúng ta sẽ viết trong tutorial này:

<img src="https://lh3.googleusercontent.com/jEholXw_6GnVWd2hbG81UE_Wmxmod3eWQopflauI2Ho=w1160-h470-no">

<a href="https://github.com/hoanvu/basic_flask_tutorial/blob/master/1.%20C%C3%A0i%20%C4%91%E1%BA%B7t%20v%C3%A0%20chu%E1%BA%A9n%20b%E1%BB%8B%20m%C3%B4i%20tr%C6%B0%E1%BB%9Dng.md">Phần 2: Cài đặt và chuẩn bị môi trường</a>