### Exercise 6 - Sending An Email

- Context

  We now have a working ALV report where we can view and modify our database entries. We now want to choose different entries and send them e-mails.

- Problem

  Add a send email button to your ALV report that allows you to send an email to
  the selected users.

  - Hints
    <details>
      <summary> Show Hints </summary>

    - First get the selected rows and fetch the email addresses from each row

    - Add a popup window that allows you to type in the body (and optionally the subject) of the email

    - Send the email via the **SO_NEW_DOCUMENT_SEND_API1** standard module (don't forget that you can check how a function is called using CTRL+F6)
</details>

- Solution
    <details>
    <summary> Show solution </summary>
    Lets first start off by adding our data declarations that we will need in our SEND_EMAIL form

  ```abap
  FORM SEND_EMAIL.

   DATA: W_SELECTED_ROW TYPE ZEXOSALARIES.

   DATA: WA_DOCDATA  LIKE SODOCCHGI1,
      I_CONTENTS  TYPE TABLE OF SOLISTI1 WITH HEADER LINE, " to store e-mail body
      I_RECEIVERS TYPE TABLE OF SOMLRECI1 WITH HEADER LINE, " receivers internal table
      WA_RECEIVER TYPE SOMLRECI1, " receivers single row
      TEXT        TYPE CATSXT_LONGTEXT_ITAB. " body of the e-mail

  ENDFORM.
  ```

  Next, let's loop over our selected users and grab their emails

  ```abap
  FORM SEND_EMAIL.

   DATA: WA_DOCDATA  LIKE SODOCCHGI1,
      I_CONTENTS  TYPE TABLE OF SOLISTI1 WITH HEADER LINE, " to store e-mail body
      I_RECEIVERS TYPE TABLE OF SOMLRECI1 WITH HEADER LINE, " receivers internal table
      WA_RECEIVER TYPE SOMLRECI1, " receivers single row
      TEXT        TYPE CATSXT_LONGTEXT_ITAB. " body of the e-mail

  ENDFORM.
  ```

  " loop over selected recipients and fetch emails,
  " then add them to the receivers table.

  ```abap
      CALL METHOD GV_GRID001->GET_SELECTED_ROWS
          IMPORTING
          ET_INDEX_ROWS = I_SELECTED_ROWS.
      LOOP AT I_SELECTED_ROWS INTO W_SELECTED_ROWS.
          READ TABLE IT_SALARIES INTO WA INDEX W_SELECTED_ROWS.
          WA_RECEIVER-RECEIVER = WA-ADRES_MAIL_SALARIES.
          WA_RECEIVER-REC_TYPE = 'U'.
          WA_RECEIVER-COM_TYPE = 'INT'.
          APPEND WA_RECEIVER TO I_RECEIVERS.
          CLEAR WA_RECEIVER.
      ENDLOOP.
      APPEND I_RECEIVERS.
  ```

  Now let's prompt the user to input the e-mail's body and subject (try finding a way to make the subject a user input aswell)

  ```abap
      " get email contents from popup and add it to the contents table.
      CALL FUNCTION 'CATSXT_SIMPLE_TEXT_EDITOR'
          EXPORTING
          IM_TITLE = 'test'
          CHANGING
          CH_TEXT  = TEXT.

      LOOP AT TEXT ASSIGNING FIELD-SYMBOL(<LINE>).
          APPEND <LINE> TO I_CONTENTS.
      ENDLOOP.
      APPEND I_CONTENTS.

      " set e-mail subject
      WA_DOCDATA-OBJ_DESCR = 'Email Subject'.
  ```

  Finally, let's call our **SO_NEW_DOCUMENT_SEND_API1** function to send the e-mail

  ```abap
      " send e-mail
      CALL FUNCTION 'SO_NEW_DOCUMENT_SEND_API1'
          EXPORTING
          DOCUMENT_DATA              = WA_DOCDATA
          DOCUMENT_TYPE              = 'RAW'
          COMMIT_WORK                = 'X'
          TABLES
          OBJECT_CONTENT             = I_CONTENTS
          RECEIVERS                  = I_RECEIVERS
          EXCEPTIONS
          TOO_MANY_RECEIVERS         = 1
          DOCUMENT_NOT_SENT          = 2
          DOCUMENT_TYPE_NOT_EXIST    = 3
          OPERATION_NO_AUTHORIZATION = 4
          PARAMETER_ERROR            = 5
          X_ERROR                    = 6
          ENQUEUE_ERROR              = 7.
      IF SY-SUBRC <> 0.
          " Handle error
          MESSAGE 'Fail' TYPE 'E' DISPLAY LIKE 'E'.
      ELSE.
          " Email sent successfully
          MESSAGE 'Success TYPE 'S' DISPLAY LIKE 'S'.
      ENDIF.
  ```

  We should now be able to select users, input our email body, and send them.

  ![Email](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Email.png?raw=true)

  To verify our send email functionality works properly, let's check through the SOST transaction

  ![SOST](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/SOST.png?raw=true)

  As you can see, our email went through and is awaiting processing.
    </details>
