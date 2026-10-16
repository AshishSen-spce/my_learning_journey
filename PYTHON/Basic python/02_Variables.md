02: Variables

1. What is a variable?

A variable is a name that refers to a value.

Imagine a labelled box. You put a value in it, and the label helps you
refer to that value later.

In Python:

name = "Ashu"

name is the variable name.

"Ashu" is the value.

= is the assignment operator. It assigns the value to the
variable.

Now we can use the variable:

name = "Ashu"
print(name)

Output:

Ashu

2. Variables store different kinds of information

name = "Ashu"
age = 25
city = "Bhopal"

print(name)
print(age)
print(city)

Output:

Ashu
25
Bhopal

The values do not have to be text. A variable can refer to a number, a
Boolean, a list, or many other kinds of objects.

3. Variable names versus text

Look carefully at this example:

name = "Ashu"

print(name)
print("name")

Output:

Ashu
name

print(name) looks up the variable and displays its value.

print("name") displays the literal text name.

Quotation marks make a big difference!

4. Changing a variable's value

score = 10
print(score)

score = 20
print(score)

Output:

10
20

The second assignment makes score refer to the new value.

5. Using variables in calculations

apples = 10
oranges = 5

total_fruits = apples + oranges

print(total_fruits)

Output:

15

This is useful because the calculation updates when the input values
change.

price = 100
quantity = 3

total = price * quantity
print(total)

Output:

300

6. Rules for naming variables

Valid examples:

name = "Ashu"
age = 25
total_sales = 50000
customer_name = "Rahul"
_private_value = 10
number2 = 20

Invalid examples:

# Invalid: a variable name cannot start with a digit
# 2name = "Ashu"

# Invalid: a hyphen is treated as a minus operator
# my-name = "Ashu"

Follow these naming rules:

Start with a letter or underscore.

After the first character, use letters, numbers, and underscores.

Do not use spaces in variable names.

Do not use Python reserved keywords such as if, for, or class
as variable names.

Use descriptive names so your code is easy to understand.

Python convention commonly uses snake_case for variable names:

first_name = "Ashu"
monthly_revenue = 75000
total_students = 120

7. Python is case-sensitive

name = "Ashu"
Name = "Rahul"

print(name)
print(Name)

Output:

Ashu
Rahul

name and Name are two different variable names.

8. Assigning multiple variables

You can assign values to multiple variables in one line:

name, age, city = "Ashu", 25, "Bhopal"

print(name)
print(age)
print(city)

You can also assign the same value to several variables:

x = y = z = 0
print(x, y, z)

Output:

0 0 0

Use these shortcuts when they make the code clearer.

9. A common mistake

This code causes an error:

# print(total)

If you try to use a variable before assigning it, Python may raise a
NameError.

Correct:

total = 100
print(total)

10. Practice questions

Create a variable called name and store your name in it.

Create variables called age and city, then print them.

Store 100 in price and 4 in quantity. Calculate and print
the total.

What is the difference between score and "score"?

Are customer_name and Customer_Name the same variable?

Identify which variable names are valid: first_name, 2nd_name,
total-sales, total_sales, class.

11. Mini challenge: Personal introduction

Create variables for: - Your name. - Your age. - Your city. - The
programming language you are learning.

Print each value. Then create a variable called learning_message and
store a sentence in it.

Key takeaways

A variable is a name that refers to a value.

Use = to assign a value.

Use the variable name without quotes to access its value.

Use quotation marks when you want to display literal text.

Choose clear, descriptive variable names.

Python variable names are case-sensitive.