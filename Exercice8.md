### Exercise 8 – Hotspot

- Context \
Now that we can see more relevant information in our ALV report, let's make the **Company Name** field clickable to display extra information

- Problem \
Add a **Hotspot** to the **Company Name** field
    - Hints
        <details>
        <summary>Show hints</summary>

        * Enable the Hotspot field of the **Company Name** column

        * Make a class that will handle the click event and implement it

        </details>

- Solution 
    <details>
    <summary>Show solution</summary>

    First, let's enable the hotspot click field of our **Company Name** column in our **DISPLAY_DATA** form

    ```abap
    " enable hotspot for company name field
    LOOP AT GT_FCAT1 INTO GS_FCAT1 WHERE FIELDNAME = 'BUTXT'.
        GS_FCAT1-HOTSPOT = 'X'.
        MODIFY GT_FCAT1 FROM GS_FCAT1 TRANSPORTING HOTSPOT WHERE FIELDNAME = 'BUTXT'.
    ENDLOOP.
    ```
    </details>

    Our **Company Name** column should now be clickable

  ![Clickable](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Clickable.png?raw=true)

  But nothing happens when we click on a company's name.
  We need to create a class that will handle these hotspot clicks.


  Let's start off by declaring the class and the instance reference in our main program


  ```abap
    CLASS LCL_EVENT_HANDLER DEFINITION FINAL.
    PUBLIC SECTION.

        METHODS:HANDLE_HOTSPOT_CLICK
            FOR EVENT HOTSPOT_CLICK OF CL_GUI_ALV_GRID.


    ENDCLASS. "lcl_event_handler DEFINITION
    DATA: GR_EVENT_HANDLER TYPE REF TO LCL_EVENT_HANDLER.

  ```	

  Let's now implement this class in our PBO module

  ```abap
    *----------------------------------
    * Event Handler Class implimentation
    *
    CLASS LCL_EVENT_HANDLER IMPLEMENTATION.

    METHOD HANDLE_HOTSPOT_CLICK.
        MESSAGE 'CLICKED' TYPE 'S'. "you can put any treatment here
        "CALL TRANSACTION 'ZEXO_DISPLAY_BUKRS_ALV'.
    ENDMETHOD. "handle_hotspot_click


    ENDCLASS. "lcl_event_handler IMPLEMENTATION
  ```

  Finally, let's add the hotspot click handler to our **GRID001** instance in our **DISPLAY_DATA** form


  ```abap
    CREATE OBJECT GR_EVENT_HANDLER .
    SET HANDLER GR_EVENT_HANDLER->HANDLE_HOTSPOT_CLICK FOR GRID0100.
  ```

  Our code should now work and when clicking on a company's game we get the following result

  ![Clicked](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Clicked.png?raw=true)







