# Shell Notebook

## Table of Content

- [Shebang](#shebang)
- [Variables](#variables)
- [Passing Arguments to Scripts](#passing-arguments-to-scripts)
- [Arrays](#arrays)
- [Arithmetic Operations](#arithmetic-operations)

## Shebang

- Format: `#!<path_to_shell>` (e.g., `#!/bin/bash` for Bash or `#!/bin/zsh` for Zsh)
- Position: The **first line**  of the script.
- Functionality: Specify which interpreter to use to run the script.

## Variables

- Definition: A variable can contain **a number**, **a character** or **a string of characters**. For example,

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

- **Parameter Expansion**: Use `${variable_name}` to manipulate the value of variables. For example,

  ```bash
  price_of_milk=3
  echo "Today's milk price is ${price_of_milk} dollars per bottle."
  ```

- **Command Substitution**: Use ``` `<command>` ``` or  `$(<command>)` to capture the output of a command as part of another command. For example,

  ```  bash
  current_path=`pwd`
  file_with_timestamp="file_$(date +%Y-%m-%d).txt"
  ```

## Passing Arguments to Scripts

- Passing Arguments: Append arguments sequentially after the script name. For example:

  ```bash
  source ./student.sh Bob 20 Male
  ```

- Accessing Arguments in the Script:

  - `$0`: Script name.
  - `$1`, `$2`, `$3`, etc.: The first, second, third, etc. arguments passed to the script.
  - `$@`: All the arguments passed to the script.
  - `$#`: The number of arguments passed to the script.

## Arrays

- Declaring Arrays: Use `()` to enclose array values **separated by spaces**, and assign them to a variable. For example,

  ```shell
  null=()
  students=("Jeff" "Jerry" "Jason" "Jimmy")
  ```

- Indexing Elements: Use `${<array>[<index>]}` or `$<array>[<index>]` to access the  **indexed element**. Use `${<array>[@]}` to access **all elements**. For example:

  ``` bash
  # Array indices start with 1
  echo ${students[1]}   # Jeff
  echo ${students[-1]}  # Jimmy
  echo ${students[@]}   # Jeff Jerry Jason Jimmy
  
  # Modify elements by indices
  students[2]="Alice"
  echo ${studnets[2]}   # Alice
  ```

- Length: Use `${#<array>[@]}` or `$#<array>[@]` to access the **length** of an array. The length doesn't necessarily represent the number of elements inside the array. Instead, it indicates the **number of indices** assigned to the array during its initialisation. For example,

  ```bash
  echo "The total number of students is ${#students[@]}."
  ```

- Deleting Elements: Use `unset '<array>[<index>]'` to delete an indexed element or `unset '<array>'` to delete the entire array. After unsetting an element, the length doesn't change as its index is not deleted, (i.e., the array structure doesn't change). For example,

  ```bash
  unset 'students[-1]'  	 	 # Delete Jimmy
  echo ${studnets[@]}     	 # Jeff Alice Jason
  echo ${#students[@]}  		 # Still 4
  
  students=(${students[@]})  # Re-index the array to update its structure
  echo ${#students[@]}       # Update to 3
  
  unset 'students'           # Delete the entire array
  echo ${#students[@]}       # 0 (no need of re-indexing)
  ```


## Arithmetic Operations

Syntax: `((<expression>))` or `$((<expression>))`

```shell
n=10
((a = n + 1))   # a=$((n + 1))
echo $a         # 11
((b = n - 1))   # b=$((n - 1))
echo $b         # 9
((c = n * 3))   # c=$((n * 3))
echo $a         # 30
((d = n / 3))   # d=$((n / 3)), integer division
echo $d         # 3, quotient
((e = n % 3))   # e=$((n % 1)), modulo
echo $e         # 1, remainder
((f = n ** 2))  # f=$((n ** 2))
echo $f					# 100
((g = n++))     # assign n to g before adding 1 to n
echo $g $n      # 10 11
((h = n--))			# assign n to h first before subtracting 1 from n
echo $h $n      # 11 10
((i = ++n))     # add 1 to n before assigning n to i
echo $g $n      # 11 11
((j = --n))			# subtract 1 from n before assigning n to i
echo $h $n      # 10 10
```

`((<expression>))`: Used for arithmetic operations, assignments, and conditional checks. For example,

```shell
#!/bin/zsh

# ((<expression>)) will return 0 (false) or 1 (true)
if (($1 < $2)); then
	echo "$1 is smaller than $2"
else
	echo "$1 is greater then $2"
fi
```

`$((<expression>))`: Used for inline calculations, variable assignments, and output. For example,

```shell
#!/bin/zsh

echo "The sum of $1 and $2 is $(($1 + $2))"
```
