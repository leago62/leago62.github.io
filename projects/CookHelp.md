---
layout: post
title:  "Cook Help Project"
date:   2019-10-4
permalink: /projects/Cook_Help
---

## Project Overview:

As I began to start having to cook for myself, I found that I was starting to
generate a lot of food waste for no good reason. It was simply that I would
only work with one or two leftover ingredients at a time. I wanted a way to look
up recipes based on the inventory of my fridge and pantry. Not just the stuff I
bought within the week, but the things that I bought a lot time ago for one
recipe, that I might now use in another. Currently I'm trying to build some sort
of application that will allow me to query recipes based on what I want to cook
with and my own ingredient inventory. It will hopefully minimize the amount of
items I would need to buy and my overall food waste.

Goals: My goal is to be able to build this dataset using SQL to grow my
knowledge of databases and the SQL language. I also hope to be able to directly
take recipe information from images and websites and add it into the database.
I also hope to have some sort of recommendation system, where it will rank the
recipes based on how little you'd need to buy and what your tastes are like.

Challenges: There will be many, many challenges for me on this project. I don't
have very much knowledge of building apps and databases, but hopefully this
experience will really help me grow.


This is how my tables are set up right now using MySQL. I am missing the tables
for my own inventory and also for the instruction steps of the recipe.\
<img src="/projects/t1_tables.png" alt="Recipe database setup" width=500px>


This is the python code I am working on right now. I am trying to input into the
table and once that works, I will working on how to parse the strings from the
recipe documents to insert into all tables.

```python
import mysql.connector

mydb = mysql.connector.connect (
    # Connect information: host user, passwd
    database="cookhelp"
)

mycursor = mydb.cursor(buffered=True)

recipe_name = "Baked Salmon"
ing = ["Salmon (w/skin on)", "lemon (fresh)", "salt", "pepper"]
ing_qty = [1.0, 0.5, None, None]
ing_amt = ["fillet", None, None, None]
cooktime = 35

## INSERT IGNORE is not doing the IGNORE.

sql = "INSERT IGNORE INTO `recipes` (`Recipe_Name`, `Cooktime`) VALUES (%s,%s)"
val = (recipe_name,cooktime)
mycursor.execute(sql,val)
recipe_id = mycursor.lastrowid

for x in range(len(ing)):
    sql2 = "INSERT IGNORE INTO `ing_list` (`Ing_Name`) VALUES (%s)"
    ing_query = "SELECT `Ing_ID` FROM `ing_list` WHERE `Ing_Name` = %s"
    print(ing[x])
    mycursor.execute(sql2,(ing[x],))

    mycursor.execute(ing_query,(ing[x],))
    ing_id = mycursor.fetchone()

    val3 = (recipe_id, ing_id[0], ing_qty[x], ing_amt[x])
    print(val3)
    sql3 = "INSERT IGNORE INTO `recipe_ing` VALUES (%s, %s, %s, %s)"
    mycursor.execute(sql3,val3)

mydb.commit()
mycursor.close()
mydb.close()
```
