# Loop Control Flow: Điều khiển vòng lặp

## Vòng lặp while và do while

Tương tự Java, vòng lặp while và do while của Kotlin sẽ tiến hành thực hiện một công việc nhiều lần dựa theo điều kiện

#### while
```kotlin
var x = 1
while (x % 10 != 0){
    println(x)
    x += 3
}
```

#### do while

```kotlin
var y = 1
do {
    println(y)
    y += 4
}while (y % 10 != 0)
```

Ta cũng sẽ có các từ khoá break, continue để điều kiển khối lệnh

## Vòng lặp for

Vòng lặp for hoạt động dựa theo một biến đếm chạy trong một Iterator, ví dụ

```kotlin
val x = listOf(1, 2, 3, 8, 20)

for(i in x){
    println(i) //Lặp các giá trị trong danh sách x
}
```

Ở đây x là một danh sách các số nguyên và đồng thời là một iterator.

Dưới đây là một số dạng thường gặp về cách dùng vòng lặp for, vì bản chất vòng lặp for hoạt động dựa trên iterator nên cách dùng vòng for có thể ở rất nhiều tình huống khác nữa.

### Lặp theo các phần tử của mảng, list, chuỗi ký tự, set ...

Kiểu lặp này giống kiểu lặp for-each của Java

```kotlin
val x = "Vietnam"

for(i in x){
    println(i)
}

val arr = arrayOf(1, 3, 2, 29, 23)

for(i in arr){
    println(i)
}

for(i in 0 .. 10){ // Từ 0 đến 10
    println("$i * $i = ${i*i}")
}

for(i in 0 until 10){ // Từ 0 đến 9
    println("$i * $i = ${i*i}")
}
```

### Lặp theo chỉ số của mảng, list, chuỗi ký tự, set ...

- indices là một thuộc tính tương đương với 0 .. size - 1 (hay 0 until size)

```kotlin

val arr = arrayOf(1, 3, 2, 29, 23)

for(i in arr.indices){
    println("Arr[$i] = ${arr[i]}")
}
```

Lệnh trên tương đương với

```kotlin
val arr = arrayOf(1, 3, 2, 29, 23)

for(i in 0 until arr.size){
    println("Arr[$i] = ${arr[i]}")
}
```

Nhưng cách đầu tiên được khuyến khích hơn

### Lặp tăng dần, giảm dần

Cú pháp

```kotlin
for(i in 0 until 10){

}
```

Là tương đương với vòng lặp for thường thấy của Java

```java
for(int i = 0; i < 10; i++){

}
```

Theo đó, biến i sẽ có một giá trị bắt đầu, thực hiện công việc và tăng dần 1 đơn vị cho đến khi đạt giá trị tối đa.

Nếu thay vì tăng lên 1 giá trị, ta muốn tăng nhanh hơn thì có thể bổ sung `step`, ví dụ

```kotlin
for(i in 0 until 10 step 2){ // 0, 2, 4, 6, 8

}
```

Với step âm, ta sẽ có biến i giảm dần nhưng code sẽ hơi ngu, thay vào đó, ta dùng mệnh đề `downTo`

```kotlin
for(i in 10 downTo 0){
    
}
```
