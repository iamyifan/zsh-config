# Shell Notebook

## Announcement

Code in this notebook is tested under the following environment:

- OS: macOS 14.6.1 23G93 arm64
- Kernel: Darwin 23.6.0
- Shell: zsh 5.9
- Terminal: Apple_Terminal

Notice that command behaviours may vary depending on different specifications.

## Table of Content

- [Shebang](#shebang)
- [Variables](#variables)
- [Passing Arguments to Scripts](#passing-arguments-to-scripts)
- [Arrays](#arrays)
- [Arithmetic Operations](#arithmetic-operations)
- [String Operations](#string-operations)

## Shebang

- Format: `#!<path_to_shell>` (e.g., `#!/bin/bash` for Bash or `#!/bin/zsh` for Zsh)
- Position: The **first line** of the script.

## Variables

- Definition: A variable can contain **a number**, **a character** or **a string of characters**.

  ```shell
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

## Arrays

- Format:  `(<element_1> <element_2> <element_3> <etc.>)`

  ```shell
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
  students[2]="Alice"   # Replace Jerry with Alice
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
  students[3]=()	      # Delete Jason
  echo ${students[@]}   # Jeff Alice Jimmy
  echo ${#students[@]}  # 3
  ```


## Arithmetic Operations

- Syntax: `((<expression>))` or `$((<expression>))`

  ```shell
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
  ((h = n--))	# assign n to h first before subtracting 1 from n, 11 = 11--
  echo $h $n      # 11 10
  ((i = ++n))     # add 1 to n before assigning n to i, 11 = ++10
  echo $g $n      # 11 11
  ((j = --n))	# subtract 1 from n before assigning n to i, 10 = --10
  echo $h $n      # 10 10
  ```

- Difference
	
  - `((<expression>))`: Used for arithmetic operations, assignments, and conditional checks.
  - `$((<expression>))`: Used for inline calculations, variable assignments, and output.
  
  ```shell
  compare() {
    # ((<expression>)) will return 0 (false) or 1 (true)
    if (($1 < $2)); then
        echo "$(($1)) is smaller than $(($2))"
    else
        echo "$(($1)) is greater then $(($2))"
    fi
  }
  
  compare 2**3 3**2  # 8 (2^3) is smaller than 9 (3^2)
  ```

## String Operations

- String Length: `${#<string>}`

- **Extracting Substrings**

  -  `${<string>:<start>:<length>}`: Extract a substring of length `<length>` starting after the index `<start>`.

    ```shell
    string="hello world"
    substring=${string:6:5}  # a substring of length 5 starting after the index 6
    echo ${substring}  			 # "world"
    ```

  - `${<string>:<start>>`: Extract a substring starting after the index `<start>` and ending to the end of line.

    ```shell
    string="hello world"
    substring=${string:2}  # a substring starting after the index 2
    echo ${substring}			 # "llo world"
    ```

- **Replacing Substrings**

  - Replacing the **First Occurrence** of a Substring with Replacement

    Format: `${<string>[@]/$<substring_to_replace>/$<replacement_of_substring>}`

    ```shell
    string="To study or not to study, that is the question."
    substring="study"
    replacement="sleep"
    echo ${string[@]/$substring/$replacement}   # "To sleep or not to study, that is the question."
    ```
	  
	- Replacing **All Occurrences** of a Substring with Replacement
	
	  Format: `${<string>[@]//$<substring_to_replace>/$<replacement_of_substring>}`
	
	  ```shell
	  string="To study or not to study, that is the question."
	  substring="study"
	  replacement="be"
	  echo ${string[@]//$substring/$replacement}  # "To be or not to be, that is the question."
	  ```

	- Deleting the **First Occurrence** of a Substring
	
	  Format: `${<string>[@]/$<substring_to_replace>}`
	
	  ```shell
	  string="To study or not to study, that is the question."
	  substring=" study"
	  echo ${string[@]/$substring}  # "To or not to study, that is the question."
	  ```

	- Deleting **All Occurrences** of a Substring
	
	  Format: `${<string>[@]//$<substring_to_replace>}`
	
	  ```shell
	  string="To study or not to study, that is the question."
	  substring=" study"
	  echo ${string[@]//$substring}  # "To or not to, that is the question."
	  ```
	
	- Replacing the Substring if It Is at the **Start of a String**

	  Format: `${<string>[@]/#$<substring_to_replace>/$<replacement_of_substring>}`
	
	  ```shell
	  string="To study or not to study, that is the question."
	  substring="To study"
	  replacement="To play"
	  echo ${string[@]/#$substring/$replacement}  # "To play or not to study, that is the question."
	  ```
	
	- Replacing the Substring if It Is at the **End of a String**
	
	  Format: `${<string>[@]/%$<substring_to_replace>/$<replacement_of_substring>}`
	
	  ```shell
	  string="To study or not to study, that is the question."
	  substring="the question."
	  replacement="not a question."
	  echo ${string[@]/%$substring/$replacement}  # "To study or not to study, that is not a question."
	  ```
	
- Capitalising/Uncapitalising

  - Capitalising/Uncapitalising **All Letters**

    Format: `${<string>:u}` for uppercasing; `${<string>:l}` for lowercasing

    ```shell
    string="Hello World"
    echo ${string:u}  # HELLO WORLD
    echo ${string:l}  # hello world
    ```
  
	- Capitalising the **First Letter of Each Word**
	
	  Format: `${(C)<string>}`
	
	  ```shell
	  string="i love shell"
	  echo ${(C)string}  # I Love Shell
	  ```
	
	  
