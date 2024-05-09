# Discussion and Reflection


The bulk of this project consists of a collection of five
questions. You are to answer these questions after spending some
amount of time looking over the code together to gather answers for
your questions. Try to seriously dig into the project together--it is
of course understood that you may not grasp every detail, but put
forth serious effort to spend several hours reading and discussing the
code, along with anything you have taken from it.

Questions will largely be graded on completion and maturity, but the
instructors do reserve the right to take off for technical
inaccuracies (i.e., if you say something factually wrong).

Each of these (six, five main followed by last) questions should take
roughly at least a paragraph or two. Try to aim for between 1-500
words per question. You may divide up the work, but each of you should
collectively read and agree to each other's answers.

[ Question 1 ] 

For this task, you will three new .irv programs. These are
`ir-virtual?` programs in a pseudo-assembly format. Try to compile the
program. Here, you should briefly explain the purpose of ir-virtual,
especially how it is different than x86: what are the pros and cons of
using ir-virtual as a representation? You can get the compiler to to
compile ir-virtual files like so: 

racket compiler.rkt -v test-programs/sum1.irv 

(Also pass in -m for Mac)

add.irv:
```
(mov-lit r1 10)
(mov-lit r2 20)
(add r3 r1 r2)
(print r3)
```

mul.irv: 
```
(mov-lit r1 5)
(mov-lit r2 7)
(mul r3 r1 r2)
(print r3)
```
sub.irv:
```
(mov-lit r1 7)
(mov-lit r2 28)
(sub r3 r2 r1)
(print r3)
```

The purpose of the IR-Virtual is to serve as an intermediate step between the higher-level representations and the final x86 assembly code. IR-Virtual represents a simplified set of instructions with virtual registers, abstracting away the complexities of the target architecture 

[ Question 2 ] 

For this task, you will write three new .ifa programs. Your programs
must be correct, in the sense that they are valid. There are a set of
starter programs in the test-programs directory now. Your job is to
create three new `.ifa` programs and compile and run each of them. It
is very much possible that the compiler will be broken: part of your
exercise is identifying if you can find any possible bugs in the
compiler.

For each of your exercises, write here what the input and output was
from the compiler. Read through each of the phases, by passing in the
`-v` flag to the compiler. For at least one of the programs, explain
carefully the relevance of each of the intermediate representations.

For this question, please add your `.ifa` programs either (a) here or
(b) to the repo and write where they are in this file.
sub1.ifa
```
(let* ([x 20]
       [y 10]
       [z (- x y)])
  (print z))
```
output: 10

sub2.ifa
```
(let* ([x 30]
       [y 10]
       [z (- x y)])
  (print z))
```
output: 20

aub3.ifa
```
(let* ([x 25]
       [y 5]
       [z (- x y)])
  (print z))
```
output: 20
[ Question 3 ] 

Describe each of the passes of the compiler in a slight degree of
detail, using specific examples to discuss what each pass does. The
compiler is designed in series of layers, with each higher-level IR
desugaring to a lower-level IR until ultimately arriving at x86-64
assembler. Do you think there are any redundant passes? Do you think
there could be more?

In answering this question, you must use specific examples that you
got from running the compiler and generating an output.

The first pass converts the high-level IfArith language into a more restricted subset called IfArith-Tiny. This involves desugaring constructs like let*, and, or, and cond into simpler forms using let, if, and primitive operations.
Example: (let* ([x 1] [y 2]) (+ x y)) is desugared to (let ([x 1]) (let ([y 2]) (+ x y))).

The next pass transforms the IfArith-Tiny representation into Administrative Normal Form (ANF)to break down complex expressions into simple bindings, making it easier to reason about the control flow.
Example: (+ (* 2 3) 4) is transformed into (let ([x (* 2 3)]) (let ([y (+ x 4)]) y)).

The ANF representation is then converted into a sequence of virtual instructions with virtual registers, called IR-Virtual.
Example: (let ([x 1]) (let ([y (+ x 2)]) y))would be translated into:
```
(mov-lit r1 1)
(mov-reg r2 r1)
(add r2 2)
```
The IR-Virtual representation is then translated into x86-64 assembly code which is printed in a format that can be assembled by the NASM 


[ Question 4 ] 

This is a larger project, compared to our previous projects. This
project uses a large combination of idioms: tail recursion, folds,
etc.. Discuss a few programming idioms that you can identify in the
project that we discussed in class this semester. There is no specific
definition of what an idiom is: think carefully about whether you see
any pattern in this code that resonates with you from earlier in the
semester.

The code heavily relies on pattern matching, particularly in the ifarith?, ifarith-tiny?, virtual-instr?, and labeled-virtual-instr? functions. Many functions in the project also recursively process data structures, such as the ifarith->ifarith-tiny function, which recursively transforms an IfArith expression into an IfArith-Tiny expression

[ Question 5 ] 

In this question, you will play the role of bug finder. I would like
you to be creative, adversarial, and exploratory. Spend an hour or two
looking throughout the code and try to break it. Try to see if you can
identify a buggy program: a program that should work, but does
not. This could either be that the compiler crashes, or it could be
that it produces code which will not assemble. Last, even if the code
assembles and links, its behavior could be incorrect.

To answer this question, I want you to summarize your discussion,
experiences, and findings by adversarily breaking the compiler. If
there is something you think should work (but does not), feel free to
ask me.

Your team will receive a small bonus for being the first team to
report a unique bug (unique determined by me).

Incorrect Handling of Negative Numbers:
The compiler does not seem to handle negative numbers correctly. When I tried compiling the following program:
```
(let* ([x -10]
       [y 5]
       [z (+ x y)])
  (print z))
```
The output should be -5, but it printed a large positive number instead.

[ High Level Reflection ] 

In roughly 100-500 words, write a summary of your findings in working
on this project: what did you learn, what did you find interesting,
what did you find challenging? As you progress in your career, it will
be increasingly important to have technical conversations about the
nuts and bolts of code, try to use this experience as a way to think
about how you would approach doing group code critique. What would you
do differently next time, what did you learn?

Working on this project has been an enlightening experience that provided valuable insights into the inner workings of compilers and the challenges involved in their development. One of the most fascinating aspects was the opportunity to observe the step-by-step transformation of a high-level language into low-level machine code through a series of intermediate representations.
The project was particularly interesting because it allowed me to explore the practical application of many concepts and idioms we discussed throughout the semester, such as pattern matching and recursive data structure processing. Seeing these idioms in action within the context of a real-world problem, like building a compiler, reinforced my understanding and appreciation of their utility. A challenging aspect was the process of identifying potential bugs in the compiler.
This experience highlighted the importance of thorough testing and the need to consider edge cases and potential pitfalls when developing complex software systems. Overall, this project has been an invaluable learning experience that has deepened my understanding of compilers.
