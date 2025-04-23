<h4 id="HMWUZ">问题：</h4>
**<font style="color:#db7800;">指针和引用有什么区别？C++的4个cast是什么，有什么应用场景？为什么要尽量使用C++的cast？</font>**

**<font style="color:#db7800;">为什么不要对数组使用多态，如何解决这个问题?</font>**

<h4 id="kLTg8">思考：</h4>
<h5 id="KRVaW">**<u>在CPU眼中，引用的本质是什么？</u>**</h5>
**引用的本质：**

引用在底层实现上与指针相同，可以理解为一种“指针常量”，即`int *const p = &a;`。引用是某个变量的别名，修改引用会直接修改其绑定的变量。引用一旦绑定后不能改变其指向的对象。

<u>C语言不支持引用语法，引用是C++引入的特性。引用可以看作是指针的语法糖，提供了更简洁的语法，但功能上可以通过指针实现。</u>

**<u>java中的引用是什么？和C++的引用有什么区别？</u>**

**Java中的引用**

**引用的本质**：

    - Java中，引用是指向对象的“指针”，但Java语言本身屏蔽了指针的概念，开发者无法直接操作内存地址。
    - 引用变量存储的是对象在堆内存中的地址（逻辑地址，而非物理地址），通过引用可以访问对象的属性和方法。

**JVM中的引用实现**：

    - 在JVM中，引用是一个指向堆内存中对象的指针，但这个指针是由JVM管理的，开发者无法直接访问或修改。
    - JVM通过垃圾回收机制（GC）自动管理对象的生命周期，引用是垃圾回收的重要依据。
    - JVM中的引用分为四种类型：
        * **强引用：**最常见的引用类型，只要强引用存在，对象就不会被垃圾回收。
        * **软引用：**在内存不足时，垃圾回收器会回收软引用指向的对象。
        * **弱引用：**无论内存是否充足，垃圾回收器都会回收弱引用指向的对象。
        * **虚引用：**虚引用主要用于跟踪对象被垃圾回收的状态，必须与引用队列配合使用。

**Java引用的特点**：

    - Java引用是**动态的**，可以在运行时指向不同的对象。
    - Java引用是**安全的**，开发者无法直接操作内存地址，避免了指针操作带来的内存问题。
    - Java引用是**可被垃圾回收的**，当没有引用指向某个对象时，该对象会被垃圾回收器回收。

**C++中的****引用的本质**：

C++中的引用是某个变量的“别名”，底层实现通常是指针常量（`int *const p`），即引用一旦绑定到一个变量后，不能改变其绑定的对象。引用在语法上更简洁，不需要使用`*`操作符来访问或修改值。

**C++引用的特点**：

    - C++引用是**静态的**，一旦绑定到一个变量后，不能改变其绑定的对象。
    - C++引用是**不可为空的**，必须在声明时初始化。
    - C++引用是**不可被重新绑定的**，不能像指针那样指向不同的对象。
    - C++引用是**语法糖**，底层实现是指针，但提供了更简洁的语法。

<h1 id="kEDbj">基础议题</h1>
<h4 id="RqEwQ">**Item M1：指针与引用的区别**</h4>
_**指针**_

定义：指针是一个变量，它存储的是另一个变量的内存地址。

语法：通过*操作符来定义，并且可以通过->操作符来访问成员。

灵活性：指针可以在任何时候被重新赋值，以指向不同的对象。

空值：指针可以被设置为nullptr（或NULL/0），表示它不指向任何有效的对象。

检查：在使用指针之前通常需要检查它是否为nullptr，以避免潜在的运行时错误。

初始化：指针可以不初始化就声明，但这通常是不安全的做法。

用途：当程序逻辑允许一个变量在某些情况下不指向任何东西，或者需要改变指向的对象时，使用指针是合适的。

_**引用**_

定义：引用是已存在的变量的别名。一旦初始化之后，引用就不能再指向其他的对象。

语法：通过&操作符来定义，并且通过.操作符来访问成员。

不可更改：一旦引用被初始化指向一个对象，它就不能再指向别的对象。

非空性：引用必须总是引用一个有效的对象，不能指向空值。

初始化：引用必须在声明时就进行初始化，否则会导致编译错误。

效率：由于引用总是有效的，所以在使用引用前不需要做空检查，这使得引用通常比指针更高效。

用途：当确保变量始终指向一个有效对象，并且不需要改变所指向的对象时，使用引用更为合适。此外，在重载某些操作符如[]时，通常返回引用以保持直观的语义。

_**总结**_

指针适合于那些需要改变指向对象或者可能不指向任何对象的情况。引用适用于那些始终需要引用到一个特定对象，并且不会改变引用对象的情况。此外，引用在函数参数传递中也常用来实现类似传址的效果，同时提供了更好的可读性和安全性。

<h4 id="MZmxt">**Item M2：尽量使用 C++风格的类型转换**</h4>
_**static_cast**_

<u>用途：用于基础类型的转换，比如整数到浮点数的转换，或者指针之间的转换，这些转换在编译时是明确的。</u>

限制：不能用于转换掉const、volatile属性，也不能用于不相关的类型转换，如结构体到整数的转换。

```plain
int firstNumber = 7;
double result = static_cast<double>(firstNumber) / 6; // 结果是 1.16667
```

`<u>static_cast</u>`<u> 是 C++ 中的一种类型转换运算符，用于执行</u>**<u>明确的类型转换</u>**<u>。它是 C++ 中最常用的类型转换方式之一，通常用于相关类型之间的转换（例如基本数据类型之间的转换、基类与派生类之间的转换等）。与 </u>`<u>reinterpret_cast</u>`<u> 和 </u>`<u>const_cast</u>`<u> 不同，</u>`<u>static_cast</u>`<u> 在编译时会进行一定的类型检查，因此相对更安全。</u>

`**static_cast**`** 的语法**

```plain
static_cast<new_type>(expression)
```

+ `new_type`：目标类型。
+ `expression`：需要转换的表达式

`**static_cast**`** 的用途**

**基本数据类型之间的转换，**比如将`int`转换为`double`，以及将`double`转换为`int`。

将基类引用或指针 `Base&` * 转换为派生类引用 `Derived&`*。

**将 **`**void***`** 转换为其他指针类型**

```plain
int a =100;
void* voidPtr =&a;
int* intPtr = static_cast<int*>(voidPtr); //将 void* 转换为 int*
```

**注意事项**

    - `static_cast` 在编译时会进行一定的类型检查，但不能保证运行时的安全性。
    - 例如，在向下转型（将基类指针转换为派生类指针）时，如果实际对象不是目标类型，可能会导致未定义行为。

**（1）不能用于去除 **`**const**`** 或 **`**volatile**`** 属性**：

    - 如果需要去除 `const` 或 `volatile` 属性，应该使用 `const_cast`。

**（2）不能用于不相关类型之间的转换**：

    - 例如，不能将 `int*` 转换为 `char*`，这种转换需要使用 `reinterpret_cast`。

**（3）不能用于动态类型检查**：

    - 如果需要运行时类型检查（例如多态类型的向下转型），应该使用 `dynamic_cast`。

_**const_cast**_

用途：用于添加或移除const或volatile属性。 

重要性：确保只改变变量的const或volatile状态，而不改变其他属性。

const_cast 是 C++ 中的一个类型转换运算符，它主要用于添加或移除变量的 const 或 volatile 属性，但通常情况下，我们使用 const_cast 来移除变量的 const 属性，而不是添加。

为什么通常不使用 const_cast 添加 const 属性

const_cast 的语法是 const_cast<type〉(expression)，其中 type 是目标类型，expression 是要转换的表达式。当尝试使用 const_cast 来添加 const 属性时，语法上是可以的，但实际效果并不符合预期。

```plain
const int x = 10;
int *nonConstX = const_cast<int*>(&x); // 移除 const 属性
```

不可以通过 nonConstX 修改 x 的值，因为 x 本身还是 const 的。根据C++标准，尝试通过const_cast得到的非const指针修改一个原本是const的对象是未定义行为。标准没有定义这种行为的具体结果，它可能工作，也可能不工作，甚至可能导致程序崩溃。

移除const属性的主要用途是在某些特定情况下，当需要修改原本被声明为const的对象时。通常，const关键字用于表明一个对象不应该被修改，以增强程序的安全性和可读性。

然而有时候会遇到需要绕过const保护的情况

_**（1）库函数要求非const参数**_

当调用一个库函数，该函数需要一个非const参数，但只有一个const对象时，可以使用const_cast来临时移除const属性，以便将这个对象传递给函数。这种情况下的前提是，你确定这个库函数不会实际修改这个对象。有时，为了兼容旧代码或特定的API，你可能需要使用const_cast来适应那些不接受const参数的接口。

```plain
void someFunction(int* ptr); // 假设这是一个第三方库函数，要求非const指针
const int x = 1;
someFunction(const_cast<int*>(&x)); // 临时移除const属性
```

const函数修改非const成员变量，加mutable。

```plain
class MyClass {
private:
    mutable int counter; // 使用mutable关键字，允许const成员函数修改它
public:
    void incrementCounter() const {
        ++counter; // 允许修改
    }
};
```

```plain
class MyClass {
private:
    int data; // 非 mutable 数据成员
public:
    MyClass(int d) : data(d) {}

    void modifyData(int newValue) const {
        // 使用 const_cast 移除 const 属性
        int* nonConstData = const_cast<int*>(&data);
        *nonConstData = newValue; // 修改 data
    }
    int getData() const {
        return data;
    }
};
int main() {
    const MyClass obj(10); // 创建一个 const 对象
    obj.modifyData(20); // 尝试修改 data
    std::cout << "Data: " << obj.getData() << std::endl; // 输出 Data: 20
    return 0;
}
```

在C++中，当声明一个对象为const类型时，这个对象的所有非mutable数据成员都会被视为const。不能通过任何非const成员函数或非const引用修改这些数据成员。

当一个成员函数被声明为`const`时，`this`指针实际上是`const`的。`this`指针指向的对象是`const`的，不能通过`this`指针修改对象的任何非`mutable`成员。在`modifyData`函数中，`this`指针是`const MyClass* const this`。这意味着不能直接通过`this`指针修改`data`成员，因为`data`是非`mutable`的。在`modifyData`函数中，`const_cast<int*>(&data)`的作用是将`&data`从`const int*`转换为`int*`。`const_cast`去除了`&data`的`const`属性，使得可以通过这个指针修改`data`的值。

_**dynamic_cast**_

用途：<u>主要用于多态类型的安全转换，特别是从基类指针或引用来转换到派生类指针或引用。</u>

<u>特点：如果转换是不可能的（比如基类指针实际上并不指向派生类对象），dynamic_cast会返回nullptr（对于指针）或抛出std::bad_cast异常（对于引用）。</u>

<u>从基类转换到派生类的主要原因包括：</u>

<u>• 访问派生类特有的成员</u>

<u>• 调用派生类特有的方法</u>

<u>• 在多态容器中对特定类型的对象进行操作</u>

<u>• 实现特定的算法或逻辑</u>

这些需求在面向对象编程中非常常见，dynamic_cast 提供了一种安全且灵活的方式来实现这些转换。

_**reinterpret_cast**_

`reinterpret_cast`是 C++ 中的一种类型转换运算符，用于执行低级别的、不安全的类型转换。它可以将一种类型的指针或引用转换为另一种完全不相关的类型的指针或引用，甚至可以用于将指针转换为整数类型，或者将整数类型转换为指针。由于其行为非常底层且不安全，需要谨慎使用。

**指针类型之间的转换：**

    - 将一个类型的指针转换为另一个类型的指针，即使这两种类型之间没有任何继承关系。
    - 例如，将 `int*` 转换为 `char*`。

**指针与整数之间的转换：**

    - 将指针转换为整数类型（如 `uintptr_t`），或者将整数类型转换为指针。
    - 如将指针的值存储为一个整数，或者从整数恢复为指针。

**函数指针之间的转换：**

    - 将一个函数指针类型转换为另一个函数指针类型。

**不相关类型之间的转换：**

    - 用于完全不相关的类型之间的转换，例如将结构体指针转换为整数指针。

**缺点：**

**（1）不安全：**

    - `reinterpret_cast`不会进行任何类型检查，因此容易引发未定义行为。
    - 将一个 `float*` 转换为 `int*` 并解引用，可能会导致错误的结果或程序崩溃。

**（2）平台依赖性：**

    - `reinterpret_cast` 的行为可能依赖于具体的平台和编译器实现，因此在跨平台代码中需要格外小心。

**避免滥用**：只有在确实需要低级别转换时才使用 `reinterpret_cast`，优先使用更安全的 `static_cast` 或 `dynamic_cast`。

_**为什么要使用C++风格的类型转换？**_

精确性：C++风格的类型转换更加精确，可以更好地表达转换意图。

<u>安全性：dynamic_cast可以安全地处理多态类型转换，避免了C风格转换中可能出现的错误</u>。

可读性：使用特定的类型转换关键字，代码的意图更加清晰。

<u>虽然C++风格的类型转换可能显得冗长，但它们提供了更好的类型安全性和代码清晰度。因此，推荐在C++代码中使用这些现代的类型转换操作符，以提高代码质量和可靠性。</u>

<h4 id="aElPq">Item M3：**不要对数组使用多态**</h4>
```plain
class BST {
public:
    virtual ~BST() {}
    virtual void print(ostream& out) const = 0; // 纯虚函数
};
class BalancedBST : public BST {
public:
    void print(ostream& out) const override {
        out << "BalancedBST object\n";
    }
};
```

打印数组的函数

```plain
void printBSTArray(ostream& s, const BST array[], int numElements) {
    for (int i = 0; i < numElements; ++i) {
        array[i].print(s); // 通过基类指针调用派生类的函数
    }
}
int main() {
    BST* bstArray = new BST[10]; // 假设 BST 有默认构造函数
    printBSTArray(cout, bstArray, 10); // 正常运行
    delete[] bstArray;
    return 0;
}
```

问题情况

```plain
int main() {
    BalancedBST* bBSTArray = new BalancedBST[10];
    printBSTArray(cout, bBSTArray, 10); // 可能会出错
    delete[] bBSTArray; // 可能会导致未定义行为
    return 0;
}
```

**问题解释**

<u>1. 数组元素大小的差异</u>

声明为基类指针的数组调用多态会出现问题，

+ <u>当数组中的元素是派生类对象（例如 </u>`<u>Derived</u>`<u>）时，由于数组的类型是基类指针（</u>`<u>Base*</u>`<u>），编译器会按照基类的大小来分配内存。</u>
+ <u>如果派生类的大小大于基类（通常是这样），那么数组的内存布局会出错，因为数组的每个元素的大小仍然是</u>`<u>sizeof(Base)</u>`<u>,而不是</u>`<u>sizeof(Derived)</u>`<u>。</u>
+ <u>当对基类指针进行指针算术操作时(例如</u>`<u>ptr++</u>`<u>)，编译器会按照基类的大小来移动指针。</u>
+ <u>如果实际存储的是派生类对象，指针算术的结果会错误，导致访问错误的内存地址。</u>

<u>2. 析构函数的调用</u>

<u>删除派生类数组：在 delete[] bBSTArray 时，编译器会调用 BST 的析构函数，而不是 BalancedBST 的析构函数。这会导致 BalancedBST 对象的资源没有被正确释放，从而引发资源泄漏或未定义行为。</u>

解决方案

<u>（1）使用容器：使用 std::vector 或其他标准库容器，它们可以正确处理多态性。</u>

<u>（2）使用智能指针：使用 std::unique_ptr 或 std::shared_ptr 来管理动态分配的对象。</u>

<u>（3）避免从具体类派生具体类(M33)(避免使用多态）</u>

**<u>为什么使用vector就可以避免这个问题，他能避免这个问题的本质是什么？</u>**

`std::vector` 会在堆上动态分配内存，存储派生类对象的指针。(通常是智能指针，如`std::unique_ptr` 或 `std::shared_ptr`),而不是对象本身。由于存储的是指针，每个元素的大小是固定的(指针的大小)，与对象的实际大小无关。

<h4 id="UFiyz">**Item M4：避免无用的缺省构造函数**</h4>
C++默认构造函数是指不接受任何参数的构造函数，它允许在不提供任何外部数据的情况下创建对象。这种构造函数对于某些类是有意义的，比如那些可以被初始化为“空”或“未指定”状态的类，或者像链表、哈希表这样的容器类。

然而，对于需要特定数据才能正确初始化的对象，比如一个地址簿对象或带有公司ID的设备对象，缺省构造函数就不合适了。因为这样的对象如果没有正确的初始化数据，它们就没有意义。

```plain

```

如果一个类没有缺省构造函数，那么在使用这个类时会遇到一些限制：

1. **数组问题**：C++中不能在创建对象数组时传递构造函数参数。如果一个类没有缺省构造函数，不能直接创建该类的数组。`EquipmentPiece`类没有缺省构造函数，它需要一个`IDNumber`参数。

文章提供了几种**解决方案：**

    - 使用初始化列表来创建非堆数组。

```plain
// 没有缺省构造函数的类
class EquipmentPiece {
public:
    EquipmentPiece(int IDNumber);
    // ...
};
// 使用初始化列表创建数组
int ID1, ID2, ID3, ..., ID10; // 存储设备ID号的变量
EquipmentPiece bestPieces[] = {
    EquipmentPiece(ID1),
    EquipmentPiece(ID2),
    EquipmentPiece(ID3),
    // ...
};
```

    - 使用指针数组来代替对象数组。

```plain
//使用指针数组
typedef EquipmentPiece* PEP; // PEP指针指向一个EquipmentPiece对象
PEP bestPieces[10]; // 正确，没有调用构造函数
for (int i = 0; i < 10; ++i) {
    bestPieces[i] = new EquipmentPiece(IDNumber);
}
```

    - 使用placement new来在原始内存中构造对象。

```plain
// 使用placement new在原始内存中构造对象
void *rawMemory = operator new[](10 * sizeof(EquipmentPiece)); // 分配足够的内存
EquipmentPiece *bestPieces = static_cast<EquipmentPiece*>(rawMemory);
for (int i = 0; i < 10; ++i) {
    new (&bestPieces[i]) EquipmentPiece(IDNumber); // 使用"placement new"
}
```

1. **虚基类**：在设计虚基类时，如果没有缺省构造函数，那么所有派生类在实例化时都必须提供虚基类的构造函数参数，这可能会导致代码复杂和难以维护。

**不要提供无意义的缺省构造函数**：有些人认为所有类都应该有缺省构造函数，即使它们不能正确初始化对象。这可能会导致代码中的错误处理变得复杂，并且影响性能如果一个类的对象在没有接收到必要的初始化数据时没有意义，那么提供缺省构造函数（即无参数的构造函数）是不合理的。这样的缺省构造函数可能会导致对象处于未正确初始化的状态。提供无意义的缺省构造函数可能会导致类的成员函数需要额外检查对象是否已经正确初始化，这会增加运行时间的开销。因为每次调用成员函数时，都需要进行初始化状态的检查。如果成员函数需要检查对象的初始化状态，那么就需要更多的代码来处理这些检查和可能出现的错误情况。这会导致可执行文件或库的大小增加。

<h1 id="vMXis">运算符</h1>
<h4 id="qxmCm">**Item M5：谨慎定义类型转换函数 **</h4>
单参数构造函数和隐式类型转换运算符这两种可能导致意外行为的语言特性，并提出了几种解决方案。

隐式类型转换：C++编译器会在某些情况下自动执行类型转换，这可能会导致意料之外的行为，尤其是当自定义类型涉及其中时。

隐式类型转换运算符：定义了operator T()形式的成员函数后，对象可以隐式转换为目标类型T。这可能导致错误的类型转换，特别是在重载运算符的情况下。单参数构造函数：单参数构造函数可以用于隐式类型转换，这可能导致不合理的代码被编译，而不会产生编译错误。

解决方案

使用显式转换函数：替换隐式类型转换运算符，使用普通的成员函数来执行转换。

使用explicit关键字：在单参数构造函数前加上explicit关键字，防止编译器使用该构造函数进行隐式类型转换。

有理数类Rational的隐式类型转换

```plain
class Rational {
public:
    Rational(int numerator = 0, int denominator = 1);
    operator double() const; // 隐式转换为double
};
```

Rational类有一个隐式类型转换运算符operator double()，允许Rational对象被隐式转换为double。为了避免这种情况，可以定义一个显式的转换函数：

```plain
class Rational {
public:
    Rational(int numerator = 0, int denominator = 1);
    double asDouble() const; // 显式转换为double
};
```

数组类Array的单参数构造函数

单参数构造函数可以被编译器用于隐式类型转换，从而导致意外的行为

单参数构造函数的类可以被编译器用于隐式类型转换，除非该构造函数被声明为 explicit。

具体来说，当我们尝试比较数组a和数组b的一个元素b[5]时，如果写成了a == b[5]，编译器可能会尝试将b[5]（一个int）隐式转换为一个Array<int>对象，而这显然不是我们期望的行为。

（1）使用explicit关键字

explicit关键字可以防止单参数构造函数被用于隐式类型转换。当构造函数被声明为explicit时，它只能用于直接初始化，而不能用于隐式类型转换。

```plain
template<typename T>
class Array {
public:
    explicit Array(int size) : _size(size), _data(new T[size]) {}
    T& operator[](int index) {
        return _data[index];
    }
    // 其他成员函数...
private:
    int _size;
    T* _data;
};
// 使用
Array<int> a(10); // 正确
// Array<int> b = 8; // 错误，不能隐式转换
```

（2）创建代理类ArraySize

另一种方法是创建一个代理类ArraySize，该类仅用于表示数组的大小。这样，只有ArraySize对象才能用于构造Array对象，从而避免了隐式类型转换。（封装基本类型）

```plain
template<typename T>
class Array {
public:
    class ArraySize {
    public:
        ArraySize(int numElements) : theSize(numElements) {}
        int size() const { return theSize; }
    private:
        int theSize;
    };
    Array(ArraySize size) : _size(size.size()), _data(new T[_size]) {}
    T& operator[](int index) {
        return _data[index];
    }
    // 其他成员函数...
private:
    int _size;
    T* _data;
};
```

```plain
Array<int> a(Array<int>::ArraySize(10)); // 正确
// Array<int> b = 8; // 错误，不能隐式转换
```

在这个版本中，Array的构造函数接受一个ArraySize对象。这意味着你必须明确地创建一个ArraySize对象来初始化Array对象，如下8所示：

```plain
Array<int> a(Array<int>::ArraySize(10)); // 正确
// Array<int> b = 8; // 错误，不能隐式转换
```

如果尝试像下面这样写代码：

```plain
if (a == b[5]) { // 错误！不能隐式转换int到Array<int>
    //...
}
```

编译器会报告错误，因为不能隐式地将int转换为Array<int>。因为b[5]是一个int，Array的构造函数现在需要一个ArraySize对象，这阻止了隐式类型转换的发生。

<h4 id="eeFaf">**Item M6：自增、自减操作符前缀形式与后缀形式的区别**</h4>
自增(++)和自减(--)操作符有两种形式：前缀形式和后缀形式。

前缀形式在操作之后立即返回更新后的值，而后缀形式则返回操作之前的值，并在返回之后才更新值。

为了在自定义类中实现这两种行为，C++提供了特殊的语法来区分前缀和后缀形式。

前缀形式 (++obj 或 --obj)：没有额外参数。

后缀形式 (obj++ 或 obj--)：需要一个额外的int参数，这个参数通常被忽略，只是为了区分前缀和后缀形式。

前缀形式 返回引用(T&)，以便可以链式调用。后缀形式 返回常量对象(const T)，以防止进一步的修改，并且避免连续的后缀操作。

自定义整数类UPInt，下面是UPInt类中++和--操作符的实现：

```plain
class UPInt {
public:
    UPInt& operator++() {
        *this += 1;  // 增加值
        return *this; // 返回更新后的引用
    }
    const UPInt operator++(int) {
        UPInt oldValue = *this; // 存储旧值
        ++(*this);             // 调用前缀自增
        return oldValue;       // 返回旧值
    }
    UPInt& operator--() {
        *this -= 1;  // 减少值
        return *this; // 返回更新后的引用
    }
    const UPInt operator--(int) {
        UPInt oldValue = *this; // 存储旧值
        --(*this);             // 调用前缀自减
        return oldValue;       // 返回旧值
    }
    UPInt& operator+=(int value) {
        // ...
        return *this;
    }
    UPInt& operator-=(int value) {
        // ...
        return *this;
    }
};
```

```plain
UPInt i;
++i;  //调用前缀自增
i++;  //调用后缀自增
--i;  //调用前缀自减
i--;  //调用后缀自减
```

_**前缀形式：**_

```plain
UPInt& operator++() 和 UPInt& operator--()
```

直接修改对象并返回引用。效率较高，因为它不需要创建临时对象。

_**后缀形式：**_

```plain
const UPInt operator++(int) 和 const UPInt operator--(int)
```

创建一个临时对象保存旧值，然后调用前缀形式进行更新，最后返回旧值。返回const对象是为了防止连续的后缀操作，如i++++，这会导致未定义行为，因为i++返回的是一个临时对象，不能再次进行自增操作。

_**为什么后缀形式返回const对象？**_

防止连续的后缀操作：如i++++，这会导致未定义行为，因为i++返回的是一个临时对象，不能再次进行自增操作。

保持一致性：与内置类型的int一致，int类型也不允许连续的后缀操作。

前缀形式更高效，因为它不需要创建临时对象。后缀形式需要创建临时对象，因此在性能敏感的场景中，应优先使用前缀形式。

后缀形式基于前缀形式实现：这样可以确保两者的行为一致，减少了维护的复杂性。

只需要维护前缀形式的实现，后缀形式自动跟随前缀形式的行为。

<h4 id="xSODT">**Item M7：不要重载“&&”,“||”, 或“,”**</h4>
&&（逻辑与)，||(逻辑或)和,（逗号）操作符具有特定的短路求值行为,表达式的某些部分可能不会被执行。例如，对于&&操作符，如果左侧表达式为false，则右侧表达式不会被评估；对于||操作符，如果左侧表达式为true，则右侧表达式不会被评估。逗号操作符总是从左到右依次计算各个子表达式，并返回最后一个表达式的结果。

然而，C++允许用户自定义类型重载这些操作符。一旦重载了这些操作符，就会失去原有的短路求值特性，因为重载后的操作符实际上变成了函数调用。

C++标准没有定义函数参数的计算顺序，因此重载后的操作符可能无法保证从左到右的计算顺序。重载这些操作符会使代码变得难以理解和维护，因为它们的行为与内置操作符不同。例如，其他开发者可能会期望&&和||具有短路求值特性，但重载后的操作符并不具备这一特性。因此，不应该重载&&、||和,操作符，因为这样做会破坏这些操作符的自然行为，使得代码变得难以理解和维护。

逻辑与&&操作符

自定义类型MyBool，并尝试重载&&操作符：

```plain
class MyBool {
public:
    bool value;
    MyBool(bool v) : value(v) {}
    // 重载&&操作符
    MyBool operator&&(const MyBool& other) const {
        return MyBool(value && other.value);
    }
};
MyBool a(true);
MyBool b(false);
if(a&&b){
// 调用 a.operator&&(b)
// 逻辑块
}
```

在这个例子中，a && b会被解析为a.operator&&(b)。这会导致a和b都被计算，失去了短路求值的特性。如果a的值为false，原本b不应该被计算，但现在b仍然会被计算。

<h4 id="FcKMn">**Item M8：理解各种不同含义的 new 和 delete**</h4>
(见其他整理)

new 操作符：分配内存并调用构造函数。

operator new：分配原始内存。

placement new：在已分配的内存中构造对象。

delete 操作符：调用析构函数并释放内存。

operator delete：释放内存。

数组 new 和 delete：分别为数组分配和释放内存，并调用每个元素的构造函数和析构函数。

<h1 id="sjNUC">**异常**</h1>
异常处理的重要性：

强制性：异常不能被忽略，这与传统的错误代码不同，后者可能被忽略。

安全性：异常确保了错误状态被正确处理，而非让程序继续运行在一个不确定的状态下。

异常安全编程：

异常安全的程序不是偶然形成的，而是通过精心设计得到的。就像一个多线程环境下的程序需要专门设计一样，异常安全的程序也需要特别考虑。

（1）确保资源正确释放，即使在发生异常的情况下也是如此。

（2）在异常被抛出后，程序应该能够保持一致的行为，即要么完全成功，要么完全回滚到之前的状态。

<h4 id="ymkvQ">Item M9：使用析构函数防止资源泄漏</h4>
使用裸指针来管理动态分配的资源可能会导致资源泄漏，尤其是在异常抛出的情况下。为了避免这种情况，可以利用对象的生命周期和析构函数来自动化资源的清理过程。这种技术的核心在于资源获取即初始化(RAII)，它确保资源在其管理对象的生命周期内被妥善管理和释放。

_**例子场景**_

基类 ALA（动物）和两个派生类 Puppy（小狗）和 Kitten（小猫），每个类都有一个processAdoption 方法来处理领养流程。在处理领养过程中，如果使用裸指针来管理动态创建的对象，那么当 processAdoption 方法抛出异常时，可能会导致内存泄漏，因为后续的 delete 操作会被跳过。

_**解决方案**_

使用try-catch块：可以在调用 processAdoption 时包裹一个try-catch块，确保即使抛出异常也能正确释放资源。

```plain
void processAdoptions(istream& dataSource) {
    while (dataSource) {
        ALA *pa = readALA(dataSource);
        try {
            pa->processAdoption();
        } catch (...) {
            delete pa; // 避免内存泄漏
            throw; // 重新抛出异常
        }
        delete pa; // 正常情况下的资源释放
    }
}
```

使用智能指针：使用C++标准库提供的 std::auto_ptr 或者更现代的 std::unique_ptr 来自动管理资源。这些智能指针会在其生命周期结束时自动调用 delete，从而防止资源泄漏。

```plain
#include <memory>  // 引入 std::unique_ptr
void processAdoptions(istream& dataSource) {
    while (dataSource) {
        std::unique_ptr<ALA> pa(readALA(dataSource));
        pa->processAdoption();  // 不需要额外的 try-catch 或 delete
    }
}
```

RAII技术的应用：除了内存管理外，RAII还可以应用于其他类型的资源，如窗口句柄。可以定义一个类来封装窗口的创建和销毁。

```plain
class WindowHandle {
public:
    explicit WindowHandle(WINDOW_HANDLE handle) : w(handle) {}
    ~WindowHandle() { destroyWindow(w); }
    operator WINDOW_HANDLE() const {return w;}
private:
    WINDOW_HANDLE w;
};
void displayInfo(const Information& info) {
    WindowHandle w(createWindow());
    // 在 w 对应的窗口中显示信息
}
```

这种方式同样确保了即使在异常情况下，窗口资源也会被正确地销毁。通过使用智能指针和RAII模式，我们可以有效地防止由于异常导致的资源泄漏。

C++新增的异常（exception）机制改变了某些事情，这种改变是深刻的，彻底的，可能

是令人不舒服的。例如：使用未经处理的或原始的指针变得很危险；资源泄漏的可能性增加

了；写出具有你希望的行为的构造函数与析构函数变得更加困难。特别小心防止程序执行时

突然崩溃。执行程序和库程序尺寸增加了，同时运行速度降低了。 

这就使我们所知道的事情。很多使用 C++的人都不知道在程序中使用异常，大多数人不

知道如何正确使用它。在异常被抛出后，使软件的行为具有可预测性和可靠性，在众多方法

中至今也没有一个一致的方法能做到这点。（为了深刻了解这个问题，参见 Tom Cargill 写

的 Exception Handling: A False Sense of Security。有关这些问题的进展情况的信息，

参见 Jack Reeves 写的 Coping with Exceptions 和 Herb Sutter 写的 Exception-Safe Generic Containers） 

我们知道：程序能够在存在异常的情况下正常运行是因为它们按照要求进行了设计，而不是因为巧合。异常安全（Exception-safe）的程序不是偶然建立的。一个没有按照要求进

行设计的程序在存在异常的情况下运行正常的概率与一个没有按照多线程要求进行设计的

程序在多线程的环境下运行正常的概率相同，概率为 0。 

<u>为什么使用异常呢？自从 C 语言被发明初来，C 程序员就满足于使用错误代码（Error </u>

<u>code），所以为什么还要弄来异常呢，特别是如果异常如我上面所说的那样存在着问题。答</u>

<u>案是简单的：异常不能被忽略。如果一个函数通过设置一个状态变量或返回错误代码来表示</u>

<u>一个异常状态，没有办法保证函数调用者将一定检测变量或测试错误代码。结果程序会从它</u>

<u>遇到的异常状态继续运行，异常没有被捕获，程序立即会终止执行。 </u>

<u>C 程序员能够仅通过 setjmp 和 longjmp 来完成与异常处理相似的功能</u>。但是当 longjmp

在 C++中使用时，它存在一些缺陷，当它调整堆栈时不能对局部对象调用析构函数。

（WQ 加注，VC＋＋能保证这一点，但不要依赖这一点。）而大多数 C＋＋程序员依赖于这些析构函数的调用，所以 setjmp 和 longjmp 不能够替换异常处理。如果你需要一个方法，能够通知

不可被忽略的异常状态，并且搜索栈空间（searching the stack）以便找到异常处理代码

时，还得确保局部对象的析构函数必须被调用，这时就需要使用 C++的异常处理。 

因为已经对使用异常处理的程序设计有了很多了解，下面这些条款仅是一个对于写出异常安全（Exception-safe）软件的不完整的指导。然而它们给任何在 C++中使用异常处

理的人介绍了一些重要思想。通过留意下面这些指导，你能够提高自己软件的正确性，强壮

性和高效性，并且你将回避开许多在使用异常处理时经常遇到的问题。

<h4 id="lSySV">Item M10：在构造函数中防止资源泄漏</h4>
在C++中，构造函数可能抛出异常，导致对象的部分构造。这种情况下，C++不会自动调用析构函数来清理已分配的资源，从而可能导致资源泄漏。为了解决这个问题，需要在构造函数内部适当地管理资源，确保即使在异常情况下也能正确释放资源。

假设我们正在开发一个多媒体通讯录程序，其中每个条目可以包含文字信息（如姓名、地址）、照片和声音片段。我们定义了以下类：

Image：用于处理图像数据。

AudioClip：用于处理声音数据。

PhoneNumber：用于存储电话号码。

BookEntry：通讯录中的条目，包含姓名、地址、电话号码、照片和声音片段。

初始的 BookEntry 构造函数和析构函数如下：

```plain
class BookEntry {
public:
    BookEntry(const string& name,
              const string& address = "",
              const string& imageFileName = "",
              const string& audioClipFileName = "");
    ~BookEntry();
    void addPhoneNumber(const PhoneNumber& number);
private:
    string theName;
    string theAddress;
    list<PhoneNumber> thePhones;
    Image* theImage;
    AudioClip* theAudioClip;
};
BookEntry::BookEntry(const string& name,
                     const string& address,
                     const string& imageFileName,
                     const string& audioClipFileName)
    : theName(name), theAddress(address), theImage(nullptr), theAudioClip(nullptr) {
    if (imageFileName != "") {
        theImage = new Image(imageFileName);
    }
    if (audioClipFileName != "") {
        theAudioClip = new AudioClip(audioClipFileName);
    }
}
BookEntry::~BookEntry() {
    delete theImage;
    delete theAudioClip;
}
```

如果在构造函数中抛出异常（例如，new 失败或 Image/AudioClip 构造函数抛出异常），BookEntry 对象将不会完全构造，析构函数不会被调用，从而导致资源泄漏。

_**解决方案**_

使用 try-catch 块

在构造函数中使用 try-catch 块来捕获异常，并在捕获异常后手动释放资源，然后再重新抛出异常。

```plain
BookEntry::BookEntry(const string& name,
                     const string& address,
                     const string& imageFileName,
                     const string& audioClipFileName)
    : theName(name), theAddress(address), theImage(nullptr), theAudioClip(nullptr) {
    try {
        if (imageFileName != "") {
            theImage = new Image(imageFileName);
        }
        if (audioClipFileName != "") {
            theAudioClip = new AudioClip(audioClipFileName);
        }
    } catch (...) {
        delete theImage;
        delete theAudioClip;
        throw;
    }
}
```

使用辅助函数

将资源释放的代码提取到一个私有辅助函数 cleanup 中，供构造函数和析构函数共同使用。

```plain
class BookEntry {
public:
    BookEntry(const string& name,
              const string& address = "",
              const string& imageFileName = "",
              const string& audioClipFileName = "");
    ~BookEntry();
    void addPhoneNumber(const PhoneNumber& number);
private:
    string theName; string theAddress;
    list<PhoneNumber>thePhones;
    Image* theImage;
    AudioClip* theAudioClip;
    void cleanup();
};
void BookEntry::cleanup() {
    delete theImage;
    delete theAudioClip;
}
BookEntry::BookEntry(const string& name,
                     const string& address,
                     const string& imageFileName,
                     const string& audioClipFileName)
    : theName(name), theAddress(address), theImage(nullptr), theAudioClip(nullptr) {
    try {
        if (imageFileName != "") {
            theImage = new Image(imageFileName);
        }
        if (audioClipFileName != "") {
            theAudioClip = new AudioClip(audioClipFileName);
        }
    } catch (...) {
        cleanup();
        throw;
    }
}
BookEntry::~BookEntry() {
    cleanup();
}
```

使用智能指针

使用 std::auto_ptr 或 std::unique_ptr 来管理资源，从而自动处理资源的释放。

```plain
#include <memory>
class BookEntry {
public:
    BookEntry(const string& name,
              const string& address = "",
              const string& imageFileName = "",
              const string& audioClipFileName = "");
    ~BookEntry();
    void addPhoneNumber(const PhoneNumber& number);
private:
    string theName;
    string theAddress;
    list<PhoneNumber> thePhones;
    std::unique_ptr<Image> theImage;
    std::unique_ptr<AudioClip> theAudioClip;
};
BookEntry::BookEntry(const string& name,
                     const string& address,
                     const string& imageFileName,
                     const string& audioClipFileName)
    : theName(name)，theAddress(address)，theImage(imageFileName != "" ? new Image(imageFileName) : nullptr),
      theAudioClip(audioClipFileName != "" ? new AudioClip(audioClipFileName) : nullptr) {
}
BookEntry::~BookEntry() {
    // 无需手动删除，智能指针会自动处理
}
```

通过在构造函数中使用 try-catch 块和智能指针，我们可以确保即使在异常情况下也能正确释放资源。智能指针（如 std::unique_ptr）提供了一种更加简洁和安全的方式来管理动态分配的资源，避免了手动管理指针带来的复杂性和潜在的错误。

<h4 id="KrzUN">Item M11：禁止异常信息传递到析构函数外</h4>
析构函数在两种情况下会被调用：一是对象正常离开作用域或被显式删除时；二是在异常处理过程中，异常堆栈展开。

析构函数可能会在异常激活的状态下调用。如果析构函数本身抛出异常，程序将调用 std::terminate 函数，这会导致程序立即终止，而不执行任何进一步的清理工作。析构函数不应抛出异常，以确保程序的健壮性和资源的正确释放。

假设有一个 Session 类，用于跟踪在线会话的创建和销毁时间。需要记录会话的创建和销毁信息。

初始的 Session 类及其析构函数如下

```plain
#include<iostream>
class Session {
public:
    Session();
    ~Session();
private:
    static void logCreation(Session *objAddr);
    static void logDestruction(Session *objAddr);
};
Session::Session() {
    logCreation(this);
}
Session::~Session() {
    logDestruction(this);
}
void Session::logCreation(Session *objAddr) {
    // 记录会话创建
    std::cout << "Session created at address: " << objAddr << std::endl;
}
void Session::logDestruction(Session *objAddr) {
    // 记录会话销毁
    std::cout << "Session destroyed at address: " << objAddr << std::endl;
}
```

如果 logDestruction 抛出异常，异常将传递到 Session 的析构函数外部。如果此时正处于异常堆栈展开过程中，std::terminate 将被调用，程序将立即终止，导致资源泄漏或其他未定义行为。

_**解决方案**_

使用 try-catch 块捕获异常

在析构函数中使用 try-catch 块来捕获并处理可能的异常，确保异常不会传递到析构函数外部。

```plain
Session::~Session() {
    try {
        logDestruction(this);
    } catch (...) {
        // 忽略异常，防止异常传递到析构函数外部
    }
}
```

确保析构函数完成所有必要的清理工作。如果析构函数中有多个操作，确保这些操作都能完成，即使其中一个操作抛出异常。

```plain
#include <iostream>
class Session {
public:
    Session();
    ~Session(); 
   //其他成员函数
private:
    static void logCreation(Session *objAddr);
    static void logDestruction(Session *objAddr);
    void startTransaction();
    void endTransaction();
};
Session::Session() {
    logCreation(this);
    startTransaction();
}
Session::~Session() {
    try {
        logDestruction(this);
        endTransaction();
    } catch (...) {
        // 忽略异常，防止异常传递到析构函数外部
    }
}
```

使用 try-catch 块：

在析构函数中使用 try-catch 块捕获并处理可能的异常，确保异常不会传递到析构函数外部。这样可以防止 std::terminate 被调用，从而避免程序立即终止。

确保所有操作完成：

在析构函数中，如果有多个操作需要完成，确保这些操作都能完成，即使其中一个操作抛出异常。通过这种方式，即使 logDestruction 或 endTransaction 抛出异常，析构函数也能完成必要的清理工作。

<h4 id="FzB33">Item M12：理解“抛出一个异常”与“传递一个参数”或“调用一个虚函数”间的差异</h4>
在C++中，抛出异常、传递参数和调用虚函数虽然在语法上看起来有些相似，但它们在实际操作过程中有着显著的区别。这些区别主要体现在以下几个方面：对象拷贝、类型转换以及匹配规则。

相同点:无论是传递参数还是抛出异常，都可以通过值、引用或指针来进行。两者都可以使用引用或指针来避免不必要的拷贝。

（这条没有看懂）

差异

对象拷贝

传递参数：当通过值传递参数时，会创建参数的一个副本。通过引用或指针传递则不会创建副本。

抛出异常：抛出异常时，总是会创建异常对象的一个副本，即使通过引用捕获也是如此。这是因为异常对象在其抛出点可能已经离开了作用域，需要确保在捕获点有一个有效的副本。

当在C++中抛出异常时，系统会创建异常对象的一个副本，即使在捕获异常时使用的是引用。这样做的目的是确保在异常传播过程中，即使原始异常对象已经离开其作用域，仍然有一个有效的异常对象副本可以被处理。

```plain
#include <iostream>
#include <stdexcept>
class MyException : public std::exception {
public:
    MyException(const char* msg) : message(msg) {
        std::cout << "MyException constructor called" << std::endl;
    }
    MyException(const MyException& other) : message(other.message) {
        std::cout << "MyException copy constructor called" << std::endl;
    }
    const char* what() const noexcept override {
        return message;
    }
private:
    const char* message;
};
void throwException() {
    MyException e("An error occurred");
    std::cout << "Throwing exception" << std::endl;
    throw e;  // 抛出异常
}
int main() {
    try {
        throwException();
    } catch (const MyException& e) {  // 通过引用捕获异常
        std::cout << "Caught exception: " << e.what() << std::endl;
    }
    return 0;
}
```

自定义异常类MyException 类继承自 std::exception，并重写了 what() 方法。MyException 有一个构造函数和一个拷贝构造函数，用于输出构造和拷贝构造的调用信息。

抛出异常：throwException 函数中，创建了一个 MyException 对象 e。

通过 throw e; 语句抛出异常。这时，系统会调用 MyException 的拷贝构造函数，创建 e 的一个副本，并将这个副本传递给异常处理机制。

捕获异常：

main 函数中的 try 块调用了 throwException 函数。当 throwException 抛出异常时，控制权会转移到 catch 块。catch (const MyException& e) 通过引用捕获异常。尽管这里使用的是引用，但实际上捕获的是在抛出点创建的副本。

类型转换

传递参数：C++允许隐式的类型转换，例如从int到double。

抛出异常：异常捕获时不允许隐式类型转换。唯一的例外是基类和派生类之间的转换，以及从类型化的指针到const void*的转换。

匹配规则

传递参数：虚函数调用遵循动态绑定，即根据对象的实际类型调用相应的虚函数。

抛出异常：异常捕获遵循顺序匹配规则，即按照catch子句在代码中出现的顺序进行匹配，第一个匹配成功的catch子句将被调用。

```plain
#include <iostream>
#include <stdexcept>
#include <cmath>
class Widget {
public:
    Widget() { std::cout << "Widget constructed\n"; }
    Widget(const Widget&) { std::cout << "Widget copy constructed\n"; }
    ~Widget() { std::cout << "Widget destructed\n"; }
};
class SpecialWidget : public Widget {
public:
    SpecialWidget() { std::cout << "SpecialWidget constructed\n"; }
    SpecialWidget(const SpecialWidget&) { std::cout << "SpecialWidget copy constructed\n"; }
    ~SpecialWidget() { std::cout << "SpecialWidget destructed\n"; }
};
void passAndThrowWidget() {
    Widget localWidget;
    std::cin >> localWidget; // 传递 localWidget 到 operator>>
    throw localWidget; // 抛出 localWidget 异常
}
void passAndThrowSpecialWidget() {
    SpecialWidget localSpecialWidget;
    Widget& rw = localSpecialWidget; // rw 引用 SpecialWidget
    throw rw; // 抛出一个类型为 Widget 的异常
}
void testExceptionHandling() {
    try {
        passAndThrowWidget();
    } catch (Widget& w) {
        std::cout << "Caught Widget by reference\n";
    }
    try {
        passAndThrowSpecialWidget();
    } catch (Widget& w) {
        std::cout << "Caught Widget by reference\n";
    }
}
void testRethrow() {
    try {
        throw std::runtime_error("An error occurred");
    } catch (std::exception& e) {
        std::cout << "Caught an exception: " << e.what() << "\n";
        throw; // 重新抛出当前异常
    }
}
int main() {
    testExceptionHandling();
    testRethrow();
    return 0;
}
```

_**对象拷贝**_

passAndThrowWidget函数中，localWidget被抛出时，会创建一个副本。即使通过引用捕获异常，也会创建一个副本。在passAndThrowSpecialWidget函数中，尽管rw引用的是SpecialWidget，但抛出的是Widget类型的异常。这是因为rw的静态类型是Widget。

_**类型转换**_

在testExceptionHandling函数中，passAndThrowSpecialWidget抛出的Widget异常可以被Widget&的catch子句捕获，但不能被SpecialWidget&的catch子句捕获，除非明确指定。

通过const void*可以捕获任何指针类型的异常。

_**匹配规则**_

在testRethrow函数中，throw;重新抛出当前异常，保持其原始类型。而throw e;会创建一个新的异常对象，其类型为e的静态类型。

（1）对象拷贝：异常抛出时总是会创建副本，而参数传递可以根据传递方式决定是否创建副本。

（2）类型转换：异常捕获时不允许隐式类型转换，只有基类与派生类之间的转换以及从类型化的指针到const void*的转换是允许的。

（3）匹配规则：异常捕获遵循顺序匹配规则，而虚函数调用遵循动态绑定。

<h4 id="nqTlu">**Item M13：通过引用（reference）捕获异常**</h4>
通过指针捕获异常：这种方式看似高效，因为它避免了对象的拷贝。如果抛出的是局部对象的指针，那么当控制流离开抛出异常的函数时，该对象会被销毁，导致指针悬空。如果抛出的是动态分配的对象指针，那么需要在catch块中负责清理（删除）该对象，这容易被遗忘，造成内存泄漏。

标准异常类型（如std::bad_alloc等）不是指针，因此不能通过指针来捕获。

（1）通过值捕获异常：这种方式解决了通过指针捕获的问题，但是它会导致异常对象被拷贝两次，并且如果抛出的是派生类对象而捕获的是基类，则会发生对象切割（slicing），丢失派生类特有的部分。

（2）通过引用捕获异常：这是推荐的做法，因为它避免了对象的拷贝，不会发生对象切割，可以正确地处理继承关系，并且可以直接捕获标准异常类型。

```plain
//基类异常
class MyException : public std::exception {
public:
    virtual const char* what() const throw() {
        return "MyException occurred";
    }
};
```

```plain
//派生类异常
class DerivedException : public MyException {
public:
    const char* what() const throw() override {
        return "DerivedException occurred";
    }
};
```

```plain
//抛出异常的函数
void someFunction(bool shouldThrow) {
    if (shouldThrow) {
        throw DerivedException();
    }
}
```

```plain
//主函数
int main() {
    try {
        someFunction(true); // 抛出异常
    } catch (const MyException& e) { // 通过引用捕获异常
        std::cerr << e.what() << '\n'; // 输出派生类的 what()
    }
    return 0;
}
```

someFunction函数会在特定条件下抛出一个DerivedException对象。在main函数中，我们通过catch (const MyException& e)语句来捕获异常。由于我们使用了引用，所以即使捕获的是基类MyException，实际调用的还是DerivedException的what()方法，这样就避免了对象切割的问题。同时，异常对象只被拷贝了一次，提高了性能。

<h4 id="Y1nCV">**Item M14：审慎使用异常规格(exception specifications)**</h4>
C++中异常规格的使用及其潜在的问题。异常规格是一种在函数声明中指定函数可能抛出哪些异常类型的机制。虽然异常规格看起来很有吸引力，因为它提供了关于函数行为的额外文档，但实际上它们可能会引发一系列问题。

异常规格的主要问题

（1）意外的程序终止：如果一个函数抛出了不在其异常规格中列出的异常，系统会调用unexpected函数，该函数默认会调用terminate，最终可能导致程序非正常终止，而不会释放栈上的局部变量。

（2）编译器检测有限：编译器只能部分检测异常规格的一致性。如果一个函数调用另一个函数，而后者抛出的异常不在前者的异常规格之内，编译器可能不会报错，但程序在运行时会出错。

（3）模板与异常规格的兼容性：由于模板实例化时类型参数的不确定性，很难为模板函数提供有意义的异常规格。

unexpected的处理：即使高阶的调用者准备好处理某些异常，如果底层函数违反了异常规格，unexpected仍然会被调用，从而可能中断异常处理流程。

示例 1：违反异常规格的情况

```plain
extern void f1(); // 可以抛出任意的异常
void f2() throw(int) {
    // 即使 f1 可能抛出不是 int 类型的异常，这也是合法的
    f1();
}
```

f2函数声明了它可以抛出int类型的异常，但它调用了f1，而f1可能抛出任何类型的异常。如果f1抛出了非int类型的异常，那么f2的异常规格就被违反了。

在C++中，throw(int) 是一个异常规格（exception specification），它出现在函数声明或定义的末尾，用来指定该函数可能抛出的异常类型。具体来说，throw(int) 表示这个函数只能抛出 int 类型的异常，或者不抛出任何异常。

异常规格的语法是在函数声明或定义的参数列表之后加上 throw 关键字，后面跟着一个括号，括号内可以列出函数可能抛出的异常类型，多个类型之间用逗号分隔。如果没有列出任何类型，即 throw()，则表示这个函数承诺不会抛出任何异常。

下面是一个使用 throw(int) 异常规格的函数示例：

```plain
void someFunction() throw(int) {
    // 函数体
    if (/* 某些条件 */) {
        throw 7; // 抛出一个整数异常
    }
    // 其他代码
}
```

someFunction 函数声明了它只会抛出 int 类型的异常。如果这个函数试图抛出其他类型的异常，那么程序的行为将是未定义的，通常会调用 std::unexpected 函数，这可能导致程序终止。

异常规格在C++11及以后的标准中已经被标记为弃用（deprecated），取而代之的是 noexcept 关键字。noexcept 提供了一种更简洁的方式来指定函数是否可以抛出异常。例如：

noexcept 表示函数不会抛出任何异常。

noexcept(false) 表示函数可能会抛出异常。

noexcept(expression) 表示函数是否会抛出异常取决于表达式 expression 的结果。

<h4 id="OpNeB">Item M15:异常处理的系统开销</h4>
_**基本开销：**_

即使不使用 try, throw, 或 catch，编译器也需要维护一些数据结构来支持异常处理。这些数据结构用来跟踪对象的构造状态、对象的生命周期以及异常处理的相关信息。这些开销通常很小，但如果程序完全不使用异常处理，编译器通常可以生成更小、更快的代码。

_**编译器支持：**_

理论上，C++编译器必须支持异常处理，即使程序中没有任何地方使用异常。实际上，许多编译器提供了选项来禁用异常处理的支持。如果你确定程序和所有链接的库都不使用异常，可以通过禁用异常处理来优化程序的大小和性能。

_**try 块的开销：**_

使用 try 块会增加额外的代码和运行时开销。不同的编译器实现方式不同，但通常会增加 5% 到 10% 的代码尺寸和相应的运行时开销。为了避免不必要的开销，应仅在真正需要捕获异常的地方使用 try 块。

_**抛出异常的开销：**_

抛出异常的开销通常比正常函数返回大得多，可能慢几个数量级。由于异常通常是罕见的情况，这些开销通常不会显著影响整体性能。但如果频繁抛出异常，会对性能产生严重影响。应避免使用异常来表示常见的情况，如控制流的结束或循环的退出。

优化异常处理开销的方法

禁用异常处理：如果确定程序和所有链接的库都不使用异常，可以在编译时禁用异常处理。这可以显著减小程序的大小并提高性能。

_**最小化 try 块的使用：**_

只在确实需要捕获异常的地方使用 try 块，避免无用的 try 块。例如，如果某个函数内部的操作不太可能抛出异常，就不必将其包裹在 try 块中。谨慎使用异常规格：避免使用旧式的异常规格（如 throw(int)），改用 noexcept。例如使用 noexcept 来指定函数不会抛出异常，或者使用 noexcept(false) 来表示函数可能会抛出异常。仅在异常情况下抛出异常：异常应该是罕见的、异常的情况，而不是常见的控制流手段。例如，不要用异常来控制循环的退出或表示函数的正常返回。

<h1 id="LBXDl">效率</h1>
<h4 id="q18ZO">**Item M16：牢记 80－20 准则(80－20 rule)**</h4>
80-20准则是大约20%的代码负责了80%的资源消耗、运行时间、内存使用、磁盘访问等关键性能指标,同时80%的维护工作也集中在大约20%的代码。

80-20准则的意义

不必过分担心大部分代码的性能，因为这些代码的效率对整体影响不大。当出现性能问题时，需要精确定位并优化那20%的关键代码。

很多开发者倾向于根据个人经验和直觉来猜测性能瓶颈所在，但这种方式往往不准确，甚至可能导致无效的优化努力。在I/O受限的应用中优化CPU性能，或是在CPU密集型任务中优化I/O性能，都不会带来明显的性能提升。使用性能分析工具（Profiler）：应该依赖专业的性能分析工具来准确识别程序中的性能瓶颈。Profiler可以提供关于程序各部分运行时间、资源消耗等详细数据，帮助开发者集中精力优化真正重要的部分。最终目标是改善用户的体验，减少等待时间，而不是单纯地减少语句数量或函数调用次数。

<h4 id="NjfvX">**Item M17：考虑使用 lazy evaluation**</h4>
⭐

关联知识点：**copy-on-write**

Lazy Evaluation 是一种延迟计算的策略，其核心思想是“推迟计算直到必要时”。只有当确实需要某个值时，程序才会进行相应的计算。这种方法可以减少不必要的计算，从而提高程序的效率。

_**<u>1. 引用计数</u>**_

拓展

在 C++ 中，std::string 类可以通过引用计数来实现懒惰计算。当一个字符串对象被复制时，不是立即创建一个新的字符串副本，而是让多个对象共享同一个底层数据。只有当其中一个对象需要修改时，才创建独立的副本。

```plain
class String {
public:
  String(const char* str):data(new std::string(str)),refCount(new int(1)){}
  String(const String& other) : data(other.data),refCount(other.refCount){(*refCount)++;}
  ~String(){decrementRefCount();}
  void convertToUpperCase() {
      if (*refCount > 1) {
            data = new std::string(*data);
            refCount = new int(1);
      }
      for (char& c : *data) {
            c = toupper(c);
      }
  }
private:
   std::string* data;
   int* refCount;
   void decrementRefCount() {
        if (--(*refCount) == 0) {
            delete data;
            delete refCount;
        }
   }
};
int main() {
    String s1 = "Hello";
    String s2 = s1; // 不立即创建副本
    s2.convertToUpperCase(）); // 创建副本并修改
    return 0;
}
```

**2. 区别对待读取和写入**

在 operator[] 的实现中，可以区分读操作和写操作。读操作可以直接返回共享数据，而写操作则需要创建独立的副本。

```plain
class String {
public:
    char& operator[](size_t index) {
        if (isShared()) {
            makeUnique();
        }
        return data[index];
    }
    const char& operator[](size_t index) const {
        return data[index];
    }
private:
    std::string data;
    int* refCount;
    bool isShared() const {
        return refCount != nullptr && *refCount > 1;
    }
    void makeUnique() {
       if (isShared()) {
            data = std::string(data);
            refCount = new int(1);
        }
    }
};
int main() {
    String s = "Homer's Iliad";
    std::cout << s[3]; // 读操作
    s[3] = 'x'; // 写操作
    return 0;
}
```

非常量版本:当 s1[0]='h'时,isShared()返回 true，因此调用 makeUnique()创建了一个独立的副本。修改 s1 的第一个字符后，s1 变为 "hello"，而 s2 仍然是 "Hello"。

常量版本:当读取 s2 的第一个字符时，调用的是常量版本的 operator[]，不会创建独立的副本，直接返回共享数据中的字符引用。

_**3. Lazy Fetching（懒惰提取）**_

在处理大型持久对象时，可以使用懒惰提取来延迟从数据库中读取数据，直到确实需要时才读取。

```plain
class LargeObject {
public:
LargeObject(ObjectID id):oid(id),field1Value(nullptr),field2Value(nullptr){}
    const std::string& field1() const {
        if (!field1Value) {
            field1Value = new std::string(fetchFromDatabase(oid, "field1"));
        }
        return *field1Value;
    }
    int field2() const {
        if (!field2Value) {
            field2Value = new int(fetchFromDatabase(oid, "field2"));
        }
        return *field2Value;
    }
private:
   ObjectID oid;
   mutable std::string* field1Value;
   mutable int* field2Value;
std::string fetchFromDatabase(ObjectID id, const std::string& fieldName) const{
 return "some value";
}
};
int main() {
    LargeObject obj(123);
    std::cout << obj.field2(); // 懒惰提取 field2
    return 0;
}
```

在构造函数中，field1Value 和 field2Value 被初始化为 nullptr，表示这些字段尚未从数据库中读取。当调用 field1() 方法时，首先检查 field1Value 是否为 nullptr。如果 field1Value 为 nullptr，表示 field1 尚未从数据库中读取，此时调用 fetchFromDatabase 方法从数据库中读取 field1 的值，并将其存储在 field1Value 指向的新分配的 std::string 对象中，返回field1Value 指向的 std::string 对象的引用。

filed2同理。

_**4 懒惰计算**_

懒惰表达式计算是一种延迟计算的技术，尤其是在处理大型数据结构（如矩阵）时。通过懒惰计算，可以避免不必要的计算和内存分配，从而提高程序的性能。

```plain
class Matrix {
public:
    Matrix(size_t rows, size_t cols) : rows(rows), cols(cols), data(rows * cols, 0) {}
    // 懒惰计算的加法操作
    Matrix operator+(const Matrix& other) const {
        return MatrixExpression<T>(*this, other, '+');
    }
    // 懒惰计算的乘法操作
    Matrix operator*(const Matrix& other) const {
        return MatrixExpression<T>(*this, other, '*');
    }
    // 获取矩阵元素
    T& operator()(size_t row, size_t col) {
        return data[row * cols + col];
    }
    const T& operator()(size_t row, size_t col) const {
        return data[row * cols + col];
    }
    // 显式计算矩阵
    void evaluate() {
        if (expression) {
            expression->evaluate(*this);
            expression = nullptr;
        }
    }
private:
    size_t rows, cols;
    std::vector<T> data;
    std::unique_ptr<MatrixExpression<T>> expression;
    // 表达式类
    class MatrixExpression {
    public:
        MatrixExpression(const Matrix& lhs, const Matrix& rhs, char op)
            : lhs(lhs), rhs(rhs), op(op) {}
        void evaluate(Matrix& result) const {
            if (op == '+') {
                for (size_t i = 0; i < lhs.rows * lhs.cols; ++i) {
                    result.data[i] = lhs.data[i] + rhs.data[i];
                }
            } else if (op == '*') {
                for (size_t i = 0; i < lhs.rows; ++i) {
                    for (size_t j = 0; j < rhs.cols; ++j) {
                        T sum = 0;
                        for (size_t k = 0; k < lhs.cols; ++k) {
                            sum += lhs(i, k) * rhs(k, j);
                        }
                result(i, j) = sum;
    private:
        const Matrix& lhs;
        const Matrix& rhs;
        char op;
    };
};
int main() {
    Matrix<int>m1(1000, 1000);
    Matrix<int>m2(1000, 1000);
    // 懒惰计算m1+m2
    Matrix<int>m3 = m1 + m2;
    // 懒惰计算m4*m1
    Matrix<int>m4(1000, 1000);
    m3=m4*m1;
    // 显式计算 m3 的第四行
    m3.evaluate();
    std::cout << m3(4, 0) << std::endl;
    // 显式计算 m3 的所有值
    m3.evaluate();
    for (size_t i = 0; i < m3.rows; ++i) {
        for (size_t j = 0; j < m3.cols; ++j) {
            std::cout << m3(i, j) << " ";
        }
        std::cout << std::endl;
    }
    return 0;
}
```

懒惰表达式计算的优势：

（1）按需计算:只有在真正需要某个值时，才会进行计算。例如，如果只需要矩阵的一部分，可以避免计算整个矩阵。

（2）节省内存:通过延迟计算，可以减少不必要的内存分配。例如，如果只需要矩阵的某些行或列，可以避免分配整个矩阵的内存。

（3）提高性能:避免不必要的计算和内存分配，特别是在处理大型数据结构时，可以显著提高程序的性能。

懒惰计算 m1 + m2

```plain
Matrix<int> m3 = m1 + m2;
```

计算 m1 + m2,但不立即执行加法操作,而是创建一个 MatrixExpression 对象表示这个表达式。

按需计算 m3 的第四行第一个元素：

```plain
std::cout << m3(4, 0) << std::endl;
```

访问 m3 的第 4 行第 0 列的元素时，调用 evaluateIfNecessary(4, 0) 方法。evaluateIfNecessary 方法检测到存在懒惰表达式，调用 evaluatePartial 方法计算 m3(4, 0) 的值。

<h4 id="RwHVH">**Item M18：分期摊还期望的计算**</h4>
在条款 M18 中，作者讨论了 Over-Eager Evaluation（过度热情计算法） 的概念。 Eager Evaluation（热情计算法） 和 Lazy Evaluation（懒惰计算法） 不同，Over-Eager Evaluation 在需求出现之前就预先完成计算或准备工作，以提高后续操作的效率。

Eager Evaluation：当需要某个值时，立即计算并返回。

Lazy Evaluation：只有在真正需要某个值时，才进行计算。

Over-Eager Evaluation：在需求出现之前，预先完成计算或准备工作，以便在实际需要时能够快速响应。

1. DataCollection 类

```plain
template<class NumericalType>
class DataCollection {
public:
    NumericalType min() const;
    NumericalType max() const;
    NumericalType avg() const;
    ...
};
```

假设 min, max 和 avg 函数分别返回集合的最小值、最大值和平均值。有三种实现方法：

Eager Evaluation：每次调用这些函数时，遍历整个集合并计算结果。

Lazy Evaluation：只有在确实需要这些值时，才计算并缓存结果。

Over-Eager Evaluation：随时跟踪集合的最小值、最大值和平均值，这样在调用这些函数时可以直接返回结果，无需重新计算。

Over-Eager Evaluation 实现

```plain
template<class NumericalType>
class DataCollection {
public:
    void add(NumericalType value) {
        data.push_back(value);
        updateStatistics(value);
    }
    NumericalType min() const {
        return minValue;
    }
    NumericalType max() const {
        return maxValue;
    }
    NumericalType avg() const {
        return averageValue;
    }
private:
    std::vector<NumericalType> data;
    NumericalType minValue;
    NumericalType maxValue;
    NumericalType averageValue;
    size_t count;
    void updateStatistics(NumericalType value) {
        if (count == 0) {
            minValue = value;
            maxValue = value;
            averageValue = value;
        } else {
            if (value < minValue) minValue = value;
            if (value > maxValue) maxValue = value;
            averageValue = (averageValue * count + value) / (count + 1);
        }
        count++;
    }
};
```

每次添加一个新元素时，不仅将元素添加到 data 向量中，还调用 updateStatistics 方法更新最小值、最大值和平均值。

_**定义静态缓存**_

```plain
typedef std::map<std::string, int> CubicleMap;
static CubicleMap cubes;
```

使用 std::map 作为缓存，存储员工姓名和隔间号的映射关系。

_**查找缓存:**_

```plain
CubicleMap::iterator it = cubes.find(employeeName);
```

尝试在缓存中查找员工的隔间号。

_**缓存命中:**_

```plain
if (it == cubes.end()) {
    int cubicle = /* the result of looking up employeeName's cubicle number in the database */;
    cubes[employeeName] = cubicle;
    return cubicle;
} else {
    return (*it).second;
}
```

如果缓存中没有找到员工的隔间号，则从数据库中查询，并将结果添加到缓存中。

如果缓存中已存在员工的隔间号，则直接返回缓存中的值。

预提取示例：DynArray 类

```plain
template<class T>
class DynArray {
public:
    T& operator[](int index) {
        if (index < 0) {
            throw std::out_of_range("Negative index is not allowed");
        }
        if (index >= capacity) {
            int newCapacity = std::max(capacity * 2, index + 1);
            T* newData = new T[newCapacity];
            for (int i = 0; i < capacity; ++i) {
                newData[i] = data[i];
            }
            delete[] data;
            data = newData;
            capacity = newCapacity;
        }
        if (index >= size) {
            size = index + 1;
        }
        return data[index];
    }
private:
    T* data = nullptr;
    int capacity = 0;
    int size = 0;
};
```

动态数组的索引操作:

```plain
T& operator[](int index) {
    if (index < 0) {
        throw std::out_of_range("Negative index is not allowed");
    }
    if (index >= capacity) {
        int newCapacity = std::max(capacity * 2, index + 1);
        T* newData = new T[newCapacity];
        for (int i = 0; i < capacity; ++i) {
            newData[i] = data[i];
        }
        delete[] data;
        data = newData;
        capacity = newCapacity;
    }
    if (index >= size) {
        size = index + 1;
    }
    return data[index];
}
```

检查索引是否为负数，如果是则抛出异常。如果索引超出当前容量，预先分配更大的内存（通常是当前容量的两倍或直接满足索引需求)。如果索引超出当前大小，更新数组的大小。返回指定索引处的元素。

总结：

（1）Over-Eager Evaluation：在需求出现之前预先完成计算或准备工作，以提高后续操作的效率。

（2）Caching：缓存已经计算出来的结果，减少重复计算的开销。

（3）Prefetching：预先分配更多资源，以减少未来扩展的开销。

<h4 id="DGF9Z">**Item M19：理解临时对象的来源**</h4>
代码示例 1: swap 函数

```plain
template <class T>
void swap(T& object1, T& object2) {
    T temp = object1;  // 局部对象，而非临时对象
    object1 = object2;
    object2 = temp;
}
```

代码示例 2: countChar 函数

```plain
size_t countChar(const string& str, char ch) {
    size_t count = 0;
    for (char c : str) {
        if (c == ch) ++count;
    }
    return count;
}
```

代码示例 3: uppercasify 函数

```plain
void uppercasify(string& str) {
    for (char& c : str) {
        c = toupper(c);
    }
}
```

_**局部对象**_

（1） swap 函数中的 temp，它是一个有名字的局部对象，存在于函数的作用域内。当函数执行完毕后，该对象会被销毁。

（2）临时对象：C++ 中的临时对象是没有名字的。

隐式类型转换：当函数参数类型与实际传入的参数类型不匹配时，编译器可能会创建一个临时对象来进行类型转换。例如，在 countChar(buffer, c) 调用中，buffer 是一个 char[] 类型，但 countChar 的参数要求是 const string&，因此编译器会创建一个临时的 string 对象。函数返回对象：当一个函数返回一个对象时，返回的对象通常是一个临时对象。例如，operator+ 返回一个 Number 对象，这个对象是临时的，因为它没有名字，只是作为函数的返回值。

_**临时对象的开销**_

构造和析构：临时对象的创建和销毁都会带来一定的开销。特别是当对象较大或构造函数/析构函数复杂时，这种开销可能会影响程序性能。编译器可以通过一些优化技术 (返回值优化 RVO) 来减少临时对象的开销。

_**避免临时对象**_

尽量避免需要隐式类型转换的情况。例如，可以修改 countChar 函数，使其接受 const char* 类型的参数，而不是 const string&。

使用常量引用：对于不需要修改的参数，使用常量引用（const &）可以避免不必要的复制。

避免非常量引用：对于需要修改的参数，使用非常量引用（&）时，编译器不会创建临时对象，因为这会导致逻辑错误（临时对象被修改后立即销毁，实际参数不会改变）。

总结:通过合理的设计和优化，可以显著减少临时对象带来的性能开销。

<h4 id="jkju6">**Item M20：协助完成返回值优化**</h4>
代码示例 1: Rational 类定义

```plain
class Rational {
public:
    Rational(int numerator = 0, int denominator = 1);
    int numerator() const;
    int denominator() const;
private:
    int num;  // 分子
    int den;  // 分母
};
// 构造函数
Rational::Rational(int numerator, int denominator) {
    if (denominator == 0) {
        throw std::invalid_argument("Denominator cannot be zero");
    }
    num = numerator;
    den = denominator;
}
// 获取分子
int Rational::numerator() const {
    return num;
}
// 获取分母
int Rational::denominator() const {
    return den;
}
```

代码示例 2: operator* 实现

```plain
// 返回值为 const 的原因详见条款 M6
const Rational operator*(const Rational& lhs, const Rational& rhs) {
    return Rational(lhs.numerator() * rhs.numerator(), lhs.denominator() * rhs.denominator());
}
```

传值返回的开销：当一个函数返回一个对象时，通常需要调用构造函数和析构函数来创建和销毁临时对象。例如，operator* 函数返回一个 Rational 对象，这个过程涉及临时对象的创建和销毁。某些情况下，返回对象是不可避免的。例如，operator* 必须返回一个新的 Rational 对象来表示两个有理数的乘积。

_**错误的方法**_

（1）返回指针

虽然可以避免直接返回对象，但这种方法会导致资源管理问题，如内存泄漏。

```plain
const Rational * operator*(const Rational& lhs, const Rational& rhs) {
    return new Rational(lhs.numerator() * rhs.numerator(), lhs.denominator() * rhs.denominator());
}
```

使用这种方式时，调用者需要负责删除返回的指针，这容易出错。

（2）返回引用

返回引用也是错误的，因为返回的引用指向的局部对象在函数退出时会被销毁。

```plain
const Rational& operator*(const Rational& lhs, const Rational& rhs) {
    Rational result(lhs.numerator() * rhs.numerator(), lhs.denominator() * rhs.denominator());
    return result; // 错误：返回的引用指向已销毁的局部对象
}
```

直接返回对象：最简单和最安全的方法是直接返回对象。编译器可以通过返回值优化（Return Value Optimization, RVO）来减少临时对象的开销。

返回值优化（RVO）

RVO 的原理：RVO 是一种编译器优化技术，允许编译器在函数返回时直接在目标变量的位置构造对象，从而避免临时对象的创建和销毁。

```plain
Rational a = 10;
Rational b(1, 2);
Rational c = a * b; // 在这里调用 operator*
```

编译器可以在 c 的内存位置直接构造 Rational 对象，而不是先创建一个临时对象再拷贝到 c。

通过 RVO可以显著减少临时对象的开销，提高程序的性能。现代编译器通常都支持 RVO，因此在编写返回对象的函数时，可以直接返回对象而不必担心性能问题。

**<font style="color:#c21c13;">最佳实践</font>**

（1）直接返回对象：在大多数情况下，直接返回对象是最简单和最安全的方法。编译器会自动应用 RVO 来优化性能。

（2）使用 inline 关键字：如果函数体较小，可以考虑将其声明为 inline，以进一步减少函数调用的开销。

<h4 id="NhV4M">**Item M21：通过重载避免隐式类型转换 **</h4>
通过函数重载来避免隐式类型转换带来的性能开销。当一个用户自定义类型（如 UPInt）与一个基本类型（如 int）一起使用算术运算符（如 +）时，C++ 编译器会尝试通过创建临时对象来进行隐式类型转换，以便能够调用相应的运算符重载函数。虽然这使得编程更加方便，但同时也带来了不必要的性能开销，因为每次都需要创建临时对象。

为了避免这种开销，建议显式地为不同类型的参数重载运算符。例如，对于 UPInt 类，可以重载 operator+ 以接受一个 UPInt 和一个 int 作为参数，或者两个 UPInt 作为参数。这样做可以让编译器直接选择正确的重载版本，而不是创建临时对象来匹配已有的重载版本。

```plain
class UPInt { // 无限精度整数类
public:
    UPInt();
    UPInt(int value);
    // 其他成员函数...
};

// 重载运算符以支持不同类型的参数
const UPInt operator+(const UPInt& lhs, const UPInt& rhs); // 加两个 UPInt
const UPInt operator+(const UPInt& lhs, int rhs);           // 加一个 UPInt 和一个 int
const UPInt operator+(int lhs, const UPInt& rhs);           // 加一个 int 和一个 UPInt
// 示例使用
UPInt upi1, upi2;
UPInt upi3 = upi1 + upi2; // 直接调用两个 UPInt 参数的重载
upi3 = upi1 + 10;         // 直接调用一个 UPInt 和一个 int 参数的重载
upi3 = 10 + upi2;         // 直接调用一个 int 和一个 UPInt 参数的重载
```

作者强调了一个重要的点，即不应该重载只有基本类型参数的运算符，如 const UPInt operator+(int lhs, int rhs);，因为 C++ 规定每个重载的运算符至少需要一个用户定义类型的参数。这是因为如果允许对基本类型重载运算符，可能会导致预定义行为的意外改变，进而引发程序错误。

如果允许程序员重载这些基本类型的运算符，那么就有可能改变这些基本类型的默认行为。如果允许重载 int + int，那么 2 + 2 可能不再等于 4，而是其他值，这会导致程序行为不可预测。

<h4 id="pA3k8">**Item M22：考虑用运算符的赋值形式（op=）取代其单独形式（op）**</h4>
1. 定义 Rational 类

首先，定义一个简单的 Rational 类，用于表示有理数，并实现operator+=和operator-=：

```plain
class Rational {
public:
    Rational(int numerator = 0, int denominator = 1) : num(numerator), den(denominator) {
        if (den == 0) {
            throw std::invalid_argument("Denominator cannot be zero");
        }
        normalize();
    }
    Rational& operator+=(const Rational& rhs) {
        num = num * rhs.den + rhs.num * den;
        den *= rhs.den;
        normalize();
        return *this;
    }
    Rational& operator-=(const Rational& rhs) {
        num = num * rhs.den - rhs.num * den;
        den *= rhs.den;
        normalize();
        return *this;
    }
private:
    void normalize() {
        int gcd = std::gcd(num, den);
        num /= gcd;
        den /= gcd;
    }
    int num; // 分子
    int den; // 分母
};
```

2. 实现 operator+ 和 operator- 通过 operator+= 和 operator-=

接下来，通过 operator+= 和 operator-= 来实现 operator+ 和 operator-：

```plain
const Rational operator+(const Rational& lhs, const Rational& rhs) {
    return Rational(lhs) += rhs;
}
const Rational operator-(const Rational& lhs, const Rational& rhs) {
    return Rational(lhs) -= rhs;
}
```

3. 使用模板实现通用的 operator+ 和 operator-

为了进一步简化代码，可以使用模板来实现通用的 operator+ 和 operator-：

```plain
template<class T>
const T operator+(const T& lhs, const T& rhs) {
    return T(lhs) += rhs;
}
template<class T>
const T operator-(const T& lhs, const T& rhs) {
    return T(lhs) -= rhs;
}
```

效率：operator+= 和 operator-= 是赋值运算符，它们直接在左操作数上进行操作，不需要创建临时对象。这使得它们比单独的 operator+ 和 operator- 更高效。

代码复用：通过 operator+= 和 operator-= 来实现 operator+ 和 operator-，可以减少代码重复，只需要维护 operator+= 和 operator-=。

避免临时对象：operator+ 和 operator- 通过创建临时对象来调用 operator+= 和 operator-=，但这仍然是一个高效的实现方式，因为临时对象的创建和销毁开销相对较小。

临时对象：operator+ 和 operator- 会创建临时对象，这可能会带来一些开销。但是，现代编译器通常会进行返回值优化（RVO），减少临时对象的创建和销毁开销。

使用未命名的临时对象（如 return T(lhs) += rhs;）通常比命名对象（如 T result(lhs); return result += rhs;）更高效，因为未命名对象更容易被编译器优化。

通过实现 operator+= 和 operator-=，并利用这些赋值运算符来实现 operator+ 和 operator-，可以确保代码的高效性和一致性

<h4 id="sy7ma">**Item M23：考虑变更程序库**</h4>
理想的程序库应该是短小、快速、强大、灵活、可扩展、直观、普遍适用、有良好支持、没有使用约束、没有错误的。但现实中不可能同时具备所有这些特性。不同的设计者会对这些特性赋予不同的优先级，因此即使是提供相同功能的程序库，其性能特征也可能完全不同。

_**iostream 和 stdio 的比较：**_

类型安全和可扩展性：iostream 是类型安全的，支持面向对象的扩展，而 stdio 则不具备这些特性。

性能：stdio 通常在执行速度和生成的执行文件大小上优于 iostream。作者通过一个基准测试（benchmark）程序验证了这一点，结果显示 stdio 在大多数情况下更快，有时甚至快很多。

代码实现：stdio 的高效性主要来自于其代码实现，特别是在运行时解析格式字符串的方式。而 iostream 在编译时确定操作数的类型，理论上可以更高效，但实际表现取决于具体的实现。

性能优化：

一旦找到软件的瓶颈（通过性能分析工具如 profiler），可以考虑更换程序库来消除瓶颈。例如，如果程序有 I/O 瓶颈，可以考虑用 stdio 替代 iostream；如果程序在动态内存分配和释放上花费大量时间，可以考虑使用其他 operator new 和 operator delete 的实现。

<h4 id="hRCxn">**Item M24：理解虚拟函数、多继承、虚基类和 RTTI 所需的代价**</h4>
_**1. 虚拟函数（Virtual Functions）**_

虚拟函数的实现通常依赖于虚拟表（vtbl）和虚拟表指针（vptr）。每个包含虚函数的类都有一个 vtbl，其中包含指向虚函数实现的指针。每个对象都有一个 vptr，指向其类的 vtbl。

性能和内存开销：对象大小：每个包含虚函数的对象都会有一个额外的 vptr，增加了对象的大小。

类数据：每个类需要一个 vtbl，其大小与类中声明的虚函数数量成正比。

函数调用开销：调用虚函数需要通过 vptr 查找 vtbl，再通过 vtbl 查找函数指针，这比调用非虚函数稍微复杂一些，但通常不是性能瓶颈。

内联限制：虚函数不能内联，因为其调用只能在运行时确定。

_**2. 多继承（Multiple Inheritance）**_

在多继承中，对象可能有多个 vptr，每个基类对应一个 vptr。每个基类可能有自己的 vtbl，派生类还需要生成特殊的 vtbl。

对象大小：多继承增加了对象的复杂性，对象中可能有多个 vptr，增加了对象的大小。

类数据：多继承增加了 vtbl 的数量和复杂性，增加了类数据的大小。

函数调用开销：多继承使得查找 vptr 和 vtbl 更复杂，增加了函数调用的开销。

_**3. 虚基类（Virtual Base Classes）**_

虚基类用于避免多继承时基类数据成员的重复。

实现虚基类通常需要在对象中添加额外的指针，指向虚基类。

对象大小：虚基类增加了对象的大小，因为需要额外的指针。

类数据：虚基类可能需要额外的 vtbl 和 vptr，增加了类数据的大小。

函数调用开销：虚基类使得对象布局更复杂，增加了函数调用的开销。

_**4. 运行时类型识别（RTTI）**_

RTTI 通过 typeid 操作符获取对象的类型信息。

类的 vtbl 中包含一个指向 type_info 对象的指针，用于存储类型信息。

对象大小：RTTI 本身不会增加对象的大小，因为它依赖于 vtbl。

类数据：每个类需要一个 type_info 对象，增加了类数据的大小。

函数调用开销：RTTI 的开销主要在于查找 type_info 对象，通常不是性能瓶颈。

<h1 id="AYTL5">**技巧**</h1>
<h4 id="OkhFU">**Item M25：将构造函数和非成员函数虚拟化 **</h4>
虚拟构造函数并不是一个真正意义上的构造函数，而是一个设计模式。这种模式允许根据输入数据动态创建不同子类的对象。通常，这种函数是一个静态成员函数或全局函数，它能够根据输入数据确定应该创建哪个子类的对象，并返回该对象的指针或智能指针

```plain
#include <iostream>
#include <list>
#include <memory>
// 抽象基类
class NLComponent {
public:
    virtual ~NLComponent() {} // 虚析构函数
    virtual NLComponent* clone() const = 0; // 虚拟拷贝构造函数
};
// 文本块类
class TextBlock : public NLComponent {
public:
    virtual TextBlock* clone() const override {
        return new TextBlock(*this);
    }
};
// 图形类
class Graphic : public NLComponent {
public:
    virtual Graphic* clone() const override {
        return new Graphic(*this);
    }
};
// 新闻简报类
class NewsLetter {
public:
    // 构造函数，从输入流中读取数据并创建组件
    NewsLetter(std::istream& str) {
        while (str) {
            components.push_back(readComponent(str));
        }
    }
    // 拷贝构造函数
    NewsLetter(const NewsLetter& rhs) {
        for (auto it = rhs.components.begin(); it != rhs.components.end(); ++it) {
            components.push_back((*it)->clone());
        }
    }
    // 析构函数
    ~NewsLetter() {
        for (auto component : components) {
            delete component;
        }
    }
    // 静态成员函数，根据输入流创建组件
    static NLComponent* readComponent(std::istream& str) {
        std::string type;
        str >> type;
        if (type == "TextBlock") {
            return new TextBlock();
        } else if (type == "Graphic") {
            return new Graphic();
        } else {
            throw std::runtime_error("Unknown component type");
        }
    }
private:
    std::list<NLComponent*> components;
};
// 测试函数
int main() {
    // 创建一个输入流，模拟从文件中读取数据
    std::istringstream input("TextBlock Graphic");
    // 使用输入流创建一个 NewsLetter 对象
    NewsLetter nl1(input);
    // 使用拷贝构造函数创建另一个 NewsLetter 对象
    NewsLetter nl2(nl1);
    // 清理资源
    // (这里不需要手动清理，因为 NewsLetter 的析构函数会处理)
    return 0;
}
```

抽象基类 NLComponent：包含一个纯虚函数 clone，确保所有派生类都必须实现这个函数。

提供了一个虚析构函数，确保派生类的对象在通过基类指针删除时能够正确析构。

子类 TextBlock 和 Graphic继承自 NLComponent，并实现了 clone 方法，用于创建各自类的副本。

构造函数 NewsLetter(std::istream& str):从输入流中读取数据，并使用 readComponent 函数创建相应的 NLComponent 对象。

拷贝构造函数 NewsLetter(const NewsLetter& rhs):遍历 rhs 的 components 列表，调用每个组件的 clone 方法创建新的组件，并将其添加到当前对象的 components 列表中。

析构函数 ~NewsLetter():删除所有 NLComponent 对象，释放内存。

静态成员函数 readComponent：根据输入流中的数据类型创建相应的 TextBlock 或 Graphic 对象。

**虚拟化的非成员函数**

由于非成员函数不能直接被声明为虚拟函数，作者提出了一种解决方案，即通过一个公共接口（虚拟成员函数）来间接实现非成员函数的虚拟化。

定义一个虚拟成员函数（如print），它负责具体的输出逻辑。编写一个非成员函数operator<<，该函数接受一个ostream&对象和一个NLComponent对象作为参数，然后调用NLComponent对象的print方法来执行实际的输出操作。虽然operator<<本身不是虚拟的，但它可以通过调用对象的虚拟方法来实现多态性，从而根据对象的实际类型执行不同的输出逻辑。

抽象基类 NLComponent

首先定义一个抽象基类 NLComponent，它包含一个纯虚函数 print，用于实现具体的输出逻辑。

```plain
class NLComponent {
public:
    virtual ~NLComponent() {} // 虚析构函数
    virtual NLComponent* clone() const = 0; // 虚拟拷贝构造函数
    virtual std::ostream& print(std::ostream& os) const = 0; // 虚拟打印函数
};
```

子类 TextBlock 和 Graphic，接下来定义两个子类 TextBlock 和 Graphic，它们继承自 NLComponent 并实现 print 方法。

```plain
class TextBlock : public NLComponent {
public:
    virtual TextBlock* clone() const override {
        return new TextBlock(*this);
    }
    virtual std::ostream& print(std::ostream& os) const override {
        return os << "TextBlock: Some text content"; // 实际打印逻辑
    }
};
class Graphic : public NLComponent {
public:
    virtual Graphic* clone() const override {
        return new Graphic(*this);
    }
    virtual std::ostream& print(std::ostream& os) const override {
        return os << "Graphic: Some graphic content"; // 实际打印逻辑
    }
};
```

非成员函数 operator<<

编写一个非成员函数 operator<<，该函数接受一个 ostream& 对象和一个 NLComponent 对象作为参数，并调用 NLComponent 对象的 print 方法来执行实际的输出操作。

```plain
std::ostream& operator<<(std::ostream& os, const NLComponent& comp) {
    return comp.print(os);
}
```

<h4 id="mNW6J">**Item M26：限制某个类所能产生的对象数量**</h4>
在某些情况下，希望限制某个类所能产生的对象数量。可能希望系统中只有一台打印机，或者你只有16个可分发的文件描述符，因此需要确保文件描述符对象的数量不超过16个。

_<font style="color:#c21c13;">方法1：单例模式</font>_

单例模式是最常见的限制对象数量的方法之一，确保某个类只有一个实例，并提供一个全局访问点。

通过将构造函数设为私有，确保只有一个实例。使用静态成员函数 thePrinter 提供全局访问点。适用于需要严格控制对象数量的场景。

```plain
class Printer {
  public:
    static Printer& thePrinter() {
        static Printer p; // 单个打印机对象
        return p;
    }
    void submitJob(const PrintJob& job);
    void reset();
    void performSelfTest();
  private:
    Printer() = default; // 私有构造函数
    Printer(const Printer&) = delete; // 禁止拷贝
    Printer& operator=(const Printer&) = delete; // 禁止赋值
};
// 客户端代码
Printer::thePrinter().reset();
Printer::thePrinter().submitJob(buffer);
```

_<font style="color:#c21c13;">方法2：使用对象计数</font>_

通过对象计数来限制对象的数量。静态成员变量 numObjects 记录对象数量。在构造函数中检查对象数量，超过最大值时抛出异常。适用于需要限制对象数量但允许多个对象的场景。

```plain
class Printer {
public:
    class TooManyObjects {};
    static Printer* makePrinter() {
        if (numObjects >= maxObjects) {
            throw TooManyObjects();
        }
        return new Printer();
    }
    static Printer* makePrinter(const Printer& rhs) {
        if (numObjects >= maxObjects) {
            throw TooManyObjects();
        }
        return new Printer(rhs);
    }
    ~Printer() {
        --numObjects;
    }
    void submitJob(const PrintJob& job);
    void reset();
    void performSelfTest();
private:
    static size_t numObjects;
    static const size_t maxObjects = 1; // 最大允许的对象数量
    Printer() {
        ++numObjects;
    }
    Printer(const Printer&) = default; // 允许拷贝
    Printer& operator=(const Printer&) = default; // 允许赋值
};
// 必须在类外定义静态成员
size_t Printer::numObjects = 0;
const size_t Printer::maxObjects;
// 客户端代码
Printer* p1 = Printer::makePrinter();
p1->submitJob(buffer);
delete p1;
try {
    Printer* p2 = Printer::makePrinter(); // 抛出 TooManyObjects 异常
} catch (const Printer::TooManyObjects&) {
    std::cerr << "Too many Printer objects!" << std::endl;
}
```

方法3：计数模板类

可以创建一个具有对象计数功能的基类，可以被其他类继承，从而自动管理对象的数量。

计数类模板 Counted

```plain
template<class BeingCounted>
class Counted {
public:
    class TooManyObjects : public std::exception {
    public:
        const char* what() const noexcept override {
            return "Too many objects!";
        }
    };
    static int objectCount() { return numObjects; }
protected:
    Counted() { init(); }
    Counted(const Counted&) { init(); }
    ~Counted() { --numObjects; }
private:
    static int numObjects;
    static const size_t maxObjects;
    void init() {
        if (numObjects >= maxObjects) {
            throw TooManyObjects();
        }
        ++numObjects;
    }
};
// 必须在类外定义静态成员
template<class BeingCounted>
int Counted<BeingCounted>::numObjects = 0;
template<class BeingCounted>
const size_t Counted<BeingCounted>::maxObjects = 1; // 默认最大值为1
```

使用 Counted 的 Printer 类

```plain
class PrintJob {
public:
    PrintJob(const std::string& whatToPrint) : content(whatToPrint) {}
    std::string getContent() const { return content; }
private:
    std::string content;
};
class Printer : private Counted<Printer> {
public:
    class TooManyObjects : public Counted<Printer>::TooManyObjects {};
    static Printer* makePrinter() {
        try {
            return new Printer();
        } catch (const TooManyObjects&) {
            std::cerr << "Too many Printer objects!" << std::endl;
            return nullptr;
        }
    }
static Printer* makePrinter(const Printer& rhs) {
        try {
            return new Printer(rhs);
        } catch (const TooManyObjects&) {
            std::cerr << "Too many Printer objects!" << std::endl;
            return nullptr;
        }
    }
    ~Printer() = default;
    void submitJob(const PrintJob& job) {
        std::cout << "Submitting job: " << job.getContent() << std::endl;
    }
   void reset() {
        std::cout << "Resetting printer." << std::endl;
   }
   void performSelfTest() {
        std::cout << "Performing self test." << std::endl;
   }
   static int objectCount() { return Counted<Printer>::objectCount(); }
private:
   Printer() = default;
   Printer(const Printer&) = default;
   Printer& operator=(const Printer&) = default;
};
// 必须在类外定义静态成员
const size_t Counted<Printer>::maxObjects = 1; // 最大允许的对象数量
```

使用 Counted 的 Printer 类：

私有继承 Counted<Printer>：确保 Counted 的实现细节对 Printer 的用户是隐藏的。

伪构造函数 makePrinter：提供创建 Printer 对象的接口，同时处理对象数量的限制。

静态成员函数 objectCount：提供获取当前对象数量的接口。

构造函数和析构函数：默认实现，不需要额外的逻辑。成员函数 submitJob, reset, performSelfTest：具体的业务逻辑。私有继承：使用 private 继承 Counted<Printer>，确保 Counted 的实现细节对 Printer 的用户是隐藏的。

Counted 模板封装了对象计数的逻辑，使得其他类可以通过继承 Counted 来自动管理对象的数量。这种方式不仅减少了代码重复，还提高了代码的可维护性和可扩展性。

<h4 id="gin4l">**Item M27：要求或禁止在堆中产生对象**</h4>
在某些情况下，可能要求或禁止在堆中创建对象。

例如，某些对象需要能够自我销毁（即调用 `delete this`），这就要求这些对象必须在堆中创建。

在嵌入式系统中，为了避免内存泄漏，可能需要禁止在堆中创建对象。

**要求在堆中建立对象**

_方法1：禁止非堆对象的构造和析构_

为了确保某个类的对象只能在堆中创建：

**（1）将析构函数声明为 **`**private**`**：**这样可以防止非堆对象的自动析构。

**（2）**提供伪析构函数：客户端通过伪析构函数来释放对象。

```plain
class UPNumber {
public:
    UPNumber();
    UPNumber(int initValue);
    UPNumber(double initValue);
    UPNumber(const UPNumber& rhs);
    // 伪析构函数
    void destroy() const { delete this; }
private:
    ~UPNumber();
};
// 客户端代码
UPNumber n; // 错误! (在这里合法，但是当它的析构函数被隐式地调用时，就不合法了)
UPNumber *p = new UPNumber; // 正确
...
delete p; // 错误! 试图调用 private 析构函数
p->destroy(); // 正确
```

_<font style="color:#c21c13;">方法2：将所有构造函数声明为 </font>_`_<font style="color:#c21c13;">private</font>_`

所有构造函数声明为 `private`,并通过静态成员函数(伪构造函数)来创建对象。

```plain
class UPNumber {
public:
    // 伪构造函数
    static UPNumber* create() {
        return new UPNumber();
    }
    // 伪析构函数
    void destroy() const { delete this; }
private:
    UPNumber() {}
    ~UPNumber() {}
    UPNumber(const UPNumber&) = delete;
    UPNumber& operator=(const UPNumber&) = delete;
};
// 客户端代码
UPNumber *p = UPNumber::create(); // 正确
...
p->destroy(); // 正确
```

**判断一个对象是否在堆中**

**方法1：使用**`**operator new**`**和标志位**

通过重载`operator new`和设置一个标志位来判断对象是否在堆中创建。

```plain
class UPNumber {
public:
    class HeapConstraintViolation {};
    static void* operator new(size_t size) {
        onTheHeap = true;
        return ::operator new(size);
    }
    UPNumber() {
        if (!onTheHeap) {
            throw HeapConstraintViolation();
        }
        // 继续正常的构造函数
        onTheHeap = false;
    }
private:
    static bool onTheHeap;
};
// 必须在类外定义静态成员
bool UPNumber::onTheHeap = false;
```

构造函数中，首先检查 onTheHeap 标志位。如果 onTheHeap 为 false，说明对象不是在堆中创建的，此时抛出 HeapConstraintViolation 异常。继续正常的构造函数，如果 onTheHeap 为 true，则继续执行构造函数的其他部分。重置 onTheHeap 标志位，在构造函数的末尾，将 onTheHeap 标志位重置为 false。

**方法2：使用不可移植的方法**

如果需要判断对象是否在堆中，可以使用不可移植的方法，例如利用栈和堆的地址分布特点。

```plain
bool onHeap(const void* address) {
    char onTheStack; // 局部栈变量
    return address < &onTheStack;
}
```

**使用抽象混合基类**

为了提供一个可移植且高效的方法来判断对象是否在堆中，可以使用抽象混合基类。

```plain
#include <list>
#include <stdexcept>
class HeapTracked {
public:
    class MissingAddress : public std::runtime_error {
    public:
        MissingAddress() : std::runtime_error("Address not found in heap") {}
    };

    virtual ~HeapTracked() = 0;
    static void* operator new(size_t size) {
        void* p = ::operator new(size);
        addresses.push_back(p);
        return p;
    }

    static void operator delete(void* ptr) {
        auto it = std::find(addresses.begin(), addresses.end(), ptr);
        if (it != addresses.end()) {
            addresses.erase(it);
        }
        ::operator delete(ptr);
    }

    bool isOnHeap() const {
        return std::find(addresses.begin(), addresses.end(), this) != addresses.end();
    }

private:
    typedef const void* RawAddress;
    static std::list<RawAddress> addresses;
};

std::list<HeapTracked::RawAddress> HeapTracked::addresses;
// 派生类示例
class MyObject : public HeapTracked {
public:
    MyObject() {}
    ~MyObject() {}
};

// 客户端代码
MyObject* obj1 = new MyObject;
std::cout << "obj1 on heap: " << obj1->isOnHeap() << std::endl; // 输出: true

MyObject obj2;
std::cout << "obj2 on heap: " << obj2.isOnHeap() << std::endl; // 输出: false
```

1. 要求在堆中建立对象：
    - 禁止非堆对象的构造和析构：通过将析构函数声明为 `private`，并提供伪析构函数来释放对象。
    - 将所有构造函数声明为 `private`：通过静态成员函数（伪构造函数）来创建对象。
2. 判断一个对象是否在堆中：

使用 `operator new` 和标志位：通过重载 `operator new` 和设置一个标志位来判断对象是否在堆中创建。

使用不可移植的方法：利用栈和堆的地址分布特点来判断对象是否在堆中，但这不是可移植的方法。

1. 使用抽象混合基类：

通过抽象混合基类来跟踪从 `operator new` 返回的地址，提供 `isOnHeap` 方法来判断对象是否在堆中。

使用抽象混合基类是一种较为可移植且高效的方法，适用于大多数场景。这些技术不仅有助于管理对象的生命周期，还能避免内存泄漏等问题。

**禁止直接实例化对象**

为了确保对象不能通过 new 关键字在堆上创建，可以将类的 operator new 和 operator delete 方法声明为私有的。

```plain
class UPNumber {
private:
    static void* operator new(size_t size);
    static void operator delete(void* ptr);
public: //其他成员和方法
};
```

尝试在堆上创建 UPNumber 对象将会导致编译错误，因为 operator new 是私有的，不可从外部访问。

```plain
UPNumber n1; // 正确，对象在栈上创建
static UPNumber n2; // 也正确，静态对象在全局或静态存储区创建
UPNumber* p = new UPNumber; // 编译错误！尝试调用私有的 operator new
```

处理派生类的情况

如果一个类是从 UPNumber 派生的，并且没有重新声明operator new和operator delete为public，那么尝试在堆上创建该派生类的对象也会失败，因为派生类会继承基类的私有 operator new:

```plain
class NonNegativeUPNumber : public UPNumber {
   //没有声明 operator new
};
NonNegativeUPNumber n1; // 正确
static NonNegativeUPNumber n2; // 也正确
NonNegativeUPNumber* p = new NonNegativeUPNumber; 
// 编译错误！尝试调用私有的 operator new
```

如果希望允许在堆上创建派生类的对象，可以在派生类中重新声明 operator new 和 operator delete 为公共的。

_<font style="color:#c21c13;">包含对象的情况</font>_

如果一个类包含了一个 UPNumber 类型的成员变量，那么这个类的对象仍然可以在堆上创建，因为创建这个类的对象时，调用的是这个类的 operator new，而不是 UPNumber 的 operator new。

```plain
class Asset {
public:
    Asset(int initValue);
private:
    UPNumber value;
};
Asset* pa = new Asset(100); // 正确，调用 Asset::operator new 或 ::operator new
```

<h4 id="YpPI4">Item M28：灵巧指针</h4>
同智能指针。

<h4 id="E885G">Item M29 ：引用计数</h4>
引用计数用于追踪有多少个对象引用了同一个资源。当引用计数降为零时，表示没有对象再使用这个资源，可以安全地释放它。

C++可以通过创建一个包含引用计数和指向实际资源指针的结构体或类来实现。每当有新的对象引用这个资源时，引用计数就增加；当对象不再使用资源时，引用计数减少。如果引用计数变为0，则自动删除资源。

（1）线程安全问题：如果多个线程同时访问和修改引用计数，可能会导致数据竞争，因此需要使用互斥锁或其他同步机制来保证线程安全。

（2）循环引用问题：当两个或更多对象互相持有对方的引用时，可能会形成循环引用，导致引用计数永远不会降到0，从而造成内存泄漏。解决方法包括使用弱引用来打破循环。

创建一个基本的引用计数智能指针：

```plain
class RefCounted {
public:
    RefCounted() : count(1) {}
    void addRef() { ++count; }
    void release() {
        if (--count == 0) {
            delete this;
        }
    }
private:
    int count;
};
template <typename T>
class SmartPtr {
public:
    explicit SmartPtr(T* ptr = nullptr) : ptr(ptr) {
        if (ptr != nullptr) {
            ref = new RefCounted();
        } else {
            ref = nullptr;
        }
    }
   ~SmartPtr() {
        if (ref != nullptr) {
            ref->release();
        }
    }
    SmartPtr(const SmartPtr& other) : ptr(other.ptr), ref(other.ref) {
        if (ref != nullptr) {
            ref->addRef();
        }
    }
    SmartPtr& operator=(const SmartPtr& other) {
        if (this != &other) {
            if (ref != nullptr)ref->release();
            ptr = other.ptr;
            ref = other.ref;
            if (ref != nullptr) ref->addRef();
        return *this;
    }
    T& operator*() const {
        return *ptr;
    }
    T* operator->() const {
        return ptr;
    }
private:
    T* ptr;
    RefCounted* ref;
};
int main() {
    SmartPtr<int> p1(new int(10));
    SmartPtr<int> p2 = p1; // p1和p2共享相同的int对象
    std::cout << *p1 << std::endl; // 输出10
    std::cout << *p2 << std::endl; // 输出10
    return 0;
}
```

SmartPtr通过复制构造函数和赋值操作符实现了引用计数的增加与减少。当最后一个SmartPtr对象销毁时，RefCounted对象的release成员函数会检测到引用计数降为0，并删除原始资源。

上述代码没有考虑多线程环境下的安全性问题，也没有处理循环引用的情况。

<h4 id="V7zyQ">Item M30：代理类</h4>
C++中通过代理类实现类似二维数组的多维数组支持，并利用代理类区分数组索引操作是读操作还是写操作。C++ 中不支持用动态变量来定义多维数组的大小。编译时数组的维度必须是常量。因此，尝试使用动态变量定义数组，如 `int data[dim1][dim2]` 是非法的。

为了解决这个问题，可以通过定义一个类模板`Array2D`来模拟二维数组的行为。

```plain
template <classT>
classArray2D {
public:
    Array2D(int dim1, int dim2);
    //其它成员
};
```

可以创建二维数组

```plain
Array2D<int> data(10, 20);  //合法
```

试图重载`operator[][]` 来模拟二维数组的索引行为是不可行的，因为 C++ 不支持这种重载。可以通过重载 `operator[]`，使得返回一个代理类 `Array1D` 来模拟二维数组的索引。在 `Array2D` 类中重载 `operator[]`，使其返回 `Array1D` 类的对象：

```plain
template<classT>
classArray2D {
public:
    classArray1D {
    public:
        T& operator[](int index);
        const T& operator[](int index) const;
    };
    Array1D operator[](int index);
};
```

这样使用时，可以像访问真实的二维数组一样：

```plain
Array2D<int> data(10, 20);
cout << data[3][6];  // 合法
```

通过代理类，可以实现区分 `operator[]` 是进行读取操作还是写入操作。通常，读操作会返回一个临时对象（如代理对象），而写操作则会返回一个引用，允许赋值操作。通过这种方式，可以为引用计数的数据结构（如 `String`）优化内存管理，避免在读操作时进行不必要的拷贝。在一个带引用计数的字符串类 `String` 中，可以使用代理类 `CharProxy` 来区分读取和写入操作。

```plain
class String {
public:
    class CharProxy {
    public:
        CharProxy(String& str, int index);
        CharProxy& operator=(const CharProxy& rhs);
        CharProxy& operator=(char c);
        operator char() const;
    private:
        String& theString;
        int charIndex;
    };
    const CharProxy operator[](int index) const;
    CharProxy operator[](int index);
    friend class CharProxy;
private:
    struct StringValue {
        std::string data;
        int refCount;
        bool isShared() const { return refCount > 1; }
    };
    std::shared_ptr<StringValue> value;
};
// 构造函数
String::CharProxy::CharProxy(String& str, int index)
    : theString(str), charIndex(index) {}
// 隐式转换为 char
String::CharProxy::operator char() const {
    return theString.value->data[charIndex];
}
// 赋值操作符
String::CharProxy& String::CharProxy::operator=(const CharProxy& rhs) {
    if (theString.value->isShared()) {
        theString.value = std::make_shared<StringValue>(*theString.value);
    }
    theString.value->data[charIndex] = rhs.theString.value->data[rhs.charIndex];
    return *this;
}
// 赋值操作符
String::CharProxy& String::CharProxy::operator=(char c) {
    if (theString.value->isShared()) {
        theString.value = std::make_shared<StringValue>(*theString.value);
    }
    theString.value->data[charIndex] = c;
    return *this;
}
// 常量版本的 operator[]
const String::CharProxy String::operator[](int index) const {
    return CharProxy(const_cast<String&>(*this), index);
}
// 非常量版本的 operator[]
String::CharProxy String::operator[](int index) {
    return CharProxy(*this, index);
}
```

String 类

CharProxy 类:一个代理类,用于区分 operator[] 的读写操作。

构造函数:初始化代理对象，记录所属的 String 对象和字符索引。

operator char():将代理对象隐式转换为 char，用于读操作。

operator=:重载赋值操作符，用于写操作。当字符串被共享时，会创建一个新的副本以避免影响其他共享对象。

operator[] 方法:

常量版本：返回一个常量的 CharProxy 对象，确保不能对其进行写操作。

非常量版本：返回一个非常量的 CharProxy 对象，可以对其进行读写操作。

3. 工作原理

读操作：当 operator[] 返回的 CharProxy 对象被用作右值时，会调用 operator char() 方法，返回字符值。

写操作：当operator[] 返回的 CharProxy 对象被用作左值时，会调用 operator= 方法，执行写操作。如果字符串被共享，则会创建一个新的副本以避免影响其他共享对象。

延迟决策：通过代理类，可以在实际使用时才决定是读操作还是写操作，从而优化性能。

引用计数：在写操作时，只有当字符串被共享时才会创建新的副本，避免不必要的复制开销。

类型转换：在常量版本的 operator[] 中使用 const_cast 是安全的，因为返回的 CharProxy 对象本身是常量，不能对其进行写操作。

代码复用：可以将两个 operator= 方法中的公共部分提取到一个私有成员函数中，以减少代码重复。

<h4 id="JuXZB">Item M31：让函数根据多个对象来决定虚拟</h4>
在一个模拟太空环境中，不同类型的对象（如宇宙飞船、太空站、小行星）之间发生碰撞时应如何处理。由于 C++ 的虚函数机制仅支持单个对象的动态绑定，即只能根据一个对象的类型来决定调用哪个版本的方法，因此当需要根据两个对象的类型来决定行为时，就遇到了挑战。

**使用虚函数加 RTTI（运行时类型信息）**

在 GameObject 基类中声明一个纯虚函数 collide，该函数在派生类中被重写。

当两个对象发生碰撞时，首先会调用一个对象的 collide 方法，传递另一个对象作为参数。

在 collide 方法内，使用 typeid 操作符来获取传入对象的实际类型。

根据传入对象的类型，使用一系列的 if...else 语句来决定具体的碰撞处理逻辑。

如果遇到未预期的对象类型，则抛出异常 CollisionWithUnknownObject。

作者的观点

（1）封装性受损

每个 collide 函数都需要了解所有其他派生类的存在，这意味着每当添加一个新的派生类时，就需要更新所有已有的 collide 实现以包含新的类型检查。这种做法破坏了类之间的封装性。

（2）可维护性差

随着系统的扩展，这种类型检查的链条会变得越来越长，难以维护。如果某个地方忘记更新，就会导致潜在的错误。

（3）编译器无法帮助

由于编译器无法理解这些类型检查的具体含义，因此无法在编译阶段捕获到因忘记更新类型检查而导致的错误。

（4）退回到过去

这种方法类似于早期 C 语言中的类型相关编程，这些程序本质上缺乏可维护性，正是这种不足促使了虚函数等面向对象特性的出现。

（5）异常处理

虽然异常处理可以作为一种安全网来捕捉未预期的情况，但它并不能真正解决问题，反而可能掩盖问题的本质，使调试变得更加困难。

继承体系如下：

```plain
class GameObject { ... }; 
class SpaceShip: public GameObject { ... }; 
class SpaceStation: public GameObject { ... }; 
class Asteroid: public GameObject { ... };
```

基类 GameObject

```plain
#include <typeinfo>
#include <stdexcept>
class GameObject {
public:
    virtual ~GameObject() = default;
    virtual void collide(GameObject& otherObject) = 0;
};
class CollisionWithUnknownObject : public std::runtime_error {
public:
    CollisionWithUnknownObject(const GameObject& whatWeHit)
        : std::runtime_error("Collision with unknown object: " + std::string(typeid(whatWeHit).name())) {}
};
```

派生类 SpaceShip

```plain
class SpaceShip : public GameObject {
public:
    void collide(GameObject& otherObject) override {
        const std::type_info& objectType = typeid(otherObject);
        if (objectType == typeid(SpaceShip)) {
            SpaceShip& ss = static_cast<SpaceShip&>(otherObject);
            processCollisionWithSpaceShip(ss);
        } else if (objectType == typeid(SpaceStation)) {
            SpaceStation& ss = static_cast<SpaceStation&>(otherObject);
            processCollisionWithSpaceStation(ss);
        } else if (objectType == typeid(Asteroid)) {
            Asteroid& a = static_cast<Asteroid&>(otherObject);
            processCollisionWithAsteroid(a);
        } else {
            throw CollisionWithUnknownObject(*this);
        }
    }
private:
    void processCollisionWithSpaceShip(SpaceShip& otherShip) {
        // 处理飞船与飞船的碰撞
        // ...
    }
    void processCollisionWithSpaceStation(SpaceStation& station) {
        // 处理飞船与太空站的碰撞
        // ...
    }
    void processCollisionWithAsteroid(Asteroid& asteroid) {
        // 处理飞船与小行星的碰撞
        // ...
    }
};
```

派生类 SpaceStation

```plain
class SpaceStation:public GameObject {
public:
    void collide(GameObject& otherObject) override {
        const std::type_info& objectType = typeid(otherObject);
        if (objectType == typeid(SpaceShip)) {
            SpaceShip& ss = static_cast<SpaceShip&>(otherObject);
            processCollisionWithSpaceShip(ss);
        } else if (objectType == typeid(SpaceStation)) {
            SpaceStation& ss = static_cast<SpaceStation&>(otherObject);
            processCollisionWithSpaceStation(ss);
        } else if (objectType == typeid(Asteroid)) {
            Asteroid& a = static_cast<Asteroid&>(otherObject);
            processCollisionWithAsteroid(a);
        } else {
            throw CollisionWithUnknownObject(*this);
        }
    }
private:
    void processCollisionWithSpaceShip(SpaceShip& ship) {
        // 处理太空站与飞船的碰撞
        // ...
    }
    void processCollisionWithSpaceStation(SpaceStation& otherStation) {
        // 处理太空站与太空站的碰撞
        // ...
    }
    void processCollisionWithAsteroid(Asteroid& asteroid) {
        // 处理太空站与小行星的碰撞
        // ...
    }
};
```

派生类 Asteroid

```plain
class Asteroid : public GameObject {
public:
    void collide(GameObject& otherObject) override {
        const std::type_info& objectType = typeid(otherObject);
        if (objectType == typeid(SpaceShip)) {
            SpaceShip& ss = static_cast<SpaceShip&>(otherObject);
            processCollisionWithSpaceShip(ss);
        } else if (objectType == typeid(SpaceStation)) {
            SpaceStation& ss = static_cast<SpaceStation&>(otherObject);
            processCollisionWithSpaceStation(ss);
        } else if (objectType == typeid(Asteroid)) {
            Asteroid& a = static_cast<Asteroid&>(otherObject);
            processCollisionWithAsteroid(a);
        } else {
            throw CollisionWithUnknownObject(*this);
        }
    }
private:
    void processCollisionWithSpaceShip(SpaceShip& ship) {
        // 处理小行星与飞船的碰撞
    }
    void processCollisionWithSpaceStation(SpaceStation& station) {
        // 处理小行星与太空站的碰撞
    }
    void processCollisionWithAsteroid(Asteroid& otherAsteroid) {
        // 处理小行星与小行星的碰撞
    }
};
```

检测碰撞的函数

```plain
void checkForCollision(GameObject& object1, GameObject& object2) {
    if (theyJustCollided(object1, object2)) {
        object1.collide(object2);
    } else {
        // 处理没有碰撞的情况
        // ...
    }
}
bool theyJustCollided(GameObject& object1, GameObject& object2) {
    // 检测两个对象是否刚刚发生碰撞
    // ...
    return true; // 示例中返回 true
}
```

基类 GameObject：定义了一个纯虚函数 collide，所有派生类都必须实现这个函数。

派生类 SpaceShip, SpaceStation, Asteroid：每个派生类都实现了 collide 函数。在 collide 函数中，使用 typeid 获取传入对象的类型，并使用 if...else 语句来决定具体的碰撞处理逻辑。

异常处理：如果遇到未知类型，抛出 CollisionWithUnknownObject 异常。

检测碰撞的函数 checkForCollision：检查两个对象是否发生碰撞，如果发生碰撞则调用 collide 函数。

**只使用虚函数 **

基类 GameObject

```plain
class SpaceShip; // 前向声明
class SpaceStation; // 前向声明
class Asteroid; // 前向声明
class GameObject {
public:
    virtual ~GameObject() = default;
    virtual void collide(GameObject& otherObject) = 0;
    virtual void collide(SpaceShip& otherObject) = 0;
    virtual void collide(SpaceStation& otherObject) = 0;
    virtual void collide(Asteroid& otherObject) = 0;
};
```

派生类 SpaceShip

```plain
class SpaceShip : public GameObject {
public:
    void collide(GameObject& otherObject) override {
        otherObject.collide(*this);
    }
    void collide(SpaceShip& otherObject) override {
        // 处理飞船与飞船的碰撞
        // ...
    }
    void collide(SpaceStation& otherObject) override {
        // 处理飞船与太空站的碰撞
        // ...
    }
    void collide(Asteroid& otherObject) override {
        // 处理飞船与小行星的碰撞
        // ...
    }
};
```

派生类 SpaceStation

```plain
class SpaceStation : public GameObject {
public:
    void collide(GameObject& otherObject) override {
        otherObject.collide(*this);
    }
    void collide(SpaceShip& otherObject) override {
        // 处理太空站与飞船的碰撞
        // ...
    }
    void collide(SpaceStation& otherObject) override {
        // 处理太空站与太空站的碰撞
        // ...
    }
    void collide(Asteroid& otherObject) override {
        // 处理太空站与小行星的碰撞
        // ...
    }
};
```

派生类 Asteroid

```plain
class Asteroid : public GameObject {
public:
    void collide(GameObject& otherObject) override {
        otherObject.collide(*this);
    }
    void collide(SpaceShip& otherObject) override {
        // 处理小行星与飞船的碰撞
        // ...
    }
    void collide(SpaceStation& otherObject) override {
        // 处理小行星与太空站的碰撞
        // ...
    }
    void collide(Asteroid& otherObject) override {
        // 处理小行星与小行星的碰撞
        // ...
    }
};
```

检测碰撞的函数

```plain
void checkForCollision(GameObject& object1, GameObject& object2) {
    if (theyJustCollided(object1, object2)) {
        object1.collide(object2);
    } else {
        // 处理没有碰撞的情况
        // ...
    }
}
bool theyJustCollided(GameObject& object1, GameObject& object2) {
    // 检测两个对象是否刚刚发生碰撞
    // ...
    return true; // 示例中返回 true
}
```

基类 GameObject声明了四个纯虚函数 collide，分别用于处理不同类型对象的碰撞。

这些函数在派生类中被重写。派生类 SpaceShip, SpaceStation, Asteroid：每个派生类都实现了 GameObject 中声明的四个 collide 函数。collide(GameObject& otherObject) 函数用于第一次调度，它调用 otherObject.collide(*this)，从而触发第二次调度。其他三个 collide 函数分别处理特定类型对象的碰撞。

双调度机制：第一次调度：object1.collide(object2) 调用 collide(GameObject& otherObject)。第二次调度：在 collide(GameObject& otherObject) 中，调用 otherObject.collide(*this)，这里 *this 的静态类型已知，因此会调用相应的 collide 函数。通过两次调度，确定了两个对象的真实类型，从而可以调用正确的碰撞处理函数。

**优点**

无需 RTTI：避免了使用 typeid 和 dynamic_cast，减少了运行时开销。

类型安全：编译器会在编译时检查类型，避免了运行时类型错误。

**缺点**

代码冗余：每个派生类都需要实现多个 collide 函数，增加了代码量。

扩展困难：每增加一个新类，所有现有类都需要添加新的 collide 函数，这在大型项目中可能导致维护困难。

依赖性：如果 GameObject 或其他基类是由第三方库提供的，你可能无法修改这些类，从而无法添加新的 collide 函数。

**<font style="color:#c21c13;">模拟虚函数表 </font>**

基类 GameObject

```plain
#include <map>
#include <string>
#include <typeinfo>
#include <stdexcept>
class SpaceShip; // 前向声明
class SpaceStation; // 前向声明
class Asteroid; // 前向声明
class GameObject {
public:
    virtual ~GameObject() = default;
    virtual void collide(GameObject& otherObject) = 0;
};
```

派生类 SpaceShip

```plain
class SpaceShip : public GameObject {
public:
    virtual void collide(GameObject& otherObject) override {
       HitFunctionPtr hfp = lookup(otherObject);
       if(hfp){
           (this->*hfp)(otherObject);
        }else{
           throw CollisionWithUnknownObject(otherObject);
        }
    }
    virtual void hitSpaceShip(SpaceShip& otherObject) {
        //处理飞船与飞船的碰撞
    }
    virtual void hitSpaceStation(SpaceStation& otherObject) {
        // 处理飞船与太空站的碰撞
    }
    virtual void hitAsteroid(Asteroid& otherObject) {
        // 处理飞船与小行星的碰撞
    }
private:
    typedef void (SpaceShip::*HitFunctionPtr)(GameObject&);
    typedef std::map<std::string, HitFunctionPtr> HitMap;
    static HitFunctionPtr lookup(const GameObject& whatWeHit) {
    static HitMap collisionMap = initializeCollisionMap();
     HitMap::iterator mapEntry = collisionMap.find(typeid(whatWeHit).name());
    if (mapEntry == collisionMap.end()) return nullptr;
    return (*mapEntry).second;
}
static HitMap initializeCollisionMap() {
        HitMap map;
        map[typeid(SpaceShip).name()] = &SpaceShip::hitSpaceShip;
        map[typeid(SpaceStation).name()] = &SpaceShip::hitSpaceStation;
        map[typeid(Asteroid).name()] = &SpaceShip::hitAsteroid;
        return map;
    }
};
```

派生类 SpaceStation

```plain
class SpaceStation : public GameObject {
public:
    virtual void collide(GameObject& otherObject) override {
        HitFunctionPtr hfp = lookup(otherObject);
        if (hfp) {
            (this->*hfp)(otherObject);
        } else {
            throw CollisionWithUnknownObject(otherObject);
        }
    }
    virtual void hitSpaceShip(SpaceShip& otherObject) {
        // 处理太空站与飞船的碰撞
        // ...
    }
    virtual void hitSpaceStation(SpaceStation& otherObject) {
        // 处理太空站与太空站的碰撞
        // ...
    }
    virtual void hitAsteroid(Asteroid& otherObject) {
        // 处理太空站与小行星的碰撞
        // ...
    }
private:
    typedef void (SpaceStation::*HitFunctionPtr)(GameObject&);
    typedef std::map<std::string, HitFunctionPtr> HitMap;

    static HitFunctionPtr lookup(const GameObject& whatWeHit) {
        static HitMap collisionMap = initializeCollisionMap();
        HitMap::iterator mapEntry = collisionMap.find(typeid(whatWeHit).name());
        if (mapEntry == collisionMap.end()) return nullptr;
        return (*mapEntry).second;
    }
    static HitMap initializeCollisionMap() {
        HitMap map;
        map[typeid(SpaceShip).name()] = &SpaceStation::hitSpaceShip;
        map[typeid(SpaceStation).name()] = &SpaceStation::hitSpaceStation;
        map[typeid(Asteroid).name()] = &SpaceStation::hitAsteroid;
        return map;
    }
};
```

派生类 Asteroid

```plain
class Asteroid : public GameObject {
public:
    virtual void collide(GameObject& otherObject) override {
        HitFunctionPtr hfp = lookup(otherObject);
        if (hfp) {
            (this->*hfp)(otherObject);
        } else {
            throw CollisionWithUnknownObject(otherObject);
        }
    }
    virtual void hitSpaceShip(SpaceShip& otherObject) {
        // 处理小行星与飞船的碰撞
    }
    virtual void hitSpaceStation(SpaceStation& otherObject) {
        // 处理小行星与太空站的碰撞
    }
    virtual void hitAsteroid(Asteroid& otherObject) {
        // 处理小行星与小行星的碰撞
    }
private:
    typedef void (Asteroid::*HitFunctionPtr)(GameObject&);
    typedef std::map<std::string, HitFunctionPtr> HitMap;
    static HitFunctionPtr lookup(const GameObject& whatWeHit) {
        static HitMap collisionMap = initializeCollisionMap();
        HitMap::iterator mapEntry = collisionMap.find(typeid(whatWeHit).name());
        if (mapEntry == collisionMap.end()) return nullptr;
        return (*mapEntry).second;
    }
    static HitMap initializeCollisionMap() {
        HitMap map;
        map[typeid(SpaceShip).name()] = &Asteroid::hitSpaceShip;
        map[typeid(SpaceStation).name()] = &Asteroid::hitSpaceStation;
        map[typeid(Asteroid).name()] = &Asteroid::hitAsteroid;
        return map;
    }
};
```

检测碰撞的函数

```plain
void checkForCollision(GameObject& object1, GameObject& object2) {
    if (theyJustCollided(object1, object2)) {
        object1.collide(object2);
    } else {
        // 处理没有碰撞的情况
    }
}
bool theyJustCollided(GameObject& object1, GameObject& object2) {
    // 检测两个对象是否刚刚发生碰撞
    // ...
    return true; // 示例中返回 true
}
```

_异常类CollisionWithUnknownObject_

```plain
class CollisionWithUnknownObject : public std::runtime_error {
public:
    CollisionWithUnknownObject(const GameObject& whatWeHit):std::runtime_error("Collision with unknown object: " + std::string(typeid(whatWeHit).name()))
{}
};
```

基类 GameObject：声明了一个纯虚函数 collide，所有派生类都必须实现这个函数。

派生类 SpaceShip, SpaceStation, Asteroid：每个派生类都实现了 collide 函数。collide 函数通过 lookup 函数查找并调用相应的碰撞处理函数。每个派生类都有多个 hit 函数，分别处理不同类型的碰撞。lookup 函数使用 std::map 来映射对象类型到相应的成员函数指针。initializeCollisionMap 函数初始化 collisionMap，将对象类型映射到相应的成员函数指针。

双调度机制：

第一次调度：object1.collide(object2) 调用 collide(GameObject& otherObject)。

第二次调度：在 collide(GameObject& otherObject) 中，使用 lookup 函数查找并调用相应的 hit 函数。

通过两次调度，确定了两个对象的真实类型，从而可以调用正确的碰撞处理函数。

优点

（1）高效：使用 std::map 和成员函数指针，避免了 if...then...else 链，提高了效率。

（2）集中管理：RTTI 的使用范围限定在 lookup 函数中，减少了代码的复杂性。

缺点

（1）代码复杂性：增加了 lookup 和 initializeCollisionMap 函数，代码量增加。

（2）扩展困难：每增加一个新类，所有现有类都需要更新 collisionMap。

（3）依赖性：如果 GameObject 或其他基类是由第三方库提供的，你可能无法修改这些类，从而无法添加新的 hit 函数。

基类 GameObject

通过使用非成员的碰撞处理函数和 CollisionMap 类，我们能够有效地管理碰撞处理函数，避免了在增加新类时需要重新编译的问题。这种方式不仅提高了代码的灵活性和扩展性，还减少了代码的耦合度。尽管引入了一些额外的复杂性和性能开销，但这些改进使得系统更加健壮和易于维护。

```plain
#include <map>
#include <string>
#include <memory>
#include <stdexcept>
#include <typeinfo>
class GameObject {
public:
    virtual ~GameObject() = default;
    virtual void collide(GameObject& otherObject) = 0;
};
class UnknownCollision : public std::runtime_error {
public:
    UnknownCollision(const GameObject& obj1, const GameObject& obj2)
        : std::runtime_error("Unknown collision between " + std::string(typeid(obj1).name()) + " and " + std::string(typeid(obj2).name())) {}
};
void processCollision(GameObject& object1, GameObject& object2) {
    using namespace collision;
    HitFunctionPtr phf = lookup(typeid(object1).name(), typeid(object2).name());
    if (phf) {
        phf(object1, object2);
    } else {
        throw UnknownCollision(object1, object2);
    }
}
```

碰撞处理函数

```plain
namespace collision {
    // 主要的碰撞处理函数
    void shipAsteroid(GameObject& spaceShip, GameObject& asteroid) {
        // 处理飞船与小行星的碰撞
        // ...
    }
    void shipStation(GameObject& spaceShip, GameObject& spaceStation) {
        // 处理飞船与太空站的碰撞
        // ...
    }
    void asteroidStation(GameObject& asteroid, GameObject& spaceStation) {
        // 处理小行星与太空站的碰撞
        // ...
    }
    // 辅助的碰撞处理函数，实现对称性
    void asteroidShip(GameObject& asteroid, GameObject& spaceShip) {
        shipAsteroid(spaceShip, asteroid);
    }
    void stationShip(GameObject& spaceStation, GameObject& spaceShip) {
        shipStation(spaceShip, spaceStation);
    }
    void stationAsteroid(GameObject& spaceStation, GameObject& asteroid) {
        asteroidStation(asteroid, spaceStation);
    }
    // 类型定义
    typedef void (*HitFunctionPtr)(GameObject&, GameObject&);
    typedef std::map<std::pair<std::string, std::string>, HitFunctionPtr> HitMap;
    // 辅助函数
    std::pair<std::string, std::string> makeStringPair(const char* s1, const char* s2) {
        return std::make_pair(s1, s2);
    }
    // 初始化碰撞映射表
    HitMap* initializeCollisionMap() {
        HitMap* phm = new HitMap;
        (*phm)[makeStringPair("SpaceShip", "Asteroid")] = &shipAsteroid;
        (*phm)[makeStringPair("SpaceShip", "SpaceStation")] = &shipStation;
        (*phm)[makeStringPair("Asteroid", "SpaceStation")] = &asteroidStation;
        return phm;
    }
    // 查找碰撞处理函数
    HitFunctionPtr lookup(const std::string& class1, const std::string& class2) {
        static std::auto_ptr<HitMap> collisionMap(initializeCollisionMap());
        HitMap::iterator mapEntry = collisionMap->find(std::make_pair(class1, class2));
        if (mapEntry == collisionMap->end()) return nullptr;
        return (*mapEntry).second;
    }
}
```

CollisionMap 类

```plain
class CollisionMap {
public:
    typedef void (*HitFunctionPtr)(GameObject&, GameObject&);
    void addEntry(const std::string& type1, const std::string& type2, HitFunctionPtr collisionFunction, bool symmetric = true) {
        hitMap[std::make_pair(type1, type2)] = collisionFunction;
        if (symmetric) {
            hitMap[std::make_pair(type2, type1)] = collisionFunction;
        }
    }
    void removeEntry(const std::string& type1, const std::string& type2) {
        hitMap.erase(std::make_pair(type1, type2));
        hitMap.erase(std::make_pair(type2, type1));
    }
    HitFunctionPtr lookup(const std::string& type1, const std::string& type2) {
        HitMap::iterator it = hitMap.find(std::make_pair(type1, type2));
        if (it != hitMap.end()) {
            return it->second;
        }
        return nullptr;
    }
    static CollisionMap& theCollisionMap() {
        static CollisionMap instance;
        return instance;
    }
private:
    CollisionMap() {}
    CollisionMap(const CollisionMap&) = delete;
    CollisionMap& operator=(const CollisionMap&) = delete;
    std::map<std::pair<std::string, std::string>, HitFunctionPtr> hitMap;
};
```

注册碰撞处理函数

```plain
class RegisterCollisionFunction {
public:
    RegisterCollisionFunction(
        const std::string& type1,
        const std::string& type2,
        CollisionMap::HitFunctionPtr collisionFunction,
        bool symmetric = true
    ) {
        CollisionMap::theCollisionMap().addEntry(type1, type2, collisionFunction, symmetric);
    }
};
// 全局对象注册碰撞处理函数
RegisterCollisionFunction cf1("SpaceShip", "Asteroid", &collision::shipAsteroid);
RegisterCollisionFunction cf2("SpaceShip", "SpaceStation", &collision::shipStation);
RegisterCollisionFunction cf3("Asteroid", "SpaceStation", &collision::asteroidStation);
// 新增的 Satellite 类
class Satellite : public GameObject {
public:
    virtual void collide(GameObject& otherObject) override {
        processCollision(*this, otherObject);
    }
};
// 新增的碰撞处理函数
void satelliteShip(GameObject& satellite, GameObject& spaceShip) {
    // 处理卫星与飞船的碰撞
    // ...
}
void satelliteAsteroid(GameObject& satellite, GameObject& asteroid) {
    // 处理卫星与小行星的碰撞
    // ...
}
// 注册新的碰撞处理函数
RegisterCollisionFunction cf4("Satellite", "SpaceShip", &satelliteShip);
RegisterCollisionFunction cf5("Satellite", "Asteroid", &satelliteAsteroid);
```

基类 GameObject声明一个纯虚函数 collide，所有派生类都必须实现这个函数。定义了一个 UnknownCollision 异常类，用于处理未知类型的碰撞。

定义了一系列的碰撞处理函数，如 shipAsteroid、shipStation 等。

使用无名命名空间来包含这些函数，确保它们仅在当前编译单元内可见。

makeStringPair 辅助函数用于创建 pair<string, string> 对象。initializeCollisionMap 函数初始化碰撞映射表。lookup 函数查找并返回相应的碰撞处理函数。

CollisionMap 类：

提供了动态管理碰撞映射表的功能。

addEntry 方法用于添加新的碰撞处理函数。

removeEntry 方法用于移除已有的碰撞处理函数。lookup 方法用于查找碰撞处理函数。

theCollisionMap 方法确保整个系统中只有一个 CollisionMap 实例。

注册碰撞处理函数：

使用 RegisterCollisionFunction 类的全局对象在程序启动时自动注册碰撞处理函数。

当新增一个派生类（如 Satellite）时，只需注册新的碰撞处理函数，无需修改现有代码。

（1）优点

灵活性：CollisionMap 类允许在运行时动态添加、删除或修改碰撞处理函数。

扩展性：新增类和碰撞处理函数时，只需注册新的函数，无需修改现有代码。

封装性：碰撞处理函数和映射表的管理封装在 CollisionMap 类中，减少了代码的耦合度。

（2）缺点

性能开销：动态管理映射表会带来一定的性能开销，尤其是在频繁添加和删除碰撞处理函数的情况下。

复杂性：引入了 CollisionMap 类和 RegisterCollisionFunction 类，增加了代码的复杂性。

<h1 id="hkYNt">**杂项 **</h1>
<h4 id="u8XLh">**Item M32：在未来时态下开发程序**</h4>
（1）适应变化：软件开发人员应认识到软件需求和技术环境会随时间变化，因此在设计和编码时应具备前瞻性，使软件能够适应未来的需求和平台变化。

（2）预防性设计：在设计类和接口时，应该考虑未来可能出现的功能扩展和代码重用。例如，如果一个类有可能被继承，则应提前声明虚析构函数，即便当前没有派生类。

（3）使用语言特性强制设计约束：利用C++的语言特性来实现设计意图，比如通过将某些成员函数声明为私有来防止不希望的操作，或者使用模板和泛型编程来提高代码的灵活性和复用性。

（4）易于理解和维护：编写清晰、易于理解的代码，考虑到未来的维护者可能不是原始开发者。这包括提供良好的文档、遵循良好的编程实践等。

（5）健壮性和错误处理：预见用户可能会犯的错误，并设计代码以防止、检测或修复这些错误。这有助于提高软件的健壮性和用户体验。

（6）可移植性：尽可能编写可移植的代码，除非性能是绝对关键的因素。可移植的代码更容易适应不同的平台和环境，也有助于扩大用户基础。

（7）局部化变化的影响：设计时应尽量减小更改一处代码对其他部分的影响，通过封装和模块化等方式实现。

（8）考虑长期生存能力：虽然软件需要满足当前的需求，但是过度关注眼前可能会牺牲软件的长期生存能力和价值。优秀的软件不仅能满足当前的需求，还能在未来的变化中保持相关性和竞争力。

<h4 id="NBw3W">**Item M33：将非尾端类设计为抽象类**</h4>
非尾端类（Non-Leaf Class）指那些被设计为基类，且预期会有派生类的类。这些类通常包含一些通用的行为和属性，但它们本身可能不适合直接实例化。它们的设计目的是为了让派生类继承并扩展其功能。在继承层次中，如果基类 Animal 有赋值操作符 operator=，而派生类 Lizard 和 Chicken 也有各自的赋值操作符，通过基类指针进行赋值时，只会调用基类的赋值操作符，导致部分赋值问题

首先，我们来看一下作者给出的初始代码示例，以及如何通过引入抽象类来解决问题。

```plain
class Animal {
public:
    Animal& operator=(const Animal& rhs);
    // 其他成员...
};
class Lizard : public Animal {
public:
    Lizard& operator=(const Lizard& rhs);
    // 其他成员...
};
class Chicken : public Animal {
public:
    Chicken& operator=(const Chicken& rhs);
    // 其他成员...
};
```

```plain
Lizard liz1;
Lizard liz2;
Animal *pAnimal1 = &liz1;
Animal *pAnimal2 = &liz2;
*pAnimal1 = *pAnimal2;  // 只赋值了 Animal 部分，Lizard 部分未被赋值
```

解决方案：引入抽象类

```plain
class AbstractAnimal {
protected:
    AbstractAnimal& operator=(const AbstractAnimal& rhs);
public:
    virtual ~AbstractAnimal() = 0;  // 纯虚析构函数
    // 其他成员...
};
// 纯虚析构函数的实现
AbstractAnimal::~AbstractAnimal() {}
class Animal : public AbstractAnimal {
public:
    Animal& operator=(const Animal& rhs);
    // 其他成员...
};
class Lizard : public AbstractAnimal {
public:
    Lizard& operator=(const Lizard& rhs);
    // 其他成员...
};
class Chicken : public AbstractAnimal {
public:
    Chicken& operator=(const Chicken& rhs);
    // 其他成员...
};
```

**解决方案：**

通过引入一个抽象基类 AbstractAnimal，将 Animal、Lizard 和 Chicken 都从 AbstractAnimal 继承。

纯虚析构函数：在 AbstractAnimal 中声明一个纯虚析构函数 virtual ~AbstractAnimal() = 0;，并在类外实现它。这使得 AbstractAnimal 成为一个抽象类，不能被实例化。

保护赋值操作符：将 AbstractAnimal 的赋值操作符声明为 protected，以防止通过基类指针进行赋值。

**优点：**

通过抽象类和保护赋值操作符，避免了通过基类指针进行部分赋值的问题。

确保赋值操作的正确性，避免运行时错误。如果未来需要添加新的动物类，只需从 AbstractAnimal 继承即可，不会影响现有代码。

**缺点：**

增加复杂度：引入抽象类会增加代码的复杂度，需要额外的类和成员函数。

编译开销：需要重新编译涉及 Animal、Lizard 和 Chicken 的代码。

**设计原则：**

非尾端类应该是抽象类：如果一个类被设计为基类，且有派生类，那么它应该是抽象类。这有助于确保派生类的行为正确性，避免部分赋值等问题。

明确抽象行为：通过引入抽象类，强迫设计者明确识别和定义有用的抽象行为，提高代码的可读性和可维护性。

<h4 id="HnKbq">**Item M34：如何在同一程序中混合使用 C++和 C**</h4>
_**1. 确保编译器兼容性**_

在混合使用 C++ 和 C 时，首先需要确保 C++ 编译器和 C 编译器生成的 .obj 文件是兼容的。因为不同的编译器在实现相关的特性（如 int 和 double 的字节大小、传参方式等）上可能有所不同。

_**2. 名变换**_

名变换：C++ 编译器会给函数名进行变换，以支持函数重载。C 语言中没有函数重载，因此不需要名变换。如果 C++ 代码调用 C 函数，C++ 编译器会对函数名进行变换，而 C 代码中函数名保持不变，导致链接错误。

解决方法：使用 extern "C" 声明 C 函数，禁止名变换。

```plain
extern "C" void drawLine(int x1, int y1, int x2, int y2);
```

批量声明：可以使用 extern "C" 包裹一组函数，以简化代码。

```plain
extern "C" {
    void drawLine(int x1, int y1, int x2, int y2);
    void twiddleBits(unsigned char bits);
    void simulate(int iterations);
}
```

条件编译：使用 #ifdef __cplusplus 宏来确保在 C++ 编译时添加 extern "C"，而在 C 编译时不添加。

```plain
#ifdef __cplusplus
extern "C" {
#endif
void drawLine(int x1, int y1, int x2, int y2);
void twiddleBits(unsigned char bits);
void simulate(int iterations);
#ifdef __cplusplus
}
#endif
```

_**3. 静态初始化**_

静态初始化：C++ 在 main 执行前会自动调用静态对象的构造函数，执行静态初始化。同样，在 main 结束后会调用静态对象的析构函数。

如果 main 是用 C 编写的，静态对象的构造和析构可能不会被正确调用。

解决方法：用 C++ 写 main 函数，即使程序的大部分是用 C 编写的。可以通过将 C 的 main 改名为 realMain，然后在 C++ 的 main 中调用 realMain。

```plain
extern "C" int realMain(int argc, char *argv[]);
int main(int argc, char *argv[]) {
    return realMain(argc, argv);
}
```

_**4. 动态内存分配**_

规则：C++ 部分使用 new 和 delete，C 部分使用 malloc 和 free。

用 free 释放 new 分配的内存或用 delete 释放 malloc 分配的内存，其行为未定义。

解决方法：严格隔离 new 和 delete 与 malloc 和 free 的使用。

```plain
char *strdup(const char *ps); // C 函数库中的函数
char *str = strdup("Hello, World");
free(str); // 用 free 释放 C 函数库分配的内存
```

尽量避免调用那些既不在标准库中也没有固定形式的函数，以减少可移植性问题。

5. 数据结构的兼容性

C 函数不了解 C++ 的特性，因此在 C++ 和 C 之间传递数据时，只能使用 C 可表示的概念。：可以安全传递普通指针、指向非成员函数或静态成员函数的指针、结构和内建类型（如 int、char 等）。

结构兼容性：C++ 中的 struct 规则兼容 C 中的规则，因此相同的结构可以在 C++ 和 C 之间安全传递。增加非虚成员函数不会影响兼容性，但增加虚函数或进行继承会影响内存结构，从而影响兼容性。

```plain
struct Point {
    int x;
    int y;
};
```

```plain
// C++ 版本
struct PointWithMethods {
    int x;
    int y;
    void move(int dx, int dy) {
        x += dx;
        y += dy;
    }
};
```

```plain
// C 版本
struct PointWithMethods {
    int x;
    int y;
};
```

