---
title: HSCTF 2021 - class-meets | Programming
published: true
---

## [](#header-2)class-meets

[Class Meets.pdf](https://github.com/DamoNeer/hacker-blog/files/6680071/Class.Meets.pdf)


### [](#header-3)Solution

Rather than looking at this situation as a 2D 5x5 matrix, I decided to think of this as just a 1D array. 

12 months with 30 days each and no holidays except weekends means there will be 360 elements in a list. Subtracting that amount by 96 days will give us the total amount of weekdays in a year.

Based on the inputs for each student, I will tell the program to follow a for loop that is in range of (360-96) times, and append the student's meeting habits respectively.

For example, in the sample input 1, I2 V4 for student 1 means that the list will first append 2 "I" then append 4 "V" and it will repeat this pattern until the loop ends.

So, student 1's list will look like (I, I, V, V, V, V, I, I, V, V, V, V...)

For every 5 elements, there will be 2 days that have no important values because there is no school on the weekends.

I will insert the value "x" to indicate weekends into each student's list.

Student 1's list will now become (I, I, V, V, V, x, x  V, I, I, V, V, x, x, V, V...)

Student 2's list will be created using the same manner as well. 

Now the start date and end date are represented with something like this "M0 D3" which indicates 0th Month and 3rd Day.

I will convert this to a base 30 number. In other words, multiply the number next to M with 30 and add the number next to D. So, M0 D3 will refer to the third element on my list.

Then, I will put them as the start and end of my range so that I can tell the program to only focus on that part of my student1 and student2 list for comparison.

I didn't use pwntools to automate the answering. The amount of time I spend to format the way I listen for those strings would be longer than the amount of time I spend to just manually read and enter each answer.

Here's my script:

```
# M0 D3 -> 0*30 + 3 -> 3    From Day3
# M0 D16 -> 0*30 + 16 -> 16 To   Day16

start = 0*30+3
end = 0*30+16

# I2 V4 -> 2 4
student1 = []
input1 = 2
input2 = 4
for i in range(0, 12*30-8*12, input1+input2):  # (0,6,12,18...)
    for j in range(input1):
        student1.append('I')
    for k in range(input2):
        student1.append('V')
i = 5
while i < len(student1):
    student1.insert(i, 'x')
    i += (5+1)
i = 6
while i < len(student1):
    student1.insert(i, 'x')
    i += (6+1)

# I4 V1 -> 4 1
student2 = []
input3 = 4
input4 = 1
for i in range(0, 12*30-8*12, input3+input4):  # (0,6,12,18...)
    for j in range(input3):
        student2.append('I')
    for k in range(input4):
        student2.append('V')

i = 5
while i < len(student2):
    student2.insert(i, 'x')
    i += (5+1)
i = 6
while i < len(student2):
    student2.insert(i, 'x')
    i += (6+1)

student1 = student1[start:end+1]
student2 = student2[start:end+1]


counter = 0
for i in range(end+1-start):
    if student1[i] != 'x':
        if student1[i] == student2[i]:
            counter += 1

print(counter)

```
![image](https://user-images.githubusercontent.com/81070073/122631994-a2c4d080-d084-11eb-9c20-8010f10ab9ee.png)
