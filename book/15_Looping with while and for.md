# Chapter 15 - Looping with while and for

Often we need to repeat a block of code a number of times (a for-loop) or as long as a condition is true (a while-loop). Looping is also often called _iterating_.

## 15.1 While loop
See _15.1_while.jai_:

```c++
```

In line (1) we see the typical while condition { } loop. The one-line while in line (2) shows that ( ) around the condition and { } are optional.  
The while block is executed as long as condition is true, which is checked at the beginning of each new iteration of the loop.

Like with if (see § 14.4) the 'condition' can simply be a variable: as long as the variable is not zero, empty or null the loop can continue. We see this in action in lines (3)-(5).
Line (6) shows an if inside a while, which is used to terminate the loop. 

### 15.1.1 Nested while loops
Starting in line (6), we see a loop over counter i which is enclosed with a loop over counter j. At the end of the respective loops, we must increment the counter. Using `defer` we can code this at the start of the loop to achieve that.

> If a while loop uses a counter, change the counter by prefixing with defer. But  be careful in combination with break and continue (put defer after these).

### 15.1.2 Named while-loop
In line (7) you see the same while loop as used for the counting down, but now the condition has been given a name (`counting`). This name can be used to break out of the loop when you have nested while loops (see § 15.3).

### 15.1.3 Printing out a recursive list
In § 12.6 we constructed a linked-list with a recursive struct ListNode. To print out its data, now we can do this through a loop mechanism, see _15.2_while_looping_through_a_linked_list.jai_. This has the same definition of ListNode and variable lst as in example 13.3_linked_list.jai, so we omit this code here; the following code is used between c.next = null and the free statements:

```c++
print("List printed in a while loop: \n");
    print("% -> ", lst.data);
    r := lst.next;              //   (1)
    while r {                   //   (2) 
        print("% -> ", r.data);
        r = r.next;             //   (3) 
    }
    print("\n");
    // => List printed in a while loop:
    // => 0 -> 12 -> 24 -> 36 ->
```
We declare a variable r of type *ListNode in line (1). As long as r is not a null pointer, the while loop in (2) will keep on going. We print out the data of the loop and point to the next node in line (3). In our example r becomes null for c, which stops the loop.


## 15.2 For loop
When you want to iterate over a range of things, `for` is your best tool.

See _15.3_for.jai_:

```c++
#import "Basic";

main :: () {
// for loop over a range:
    for 1..5 print("Number % - ", it);              // (1)
    print("\n");
    for number: 1..5 print("Number % - ", number);  // (2)
    print("\n");
    
    count := 5;           // (3)
    for n: 1..count {
        print("Number % - ", n);
    }
    print("\n");

// Reverse for loop: 
    for < i: 5..0 { // (4) => 5 4 3 2 1 0
        print("% - ", i); 
    }
    print("\n");

// Nested for loops:
    for i: 1..5 {   // (5)
        for j: 10..15 {
            print("%, % / ", i, j);
        }
        print("\n");
    }
}

/*
Number 1 - Number 2 - Number 3 - Number 4 - Number 5 - 
Number 1 - Number 2 - Number 3 - Number 4 - Number 5 -
Number 1 - Number 2 - Number 3 - Number 4 - Number 5 -
5 - 4 - 3 - 2 - 1 - 0 -
1, 10 / 1, 11 / 1, 12 / 1, 13 / 1, 14 / 1, 15 /
2, 10 / 2, 11 / 2, 12 / 2, 13 / 2, 14 / 2, 15 /
3, 10 / 3, 11 / 3, 12 / 3, 13 / 3, 14 / 3, 15 /
4, 10 / 4, 11 / 4, 12 / 4, 13 / 4, 14 / 4, 15 /
5, 10 / 5, 11 / 5, 12 / 5, 13 / 5, 14 / 5, 15 /
*/
```

Jai knows the concept of a **range** like 1..100, which means all successive integers starting from 1 to 100 included.
In general `for start..end { }`; for an exclusive range write end–1. start and end can be variables, expressions, even procedure calls.
If there is no name for the loop variable, Jai has its own implicit iteration variable called `it`, see line (1). In line (2) we give the loop variable a name like `number`. This is the same as naming the condition variable in a while. 
In that case `it` is no longer defined. 

(1) and (2) are on-line for-loops. (3) shows that we need { } to write a for with a code block; end here is a variable.  
In line (4) we see a reversed for-loop indicated with **<**. Note that you still have to write the range as end..start, or put it in another way, as : `for < i: max..0 { ... }`.

Like with while we can nest for-loops, as shown in line (5)

**Exercises**
1) Try out that a for-loop like:
		for < x..y { } 
where x < y doesn't run at all (see exercises/15.1_reversed_for.jai).

2) Print the squares of all integers from 10 to 20 (see  15.2_squares.jai) with and without a loop counter.

3) Write a program that produces a typical FizzBuzz output: see [Explanation of FizzBuzz](https://imranontech.com/2007/01/24/using-fizzbuzz-to-find-developers-who-grok-coding/). Use a for loop, an if-else if statement, and the modulo % operator (see 15.3_fizzbuzz.jai).

## 15.3 Breaking out or continuing a loop
With `while true { }` you can make an infinite loop that could crash your machine. In this as well as in normal while or for-loops you should be able to:

i) break out of a loop at a certain condition (that is: leave the loop and continue with the statement next after the loop): use the **break** keyword.

ii) break out of the current loop iteration at a certain condition, and continue with the next loop iteration: use the **continue** keyword.

Here are examples of these situations:
See 15.4_break.jai:

```c++
#import "Basic";

main :: () {
    for i: 0..5 {  // This loop prints out 0, 1, 2 then breaks out of the loop
        if i == 3 break; // (1)
        print("%, ", i);
    }
    print("\n");  // => 0, 1, 2,

  // equivalent while-loop:
    i := 0;
    while i <= 5 {  // (2) This loop prints out 0, 1, 2 then breaks out of the loop
        if i == 3 break; // 
        print("%, ", i);
        i += 1;
    }
    print("\n"); // => 0, 1, 2,

    for i: 0..5 {    // (3)
        for j: 0..5 {
            if i == 3 break i; // breaks out of the outer loop for i:
            print("(%, %) - ", i, j);
        }
    }
    print("\n");

    x := 0;
  	while cond := x < 6 {   // (4)
        defer x += 1;
        if x == 3 break cond;
        print("%, ", x);
  	} // => 0, 1, 2,
}

/*
0, 1, 2,
0, 1, 2,
(0, 0) - (0, 1) - (0, 2) - (0, 3) - (0, 4) - (0, 5) - 
(1, 0) - (1, 1) - (1, 2) - (1, 3) - (1, 4) - (1, 5) - 
(2, 0) - (2, 1) - (2, 2) - (2, 3) - (2, 4) - (2, 5) - 
0, 1, 2,
*/
```
In line (1), we break out of the for loop when i gets the value 3. Note that the variable i is known only inside the for loop.  
In line (2), we see the same logic written as a while loop. The nested loop in line (3) does a `break i`, it stops looping when i becomes 3.
In line (4) we see how you can break out of a while-loop using the condition variable.
So normally a `break` terminates the current innermost loop immediately, but a `break var` breaks out of the loop where `var` is the iteration or condition variable. 

See 15.5_continue.jai:

```c++
#import "Basic";

main :: () {
    for i: 0..5 {  // This for loop prints out 0, 1, 2, 4, 5
        if i == 3 continue;  // (1)
        print("%, ", i);
    }
    print("\n");

    x := 0;
  	while cond := x < 6 {   // (2)
        defer x += 1;
        if x == 3 continue cond;
        print("%, ", x);
  	}

    for i: 0..5 {  
        for j: 0..5 {
            if i == 3 continue i; // (3) continues with the outer loop for 4 and 5
            print("(%, %) - ", i, j);
         }
    }
    print("\n");

    for i: 1..4 {     // (4)
        if i == 2  continue;
        for j: 11..14 {
            print("(%, %) - ", i, j);
            // Stop on unlucky number 13.
            if (i == 3) && (j == 13)  break i;  // Break from the outer loop, so we are done, last printing 13, 3.
        }
    }
}

/*
0, 1, 2, 4, 5,
0, 1, 2, 4, 5,
(0, 0) - (0, 1) - (0, 2) - (0, 3) - (0, 4) - (0, 5) - 
(1, 0) - (1, 1) - (1, 2) - (1, 3) - (1, 4) - (1, 5) - 
(2, 0) - (2, 1) - (2, 2) - (2, 3) - (2, 4) - (2, 5) - 
(4, 0) - (4, 1) - (4, 2) - (4, 3) - (4, 4) - (4, 5) - 
(5, 0) - (5, 1) - (5, 2) - (5, 3) - (5, 4) - (5, 5) -
(1, 11) - (1, 12) - (1, 13) - (1, 14) - 
(3, 11) - (3, 12) - (3, 13) -
*/

```

`continue` starts the enclosing loop again with the next iteration value.
In line (1) we see that the value 3 is not printed out because of the continue.
In line (2) we see the exact same effect in a while-loop, using the condition name `cond` used as continue `cond`.
Line (3) shows that the inner loop is not processed for i == 3, but `continue i` continues with the next value of i being 4.   
For both break and continue we see that the iteration variable can be used as a label here.

Line (4) and following show a combined use of break and continue. A lot of flexibility is possible, but a bug is easily created, so check thoroughly!

## 15.4 Looping over an enum's values:
See _15.6_for_enum.jai_:

```c++
#import "Basic";

Direction :: enum {
    EAST;  
    NORTH; 
    WEST;  
    SOUTH; 
}

main :: () {
    print("% contains the following values:\n", Direction);
    for enum_values_as_s64(Direction) {         // (1)
        print("\t%: %\n", it, cast(Direction) it);
    }
}
/*
Direction contains the following values:
        0: EAST
        1: NORTH
        2: WEST
        3: SOUTH
*/
```

In addition to the enum methods we discovered in § 13.6 that give us the range and the member names, we can also user a for-loop shown in line (1) to get the enum's values with the `enum_values_as_s64` proc. 