# Getting started - Giới thiệu chung về Kotlin

Kotlin là ngôn ngữ lập trình do hãng JetBrain tạo ra. Nói về [Jetbrain](https://www.jetbrains.com/), đây là công ty công nghệ đứng sau các sản phẩm khủng như InteliJ, Reshaper, PhpStorm, RubyMine, PyCharm, WebStorm. Android Studio thật ra là một phiên bản tuỳ biến của InteliJ. 

Kotlin thực chất dựa trên ngôn ngữ Java, tất cả các dòng lệnh ta viết trên Kotlin đều được biên dịch sang ngôn ngữ Java trước khi biên dịch sang java byte code chạy trên JVM.

Kotlin có độ tương thích cao với Java, cho dù có nhiều cú pháp khác nhau nhưng cái gì Java làm được thì Kotlin cũng làm được. Thậm chí, ta cũng có thể biên dịch trực tiếp mã nguồn Java và Kotlin qua lại lẫn nhau.

Kotlin được Google khuyến khích sử dụng để phát triển ứng dụng android native nhờ các ưu điểm:

- Google đã chọn Kotlin là ngôn ngữ Offical để phát triển ứng dụng Android
- Ngôn ngữ gọn ngàng, dễ đọc, dễ hiểu, dễ sử dụng
- Hiệu quả cao nhờ vào một tá tính năng mà Java không có
- Giải quyết được nhiều giới hạn của Java
- Giảm thiểu lỗi lập trình
- Rất dễ học nếu đã làm biết Java (hoặc bất kỳ một ngôn ngữ OOP nào khác)
- Tương thích cao với các thư viện Java và có nhiều thư viện riêng cho Kotlin
- Các tài liệu từ Google giờ đây hỗ trợ mặc định Kotlin
- Mã nguồn mở (cái này thì google thích chứ thật ra mình méo liên quan)
- Cộng đồng đang phát triển mạnh

Tất nhiên Kotlin cũng có vài nhược điểm

- Cộng đồng trẻ, gặp 1 stuck, tra google --> Kết quả là Java, tìm tài liệu: tất nhiên tài liệu Java nhiều hơn
- Tốc độ chậm hơn nếu lạm dụng một số tính năng
- Có tính gây nghiện cao, quen rồi không muốn quay về Java. Khi bạn quá rành Kotlin nhưng công ty bắt bạn làm Java
- Haiz một ngôn ngữ mới à, lại phải học :p
- Nhiều thứ mới quá thì đúng là phải mất công tìm hiểu thật
- Một vài tính năng có thể gây khó chịu (không nhiều đâu, hứa)
    - Kiểm tra kiểu quá chặt, số Int gán cho biến Long --> Báo lỗi
    - Mặc định, lớp là final

Có rất nhiều tính năng mà chỉ kotlin mới có, dưới đây là ví dụ

    - Infix function, 
    - Inner function
    - Operator Overloading
    - Extension function
    - Lambda function
    - Build in delegate
    - Inline class, data class
    - Delegated properties
    - Kiểm tra null
    - Smart cast

Kotlin là một ngôn ngữ mã nguồn mở, thật vậy, ta có thể xem mã nguồn của nó cũng như mã nguồn của các thư viện mà nó viết ra ở đây https://github.com/JetBrains/kotlin

Chẳng hạn, muốn xem cách mà Kotlin tạo interface MultableList https://github.com/JetBrains/kotlin/blob/master/libraries/stdlib/src/kotlin/collections/Collections.kt

Trên InteliJ và Android Studio, ta cũng dễ dàng xem cách mà một hàm được implement bằng phím tắt Ctrl + B (Command + B)