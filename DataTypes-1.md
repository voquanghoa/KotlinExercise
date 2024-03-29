# Biến và các kiểu dữ liệu thường gặp

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

Thật ra Kotlin không có kiểu dữ liệu nguyên thuỷ, tất cả các giá trị đều là object. Ví dụ số `1` thực chất là một `Object` kiểu `Int` và có giá trị là 1. Ta có thể gọi các hàm của `1`, điều này không giống như Java

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

- Khác với Java, các phép ép kiểu số học bằng các cú pháp ép kiểu thông thường là bị cấm. Nguyên nhân là ở chổ, tất cả các giá trị số đều là những Object và 1 Object của class này không thể gán cho biến thuộc class kia nếu 2 class không có quan hệ kế thừa.
- Để chuyển một giá trị sang một kiểu dữ liệu mới, ta luôn sử dụng hàm `toXXX`. Ví dụ

```kotlin
val x = 123             // Khai báo biến x là Int
val y = x.toDouble()    // Ép giá trị sang Double để gán cho y
```

Ở đây class `Int` và `Double` là 2 class khác nhau và chúng đều kế thừa lớp `Number`. 

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

## 3. Kiểu mảng

Giống như Java, mảng là danh sách các phần tử với số lượng cố định, mỗi khi đã tạo mảng thì không được thêm bớt số lượng phần tử. Các phần tử trong mảng phải có kiểu phù hợp với kiểu mảng.

Tất cả các mảng đều có:

- Thuộc tính `size` lấy về số lượng phần tử
- Toán tử `[]` để đọc ghi giá trị của mảng theo chỉ số
- Thuộc tính `indicates` để lấy tập hợp các chỉ số mảng `[0 .. (size-1)]`
- Và hàng tá các thứ khác :D

Để duyệt qua các phần tử trong mảng, ta có các cách thức sau

```kotlin
var array = IntArray(10){it * it}
    
//Lặp qua các phần tử
for(x in array){
    println(x)
}

//Lăp theo chỉ số
for(i in 0 until array.size){
    println(array[i])
}

//Lăp theo chỉ số build-in
for(i in array.indices){
    println(array[i])
}
```

Tuỳ theo việc cách nào tiện hơn (tuỳ lúc) mà ta nên chọn cách phù hợp

### Mảng nguyên thuỷ

Mảng nguyên thuỷ là các mảng được khai báo sẵn với các kiểu dữ liệu nguyên thuỷ.

Chẳng hạn với kiểu Int, ta có mảng cho kiểu Int là `IntArray`.

Để khởi tạo mảng có n phần tử

```kotlin
var array = IntArray(n)
```

Mảng `array` được tạo ra sẽ có `n` phần tử là các số nguyên với giá trị là `0`

Ta có thể đưa block để khởi tạo giá trị các phần tử thay vì là `0`

```kotlin
var arr1 = IntArray(n){10} // Các phần tử sẽ là 10
var arr2 = IntArray(n){it * it} // Các phần tử sẽ là 0, 1, 4, 9, ... (n-1)*(n-1)
```

Để khởi tạo một mảng từ danh sách các phần tử

```kotlin
var arr = intArrayOf(1, 2, 3, 4)
```

Tương tự, ta cũng có các mảng khác với các kiểu dữ liệu nguyên thuỷ khác


| Kiểu phần tử | Kiểu mảng    | Lệnh khởi tạo mảng từ danh sách |
|--------------|--------------|---------------------------------|
| Int          | IntArray     | intArrayOf(...)                 |
| Char         | CharArray    | charArrayOf(...)                |
| Double       | DoubleArray  | doubleArrayOf(...)              |
| String       | StringArray  | stringArrayOf(...)              |
| Byte         | ByteArray    | byteArrayOf(...)                |
| Boolean      | BooleanArray | booleanArrayOf(...)             |
| Long         | LongArray    | longArrayOf(...)                |
| ...          | ...          | ...                             |

### Mảng định kiểu (TypedArray)

Với kiểu dữ liệu nguyên thuỷ, ta sẽ dùng kiểu mảng nguyên thuỷ như trên. Tuy nhiên với một kiểu dữ liệu bất kỳ, ta sẽ dùng đến kiểu mảng định kiểu như ví dụ sau:

Để khởi tạo mảng 10 phần tử cho kiểu `Point?` với các phần tử là `null`

```kotlin
var points = Array<Point?>(10){null}
```

Với kiểu `Point`, ta bắt buộc phải đưa các đối tượng của lớp `Point` vào mảng

```kotlin
var points = Array<Point>(10){ Point(it, it) }
```

Câu lệnh trên có thể rút ngắn bằng cách bỏ `<Point>` đi vì trình biên dịch dựa vào `{ Point(it, it)}` đã biết rõ kiểu của mảng là gì

```kotlin
var points = Array(10){ Point(it, it) }
```

Cũng tương tự mảng build-in, ta cũng có lệnh để tạo ra mảng từ một danh sách cố định các phần tử

```kotlin
var points = arrayOf(point1, point2, point3)
```

Lưu ý rằng, một mảng các phần tử NotNull thì không được gán giá trị `null` vào danh sách

```kotlin
var points = Array(10){ Point(it, it) }

points[0] = Point(2, 3) //Ok
points[1] = null // NOT OK
```

Ngoài ra, ta cũng có thể khai báo mảng cho kiểu dữ liệu nguyên thuỷ bằng cách này

```kotlin
var arr1 = Array(4){1} //Mảng 4 số 1
var arr2 = arrayOf(1, 1, 1, 1)
```

Mặc dù `Array<Int>` và `IntArray` đều có cùng một ý nghĩa: `Mảng của các số nguyên Int` nhưng đáng tiếc, chúng vẫn là 2 kiểu khác nhau nên không thể gán qua lại

```kotlin
var arr1 = Array(4){1}
var arr2 = IntArray(4){1}

arr1 = arr2 //NOT OK
```

Tuy nhiên cũng không phải lo, ta luôn có hàm để chuyển đổi giữa chúng

```kotlin
var arr1 = Array(4){1}
var arr2 = IntArray(4){1}

arr1 = arr2.toTypedArray()      //OK
arr2 = arr1.toIntArray()        //OK
```

Đọc thêm
- [1](https://kotlinlang.org/docs/reference/basic-types.html)