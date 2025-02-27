# get_next_line

![Moulinette](moulinette.png)

## Overview
`get_next_line` is a function that reads a file line by line from a given file descriptor. It is designed to handle both standard input and files efficiently, returning each line separately.

## Features
- Reads a file descriptor one line at a time.
- Handles multiple file descriptors simultaneously.
- Efficient memory management using static variables and buffers.
- Works with any valid file descriptor (including stdin, regular files, and pipes).

## Usage
### Function Prototype
```c
char *get_next_line(int fd);
```
### Compilation
To compile get_next_line with a test file:
```sh
gcc -Wall -Wextra -Werror get_next_line.c get_next_line_utils.c main.c -o gnl
```

### Example
```c
#include <fcntl.h>
#include <stdio.h>
#include "get_next_line.h"

int main(void)
{
    int fd = open("test.txt", O_RDONLY);
    char *line;
    
    while ((line = get_next_line(fd)))
    {
        printf("%s", line);
        free(line);
    }
    close(fd);
    return 0;
}
```

## Implementation Details
- Uses a buffer (defined by `BUFFER_SIZE`) to read from the file descriptor.
- Keeps track of leftover data between function calls to optimize performance.
- Properly frees memory to avoid leaks.

## Requirements
- Must compile with `gcc` and `-Wall -Wextra -Werror` flags.
- Must not use standard library functions like `malloc`, `free`, `read`, and `write` except where allowed.

## Notes
- If `BUFFER_SIZE` is not defined during compilation, behavior may be undefined.
- The function should return `NULL` when the end of the file is reached or in case of an error.
