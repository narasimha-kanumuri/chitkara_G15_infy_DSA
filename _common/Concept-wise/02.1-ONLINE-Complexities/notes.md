## Time, Space, Auxillary


- [Complexities](#complexities)
  - [Nested](#nested)
  - [Complexities](#complexities-1)
  - [Growth Types](#growth-types)
  - [Assignments](#assignments)
- [BEST / AVG / WORST](#best--avg--worst)
  - [Complexity Examples](#complexity-examples)
  - [Constants in Complexity Equations](#constants-in-complexity-equations)
  - [Ignoring lower order terms in Complexity Equations](#ignoring-lower-order-terms-in-complexity-equations)
  - [Complexity Equations](#complexity-equations)
  - [When not to ignore constants and lower order terms in complexity equations](#when-not-to-ignore-constants-and-lower-order-terms-in-complexity-equations)
  - [When not to take which type of complexity](#when-not-to-take-which-type-of-complexity)
  - [Better Complexity](#better-complexity)
  - [Build Equations (Recursion)](#build-equations-recursion)


1. Speed
2. Memory

## Time

Work done w.r.t n(input size)

```java
int x = 10000000// 1 crore
for(int i = 0; i < x; i++) {
    System.out.println(i);
}>)
```

```java
//n is user input
for(int i = 0; i < n; i++) {
    System.out.println(i);
}>)

//n = 10
n < 1 crore, n > 1 crore, worse than above
n --> infinity
```

```java
for (i = 1 ; i <=n ; i = i + 1) {/*code*/}
for (i = 1 ; i <=n ; i = i + 5) {/*code*/}
for (i = 1 ; i <=n ; i = i * 2) {/*code*/}
for (i = 2 ; i <=n ; i = i * i) {/*code*/}
for (i = 2,k = 1 ; i <=n ; i = i * k, k++) {/*code*/} //Logarithmic Time Complexity O(log n); Constant Space Complexity O(1)
for (i = 2 ; i <=n ; i = i * k, k = k * 2) {/*code*/} //Double Logarithmic Time Complexity O(log log n); Constant Space Complexity O(1)

```

# Complexities

1. Constant Time Complexity O(1); Constant Space Complexity O(1)
2. Linear Time Complexity O(n); Linear Space Complexity O(n)(i++, i = i+5)
3. Logarithmic Time Complexity O(log n); Constant Space Complexity O(1)(i = i*2)
4. Double Logarithmic Time Complexity O(log log n); Constant Space Complexity O(1)(i = i*k, k = k*2)

## Nested

```java
// n ^ 2 - Quadratic
for (i = 0; i < n; i++) {
    for (j = 0; j < n; j++) {
        //code
    }
}
//If n = 1000; loop runs 1000 * 1000 = 10^6 times

// m * n - Quadratic
for (i = 0; i < m; i++) {
    for (j = 0; j < n; j++) {
        //code
    }
}

//Quadratic - n^2 + n
public void (){
    //n ^ 2
    for (i = 0; i < n; i++) {
        for (j = 0; j < n; j++) {
            //code
        }
    }
    //n
    for (k = 0; k < n; k++) {
        //code
    }
}

//cubic - n^3
for (i = 0; i < n; i++) {
    for (j = 0; j < n; j++) {
        for (k = 0; k < n; k++) {
            //code
        }
    }
}

// m* 2n
public void (){
    for (i = 0; i < m; i++) {
        //n
        for (j = 0; j < n; j++) {
            //code
        }
        //n
        for (j = 0; j < n; j++) {
            //code
        }
    }
}

//3m^2 + 5n
public void (){
    //3m^2
    for (i = 0; i < m; i++) {
        for (j = 0; j < m; j++) {
            //code
        }
    }
    for (i = 0; i < m; i++) {
        for (j = 0; j < m; j++) {
            //code
        }
    }
    for (i = 0; i < m; i++) {
        for (j = 0; j < m; j++) {
            //code
        }
    }
    //5n
    for (k = 0; k < n; k++) {
        //code
    }
    for (k = 0; k < n; k++) {
        //code
    }
    for (k = 0; k < n; k++) {
        //code
    }
    for (k = 0; k < n; k++) {
        //code
    }
    for (k = 0; k < n; k++) {
        //code
    }

}
```

## Complexities

1. n ^ 2 Time Complexity O(n^2); Constant Space Complexity O(1)

## Growth Types

1. Constant Growth
2. Linear Growth
3. Quadratic Growth
4. Cubic Growth
5. Exponential Growth
6. Logarithmic Growth
7. Double Logarithmic Growth (log log n)
8. Factorial Growth (n!)(Assignment)


## Assignments

1. cubic - 3m * n^2
2. cubic - m * n * o
3. cubic - n^3 + n^2 + n (ASsignment)
4. cubic - n^3 + n^2 + n + 1 (ASsignment)
5. m ^ 3 + n ^ 3 + o ^ 3 (ASsignment)
6. m ^ 3 + n ^ 3 + 3 m ^ 2 n + 3 m n ^ 2 + o ^ 3 (ASsignment)


# BEST / AVG / WORST

Notations:

1. Omega (Ω) - Best Case
2. Theta (Θ) - Average Case
3. Big O (O) - Worst Case

Example:
Linear search

## Complexity Examples

1. O(1) - Constant Time Complexity
2. O(n) - Linear Time Complexity
3. O(n^2) - Quadratic Time Complexity


## Constants in Complexity Equations

1. 2n + 5 -> O(2n + 5) = O(n)
2. 100n + 1000 -> O(100n + 1000) = O(n)
 **Philosophy** Growth dominates constants. Constants are negligible in the long run.

MAth concept: 2 * infinity + 5 = infinity
n --> infinity

## Ignoring lower order terms in Complexity Equations

1. O(n^2 + n) = O(n^2)
2. O(n^3 + n^2 + 2n) = O(n^3)
3. O(n^3 + n^2 + n + 1) = O(n^3)

## Complexity Equations

1. O(n^2 + n) = O(n^2)
2. O(n^3 + n^2 + n) = O(n^3)
3. O(n^3 + n^2 + n + 1) = O(n^3)
4. O(m^3 + n^3 + o^3) = O(m^3 + n^3 + o^3)

**No constants or lower order terms in complexity equations.**

## When not to ignore constants and lower order terms in complexity equations

** When input is small or real system **

## When not to take which type of complexity

## Better Complexity

## Build Equations (Recursion)

1. Binary Search - T(n) = T(n/2) + O(1)
2. Merge Sort - T(n) = 2T(n/2) + O(n)
3. Fibonacci - T(n) = T(n-1) + T(n-2) + O(1) 
   [T(1) = 1, T(2) = 1]
   [T(3) = T(2) + T(1) + O(1) = 1 + 1 + O(1) = 2 + O(1)]
   [T(5) = T(4) + T(3) + O(1) = 3 + 2 + O(1) = 5 + O(1)]
]
