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

### 1. Hàm run và let

Hàm run là môt extension method và nó đơn giản là sử dụng đối tượng invoke để truyền dưới dạng một đối tượng `this` vào scope.

Ví dụ

Thay vì viết

```kotlin
val x = Object()

x.methodA()
x.methodB()
```

Ta có thể viết

```kotlin
Object().run{
    methodA()
    methodB()
}
```

Nó thường được dùng để check null cho một giá trị rồi thực hiện một chuỗi các thao tác trên giá trị đó.

Ví dụ

```kotlin
var x = someMethod()

if(x != null){
    x.method1()
    x.method2()
    x.method3()
}
```

Ta có thể viết

```kotlin
someMethod()?.let{
    method1()
    method2()
    method3()
}
```

Hàm `let` tương tự hàm `run` nhưng đối tượng invoke sẽ được gửi vào bằng từ khoá `this` thay vì là `it`

```kotlin
someMethod()?.let{
    it.method1()
    it.method2()
    it.method3()
}
```

### 2. Hàm apply và also

Hàm apply là một extension method và nó sử dụng đối tượng invoke để truyền vào scope dưới dạng từ khoá `this`. Cuối cùng scope trả về chính đối tượng đó

Hàm apply có thể sử dụng như hàm `run` nhưng với việc hàm apply trả về giá trị invoke, nó có thể được sử dụng như một chuỗi các lệnh gọi

Ví dụ:

```kotlin
class A{
    var x = 0
    var y = 0
    var z = 0

    fun method1(){}
    fun method2(){}
}

class Test{
    fun test(){
        val a = A()

        a.x = 10
        a.y = 20
        a.z = 30

        a.method1()
        a.method2()
        
        otherMethod(a)
    }

    fun otherMethod(a: A){
        //Something
    }
}
```

Ở hàm Test.test, ta tạo một object kiểu A, khởi tạo giá trị các fields, gọi hàm trước khi gửi nó cho hàm `otherMethod`

Thao tác trên có thể viết gọn bằng hàm apply như sau

```kotlin

class Test{
    fun test(){
        otherMethod(A().apply {
            this.x = 10
            this.y = 20
            this.z = 30
            this.method1()
            this.method2()
        })
    }

    fun otherMethod(a: A){
        //Something
    }
}
```

Ở đây, `this` chính là `A()` thay vì là đối tượng Test. Thậm chí, `this.` còn có thể được bỏ qua

```kotlin
otherMethod(A().apply {
    x = 10
    y = 20
    z = 30
    method1()
    method2()
})
```

Hàm also tương tự hàm apply nhưng đối tượng invoke được truyền vào scope dưới dạng biến `it` thay vì `this`.

Tại sao phải rắc rối giữa `it` và `this` trong cả tình huống `run` với `let` và `apply` với `also`.

Xem ví dụ

```kotlin
class A{
    fun method1(){} //(1)
}

class Test{
    
    fun method1(){}     //(2)
    fun test(){
        otherMethod(A().apply {
            method1()
        })
    }

    fun otherMethod(a: A){
        //Something
    }
}
```

Khi ta dùng `apply`, lời gọi hàm `method1()` sẽ gọi tới hàm (1) vì `this` đang trỏ tới đối tượng `A()`, kể cả là viết `method1()` hay `this.method1()` thì cũng chung kết quả.

Nếu đây là điều ta mong muốn thì quá ok, vừa gọi đúng hàm, vừa tiện. Nhưng nếu không phải điều ta mong muốn thì ... hơi rắc rối, có lẽ nên dùng phiên bản `it`

Khi ta dùng `also`

```kotlin

class A{
    fun method1(){}     //1
}

class Test{

    fun method1(){}     //2
    fun test(){
        otherMethod(A().also {
            method1()
            it.method1()
        })
    }

    fun otherMethod(a: A){
        //Something
    }
}
```

Việc gọi `method1()` là trỏ tới hàm (2) vì `this` đang là đối tượng trên lớp `Test`. Hàm (1) vẫn có thể được gọi thông qua `it` dễ dàng.

### 3. Hàm with

Hàm with đơn giản là dùng để nhóm các câu lệnh để tăng tính dễ đọc của code

Ví dụ ta có đống code như thế này

```kotlin
x.a = 1
x.b = 20
x.method1()
x.method2()

y.a = 20
y.b = 22
y.method3()
y.method4()
```

Ta có thể viết thế này

```kotlin
with(x){
    a = 1
    b = 20
    method1()
    method2()
}

with(y){
    a = 20
    b = 22
    method3()
    method4()
}
```