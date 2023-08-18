
### Exercice 5 : Data Edition

- Context \
  In this exercice you will learn how to add, remove and enable data editing on specific grids of your ALV report, and save these changes to your database table.
- Problem \
  Implement CRUD functionalities in your ALV grid
   - Hints 
     <details>
        <summary> Show Hints </summary>
        * Look into the fields available for each row of the GT_FCAT1 table that we created earlier. You can use CTRLK+SPACE to get a list of available fields.

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
    (adding the button has already been covered, refer to [Exercice4](https://github.com/Fabeure/ABAP-Initiation/blob/main/Exercice5.md))


  </details>