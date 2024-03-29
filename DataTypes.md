# Biến và các kiểu dữ liệu cơ bản

## 1. Biến

### Khai báo biến đọc ghi

Để khai báo một biến theo kiểu của Java, ta dùng từ khoá `var` với cú pháp

```kotlin
var <tên biến>[: <tên kiểu>] [= giá trị]
```

Ví dụ:
```kotlin
var x: Double = 12.3
```

Sẽ tương đương với

```java
double x = 12.3;
```

Tương tự Java, phần giá trị cũng có thể lược bỏ và biến sẽ có giá trị mặc định (thường là 0) của kiểu được khai báo

Ví dụ:

```kotlin
var x : Double // x sẽ có giá trị mặc định là 0.0
```

Ngược lại, nếu ta đưa giá trị khởi tạo cho biến thì tên kiểu có thể được lược bớt vì dựa vào kiểu của giá trị khởi tạo, trình biên dịch có thể chọn kiểu dữ liệu phù hợp nhất

Ví dụ:

```kotlin
var x = 0.0 // x là biến kiểu Double
var y = 12  // y là biến kiểu Int
```

Tương tự Java, một biến khi được khai báo với kiểu này thì chỉ có thể nhận giá trị của kiểu đó. Thậm chí, việc kiểm tra kiểu có khi còn chặt hơn Java (Bullshit)

Ví dụ các lệnh dưới đây là ok với Java

```java
int x = 10;
long y = x;
```

Điều tương tự không xảy ra với kotlin

```kotlin
val x = 10
val y: Long = x
```

Kotlin không cho phép ta gán trực tiếp một giá trị kiểu Int cho biến kiểu Long, thay vào đó, ta bắt buộc phải chuyển kiểu

```kotlin
val x = 10
val y: Long = x.toLong()
```

#### Notes

Trong phần lớn các trường hợp, ta nên chọn kiểu khai báo này vì nó nhanh và kiểu dữ liệu linh hoạt, ta không phải mất công xác định kiểu của một biểu thức vì trình biên dịch sẽ tự làm giùm.

### Khai báo biến chỉ đọc

Cú pháp khai báo biến chỉ đọc cũng tương tự cú pháp khai báo biến đọc ghi


```kotlin
val <Tên biến> [: <Tên kiểu>] = <giá trị khởi tạo>
```

Ví dụ

```kotlin
val x = 123 // Biến x là kiểu Int và luôn có giá trị là 123
val y: Double = 12.3 // Biến y là kiểu Double và luôn có giá trị là 12.3
```

Chỉ khác:

- Thay thế từ khoá `var` bằng `val`
- Giá trị khởi tạo là bắt buộc (không được lược bỏ)

#### Notes

Ưu tiên sử dụng `val` thay cho `var`. Một biến chỉ nên được khởi tạo với một giá trị cố định đến hết vòng đời và cho một mục đích. Đừng cố tái sử dụng biến cho một mục đích khác với mục đích được khai báo lúc đầu.

## 2. Các kiểu dữ liệu nguyên thuỷ

### Các kiểu số học

#### Các kiểu thường gặp

- Kiểu số nguyên:
    - Byte
    - Int
    - Long
- Kiểu số thực:
    - Float
    - Double

#### Notes:

- Sử dụng các kiểu số thực khi và chỉ khi cần lưu trữ dữ liệu có phần thập phân.
- Mặc định, hãy sử dụng Int cho số nguyên và Double cho số thực vì nó hợp lý trong đa phần các trường hợp

#### Các biểu diễn số

- Một số thực có phần thập phân sẽ tự động được hiểu là Double
- Một số nguyên được viết sẽ tự động được hiểu là số Int
- Một số nguyên nếu không thuộc phạm vi kiểu Int thì sẽ tự động được hiểu là Long
- Để mô tả một số thực thuộc Float thì thêm posFix `f`, tương tự, thêm posfix `l` để đánh dấu số Long thay vì Int
- Có thể dùng ký tự gạch dưới `_` để viết các chữ số theo nhóm cho dễ đọc mà không làm thay đổi giá trị của số
- Có thể viết số thực dưới dạng số học `xey = x * 10^y`

Ví dụ:

```kotlin
val doubleVal1 = 123.2 //Biến kiểu Double
val doubleVal2 = 1e6    // Biến kiểu Double
val floatVal = 12.23f   // Biến kiểu Float

val intVal = 123        // Biến kiểu Int
val longVal = 4000000000    // Biến kiểu Long
val longVal2 = 123l         // Biến kiểu Long
val iPhonePrice = 30_000_000 // Biến kiểu Int

```

#### Ép kiểu

- Khác với Java, các phép ép kiểu số học bằng các cú pháp ép kiểu thông thường là bị cấm
- Để chuyển một giá trị sang một kiểu dữ liệu mới, ta luôn sử dụng hàm `toXXX`. Ví dụ

```kotlin
val x = 123             // Khai báo biến x là Int
val y = x.toDouble()    // Ép giá trị sang Double để gán cho y
```

### Kiểu luận lý

Kiểu luận lý Boolean là kiểu chỉ nhận 2 giá trị là `true` và `false`

Các phép toán trên Boolean

- Phủ định: Có thể dùng toán tử `!` hoặc hàm `not`
- Kết hợp and: có thể dùng toán tử `&` hoặc hàm `and`
- Kết hợp or: có thể dùng toán tử `|` hoặc hàm `or`
- Kết hợp xor: có thể dùng toán tử `^` hoặc hàm `xor`

### Kiểu chuỗi ký tự

Kiểu chuỗi ký tự String tương ứng với kiểu String của Java với một số cải tiến

- Truy cập ký tự theo chỉ số bằng cú pháp `[]` thay vì dùng hàm `charAt`
- Hỗ trợ Interpolation với cú pháp `$<tên biến>` hoặc `${<biểu thức>}`.

Ví dụ:

```kotlin

val str = "Vietnam"

println(str[2]) // Print `e`

val str2 = "Chuoi $str có độ dài ${str.length}"
```

Giống như Java, kotlin cũng hỗ trợ Escaping character

- \\\\ Ký tự \
- \n Xuống dòng
- \\\" dấu 2 nháy
- \t tab
- ...

Đọc thêm [Here](http://zetcode.com/kotlin/strings/)

#### Notes

Trong phần lớn các trường hợp, hãy sử dụng string interpolation thay cho các phép `+` giữa string và các giá trị

### Kiểu Char

Kiểu Char dùng để biểu diễn 1 ký tự

```kotlin
val x = 'A'
```

Có 3 thao tác hữu ích có thể dùng trên kiểu Char là

- Phép trừ giữa 2 giá trị Char sẽ cho ra khoảng cách của 2 ký tự đó
- Phép cộng/trừ giữa 1 Char ch và một số nguyên n sẽ cho ra 1 ký tự Char sau hoặc trước ch một khoảng cách là n
- Chuyển ch về String

```kotlin
val x = '8'
val y = x - '0' // y : Int = 8
val z = 'd' + 2 // z : Char = 'f'
val t = z.toString()
```

### Kiểu phạm vi

Kiểu phạm vi dùng để xác định miền giá trị, có 2 cách biểu diễn, ví dụ

- 1 .. 9 // Các giá trị từ 1 đến 9, bao gồm cả 1 và 9
- 1 until 9 // Các giá từ 1 đến 9 nhưng không bao gồm 9, hay nói cách khác là 1 đến 8

Ví dụ

```kotlin
val range = 1..9
println(6 in range) // true
```

Đọc thêm
- [1](https://kotlinlang.org/docs/reference/basic-types.html)