# Branch Control Flow

## Câu lệnh if

Tương tự Java, câu lệnh if dùng để kiểm tra một điều kiện rồi quyết định có thực hiện các khối lệnh sau nó

```kotlin
if(a == 9){
    println("A bằng 9")
} else {
    println("A không bằng 9")
}
```

Tất nhiên, những gì Java có thì Kotlin cũng có

- Mệnh đề else
- Các khối lệnh if else lồng nhau

Ngoài ra, điểm khác biệt của Kotlin là lệnh if thực chất là một biểu thức `(expression)`, nó có giá trị trả về. Ví dụ

```kotlin
val x = 10

val str = if(x == 10){
    "X bang 10"
}else{
    "X khac 10"
}

println(str) // X bang 10
```

Kotlin sẽ lấy giá trị cuối cùng trong khối lệnh được thực hiện để trả về giá trị cho câu lệnh if. 

Ví dụ
```kotlin

val xz = 9

val t = if(xz == 9){
    1
    2
    3
    4 // Giá trị này được chấp nhận
}else{
    111
}

println(t) // 4
```

Ngoài ra, nếu ta sử dụng giá trị của câu lệnh if thì mệnh đề else bắt buộc phải tồn tại.

```kotlin
val x = if( t == 0) "T = 0" // Compiler error vì không có khối else
```

Câu lệnh if được dùng để thay thế toán tử check

Java:

```java
String x = (y % 2 == 0) ? "Số chẵn" : "Số lẻ";
```

Kotlin

```kotlin
val x = if(y % 2 == 0) "Số chẵn" else "Số lẻ"
```

## Câu lệnh when

Câu lệnh when tương tự câu lệnh swith - case của Java

```kotlin

val day = 2
when(day){
    2 -> print("Monday")
    3 -> print("Tuesday")
    else -> print("Other day")
}
```

Tương tự Java, mệnh đề `else` tương tự mệnh đề `default` của Java, nó hoạt động khi không có bất kỳ trường hợp nào đúng trước đó và có thể bị lược bỏ khi không cần thiết.

Tuy nhiên, Kotlin có những điểm cải tiến lớn so với Switch-Case khá đơn giản của Java.

### Câu lệnh When có trả về giá trị

```kotlin
val day = 2
val dateName = when(day){
    2 -> "Monday"
    3 -> "Tuesday"
    else -> "Other day"
}
println(dateName)
```

Khi ta sử dụng giá trị trả về của lệnh when, mệnh đề else là bắt buộc.

### Các khối case không chỉ chứa một giá trị đơn thuần

Với Kotlin, ta có thể sử dụng nhiều cách để biểu diễn một case:

- Liệt kê nhiều giá trị
- Kiểm tra giá trị có thuộc phạm vi
- Kiểm tra kiểu của giá trị
- ...

Ví dụ

```kotlin
when(v){
    1 -> println("V = 1")
    2, 3, 4 -> println("V is smaller than 1 but less than 5")
    in 10..20 -> println("V is in range 10 - 20")
    in 30 until 40 -> println("V is in range 30 - 39")
    ! in 100 .. Int.MAX_VALUE -> println("V is smaller than 100 or ...")
    is Point -> println("V is a point and v.x=${v.x}, v.y=${v.y}")
    is String -> println("V is a string $v")
    z * z -> println("v == z * z")
}
```

* Java chỉ cho phép dùng các hằng số để làm nhãn case nhưng Kotlin thì thoải mái!

Mệnh đề when có thể không cần giá trị

Ví dụ

```kotlin
when{
    a > 0 -> println("a > 0")
    b > 0 -> println("b > 0")
    else -> println("a and b <= 0 ")
}
```

#### Notes:

- Nên dùng kết quả trả về của when khi cần thiết
- Nên dùng when để tránh các lệnh if quá phức tạp
