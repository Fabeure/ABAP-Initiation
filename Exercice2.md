
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
          IT_SALARIES TYPE TABLE OF ZEXOSALARIES,
          WA_SALARIES TYPE ZEXOSALARIES.

  DATA : IT_SOCIETE TYPE TABLE OF T001,
         WA_SOCIETE TYPE T001.

  SELECT-OPTIONS :
     s_id for s_idsal,
     s_nom for s_nomsal NO INTERVALS,
     s_prenom for s_prenomsal NO INTERVALS,
     s_dat for s_datnaissancesal.


  Select *
  from ZEXOSALARIES
  into table IT_SALARIES
  where ID_SAL IN S_ID
  AND NOM_SALARIES IN S_NOM
  AND PRENOM_SALARIES IN S_PRENOM
  AND DATE_DE_NAISSANCE IN S_DAT.

  " sy-subrc will return 0 if atleast 1 result is found
  IF sy-subrc <> 0.
  MESSAGE text-E02 TYPE 'E'.
  ENDIF.


  IF IT_SALARIES[] IS NOT INITIAL.
    SELECT *
      FROM T001
      INTO TABLE IT_SOCIETE
      FOR ALL ENTRIES IN IT_SALARIES
      WHERE BUKRS = IT_SALARIES-SOCIETE.
    IF SY-SUBRC = 0.
      SORT IT_SOCIETES BY BUKRS.
      "ELSE.
      "MESSAGE TEXT-E02 TYPE 'E'.
    ENDIF.
  ENDIF.

  

  LOOP AT IT_SALARIES into WA_SALARIES.
    CLEAR WA_SOCIETE.
    READ  TABLE IT_SOCIETE INTO WA_SOCIETE WITH KEY BUKRS = WA_SALARIES-SOCIETE.
      WRITE WA_SALARIES-ID_SAL.
      WRITE WA_SALARIES-NOM_SALARIES.
      WRITE WA_SOCIETE-BUTXT.
      WRITE /. "breakline to make result more readable
  ENDLOOP.
  ```

  ![Societe-Result](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Societe_Result.png?raw=true)

    🔴🔴🔴🔴🔴
     Be careful when using FOR ALL ENTRIES in an empty internal table, as this will cause the system to select all entries in every database table which could cause a system crash or a memory overflow.
   🔴🔴🔴🔴🔴
  </details>
