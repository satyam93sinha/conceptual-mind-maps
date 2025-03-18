# Aditya Verma's Recursion Playlist Mind Map

## 1. **Introduction to Recursion**
   - **What is Recursion?**
     - Function calling itself.
     - Base Condition: Stopping condition for recursion.
     - Recursive Hypothesis: Assume the function works for smaller inputs.
   - **Recursive Tree Visualization**
     - Visual representation of recursive calls.
     - Helps in understanding time and space complexity.
   - **Stack Space in Recursion**
     - Each recursive call uses stack memory.
   - **Tail Recursion**
     - Recursive call is the last operation in the function.

---

## 2. **Recursion Patterns and Techniques**
   - **Input-Output Method**
     - **Concept**:
       - Break the problem into smaller subproblems.
       - Modify the input and pass it to the next recursive call.
     - **Examples**:
       - Print all subsets of a string.
       - Print all permutations of a string.
     - **Sample Code**:
       ```python
       def subsets(input, output):
           if len(input) == 0:
               print(output)
               return
           # Exclude the current character
           subsets(input[1:], output)
           # Include the current character
           subsets(input[1:], output + input[0])
       subsets("abc", "")
       ```
     - **Diagram**:
       ```
       Input: "abc", Output: ""
       - Exclude 'a' → Input: "bc", Output: ""
       - Include 'a' → Input: "bc", Output: "a"
       ```

   - **Choice Diagram**
     - **Concept**:
       - At each step, make a choice (include/exclude).
       - Represented as a binary tree.
     - **Examples**:
       - Subset generation.
       - Permutation generation.
     - **Sample Code**:
       ```python
       def permutations(input, output):
           if len(input) == 0:
               print(output)
               return
           for i in range(len(input)):
               # Choose one character and recurse
               permutations(input[:i] + input[i+1:], output + input[i])
       permutations("abc", "")
       ```
     - **Diagram**:
       ```
       Input: "abc", Output: ""
       - Choose 'a' → Input: "bc", Output: "a"
       - Choose 'b' → Input: "ac", Output: "b"
       - Choose 'c' → Input: "ab", Output: "c"
       ```

   - **Backtracking**
     - **Concept**:
       - Explore all possible solutions and backtrack if a solution is invalid.
     - **Examples**:
       - Rat in a Maze.
       - N-Queens Problem.
     - **Sample Code**:
       ```python
       def solve_maze(maze, x, y, sol):
           if x == len(maze)-1 and y == len(maze[0])-1:
               sol[x][y] = 1
               return True
           if is_safe(maze, x, y):
               sol[x][y] = 1
               if solve_maze(maze, x+1, y, sol):
                   return True
               if solve_maze(maze, x, y+1, sol):
                   return True
               sol[x][y] = 0  # Backtrack
               return False
           return False
       ```

---

## 3. **Basic Recursion Problems**
   - **Print 1 to N using Recursion**
     ```python
     def print_1_to_n(n):
         if n == 0:
             return
         print_1_to_n(n-1)
         print(n)
     ```
   - **Print N to 1 using Recursion**
     ```python
     def print_n_to_1(n):
         if n == 0:
             return
         print(n)
         print_n_to_1(n-1)
     ```
   - **Sum of First N Natural Numbers**
     ```python
     def sum_n(n):
         if n == 1:
             return 1
         return n + sum_n(n-1)
     ```
   - **Factorial of a Number**
     ```python
     def factorial(n):
         if n == 1:
             return 1
         return n * factorial(n-1)
     ```
   - **Fibonacci Series**
     ```python
     def fibonacci(n):
         if n <= 1:
             return n
         return fibonacci(n-1) + fibonacci(n-2)
     ```

---

## 4. **Advanced Recursion Problems**
   - **Generate All Balanced Parentheses**
     ```python
     def generate_parentheses(n, open, close, output):
         if len(output) == 2*n:
             print(output)
             return
         if open < n:
             generate_parentheses(n, open+1, close, output + "(")
         if close < open:
             generate_parentheses(n, open, close+1, output + ")")
     ```
   - **Josephus Problem**
     ```python
     def josephus(n, k):
         if n == 1:
             return 0
         return (josephus(n-1, k) + k) % n
     ```
   - **Word Break Problem**
     ```python
     def word_break(s, word_dict):
         if not s:
             return True
         for i in range(1, len(s)+1):
             if s[:i] in word_dict and word_break(s[i:], word_dict):
                 return True
         return False
     ```

---

## 5. **Recursion with Dynamic Programming**
   - **Memoization**
     - Store results of subproblems to avoid redundant calculations.
     ```python
     def fibonacci(n, memo={}):
         if n in memo:
             return memo[n]
         if n <= 1:
             return n
         memo[n] = fibonacci(n-1, memo) + fibonacci(n-2, memo)
         return memo[n]
     ```
   - **Tabulation**
     - Build a table iteratively to store results.
     ```python
     def fibonacci(n):
         dp = [0, 1]
         for i in range(2, n+1):
             dp.append(dp[i-1] + dp[i-2])
         return dp[n]
     ```

---

## 6. **Recursion Tree Visualization**
   - **Example: Fibonacci**
     ```
     fib(4)
     /     \
   fib(3)  fib(2)
   /   \    /   \
 fib(2) fib(1) fib(1) fib(0)
 /   \
fib(1) fib(0)
     ```

---

## 7. **Resources**
   - [Aditya Verma's Recursion Playlist](https://www.youtube.com/playlist?list=PL_z_8CaSLPWeT1ffjiImo0sYTcnLzo-wY)
   - Practice Problems: LeetCode, Codeforces, HackerRank
   - Recommended Books:
     - "Introduction to Algorithms" by Cormen
     - "The Algorithm Design Manual" by Skiena

---