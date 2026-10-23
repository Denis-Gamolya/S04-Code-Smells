# S04-Code-Smells
# Denis Gamolya, Yahir Prenger

# Long Parameter List

# Long Functions

## What are Long Functions?

Lengthy methods that contain many lines and blocks of code. While they might all serve one end result, the abundance of code can confuse readers and make it harder to understand what each line achieves.

## How do they Arise?

Some programmers might find it easier to continue adding onto a function than to begin creating new functions. Especially if it's just a few lines of code, it might seem redundant or unnecessary to create a brand new functions. However, the lines begin to add up until a monstrous and multiserving function exists.

## How to Avoid and Fix?

When a block of code needs to be preceded with a comment for understanding, that block can most likely be rewritten into a new function (Extract Function). 

## Javascript Example




