Given two files:

- **add.c**

```c
int add(int a, int b) {
    return a + b;
}
```

- **main.c**

```c
#include <stdio.h>

extern int add(int a, int b);

int main()
{
    printf("Add 3 + 4 = %d", add(3, 4));
    return 0;
}
```

## Compile

```bash
$ clang add.c main.c -o main
```

or

```bash
$ clang -c add.c -o add.o
$ clang add.o main.c -o main
```