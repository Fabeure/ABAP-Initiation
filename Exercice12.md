### Exercise 12 - Writing a File

- Context \
    We now want to be able to save our ALV report to our local machine in different formats.

- Problem 

    Save the ALV report as a **CSV** file on your local machine

    - Hints 
        <details>
        <summary>Show hints</summary>
        
       * Use the **GUI_DOWNLOAD** function module and the **FILE_SAVE_DIALOG** method of the **CL_GUI_FRONTEND_SERVICES** class to save your ALV report to your local machine. 
        </details>


 
        
        

- Solution
    <details>
    <summary>Show solution</summary>

    Let's start off with a few data declarations that will hold the relevant information for converting and saving our ALV report

    ```abap
    FORM CONVERT_CSV.
        DATA :
            IT_DOWNLOADABLE TYPE TABLE OF ZTLISTE_SALARIES_FULL,
            LD_FILENAME     TYPE STRING,
            LD_PATH         TYPE STRING,
            LD_FULLPATH     TYPE STRING,
            LD_RESULT       TYPE I.
    ENDFORM.
    ```
    Now lets fetch the rows we want to save (if no rows are selected, we save all of them)

    ```abap
    FORM CONVERT_CSV.
    DATA :
        IT_DOWNLOADABLE TYPE TABLE OF ZTLISTE_SALARIES_FULL,
        LD_FILENAME     TYPE STRING,
        LD_PATH         TYPE STRING,
        LD_FULLPATH     TYPE STRING,
        LD_RESULT       TYPE I.



    " get selected rows and append them to internal table that will be downloaded
    CALL METHOD GRID0100->GET_SELECTED_ROWS
        IMPORTING
        ET_INDEX_ROWS = I_SELECTED_ROWS.
    LOOP AT I_SELECTED_ROWS INTO W_SELECTED_ROWS.
        READ TABLE WS_LISTE_SALARIES_FULL INTO WA INDEX W_SELECTED_ROWS.
        APPEND WA TO IT_DOWNLOADABLE.
    ENDLOOP.

    " if no rows are selected, append every row to IT_DOWNLOADABLE
    IF IT_DOWNLOADABLE IS INITIAL.
        IT_DOWNLOADABLE[] = WS_LISTE_SALARIES_FULL[].
    ENDIF.
    ENDFORM.
    ```

    Now let's call the **FILE_SAVE_DIALOG** method to choose where we will save the file

    ```abap
        " display file save window and fetch file name and file path
    CALL METHOD CL_GUI_FRONTEND_SERVICES=>FILE_SAVE_DIALOG
        EXPORTING
    *     window_title      = ' '
        DEFAULT_EXTENSION = 'CSV'
        DEFAULT_FILE_NAME = 'lISTE_SALARIES'
        INITIAL_DIRECTORY = 'c:\'
        CHANGING
        FILENAME          = LD_FILENAME
        PATH              = LD_PATH
        FULLPATH          = LD_FULLPATH
        USER_ACTION       = LD_RESULT.
    ```

    Finally, let's save the file in CSV format

    ```abap
    " save file using full_path = file_path + file_name
    CALL FUNCTION 'GUI_DOWNLOAD'
        EXPORTING
        FILENAME              = LD_FULLPATH  " File name including path, give CSV as extention of the file
        FILETYPE              = 'ASC'
        WRITE_FIELD_SEPARATOR = ','    " Provide comma as separator
        TABLES
        DATA_TAB              = IT_DOWNLOADABLE     " Pass the Output internal table
        EXCEPTIONS
        OTHERS                = 22.
    IF SY-SUBRC <> 0.
    * MESSAGE ID SY-MSGID TYPE SY-MSGTY NUMBER SY-MSGNO
    *         WITH SY-MSGV1 SY-MSGV2 SY-MSGV3 SY-MSGV4.
    ENDIF.
    ```
    We can now select rows and choose where to save them on our local storage


    ![Save](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Save.png?raw=true)

    ![CSV](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/CSV.png?raw=true)

    

    </details>



