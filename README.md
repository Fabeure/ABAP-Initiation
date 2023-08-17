# ABAP Initiation: A beginner's guide to programming in ABAP

1. [**Introduction**](#introduction)
2. [**Pre-requesites**](#pre-requesites)

3. **Exercises**

   - **Exercise 1.1 – Program Creation**

     - Context
     - Solution

   - **Exercise 1.2 – Selection Screen**

     - Objectives
     - Development Standards
     - Useful Information
     - Results

   - **Exercise 1.3 - Messages**

     - Objectives
     - Useful Information
     - Results

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

### Exercice 1 : Hello World

- Context \
  In this exercice you will learn how to create a new program in the ABAP GUI, you will understand what a transaction is, and you will get familiarised with the work-flow of writing and executing ABAP code.
- Problem \
  Acces the SAP code editor and make a 'Hello World' program.
- Solution :

After getting access to an SAP system and opening the SAP GUI, you should be presented with this screen after logging in:

![SAP GUI Home Screen](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Home_Screen.png?raw=true)


You can now access the ABAP code editor through the transaction SE38

Transactions are just executable programs that are included in the SAP ABAP GUI. We will be using different transactions for different purposes.

you should now be able to create or modify programs.

![SE38 program creation]()

Let's create a new program and name it *Z*MM_INITIATION

**NOTE**: The Z prefix in sap standards means that this program is a custom user made program, and not a standard ABAP program.\
The MM is an abreviation for SAP modules.