# Method - Lập trình hàm

## VI. Block - khối lệnh

Block có thể coi là một khối lệnh bắt đầu và kết thúc bởi cặp `{` và `}`. Chẳng hạn với mã nguồn sau đây

```kotlin

val x = 10
val y = 20

if(x == y){
    print("x and y are equal")
}else{
    print("x and y are not equal")
}
```

Ta có 2 khối lệnh

```kotlin
{
    print("x and y are equal")
}
```

Và


```kotlin
{
    print("x and y are not equal")
}
```
 sau lệnh lần lượt sau `if` và `else`. 

Với Kotlin, block là một thực thể, cũng giống như giá trị. Điều đó có nghĩa là nó có thể được gán cho một biến, truyền cho một hàm số dưới dạng parametter hay bất kỳ điều gì mà ta dùng với một giá trị bình thường.

Trở lại với ví dụ trên, ta có thể viết nó dưới dạng

```kotlin

val x = 10
val y = 20

val whenEqual = {
    print("x and y are equal")
}


val whenNotEqual = {
    print("x and y are not equal")
}

if(x == y) whenEqual else whenNotEqual
```

Ta khai báo 2 biến để lưu block, lệnh if sẽ quyết định chọn block nào để thực hiện. Chạy đoạn lệnh trên sẽ không thấy in ra gì cả bởi vì lệnh if chỉ đơn thuần là chọn khối lệnh mà chưa thực thi nó. Ta có thể sửa lại lệnh `if` thành bất kỳ kiểu gì dưới đây và sẽ thấy dòng message tương ứng được in ra

```kotlin
(if(x == y) whenEqual else whenNotEqual).invoke()
```
```kotlin
(if(x == y) whenEqual else whenNotEqual)()
```

```kotlin
if(x == y) whenEqual() else whenNotEqual()
```

```kotlin
if(x == y) whenEqual.invoke() else whenNotEqual.invoke()
```

```kotlin
val action = if(x == y) whenEqual else whenNotEqual
action.invoke()
```

Một block có thể được thực hiện bằng cách gọi operator `()` hoặc hàm tương ứng `invoke()`.

Khi ta khai báo 

```kotlin
val whenEqual = {
    print("x and y are equal")
}
```

Viết ở dạng đầy đủ của là

```kotlin
val whenEqual: ()->Unit = {
    print("x and y are equal")
}
```

Ở đây `()->Unit` là kiểu của block, bao gồm

- `()` là danh sách tham số (input) của block
- `Unit` là kiểu giá trị trả về của block. Như đã đề cập ở phần trước, Unit có nghĩa là không trả về gì cả, tương tự hàm `void`
- `->` dùng để phân biệt giữa danh sách tham số và kiểu trả về

Ví dụ dưới đây minh hoạ việc sử dụng block cho việc tạo và gọi hàm

```kotlin
val sqrtInt : (Int) -> Int = { x -> sqrt(x.toDouble()).toInt() }
val unchanged : (Int) -> Int = { x -> x}
val square : (Int) -> Int = {x -> x * x }
val cube : (Int) -> Int = {x -> x * x * x}

fun sumToN(n: Int, transform: (Int)-> Int): Int{
    var sum = 0
    for(i in 1 .. n) sum += transform(i)
    return sum
}

val n = 10
println("Sum sqrt(1) +  ..  sqrt($n) = ${sumToN(n, sqrtInt)}")
println("Sum 1 +  ..  $n = ${sumToN(n, unchanged)}")
println("Sum 1^2 +  ..  $n^2 = ${sumToN(n, square)}")
println("Sum 1^3 +  ..  $n^3 = ${sumToN(n, cube)}")
```

Ở đây:

- `transform` là một param của hàm, nó có kiểu là một khối block chấp nhận 1 số nguyên và cũng trả về 1 số nguyên, nó được gọi bằng operator `()`

- `x -> x * x * x` có nghĩa rằng nếu đầu vào là `x` thì đầu ra sẽ là `x * x * x`, để ý rằng cả đầu vào và đầu ra đều thoả mãn kiểu đã quy định bởi `(Int)->Int`

Ngoài ra:
- Nếu block chỉ có 1 tham số thì tham số đó sẽ có tên mặc định là `it`

```kotlin
val sqrtInt: (Int) -> Int = { sqrt(it.toDouble()).toInt() }
val unchanged: (Int) -> Int = { it }
val square: (Int) -> Int = { it * it }
val cube: (Int) -> Int = { it * it * it }
```

- Nếu block được viết tại nơi được sử dụng thì ta có thể đưa nó ra khỏi cặp dấu `(` và `)`

```kotlin
val sum1 = sumToN(10, { it * it }) //Viết block trong ()

val sum2 = sumToN(10) { it * it }  //Viết block ngoài ()
```

Lưu ý rằng, khi khai báo block, sẽ không có từ khoá return để kết thúc, thay vì đó, biểu thức cuối cùng sẽ chứa giá trị cho block. 

Cũng vì lý do đó, khi viết block, hãy viết ngắn nhất có thể, tránh đưa các câu lệnh phức tạp vào block. Nếu thực sự cần xử lý nhiều thì ta có thể viết thành hàm và block chính là lời gọi hàm.

Ta còn gọi block là một delegate vì ta chuẩn bị 1 action nhưng ta không thực thi action ngay mà sẽ thực thi vào tương lai, ở một thời điểm thích hợp nào đó.

Delegate (Đại biểu): Ta cử 1 đại biểu đi họp liên hợp quốc, trước khi đi, ta dặn nó phải làm XYZ khi cuộc họp diễn ra. Đại biểu đó làm đúng như yêu cầu, khi cuộc họp diễn ra, nó đã làm những điều đã được dặn.