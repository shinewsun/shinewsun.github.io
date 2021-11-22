---
layout: page
title: Notes on my nand2tetris solutions
---

The course can be found at <https://www.nand2tetris.org/>.

This page does not contain full solutions for any of the problems. I don't do that. This page is just some comments, especially where my approach differed from the suggested approach. You *can* find some hints and clarifications <a href="index">here</a>.

## Project 1
For the 16-bit versions of gates, I just copypasted the 1-bit versions 16 times. I'm not sure if there's a better way to do those.

## Project 2
INC16 is just `ADD16(input, 1)`. If you wanted to make a decrementer, you would `ADD16(input, -1)` with the realization that `-1` is `11111111 11111111` and ADD16 returns the result mod 2^16.

## Project 3
Nothing special here.

## Project 4
On Fill, I'm checking for a keypress on every cycle. A real computer would probably handle this with interrupts, but this course doesn't cover interrupts.

## Project 5
Nothing special here.

## Project 6
I am a fool and didn't realize that `.hack` is a text file format. I had the binary output all set up to go and had to replace it with a string format to send my beautiful two-byte instructions to text.

## Project 7
Nothing special here.

## Project 8
I moved `THIS` and `THAT` and then wrote `return address` over `saved THIS` and wrote `return value` over `saved THAT`. I would not recommend it.

The benefits of this approach are that I never need the temp segment. Therefore, with this approach, I can save an entire 8 registers of memory. It's completely pointless. I should have used temp.

## Project 9
I skipped this project and it surprisingly did not come back to bite me.

## Project 10
I use Ruby so the structure is stored in array-likes:
```
['class', [
    ['subroutine', [
        ...
    ]]
]]
```
Originally I was storing them using [Nokogiri](https://nokogiri.org/), but that was complexity I didn't need.

## Project 11
Nothing special here.

## Project 12
In general I tried to avoid using the `Math.multiply` function. Instead I used repeated addition to do bitshifting. If the ALU and languages had bitshifting, these would be a lot faster.

### Memory
The way I handled memory was not the way the slides handled it. For the tops of blocks, they used a pointer followed by a size. I used a size followed by a pointer.

The benefit of my approach is that an allocated block does not need to know where the next free block is, but it does need to know its size. So when we go to allocate a block, we need to hold onto the size but we can treat the pointer to the next block as garbage (after we extract its information for the linked list). This means each of my blocks is one full register smaller than the suggested implementation. Thus my implementation can handle more objects in memory at once.

There aren't really any major downsides to my approach in this system, as the complexity it adds are insignificant compared to the complexity already present. In real-world systems, the fact that the header block is partially destroyed upon allocation may lead to some strange behavior which I cannot forsee.

For freeing blocks, I am adding them to the beginning of the linked list. This of course saves time by not requiring the computer to look for the end of the linkedlist, but it also helps limit fragmentation by giving recently freed blocks more often than blocks that have been free a while. The block which was first freed was of course the block that started as the whole heap. Near the beginning of a program, this is the largest block since only a few chunks have been carved off it as memory is allocated. It tends to stay the largest block since no more chunks are carved off the main heap block unless no other free blocks are large enough to handle the request.

I do not defragment the heap since this is in general not fun. According to its wiki, SYSLINUX minimizes fragmentation by looking through its linkedlist for blocks immediately before and after the freed block. In that way, it guarantees that no more than three blocks can be merged on each deallocation, and no back-checking has to be done.

### Screen
I use a different line-drawing algorithm:

1. Handle the cases for vertical and horizontal lines. (This isn't absolutely necessary but both these cases are common and can be implemented with highly efficient approaches.)

2. Determine which direction, `x` or `y`, is the one with larger absolute delta. For this description we'll assume `x` has the larger absolute delta. For `y`, the process is the same but with `x` and `y` swapped everywhere.

3. Possibly swap `(x1, y1)` with `(x2, y2)` to make `x1 < x2`. This ensures we can iterate from `x1` to `x2` by incrementing and not decrementing.

4. Iterate for `i` from `x1` to `x2`. For each `i`, calculate the approximate position of the point `(i, j)` on the line from `(x1, y1)` to `(x2, y2)`. This is done using similar triangles. I also use a function that does rounded division instead of floored:
```java
function int divRound(int a, int b) {
    var int q, r;
    let q = a / b;
    let r = a - (q * b);
    if (r + r < b) {
        return q;
    } else {
        return q + 1;
    }
}
```

5. Draw the point at `(i, j)`

The result is the same as [Bresenham's line algorithm](https://en.wikipedia.org/wiki/Bresenham%27s_line_algorithm) but using more division since our OS does not handle floats. In particular, since division is handled in software on this device, I try to avoid it whenever possible. This algorithm performs one division for each pixel, which isn't too bad, considering the suggested algorithm also uses one division per pixel but draws more pixels.

In fact, this approach can easily be modified to act like [Xiaolin Wu's line algorithm](https://en.wikipedia.org/wiki/Xiaolin_Wu%27s_line_algorithm) which produces antialiased lines! (Floats are needed, but they're not terribly difficult to implement in the OS if you know how they work.)

## Overall thoughts
The course content was quite compact and the order made a lot of sense. Some of the presentations were not entirely to my liking, but I also didn't get the chance to watch any of the presentations in video form.

None of the projects were extremely difficult, but they weren't all easy. A few required me to take breaks and return later. I felt like I always had the tools necessary to complete the assignments up to the specification. I never had to do outside research to learn anything, which is nice since I believe a course like this should not require outside research.

I was slightly disappointed that the course did not go into more complex topics such as virtual addressing and protection rings, but I understand that the pacing of the course was already quite quick and if one unit was to be covered per week, then the course would already take roughly a semester once holidays and exams are factored in.
