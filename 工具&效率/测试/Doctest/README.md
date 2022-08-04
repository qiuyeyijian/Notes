# Doctest



```cpp
#define DOCTEST_CONFIG_IMPLEMENT_WITH_MAIN
#include "doctest.h"

#include<string>


class Base
{
private:
	static int UID;

protected:
	std::string name;

public:
	Base() : name("myBase")
	{
	}

protected:
	int getUID()
	{
		return ++UID;
	}
};

int Base::UID = 0;

TEST_CASE_FIXTURE(Base, "test-1")
{
	printf("UID: %d\n", getUID());
	SUBCASE("dd")
	{

	}
}

TEST_CASE_FIXTURE(Base, "test-1")
{
	printf("UID: %d\n", getUID());
}
```



































