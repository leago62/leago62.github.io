---
layout: post
title:  "Cook Help Project"
date:   2019-10-22
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

Next Step: Set up foreign keys to connect my tables. Study NLP methods nd then
implement one to take out the important ingredient information from any written
recipe source.

Goals: My goal is to be able to build this dataset using SQL to grow my
knowledge of databases and the SQL language. I also hope to be able to directly
take recipe information from images and websites and add it into the database.
I also hope to have some sort of recommendation system, where it will rank the
recipes based on how little you'd need to buy and what your tastes are like.

Challenges: There will be many, many challenges for me on this project. I don't
have very much knowledge of building apps and databases, but hopefully this
experience will really help me grow.


This is how my tables are set up right now using MySQL. I am missing the tables
for my own inventory and also for the instruction steps of the recipe.
<img src="/projects/images/t1_tables.png" alt="Recipe database setup" width=500px>


This is the python code I am working on right now. It takes prompts and takes
user input to fill out the categories and adds them to the MySQL database. The
parsing method is very simple, but I am looking at NLP methods to parse the data.

```python
import mysql.connector

mydb = mysql.connector.connect (
    # host, user, passwd
    database="cookhelp"
)

mycursor = mydb.cursor(buffered=True)

def is_numeric(s):
    try:
        float(s)
        return True
    except ValueError:
        return False

# Can only be used for "qty amt ing_name"
def manualParse():
    ing_list = []
    ing_qty = []
    ing_amt = []
    print("Enter recipe name: ")
    recipe_name = input()
    print("Enter cooktime: ")
    cooktime = input()
    print("Enter ingredients: ")
    ing1 = input()
    while ing1 != "":
        try:
            qty, p1 = ing1.split(" ",1)
            if is_numeric(qty):
                amt, ing_name = p1.split(" ",1)
            else:
                qty = None
                amt = None
                ing_name = ing1
        except ValueError:
            qty = None
            amt = None
            ing_name = ing1
        ing_qty.append(qty)
        ing_amt.append(amt)
        ing_list.append(ing_name)
        ing1 = input()
    return recipe_name, cooktime, ing_list, ing_qty, ing_amt;

recipe_name, cooktime, ing, ing_qty, ing_amt = manualParse()

insert_recipe = ("INSERT INTO recipes (`Recipe_Name`, `Cooktime`) "
    "SELECT * FROM (SELECT %s,%s) AS tmp "
    "WHERE NOT EXISTS ("
	   "SELECT `Recipe_ID` FROM recipes WHERE `Recipe_Name` = %s"
    ") LIMIT 1;")
val = (recipe_name, cooktime, recipe_name)
mycursor.execute(insert_recipe,val)
mydb.commit()

select_rid = ("SELECT `Recipe_ID` FROM recipes WHERE `Recipe_Name` = %s;")
val1 = (recipe_name,)
mycursor.execute(select_rid,val1)
last_rid = mycursor.fetchone()[0]

for x in range(len(ing)):
    insert_ing_list = ("INSERT INTO `ing_list` (`Ing_Name`) "
        "SELECT * FROM (SELECT %s) AS tmp_i "
        "WHERE NOT EXISTS ("
            "SELECT Ing_ID FROM `ing_list` WHERE Ing_Name = %s"
        ") LIMIT 1;")
    ing_query = "SELECT `Ing_ID` FROM `ing_list` WHERE `Ing_Name` = %s"
    print(ing[x])
    mycursor.execute(insert_ing_list,(ing[x],ing[x]))

    mycursor.execute(ing_query,(ing[x],))
    ing_id = mycursor.fetchone()[0]

    val2 = (last_rid, ing_id, ing_qty[x], ing_amt[x])
    print(val2)
    insert_rec_ing = "INSERT IGNORE INTO `recipe_ing` VALUES (%s, %s, %s, %s)"
    mycursor.execute(insert_rec_ing,val2)
    mydb.commit()

mycursor.close()
mydb.close()
```

SQL code to create the database:

``` sql
DROP DATABASE IF EXISTS `cookhelp`;
CREATE DATABASE `cookhelp`;
USE `cookhelp`;

SET NAMES utf8;
SET character_set_client = utf8mb4;

CREATE TABLE `recipes` (
  `Recipe_ID` tinyint NOT NULL AUTO_INCREMENT,
  `Recipe_Name` varchar(50) NOT NULL,
  `Cooktime` tinyint,
  PRIMARY KEY (`Recipe_ID`)
);

CREATE TABLE `ing_list` (
  `Ing_ID` tinyint NOT NULL AUTO_INCREMENT,
  `Ing_Name` varchar(50) NOT NULL,
  PRIMARY KEY (`Ing_ID`)
);

CREATE TABLE `recipe_ing` (
  `Recipe_ID` tinyint NOT NULL,
  `Ing_ID` tinyint NOT NULL,
  `Qty` float,
  `Amt` varchar(25),
  FOREIGN KEY (Recipe_ID) REFERENCES recipes(Recipe_ID),
  FOREIGN KEY (Ing_ID) REFERENCES ing_list(ING_ID)
  -- Need to figure out how to store diced vs canned vs paste etc.
);
```
