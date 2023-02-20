**C++**

Arrays 不允許複製或賦值

    int ia[] = {0, 1, 2};
    int ia2[](ia); // error

pointers相當於arrays的iterators，當我們把deference運算子和increment運算子套用於"指向array元素"的pointer時，其行為和套用在iterator身上類似

    string s("hello world");
    string *sp = &s;  // sp = address-of s
    

**Pointer and Constant**

對於pointer來說，指向const int和指向int是兩種不同的指針，不能互相賦值

Read it backwards:

    int * ptr; // pointer to int
    const int * ptr; // pointer to const int, allowed to redirect ptr, but the content (const int) cannot be modified
    int * const ptr; // const pointer to int, not allowed to redirect ptr
    const int * const ptr; // const pointer to const int

*const* beside ptr is to modify the constancy of the pointer, we must initalize the pointer in its definition

**Call by pointer**

    void swap(int* p1, int* p2) {
        int temp = *p1;
        *p1 = *p2;
        *p2 = temp;
    }
    
    int a = 10;
    int b = 20;
    
    swap(&a, &b);
    

**引用實際上是指針常量**

int& 即是 int* const，指針常量是不可改指向，所以引用也是不能更改的
指針常量是一個指針，但不可另指新位址

    int a = 10;
    int& aRef = a;
    // 相當於
    // int* const aRef = &a; //指針常量，不可另指
    aRef = 20;
    // 相當於
    // *aRef = 20;

    
**Reference as a return**

Invoking function can be a left-value.

    int& test() {
        static int a = 10;  // saved in global memory
        return a;
    }
    
    int main() {
        int &ref = test();
        cout << "ref = " << ref << endl;  // ref = 10
        test() = 20;  // can be as a left-value, test() returns a reference of a
        
        return 0;
    }
    
初始化引用的其中一個方法

    const int& ref = 10;
    // int temp = 10; 
    // const int& ref = temp;
    // 不可修改變量 ref = 20;會發生錯誤
    

non-static methods do not belong to object
only non-static attributes belong to object

Size of blank object (contains no attributes) is 1 byte, because every object have to own 

*this* pointers are implicitly embedded

**Class method with const**

    class Person {
        void getAge() const {} // this const makes the member variables can not amended by getAge
    }
    

Static variable is a property of a Class rather than the instance of class. It represent a kind of a global value for all the instances of that class and can able to call them using class name.

https://stackoverflow.com/questions/177437/what-does-const-static-mean-in-c-and-c

https://blog.csdn.net/Augenstern_QXL/article/details/117249021
