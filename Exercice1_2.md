
### Exercice 1.2: Data retrieval and selection screens

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

  For more information on Data declarations, refer to:
    - https://github.com/SAP-samples/abap-cheat-sheets/blob/main/01_Internal_Tables.md
    - https://github.com/SAP-samples/abap-cheat-sheets/blob/main/02_Structures.md



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

   input no parametres will select all entries in the database table.


  ![Selection-Result](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Selection_Result.png?raw=true)

  
  Let's now make our selection screen look a little better by changing the display name of our selection variable 

  We can do this by adding a TEXT SYMBOL 

  ![Goto](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Goto.png?raw=true)

  ![Text-Symbol](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Text_Symbol.png?raw=true)

  When using the selection screen again, this is now the result

  ![Text-Symbol-Result](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Text_Symbol_Result.png?raw=true)


  lets modify our code to add a few more selection parameters:

  ```ABAP
   DATA : s_idsal TYPE ZEXOSALARIES-ID_SAL,
          s_nomsal TYPE ZEXOSALARIES-NOM_SALARIES,
          s_prenomsal TYPE ZEXOSALARIES-PRENOM_SALARIES,
          s_datnaissancesal TYPE ZEXOSALARIES-DATE_DE_NAISSANCE,
          it_salaries TYPE TABLE OF ZEXOSALARIES,
          wa_salaries TYPE ZEXOSALARIES.

  SELECT-OPTIONS :
     s_id for s_idsal,
     s_nom for s_nomsal NO INTERVALS,
     s_prenom for s_prenomsal NO INTERVALS,
     s_dat for s_datnaissancesal.


  Select *
  from ZEXOSALARIES
  into table it_salaries
  where ID_SAL IN S_ID
  AND NOM_SALARIES IN S_NOM
  AND PRENOM_SALARIES IN S_PRENOM
  AND DATE_DE_NAISSANCE IN S_DAT.

  

  LOOP AT it_salaries into wa_salaries.
    WRITE wa_salaries-ID_SAL.
    WRITE wa_salaries-NOM_SALARIES.
    WRITE /. "breakline to make result more readable
  ENDLOOP.
 

  ```
  ![Selection-Screen-Updated](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Selection_Screen_Updated.png?raw=true)


  </details>