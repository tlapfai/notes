**C++**

Arrays 不允許複製或賦值

    int ia[] = {0, 1, 2};
    int ia2[](ia); // error

pointers相當於arrays的iterators，當我們把deference運算子和increment運算子套用於"指向array元素"的pointer時，其行為和套用在iterator身上類似

    string s("hello world");
    string *sp = &s;  // sp = address-of s

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
