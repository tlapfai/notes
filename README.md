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
    
