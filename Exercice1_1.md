
### Exercice 1.1 : Hello World

- Context \
  In this exercice you will learn how to create a new program in the ABAP GUI, you will understand what a transaction is, and you will get familiarised with the work-flow of writing and executing ABAP code.
- Problem \
  Acces the SAP code editor and make a 'Hello World' program. 

- Solution :
  <details>
    <summary>Click to show solution</summary>
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

  Don't forget to activate your source code IN EVERY STEP.
  Any modifications you make without activating your code won't take effect

  To execute your program, you can use the F8 key.

  Try writing this simple 'Hello World' program and execute it

  ```abap
  " You can write comments using double quotes
  " in ABAP every line of code has to end with a DOT (.)
  WRITE 'Hello World'.
  ```


  </details>