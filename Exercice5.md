
### Exercice 5 : Data Edition

- Context \
  In this exercice you will learn how to add, remove and enable data editing on specific grids of your ALV report, and save these changes to your database table.
- Problem \
  Implement CRUD functionalities in your ALV grid
   - Hints 
     <details>
        <summary> Show Hints </summary>

        * Look into the fields available for each row of the GT_FCAT1 table that we created earlier. You can use CTRLK+SPACE to get a list of available fields.

        * Try to code your own versions of the ADD_ROW and DELETE_ROW forms. Make sure they don't allow the user to modify values that they shouldn't be able to, and make sure those values are handled automatically

        * 
     </details>

- Solution 
  <details>
    <summary>Click to show solution</summary>
    Let's loop over the fields we want to make editable on our alv grid and change their Edit value by adding this piece of code to our DISPLAY_DATA form

     ```abap
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
     ```

    We can now edit the Name fields of our alv report

    ![Editable](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Editable.png?raw=true)


    So far, only the display value of the field we are editing changes. Neither the internal table nor the database table are being changed.

    Let's first make the changes propagate to the internal table by calling the 
    **REGISTER_EDIT_EVENT** of our grid instance inside of our DISPLAY_DATA form.

    ```abap
        " register edit events on grid to propagate to internal table
    CALL METHOD GRID001->REGISTER_EDIT_EVENT
     EXPORTING
      I_EVENT_ID = CL_GUI_ALV_GRID=>MC_EVT_MODIFIED.
    ```

    Now that our edits are propagated to the internal table, we can add a 'Save' button to our screen that will persist the changes to the database table.
    (adding the button has already been covered, refer to [Exercice 4](https://github.com/Fabeure/ABAP-Initiation/blob/main/Exercice4.md))

    Lets now code the logic for our UPDATE form

    ```abap
        *&---------------------------------------------------------------------*
        *& Form update
        *&---------------------------------------------------------------------*
        *& updates db table after modifiying internal table via alv
        *& need to hit ENTER before pressing save
        *&---------------------------------------------------------------------*
        *& -->  p1        text
        *& <--  p2        text
        *&---------------------------------------------------------------------*
        FORM UPDATE.

        FIELD-SYMBOLS: <FS_DATA> LIKE LINE OF IT_SALARIES.

        LOOP AT IT_SALARIES ASSIGNING <FS_DATA>.
            MOVE-CORRESPONDING <FS_DATA> TO WA_SALARIES.
            MODIFY ZEXOSALARIES FROM WA_SALARIES.
        ENDLOOP.


        ENDFORM.
    ```

    For more information on field-symbols and how to use them, refer to: [ABAP CheatSheet - Dynamic Programming - Field Symbols](https://github.com/SAP-samples/abap-cheat-sheets/blob/main/06_Dynamic_Programming.md#field-symbols)

    Clicking on the save button after modifying entries will now persist the data to our database table.


    Before we move on to adding and deleting entries, let's first remove the default add and delete buttons that are on our screen using the **IT_TOOLBAR_EXCLUDING**
    parameter of our **SET_TABLE_FOR_FIRST_DISPLAY** method

    ```abap
      DATA : LT_EXCLUDE_FUNCTIONS TYPE UI_FUNCTIONS.

      " Add the default buttons you want to remove to the LT_EXCLUDE_FUNCTIONS table 
      APPEND CL_GUI_ALV_GRID=>MC_FC_LOC_INSERT_ROW TO LT_EXCLUDE_FUNCTIONS.
      APPEND CL_GUI_ALV_GRID=>MC_FC_LOC_APPEND_ROW TO LT_EXCLUDE_FUNCTIONS.
      APPEND CL_GUI_ALV_GRID=>MC_FC_LOC_PASTE TO LT_EXCLUDE_FUNCTIONS.
      APPEND CL_GUI_ALV_GRID=>MC_FC_LOC_PASTE_NEW_ROW TO LT_EXCLUDE_FUNCTIONS.

      " display alv report
            CALL METHOD GRID001->SET_TABLE_FOR_FIRST_DISPLAY
            EXPORTING
            *     I_BUFFER_ACTIVE               =
            *     I_BYPASSING_BUFFER            =
            *     I_CONSISTENCY_CHECK           =
            *     I_STRUCTURE_NAME              = 
            *     IS_VARIANT                    =
                  I_SAVE                        = 'A'
            *     I_DEFAULT                     = 'X'
                  IS_LAYOUT                     = GS_LAYOUT1
            *     IS_PRINT                      =
            *     IT_SPECIAL_GROUPS             =
                  IT_TOOLBAR_EXCLUDING          = LT_EXCLUDE_FUNCTIONS
            *     IT_HYPERLINK                  =
            *     IT_ALV_GRAPHICS               =
            *     IT_EXCEPT_QINFO               =
            *     IR_SALV_ADAPTER               =
            CHANGING
                  IT_OUTTAB                     = it_salaries
                  IT_FIELDCATALOG               = GT_FCAT1
            *     IT_SORT                       =
            *     IT_FILTER                     =
            EXCEPTIONS
                  INVALID_PARAMETER_COMBINATION = 1
                  PROGRAM_ERROR                 = 2
                  TOO_MANY_LINES                = 3
                  OTHERS                        = 4.
            IF SY-SUBRC <> 0.
            *     Implement suitable error handling here
            ENDIF.

    ```

    This is what our display will now look like

    ![Toolbar](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Toolbar.png?raw=true)


    Let's add our own Add Salarie and Remove Salarie toolbar keys, using the GUI STATUS


    ![Toolbar_Add](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Toolbar_Add.png?raw=true)

    Let's add these two keys to our USER COMMANDS module 

    ```abap
    *----------------------------------------------------------------------*
    ***INCLUDE ZMM_DOCUMENTATION_SABER_USEI01.
    *----------------------------------------------------------------------*
    *&---------------------------------------------------------------------*
    *&      Module  USER_COMMAND_0001  INPUT
    *&---------------------------------------------------------------------*
    *       text
    *----------------------------------------------------------------------*
    MODULE USER_COMMAND_0001 INPUT.
      CASE sy-ucomm.
      WHEN 'BACK'.
        LEAVE TO SCREEN 0.
      WHEN 'LEAVE'.
        LEAVE PROGRAM.
      WHEN 'EXIT'.
        LEAVE PROGRAM.
      WHEN 'SAVE'.
        PERFORM UPDATE.
      WHEN 'ADD'.
        PERFORM ADD.
      WHEN 'REMOVE'.
        PERFORM REMOVE.
      ENDCASE.
    ENDMODULE.	
    ```

    Let's add a few data declarations to our main source file that we will re-use throughout the rest of our forms
    Try understanding what these declarations will be useful for

    ```abap
      DATA : I_SELECTED_ROWS TYPE LVC_T_ROW,
        W_SELECTED_ROWS TYPE  LVC_S_ROW,
        WA              TYPE ZTLISTE_SALARIES_FULL.
    ```

    Next, let's code the logic for our ADD_ROW and REMOVE_ROW forms:

    ```abap
    *&---------------------------------------------------------------------*
    *& Form add_row
    *&---------------------------------------------------------------------*
    *& add row to alv display and then persist it to db
    *& ID_SAL and CREATED_ON automatically assigned
    *&---------------------------------------------------------------------*
    *& -->  p1        text
    *& <--  p2        text
    *&---------------------------------------------------------------------*
    FORM ADD_ROW.
      DATA : WA_NEW_ROW     TYPE ZEXOSALARIES,
            LV_MAX_ID      TYPE ZEXOSALARIES-ID_SAL,
            LV_MAX_ID_INT  TYPE INT8,
            LV_MAX_ID_CHAR TYPE CHAR30.

      SELECT MAX( ID_SAL ) INTO LV_MAX_ID FROM ZEXOSALARIES.

      LV_MAX_ID_INT = CONV I( LV_MAX_ID ).
      LV_MAX_ID_INT = LV_MAX_ID_INT + 1.


      LV_MAX_ID_CHAR = |{ LV_MAX_ID_INT }|.
      WA_NEW_ROW-ID_SAL = LV_MAX_ID_CHAR.
      WA_NEW_ROW-DATE_DE_NAISSANCE = SY-DATUM.
      APPEND WA_NEW_ROW TO IT_SALARIES.

    ENDFORM.



    *&---------------------------------------------------------------------*
    *& Form delete_row
    *&---------------------------------------------------------------------*
    *& delete selected row(s) from alv display and persist changes to db
    *&---------------------------------------------------------------------*
    *& -->  p1        text
    *& <--  p2        text
    *&---------------------------------------------------------------------*
    FORM DELETE_ROW.
      DATA: WA_SELECTED_ROW TYPE SY-TABIX,
            WA_MODIFIED     TYPE ZEXOSALARIES.

      CALL METHOD GRID001->GET_SELECTED_ROWS
        IMPORTING
          ET_INDEX_ROWS = I_SELECTED_ROWS.

      LOOP AT I_SELECTED_ROWS INTO WA_SELECTED_ROW.
        READ TABLE IT_SALARIES INTO WA INDEX WA_SELECTED_ROW.
        MOVE-CORRESPONDING WA TO WA_MODIFIED.

      DELETE FROM ZEXOSALARIES WHERE ID_SAL = WA_MODIFIED-ID_SAL.
      ENDLOOP.

    ENDFORM.
    ```		


    Finally, let's add a REFRESH form to our PBO module that will refresh our internal table that is displayed in oru alv report, so we can see our modifications in real time.


    ```abap
    *&---------------------------------------------------------------------*
    *& Form refresh
    *&---------------------------------------------------------------------*
    *& refresh data in internal tables AND database tables
    *& resource extensive but important for maintaining consistency
    *& when manipulating entries
    *&---------------------------------------------------------------------*
    *& -->  p1        text
    *& <--  p2        text
    *&---------------------------------------------------------------------*
    FORM REFRESH.

      IF NOT sy-ucomm = 'ADD'.
      PERFORM SELECT_SALARIES.
      PERFORM SELECT_SOCIETES.
      ENDIF.
      CALL METHOD GRID001->REFRESH_TABLE_DISPLAY.
    ENDFORM.
    ```

    Our pbo module should look something like this now

    ```abap
    MODULE STATUS_0001 OUTPUT.
    SET PF-STATUS 'STATUS001'.
    SET TITLEBAR 'SCREEN001'.

      IF CONTAINER001 IS INITIAL. " we add this condition to only create the container and grid once.
      CREATE OBJECT CONTAINER001
      EXPORTING
      CONTAINER_NAME = 'CONTAINER001'.

      CREATE OBJECT GRID001
      EXPORTING
      I_PARENT = CONTAINER001.
      PERFORM DISPLAY_DATA.
      ENDIF.

      PERFORM REFRESH.
    ENDMODULE.
    ```

    We can now add new entries, save them, and delete them through our ALV report.

  </details>