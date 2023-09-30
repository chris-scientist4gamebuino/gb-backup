
# Memory optimization

[Back to Home](./../../../README.MD) | [Back to Learning for Classic](./README.MD) | [Go to Classic Reference Summary](./../reference/README.MD)

This page details tips and tricks for tracking memory usage in your Gamebuino applications.

## Determining Code Size

If you want to see how much memory your app is using for code space and global variables then you can use the objdump utility provided with the IDE. If you go to the Arduino IDE's File -> Preferences dialog and turn on verbose output for compilation then the output window will display the location of the hex file it makes when it compiles your project, i.e. something like this:

```
C:\arduino-1.0.5\hardware\tools\avr\bin\avr-objcopy -O ihex -R .eeprom C:\Users\username\AppData\Local\Temp\build8635125389519731391.tmp\your_sketch.cpp.elf C:\Users\username\AppData\Local\Temp\build8635125389519731391.tmp\your_sketch.cpp.hex
```

The hex file is what gets uploaded to your Gambuino but the elf file contains information about the code that created it, and this info can be dump to a list file by running avr-objdump like this:

```
C:\arduino-1.0.5\hardware\tools\avr\bin\avr-objdump -h -S -j .data -j .text C:\Users\username\AppData\Local\Temp\build8635125389519731391.tmp\your_sketch.cpp.elf > dump.lst
```

This will dump a whole bunch of useful info in dump.lst including a disassembly of all routines imported into your project. Of particular interest are the text and data sections listed at the top of the file which show how much memory is being used for global variables and code, respectively:

```
Sections:
Idx Name          Size      VMA       LMA       File off  Algn
  0 .data         0000040c  00800100  000056ac  00005740  2**0
                  CONTENTS, ALLOC, LOAD, DATA
  1 .text         000056ac  00000000  00000000  00000094  2**1
                  CONTENTS, ALLOC, LOAD, READONLY, CODE
  2 .bss          0000016b  0080050c  0080050c  00005b4c  2**0
                  ALLOC
```

This particular example is Rodot's AlienKiller demo, and we can see that there are 1036 (i.e. 0x40c) bytes used for global variables (half of which is the display buffer) and 22188 (i.e. 0x56ac) bytes of code. For comparison the Gamebuino has 2KB of usable RAM and 32KB for code (less 2KB being used by the boot loader). However it's also allocating 363 (0x16b) bytes for the .bss section which is globally allocated variables that aren't initialized to a particular value.

Another useful tool is nm, which can be used to determine the size of symbols in your project. You can use it to determine the size of all functions and variables in your program in order to target large functions for size optimizations. It can be used like this:

```
C:\arduino-1.6.9\hardware\tools\avr\bin\avr-nm --size-sort -C -r "C:\Users\username\AppData\Local\Temp\build8635125389519731391.tmp\your_sketch.ino.elf" > symbol_sizes_output.txt
```

This example actually dumps the result in a file called symbol_sizes_output.txt for convenience, but you don't have to do this. The result is a list of functions and variables sorted by size, which can be very useful.

## Determining Available Memory at Runtime

Looking at the lst file will tell you how much memory is allocated at the start-up, but you may also need to keep track of run-time usage. When a Gamebuino game starts up the global buffer is allocated at the very bottom of SRAM, the heap appears immediately after it and the stack is placed at the very top of RAM. Any calls to malloc() will cause the heap to grow "up", and any data allocated on the stack by calling functions etc will cause the stack to grow "down". If they meet up in the middle your program will either start acting weird or simply crash.

To help prevent this you can use code like this during development to keep track of the heap and stack pointers and if they get too close print an error message or something:

```
void mem_check()
{
  void * heap_ptr = malloc(0);  
  if ((!heap_ptr) || ((int)heap_ptr + 100 >= SP))
  {
     Serial.println("Out of memory!");
     while (1);
  }
  free(heap_ptr);
}
```

Here's an example to demonstrate this, the test() function calls itself recursively, increasing the stack a little each time, until finally we run out of memory:

```
void dump_mem()
{
  void * heap_ptr = malloc(0);
  if (heap_ptr)
    free(heap_ptr);
  Serial.print("heap: ");
  Serial.print((int)heap_ptr);
  Serial.print(",  stack: ");
  Serial.println(SP);
}

void test()
{
  dump_mem();
  mem_check();
  test();
  test();   // call test twice so that the compiler doesn't convert the first one to a tail call
}

void setup() {
  Serial.begin(9600);
  test();
}

void loop() {
}
```

## General information

Saved on September 30, 2023
