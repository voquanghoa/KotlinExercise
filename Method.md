# Method - Lập trình hàm

## I. Hàm số

Khác với Java, Kotlin không phải là ngôn ngữ thuần OOP. Điều đó có nghĩa là ta có thể khai báo một hàm số mà không phải khai báo bất kỳ một lớp nào. Ta cũng có thể viết các chương trình mà chỉ cần viết và gọi các hàm số theo kiểu của C, Pascal...

### 1. Khai báo và lời gọi

Cách khai báo hàm số của Kotlin hơi khác so với Java

```kotlin
fun circleArea(length: Int): Double{
    return length * length * Math.PI
}
```

Ở đây:
- `fun` là từ khoá luôn phải có
- `square` là tên hàm số
- `(x: Int)` là danh sách tham số với x là một số kiểu Int, tất nhiên, danh sách có thể rỗng
- `: Int` là kiểu dữ liệu trả về
- Từ khoá return dùng để trả kết quả cho hàm

Việc gọi nó cũng giống java

```kotlin
val s = circleArea(5)
```

### 2. Tham số hàm

Trên đây, ta gọi hàm số với tham số là 5, ta có thể đưa tên parameter vào lời gọi hàm để dễ đọc hơn

```kotlin
val s = circleArea(length = 5)
```

Hàm số không có giá trị trả về thì có thể lược bỏ kiểu trả về. Và ngoài ra, ta cũng có thể gọi hàm với danh sách parametter không giống với cách khai báo

```kotlin
fun someMethod(a: Int, b: String){
    println("a = $a, b = $b")
}

someMethod(a = 10, b = "Hello")
someMethod(b = "Test", a = 9)
```

Kotlin hỗ trợ default parametter (Điều mà Java không hỗ trợ : méo biết tại sao)

```kotlin
fun greeting(name: String, time: String = "Morning"){
    println("Good $time, my name is $name")
}

greeting("Joel") // Time will be Morning
greeting("Mattia", "Afternoon") 
```

Với Java, hàm không trả về giá trị là một hàm `void`. Với Kotlin, hàm không có kiểu trả về thực chất là trả về một Unit, hãy nhớ lấy điều này mặc dù chưa có dùng đến nó

```kotlin
fun someMethod(a: Int, b: String): Unit{
    println("a = $a, b = $b")
}
```

Kotlin cũng hỗ trợ `vararg` (var arg) giúp tạo các hàm có số lượng tham số `bao nhiêu cũng được`. Ví dụ:

```kotlin
fun joinToString(separator: String, vararg strs: String): String{
    return strs.joinToString(separator)
}

fun main() {
    println(joinToString(" ", "a", "b"))
    println(joinToString(" ", "a", "b", "c"))
    println(joinToString(" ", "a", "b", "c", "d"))
    println(joinToString(" ", "a", "b", "c", "d", "e"))
}
```

Với `vararg`:

- Mỗi hàm số chỉ được phép có tối đa 1 tham số là `vararg`
- Tham số `vararg` luôn phải đặt ở cuối
- Giá trị của `vararg` được hiểu là mảng của kiểu dữ liệu `vararg`

### 3. Rút gọn hàm

Nếu hàm số chỉ có 1 dòng thì ta có thể viết gọn như sau

```kotlin
fun circleArea(length: Int): Double = length * length * Math.PI
```

Trong tình huống này, thậm chí kiểu trả về cũng có thể lược bỏ (trình biên dịch biết tự chọn kiểu phù hợp)

```kotlin
fun circleArea(length: Int) = length * length * Math.PI
```
## II. Inner method

Inner method đơn giản là hàm được viết trong phạm vi một hàm khác và cũng chỉ cho phép hàm trực tiếp chứa nó gọi mà thôi.

```kotlin
fun sumSquares(from: Int, to: Int): Int{
    fun square(x: Int) = x * x
    
    var sum = 0
    
    for(i in from .. to){
        sum += square(i)
    }
    
    return sum
}
```

## III. Extension method - Hàm số mở rộng

C# là một ngôn ngữ rất mạnh và một trong những điểm làm nên sức mạnh của nó là các `Extension method`. Rất nhiều thư viện được implement chỉ nhờ vào `Extension method` và điều đó khiến việc sử dụng rất dễ dàng, lập trình viên không cần quan tâm xem thư viện bổ đã sung thêm class nào cũng như đi lùng một núi tài liệu về cách sử dụng. 

Hàm số mở rộng là một tính năng mà Kotlin vay mượn từ ngôn ngữ C#, mặc dù khác biệt về cú pháp nhưng tư tưởng của nó thì hoàn toàn tương đồng.

Để hiểu về `Extension method`, ta cần biết một số hạn chế khi khai báo lớp và hàm theo cách thông thường.

Để thêm 1 hàm vào một class sẵn có, ta có 2 cách:

1. Sửa mã nguồn của lớp đó --> Hơi rắc rối vì phải tìm mà nguồn, sửa nó, build lại. Thậm chí nhiều lúc rất khó, không khả thi.

2. Kế thừa nó --> Không tiện lắm. Ví dụ

```kotlin
open class Point(){
    var x = 0
    var y = 0
}

class PointEx: Point(){
    fun getHyper() = sqrt(1.0 * x*x + y * y)
}

val point1 = Point()
val point2 = PointEx()

point1.getHyper() //Compile error
point2.getHyper()
```

Đối tượng point2 có hàm số `getHyper` nhưng point1 thì lại không có.

Với extension method, ta có thể thêm hàm vào bất kỳ đối tượng của bất kỳ lớp nào. Ví dụ với lớp String, ta muốn có thêm method để chuyển các chữ cái đầu của mỗi từ thành chữ hoa như sau.

```kotlin
fun String.capitalizeEachWord(): String{
    return this.split(' ')
            .joinToString(" ") { it[0].toUpperCase() + it.substring(1) }
}
```

Sau đó ta có thể gọi hàm này từ bất kỳ một string nào

```kotlin
println("cong hoa xa hoi chu nghia viet nam".capitalizeEachWord())
```

Ở đây, cách khai báo một extension method cho một lớp bất kỳ cũng không khác mấy so với cách khai báo hàm bình thường, chỉ cần thêm `<Tên lớp>.` vào trước tên hàm được khai báo. Và lời gọi thì ta sẽ gọi trực tiếp (rất tiện lợi) từ bất kỳ một đối tượng nào thuộc lớp đó.

Nếu ta đang viết một chương trình xử lý văn bản thì hàm trên sẽ rất cần thiết nhưng nếu ta đang viết chương trình tính tính toán đơn thuần thì hẳn là sẽ không cần chút nào. Nên cùng là một lớp String, có một số hàm chỉ quan trọng cho tình huống này nhưng sẽ không cần cho tình huống khác --> Ý nghĩa của extension method: Ta chỉ load các chức năng nào đó của lớp nếu cần thiết.

## IV. Infix function

Phần này chỉ đọc hiểu bởi vì ta sẽ gặp nó khá nhiều lần và cần hiểu cái cú pháp đó là gì. Hiểu rồi thì sẽ biết rằng nó cũng chỉ là một lời gọi hàm mà thôi.

Infix là từ khoá lúc khai báo hàm để cho phép lời gọi hàm được tự nhiên hơn.

Ví dụ trong câu lệnh

```kotlin
for(i in 1 until 10){
    println(i)
}
``` 

Thì thực chất `1 until 10` được khai báo là một hàm `infix` như thế này

```kotlin
public infix fun Int.until(to: Int): IntRange {
    //Thân hàm, đừng quan tâm
}
```

Ở đây: `Int.until` hàm số `until` mở rộng cho lớp `Int` còn `infix` cho phép lời gọi hàm được thực hiện rất tự nhiên `1 until 10` thay vì là `1.until(10)`

Ví dụ khác, ta có muốn lớp String có thêm method `repeat` để lặp lại một số lần. Ta sẽ viết là

```kotlin
fun String.repeat(n: Int): String{
  return (0 until n).joinToString(" "){this}
}

fun main() {
    println("Hello".repeat(10))
}
```

Nhưng nếu thêm từ khoá `infix`

```kotlin
infix fun String.repeat(n: Int): String{
  return (0 until n).joinToString(" "){this}
}

fun main() {
    println("Hello" repeat 10)
    
    val x = "x"
    val y = x repeat 4
    val z = y repeat 10
    
    print(z)
}
```

Do phải thoả mãn cú pháp lúc gọi nên `infix` chỉ được áp dụng nếu thoả mãn các điều kiện

- Method thuộc lớp (như đã nói ở trên, với Kotlin không phải method nào cũng thuộc lớp) hoặc extension method
- Method phải có đúng 1 tham số và không được phép có giá trị default và cũng không phải là `vararg`

## V. Operator function

Một số ngôn ngữ như C++, C# có hỗ trợ operator overloading, đây là cách hỗ trợ các toán tử thường gặp như +-*/... lên các lớp mà bản thân ngôn ngữ chưa hỗ trợ.

Ví dụ: hiện tại Kotlin chưa hỗ trợ phép nhân (*) giữa một String và một số nguyên. Do đó nếu viết

```kotlin
println("abc" * 4)
```

Thì trình biên dịch sẽ báo lỗi.

Ta tạm định nghĩa phép nhân đó có ý nghĩa là lặp lại chuỗi với n lần (n là số sau ký tự *) và cần implement nó như thế này và kết quả sẽ như mong đợi.

```kotlin
operator fun String.times(n: Int): String{
    return (0 until n).joinToString(" "){this}
}

fun main() {
    println("abc" * 4)
}
```

Ở đây: hàm times được dịch thành toán tử *.

Ngoài ra:

- Operator phải là method thuộc lớp hoặc extension method
- Số lượng tham số tối đa là 1 (có thể 0), tuỳ thuộc operator
- Tên hàm phải theo chính xác theo [quy định](https://kotlinlang.org/docs/reference/operator-overloading.html)