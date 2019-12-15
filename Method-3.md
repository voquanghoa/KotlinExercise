# Method - Lập trình hàm

## VII. Scoped function - Hàm phạm vi

Scope là gì: Scope là phạm vi của một hàm, một block. Một scope có thể thuộc về 1 scope cha và có thể truy cập được từ scope cha nhưng không có ngược lại.

Xem ví dụ dưới đây


```kotlin
class Test{
    fun method(){
        var x = 10              //1
        var z = 50              //2

        val block = {
            var x = 20          //3
            var y = 30          //4
            println(x)          //5
            println(y)          //6
            println(z)          //7
            println(this)       //8
        }
        
        println(this)           //9
    }
}
```

Ở đây, khối `block` thuộc hàm `method` và:

- Nó dùng được biến x, y (dòng 3, 4) vì là biến khai báo trong nội bộ block
- Nó dùng được biến z thuộc scope của hàm `method`
- Biến x (dòng 1) của hàm `method` bị biến x (dòng 3) che khuất nên không truy cập được
- Câu lệnh 8 và 9 là hoàn toàn giống nhau vì từ khoá `this` đang cùng trỏ chung 1 đối tượng

Tất cả những điều trên là do scoped quy định và ta có một số hàm thay đổi scope, vì vậy những điều trên không phải lúc nào cũng đúng. Xem các hàm scope dưới đây và tập cách sử dụng

### 1. Hàm let

Hàm let là môt extension method và nó đơn giản là sử dụng đối tượng hiện tại để truyền dưới dạng một đối tượng `it` vào scope.

Ví dụ

```kotlin
1.let {
    println(it)         //it chính là 1
    println(it * 2)
}
```

