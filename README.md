# ABAP Initiation: A beginner's guide to programming in ABAP

1. [**Introduction**](#introduction)
2. [**Pre-requesites**](#pre-requesites)

3. [**Exercises**](#exercises)

   - [**Exercise 1.1 – Program Creation**](#exercice-11--hello-world)
     - Context
     - Problem
     - Solution
     

   - **Exercise 1.2 – Selection Screen**

     - Context
     - Problem
     - Solution

   - **Exercise 1.3 - Messages**

     - Context
     - Problem
     - Solution

   - **Exercise 2 – For all entries**

     - Objectives
     - Technical Information
     - Useful Information
     - Results

   - **Exercise 3 – Code Organization**

     - Objectives

   - **Exercise 4 – Display Optimization**

     - Objectives
     - Useful Information
     - Results
     - Additional Notes

   - **Exercise 5 - Data Edition**

     - Objectives
     - ALV
     - Data Update
     - Adding / Deleting Employees

   - **Exercise 6 - Sending Email**

     - Objectives
     - Useful Elements

   - **Exercise 7 - Formatting**

     - Objectives

   - **Exercise 8 - Data Management**

     - Objectives
     - Constraints

   - **Exercise 9 – Hotspot**

     - Objectives

   - **Exercise 10 - PDF Printing**

     - Objectives
     - Useful Information
     - Links

   - **Exercise 11 - Sending PDF Document by Email**

     - Objectives
     - Useful Information

   - **Exercise 12- Selection Screen Enhancement**

     - Objectives
     - Useful Information

   - **Exercise 13 - Writing a File**

     - Objectives

   - **Exercise 14 - Extended Program Control**

4. **Glossary**

   - Definitions of important terms used throughout the document.

5. **SAP Instructions**

   - Instructions on how to perform SAP-related tasks mentioned in the document.

6. **Useful Transactions**

   - List of SAP transactions that are useful for the exercises.

7. **Useful Links**
   - Additional resources or links for further learning and reference.

## Introduction

This project will serve as an initiation to programming in ABAP and is aimed toward absolute beginners. \
\
The project will be split into several small tasks, each of which will get progressively less guided.

Hints, explanations and full code solutions with respective outputs will be provided for every exercice.

You will learn the basics of data manipulation, visualization, conversion, and many more functionalities throughout this exercice.

## Pre-requesites

You'll first need access to an SAP system via the ABAP GUI, which will be provided to you.

This guide will only assume you have an understanding of basic programming concepts such as loops, conditionals, basic SQL, and object oriented programming (OO).


## Exercices


### Exercice 1.1 : Hello World

- Context \
  In this exercice you will learn how to create a new program in the ABAP GUI, you will understand what a transaction is, and you will get familiarised with the work-flow of writing and executing ABAP code.
- Problem \
  Acces the SAP code editor and make a 'Hello World' program. 

- Solution :
  <details>
    <summary>Clickt to show solution</summary>
  After getting access to an SAP system and opening the SAP GUI, you should be presented with this screen after logging in:

  ![SAP GUI Home Screen](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Home_Screen.png?raw=true)


  You can now access the ABAP code editor through the transaction SE38

  Transactions are just executable programs that are included in the SAP ABAP GUI. We will be using different transactions for different purposes.

  you should now be able to create, view, or modify programs.

  ![SE38 program creation](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Code_Editor.png?raw=true)

  Let's create a new program and name it *Z*MM_INITIATION

  **NOTE**: The Z prefix in sap standards means that this program is a custom user made program, and not a standard ABAP program.\
  The MM is an abreviation for SAP modules.

  Give your program a title, a set the type as EXECUTABLE PROGRAM then hit save.

  ![Create Program](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Create_Program.png?raw=true)

  You should save the program in the $TMP package so that your program is saved locally and is only accessible to in a development environment.

  ![Save Program](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Save_Program.png?raw=true)

  We now have an empty program and can start coding. 
  First we have to understand how executing programs in ABAP works.

  ![Empty Program](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Empty_Program.png?raw=true)

  Unlike most languages, ABAP programs are not standalone files that are executed seperatly, instead, ABAP programs exist as objects in the SAP server. 

  To create an object, you simply have to activate your source code using CTRL+F3

  To execute your program, you can use the F8 key.

  Try writing this simple 'Hello World' program and execute it

  ```abap
  " You can write comments using double quotes
  " in ABAP every line of code has to end with a DOT (.)
  WRITE 'Hello World'.
  ```


  </details>


### Exercice 1.2 : Data retrieval and selection screens

- Context \
in this exercice, we will learn how to retrieve data from a database, allow the user to input selection criteria, and show the results in the standard output.
 
- Problem  

Using the **ZEXOSALARIES** database table, allow the user to select employees using a range of IDs.

- Hints:
  <details>
  <summary> show hints </summary>

  - Use the transaction SE11 to find out the structure of the ZEXOSALARIES table and its corresponding fields

  - Use the transaction SE16n to view the contents of the ZEXOSALARIES table

  - Declare the necessary data fields to fetch the rows of the ZEXOSALARIES table

  - Use SELECTION-SCREEN to allow the user to input search parametres
  </details>
  

- Solution :
  <details>
    <summary>Click to show Solution</summary>

  The first step is to understand the structure of the ZEXOSALARIES table. We can use the SE11 transaction to achieve this.

  ![Structure](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Structure.png?raw=true)

  As you can see, the ZEXOSALARIES table is comprised of multiple fields each of which has its own data type. 

  We can now declare the correponding data types that will allows us to fetch the data from the database table into our program and write the result to the standard output.
  ```abap
  "use a DATA clause to declare data.
  DATA :   s_idsal TYPE ZEXOSALARIES-ID_SAL
           it_salaries TYPE TABLE OF ZEXOSALARIES,
           wa_salaries TYPE ZEXOSALARIES.



  " we can select all entries from the ZEXOSALARIES table using the selection parametre S_IDSAL into our internal table it_salaries
  Select *
  from ZEXOSALARIES
  into table it_salaries
  where ID_SAL in S_IDSAL.



  " now we can loop over our internal table row by row and write the output to the standard output.
  LOOP AT it_salaries into wa_salaries.
    WRITE wa_salaries-ID_SAL.
    WRITE wa_salaries-NOM_SALARIES.
    WRITE /. "breakline to make result more readable
  ENDLOOP.

  ```
  Internal tables are basically an image of your database table that exist only in runtime (when executing your program). These internal tables are used to fetch data from the database into your program. 

  Working areas are like the "rows" of the internal table. 


  At this point, our program wont give us any results since we havent set our selection parametre S_IDSAL yet.

  to do this, lets use a selection screen

  A selection screen is a screen where the user can input parametres that will be used in the rest of the program.

  ```abap
  DATA :   s_idsal TYPE ZEXOSALARIES-ID_SAL
           it_salaries TYPE TABLE OF ZEXOSALARIES,
           wa_salaries TYPE ZEXOSALARIES.

  SELECT-OPTIONS :
     s_id for s_idsal.

  Select *
  from ZEXOSALARIES
  into table it_salaries
  where ID_SAL in S_IDSAL.

  

  LOOP AT it_salaries into wa_salaries.
    WRITE wa_salaries-ID_SAL.
    WRITE wa_salaries-NOM_SALARIES.
    WRITE /. "breakline to make result more readable
  ENDLOOP.

  ```

  ![Selection-Screen](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Selection_Screen.png?raw=true)

  ![Selection-Result](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Selection_Result.png?raw=true)

  
      
  </details>