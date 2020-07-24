# Houses

## Introduction

Hogwarts is in need of a student database. For years, the professors have been maintaing a CSV file containing all of the students’ names and houses and years. But that file didn’t make it particularly easy to get access to certain data, such as a roster of all the Ravenclaw students, or an alphabetical listing of the students enrolled at the school.

The challenge ahead of you is to import all of the school’s data into a SQLite database, and write a Python program to query that database to get house rosters for each of the houses of Hogwarts.

Implement a program to import student data into a database, and then produce class rosters.

```shell
$ python import.py characters.csv
$ python roster.py Gryffindor

Lavender Brown, born 1979
Colin Creevey, born 1981
Seamus Finnigan, born 1979
Hermione Jean Granger, born 1979
Neville Longbottom, born 1980
Parvati Patil, born 1979
Harry James Potter, born 1980
Dean Thomas, born 1980
Romilda Vane, born 1981
Ginevra Molly Weasley, born 1981
Ronald Bilius Weasley, born 1980
```

## Specification

In `import.py`, write a program that imports data from a CSV spreadsheet.

1. Your program should accept the name of a CSV file as a command-line argument.

- If the incorrect number of command-line arguments are provided, your program should print an error and exit.
- You may assume that the CSV file will exist, and will have columns `name`, `house`, and `birth`.

2. For each student in the CSV file, insert the student into the `students` table in the `students.db` database.

- While the CSV file provided to you has just a `name` column, the database has separate columns for `first`, `middle`, and `last` names. You’ll thus want to first parse each name and separate it into first, middle, and last names. You may assume that each person’s name field will contain either two space-separated names (a first and last name) or three space-separated names (a first, middle, and last name). For students without a middle name, you should leave their `middle` name field as `NULL` in the table.

In `roster.py`, write a program that prints a list of students for a given house in alphabetical order.

1. Your program should accept the name of a house as a command-line argument.

- If the incorrect number of command-line arguments are provided, your program should print an error and exit.

2. Your program should query the `students` table in the `students.db` database for all of the students in the specified house.

3. Your program should then print out each student’s full name and birth year (formatted as, e.g., `Harry James Potter, born 1980` or `Luna Lovegood, born 1981`).

- Each student should be printed on their own line.
- Students should be ordered by last name. For students with the same last name, they should be ordered by first name.

## goal

- You need to do this in a virtual environment.
- Execute `wget https://cdn.cs50.net/2019/fall/psets/7/houses/houses.zip` to download a (compressed) ZIP file with this problem’s distribution.
- Execute `unzip houses.zip` to uncompress that file.
- Execute `cd houses` to change into that directory.
- Execute `ls`. You should see a `characters.csv` file and a `students.db` database.
- Implement a program `import.py`.
- Implement a program `roster.py`.

## Hints

- Recall that after you’ve imported SQL from cs50, you can set up a database connection using syntax like

```sql
db = SQL("sqlite:///students.db")
```

Then, you can use `db.execute` to execute SQL queries from inside of your Python script.

- Recall that when you call `db.execute` and perform a `SELECT` query, the return value will be a `list` of rows that are returned, where each row is represented by a Python `dict`.

This is a simple challenge, but if you want to take on a more difficult one, try it [on the website](https://cs50.harvard.edu/x/2020/psets/7/)

## reference code

The following content is for reference only. In order to have a better learning effect, please try to complete the exercises according to your own ideas.

<details>
<summary>reference</summary>

```Python
'''
import.py
'''
from sys import argv, exit
import csv
from cs50 import SQL

if len(argv) == 2 and argv[1].endswith(".csv"):
    db = SQL("sqlite:///students.db")

    file = open(argv[1], 'r')
    reader = csv.DictReader(file)

    for row in reader:
        names = row["name"]
        all_name = names.split()

        if len(all_name) == 2:
            f_name = all_name[0]
            l_name = all_name[1]
            db.execute("INSERT INTO students(first, middle, last, house, birth) VALUES(?,?,?,?,?)", f_name, None, l_name, row["house"], row["birth"])

        elif len(all_name) == 3:
            f_name = all_name[0]
            m_name = all_name[1]
            l_name = all_name[2]
            db.execute("INSERT INTO students(first, middle, last, house, birth) VALUES(?,?,?,?,?)", f_name, m_name, l_name, row["house"], row["birth"])

else:
    print("Error, missing command-line argument(.csv file)!")
    exit(1)


'''
roster.py
'''

from sys import argv, exit
import csv
from cs50 import SQL

if len(argv) == 2:
    db = SQL("sqlite:///students.db")
    all_rows = db.execute("SELECT DISTINCT first, middle, last, birth FROM students WHERE house = ? ORDER BY last, first", argv[1])
    for row in all_rows:
        if row["middle"] != None:
            middle = row["middle"].strip()
            print(row["first"] + " " + middle + " " + row["last"] + ", born " + str(row["birth"]))
        else:
            print(row["first"] + " " + row["last"] + ",born " + str(row["birth"]))

else:
    print("Error, missing command-line argument")
    exit(1)
```

</details>
