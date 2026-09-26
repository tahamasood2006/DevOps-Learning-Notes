## To Learn Next:

1. boto3
2. Flask(basic)
3. docker lib
4. kubernetes lib
5. os 
6. pathlib & shutil,
7.  subprocess
8. paramiko
9. pyyaml
10. requests 
11. fabric
12. pytest

## ALL BASIC CONCEPTS:

PASTE THE CODE IN VS-CODE AND READ THE CODE TO UNDERSTAND THE SYNTAX

```jsx
print("goodbye")

# Comments in Python :
# start with the # symbol for Single line comments
# ''' ''' for multi-line comments

# Variables in Python:

a = 10
b = 2.3
c = True
d = "Hello"
e = None

print(a, b, c, d, e)

# gives what type of variable it is
f = type(a)
print(f)

# Type-casting in Python:

g="8"
h = int(g)+1
print(h)

# Logical Operators in Python:

i = 5
j = 2

print(i >= j and i <= 10)  
print(i==j or i != 10)  
print(not(i==j))    

# String and its methods in Python:

h = "Hello World "
print(h[0:3]) # slicing of strings 

# multi-line string in python:
har = ''' 
This is a multi-line string.
and valid too
'''

print(har)

#Note: for more search pyhton string methods 

print(h.lower()) # converts to lower case
print(h.upper()) # converts to upper case

print(h.replace("World", "Python")) # replaces World with Python
print(h.split(" ")) # splits the string into a list of words
print(h.count("l")) # counts the number of occurrences of "l" in the string

# input in python:
name = input("Enter your name: ") # IMP: input func takes as string only

print("Hello, " + name + "!")

# List in Python:
 
  # a list is mutable, means it can change its elements after creation
  # list supports slicing and indexing 

kal = [1, 221,"Panda", 331, 114, 35]

print(kal) 

print(type(kal)) # gives the type of variable it is
print(kal[2]) # prints the third element of the list

# list methods in python:

kal.append("New Element") # adds a new element to the end of the list
kal.pop() # removes the last element of the list

kal.remove(221) # removes the element 221 from the list
kal.remove("Panda") # removes the element "Panda" from the list
kal.remove(kal[1]) # removes the second element of the list

kal.insert(1, "Inserted Element") # inserts a new element at the second position of the list
kal.clear() # removes all elements from the list

kal.extend([98, "pak", 31]) # adds multiple elements to the end of the list
kal.index(98) # returns the index of the element "pak" in the list

# Tuples in Python:

    # a tuple is immutable, means it cannot change its elements.

tupe = (551, 112, 5, 13, 334, 5)

print(tupe.index(13)) # returns the index of the element 13 in the tuple
print(tupe.count(5)) # counts the number of occurrences of 5 in the tuple

# Sets in Python:

se =  {77, 12, 5908 ,5 , 4, 5}
print(se) # prints the set, will not print duplicate elements like 5 will be printed only once

sd = {98,12}
ni = set() # creates an empty set

se.add(100) # adds a new element to the set
se.remove(12) # removes the element 12 from the set
se.pop() # removes a random element from the set
se.clear()  # removes all elements from the set

se.union(sd) # returns a new set with all elements from both sets
se.intersection(sd) # returns a new set with only the elements that are common to both

# Dictionary in Python:

dict1={"John": 32, "Jane": True, "Bob": "45"}

# BOTH LINE BELOW DO THE SAME THING
print (dict1["Bob"]) # prints value associated with key in the dictionary
print(dict1.get("Jane")) # prints value associated with key in the dictionary

# BOTH LINE BELOW DO THE SAME THING
dict1["Alice"] = 25 # adds a new key-value pair to the dictionary
dict1.update({"Eve": 30}) # adds a new key-value pair to the dictionary

print(dict1.keys()) # prints all keys in the dictionary
print(dict1.values()) # prints all values in the dictionary
print(dict1.items()) # prints all key-value pairs in the dictionary

dict1.pop("John") # removes the key-value pair with key "John" from the dictionary
dict1.popitem() # removes the last key-value pair from the dictionary

# IF-ELSE statements in Python:

Age = int(input("Enter your age: "))

if(Age > 18):
    print("You are an adult.")
elif(Age == 18):
    print("You are exactly 18 years old.")
else:
    print("You are a minor.")

# Match-Case in Python:

kat = 6

match kat:
    case 5:
        print("kat is 5")

    case 6:
        print("kat is 6")

    case _: # default case , OPTIONAL TO USE
        print("By default")

# For loop in Python:  

for i in range(5): # prints 0 to 4
    print(i)

 #IMP: * break can be used to exit the loop and continue can be used to skip the current iteration and move to the next one
for h in range(8):
    if(h==3):
        continue # skips the current iteration and moves to the next one
    if(h==6):
        break # exits the loop
    print(h)

# -- for loop with set in Python: 
       # also can be used with list, tuple, dictionary

loopwithSet = {99, 12, 33, 114, 55}

for z in loopwithSet: # prints all elements in the set but in rando
    print(z) 

# While loop in Python:

whl = 91

while (whl < 94): # while loop will work till the condition is true
    print(whl)
    whl += 1

# -- while loop with break and continue in Python:
while(True):
    num = int(input("Enter a number: "))
    if(num < 5):
        print("Number entered is less than 5. Exiting the loop.")
        break # exits the loop
    elif(num == 0):
        print("Zero entered. Skipping this iteration.")
        continue # skips the current iteration and moves to the next one
    else:
        print(f"You entered: {num}")

# Functions in Python:

def licenseChecker(name, age):
    if(age >= 18):
        print(f"{name}, you are eligible for a driving license.")
    else:
        print(f"{name}, you are not eligible for a driving license.")

print("\n \n Functions in Python:")
print("Welcome to the License Eligibility Checker!" )
print("Enter your name and age to check license eligibility:")

licenseChecker("hari", 19)
licenseChecker("John", 17)

name = input("Enter your name: ")
age = int(input("Enter your age: "))
licenseChecker(name, age)

# Exception Handling in Python: try-except

print("\n \n Error Handling in Python:")
try:
    ab1 = int(input("Enter a number: "))
    print(f"You entered: {ab1}")
except:
    print("Invalid input. Please enter a valid number.")

# OR TO print the error message we can use as below:
try:
    ab1 = int(input("Enter a number: "))
    print(f"You entered: {ab1}")
except Exception as e:
    print(f" Error is : {e}")            

# File Handling in Python:

## WRITING TO A FILE IN PYTHON:

# - using content manager to open a file and write to it
sor = "This is a not sample text file."
with open("log1.txt","w") as kai:
    kai.write(sor)

# - without content manager 
op1 = "This is a sample text file. --- IGNORE ---"
f1 = open("log2.txt","w")
f1.write(op1)
f1.close()

## READING FROM A FILE IN PYTHON:

# - using content manager to  read a file
with open("log2.txt","r") as kai:
    ski = kai.read()
    print(ski)

# - without content manager reading a file

f2 = open("log2.txt","r")
kqi = f2.read()
print(kqi)
f2.close()

## APPENDING TO A FILE IN PYTHON:

# - using content manager to append to a file
with open("log2.txt","a") as kai:
    kai.write("\nThis is a new line added to the file.")

# OOPS & Classes in Python:

class Employee:
    _name = "fayyaz"
    _age = 30
    _job_title = "Software Engineer"
    _salary = 50000
    def info(self):
        print(f"Name: {self._name}, Age: {self._age}, Job Title: {self._job_title}, Salary: {self._salary}")

Ahm = Employee()
Ahm._name = "Ahmad"
Ahm.info()

Bisma = Employee()
Bisma._name = "Bisma"
Bisma._age = 25
Bisma.info()

```

# Python libraries

os library

pathlib, shutil, subprocess, paramiko

PyYaml, requests, fabric , pytest
