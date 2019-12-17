# I. Collection - Các kiểu Tập hợp

Tập hợp trên `Kotlin` thật ra là sự kế thừa từ kiểu dữ liệu `ArrayList` từ `Java`. Đây là kiểu dữ liệu sử dụng mảng một cách linh động để dữ liệu có thể được thêm vào hay bớt đi tuỳ ý.

## 1. Kiểu List

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

## 2. Kiểu MutableList

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
- Method `addAll(..)`: thêm một danh sách khác
- Method `remove(x)`: xoá bỏ một phần tử nhất định
- Method `removeAt()`: Xoá bỏ phần thử ở vị trí
- Method `clear()`: xoá toàn bộ danh sách
- ....

Tip: Các phương thức thêm/xoá phần thử có thể được thực hiện bằng các toán tử `+`, `-`, `+=`, `-=`.

## 3. Kiểu Set và MutableSet

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

## 4. Kiểu Map và MutableMap

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

# II. Thao tác trên tập hợp

## 1. Thao tác forEach

Thao tác `forEach` thật ra đơn giản là một vòng for duyệt trên tất cả các phần tử tập hợp

```kotlin
val list = listOf(1, 2, 3, 4, 5)

list.forEach { println(it) }
```

## 2. Thao tác chuyển đổi (transform)

Thao tác `transform` nhằn chuyển đổi dữ liệu từ dạng gốc sang một dạng khác.

- map

Hàm `map` cho phép chuyển đổi từng phần tử trong collection gốc từng phần tử mới trong collection mới.

```kotlin
val strs = "1 2 3 4 5"
val stringNumbers = strs.split(" ") // ("1", "2", "3", "4")

val numbers = stringNumbers.map { it.toInt() } //(1, 2, 3, 4)
```

Ngoài ra:
- `mapIndexed`: bổ sung thông tin chỉ số vào lệnh `map`
- `mapNotNull`: chỉ map trên các phần tử khác null
- ...

- flatten

Hàm `flatten` thực hiện lấy tất cả các phần tử trên tất cả các collection để cho 1 collection duy nhất

```kotlin
val numberSets = listOf(listOf(1, 2, 3), setOf(4, 5, 6), setOf(7, 8, 9))
println(numberSets.flatten())   //(1, 2, 3, 4, 5, 6, 7, 8, 9)
```

## 3. Thao tác chọn lọc

Hàm `filter` thực hiện tìm kiếm các phần tử trên collection thoả mãn một điều kiện nhất định

```kotlin
val numbersAndStrings = listOf("one", 10, 20, "two", 40)
println(numbersAndStrings.filter { it is String })

val numbers = mutableListOf(1, 2, 9, 10, 30, 22, 71)
println(numbers.filter { it % 2 == 0 })

val strings = setOf("One", "Two", "Three", "Four", "Seven")
println(strings.filter { it.length == 4 })
```

## 4. Thao tác trên đoạn

- Thao tác Slice: Lấy các phần tử trong phạm vi

```kotlin
val numbers = listOf("zero", "one", "two", "three", "four", "five", "six")
println(numbers.slice(2..5))
```

- Thao tác take và takeLast: Lấy một vài phần tử ở đầu hoặc ở cuối

```kotlin
val numbers = listOf("zero", "one", "two", "three", "four", "five", "six")
println(numbers.take(3)) //zero, one, two
println(numbers.takeLast(2)) //five, six
```

- Thao tác drop và dropLast: Ngược lại với take và takeLast: Lấy các phần tử ngoại trừ một vài phần tử ở đầu hoặc ở cuối

```kotlin
val numbers = listOf("zero", "one", "two", "three", "four", "five", "six")
println(numbers.drop(3)) //three, four, five, six
println(numbers.dropLast(2)) //zero, one, two, three, four
```

- Thao tác takeWhile: Nếu phần tử đầu tiên thoả mãn 1 điều kiện thì lấy nó và các phần tử liên tiếp sau nó chừng nào điều kiện còn đúng

```kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6, 7, 8)
println(numbers.takeWhile { it % 4 != 0 })  // 1, 2, 3
```

Khác với lệnh `filter`, lệnh `takeWhile` khi gặp phần tử đầu tiên không thoả mãn điều kiện là kết thúc tìm kiếm luôn

- Tương tự cho các lệnh `takeLastWhile`, `dropWhile`, `dropLastWhile`

## 4. Thao tác lấy 1 phần tử:

- Lấy 1 phần tử theo chỉ số: sử dụng toán tử `[]`, ngoài ra:
    - `elementAtOrNull` --> Nếu chỉ số sai (ngoài phạm vi) thì trả về `null` thay vì báo lỗi
    - `elementAtOrElse` --> Nếu chỉ số sai thì lấy giá trị mặc định bởi block
- Lấy 1 phần tử thoả mãn 1 điều kiện:
    - `first`, `last` --> Lấy phần tử đầu tiên hoặc cuối cùng thoả mãn 1 điều kiện. Nếu không tìm thấy thì báo lỗi

```kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6, 7, 8)

println(numbers.first()) //1
println(numbers.last())  //8

println(numbers.first { it * 7 > 30 }) //5
println(numbers.last { it > 20 }) //Error
```
-  `firstOrNull`, `lastOrNull` --> Tương tự `first`, `last` nhưng thay vì báo lỗi khi không tìm thấy thì chỉ đơn giản là trả về `null`

- `random` --> Lấy ra 1 phần tử ngẫu nhiên 

## 5. Sắp xếp: 

- Hàm `sorted` và `sortedByDescending`: Sắp xếp tập hợp trên cơ sở so sánh có sẵn: với số thì so sánh theo giá trị, với chuỗi string là theo mã các chữ cái...

```kotlin
val numbers = listOf(8, 6, 5, 4, 1, 2, 3, 7)
println(numbers.sortedDescending())     //[8, 7, 6, 5, 4, 3, 2, 1]

val strings = listOf("zero", "one", "two", "three", "four", "five", "six")
println(strings.sorted()) //[five, four, one, six, three, two, zero]
```

- Hàm `sorted` và `sortedByDescending` với block: Sắp xếp collection theo điều kiện

```kotlin
val countries = listOf("Indonesia",
        "Malaysia",
        "Philippines",
        "Singapore",
        "Thailand",
        "Brunei",
        "Laos",
        "Myanmar",
        "Cambodia",
        "Vietnam")

countries.sortedBy { it.length }
        .forEach { println(it) }
```

Result

```
Laos
Brunei
Myanmar
Vietnam
Malaysia
Thailand
Cambodia
Indonesia
Singapore
Philippines
```

- Hàm `shuffled`: xáo trộn ngẫu nhiên các phần tử trên danh sách
- Hàm `reversed`: Đảo ngược thứ tự các phần tử trên danh sách

Ngoài ra, ta cũng có các phiên bản của các hàm trên nhưng chỉ tồn tại trên các collection kiểu mutable: `sort`, `sortDescending`, `sortBy`, `sortByDescending`, `shuffle`, `reverse`. Các hàm này sắp xếp trực tiếp trên collection hiện tại thay vì sắp xếp trên bản copy.

## 6. Tổng hợp:

Các hàm tính toán trực tiếp:

- `min()`, `max()`: trả về giá trị nhỏ nhất, lớn nhất
- `average()`: trả về giá trị trung bình
- `sum()`: trả về giá trị tổng
- `count()`: trả về số lượng giá trị (giống với `size`)

```kotlin
val values = listOf(3, 10, 60, 20, 33, 89, 34)
println(values.min())
println(values.max())
println(values.average())
println(values.sum())
println(values.count())
```

Các hàm tính toán thông qua `delegator`

- `minBy`: tìm giá trị nhỏ nhất theo điều kiện
- `maxBy`: tìm giá trị lớn nhất theo điều kiện
- `sumBy`: tính tổng theo điều kiện
- `count`: tìm số lượng phần tử thoả mãn điều kiện

```kotlin
val countries = mutableListOf(
        "Indonesia",
        "Malaysia",
        "Philippines",
        "Singapore",
        "Thailand",
        "Brunei",
        "Laos",
        "Myanmar",
        "Cambodia",
        "Vietnam")

println(countries.minBy { it.length }) //Laos
println(countries.count { it.length == 7 }) //2
```

Hàm `reduce`. Tương đối phổ biến, khi lập trình, ta thực hiện các thao tác kiểu như thế này:

```kotlin

val v = <some value>

for(i in values){
    v = <thao tác giữa v và i>
}

```

Ví dụ: tính tổng các phần tử trong mảng

```kotlin

val sum = 0

for(i in array){
    sum = sum + i //hay sum += i
}

```

Tất nhiên thao tác trên ta đã có hàm `sum` được implement sẵn nhưng chẳng hạn cần tính tích các số chẳng hạn thì chưa có. 

Ta cần sửa thế nào để đoạn code trên trở thành tính tích các số?

```kotlin

val sum = 0

for(i in array){
    sum = sum * i // Thay phép + thành phép * ?
}

```

Nếu chỉ đơn giản là thay phép `+` thành phép `*`, có vẻ hợp lý nhưng thực tế thì không hẳn, chạy thử sẽ ra giá trị của `sum` sau vòng lặp sẽ luôn là `0`. 

Cần phải sửa 

```kotlin
val sum = 0
```

Thành

```kotlin
val sum = 1
``` 

Nữa.

Để nó đúng trong mọi trường hợp, vòng lặp trên kia

```kotlin

val v = <some value>

for(i in values){
    v = <thao tác giữa v và i>
}

```

Cần phải sửa nó thành

```kotlin
val v = values.first()

for(i in values.skip(1)){
    v = <thao tác giữa v và i>
}
```

Biến `v` ở đây là một `accumulates value` và sẽ được đổi tên thành `acc`

```kotlin
val acc = values.first()

for(i in values.skip(1)){
    acc = <thao tác giữa acc và i>
}

```

Cách thức trên là cách hàm `reduce` hoạt động, xem ví dụ

```kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6,7 )

println("Tổng các số là ${numbers.reduce { acc, i -> acc + i }}")
println("Tích các số là ${numbers.reduce { acc, i -> acc * i }}")
```

- Hàm `joinToString`

Ví dụ nếu ta có một collection các số, ta cần build 1 chuỗi các giá trị và đặt khoảng trắng vào giữa thì hơi rắc rối, ta cần làm thế này

```kotlin

val sb = StringBuilder()

for(i in values.indicates){
    sb.append(values[i])

    if(i < values.size - 1){
        sb.append(' ')
    }
}

return sb.toString()
```

Nhưng kotlin cung cấp sẵn hàm `joinToString` để sử dụng như sau

```kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6,7 )

println(numbers.joinToString())     //Mặc định là ", "
println(numbers.joinToString(" + "))
println(numbers.joinToString(" -> "))
```

Output

```
1, 2, 3, 4, 5, 6, 7
1 + 2 + 3 + 4 + 5 + 6 + 7
1 -> 2 -> 3 -> 4 -> 5 -> 6 -> 7
```


## 7. Gộp nhóm

Gộp nhóm là thao tác biến một collection thành một Map<key, values> với key là khoá gộp, values là các giá trị có cùng key. 

Ví dụ: gộp các số có cùng số dư khi chia cho 3

```kotlin
val numbers = listOf(1, 2, 3, 4, 5, 6, 7, 8 )
    
println(numbers.groupBy { it % 3 })
```

Output

```
{1=[1, 4, 7], 2=[2, 5, 8], 0=[3, 6]}
```

Gộp các string theo length

```kotlin
val countries = mutableListOf(
        "Indonesia",
        "Malaysia",
        "Philippines",
        "Singapore",
        "Thailand",
        "Brunei",
        "Laos",
        "Myanmar",
        "Cambodia",
        "Vietnam")

println(countries.groupBy { it.length })
```

Output

``
{9=[Indonesia, Singapore], 8=[Malaysia, Thailand, Cambodia], 11=[Philippines], 6=[Brunei], 4=[Laos], 7=[Myanmar, Vietnam]}
```