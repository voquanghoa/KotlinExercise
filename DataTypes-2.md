# Tập hợp

## Kiểu mảng

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

## Kiểu danh sách

Kiểu danh sách trên `Kotlin` thật ra là sự kế thừa từ kiểu dữ liệu `ArrayList` từ `Java`. Đây là kiểu dữ liệu sử dụng mảng một cách linh động để dữ liệu có thể được thêm vào hay bớt đi tuỳ ý.

### 1. Kiểu List

Kiểu List cung cấp khả năng chứa đựng một danh sách cố định các phần tử, không những số phần tử cố định (giống mảng) mà giá trị các phần tử cũng cố định luôn.

List được sử dụng khi ta muốn có một danh sách các phần tử mà sẽ không có bất kỳ sự thay đổi nào trên đó cả.

Để tạo ra một đối tượng List, ta có thể dùng các cách sau

```kotlin
//Dùng hàm List(n, block)
var strs = List(10){ "String $it" }

//Dùng hàm listOf
strs = listOf("Vietnam", "Thai Lan", "Lao")

//Dùng hàm toList trên một tập hợp khác
strs = (arrayOf("A", "B", "C")).toList()
```

Một số thành viên thường dùng của List

- Thuộc tính `size` trả về số phần tử
- Method `isEmpty()`, `isNotEmpty()` kiểm tra list có rỗng
- Method `contains()` kiểm tra list có chứa một phần tử nào đó
- Operator `[]` lấy phần tử theo index giống như mảng
- ...

### 2. Kiểu MutableList

`MutableList` là danh sách động các phần tử, ta có thể thêm, bớt phần tử thoải mái.  Tương tự `List`, ta có các cách sau để khởi tạo 1 `MutableList` 

```kotlin
//Dùng hàm MutableList(n, block)
var strs = MutableList(10){ "String $it" }

//Dùng hàm mutableListOf
strs = mutableListOf("Vietnam", "Thai Lan", "Lao")

//Dùng hàm toMutableList trên một tập hợp khác
strs = (arrayOf("A", "B", "C")).toMutableList()
```

Có đầy đủ các chức năng `List` nhưng `MutableList` còn cung cấp khả năng cập nhật danh sách bao gồm:

- Method `add(x)`: thêm một phần tử (vào cuối hoặc vào vị trí bất kỳ)
- Method `remove(x)`: xoá bỏ một phần tử nhất định
- Method `removeAt()`: Xoá bỏ phần thử ở vị trí
- Method `addAll(..)`: thêm một danh sách khác
- Method `clear()`: xoá toàn bộ danh sách
- ....


## Kiểu Set và MutableSet

Kiểu `Set` và `MutableSet` gần giống `List` và `MutableList`, tuy nhiên, ở kiểu `Set` và `MutableSet`, các phần tử là duy nhất.

Chẳng hạn

```kotlin
val x = setOf(1, 0, 2, "AS", 1, 2)
```

Khi ta viết như trên, thực chất không khác gì so với

```kotlin
val x = setOf(1, 0, 2, "AS")
```

Hàm `add` trên `MutableSet` luôn kiểm tra xem phần tử cần `add` đã tồn tại hay chưa trước khi thực sự thêm vào.

## Kiểu Map và MutableMap

Kiểu `Map` và `MutableMap` cũng giống `List` và `MutableList`, tuy nhiên, các phần tử của nó là một cặp `pair` giữa hai giá trị `key` và `value`. Kiểu dữ liệu của `key` và `value` là bất kỳ và cũng không nhất thiết phải giống nhau.

Ví dụ

```
val goldMedals = mapOf(
        "Phi" to 130,
        "Vietnam" to 98,
        "Thailand" to 92
)

println("Vietnamese's medals count is ${goldMedals["Vietnam"]}")
```

Ở đây:

- `goldMedals` là map với `key` là String và `value` là Int
- Ta sẽ dùng hàm `to` để tạo ra pair giữa `key` và `value`
- Ta sẽ dùng operator `[key]` để lấy ra giá trị của `value` 

Ngoài ra:

- `containsKey`: để kiểm tra một key có tồn tại hay không
- `containsValue`: để kiểm tra một value có tồn tại hay không

Để duyệt qua các phần tử của Map, ta có 2 cách như sau

```kotlin
for (x in goldMedals){
    println("${x.key} -> ${x.value}")
}

for ((key, value) in goldMedals){
    println("$key -> $value")
}
```