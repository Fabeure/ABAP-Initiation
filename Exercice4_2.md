### Exercise 4.5 – Displaying More Information

- Context \
    Now that we have our ALV report working, we want to display some extra data that doesnt exist in the **ZEXOSALARIES** database table.

- Problem \
    Show the **Age** and **Company Name** fields in our ALV report
    - Hints
      <details>
      <summary>Show hints</summary>

       * Create a new structure that can hold all the fields that we want to display and then use that new structure to create an internal table that you can pass to the display form we made earlier.
      </details>
- Solution 
    <details>
    <summary> Show solution </summary>
    Let's start by creating our new structure
    We're going to call it **ZTLISTE_SALARIES_FULL**

    ![Full](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Full.png?raw=true)

    Next, let's add a few data declarations for our new internal table that will hold all the relevant information we want to show in our ALV report


    ```abap
        DATA: WS_LISTE_SALARIES_FULL TYPE TABLE OF ZTLISTE_SALARIES_FULL,
              LS_SALARIES_FULL       TYPE ZTLISTE_SALARIES_FULL.
    ```

    Let's make a form responsible for filling our **WS_LISTE_SALARIES_FULL** internal table :

    ```abap
        *&---------------------------------------------------------------------*
        *& Form fill_full
        *&---------------------------------------------------------------------*
        *& populate wt_liste_salaries_full with data from both wt_liste_salaries
        *& and wt_societes
        *&---------------------------------------------------------------------*
        *& -->  p1        text
        *& <--  p2        text
        *&---------------------------------------------------------------------*
        FORM FILL_FULL.
        CLEAR WS_LISTE_SALARIES_FULL.
        DATA : LV_BUTXT TYPE T001-BUTXT,
                DAYS     TYPE NUM2,
                MONTHS   TYPE NUM2,
                YEARS    TYPE NUM2.

        LOOP AT IT_SALARIES INTO DATA(LS_SALARIES).
            CLEAR LS_SALARIES_FULL.

            LS_SALARIES_FULL-ID_SAL = LS_SALARIES-ID_SAL.
            LS_SALARIES_FULL-PRENOM_SALARIES = LS_SALARIES-PRENOM_SALARIES.
            LS_SALARIES_FULL-ADRES_MAIL_SALARIES = LS_SALARIES-ADRES_MAIL_SALARIES.
            LS_SALARIES_FULL-SOCIETE = LS_SALARIES-SOCIETE.
            LS_SALARIES_FULL-DATE_DE_NAISSANCE = LS_SALARIES-DATE_DE_NAISSANCE.
            LS_SALARIES_FULL-VILLE = LS_SALARIES-VILLE.
            LS_SALARIES_FULL-POSTAL = LS_SALARIES-POSTAL.
            LS_SALARIES_FULL-ADRESSE = LS_SALARIES-ADRESSE.
            LS_SALARIES_FULL-NOM_SALARIES = LS_SALARIES-NOM_SALARIES.

            CALL FUNCTION 'HRCM_TIME_PERIOD_CALCULATE'
            EXPORTING
                BEGDA         = LS_SALARIES_FULL-DATE_DE_NAISSANCE
                ENDDA         = SY-DATUM
            IMPORTING
                NOYRS         = YEARS
                NOMNS         = MONTHS
                NODYS         = DAYS
            EXCEPTIONS
                INVALID_DATES = 1
                OVERFLOW      = 2
                OTHERS        = 3.


            LS_SALARIES_FULL-AGE = YEARS.

            READ TABLE IT_SOCIETE INTO DATA(LS_SOCIETES) WITH KEY BUKRS = LS_SALARIES-SOCIETE.
            IF SY-SUBRC = 0.
            LV_BUTXT = LS_SOCIETES-BUTXT.
            LS_SALARIES_FULL-BUTXT = LV_BUTXT.
            ENDIF.

            APPEND LS_SALARIES_FULL TO WS_LISTE_SALARIES_FULL.
        ENDLOOP.
        ENDFORM.
    ```

    Finally, let's modify our DISPLAY_DATA form to use our new internal table and new structure.

    ```abap
    *&---------------------------------------------------------------------*
    *& Form display_data
    *&---------------------------------------------------------------------*
    *& populate internal tables from ZEXOSALARIES and T001
    *& display alv usign LVC_FIELDCATALOG_MERGE and GRID0100->SET_TABLE_FOR_FIRST_DISPLAY
    *& CTRL+F6 to get function template
    *&---------------------------------------------------------------------*
    *& -->  p1        text *& <--  p2        text
    *&---------------------------------------------------------------------*
    FORM DISPLAY_DATA .
                DATA : GT_FCAT1   TYPE LVC_T_FCAT,  "table to hold fields
                        GS_FCAT1   LIKE LINE OF GT_FCAT1,
                        GS_LAYOUT1 TYPE LVC_S_LAYO. "layout of our report
                DATA : LT_EXCLUDE_FUNCTIONS TYPE UI_FUNCTIONS.

                    " initial internal table data population from database tables
                PERFORM SELECT_SALARIES.

                PERFORM SORT_SALARIES.

                PERFORM SELECT_SOCIETES.

                PERFORM FILL_FULL.



            APPEND CL_GUI_ALV_GRID=>MC_FC_LOC_INSERT_ROW TO LT_EXCLUDE_FUNCTIONS.
            APPEND CL_GUI_ALV_GRID=>MC_FC_LOC_APPEND_ROW TO LT_EXCLUDE_FUNCTIONS.
            APPEND CL_GUI_ALV_GRID=>MC_FC_LOC_PASTE TO LT_EXCLUDE_FUNCTIONS.
            APPEND CL_GUI_ALV_GRID=>MC_FC_LOC_PASTE_NEW_ROW TO LT_EXCLUDE_FUNCTIONS.

                    " fetch all fields from final internal table and merge them in GT_FCAT1 table
                    CALL FUNCTION 'LVC_FIELDCATALOG_MERGE'
                    EXPORTING
                        I_STRUCTURE_NAME = 'ZTLISTE_SALARIES_FULL'
                        I_INTERNAL_TABNAME     = 'WS_LISTE_SALARIES_FULL'
                    CHANGING
                        CT_FIELDCAT            = GT_FCAT1
                    EXCEPTIONS
                        INCONSISTENT_INTERFACE = 1
                        PROGRAM_ERROR          = 2
                        OTHERS                 = 3.
                    IF SY-SUBRC <> 0.

                    ENDIF.

                    " loop over all fields in GT_FCAT1 and edit properties accordingly
                LOOP AT GT_FCAT1 INTO DATA(GS_FCAT_ENTRY)
                    WHERE FIELDNAME = 'ADRES_MAIL_SALARIES'
                    OR FIELDNAME = 'NOM_SALARIES'.
                    GS_FCAT_ENTRY-EDIT = 'X'.
                    MODIFY GT_FCAT1 FROM GS_FCAT_ENTRY TRANSPORTING EDIT
                    WHERE FIELDNAME = 'ADRES_MAIL_SALARIES'
                    OR FIELDNAME = 'NOM_SALARIES'.
                    EXIT.
                ENDLOOP.

                GS_LAYOUT1-CWIDTH_OPT = 'X'.
                    " display alv report
                    CALL METHOD GRID001->SET_TABLE_FOR_FIRST_DISPLAY
                    EXPORTING

                        I_SAVE                        = 'A'
                        IS_LAYOUT                     = GS_LAYOUT1
                        IT_TOOLBAR_EXCLUDING          = LT_EXCLUDE_FUNCTIONS

                    CHANGING
                        IT_OUTTAB                     = WS_LISTE_SALARIES_FULL
                        IT_FIELDCATALOG               = GT_FCAT1

                    EXCEPTIONS
                        INVALID_PARAMETER_COMBINATION = 1
                        PROGRAM_ERROR                 = 2
                        TOO_MANY_LINES                = 3
                        OTHERS                        = 4.
                    IF SY-SUBRC <> 0.

                    ENDIF.
                    " register edit events on grid to propagate to internal table
            CALL METHOD GRID001->REGISTER_EDIT_EVENT
                EXPORTING
                I_EVENT_ID = CL_GUI_ALV_GRID=>MC_EVT_MODIFIED.

            ENDFORM.
    ```

    Our final ALV report should look like this now


    ![Full_Report](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Full_Report.png?raw=true)

    </details>


