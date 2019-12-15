# Null safety - Xử lý null

## I. Lỗi null point exception

Lỗi null là một lỗi phổ biến trong lập trình, nó xảy ra nếu ta cố gắng gọi một phương thức hoặc truy cập một thuộc tính từ một đối tượng null. Lỗi xảy ra nếu ta bất cẩn trong việc thiếu kiểm tra an toàn khi lập trình, ví dụ

```java
public class Test {
    private Point a;

    public void method(){
        System.out.println(a.getX());
        System.out.println(a.getY());
    }
    
    public void methodXYZ(){
        //.....
    }
}
```

Ở hàm `method`, ta nghĩ rằng đối tượng `a` đã được gán ở đâu đó nên ta không kiểm tra null lỗi hoàn toàn có thể xảy ra.

Bên cạnh lỗi do lập trình viên bất cẩn, lỗi null xảy ra nhiều cũng một phần do bản chất ngôn ngữ Java không có các tính năng cần thiết để hạn chế lỗi xảy ra. Ngôn ngữ Java không cung cấp khả năng cho trình biên dịch biết khi nào là mã nguồn an toàn, khi nào là không an toàn với lỗi NullPointException.

Cũng chính điều đó đã đẩy việc xử lý lỗi lên lập trình viên. Nếu không kiểm tra kỹ thì dễ bị lỗi còn nếu kiểm tra kỹ quá thì mất thời gian, làm cho mã nguồn bị rối rắm quá mức.

Do đó, tính năng Null-Safety của Kotlin ra đời, mặc dù không thể giải quyết triệt để lỗi null point exception nhưng vẫn là một giải pháp tuyệt vời để xử lý lỗi.

## II. Null-Safety trong Kotlin

### 1. Kiểu Nullable và NotNullable

Tất cả những kiểu dữ liệu trong Kotlin đều có 2 dạng là Nullable và NotNullable. Cụ thể, những kiểu dữ liệu ta gặp trước đây đều là NotNullable, ví dụ

```kotlin
var x : Int = 10
var y : Double = 20.0
var point: Point = Point(1, 30)
```

Vì chúng là những kiểu dữ liệu NotNullable nên ta không thể gán chúng bằng những giá trị null


```kotlin
var x : Int = null          //Error
var y : Double = null       //Error
var point: Point = null     //Error
```

Vì không bao giờ null nên các câu lệnh check null đối với biến NotNullable là không cần thiết và IDE sẽ đưa ra cảnh báo nếu ta cứ làm điều đó

```kotlin

var x: Int = 20

//....

if( x != null){     //IDE sẽ cảnh báo câu lệnh if này bị dư
    //...
}


Kiểu nullable là kiểu tuỳ biến từ kiểu NotNullable bằng cách thêm dấu `?` vào sau, và vì là nullable nên biến có thể được gán giá trị null hoặc không null. 

Ví dụ:

```kotlin
var x : Int? = null             //Gán giá trị null
var y : Double? = 20.0           //Gán giá trị khác null
var point: Point? = null         //Gán giá trị null
```

### 2. Sử dụng biến Nullable

Ta có thể gán một giá trị của biến NotNullable vào biến Nullable một cách thoải mái.

```kotlin
var x: Int = 10
var y: Int? = null

//....

y = x   // Hợp lệ
```

Điều ngược lại không xảy ra nếu ta gán một biến Nullable vào biến NotNullable

```kotlin
var x: Int = 10
var y: Int? = null

//....

x = y   // Không hợp lệ
```

Để làm điều đó, ta bắt buộc phải có bước kiểm tra

```kotlin
var x: Int = 10
var y: Int? = null

//....

if(y != null){
    x = y   // Hợp lệ
}
```

Một cách tổng quát hơn, một biến nullable không được phép dùng để:

- Gán giá trị cho một biến NotNullable
- Gửi làm giá trị truyền khi gọi hàm mà tham số của hàm quy định là NotNullable
- Gọi bất kỳ một thành viên (hàm, thuộc tính...) nào đó của đối tượng

```kotlin
var obj: SomeClass? = null
//....

var x: SomeClass = SomeClass()  
x = obj                         //Error
obj.someMethod()                //Error
obj.field = xxx                 //Error
```

### 3. Smart cast

Quay lại ví dụ trên, tại sao câu lệnh gán là hợp lệ


```kotlin
var x: Int = 10
var y: Int? = null

//....

if(y != null){
    x = y   // Hợp lệ
}
```

Bởi vì nhờ có câu lệnh kiểm tra, trình biên dịch tạm coi biến y là biến NotNullable trong phạm vi của câu lệnh if. Hay nói cách khác, nó sẽ tương đương với thế này


```kotlin
var x: Int = 10
var y: Int? = null

//....

if(y != null){
    val yNotNull = y as Int
    x = yNotNull   // Hợp lệ
}
```

Cái này gọi là smart-cast, trình biên dịch biết lúc nào thì biến Nullable chắc chắn có giá trị không null và tự động cast sang biến NotNullable. 

Ví dụ thêm:

```kotlin
var x: Int = 10
var y: Int? = null

//....

if(y == null){
    return
}

x = y           //1
y.add(10)       //2

y = someNullableValue;      //3

y.add(10)       //4
//....
```

Trình biên dịch sẽ biết ở dòng //1, //2 y sẽ NotNull và nó tự động cast y về NotNullable.

Nhưng ở dòng //3, trình biên dịch không chắc chắn về giá trị y nên ở dòng //4, y quay về là biến Nullable và câu lệnh //4 đang bị lỗi

Trên đây, x, y là biến thuộc hàm, câu chuyện sẽ phức tạp hơn nếu x, y là thuộc tính của lớp. Ví dụ

```kotlin
class Abc{
    var x: Int = 10
    var y: Int? = null
    
    fun method(){
        if(y != null){
            x = y           //*
        }
    }
}
```

Nhìn có vẻ hợp lý, ta đã kiểm tra y != null nên y sẽ tự động được cast sang NotNullable Và lệnh gán (*) là hợp lý. Nhưng không, trình biên dịch sẽ không làm vậy. Nguyên nhân là ở lệnh gán đó, y vẫn có thể null bởi vì câu lệnh kiểm tra và câu lệnh gán dù được thực hiện ở 2 thời điểm rất gần nhau nhưng không chắc là liên tiếp nhau. Hoàn toàn có thể xảy ra tình huống có 1 thread chạy song song và làm sai biệt kết quả.

- Thời điểm t0: Thread hiện tại kiểm tra `y != null` đúng và đi vào khối lệnh if
- Thời điểm t1: Thread bất kỳ gán `y = null`
- Thời điểm t2: Thread hiện tại thực hiện `x = y`, lúc này y là null và làm cho x = null.

Để khắc phục điều đó, ta chỉ đơn giản là sử dụng một biến local

```kotlin
class Abc{
    var x: Int = 10
    var y: Int? = null

    fun method(){
        val _y = y

        if(_y != null){
            x = _y
        }
    }
}
```

Mọi thao tác kiểm tra null, gọi an toàn (phần dưới), phép khẳng định trên một biến NotNullable đều là thừa thãi, gây rối rắm và khó hiểu --> Nên tránh

Điều đó cũng được áp dụng với một biến nullable khi trình biên dịch đã áp dụng một smart-cast lên nó để thành một biến NotNullable 

### 4. Phép khẳng định

Xem ví dụ dưới đây:

```kotlin
class Abc{
    var x: Audio? = null

    fun loadData(){
        x = <.....>
    }

    fun doAction(){
        x!!.play("action.mp3")
    }

    fun die(){
        x!!.play("die.mp3")
    }

    fun dispose(){
        x!!.dispose()
        x = null
    }
}
```

Theo thiết kế khi lập trình, một đối tượng của lớp `Abc` luôn phải gọi hàm `loadData()` trước khi thực hiện các hàm `doAction()`, `die()`, `dispose()` và sau khi gọi hàm `dispose` thì sẽ không gọi hàm `doAction()`, `die()`.

Vì thế ta biết x sẽ không bao giờ null khi `x` gọi hàm `play()`, `dispose()`, vì vậy toán tử `!!` dùng để khẳng định `x` sẽ không null mặc dù kiểu của nó là Nullable.

Tất nhiên đây là một việc làm nguy hiểm, rủi ro nhưng ta biết rằng nếu lỗi null point xảy ra thật thì nguồn gốc lỗi là ở nơi khác, chương trình đã không hoạt động đúng thiết kế. Nếu ta kiểm tra null trước khi gọi `play` thì rõ ràng chương trình vẫn chạy, không bị crash nhưng không hề theo đúng ý muốn, không giải quyết được triệt để vấn đề.

### 5. laterinit var

Trở lại ví dụ trên, ta luôn chắc chắn là đối tượng `x` không null khi dùng đến nó, trước đó x chắc chắn sẽ được khởi tạo ở đâu đó.

Và nếu thêm một điểm nữa là không có lý do gì để ta gán `null` thì ta có thể dùng `laterinit`

```kotlin
class Abc{

    lateinit var x: Audio

    fun loadData(){
        //Khởi tạo x
        x = Audio()
    }

    fun doAction(){
        x.play("action.mp3")
    }

    fun die(){
        x.play("die.mp3")
    }

    fun release(){
        x.release()
    }
}
```

Laterinit cho trình biên dịch biết rằng biến NotNull này tạm thời chưa được khởi tạo nhưng ta cam đoan là sẽ được khởi tạo ở đâu đó trước khi đem ra sử dụng.

Tất nhiên `laterinit`:
- Chỉ áp dụng cho khiểu NotNullable (Ta đang cam đoan là không null mà)
- Chỉ áp dụng cho `var` (Chứ `val` thì làm sao ta gán giá trị sau khi khai báo được)

### 6. Gọi an toàn

Nhắc lại: khi ta muốn gọi một thành viên (hàm, thuộc tính) từ một giá trị nullable thì việc kiểm tra phải như thế này:

- Với biến local/param

```kotlin

var x : Point? = null

//....

if(x != null){
    x.method()
}
```

- Với property của lớp

```kotlin
class Abc{
    var x: Point? = null

    fun method(){
        val _x = this.x
        if(_x != null){
            _x.method()
        }
    }
}
```

Cả 2 dạng trên đều có thể sử dụng phép gọi an toàn 

```kotlin
x?.method()
```

Cú pháp gọi an toàn `?.` có nghĩa rằng hãy kiểm tra null trước, nếu khác null thì gọi, nếu null thì đừng.

Phép gán với `?.`

```kotlin
class A{

    fun method(): Int{
        return 0
    }
}

var x: A? = null

val y = x?.method()         //(*)
```

Ở dòng lệnh (*), mặc dù hàm `method()` có kiểu trả về là `Int` không null nhưng giá trị y nhận được lại không được đảm bảo do nếu `x = null` thì hàm không được gọi. Kiểu của biến `y` sẽ là `Int?`. Trong tình huống trên, `y = null`

Đoạn mã nguồn trên tương đương với 

```kotlin
class A{

    fun method(): Int{
        return 0
    }
}

var x: A? = null

var y: Int? = null

if(x != null){
    y = x.method()
}
```

Vậy nếu ta muốn:
- Nếu `x != null` thì y = x.method()
- Nếu `x == null` thì y = <một giá trị nào đó khác>

Ta sẽ dùng cú pháp `?:`, nó có cú pháp:

```kotlin
<giá trị nullable A> ?: <Giá trị B>
```

Ý nghĩa: Nếu A khác null thì lấy A, nếu `A == null` thì lấy B

và biếu thức mong muốn sẽ có dạng như thế này

```kotlin
y = x?.method() ?: <giá trị nào đó khác>
```

Nó tương đương với

```kotlin
y = if(x != null) x.method() else <giá trị nào đó khác>
```

Khi đó biến y sẽ có kiểu NotNullable.

Nhìn cú pháp của lệnh `if` có vẻ rõ ràng dễ đọc hơn mà cũng không có quá dài, tuy nhiên hãy dùng cú pháp `?.` ... `?:`. 

Ví dụ ta có thể viết

```kotlin
val x = (a?.method()?.method2()?.method3()?.method4()) ?: 0
```

thay vì cả một núi lệnh if lồng nhau.

