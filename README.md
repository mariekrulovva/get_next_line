# get_next_line
# Get Next Line (GNL)


The aim of this project is to make you code a function that **returns a line
ending with a newline, read from a file descriptor.**

This project will not only allow you to add a very convenient function to your collection,
but it will also allow you to learn a highly interesting new concept in C programming:
**"static variables"**
____
### Function Prototype
```
char *get_next_line(int fd);
```
____
**GNL Mandatory Part**

Function name | get_next_line
--- | ---
Prototype | char *get_next_line(int fd);
Turn in files | get_next_line.c, get_next_line_utils.c, get_next_line.h
Parameters | fd: The file descriptor to read from
Return value | Read line: correct behavior. NULL: there is nothing else to read, or an error occurred
External functs. | read, malloc, free
Description  | Write a function that returns a line read from a file descriptor

• Repeated calls (e.g., using a loop) to your get_next_line() function should let
you read the text file pointed to by the file descriptor, one line at a time.
• Your function should return the line that was read.
• If there is nothing else to read or if an error occurred, it should return NULL.
• Make sure that your function works as expected both when reading a file and when
reading from the standard input.
• Please note that the returned line should include the terminating \n character,
except if the end of file was reached and does not end with a \n character.
• Your header file get_next_line.h must at least contain the prototype of the
get_next_line() function.
• Add all the helper functions you need in the get_next_line_utils.c file.

ℹ️ A good start would be to know what a static variable is.

• Because you will have to read files in get_next_line(), add this option to your compiler call: -D BUFFER_SIZE=n
It will define the buffer size for read().
The buffer size value will be modified by your peer-evaluators and the Moulinette
in order to test your code.
• You will compile your code as follows (a buffer size of 42 is used as an example):
cc -Wall -Wextra -Werror -D BUFFER_SIZE=42 <files>.c
• We consider that get_next_line() has an undefined behavior if the file pointed to
by the file descriptor changed since the last call whereas read() didn’t reach the
end of file.
• We also consider that get_next_line() has an undefined behavior when reading
a binary file. However, you can implement a logical way to handle this behavior if
you want to.

💡 Does your function still work if the BUFFER_SIZE value is 9999? If
it is 1? 10000000? Do you know why?    
ℹ️ Try to read as little as possible each time get_next_line() is
called. If you encounter a new line, you have to return the current
line. Don’t read the whole file and then process each line.

Forbidden
• You are not allowed to use your libft in this project.
• lseek() is forbidden.
• Global variables are forbidden.
____
**GNL Bonus Part**

This project is straightforward and doesn’t allow complex bonuses. However, we trust
your creativity. If you completed the mandatory part, give a try to this bonus part.
Here are the bonus part requirements:
• Develop get_next_line() using only one static variable.
• Your get_next_line() can manage multiple file descriptors at the same time.
For example, if you can read from the file descriptors 3, 4 and 5, you should be
able to read from a different fd per call without losing the reading thread of each
file descriptor or returning a line from another fd.
It means that you should be able to call get_next_line() to read from fd 3, then
fd 4, then 5, then once again 3, once again 4, and so forth.

Append the _bonus.[c\h] suffix to the bonus part files.
It means that, in addition to the mandatory part files, you will turn in the 3 following
files:
• get_next_line_bonus.c
• get_next_line_bonus.h
• get_next_line_utils_bonus.c
____
**GNL with files**
```bash
gcc tests/main.c -Wall -Wextra -Werror -D BUFFER_SIZE=42 get_next_line.c get_next_line_utils.c

./a.out tests/files/part1_test01_with_lines
```

**GNL with standard input (stdin)**
```bash
gcc tests/main_stdin.c -Wall -Wextra -Werror -D BUFFER_SIZE=32 get_next_line.c get_next_line_utils.c

./a.out
```

**GNL Bonus Part**
```bash
gcc tests/main_bonus.c -g -Wall -Wextra -Werror -D BUFFER_SIZE=1 get_next_line_bonus.c get_next_line_utils_bonus.c

./a.out
```

## Points to understand in GNL

According to our GNL subject *Calling your function get_next_line in a loop will then allow you to read the text
available on a file descriptor one line at a time until the EOF*

Call GNL from the main

```c
int main(int argc, char **argv)
{
	int fd, ret, line_count;
	char *line;

	line_count = 1;
	ret = 0;
	line = NULL;
	if (argc == 2)
	{
		fd = open(argv[1], O_RDONLY);
		while ((ret = get_next_line(fd, &line)) > 0)
		{
			printf(" \n [ Return: %d ] | A line has been read #%d => %s\n", ret, line_count, line);
			line_count++;
			free(line);
		}
		printf(" \n [ Return: %d ] A line has been read #%d: %s\n", ret, line_count++, line);
		printf("\n");
		if (ret == -1)
			printf("-----------\n An error happened\n");
		else if (ret == 0)
		{
			printf("-----------\n EOF has been reached\n");
			free(line);
		}
		close(fd);
	}
```

This next line will return an integer that will be used as a parameter for the **get_next_line** function.
```c
fd = open(argv[1], O_RDONLY);
```
**get_next_line** function will return an **integer** that will be taken to evaluate all the lines until the file ends.

### Return value
 | Value | Description         |
 |-----------|----------------------|
 |  1| A line has been read |
 |  0| EOF has been reached |
 |  -1| An error happened |

### READ() function

```c
#include <unistd.h>
ssize_t read(int fd, void *buf, size_t count);
```
**Input parameters:**
- **int fd** file descriptor is an integer and not a file pointer. The file descriptor for stdin is 0
- **void buf** pointer to buffer to store characters read by the read function
- **size_t count** maximum number of characters to read

**:traffic_light: Note.:** a character is equivalent to a 1 byte and a byte is made up of 8 bits therefore a character is made up of 8 bits (1 byte)

We can do something like this to read 20 bytes or 20 characters:
```c
char buffer[20];
read(fd, buffer, 20);
```

**:traffic_light: Note.:** remember read() doesn't add '\0' to terminate to make it string (just gives raw buffer).


### Functions Used

**External Functions**

  | Function | Description         |
 |-----------|----------------------|
 |  read() | A line has been read |
 |  malloc() | EOF has been reached |
 |  free() | An error happened |

**Utility Functions**

 | Function | Description         |
 |-----------|----------------------|
 |  ft_strnew() | Uses size as the size for a new string product of memalloc, this will return the new string, after assigning '' 0 "as elements of the string. |
 |  ft_strchr() | The  strchr() function returns a pointer to the first occurrence of the character c in the string s. |
 |  ft_strjoin() | Allocates with malloc and returns a new string, which is the result of the concatenation of 's1' and 's2'. |
 |  ft_memdel() | A line has been read |
 |  ft_strdup() | Returns a pointer to a  new  string  which  is  a duplicate  of the string s. |
 |  ft_substr | Allocates with malloc and returns a substring from the string 's'. |
 |  ft_strlen |  Calculates the length of the string s, excluding the terminating null byte ('\0') |


## Testers

### 42TESTERS-GNL
**Author:** Mazoise

 :point_right: Locate in the **42-silicon-valley-gnl** folder

```bash
git clone https://github.com/Mazoise/42TESTERS-GNL.git
cd 42TESTERS-GNL
./all_tests.sh
./all_tests_with_bonus.sh
```

### VALGRIND

**Installation**
```bash
sudo apt-get update -y
sudo apt-get install -y valgrind
```
**How to Use?**
```bash
gcc tests/main.c -g -Wall -Wextra -Werror -D BUFFER_SIZE=32 get_next_line.c get_next_line_utils.c

valgrind --tool=memcheck --leak-check=yes --show-reachable=yes --num-callers=20 --track-fds=yes ./a.out tests/files/part1_test01_with_lines
```
![alt text](img/valgrind_output.png)

**:information_source:** [ know more about valgrind ](https://valgrind.org/docs/manual/quick-start.html)

### FSANITIZE

```bash
gcc tests/main.c -g3 -fsanitize=address -Wall -Wextra -Werror -D BUFFER_SIZE=32 get_next_line.c get_next_line_utils.c -I 

./a.out tests/files/part1_test01_with_lines
```
![alt text](img/fsanitize_output.png)

## Other testers

[ GNL_lover ](https://github.com/charMstr/GNL_lover.git)   from *charMstr*

[ gnlkiller ](https://github.com/DontBreakAlex/gnlkiller.git)   from  *DontBreakAlex*

[ gnlkiller2 ](https://github.com/Sherchryst/gnlkiller.git)   from *Sherchryst*

## Debug with lldb and GUI
```bash
gcc -g tests/main.c -Wall -Wextra -Werror -D BUFFER_SIZE=1 get_next_line.c get_next_line_utils.c
lldb a.out
b get_next_line
run tests/files/part1_test01_with_lines
gui
```
**:flashlight: keys to move**

- **tab** : change window
- **s** : next line
- **directional key** : variables navigation
- **h** : help

![alt text](img/lldb_gui.png)


## Norminette

Use and install this repository: 
[ Norminette ](https://github.com/42sp/norminette-client.git)

**Run**
```bash
norminette *.*
```

## Graded by Moulinette
![alt text](img/graded_by_moulinete.png)

### GNL pdf  new curriculum
[Download from HERE](https://drive.google.com/file/d/1Dg5NWIZD0WmaiNZEp0-DjIkD22Og3e4A/view?usp=sharing)
 
