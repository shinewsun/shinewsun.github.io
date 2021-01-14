---
layout: page
title: Notes on nand2tetris
---

The course can be found at <https://www.nand2tetris.org/>.

Upon completion, I had some notes I wanted to put down.

## The website
In order to get to the Projects, you have to click the word `Projects` on the sidebar. Hovering over that button shows `Project 00` on the side which indicates that other projects should appear below it. But in fact, you should be clicking the word on the sidebar, not the `Project 00`.

Random paragraphs are links to the main page which makes no sense. So don't click anything.

## Project 1
HDL seems at first like a strangely formatted imperative language but it is not. When writing HDL, you are not moving variables around, you are creating wires between components. This will become important in Project 3 and later.

Start each design with a truth table. Ask if there are any ways to AND or OR things together to make gates. Don't underestimate the value of plugging the same wire into both inputs of a gate (e.g. you can fork a wire with `OR(a=a, b=false, out=a2)` or `AND(a=a, b=true, out=a2)`), or plugging `true` or `false` into inputs.

For MUX and DMUX, use AND gates.

For the 16-bit versions of gates, I just copypasted the 1-bit versions 16 times. I'm not sure if there's a better way to do those.

For MUX8WAY and DMUX8WAY, only three components are required, including the corresponding 4WAY gates

## Project 2
~~I built these in Minecraft as a kid.~~

The HalfAdder only takes one XOR and one AND. The FullAdder uses two HalfAdders (go figure).

Inc16 comes after Add16 for a reason. Use `true`.

The ALU builds exactly as on Slide 53. The flags take effect in order, e.g. `zx nx` gives `!0` regardless of input. Don't forget the output flags on Slide 57.

## Project 3
Bit is built almost exactly as on Slide 23, but because of how MUX works, I had to swap the MUX inputs.

Register is just a bunch of Bits with the same load wire. RAM is just a bunch of Registers or smaller RAMs with some clever MUX8WAYing.

If PC receives multiple signals at the same time, it should behave as the code in `PC.hdl`, starting on line 8.

## Project 4
Mult assumes that the inputs are nonnegative and the result won't overflow, so you don't have to check for those.

On Fill, I'm checking for a keypress on every cycle. Remember that `-1` is `11111111 11111111`, all black.

## Project 5
In Memory, your main components should be a RAM16K, a Screen, and a Keyboard. Don't use RAM8K for the screen and don't use Register for the keyboard! Also note that bit 15 (`address[14]`) will DMUX between main RAM and IO memory. In IO memory, bit 14 (`address[13]`) will DMUX between screen and keyboard. Also remember to send the load signal to the correct component.

The CPU builds exactly as in Slide 34. The slides after that give insight into the subsystems. Most of the flags are bits from the instruction, so you kinda have to parse the instruction.

The Computer builds exactly as on Slide 70.

## Project 6
I recommend using strings everywhere. The output `.hack` is *not* a binary file, but rather a file of ascii chars `1` and `0` and newlines.

Also here's all the instructions:
```
"0"      0101010
"1"      0111111
"-1"     0111010
"D"      0001100
"A"      0110000
"M"      1110000
"!D"     0001101
"!A"     0110001
"!M"     1110001
"-D"     0001111
"-A"     0110011
"-M"     1110011
"D+1"    0011111
"A+1"    0110111
"M+1"    1110111
"D-1"    0001110
"A-1"    0110010
"M-1"    1110010
"D+A"    0000010
"D+M"    1000010
"D-A"    0010011
"D-M"    1010011
"A-D"    0000111
"M-D"    1000111
"D&A"    0000000
"D&M"    1000000
"D|A"    0010101
"D|M"    1010101
```
```
"M"      001
"D"      010
"MD"     011
"A"      100
"AM"     101
"AD"     110
"AMD"    111
```
```
"JGT"    001
"JEQ"    010
"JGE"    011
"JLT"    100
"JNE"    101
"JLE"    110
"JMP"    111
```

## Project 7
If you skimmed instead of reading the slides closely (like I did), `THIS` stores `this` when calling non-static methods and `THAT` is used for the base index of arrays. It's on Slide 68.

The locations `local`, `argument`, `this`, and `that` all work the same, they just use different addresses `LCL`, `ARG`, `THIS`, and `THAT` respectively.

The way `static` works is, the assembler assigns a location between `RAM[16]` and `RAM[255]` (inclusive) for each named variable to go into. All the translator (which you're writing) does is map `static n` to `RAM[n + 16]`. `temp` works the same way, except your locations are between `RAM[5]` and `RAM[12]` (inclusive). Note that programs don't use `temp` so it's a safe location to put stuff needed for running assembly.

The way `pointer` works is, if you set `pointer 0` to a location then you make `THIS` point to that location. For example, before you call a class method, you need to set `pointer 0` to the address of the class.

## Project 8
> Note that a VM program may span either a single `.vm` file or a directory containing one or more `.vm` files. In the former case, the translator creates an output file named `fileName.asm`, which is stored in the same directory of the input file. In the latter case, the translator creates a single output file named `dirName.asm`, which is stored in the same directory. This single file contains the assembly code resulting from translating all the `.vm` files in the input directory.

In `function myFunc n`, the `n` is *not* the number of arguments. Instead, it's the number of local variables allocated. When processing a `function` direction, all you have to do is add a label and prep the local variables.

In `call myFunc n`, the `n` *is* the number of arguments. When processing a `call` direction, you have to do a bunch of setup. The pseudocode is on Slide 117.

The pseudocode for `return` is on Slide 135. Here's the case that tells you why it's so weird: 0 arguments.

|Pointers| Global Stack |
|:------:|:------------:|
| ARG -> |return address|
|        |   saved LCL  |
|        |   saved ARG  |
|        |  saved THIS  |
|        |  saved THAT  |
| LCL -> | return value |
|  SP -> |              |

- You will need to move `SP` to `ARG + 1` at some point, but `return` doesn't know how many args there are. So if you move `ARG` before you move `SP`, you won't be able to find where `SP` should go.
- You will end up overwriting `return address` with `return value`, but you need the `return address` after this overwrite so you need to copy `return address` into some other location.
- It's natural to try to copy `return address` over `saved LCL` but you would have to move `LCL` first. If you move `LCL` then the best pointer you have for the frame is `SP`. But you will have to move `SP` to `ARG + 1` and you will lose the frame when that happens (you don't know how many args there are). So you would have to save `saved ARG` somewhere first.

So instead it's easier to just save `return address` and write `return value` over it. Then you can move `SP` and then use `LCL` as a "fake `SP`". The last thing `LCL` will do is move itself, so losing the frame at that point is okay since by that time, we've gotten everything we need out of the frame.

To actually save `return address`, use a temp variable. ~~I moved `THIS` and `THAT` and then wrote `return address` over `saved THIS` and write `return value` over `saved THAT`. I would not recommend it.~~

## Project 9
~~I skipped this project.~~

Things to keep in mind about the Jack language:
- Variables
  - Declaring variables is done with `kind type name;` where `kind` is `static` for static variables, `field` for instance variables, and `var` for local variables.
  - You can't declare a local variable anywhere but the top of a subroutine.
  - Types are not checked anywhere. All data is stored as integers. The `char` table is on Slide 75.
  - Whenever you set a variable, you have to `let name = ...;`. You can't just `name = ...;`.
- Numbers
  - Negative numbers are made by pushing the positive number then using a `neg`. This means that even though you can store `-32768`, you can't enter it like that. Instead use `~32767`.
  - There is no `%` operator for modulo.
  - There is no order of operations at all. Neither PEMDAS/BEDMAS nor left-to-right. Use `()`.
- Strings
  - Strings have a `length()` and a `maxLength`. You set the `maxLength` when constructing a String, but you can't access it after that.
  - If you want to store `"` in a string, you have to use `String.doubleQuote()`. You cannot `\"`.
- Subroutines
  - There are three ways to call subroutines:
    - `MyClass.myFunction()`, note that you can't call functions from your own class without adding the class name
    - `myInstance.myMethod()`
    - `myMethod()` will act like `this.myMethod()`
  - In order to call `void` subroutines, you have to use `do myFunction();`. You can also `do` subroutines that return values.
  - You can't `return myFunction();` for some reason.
  - You must always `return`, even in a `void` subroutine.
- Other
  - The language is case-sensitive everywhere.
  - You always need the `{}` after an `if`, `else`, or `while`, even if there's only one statement. You can have empty `{}`.
  - A `class Foo` must go in a file `Foo.jack`. Only one class per file.
  - You must `dispose()` arrays and objects in order to free memory. Always implement a `dispose()` for objects that might get freed, since manually `Memory.deAlloc()`ing can leak if the object contains pointers to arrays or other objects.

## Project 10
You'll be finishing off the compiler in Project 11, and you won't need to generate the XML files there. I would keep the XML generation separate from the parsing. Keep the result of the parse in some format you can easily process. (I use Ruby so each node is an array.)

The text checker doesn't care about whitespace as far as I can tell.

## Project 11
In methods, you have to remember that `argument 0` is always `this`. If you call a method, you have to add one argument for `this`. When calling `function myInstance.myMethod()` you must `pop` myInstance before the other arguments. The last argument goes on the top of the stack.

Objects are stored as pointers. The `constructor`, usually `MyClass.new`, will `return` that pointer. It is up to the compiler (that's us) to generate the `Memory.alloc()` to get the pointer. The pseudocode is on Slide 44.

Remember, you get `field` variables with `this n` and you get `var` variables with `local n`.

A `void` method actually does `return` a 0. A `do` statement must then `pop` that 0.

The way arrays get accessed is kinda weird. In order to read from an array, you want to set `THIS` not to the start of the array, but instead to the location you want to read from. Then you `push that 0`. When writing to an array, you have to be careful about the orders that things happen in. Slide 72 has an example problem that can happen from rebinding `THAT` and Slide 74 has the solution.

## Project 12

I used this batch script to automate compilation. If you save it as `projects\12\run.bat` and call it from `projects\12\` as `run.bat Math` it will save `Math.jack` into the right place and compile the folder.
```batch
@ECHO OFF
COPY %1.jack %1Test
..\..\tools\JackCompiler %1Test
```
This (untested) bash script should do the same thing.
```bash
cp "$1.jack" "$1Test"
../../tools/JackCompiler.sh "$1Test"
```


### Math
Note that `a * 2` is the same as `a + a` which is why `Math.multiply()` only uses addition.

Slide 16 claims that `Math.divide()` only uses addition, which is only kind of true since `2 * q * y` cannot be done with only addition, unless you count `q * y` as addition.

If you implement a `twoToThe[i]` array, recall that `~32767` is `10000000 0000000`.

### Memory
You could take a full course on heap management. Don't worry about fragmentation for this implementation.

### Screen
For pixel drawing, Slide 55 has a typo. You should `let address = 16384 + (32 * y) + (x / 16);` and `let value = Memory.peek(address);`.

When clearing the screen, write `0` to all the locations, don't try to `drawRectangle()` it.

Below is some better testing code. Replace the part between `// sun` and `return;` with
```
do Screen.drawLine(140, 26, 140,   6);
do Screen.drawLine(153, 29, 161,  10);
do Screen.drawLine(164, 36, 178,  22);
do Screen.drawLine(171, 47, 190,  39);
do Screen.drawLine(174, 60, 194,  60);
do Screen.drawLine(171, 73, 190,  81);
do Screen.drawLine(164, 84, 178,  98);
do Screen.drawLine(153, 91, 161, 110);
do Screen.drawLine(140, 94, 140, 114);
do Screen.drawLine(127, 91, 119, 110);
do Screen.drawLine(116, 84, 102,  98);
do Screen.drawLine(109, 73, 90,   81);
do Screen.drawLine(106, 60, 86,   60);
do Screen.drawLine(109, 47, 90,   39);
do Screen.drawLine(116, 36, 102,  22);
do Screen.drawLine(127, 29, 119,  10);
```
This draws some weird angles, whereas the original primarily draws perfect diagonals.

### Output
You have to implement `'A'` in the font. Here's what it looks like:
```
..##....
.####...
##..##..
##..##..
######..
##..##..
##..##..
##..##..
##..##..
........
........
```

`Output.moveCursor(i, j)` will set the cursor at `x = j` and `y = i` which is opposite what it seems like it should be.

The cursor wraps. So if you are at (63, 22) and you write a character, you end up at (0, 0).

Characters are 8 pixels wide but each register in the screen RAM is 16 bits long. Also less significant bits display on the left, e.g.
```
14  =>  .###.... ........
```

### Keyboard
The value in `RAM[24576]` is the correct value for `keyPressed()` to return.

### String
If you want to convert `-32768` from int to string and back, you may have to make it a special case.

### Array
`Array.new()` has to be a `function` not a `constructor` because a `constructor` allocates space for all the fields. An array doesn't have any fields.

### Sys
`Sys.wait()` has to be tuned to your machine.

`Sys.init()` must call `Memory.init()` before anything that uses `new` or `Array`. Calling `Memory.init()` first is a good idea.
