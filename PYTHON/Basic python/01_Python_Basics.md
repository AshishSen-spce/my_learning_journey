Lesson 01: Python Basics and Syntax

1. What is Python?

Python is a programming language. We use it to give instructions to a
computer.

Python can help us: - Perform calculations. - Automate repetitive
tasks. - Read and write files. - Work with databases. - Analyse data. -
Build websites, APIs, and AI applications. - Build data pipelines using
tools such as pandas and PySpark.

Python is popular because its code is designed to be readable and it has
many useful libraries.

2. Your first Python program

print("Hello, World!")

Output:

Hello, World!

Understand the code

print() is a built-in Python function that displays information.

"Hello, World!" is a string (text).

Parentheses () contain the information passed to the function.

The quotation marks tell Python that the value is text.

Try this:

print("My name is Ashu")
print("I am learning Python")
print("This is my first lesson")

Each print() normally displays its result on a new line.

3. Printing text and numbers

Text is usually written inside quotation marks:

print("Python is fun")

Numbers can be written without quotation marks:

print(10)
print(25 + 5)

Output:

10
30

These two examples look similar when printed, but Python treats "10"
as text and 10 as a number.

print("10")
print(10)

4. Basic Python syntax rules

Rule 1: Match quotation marks

Correct:

print("Hello")
print('Hello')

Incorrect:

# This causes a syntax error:
# print("Hello)

Rule 2: Use parentheses correctly

Correct:

print("Hello")

Incorrect:

# This causes a syntax error:
# print("Hello"

Rule 3: Python is case-sensitive

print() is correct. Print() is a different name and will not work as
the built-in print function.

print("Correct")

Rule 4: Indentation matters

Python uses indentation to show which statements belong together. We
will study this more when we learn conditions and loops.

Example:

if 5 > 2:
    print("Five is greater than two")

The indented line belongs to the if statement.

Rule 5: Comments begin with #

Comments are notes for people reading the code. Python ignores a comment
when running the program.

# This is a comment
print("Hello")

5. Simple calculations

Python can calculate directly:

print(10 + 5)
print(10 - 5)
print(10 * 5)
print(10 / 5)

Output:

15
5
50
2.0

We will learn each operator in detail in a later lesson.

6. Common beginner mistakes

Mistake                 Why it happens          How to fix it

Missing quotation mark  Text is not properly    Check that both quotes
enclosed                are present

Missing closing         The function call is    Add the matching )
parenthesis             incomplete

Writing Print()       Python is               Use lowercase print()
case-sensitive

7. Practice questions

Write a program that prints your name.

Print three lines describing why you want to learn Python.

Print the result of 100 + 250.

What is the difference between print("25") and print(25)?

Find and fix the mistake in this code:

print("Learning Python)

8. Mini challenge

Write a program that displays this exact output:

My Python Journey
I am starting from zero.
I will practise every day.

Try to write the code yourself before checking any examples.

Key takeaways

Python is a language for giving instructions to a computer.

print() displays information.

Text is written inside quotation marks.

Python is case-sensitive.

Comments start with #.

Syntax errors usually mean that Python cannot understand how the
code is written.