# 2324-R-Debugging Procedures

One of the most important skills of a programmer or engineer is debugging a program or system. The following are strategies that may help in finding and solving problems.

## Start here

Before you start debugging ask these questions:

- Do I know what the problem is? (can I explain clearly what **should** be happening and what **is** happening instead)
- Can I demonstrate the problem? (can I show someone the problem happening)
- Have I used my resources? (are there instructions, notes, example code, knowledgeable peers I can consult?)
- Do I have a hypothesis? (Do I have a theory as to why the problem is happening?)
- Can I check this hypothesis myself? (Is there a quick check I can do to test my theory? Either by changing/eliminating the problem or doing something to confirm that this really is causing the problem)

## Types of Problems

Problems fall into three big categories: 

- software syntax
- software logic, and 
- hardware

Each has its own strategies for debugging your project.

## Software Syntax

These kinds of errors are when your code won't run, won't compile or throws error messages related to the code you wrote or are using. These can be the hardest to find and the easiest to fix.

### Read the error message

Carefully read any errors or messages. Identify keywords, phrases or symbols that you recognize and work to parse what the error message is saying. For Arduino C the error will look something like:

![image-20240201085835752](C:\Users\benjamin.lawrence\AppData\Roaming\Typora\typora-user-images\image-20240201085835752.png)

The last line is the error message: *expected ',' or ';' before 'Serial'* We can parse this by seeing that the computer wanted a character, and we can see that one character it might want is a semicolon. We know that semicolons end each command statement. Maybe we forgot to put one in?

**Always read error messages, pop-ups and notifications carefully, keep them on your screen until you understand what they say.**

### Find the error

In the example above, the error message also includes a "trace back" which shows where things went wrong in the code. Start from the last line and work upwards through the trace. The long line of text **"C:\Users\benjamin.lawrence\ etc. etc."** shows the file we are running or caused the error. The end of the line is most important. The **10:3** says that the error happened on line ten, at character three. This can help us find and fix the error.

### Look at the crack in the sidewalk, not where you fell

**If you trip on the sidewalk, the problem isn't where you hit the ground but behind you where you tripped.** In code an error is often caused by a problem earlier in the code. If the problem isn't on the line indicated in the error message, look at the lines above or that ran just before that line of code, or where you have created or changed functions or variables you are using on that line.

### Tricky, tricky

**Sometimes a syntax error doesn't throw an error message.** This is because the code you wrote has missing syntax but it accidentally forms valid code. Common examples in Arduino C are below:

```c
if (a = 7){
    
} /// should be a == 7 (two equals to check, one to set). The code here will run without error, always be true and will quietly change the value of a to 7 and run what's in the if statement

if (a == 7)
    Serial.print("I want to print this only if");
    Serial.println("a is seven");
/* Should be 
if (a == 7){
    Serial.print("I want to print this only if");
    Serial.println("a is seven");
}
the missing curly brackets won't throw an error, but will mean that the if statement only applies to the next line of code, and the "a is seven" line will always print and won't depend on the if statement. This is also true for else if, else and for control structures.
*/

if (a & b == 4){
    
} // Should be a == 4 && b == 4, the two ampersands check if both values are true. A single ampersand will perform a binary AND operation, which is usually identical, but can end up with edge cases where it is not.

```


## Software Logic

These kinds of errors are when your code **runs** ("works", compiles, downloads successfully) but doesn't **do** what you want it to. These errors require the most thinking, and sometimes lead to interesting new approaches and use of the tools we have in our code.

### Be the computer

**Start at the top line of your code, and read each line in turn like the computer would.** Keep track of what functions you are running, and what variables you are creating or updating. Think about the logic of control statements. What values would make a condition true, and what would the computer do next? What values would make a condition false and what would the computer do then? It is often useful to take a piece of paper and place it under the line of code you are reading so you can focus on that one line at a time, just like a computer does.

### Diagram it

**Draw a diagram for what you think the code should do.** This can be a sketch of how things should look, or where things will move, or what the output should look like or a flow-chart of the logic. Can you clearly define what a successful code will do? Can you do it? draw it? act it out?

### Explain the code
Explain to another person exactly what the code should do and how it will do it. Be as specific, step by step and detailed as you can. Often just by talking out loud you will realize your error and where you went wrong. If you don't have another person to help with this, explain your code to an inanimate object. **Good programmers keep a rubber duck on their desk** for this purpose.

### Simplify
For each step of your design you should be working on the smallest possible goal to move your project forward. **If you are trying to test two things, that's one thing too many.** If you are trying to fix a problem, can you get it working on its own without the rest of your program? If it works on its own, test if it works with the rest of the code. If it doesn't, what about the code around it is making it fail? Check variable values, conditions and logic.

## Hardware

Hardware problems are when the physical electronic system is at fault. These are sometimes incredibly tricky to find and require careful testing and care, but most of the time are embarrassingly simple.

### Check the hardware

Is it plugged in? Plugged in to the right place? Turned on? Charged? Stuck? Hot to the touch? On fire? **Check all of these every time.**

### Power cycle

**Turn things off, close them, unplug them, then carefully turn on, re-open and plug them in.** Did this change anything?

### Connections

Are things connected in the right **order** and **orientation**? Is anything extra connected that shouldn't be? Does it match the circuit diagram, plans, images or instructions you are following?

### Conductivity and Voltages

As a last resort, track the connections using a multimeter. Start by checking for conductivity. Are the things you think are connected actually conducting to each other? Trace through the full circuit from start to finish. Next, turn on the system and place your negative terminal of the multimeter on the common ground of the circuit (you do have a common ground right?), and begin measuring the voltage of each component, pin or chip leg. Do the voltages match what you expect? Do they match the logic of any code you wrote?

