---
title: ' JAVA程式師面試32問（含答案）'
date: 2016-03-23 11:29:59
categories: Java
tags:
  - Java
---

第一，談談final, finally, finalize的区别。

第二，Anonymous Inner Class (匿名內部类) 是否可以extends(继承)其它类，是否可以implements(实现)interface(接口)?

第三，Static Nested Class 和 Inner Class的不同，說得越多越好(面試題有的很籠統)。

第四，&和&&的区别。

第五，HashMap和Hashtable的区别。

第六，Collection 和 Collections的区别。

第七，什么時候用assert。

第八，GC是什么? 为什么要有GC?

第九，String s = new String("xyz");创建了几个String Object?

第十，Math.round(11.5)等於多少? Math.round(-11.5)等於多少?

第十一，short s1 = 1; s1 = s1 + 1;有什么错? short s1 = 1; s1 += 1;有什么错?

第十二，sleep() 和 wait() 有什么区别?

第十三，Java有沒有goto?

第十四，数組有沒有length()這个方法? String有沒有length()這个方法?

第十五，Overload和Override的区别。Overloaded的方法是否可以改变返回值的类型?

第十六，Set里的元素是不能重复的，那么用什么方法來區分重复与否呢? 是用==还是equals()? 它們有何区别?

第十七，给我一个你最常見到的runtime exception。

第十八，error和exception有什么区别?

第十九，List, Set, Map是否继承自Collection接口?

第二十，abstract class和interface有什么区别?

第二十一，abstract的method是否可同時是static,是否可同時是native，是否可同時是synchronized?

第二十二，接口是否可继承接口? 抽象类是否可实现(implements)接口? 抽象类是否可继承实体类(concrete class)?

第二十三，启動一个线程是用run()还是start()?

第二十四，构造器Constructor是否可被override?

第二十五，是否可以继承String类?

第二十六，當一个线程進入一个对象的一个synchronized方法後，其它线程是否可進入此对象的其它方法?

第二十七，try {}里有一个return語句，那么緊跟在這个try後的finally {}里的code會不會被執行，什么時候被執行，在return前还是後?

第二十八，編程題: 用最有效率的方法算出2乘以8等於幾?

第二十九，兩个对象值相同(x.equals(y) == true)，但卻可有不同的hashcode，這句話对不对?

第三十，當一个对象被當作参数傳遞到一个方法後，此方法可改变這个对象的属性，並可返回变化後的結果，那么這里到底是值傳遞还是引用傳遞?

第三十一，swtich是否能作用在byte上，是否能作用在long上，是否能作用在String上?

第三十二，編程題: 写一个Singleton出來。


以下是答案

第一，談談final, finally, finalize的区别。
final— 修飾符（关键字）如果一个类被声明为final，意味着它不能再派生出新的子类，不能作为父类被继承。因此一个类不能既被声明为 abstract的，又被声明为final的。將变量或方法声明为final，可以保證它們在使用中不被改变。被声明为final的变量必須在声明時给定初值，而在以後的引用中只能讀取，不可修改。被声明为final的方法也同样只能使用，不能重载
finally—再異常處理時提供 finally 塊來執行任何清除操作。如果拋出一个異常，那么相匹配的 catch 子句就會執行，然後控制就會進入 finally 塊（如果有的話）。
finalize —方法名。Java 技術允許使用 finalize() 方法在垃圾收集器將对象從內存中清除出去之前做必要的清理工作。這个方法是由垃圾收集器在確定這个对象沒有被引用時对這个对象調用的。它是在 Object 类中定義的，因此所有的类都继承了它。子类覆蓋 finalize() 方法以整理系統資源或者執行其他清理工作。finalize() 方法是在垃圾收集器刪除对象之前对這个对象調用的。

第二，Anonymous Inner Class (匿名內部类) 是否可以extends(继承)其它类，是否可以implements(实现)interface(接口)?
匿名的內部类是沒有名字的內部类。不能extends(继承) 其它类，但一个內部类可以作为一个接口，由另一个內部类实现。

第三，Static Nested Class 和 Inner Class的不同，說得越多越好(面試題有的很籠統)。
Nested Class （一般是C++的說法），Inner Class (一般是JAVA的說法)。Java內部类与C++嵌套类最大的不同就在於是否有指向外部的引用上。具体可見http: //www.frontfree.net/articles/services/view.asp?id=704&page=1
注： 靜態內部类（Inner Class）意味着1创建一个static內部类的对象，不需要一个外部类对象，2不能從一个static內部类的一个对象訪問一个外部类对象

第四，&和&&的区别。
&是位运算符。&&是布尔逻辑运算符。

第五，HashMap和Hashtable的区别。
都属於Map接口的类，实现了將惟一鍵映射到特定的值上。
HashMap 类沒有分类或者排序。它允許一个 null 鍵和多个 null 值。
Hashtable 类似於 HashMap，但是不允許 null 鍵和 null 值。它也比 HashMap 慢，因为它是同步的。

第六，Collection 和 Collections的区别。
Collections是个java.util下的类，它包含有各種有關集合操作的靜態方法。
Collection是个java.util下的接口，它是各種集合結构的父接口。
第七，什么時候用assert。
断言是一个包含布尔表达式的語句，在執行這个語句時假定該表达式为 true。如果表达式計算为 false，那么系統會報告一个 AssertionError。它用於調試目的：
assert(a > 0); // throws an AssertionError if a <= 0
断言可以有兩種形式：
assert Expression1 ;
assert Expression1 : Expression2 ;
Expression1 應該總是产生一个布尔值。
Expression2 可以是得出一个值的任意表达式。這个值用於生成顯示更多調試信息的 String 消息。
断言在默認情況下是禁用的。要在編譯時启用断言，需要使用 source 1.4 標記：
javac -source 1.4 Test.java
要在運行時启用断言，可使用 -enableassertions 或者 -ea 標記。
要在運行時選擇禁用断言，可使用 -da 或者 -disableassertions 標記。
要系統类中启用断言，可使用 -esa 或者 -dsa 標記。还可以在包的基礎上启用或者禁用断言。
可以在預計正常情況下不會到达的任何位置上放置断言。断言可以用於驗證傳遞给私有方法的参数。不過，断言不應該用於驗證傳遞给公有方法的参数，因为不管是否启用了断言，公有方法都必須檢查其参数。不過，既可以在公有方法中，也可以在非公有方法中利用断言測試後置條件。另外，断言不應該以任何方式改变程序的狀態。


第八，GC是什么? 为什么要有GC? (基礎)。
GC是垃圾收集器。Java 程序員不用擔心內存管理，因为垃圾收集器會自動進行管理。要請求垃圾收集，可以調用下面的方法之一：
System.gc()
Runtime.getRuntime().gc()

第九，String s = new String("xyz");创建了几个String Object?
兩个对象，一个是“xyx”,一个是指向“xyx”的引用对象s。

第十，Math.round(11.5)等於多少? Math.round(-11.5)等於多少?
Math.round(11.5)返回（long）12，Math.round(-11.5)返回（long）-11;

第十一，short s1 = 1; s1 = s1 + 1;有什么错? short s1 = 1; s1 += 1;有什么错?
short s1 = 1; s1 = s1 + 1;有错，s1是short型，s1+1是int型,不能顯式轉化为short型。可修改为s1 =(short)(s1 + 1) 。short s1 = 1; s1 += 1正確。

第十二，sleep() 和 wait() 有什么区别? 搞线程的最愛
sleep()方法是使线程停止一段時間的方法。在sleep 時間間隔期滿後，线程不一定立即恢复執行。這是因为在那个時刻，其它线程可能正在運行而且沒有被調度为放棄執行，除非(a)“醒來”的线程具有更高的優先級
(b)正在運行的线程因为其它原因而阻塞。
wait()是线程交互時，如果线程对一个同步对象x 發出一个wait()調用，該线程會暫停執行，被調对象進入等待狀態，直到被喚醒或等待時間到。
第十三，Java有沒有goto?
Goto—java中的保留字，現在沒有在java中使用。

第十四，数組有沒有length()這个方法? String有沒有length()這个方法？
数組沒有length()這个方法，有length的属性。
String有有length()這个方法。

第十五，Overload和Override的区别。Overloaded的方法是否可以改变返回值的类型?
方法的重写Overriding和重载Overloading是Java多態性的不同表現。重写Overriding是父类与子类之間多態性的一種表現，重载Overloading是一个类中多態性的一種表現。如果在子类中定義某方法与其父类有相同的名稱和参数，我們說該方法被重写 (Overriding)。子类的对象使用這个方法時，將調用子类中的定義，对它而言，父类中的定義如同被“屏蔽”了。如果在一个类中定義了多个同名的方法，它們或有不同的参数个数或有不同的参数类型，則稱为方法的重载(Overloading)。Overloaded的方法是可以改变返回值的类型。

第十六，Set里的元素是不能重复的，那么用什么方法來區分重复与否呢? 是用==还是equals()? 它們有何区别?
Set里的元素是不能重复的，那么用iterator()方法來區分重复与否。equals()是判讀兩个Set是否相等。
equals()和==方法决定引用值是否指向同一对象equals()在类中被覆蓋，为的是當兩个分離的对象的內容和类型相配的話，返回真值。

第十七，给我一个你最常見到的runtime exception。
ArithmeticException, ArrayStoreException, BufferOverflowException, BufferUnderflowException, CannotRedoException, CannotUndoException, ClassCastException, CMMException, ConcurrentModificationException, DOMException, EmptyStackException, IllegalArgumentException, IllegalMonitorStateException, IllegalPathStateException, IllegalStateException,
ImagingOpException, IndexOutOfBoundsException, MissingResourceException, NegativeArraySizeException, NoSuchElementException, NullPointerException, ProfileDataException, ProviderException, RasterFormatException, SecurityException, SystemException, UndeclaredThrowableException, UnmodifiableSetException, UnsupportedOperationException

第十八，error和exception有什么区别?
error 表示恢复不是不可能但很困難的情況下的一種嚴重問題。比如說內存溢出。不可能指望程序能處理這样的情況。
exception 表示一種設計或实现問題。也就是說，它表示如果程序運行正常，從不會發生的情況。
第十九，List, Set, Map是否继承自Collection接口?
List，Set是

Map不是

第二十，abstract class和interface有什么区别?
声明方法的存在而不去实现它的类被叫做抽象类（abstract class），它用於要创建一个体現某些基本行为的类，並为該类声明方法，但不能在該类中实现該类的情況。不能创建abstract 类的实例。然而可以创建一个变量，其类型是一个抽象类，並讓它指向具体子类的一个实例。不能有抽象构造函数或抽象靜態方法。Abstract 类的子类为它們父类中的所有抽象方法提供实现，否則它們也是抽象类为。取而代之，在子类中实现該方法。知道其行为的其它类可以在类中实现這些方法。
接口（interface）是抽象类的变体。在接口中，所有方法都是抽象的。多继承性可通過实现這样的接口而獲得。接口中的所有方法都是抽象的，沒有一个有程序体。接口只可以定義static final成員变量。接口的实现与子类相似，除了該实现类不能從接口定義中继承行为。當类实现特殊接口時，它定義（即將程序体给予）所有這種接口的方法。然後，它可以在实现了該接口的类的任何对象上調用接口的方法。由於有抽象类，它允許使用接口名作为引用变量的类型。通常的動態聯編將生效。引用可以轉換到接口类型或從接口类型轉換，instanceof 运算符可以用來决定某对象的类是否实现了接口。

第二十一，abstract的method是否可同時是static,是否可同時是native，是否可同時是synchronized?
都不能

第二十二，接口是否可继承接口? 抽象类是否可实现(implements)接口? 抽象类是否可继承实体类(concrete class)?
接口可以继承接口。抽象类可以实现(implements)接口，抽象类是否可继承实体类，但前提是实体类必須有明確的构造函数。

第二十三，启動一个线程是用run()还是start()?
启動一个线程是調用start()方法，使线程所代表的虛擬處理機處於可運行狀態，這意味着它可以由JVM調度並執行。這並不意味着线程就會立即運行。run()方法可以产生必須退出的標志來停止一个线程。
第二十四，构造器Constructor是否可被override?
构造器Constructor不能被继承，因此不能重写Overriding，但可以被重载Overloading。

第二十五，是否可以继承String类?
String类是final类故不可以继承。

第二十六，當一个线程進入一个对象的一个synchronized方法後，其它线程是否可進入此对象的其它方法?
不能，一个对象的一个synchronized方法只能由一个线程訪問。

第二十七，try {}里有一个return語句，那么緊跟在這个try後的finally {}里的code會不會被執行，什么時候被執行，在return前还是後?
會執行，在return前執行。


第二十八，編程題: 用最有效率的方法算出2乘以8等於幾?
有C背景的程序員特別喜歡問這種問題。

2 << 3

第二十九，兩个对象值相同(x.equals(y) == true)，但卻可有不同的hash code，這句話对不对?
不对，有相同的hash code。

第三十，當一个对象被當作参数傳遞到一个方法後，此方法可改变這个对象的属性，並可返回变化後的結果，那么這里到底是值傳遞还是引用傳遞?
是值傳遞。Java 編程語言只由值傳遞参数。當一个对象实例作为一个参数被傳遞到方法中時，参数的值就是对該对象的引用。对象的內容可以在被調用的方法中改变，但对象的引用是永遠不會改变的。


第三十一，swtich是否能作用在byte上，是否能作用在long上，是否能作用在String上?
switch（expr1）中，expr1是一个整数表达式。因此傳遞给 switch 和 case 語句的参数應該是 int、 short、 char 或者 byte。long,string 都不能作用於swtich。
第三十二，編程題: 写一个Singleton出來。
Singleton模式主要作用是保證在Java應用程序中，一个类Class只有一个实例存在。
一般Singleton模式通常有幾種種形式:
第一種形式: 定義一个类，它的构造函数为private的，它有一个static的private的該类变量，在类初始化時实例話，通過一个public的getInstance方法獲取对它的引用,繼而調用其中的方法。
public class Singleton {
private Singleton(){}
//在自己內部定義自己一个实例，是不是很奇怪？
//注意這是private 只供內部調用
private static Singleton instance = new Singleton();
//這里提供了一个供外部訪問本class的靜態方法，可以直接訪問
public static Singleton getInstance() {
return instance;
}
}
第二種形式:
public class Singleton {
private static Singleton instance = null;
public static synchronized Singleton getInstance() {
//這个方法比上面有所改進，不用每次都進行生成对象，只是第一次
//使用時生成实例，提高了效率！
if (instance==null)
instance＝new Singleton();
return instance; }
}
其他形式:
定義一个类，它的构造函数为private的，所有方法为static的。
一般認为第一種形式要更加安全些

第三十三 Hashtable和HashMap
Hashtable继承自Dictionary类，而HashMap是Java1.2引進的Map interface的一个实现

HashMap允許將null作为一个entry的key或者value，而Hashtable不允許

还有就是，HashMap把Hashtable的contains方法去掉了，改成containsvalue和containsKey。因为contains方法容易讓人引起誤解。

最大的不同是，Hashtable的方法是Synchronize的，而HashMap不是，在
多个线程訪問Hashtable時，不需要自己为它的方法实现同步，而HashMap
就必須为之提供外同步。

Hashtable和HashMap采用的hash/rehash算法都大概一样，所以性能不會有很大的差異。

//还得看看java数據結构方面的,當然這些都是Java的基本題，那些面試的人大多数不會問你Hibernate有多先進，Eclipse的三个組成部分，或command design pattern，他們都是老一輩了，最喜歡問的就是基礎知識。別小看了這些基礎，还是很重要的.
