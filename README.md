# Basic background
1. When multiple tests in a test suite need to share common objects and subroutines,  
   you can put them into a test fixture class.

2. The assertions come in pairs that test the same thing but have different effects on the current function.  
    - ASSERT_* versions generate fatal failures when they fail, and abort the current function.  
    - EXPECT_* versions generate nonfatal failures, which don't abort the current function.  

3. Use the TEST() macro to define and name a test function. 
  ```
    TEST(TestSuiteName, TestName) {
    ... test body ...
    }
  ```

4. If you find yourself writing two or more tests that operate on similar data, you can use a test fixture.  
 - Derive a class from `::testing::Test`
 - If necessary, write a default constructor or `SetUp()` function to prepare the objects for each test.   
 - If necessary, write a destructor or `TearDown()` function to release any resources you allocated in `SetUp()`.  
 - Use `TEST_F()` to access objects and subroutines in the test fixture  
 - Different tests in the same test suite have different test fixture objects,  
   and googletest always deletes a test fixture before it creates the next one.  

# How to install (Ubuntu)
1. Download the sources
2. Build
3. Install 

Full instruction for getting the latest and greatest version is below
```
git clone https://github.com/google/googletest
cd googletest
sudo mkdir build
cd build
sudo cmake ..
sudo make
sudo cp lib/*.a /usr/lib  # (or use `sudo make install`, but be aware that this has not worked always in the past)
```

# Verify with sample project 
Put following code in sample.cpp  
```
#include <gtest/gtest.h>

TEST(MathTest, TwoPlusTwoEqualsFour) {
    EXPECT_EQ(2 + 2, 4);
}

int main(int argc, char **argv) {
    ::testing::InitGoogleTest( &argc, argv );
    return RUN_ALL_TESTS();
}
```

## Build command 
`g++ sample.cpp -lgtest -lpthread`

## Execute binary
`./a.out`

# Skip the main
If you don't have main e.g. sample.cpp 
```
#include <gtest/gtest.h>

TEST(MathTest, TwoPlusTwoEqualsFour) {
    EXPECT_EQ(2 + 2, 4);
}
```

## You can link with googltest's predefined `gtest_main` library  
`g++ sample.cpp -lgtest_main -lgtest -lpthread`  

## Execute binary  
`./a.out`
