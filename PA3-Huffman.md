# PA3: Huffman Encoding

## Due: by 11:59pm on Wednesday 7/21/2021

## Table of Contents
- [Overview](#Overview)
- [Academic Integrity and Honest Implementations](#Academic-Integrity-and-Honest-Implementations)
- [Retrieving Starter Code](#Retrieving-Starter-Code)
- [Naive ASCII Compression and Decompression](#Naive-ASCII-Compression-and-Decompression)
  - [1. Implement HCTree](#1-Implement-HCTree)
  - [2. Implement Compression](#2-Implement-Compression)
  - [3. Implement Uncompress](#3-Implement-Uncompress)

- [Testing your code and getting help](#Testing-Tips)
- [Getting Help](#Getting-Help)
- [Submission Instructions](#Submission-Instructions)
  - [Submitting Your PA](#Submitting-Your-PA)
  - [Grading](#Grading)

## Overview
In this assignment you will:

* Implement Huffman's algorithm using efficient supporting data structures to support encoding and decoding of files
* Make sure your algorithm runs correctly on test examples using ASCII 

* Complete a worksheet on gradescope like you did for the first two assignments. The same rule applies as before for worksheets.

**_Start early, and submit early and often!_**


## Academic Integrity and Honest Implementations
We will hand inspect all submissions and will use automated tools to look for plagiarism or deception. **Attempting to solve a problem by other than your own means will be treated as an Academic Integrity Violation.** This includes all the issues discussed in the Academic Integrity Agreement, but in addition, it covers deceptive implementations. For example, if you use a library (create a library object and just reroute calls through that library object) rather than writing your own code, that’s seriously not okay and will be treated as dishonest work.
  

## Retrieving Starter Code
The files you retrieve should be the following:
* Makefile
* HCNode.hpp
* HCTree.hpp

There are also two folders containing possible test files for you to test your code.

## Naive ASCII Compression and Decompression
### 1. Implement HCTree
The HCTree implementations will help you create Huffman tree/code for the input files. For now, they will support encoding and decoding with ASCII '1's and '0's only.  

Implementation Checklist:

* Implement the HCTree.hpp methods in a new file HCTree.cpp. You can modify both files in any way you want. 
* The overloaded < operator was provided for you in HCNode.hpp, make sure you read it along with the HCNodePtrComp comparator in HCTree.hpp to understand how does a priority queue storing HCNode pointers determine their priority. (Since the overloaded < operator was defined for you in the starter code, you may just ignore the additional comp() method on the bottom of HCNode.hpp)
* You may modify HCNode.hpp any way you want, but you must implement any additional methods that you added in a new file HCNode.cpp. 
Note that all other relevant files that you created are required for the submission. Also, make sure your makefile can compile all the files you have. 

Implementation Notes:

When implementing Huffman's algorithm, you can use any data structures you want (e.g., a priority queue, etc). You should also use good object-oriented design. For example, since a Huffman code tree will be used by both your compress and uncompress programs,  it makes sense to encapsulate its functionality inside a single class accessible by both programs. With a good design, the main methods in the compress and uncompress programs will be quite simple: they will create objects of other classes and call their methods to do the necessary work.

### 2. Implement Compression
Create compress.cpp (from scratch!) to compress small files in plain ASCII. The compress.cpp should generate a program named compress that can be invoked from the command line as follows

> ./compress infile outfile 

./compress will:

1. Read the contents of the file named (infile).

2. Construct a Huffman code for the contents of that file

3. Use that code to construct a compressed file named  (outfile). 


The challenge of this assignment is to translate the following high-level algorithm into real code:

Implement The Following Control Flow in compress.cpp :

1. Open the input file for reading. (infile is guaranteed to have only ASCII text and to be very small (<1KB))

2. Read bytes from the file. Count the number of occurrences of each byte value. Close the file.

3. Use the byte counts to construct a Huffman coding tree. Each unique byte with a non-zero count will be a leaf node in the Huffman tree.

4. Open the output file for writing.

5. Write enough information (a "file header") to the output file to enable the coding tree to be reconstructed when the file is read by your uncompress program. You should write the header as plain (ASCII) text. See "the file header demystified" and "designing your header" for more details.

6. Open the input file for reading, again. If you instead keep the file open without ever closing it, you will need to clear the EOF flag using the clear() method.

7. Using the Huffman coding tree, translate each byte from the input file into its code, and append these codes as a sequence of bits to the output file, after the header.  This will be done with plain ASCII characters '1' and '0'. (Note: you have just written your entire outfile in ASCII characters. Thus, your "compressed" outfile will actually be larger than the original!  The point here is purely to get the algorithm working.)

8. Close the input and output files.

9. __Test your solution. For more details, see the "Testing" section later in the PA__

__Note: Your Makefile must create the executables "compress" and "uncompress" with those exact names from the "make all" command. The Makefile given to you already does this, so just make sure you don't change any of the dependencies for 'make all'.__

#### The "file header" demystified
Both the compress and uncompress programs need to construct the Huffman Tree before they can successfully encode and decode information, respectively. The compress program has access to the original file, so it can build the tree by first deciphering the symbol counts. However, the uncompress program only has access to the compressed input file and not the original file, so it has to use some other information to build the tree. The information needed for the uncompress program to build the Huffman tree is stored in the header of the compressed file. So the header information should be sufficient in reconstructing the tree. Note that the "file header" is not a .hpp file but rather the top portion of the compressed file. 

Designing your header: A straightforward, non-optimized method that you __MUST USE__ for the assignment.

Probably the easiest way to do it is to save the frequency counts of the bytes in the original uncompressed file as a sequence of 256 integers at the beginning of the compressed file. For the PA you __MUST__ write this frequency-based header as 256 lines where each line contains a single int written as plain text.

E.g.:
> 0 <br>
> 0 <br>
> 0 <br>
> 23 <br>
> 145 <br>
> 0 <br>
> 0 <br>
> ...

### 3. Implement Uncompress
Create uncompress.cpp (from scratch!) that will use your above implementations to support decompress for small files. The uncompress.cpp should generate a program named uncompress that can be invoked from the command line as follows

> ./uncompress infile outfile <br>
> ./uncompress will:

1. Read the contents of the file named by its first command line argument, which should be a file that has been created by the compress program

2. Use the contents of that file to reconstruct the original, uncompressed version, which is written to a file named by the second command line argument. The uncompressed file must be exactly identical to the original file. 


Implement The Following Control Flow in uncompress.cpp :

1. Open the input file for reading.

2. Read the file header at the beginning of the input file, and reconstruct the Huffman coding tree. 

3. Open the output file for writing.

4. Using the Huffman coding tree, decode the bits from the input file into the appropriate sequence of bytes, writing them to the output file.

5. Close the input and output files.

6. Test your solution. For more details, see the "Testing" section later in the PA

__Submit early and often: When you have finished with the section, submit this section’s files to the PA3: Huffman Encoding - Naive submission on Gradescope.__

Files to turn-in:  Makefile, compress.cpp, uncompress.cpp, HCNode.hpp, HCNode.cpp, HCTree.hpp, HCTree.cpp

## Testing Tips

### General Tips
Because the PA writes in plain text, you can actually check the codes produced for small files by hand!  Remember large programs are hard to debug. So test each function that you write before writing more code. We will only be doing "black box" testing of your program, so you will not receive partial credit for each function that you write. However, to get correct end-to-end behavior you must unit test your program extensively.

For some useful testing tools, refer to this doc (especially cmp and diff -w). Additionally, we have provided you with two reference executables: refcompress and refuncompress. You can find both these executables by going to the following directory in your ieng6 account: /home/linux/ieng6/cs100s121/public/pa3_refs.  

Edge Cases Checklist: 

* Empty File
* Files that contain only one character repeated many times
* Think of more edge cases yourself!

Don't try out your compressor on large files (say 10 MB or larger) until you have it working on the smaller test files (< 1 MB). Even with compression, larger files (10MB or more) can take a long time to write to disk, unless your I/O is implemented efficiently. The rule of thumb here is that most of your testing should be done on files that take 15 seconds or less to compress, but never more than about 1 minute. If all of you are writing large files to disk at the same time, you’ll experience even larger writing times. Try this only when the system is quiet and you've worked your way through a series of increasing large files so you are confident that the write time will complete in about a minute.

## Getting Help

Tutors in the labs are there to help you debug. TA and Professor OH are dedicated to homework and/or PA conceptual questions, but they will not help with debugging (to ensure fairness and also so students have a clear space to ask conceptual questions). Questions about the intent of starter code can be posted on edstem. Please do not post your code to edstem either publicly or privately - debugging support comes from the tutors in the labs.

**_Format of your debugging help requests_**

At various times in the labs, the queue to get help can become rather long (all the more reason to start early). To ensure everyone can get help, we have a 5 minute debugging rule for tutors in that they are not supposed to spend more than 5 minutes with you before moving onto a new group. Please respect them and this rule by not begging them to stay longer as you’re essentially asking them to NOT help the next group in exchange for helping you more.

**_5 minutes?!_**

Yes, 5 minutes. The job of tutors is to help you figure out the next step in the debugging process, not to debug for you. So this means, if you hit a segfault and wait for help from a tutor, the tutor is going to say “run your code in gdb, then run bt to get a backtrace.” Then the tutor will leave as they have gotten you to the next step.

This means you should use your time with tutors effectively. Before asking for help, you will want to already have tried running your code in gdb (or valgrind, depending on the error). You should know roughly which line is causing the error and/or have a clear idea of the symptoms. When the tutor comes over, you should be able to say:

**What you are trying to do.** For example, “I’m working on the PA and am trying to get the insert method in the BST to work correctly.”

**What’s the error.** For example, “the code compiles correctly, but when I insert a child in my right subtree, it seems to lose the child that was there before.”

**What you’ve done already.**  For example, “I added the method which prints the whole tree (pointers and all) and you can see here _point to screen of output before and after insert_ that insert to the right subtree of the root just removes what was on the right subtree previously.  But looking at my code for that method, it seems like it should traverse past that old child before doing the insert. What do you suggest I try next?”

## Submitting Your PA

You can drag and drop your files or submit your code using Gradescope through Github submission option. You should have gotten enrolled in Gradescope by now (if not, please add your name to the appropriate edstem post). You should also be able to make **private** repositories on Github.

Instructions to submit your code on GradeScope using git:

1. **Be sure to test your code on ieng6.** We will be grading your code on the same environment as ieng6 and there may be issues with compilers/etc if you only tested your code on your personal machine.

2. Be sure to push the final version of your code to your private Github repository. That will be the code you will submit.

3. Go to gradescope and find PA3 submission. You will be asked to authorize your github account. After authorizing your account, choose the repo you pushed your PA3 code to and the correct branch.

4. You can submit as many times as you like before the deadline: only your last submission will be counted.

5. Go to gradescope and find PA3 assignment. If you successfully submitted your PA1 and PA2 (we hope you did!) then you should not have to authorize your github account again, but if you never submitted PA1/PA2 using git then you will be asked to authorize your github account. After authorizing your account, choose the repo you pushed your PA3 code to and the correct branch.

### Grading

Grading is holistic, and will be mostly black-box tested. This means that we mostly will not test your individual methods, but test your program as a whole. **It is very important that your output matches the output format mentioned in write up or any provided reference executables (See General Tips Section).**
