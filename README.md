- [Shell Notebook](#shell-notebook)
  * [Shebang](#shebang)
  * [Variables](#variables)
  * [Passing Arguments to Scripts](#passing-arguments-to-scripts)
  * [Arrays](#arrays)

# Shell Notebook

## Shebang

- Format: `#!<interpreter_path>` (e.g., `#!bin/bash` or `#!bin/zsh`)
- Position: The first line of the script.
- Functionality: Specify which interpreter to use to run the script.

## Variables

- Definition: A variable can contain a number, a character or a string. For example,

  ```shell
  STUDENT_AGE=18
  StudentGender=F
  studentName="Alice"
  ```

- No space is allowed on either side of `=` when initialising variables.

- Naming Rules: 

  - Variable names are case-sensitive.
  - Start with a letter or underscore.
  - Use only letters, numbers and underscores.
  - Lowercase for local variables, e.g. `file_name="shell_notebook.md"`.
  - Uppercase for environment variables and constants, e.g. `MAX_ITERS=10`.

- Escaping Special Characters: Use `\<special_character>` to escape.

- Encapsulating Variable Names: Use `${variable_name}` to avoid ambiguity. Any space inside double quotes `""` will be preserved. For example,

  ```bash
  price_of_milk=3
  echo "Today's milk price is ${price_of_milk} dollars per bottle."
  ```

- Encapsulating Commands: Use ``` `<command>` ``` or  `$(<command>)` to assign variables with outcomes from commands. or example,

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

- Declaring Arrays: Use `()` to enclose array values separated by spaces, and assign them to a variable. For example,

  ```shell
  null=()
  students=("Jeff" "Jerry" "Jason" "Jimmy")
  ```

- Indexing Elements: Use `${<array>[<index>]}` or `$<array>[<index>]` to access the element under the index. Use `${<array>[@]}` to access all elements. For example:

  ``` bash
  # Array indices start with 1
  echo ${students[1]}   # Jeff
  echo ${students[-1]}  # Jimmy
  echo ${students[@]}   # Jeff Jerry Jason Jimmy
  
  # Modify elements by indices
  students[2]="Alice"
  echo ${studnets[2]}   # Alice
  ```

- Length: Use `${#<array>[@]}` or `$#<array>[@]` to determine the length of an array. For example,

  ```bash
  echo "The total number of students is ${#students[@]}."
  ```

- Deleting Elements: Use `unset '<array>[<index>]'` to delete a specific element or `unset '<array>'` to delete the entire array. After unsetting an element. the length remains the same as the array structure didn't change. For example,

  ```bash
  unset 'students[-1]'  # Delete Jimmy
  echo ${studnets[@]}   # Jeff Alice Jason
  echo ${#students[@]}  # Still 4
  
  students=(${students[@]})  # Re-index the array to update its structure
  echo ${#students[@]}       # Update to 3
  
  unset 'students'      # Delete the entire array
  echo ${#students[@]}  # 0 without re-indexing
  ```

  

