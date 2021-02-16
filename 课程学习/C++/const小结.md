## 限定成员函数时:

声明带有 **const** 关键字的成员函数指定，函数是 "只读"函数，在它被调用的时候不会修改对象。 **一个常数成员函数不能修改任何非静态数据成员或调用不是常数的任何成员函数。**若要声明常数成员函数，请在参数列表的右括号后放置**const**关键字,把const关键字放在函数的参数表和函数体之间。有人可能会问：为什么不将const放在函数声明前呢？因为这样做意味着函数的返回值是常量，意义完全不同。 **声明和定义中均要求该 const 关键字。**

``` c++
// constant_member_function.cpp
class Date
{
public:
Date( int mn, int dy, int yr );
int getMonth() const; // A read-only function
void setMonth( int mn ); // A write function; can't be const
private:
int month;
};

int Date::getMonth() const
{
return month; // Doesn't modify anything
}
void Date::setMonth( int mn )
{
month = mn; // Modifies data member
}
int main()
{
Date MyDate( 7, 4, 1998 );
const Date BirthDate( 1, 18, 1953 );
MyDate.setMonth( 4 ); // Okay
BirthDate.getMonth(); // Okay
BirthDate.setMonth( 4 ); // C2662 Error
}
```

