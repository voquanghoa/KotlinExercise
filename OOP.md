# OOP - Lập trình hướng đối tượng

Hướng đối tượng trên Kotlin về cơ bản là không khác quá nhiều trên Java. Những khái niệm cũ Class, Object, Kế thừa, Interface, Overloading, Override ... đều được giữ nguyên.

Dưới đây liệt kê những điểm về hướng đối tượng, nhắc lại (không chi tiết) và nói tới những điểm thay đổi trên kotlin.

## 1. Lớp và đối tượng

Lớp là khuôn mẫu để tạo các đối tượng, đối tượng là thự thể của lớp.

Khai báo lớp trong Kotlin giống với cách khai báo trên Java

```kotlin
class Point{

}
```

Tạo đối tượng cũng giống với cách tạo đối tượng trên Java, cũng gọi tới 1 constructor với các tham số, điểm khác biệt duy nhất từ khoá `new` bị loại bỏ.

```kotlin
// Java 
Point point = new Point();

// Kotlin
val point = Point()
```

## 2. Trường (field) và thuộc tính (property)

Với Java, tất cả các biến thuộc lớp đều là private và việc đọc ghi cần phải thông qua các hàm get/set. 

Với Kotlin, các thuộc tính được khai báo thông qua `val/var` như khai báo biến thuộc lớp.


```kotlin
//Java style

public class Point{

    private int x;
    private int y;

    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }

    public int getY() {
        return y;
    }

    public void setY(int y) {
        this.y = y;
    }
}

// Kotlin style

class Point{
    var x: Int = 0
    var y: Int = 0
}
```

Setter/Getter delegator và back field

Xem ví dụ sau

```java
class Circle{
    private int r;

    public double getArea(){
        return Math.PI * r * r;
    }

    public  double getPerimeter(){
        return Math.PI * r * 2;
    }
    
    public int getR() {
        return r;
    }

    public void setR(int r) {
        if(r < 0) throw new RuntimeException("Invalid r " + r);

        this.r = r;
    }
}
```

Ở đây, lớp Circle có:

- Thuộc tính `r` với `get/set` nhưng hàm `set` kiểm tra tính hợp lệ của giá trị `r` mới
- Thuộc tính `area` và `perimeter` chỉ đọc (chỉ có hàm get nhưng không có set)

Với Kotlin, ta có thể làm điều tương tự

```kotlin
class Circle {
    var r : Int = 0
        set(value){
            if (value < 0) throw RuntimeException("Invalid r $value")
            field = value
        }

    val area: Double
        get() = Math.PI * r * r

    val perimeter: Double
        get() = Math.PI * r * 2
}
```

Ở đây

```kotlin
set(value){
    if (value < 0) throw RuntimeException("Invalid r $value")
    field = value
}
```

Là `set delegator`: nó được gọi tự động khi ta gán giá trị cho thuộc tính

```kotlin
get() = Math.PI * r * 2
```

Là `get delegator`: nó được gọi khi ta đọc giá trị thuộc tính

`field` là biến thực tế lưu trữ giá trị của thuộc tính. Nó là `context keyword` và chỉ tồn tại trong khối `get delegator` và `set delegator`


## 3. Constructor

Khái niệm constructor trên Kotlin cũng giống trên Java, đều là điểm vào để tạo đối tượng.

Tuy nhiên, Kotlin sẽ dùng từ khoá `constructor` thay vì tên lớp

```kotlin
//Java style

public class Point{

    private int x;
    private int y;

    public Point(int x, int y){
        this.x = x;
        this.y = y;
    }

    public int getX() {
        return x;
    }

    public void setX(int x) {
        this.x = x;
    }

    public int getY() {
        return y;
    }

    public void setY(int y) {
        this.y = y;
    }
}

// Kotlin style

class Point{
    var x: Int = 0
    var y: Int = 0
    
    constructor(x: Int, y: Int){
        this.x = x
        this.y = y
    }
}
```

Tuy nhiên, Kotlin có thêm khái niệm `primary constructor`, cách khai báo nó sẽ hơi khác và điều thú vị là ta có thể khai báo luôn các thuộc tính của lớp một cách gọn gàng. Ví dụ, lớp Point trên kia ta có thể khai báo ngắn gọn thế này

```kotlin
class Point(var x: Int, var y: Int)
```

Ta vẫn có thể thay đổi thuộc tính x, y để trở thành thuộc tính chỉ đọc

```kotlin
class Point(val x: Int, val y: Int)
```

Hoặc trở thành trường private

```kotlin
class Point(private val x: Int, private val y: Int)
```

Cùng 1 class, ta có thể có cả 1 `primary constructor` và vài `secondary constructor`. Khi đó, chỉ có `primary constructor` được khai báo các thuộc tính và các `secondary constructor` phải có lời gọi tới `primary constructor` bằng từ khoá `this` (trực tiếp hoặc gián tiếp)

```kotlin
class Point(val x: Int, val y: Int){
    constructor():this(0,0){    //<-- gọi tới primary constructor
        //some code
    }
}
```

## 4. Khối init

Tưởng tượng một class Java có vài constructor và ngay sau tạo đối tượng, chúng đều cần phải gọi method `loadData()`

```java
class Test{
    private int x;
    private int y;

    public Test(){
        this.x = 0;
        this.y = 0;
        loadData();
    }

    public Test(int x, int y){
        this.x = x;
        this.y = y;
        loadData();
    }

    public Test(int value){
        this.x = this.y = value;
        loadData();
    }

    private void loadData(){
        
    }
}
```

Thì với Kotlin, mọi chuyện thật đơn giản với khối `init`

```kotlin
class Test(var x: Int, var y: Int){
    constructor(value: Int): this(value, value)
    constructor():this(0)
    
    init{
        loadData() // Chỉ cần gọi 1 lần
    }

    private fun loadData(){
        
    }
}
```

Khối `init` sẽ tự động được gọi sau khi `constructor` được gọi xong.

## 5. Overloading

Khái niệm, cách sử dụng Overloading trên Kotlin là giống với Java

```kotlin

// Java
class Number{
    public String toString(){
        ///
    }
    
    public String toString(int base){
        
    }
}

//Kotlin

class Number{
    fun toString(): String{
        //...
    }

    fun toString(base: Int): String{

    }
}
```

Tuy nhiên, kotlin có hỗ trợ default parametter nên câu chuyện đơn giản hơn chút

```kotlin
class Number{
    fun toString(base: Int = 10): String{

    }
}
```


## 6. Abstract class, abstract method và interface

Các khái niệm trên hoàn toàn giống Java.

Duy nhất một lưu ý là với interface, ta thường dùng nó để mô tả một action mà khái niệm action đã được cung cấp một cách tiện lợi hơn thông qua các `block`.

Vì vậy, mục đích sử dụng của `interface` đã bớt đi rất nhiều. Ta chỉ nên dùng `interface` khi dự án có sử dụng một số mã nguồn hoặc thư viện được implement bằng java và bắt buộc ta dùng interface.

## 7. Kế thừa và cài đặt

Java sử dụng 2 từ khoá `extend` và `implements` để mô tả sự kế thừa và cài đặt. Với kotlin, ta chỉ cần dùng toán tử `:` là đủ. Tuy nhiên, nếu một class vừa kế thừa 1 lớp và vừa cài đặt vài interfaces thì tên của class cha bắt buộc phải ở đầu danh sách

```kotlin

// Java style
class A{

}

interface X{

}

interface Y{

}

class MyClass extend A implement X, Y{

}

// Kotlin style

open class A

interface X
interface Y

class MyClass : A(), X, Y //A là class nên phải đứng trước X, Y
```

Class trong kotlin mặc định là `final`, không được phép kế thừa, do vậy ta phải thêm từ khoá `open` nếu muốn khai báo class là `non-final`

Ta cũng cần phải gọi ra constructor của class cha `()`

## 8. Override

Override là hành động ghi đè một method từ class cha.

Với kotlin:

- Hàm bị ghi đè phải có modifier `open` vì mặc định là `final`
- Hàm ghi đè phải có từ khoá `override` để làm mã nguồn rõ ràng

```kotlin
open class A{
    open fun somethod(){

    }
}

class MyClass: A(){
    override fun somethod() {
        super.somethod() //Gọi method cha nếu cần
    }
}
```

## 9. Data class

Từ khoá `data` bổ nghĩa cho 1 class thì class đó sẽ có những khả năng sau

1. Tự implement hàm `equals` và `hasCode` để 2 đối tượng khi so sánh với nhau, chúng so sánh theo giá trị thay vì theo thực tế instance

Ví dụ

```kotlin

// Non data

class Point(var x: Int, var y: Int)

val point1 = Point(1, 2)
val point2 = Point(1, 2)

println(point1 == point2) //false

// With data

data class Point(var x: Int, var y: Int)

val point1 = Point(1, 2)
val point2 = Point(1, 2)

println(point1 == point2) //true
```

Điều này càng cực kỳ hữu ích khi ta tìm kiếm một đối tượng bên trong một tập hợp.

2. Hàm `toString` sẽ cho kết quả tường minh, hữu ích hơn

```kotlin
class Point(var x: Int, var y: Int)
val point1 = Point(1, 2)
println(point1) //DemoKt$main$Point@42110406

//VS


data class Point(var x: Int, var y: Int)
val point1 = Point(1, 2)
println(point1) //Point(x=1, y=2)
```

3. Thao tác copy

Ta sẽ không cần phải đi copy giá trị của từng thuộc tính nữa vì giờ mọi thứ là tự động

```kotlin
data class Point(var x: Int, var y: Int)
val point1 = Point(1, 2) 
val point2 = point1.copy()
println(point2) //Point(x=1, y=2)
```

4. Sử dụng dạng pair (hoặc trible, thậm chí hơn)

```kotlin
data class Point(var x: Int, var y: Int)

val points = listOf(
        Point(1, 2),
        Point(3, 4),
        Point(8, 1),
        Point(5, 3)
)

for((x, y) in points){
    println("Point $x -> $y")
}
```

Output

```
Point 1 -> 2
Point 3 -> 4
Point 8 -> 1
Point 5 -> 3
```

Hoặc 

```kotlin
class Point(var x: Int, var y: Int)
val point = Point(3, 4)

val x = point.x
val y = point.y

println("$x, $y")
```

Tương đương với


```kotlin
class Point(var x: Int, var y: Int)
val point = Point(3, 4)

val (x, y) = point //Khai báo và gán 1 biến 1 lúc

println("$x, $y")
```

## 10. Local class và Inner class 

Nested class đơn thuần là class được khai báo bên trong 1 class

```kotlin
class A{
    class Nested{
    }
}
```

Ta có thể khởi tạo đối tượng của lớp `Nested` bất kỳ đâu, bên ngoài hay bên trong lớp A

```kotlin
class A{
    class Local{
    }
    
    fun method(){
        val local = Local()
    }
}

class B{
    fun method(){
        val local = A.Local()
    }
}
```

Inner class là class được khai báo bên trong 1 class với modifier `inner` hoặc được khai báo bên trong một method. Inner class sử dụng lớp cha hoặc sâu hơn nữa là method chứa nó làm scope, vì thế, nó được toàn quyền sử dụng các tài nguyên mà lớp chứa nó (hoặc cả hàm chứa nó) có

```kotlin
class A{
    var x = 10

    inner class Inner{
        fun method(){
            println(x) // Hợp lệ
        }
    }
}
```

Tuy nhiên:

- Nếu class `Inner` được khai báo trong 1 hàm thì chỉ có hàm chứa nó được khởi tạo đối tượng

```kotlin
class A{
    var x = 10

    inner class Inner{
        fun method(){
            println(x) // Hợp lệ
        }
    }

    fun methodA(){
        val obj = Inner()
    }
}
```

- Nếu class `Inner` được khai báo trong 1 lớp thì chỉ có lớp chứa nó được khởi tạo đối tượng

```kotlin
class A{
    var x = 10

    fun methodA(){

        class Inner{
        }

        val obj = Inner()
    }
}
```

## 10. Anonymous class

Anonymous class là class được khai báo trong một method thông qua việc kế thừa một lớp khác và override hàm hoặc cài đặt 1 interface và implement hàm. Việc làm đó tuy tạo class mới nhưng không đặt tên cho class đó nên gọi là `Anonymous class`

```kotlin
interface CanDoAction{
    fun doAction()
}

class A{

    fun method(){
        consume(object: CanDoAction{
            override fun doAction() {
                println("I'm doing")
            }
        })
    }

    fun consume(action: CanDoAction){
        action.doAction()
    }
}
```

Nếu ta đang implement một interface và interface đó chỉ có duy nhất 1 method cần cài đặt thì Kotlin cho phép ta tạo block tương ứng thay vì dùng anonymous class

Code implement bằng Java

CanDoAction.java

```java
public interface CanDoAction{
    void doAction();
}
```

Actor.java

```java
public class Actor{
    public void doAction(CanDoAction action){

    }
}
```

Phía Kotlin

```kotlin
fun anyMethod(){
    val actor = Actor()

    actor.doAction{
        println("Hello")
    }
}
```

## 11. Object

Nếu java có khái niệm singleton và đó chỉ là khái niệm vì java không hỗ trợ nó sẵn mà ta phải tự implement

```java
public class Actor{

    private Actor(){
    }

    private static Actor instance;

    public static synchronized Actor getInstance(){
        if(instance == null){
            instance = new Actor();
        }
        return instance;
    }

    // Other methods, properties...
}
```

Dùng nó

```java
Actor.getInstance().someMethod()
```

Thì Kotlin hỗ trợ nó sẵn và mọi thứ ta cần làm là

```kotlin
object Actor{
    // Other methods, properties...
}
```

Dùng nó

```java
Actor.someMethod()
```

## 12. Static

Kotlin không có từ khoá static, nó đã có khái niệm Object sẵn ở trên. Trong vài tình huống khác, ta có thể sử dụng đối tượng companion.

```kotlin
class A{
    var name: String = ""

    init {
        objectCount ++
    }
    companion object{
        // Mọi thứ ở đây đều giống static bên Java
        var objectCount = 0
    }
}

class B{
    fun method(){
        A()
        A()
        A()
        //Gọi objectCount từ lớp A thực chất là đang gọi tới objectCount từ object companion của lớp A
        println(A.objectCount)
    }
}
```

companion là đối tượng đại diện cho lớp, ta có thể khai báo các properties, method cho nó và cách dùng nó thì hoàn toàn giống các properties, methods static của java.

