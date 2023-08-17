
### Exercise 1.3 - Messages


- Context \
  In this exercice we will use the FOR ALL ENTRIES clause in ABAP to fetch the according company names for each employee
- Problem \
  for every employee selected, also show their respective company name that is stored in the T001 table.

  - Hints:
    <details>
      <summary> Click to show hints </summary>
      * take a look at the structure and contents of the T001 table.
      * use the FOR ALL ENTIRES clause to minimise the cost of the search
    </details> 

- Solution :
  <details>
    <summary>Click to show solution</summary>
    
    After taking  a look at the structure and contents of the T001 table, we can add a few data declarations in our main program and modify the selection code as follows:



  ```abap
  DATA : s_idsal TYPE ZEXOSALARIES-ID_SAL,
          s_nomsal TYPE ZEXOSALARIES-NOM_SALARIES,
          s_prenomsal TYPE ZEXOSALARIES-PRENOM_SALARIES,
          s_datnaissancesal TYPE ZEXOSALARIES-DATE_DE_NAISSANCE,
          it_salaries TYPE TABLE OF ZEXOSALARIES,
          wa_salaries TYPE ZEXOSALARIES.

  DATA : it_societe TYPE TABLE OF T001,
         wa_societe TYPE T001.

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


  IF it_salaries[] IS NOT INITIAL.
    SELECT *
      FROM T001
      INTO TABLE it_societe
      FOR ALL ENTRIES IN it_salaries
      WHERE BUKRS = it_salaries-SOCIETE.
    IF SY-SUBRC = 0.
      SORT it_societe BY BUKRS.
      "ELSE.
      "MESSAGE TEXT-E02 TYPE 'E'.
    ENDIF.
  ENDIF.

  

  LOOP AT it_salaries into wa_salaries.
    CLEAR wa_societe.
    READ  TABLE it_societe INTO wa_societe WITH KEY BUKRS = wa_salaries-SOCIETE.
      WRITE wa_salaries-ID_SAL.
      WRITE wa_salaries-NOM_SALARIES.
      WRITE wa_societe-BUTXT.
      WRITE /. "breakline to make result more readable
  ENDLOOP.
  ```

  ![Societe-Result](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Societe_Result.png?raw=true)


  </details>
