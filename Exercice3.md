### Exercice 3 : Code Organization


- Context \
In this exercice, we will organize our code into meaningful parts and seperate them to improve code readability and maintainability.
 
- Problem  

Create different include files to store significant chunks of your code.

  

- Solution :
  <details>
    <summary>Click to show Solution</summary>

  Now that our source code is getting bigger and more complicated, we need to split it into smaller significant parts. 

  Let's first take a look at our project structure using CTRL+SHIFT+F5

  ![Project](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Project.png?raw=true)

  We can now create a new INCLUDE file by right clicking on the root project folder -> create -> include

  ![Include](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Include.png?raw=true)

  Let's name our include file Z*I*MM_DOCUMENTATION_F01 

  The I stands for Include
  The F01 means this include will contain forms
  
  You will be prompted to add an include statement to your main program, do so.

  We can now move our code into the include file.

  To make our code even more reusable, we can wrap it in a form statement

  a form is the equivalent of a function in traditional programming.

  ```abap
  *&---------------------------------------------------------------------*
  *& Include          ZIMM_DOCUMENTATION_F01
  *&---------------------------------------------------------------------*

    *&---------------------------------------------------------------------*
  *& Form select_salaries
  *&---------------------------------------------------------------------*
  *& select salaries using criteria from select-options
  *&---------------------------------------------------------------------*
  *& -->  p1        text
  *& <--  p2        text
  *&---------------------------------------------------------------------*
  FORM SELECT_SALARIES.
    CLEAR WT_LISTE_SALARIES[].
    SELECT *
      FROM ZEXOSALARIES
      INTO TABLE WT_LISTE_SALARIES
      WHERE ID_SAL IN S_IDSAL
        AND NOM_SALARIES IN S_NOMSAL
        AND PRENOM_SALARIES IN S_PSAL
        AND DATE_DE_NAISSANCE IN S_DNSAL.
  *  IF SY-SUBRC <> 0.
  *    MESSAGE TEXT-E02 TYPE 'E'.
  *  ENDIF.
  ENDFORM.


  *&---------------------------------------------------------------------*
  *& Form sort_salaries_by
  *&---------------------------------------------------------------------*
  *& sort salaries by criteria
  *&---------------------------------------------------------------------*
  *& -->  p1        text
  *& <--  p2        text
  *&---------------------------------------------------------------------*
  FORM SORT_SALARIES_BY.
    SORT WT_LISTE_SALARIES BY ID_SAL ASCENDING.
  ENDFORM.

  *&---------------------------------------------------------------------*
  *& Form select_societes
  *&---------------------------------------------------------------------*
  *& select BUKRS AND BKTXT using FOR ALL ENTRIES IN to only read relevant data
  *&---------------------------------------------------------------------*
  *& -->  p1        text
  *& <--  p2        text
  *&---------------------------------------------------------------------*
  FORM SELECT_SOCIETES.
    IF WT_LISTE_SALARIES[] IS NOT INITIAL.
      SELECT *
        FROM T001
        INTO TABLE WT_SOCIETES
        FOR ALL ENTRIES IN WT_LISTE_SALARIES
        WHERE BUKRS = WT_LISTE_SALARIES-SOCIETE.
      IF SY-SUBRC = 0.
        SORT WT_SOCIETES BY BUKRS.
        "ELSE.
        "MESSAGE TEXT-E02 TYPE 'E'.
      ENDIF.
    ENDIF.
  ENDFORM.

  *&---------------------------------------------------------------------*
  *& Form write_salaries
  *&---------------------------------------------------------------------*
  *& write salaries and CoCode to standard output
  *&---------------------------------------------------------------------*
  *& -->  p1        text
  *& <--  p2        text
  *&---------------------------------------------------------------------*
  "FORM WRITE_SALARIES.
    LOOP AT WT_LISTE_SALARIES INTO WS_LINE_SALARIES.
      CLEAR WS_LINE_SOCIETES.
      READ  TABLE WT_SOCIETES INTO WS_LINE_SOCIETES WITH KEY BUKRS = WS_LINE_SALARIES-SOCIETE.
      WRITE WS_LINE_SALARIES-NOM_SALARIES.
      WRITE WS_LINE_SALARIES-PRENOM_SALARIES.
      WRITE WS_LINE_SALARIES-ADRES_MAIL_SALARIES.
      WRITE WS_LINE_SOCIETES-BUKRS.
      WRITE WS_LINE_SOCIETES-BUTXT.
      WRITE :/.
    ENDLOOP.
  ENDFORM.
  ```

  Forms can access global variables directly, or they can have input and output parameters.
  
  For more information on writing forms, refer to: NEED TO FIND USEFUL LINK

  Don't forget to apply text formatting using the Pretty Printer to make your code easier to read.

  Our main program should look something like this now :

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


    PERFORM SELECT_SALARIES.
    PERFORM SORT_SALARIES.
    PERFORM SELECT_SOCIETES.
    PERFORM WRITE_SALARIES.

    INCLUDE ZIMM_DOCUMENTATION_F01.
  ```
  our structure is now more organized and easier to maintain.


  ![Include](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Maintain.png?raw=true)

  </details>