### Exercice 3 : Code Organization


- Context \
In this exercice, we will organize our code into meaningful parts and seperate them to improve code readability and maintainability.
 
- Problem  

Create different include files to store significant chunks of code.

  

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

  We can now move our code into the include file.

  To make our code reusable, we can wrap it in a form 

  a form is the equivalent of functions in traditional programming.

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
  *& sort salaries by criteria, should use USING instead of hard coding
  *& in sort parametres
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
  ```

  </details>