---
title: JavaConstructor
date: 2020-06-15 23:01:25
tags: [Java,constructor,javaClass]
---

```java
//SubCat.java
class Cat  {
    Cat (int c)  {
        System.out.print ("cat"+c+" ");  
    }
 }

class SubCat extends Cat  {
    SubCat (int c){
        super (5); //work here.Specifies the superclass constructor,and must be at first line!!!
        System.out.print ("cable");
    }
    SubCat()  {  
        this (4);
    }

    public static void main (String  []  args)  {
        SubCat s= new SubCat();
    }
}
```

```java
//CB.java
class Ca{
    int num = 1;
    //Ca(){}; not worked,because CB calls the default parameterless constructor of the parent class by default
    Ca(int num){
            this.num = num;
            System.out.print(this.num);
    }
}

class Cb extends Ca{

    int num = 2;
    Cb(int num){
        this.num = num;
        System.out.print(num);
    }
    public static void main(String[] args) {
            Ca a = new Cb(5);
    }
} 
```

```java
//C.java
class A {
    public void func1() {
        System.out.println("A func1 is calling");
    }
    public void func2() {
        func1();
    }
}
class B extends A {
    public void func1() {
        System.out.println("B func1 is calling");
    }

    public void func3() {
        System.out.println("B func3 is calling");
    }
}
class C {
    public static void main(String[] args) {
        //B a = new B();
        A a = new B();
        System.out.println(a.getClass());
        a.func1();
        a.func2();//类型是B的类型，函数是B的函数
        //a.func3(); not work,because Method func3 in undefined in class A
    }
}
```

- 内部类可以向上转型。
- 内部类作为父类时不能向下转型。
- 如果有一个带形参的构造函数，编译器不会自动添加无参的构造函数。

```java
//Pen.java
class Pencil{
    public void write (String content){
        System.out.println( "Write"+content);
    }
}
class RubberPencil extends Pencil{
    public void write (String content){
        System.out.println("Rubber Write"+content);
    }
  public void erase (String content){
      System.out.println( "Erase "+content);
  }
}
public class Pen {
    public static void main(String[] args) {
        Pencil p=new Pencil();
        (( RubberPencil) p).write("Hello");//Runtime error
    }
}
```

​    