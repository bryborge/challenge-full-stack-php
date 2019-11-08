# Peanut Butter

This is a code challenge that I took for a "Full Stack" developer position at a company who's name shall be referred to
as "Peanut Butter."  I have omitted anything that would trace back to the original company out of respect for their
hiring processes.

## Purpose

This repo aims to show my experience working through a real, company-issued code challenge.

## The Challenge

At Peanut Butter, a "Full Stack" developer is someone who is technically capable all the way up and down the stack, from
hosting and operations to project architecting.  The questions centered around the following concepts.

*   Networking/Ops
*   Object-Oriented PHP
*   SQL and Database Operations
*   Dependency management
*   String manipulation and encoding

The questions can be found below.  My answers are provided in the `answers.md` file of this repo.

### Questions

1.  What is an IP subnet? What kind of notation would you use to express them?

2.  Explain the differences between HEAD, GET and POST in the HTTP protocol.

3.  What is a certificate and how does it relate to HTTPS? What advantages does a CA signed certificate offer over a
    self-signed certificate (be specific)? What security benefits does HTTPS provide?

4.  Convert the following procedural PHP7 code into object oriented style code.

5.  You are given the following table in database `nasa`:

    `CREATE TABLE astronaut ( name text, weight int );`

    Write a simple PHP page that displays a form to enter the name and weight of an astronaut and on submission enters
    those values into the database. (Simple in this case means you do not have to be fancy, but you still need to be
    complete and correct. Assume the database is running on the local host and use user nasa and password nasa to
    connect). Use a DB abstraction layer to write your code. Example: PDO or WPDB. Use prepared statements.

6.  You have a group of people that are taking courses from a given list of courses. Write ANSI compliant SQL statements
    that will do the following:

    *   Create the tables that allow any person to take any course, but only allow them to sign up for any given course
        once.
    *   Show all courses taken by a given person.
    *   Show all people and the number of courses they are taking.

7.  Given the following string:

    ```php
    $str = 'drinking giving jogging 喝 喝 passing 制图 giving 跑步 吃';
    ```

    Using PHP, write a function that:

    1.  Moves all Chinese characters to the end of the string, reversing their current order.
    2.  Removes duplicate English words.
    3.  Returns the modified string

8.  Use a GitHub Branch as a Composer Dependency

    Explain how you would configure composer to use `https://github.com/peanut-butter/new-private-project` with the
    branch `bugfixes` in your project.

9.  Get "peanut-butter" working locally and attach a screenshot showing that you could do it. If there are problems,
    document and fix them.

10. This string contains information about this test. Can you determine what its significance is?

    ```php
    'aXA6MTA0LjIwMC4xMzIuMTg0IHRpbWU6MjAxOS0xMC0wNCAxNTo1MzoxMw=='
    ```
