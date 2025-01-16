# ICS 45C Homework 2

## Getting the assignment

1. Accept the assignment by click the link in canvas
2. Once you accept the invite you will reach a page that says "You're ready to go"
3. Clik the url from that page that looks similar to this: `https://github.com/klefstad-teaching/ics-45c-hw2-<GitHubUsername>`, it may take a bit of time for the repo to be ready.
4. There will be a green `<> Code` Button on the top right, click that and then click the middle tab `SSH`
5. Copy that link
6. Go to your hub and go into the ICS45C folder, and open up the terminal and type in the following command:
```bash
git clone git@github.com:klefstad-teaching/ics-45c-hw2-<GitHubUserName>.git HW2
```
7. Go to VSCode and open up the `HW2` Folder

## Directory Structure

```bash
├── CMakeLists.txt
├── CMakePresets.json
├── gtest
│   ├── coins_gtests.cpp
│   ├── gtestmain.cpp
│   └── word_count_gtests.cpp
└── src
    ├── coins.cpp
    ├── coins.hpp
    ├── coins_menu.cpp
    ├── coins_simple.cpp
    ├── coins_transfer.cpp
    ├── student_gtests.cpp
    ├── word_count.cpp
    ├── word_count.hpp
    └── word_count_main.cpp
```

## Overview and Objectives

Homework 2 consists of 2 problems in C++.
  1. Implement a class named `Coins` that functions as a coin holder, which allows coins to be deposited and removed, and to compute the dollar value of the coins it contains. All the functionality of class `Coins` is **declared** in a single `.hpp` header file and **defined** in a separate single `.cpp` file. Its functionality is used in 3 different scenarios—with each scenario in a different small program with a `main()` function. The coin holder problem is divided into these 3 small programs, instead of one big one, to illustrate the principle of **incremental development** that you should always follow when writing a new program: first, write a simple program, test it thoroughly, then expand its functionality a little at a time, testing each addition thoroughly before proceeding to the next step.
  2. Implement a word counter named `word_count`, which counts the frequency of each word in an input file, while ignoring words from another file containing [stop words](https://en.wikipedia.org/wiki/Stop_word), and writes a formatted summary of frequencies to an output file. This program will use the higher-level data structures `std::set` and `std::map` from the Standard Template Library (STL) to store the stop words and the word frequencies, respectively. STL set and map are similar to Python data structures (`set` and `dictionary`) that you may have used in the past.

## Relation to Course Objectives

With class `Coins`, you use a wider variety of C++ **statements** and **expressions** to
 - Gain experience implementing **classes**, this time with separate files for declarations, definitions, and a separate `main()`.
 - Write an **inserter** to print objects.
 - Use two parameter passing modes in C++:  pass by copy/value and **pass by reference**.
 - Use `const` to protect values from accidental modification.
 - Use `constexpr` for naming static expressions (whose value the compiler can determine at compile-time).

With `word_count`, you gain experience with **file input/output**, as well as gaining introductory exposure to the highest level of C++ using `map` and `set` from the C++ **Standard Template Library (STL)**.

## 1. Coins, a coin holder

Write a class, named `Coins`, that is a holder for the 4 most commonly used [denominations of US coins](https://www.usa.gov/currency): quarters, dimes, nickels, and pennies, to which each denomination of coin can be added or removed. The number of each coin denomination is tracked, as well as the total value. Visualize it as the device in this picture (but with very large capacity):

![Coin Holder](/assets/1-1.png)

Class `Coins` contains 4 counters to track how many of each coin denomination it contains, because Coins cannot remove more than it holds of any denomination, so coins can be removed from the holder only if there is enough of **each** denomination to make **exact change**. For example, Coins cannot convert a quarter into 2 dimes and 1 nickel.

All the functionality of class Coins will be **declared** in its header file, `coins.hpp`, and **defined** in its `.cpp` file, `coins.cpp`. Its functionality will be used in 3 different scenarios with different `main()` functions, each in its own separate file.

 - The first scenario (in `coins_simple.cpp`) initializes one Coins object, withdraws the coins needed to buy an item, and prints the balance remaining. All functions are called from a main() function with no user/keyboard input.
 - The second scenario (in `coins_objects.cpp`) initializes two Coins objects, withdraws coins from one, transfers coins between the two, and deposits coins in the second. All functions are still called from a main() with no user/keyboard input.
 - The third scenario (in `coins_menu.cpp`) presents a menu of choices to get user input from the keyboard, allowing the user to deposit coins and remove them (if enough are available), and prints the total balance.

The following sections provide more details and commented starter code to implement class Coins. Read these sections carefully to understand what the program must do, but then follow the Steps of Development for each scenario below, to implement the functionality in the 5 different files.

## 1.1 coins.hpp: declaration of class Coins

The class declaration for class `Coins`, in a file named `coins.hpp`, is given in the screenshot below. Read the comments carefully for details about what each piece does.


  > Note: Type the code carefully into your file, thinking about the meaning of each thing you type. You do **not** need to type the comments; just refer to them as necessary.

Screenshot of coins.hpp

![Screenshot of coins.hpp](/assets/1-1-1.png)

## 1.2 coins.cpp: definition of class Coins

In the file `coins.cpp`, write the definitions of the methods given in the class declaration, but follow the Steps of Development below for the **order** in which to write them, starting with the order given in `coins_simple.cpp` below.

**Be sure to implement ONE THING AT A TIME and TEST IT thoroughly, using the GTest framework**

coins.cpp #includes screenshot

![coins.cpp #includes screenshot](/assets/1-2-1.png)

## 1.3 coins_simple.cpp 

As shown in the code screenshot below, `coins_simple` defines one `Coins` object `pocket`, initializing pocket with some coins, prints the number of coins, withdraws the coins needed to buy a candy bar, prints information about that transaction, and finally prints the balance remaining.

 > Note that you also write the **inserter <<** for `Coins` and a function (not a method) `coins_required_for_cents()` as described in the comments in the `coins.hpp`.

coins_simple.cpp screenshot

![coins_simple.cpp screenshot](/assets/1-3-1.png)

**Be sure to implement ONE THING AT A TIME and TEST IT thoroughly, using the GTest framework, following these Steps of Development below.**

Steps of Development for coins_simple.cpp

Start by writing and using a small subset of the functionality of class `Coins`.

1. In coins_simple main(), you first define a Coins object, pocket, so you need to write the constructor. To see that your constructor works, you need to write the print method called in line 12. (Comment out lines 13-17.)
2. Now write the methods needed to buy the candy bar in lines 13-14: `coins_required_for_cents` and `extract_exact_change`.

   **Algorithm for extracting exact change:**  You may keep your algorithm for extracting exact change simple, even though it would not be robust enough for certain situations.
   
   **Example 1.3.1:**
       `coins_required_for_cents(100)`
   
   should return
       `4 quarters, 0 dimes, 0 nickels, 0 pennies`
   
   It should first take as many quarters, then as many dimes, etc....  You can do this with simple algorithms using division, or by using while loops to determine a number for each type of coin, then constructing a corresponding Coins object to return. This function for deciding which coins to use does not need to know how a Coins object works, it just needs to be able to construct one. (Hint in white font: save how many of each coin denomination you need using 4 local int variables, then declare a Coins object on the stack with those values used to initialize the Coins object, then you can return that Coins object.)
   
   **Example 1.3.2:**
   
   Suppose you have 1 quarter, 5 dimes, 0 nickels, 0 pennies and you want to withdraw 50 cents. If you take the quarters first, then you can't do it....  You would need to be able to replace the quarter and then take 5 dimes.

   However, your program does not need to handle these difficult cases in this Homework! You may solve this problem when you take a course in algorithms.

3. Now uncomment lines 13-17, write the needed functions, and test them, get them working.
   
## 1.4 coins_transfer.cpp

In a file named `coins_transfer.cpp`, develop more of the functionality of class `Coins` in the following scenario, which creates **two** `Coins` objects and transfers coins from one to the other. Write `main()` on your own, with the same #includes and `using` statements as `coins_simple`.

Steps of Development for coins_transfer.cpp

In `main()`, write ONE STEP AT A TIME of the scenario below, implementing any new methods needed for each step. **Compile and test each method thoroughly** with the GTest framework before moving on to the next one.

In each of the following steps, make sure to follow the format shown in the sample output in the screenshot below.

 1. Create two `Coins` objects:
     1. `piggyBank` with 50 quarters, 50 dimes, 50 nickels, 50 pennies
     2. `pocket` with 8 quarters, 3 dimes, 6 nickels, 8 pennies
 2. Print `piggyBank` and `pocket`, following the format shown in the output screenshot below.
 3. Buy a bag of chips that costs $1.49, removing the coins from your `pocket`. Print the transaction in the 3 lines shown in the output screenshot.
 4. Transfer $4.05 from `piggyBank` to `pocket`, printing the output lines as shown below
 5. While vacuuming, you find loose change in your sofa:  10 quarters, 10 dimes, 10 nickels, 10 pennies. Put it all in your `piggyBank` as follows:
    → To enter the amount found in your sofa, create an object named `sofa` with the number of coins found, then call deposit_coins to transfer coins from `sofa` to `piggyBank`, then print sofa, then print `piggyBank` to show that the sofa coins were transferred to `piggyBank`.

 6. Finally, print the dollar-and-cents value of the total balance remaining you now have in your `piggyBank`. Use the format as in this example: **$123.05**

**Be sure to implement ONE STEP AT A TIME and TEST IT thoroughly, using the GTest.**

Sample output of coins_transfer

![Sample output of coins_transfer](/assets/1-4-1.png)

## 1.5 coins_menu.cpp
In a file named `coins_menu.cpp`, call `coins_menu` that presents a text-based menu interface that allows a user to deposit change, extract change, and print the total balance, in a single `Coins` object called `myCoins`. Be sure you don't allow the user to extract more money than they have! You may assume the user gives the correct type of input (i.e., integers for commands and numbers of each coin.)  Follow the formatting shown in the sample output file below:

[Sample of running coins_menu](https://ics.uci.edu/~klefstad/public/45c/hw2/coin_menu_example.txt)

**Be sure to implement ONE THING AT A TIME and TEST IT thoroughly, using the GTest framework.**

Screenshot of coins_menu.cpp

![Screenshot of coins_menu.cpp](/assets/1-5-1.png)

## 2 word_count

Write a program named `word_count` that reads words from a file named `sample_doc.txt` - one at a time - and counts the frequency of each word, but ignores all stop words in a file named `stopwords.txt`. It then prints to the output file named `frequency.txt` the words with their corresponding frequencies in ascending alphabetical order by word.

The frequency counting is [case insensitive](https://en.wikipedia.org/wiki/Case_sensitivity), counting all the letters as if they were lowercase, so you must convert any uppercase letters to lowercase before you increment their frequency count. For example, 5 Apple and 2 apple would be counted as 7 apple. Your `word_count` program must use `std::string` for the words, `std::set` to store the stopwords, and `std::map` to store the word frequencies.

All the declarations for `word_count` are in `word_count.hpp`, and all the definitions are in `word_count.cpp`. The main function defined in `word_count_main.cpp`, is fully implemented in the screenshot below, but it will work only if you correctly define in `word_count.cpp` the functions declared in `word_count.hpp`.

**Be sure to implement ONE THING AT A TIME and TEST IT thoroughly, using the GTest framework provided, following these Steps of Development. During development, you should add some of your own tests to `student_gtests.cpp` - see below!**

Steps of Development for word_count

1. Define a map of word frequencies. The key is the word, and the value is the `int `counter for the word frequency. The counters will all be initialized to zero when you create the map.
2. Define a set of stop words. Insert each word from the stopwords file into this `set` as part of your initialization. Remember that everything is case insensitive!
3. Next, read each word from the input file. If it is not a stop word, then increment the counter associated with that word in your word frequency map.
4. When finished counting the words in the input file, write the words with their associated frequency count (one per line, in the ascending alphabetical order A to Z) to the output file. Traversing the `map` using a `for` loop iterator will automatically visit each entry in ascending alphabetical order by key, so no sorting is necessary when using map. Use the following format of your output with no extra newlines.
    1. `<Word><space><Count><newline>`
   
Sample input files
 - [sample_doc.txt](https://ics.uci.edu/~klefstad/public/45c/sample_doc.txt)
 - [stopwords.txt](https://ics.uci.edu/~klefstad/public/45c/stopwords)

Sample output:
```
apple 7
baker 3
computer 20
… // many other words with their counts
xylophone 1
yeast 2
zebra 1
```

Screenshot of word_count.hpp

![Screenshot of word_count.hpp](/assets/2-1.png)

Screenshot of word_count_main.cpp

![Screenshot of word_count_main.cpp](/assets/2-2.png)

## 2.1 student_gtests.cpp

Previously, we supplied some basic GTest files for you to experiment with and learn from. For this homework, you will need to write some of your own tests for the word count functions, and **your tests will also be graded by the autograder**. The tests which are submitted and graded must be in the `student_gtests.cpp` file. We provided some simple tests in `gtest/word_count_gtests.cpp` for inspiration, but it is **not** enough to only copy these!

As you can see in the provided sample tests, the tests work by running the function being tested on some input, and then checking if the result matches our expectations with gtest's macros:

 - `EXPECT_TRUE(x)` checks if `x` evaluates to `true`.
 - `EXPECT_FALSE(x)` checks if `x` evaluates to `false`.
 - `EXPECT_EQ(x, y)` checks if `x == y`.
 - `EXPECT_STREQ(x, y)` checks if `x` and `y` are equal as C-strings.
   
Screenshot of student_gtests.cpp

![Screenshot of student_gtests.cpp](/assets/2-1-1.png)

The grading for these tests works as follows: we run your tests against one correct implementation and several buggy implementations. In order to get full points, **all** of your tests must **pass on the correct implementation**, and **at least one** of your tests must **fail on every buggy implementation**. On the autograder this will show up as "`* (student_gtests_correct)`" and "`* (student_gtests_buggy_…)`" respectively.

> 💡For more detail and an **example** of how to write GTests, watch this video (can start around 8:00). Really! Watch the [video](https://www.youtube.com/watch?v=_OHE33s7EKw)! One important thing to learn is that a good test will reveal bugs in your code by failing when the tested code has an error, and by succeeding when your code is correct.

## How to Build, Submit, and Grade the programs

Build, run, and test your programs using the CMake and GTest files and build instructions provided [here](#build-instructions)

In GradeScope for Homework 2, upload your hw2 repo from GitHub, which will upload all the files necessary for the autograder.

## Build Instructions

```bash
# Produce the `build` folder with the presets provided for the homework:
cmake --preset default

# Build all targets at once:
cmake --build build

# Build only coins_simple.cpp:
cmake --build build --target coins_simple

# Build only coins_menu.cpp:
cmake --build build --target coins_menu

# Build only coins_transfer.cpp:
cmake --build build --target coins_transfer

# Build only coins gtests:
cmake --build build --target coins_gtests

# Build student gtests
cmake --build build --target student_gtests

# Build only word_count_main.cpp:
cmake --build build --target word_count

# Build only word_count gtests:
cmake --build build --target word_count_gtests
```

To run the above targets after compiling them:

```bash
./build/coins_simple        # Runs the 'main' function from src/coins_simple.cpp
./build/coins_menu          # Runs the 'main' function from src/coins_menu.cpp
./build/coins_transfer      # Runs the 'main' function from src/coins_transfer.cpp
./build/word_count          # Runs the 'main' function from src/word_count_main.cpp
./build/student_gtests      # Runs the 'word_count' gtests that will be graded
./build/coins_gtests        # Runs the 'coins' gtest set of tests
./build/word_count_gtests   # Runs the 'word_count' gtests
```

Once you have run the code above and it either produces the output you expected or passes
all provided tests, congratulations! You are now ready to [submit](#submission) your homework!

## Submission

As with previous submissions, you can submit via `GitHub` by:

1. `git add .`
2. `git commit -a -m "Commit Message Here"`
3. `git push --set-upstream origin main`

your changes to your private repository, and then submitting the `hw2` branch to `Gradescope`.

## Credit

All code moved from [ICS45c](https://github.com/RayKlefstad/ICS45c/tree/hw2)
