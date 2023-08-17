
### Exercise 1.3 - Messages


- Context \
  In this exercice we will add custom text symbols and use them to handle different errors.
- Problem \
  Add a 'No data found' error message and make it appear when the selection process returns no results.

- Solution :
  <details>
    <summary>Click to show solution</summary>
    
    First we add our text symbol to the text elements
    
    ![No-Data](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/No_Data.png)


    now lets modify our code to account for the case where the selection process returns no results:

  ```abap
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

  " sy-subrc will return 0 if atleast 1 result is found
  IF sy-subrc <> 0.
  MESSAGE text-E02 TYPE 'E'.
  ENDIF.

  

  LOOP AT it_salaries into wa_salaries.
    WRITE wa_salaries-ID_SAL.
    WRITE wa_salaries-NOM_SALARIES.
    WRITE /. "breakline to make result more readable
  ENDLOOP.
  ```

  ![No-Data](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/No_Data_Result.png)


  </details>
