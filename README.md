# Shell Notebook

## Announcement

Code in this notebook is tested under the following environment:

- OS: macOS 14.6.1 23G93 arm64
- Kernel: Darwin 23.6.0
- Shell: zsh 5.9
- Terminal: Apple_Terminal

> [!WARNING]
> Notice that command behaviours may vary depending on different specifications.

## Table of Content

- [Shell Notebook](#shell-notebook)
  - [Announcement](#announcement)
  - [Table of Content](#table-of-content)
  - [Shebang](#shebang)
  - [Variables](#variables)
  - [Passing Arguments to Scripts](#passing-arguments-to-scripts)
  - [Arrays](#arrays)
  - [Arithmetic Operations](#arithmetic-operations)
  - [String Operations](#string-operations)
  - [If Statement](#if-statement)
  - [Case Statement](#case-statement)
  - [For Loop Statement](#for-loop-statement)
  - [While Loop Statement](#while-loop-statement)
  - [Break and Continue Statement](#break-and-continue-statement)
  - [To-Do](#to-do)

## Shebang

- Format: `#!<path_to_shell>` (e.g., `#!/bin/bash` for Bash or `#!/bin/zsh` for Zsh)
- Position: The **first line** of the script.

[Back to Top](#shell-notebook)

## Variables

- Definition: A variable can contain **a number**, **a character** or **a string of characters**.

  ```bash
  STUDENT_AGE=18
  StudentGender="F"    # StudentGender=F
  studentName="Alice"  # studentName=Alice
  ```

- **No space** is allowed on either side of `=` when initialising variables.

- **Double quotes** ensure the string is treated as a single, literal unit, especially when it contains spaces or special characters.

- Naming Rules: 

  - Variable names are case-sensitive.
  - Start with a letter or underscore.
  - Use only letters, numbers and underscores.
  - Lowercase for local variables, e.g. `file_name="shell_notebook.md"`.
  - Uppercase for environment variables and constants, e.g. `MAX_ITERS=10`.

- Escaping Special Characters: Use `\<special_character>` to escape special characters.

- **Parameter Expansion**

  - Format:  `${<variable>}`
  - Functionality: Manipulate **variable values**.

    ```bash
    milk_price=3
    echo "Today's milk price is ${milk_price} dollars per bottle."
    ```

- **Command Substitution**

  - Format: ``` `<command>` ``` or  `$(<command>)`
  - Functionality: Capture a **command output** as part of another command.
  
    ```  bash
    current_working_path=`pwd`
    file_with_timestamp="file_$(date +%Y-%m-%d).txt"
    ```

[Back to Top](#shell-notebook)

## Passing Arguments to Scripts

- Format: Append arguments sequentially after the script name. For example:

  ```bash
  ./student.sh Bob 20 Male
  ```

- Accessing Arguments:

  - `$0`: The script name.
  - `$1`, `$2`, `$3`, etc.: The first, second, third, etc. arguments passed to the script.
  - `$@`: All the arguments passed to the script.
  - `$#`: The number of arguments passed to the script.

[Back to Top](#shell-notebook)

## Arrays

- Format:  `(<element_1> <element_2> <element_3> <etc.>)`

  ```bash
  null=()
  students=("Jeff" "Jerry" "Jason" "Jimmy")
  ```

- **Indexing**: `${<array>[<index>]}` or `$<array>[<index>]` for an indexed element; `${<array>[@]}` for all elements

  - Notice that all arrays are **1-indexed**. This behaviour may vary based on different configurations.

  ``` bash
  echo ${students[1]}   # Jeff
  echo ${students[-1]}  # Jimmy
  echo ${students[@]}   # Jeff Jerry Jason Jimmy
  ```

- Modifying

  ```bash
  echo ${students[@]}   # Jeff Jerry Jason Jimmy
  students[2]="Alice"   # replace Jerry with Alice
  echo ${students[@]}   # Jeff Alice Jason Jimmy
  ```

- **Length**: `${#<array>[@]}` or `$#<array>[@]`

  ```bash
  echo ${students[@]}   # Jeff Alice Jason Jimmy
  echo ${#students[@]}  # 4
  ```

- **Deleting**: `<array>[<index>]=()` 

  ```bash
  echo ${students[@]}   # Jeff Alice Jason Jimmy
  students[3]=()	      # delete Jason
  echo ${students[@]}   # Jeff Alice Jimmy
  echo ${#students[@]}  # 3
  ```

[Back to Top](#shell-notebook)

## Arithmetic Operations

- Syntax: `((<expression>))` or `$((<expression>))`

  ```bash
  n=10

  ((a = n + 1))   # a=$((n + 1)), 11 = 10 + 1
  echo $a         # 11
  ((b = n - 1))   # b=$((n - 1)), 9 = 10 - 1
  echo $b         # 9

  ((c = n * 3))   # c=$((n * 3)), 30 = 10 * 3
  echo $a         # 30
  ((d = n / 3))   # d=$((n / 3)), integer division, 3 = 10 / 3
  echo $d         # 3, quotient
  ((e = n % 3))   # e=$((n % 1)), modulo, 1 = 10 % 3
  echo $e         # 1, remainder
  ((f = n ** 2))  # f=$((n ** 2)), 100 = 10 ** 2
  echo $f	        # 100

  ((g = n++))     # assign n to g before adding 1 to n, 10 = 10++
  echo $g $n      # 10 11
  ((h = n--))	    # assign n to h first before subtracting 1 from n, 11 = 11--
  echo $h $n      # 11 10
  ((i = ++n))     # add 1 to n before assigning n to i, 11 = ++10
  echo $g $n      # 11 11
  ((j = --n))	    # subtract 1 from n before assigning n to i, 10 = --10
  echo $h $n      # 10 10
  ```

- Difference
	
  - `((<expression>))`: Used for arithmetic operations, assignments, and conditional checks.
  - `$((<expression>))`: Used for inline calculations, variable assignments, and output.
  
  ```bash
  compare() { 
    if (($1 < $2)); then  # ((<expression>)) will return 0 (false) or 1 (true) 
      echo "$(($1)) is smaller than $(($2))"
    else 
      echo "$(($1)) is greater then $(($2))"
    fi
  }
  
  compare 2**3 3**2  # 8 (2^3) is smaller than 9 (3^2)
  ```

[Back to Top](#shell-notebook)

## String Operations

- String Length: `${#<string>}`

- **Extracting Substrings**

  -  `${<string>:<start>:<length>}`: Extract a substring of length `<length>` starting after the index `<start>`.

    ```bash
    string="hello world"
    substring=${string:6:5}  # a substring of length 5 starting after the index 6
    echo ${substring}   # "world"
    ```

  - `${<string>:<start>>`: Extract a substring starting after the index `<start>` and ending to the end of line.

    ```bash
    string="hello world"
    substring=${string:2}  # a substring starting after the index 2
    echo ${substring}   # "llo world"
    ```

- **Replacing Substrings**

  - Replacing the **First Occurrence** of a Substring with Replacement

    Format: `${<string>[@]/$<substring_to_replace>/$<replacement_of_substring>}`

    ```bash
    string="To study or not to study, that is the question."
    substring="study"
    replacement="sleep"
    echo ${string[@]/$substring/$replacement}   # "To sleep or not to study, that is the question."
    ```
	  
	- Replacing **All Occurrences** of a Substring with Replacement
	
	  Format: `${<string>[@]//$<substring_to_replace>/$<replacement_of_substring>}`
	
	  ```bash
	  string="To study or not to study, that is the question."
	  substring="study"
	  replacement="be"
	  echo ${string[@]//$substring/$replacement}  # "To be or not to be, that is the question."
	  ```

	- Deleting the **First Occurrence** of a Substring
	
	  Format: `${<string>[@]/$<substring_to_replace>}`
	
	  ```bash
	  string="To study or not to study, that is the question."
	  substring=" study"
	  echo ${string[@]/$substring}  # "To or not to study, that is the question."
	  ```

	- Deleting **All Occurrences** of a Substring
	
	  Format: `${<string>[@]//$<substring_to_replace>}`
	
	  ```bash
	  string="To study or not to study, that is the question."
	  substring=" study"
	  echo ${string[@]//$substring}  # "To or not to, that is the question."
	  ```
	
	- Replacing the Substring if It Is at the **Start of a String**

	  Format: `${<string>[@]/#$<substring_to_replace>/$<replacement_of_substring>}`
	
	  ```bash
	  string="To study or not to study, that is the question."
	  substring="To study"
	  replacement="To play"
	  echo ${string[@]/#$substring/$replacement}  # "To play or not to study, that is the question."
	  ```
	
	- Replacing the Substring if It Is at the **End of a String**
	
	  Format: `${<string>[@]/%$<substring_to_replace>/$<replacement_of_substring>}`
	
	  ```bash
	  string="To study or not to study, that is the question."
	  substring="the question."
	  replacement="not a question."
	  echo ${string[@]/%$substring/$replacement}  # "To study or not to study, that is not a question."
	  ```
	
- Capitalising/Uncapitalising

  - Capitalising/Uncapitalising **All Letters**

    Format: `${<string>:u}` for uppercasing; `${<string>:l}` for lowercasing

    ```bash
    string="Hello World"
    echo ${string:u}  # HELLO WORLD
    echo ${string:l}  # hello world
    ```
  
	- Capitalising the **First Letter of Each Word**
	
	  Format: `${(C)<string>}`
	
	  ```bash
	  string="i love shell"
	  echo ${(C)string}  # I Love Shell
	  ```

[Back to Top](#shell-notebook)

## If Statement

- Syntax

  ```bash
  # if
  if [ <if_expr> ]; then 
    <if_code>
  fi
  
  # if-else
  if [ <if_expr> ]; then 
    <if_code>
  else 
    <else_code>  
  fi
  
  # if-elif-else
  if [ <if_expr> ]; then 
    <if_code>
  elif [ <elif_expr> ]; then 
    <elif_code>
  else 
    <else_code>
  fi
  ```

  In each `[ <expr> ]`, `<expr>` must be separated with **one space** on each side.

- Operators (more details on `man test`)

  | File Operator           | Description                                                  |
  | ----------------------- | :----------------------------------------------------------- |
  | `-d <file>`             | True if `<file>` exists and is a directory. |
  | `-e <file>`             | True if `<file>` exists (regardless of type). |
  | `-f <file>`             | True if `<file>` exists and is a regular file. |
  | `-r <file>`             | Ture if `<file>` exists and is readable. |
  | `-s <file>`             | True if `<file>` exists and has a size greater than zero. |
  | `-w <file>`             | True if `<file>` exists and is writable.  True indicates only that the write flag is on.  `<fiile>` is not writable on a read-only file system even if this test indicates true. |
  | `-x <file>`             | True if `<file>` exists and is executable.  True indicates only that the execute flag is on.  If `<file>` is a directory, true indicates that `<file>` can be searched. |
  | `<file_1> -nt <file_2>` | True if `<file_1>` exists and is newer than `<file_2>`.      |
  | `<file_1> -ot <file_2>` | True if `<file_1>` exists and is older than `<file_2>`.      |
  |`<file_1> -ef <file_2>`|True if `<file_1>` and `<file_2>` exist and refer to the same file.|
  
  | String Operator      | Description                                                  |
  | -------------------- | ------------------------------------------------------------ |
  | `-z <str>`           | True if the length of `<str>` is zero.                       |
  | `-n <str>`           | True if the length of `<str>` is nonzero.                    |
  | `<str>`              | True if `<str>` is not the null string.                      |
  | `<str_1> = <str_2>`  | True if `<str_1>` and `<str_2>` are identical.               |
  | `<str_1> != <str_2>` | True if `<str_1>` and `<str_2>` are  not identical.          |
  | `<str_1> < <str_2>`  | True if `<str_1>` comes before `<str_2>` based on the binary value of their characters. |
  | `<str_1> > <str_2>`  | True if `<str_1>` comes after `<str_2>` based on the binary value of their characters. |

  | Integer Operator              | Description                                                  |
  | ----------------------------- | ------------------------------------------------------------ |
  | `<int_1> -eq <int_2>` | True if `<int_1>` and `<int_2>` are algebraically equal. |
  |`<int_1> -ne <int_2>`|True if `<int_1>` and `<int_2>` are not algebraically equal.|
  |`<int_1> -gt <int_2>`|True if `<int_1>` is algebraically greater than `<int_2>`.|
  |`<int_1> -ge <int_2>`|True if `<int_1>` is algebraically greater than or equal to `<int_2>`.|
  |`<int_1> -lt <int_2>`| True if `<int_1>` is algebraically less than `<int_2>`. |
  | `<int_1> -le <int_2>` |True if `<int_1>` is algebraically less than or equal to `<int_2>`.|
  
  | Logical Operator       | Description                                      |
  | ---------------------- | ------------------------------------------------ |
  |`! <expr>`|True if `<expr>` is false.|
  | `<expr_1> -a <expr_2>` | True if both `<expr_1>` and `<expr_2>` are true. |
  |`<expr_1> -o <expr_2>`|True if either `<expr_1>` or `<expr_2>` are true.|
  |`( <expr_1> )`|True if `<expr>` is true. In some cases, parentheses need to be escaped.<br />E.g., `[ \( 1 \lt 2 \) -o \( "a" = "A" \) ]`|


- `test <expr>`  is equivalent to `[ <expr> ]`.

- Each evaluation has an **exit status**:


  - 0 if the expression is evaluated as **true**;

  - 1 if the expression is evaluated as **false** or **missing**;

  - \>1 if an **error** occurred.

- `$?` holds the exit status of the **last command**.

  ```bash
  test "bash" = "zsh"  # compare two strings
  echo $?   # 1 (false)
  
  [-e /bin/zsh ]       # whether zsh exists
  echo $?   # 0 (ture)
  ```

[Back to Top](#shell-notebook)

## Case Statement

- Advantage: When all decision branches depend on a **single variable**, the case statement handles **more efficiently** than the repeated if statement.

- Syntax

  ```bash
  case ${<variable>} in 
    <case_1>) 
      <case_1_code>
      ;; 
    <case_2> | <case_3>) 
      <case_2_code>
      ;;
    *)
      <case_default_code>
      ;;
  esac
  ```
  
  The Double semicolon `;;` will terminate the `case` statement and jump to the end of the code.

  The asterisk symbol `*)` will be used as the **default case**, which will handle all the other cases that are not matched above.


- Example

  ```bash
  #!/bin/zsh
  
  echo "Enter the name of a month:"
  read month  # read a month name from the terminal input
  
  case $month in 
    "February") 
      echo "There are 28/29 days in $month." 
      ;; 
    "April" | "June" | "September" | "November") 
      echo "There are 30 days in $month." 
      ;; 
    "January" | "March" | "May" | "July" | "August" | "October" | "December") 
      echo "There are 31 days in $month." 
      ;; 
    *) 
      echo "Please check if you entered the correct month name." 
      ;;
  esac
  ```

[Back to Top](#shell-notebook)

## For Loop Statement

Syntax

```bash
for <arg> in <array>; do 
  <code>
done
```

Example:

```bash
#!/bin/zsh

students=("Jeff" "Jerry" "Jason" "Jimmy")
# The following loop will print all the student names one by one
for student in ${students[@]}; do 
  echo $student 
done
```

```bash
Jeff
Jerry
Jason
Jimmy
```

[Back to Top](#shell-notebook)

## While Loop Statement

Syntax:

```bash
while [ <condition> ]
do
  <code>
done
```

Example:
  
```bash
#!/bin/zsh

students=("Jeff" "Jerry" "Jason" "Jimmy") 
# While the array contains more than one name
while [ ${#students[@]} -gt 0 ]; do 
  # Print the remaining student names 
  echo ${students[@]} 
  # Delete the last studnet name 
  students[-1]=() 
done
```

```bash
Jeff Jerry Jason Jimmy
Jeff Jerry Jason
Jeff Jerry
Jeff
```

[Back to Top](#shell-notebook)

## Break and Continue Statement

Once `break` is encountered, the loop terminates, and no further iterations are executed

Once `continue` is encountered, the remaining code in that iteration is skipped, but the loop itself continues with the next iterations.

Example:

```bash
#!/bin/zsh

# The following example will print all the even numbers less than 10
for num in {0..100}; do
  if (($num > 10)); then 
    break
  fi
  if (($num % 2 == 0)); then 
    echo $num
    ((num = num + 1))
  else
    ((num = num + 1))
    continue
  fi
done
```

```bash
0
2
4
6
8
10
```

[Back to Top](#shell-notebook)

---

## To-Do

- [ ] TODO