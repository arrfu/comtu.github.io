---
layout : post
title : "iOS swift3 闭包的用法总结"
category : iOS
duoshuo: true
date : 2016-11-22
tags : [iOS ,swift,closure]
---

#一、什么是闭包：
  * 闭包是自包含的函数代码块，可以在代码中被传递和使用。   
  * Swift 中的闭包与 C 和 Objective-C 中的代码块（blocks）以及其他一些编程语言中的匿名函数比较相似。   
  
#二、闭包的语法表达式：

<pre class="brush: swift;  ">
 { (parameters/接收的参数) -> (return type/闭包返回值类型) in
 statements/保存在闭包中需要执行的代码
 }
 </pre>
 
#三、闭包的使用例子：
 
* 闭包形式1
<pre class="brush: swift;  ">
func testClosure1(){
    // 1.typealias关键字的使用，跟oc中的typedef类似
    typealias ClosureAdd = (_ num1:Int,_ num2:Int) -> Int // 给闭包起别名为：ClosureAdd
    let closureAdd:ClosureAdd = {
        (_ num1:Int,_ num2:Int) in
        return num1+num2
    }
    
    let reslut = closureAdd(2, 3)
    print("reslut = \(reslut)")
}

 </pre>
 
* 闭包形式2
<pre class="brush: swift;  ">
func testClosure2(){
    // 2.简化
    let closureAdd:(_ num1:Int,_ num2:Int) -> Int
        
    closureAdd = {
        (_ num1:Int,_ num2:Int) in
        return num1+num2
    }
    
    let reslut = closureAdd(2, 3)
    print("reslut = \(reslut)")
}
 </pre>
  
* 闭包形式3
<pre class="brush: swift;  ">
func testClosure3(){
    // 3.简化（当闭包没有接收参数时，可省略in关键字）
    let closureAdd:() -> Int
    
    closureAdd = {
//        () in  // 可省略
        return 100
    }
    
    let reslut = closureAdd()
    print("reslut = \(reslut)")
}
 </pre>
  
* 闭包形式4
<pre class="brush: swift;  ">
func testClosure4(){
    // 4.简化（省略参数名，用$0,$1,$2,$3,...代替）
    let closureAdd:(Int,Int) -> Int
    
    closureAdd = {
        return $0+$1
    }
    
    let reslut = closureAdd(2,3)
    print("reslut = \(reslut)")
}
 </pre>
 
#四、闭包的使用：值捕获

定义：   
  * Swift 中，可以捕获值的闭包的最简单形式是嵌套函数，也就是定义在其他函数的函数体内的函数。   
  * 嵌套函数可以捕获其外部函数所有的参数以及定义的常量和变量。   

例子：   

<pre class="brush: swift;  ">
func testClosure5(){
    // 值捕获
    func makeIncrease(forIncrese num:Int) ->() -> Int{
        var total = 0
        
        // 嵌套函数，捕获上下文变量
        func increastor() -> Int{
            total += num
            return total
        }
        return increastor // 返回函数类型为() -> Int
    }
    
    let increaseFunc1 = makeIncrease(forIncrese: 3)
    print(increaseFunc1())// 3
    print(increaseFunc1())// 6
    print(increaseFunc1())// 9
    
    let increaseFunc1_1 = increaseFunc1 // 引用同一个闭包
    print(increaseFunc1_1())// 12
    
    let increaseFunc2 = makeIncrease(forIncrese: 5) // 新建一个新的闭包
    print(increaseFunc2())// 5
    print(increaseFunc2())// 10
    print(increaseFunc2())// 15
    
}
 </pre>
 
#五、闭包的使用：尾随闭包

定义：    
  * 如果你需要将一个很长的闭包表达式作为最后一个参数传递给函数，可以使用尾随闭包来增强函数的可读性。   
  * 尾随闭包是一个书写在函数括号之后的闭包表达式，函数支持将其作为最后一个参数调用。
  * 在使用尾随闭包时，你不用写出它的参数标签：

例子：   
<pre class="brush: swift;  ">
func testClosure6(){

    // 参数：一个Int型参数，一个函数型参数,返回值：String型
    func outputNameAndNumber(_ num:Int,nameFunc:(String) -> String) -> String{
        let infoString:String
        infoString = nameFunc("Peater")+"My num is \(num)"
        return infoString
    }
    
    // 将闭包作为参数
    //写法一： 不使用尾随闭包写法：
    let result1 = outputNameAndNumber(1, nameFunc: {
        (name:String)->String in
        let infoName = "My name is \(name) "
        return infoName})
    
    print(result1)
    
    //写法二： 使用尾随闭包写法：
    let result2 = outputNameAndNumber(2){
        (name:String)->String in
        let infoName = "My name is \(name) "
        return infoName
    }
    
    print(result2)
    
    //写法三：可简化忽略返回值参数列表
    let result3 = outputNameAndNumber(3){
        (name:String) in
        let infoName = "My name is \(name) "
        return infoName
    }
    
    print(result3)
}
 </pre>
 
#六、闭包的使用：逃逸闭包

定义：   
  * 当一个闭包作为参数传到一个函数中，但是这个闭包在函数返回之后才被执行，我们称该闭包从函数中逃逸。   
  * 当你定义接受闭包作为参数的函数时，你可以在参数名之前标注 @escaping，用来指明这个闭包是允许“逃逸”出这个函数的。   
 
例子：   

<pre class="brush: swift;  ">
func testCloser7(){
    // 关键字：@escaping
    
    var closerArray:[(Int)->String] = [] // 定义一个变量数组：闭包类型
    
    func escapingCloser(handle:@escaping (Int)->String){
        closerArray.append(handle) // 将该闭包存入数组中
    }
    
    // 调用函数
    escapingCloser(handle: {
        (num:Int)->String in
        return "My number is \(num)"
    })
    
    let num = 8
    print(closerArray[0](num))
    print(closerArray.first?(num))
}

 </pre>
 
#七、闭包的使用：自动闭包
 
 定义：   
  * 自动闭包是一种自动创建的闭包，用于包装传递给函数作为参数的表达式。   
  * 这种闭包不接受任何参数，当它被调用的时候，会返回被包装在其中的表达式的值。   
  * 这种便利语法让你能够省略闭包的花括号，用一个普通的表达式来代替显式的闭包。
  
例子：   

 <pre class="brush: swift;  ">

func testCloser8(){
    // 关键字：@autoclosure
    // 作用:自动闭包让你能够延迟求值，因为直到你调用这个闭包，代码段才会被执行
    
    // 例子一：
    var nameArray:[String] = ["Tom","Mary","Lily"] // 定义一个变量数组：闭包类型
    print(nameArray.count) // 3
    
    let customer = {nameArray.remove(at: 0)}
    print(nameArray.count) // 3
    
    let name = customer()
    print(name) // Tom
    
    print(nameArray.count) // 2
    
    // 例子二：将闭包作为参数传递给函数时，能获得同样的延时求值行为。
    var doingWhat = "arrfu"
    
    func namePrint(nameHandle:()->String){
        print("调用前：",doingWhat) // 调用前： arrfu
        print("调用闭包参数 nameHandle() 的结果为：\(nameHandle())") // arrfu is eating
        print("调用后：",doingWhat) // arrfu is eating
    }
    
    // 传入闭包作为参数
    namePrint(nameHandle: {doingWhat = doingWhat + " is eating"
        return doingWhat })
    
    // 例子二：利用自动闭包 @autoclosure 来获得延时求值行为
    var doingWhat1 = "arrfu"
    
    func namePrint1(nameHandle:@autoclosure ()->String){
        print("1.调用前：",doingWhat1) // 1.调用前： arrfu
        print("1.调用闭包参数 nameHandle() 的结果为：\(nameHandle())")
        print("1.调用后：",doingWhat1) // 1.调用后： arrfu is eating
    }
    
    func haveSomeFun() -> String{
        doingWhat1 = doingWhat1 + " is eating"
        return doingWhat1;
    }
    namePrint1(nameHandle: haveSomeFun()) // 可直接传入函数，而不是闭包
    // 被 @autoclosure 标记的参数 nameHandle 将自动转化为一个闭包。
}
 </pre>
 
