### Exercice 4 : Display Optimization


- Context \
Now that we have selected our entires based on different criteria, it's time to make our display optimized.
 
- Problem  

 Create an ALV report to diisplay data fetched from the previous exercices. 

   - Hints 
        <details>
        <summary> Click to show hints </summary>

        * Create a dedicated screen and design it's components for displaying our ALV report in a similar way that we created our include files

        * "link" your custom screen with your source code using the class **cl_gui_custom_container** and **cl_gui_alv_grid**
        
        * find a way to display your data in an ALV report after calling the screen you made.
        </details>

        
- Solution
    <details>
    <summary> Click to show solution </summary>
    
    ##### Step 1: Creating the custom screen 

    Let's start off by creating our new screen that will hold the ALV report. It can be created in a similar way we created our Include files: through the repository browser.

    ![Screen_Create](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Screen_Create.png?raw=true)

    Give your screen a unique number. This number will be used to access the screen in your source code.

    ![Screen_Number](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Screen_Number.png?raw=true)

    Now that our screen has been created, we need to code it's **flow logic**.

    The flow logic of the screen is simply how the screen will operate. We can visualize it using this flow chart

    ``` mermaid
      graph TD;
            A[Call Screen 001]-->B[PROCESS BEFORE OUTPUT];
            B[PROCESS BEFORE OUTPUT]-->C[WAIT FOR USER INTERACTION];
            C[WAIT FOR USER INTERACTION]-->D[PROCESS AFTER INPUT];
            D -->E[EXIT SCREEN];
            D -- Program returns to PROCESS BEFORE OUTPUT every time -->B;
            
    ```

    In summary, when you first call your screen, the PROCESS BEFORE OUTPUT code will be executed.
    Then, the screen will wait for user input. 
    After the user input comes in, the PROCESS AFTER INPUT code will be executed and then the screen loops back to the PROCESS BEFORE OUTPUT code. 
    This will keep happening until the user decides to exit the screen.

    Back to our source code, lets un-comment our PROCESS BEFORE OUTPUT and PROCESS AFTER INPUT modules.

    ![Uncomment](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Uncomment.png?raw=true)

    Double on each module and create it (you will be prompted to create a new object for each module).

    Your project structure should look something like this now: 

    ![Modules](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Modules.png?raw=true)

    Let's first take a look at our STATUS module (This is the module that will be executed every time you do an action once the screen is called)

    ![status](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/status.png?raw=true)  

    Let's uncomment the status and and title bar and create both object by **double clicking on their names**

    Your project structure should look like this now

    ![Status_Uncommented](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Status_Uncommented.png?raw=true)


    The title bar is the title that will be show on top of your screen after you call it, and it doesnt affect the contents of the screen.

    The GUI status represents the buttons and shortcuts the user will be able to see and use after calling the screen. 


    Let's setup a few usefull keys so the user can navigate in and out of the screen.

    First add the keys in the Function Keys tab of the GUI STATUS 

    ![Keys](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Keys.png?raw=true)

    Now let's move over to the **USER COMMANDS** module. This is where we will code the logic of our keys


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
      CASE sy-ucomm. "the SY-UCOMM variable allows us to access which user command has been sent to the system
      WHEN 'BACK'.
      LEAVE TO SCREEN 0.
      WHEN 'LEAVE'.
      LEAVE PROGRAM.
      WHEN 'EXIT'.
      LEAVE PROGRAM.
      ENDCASE.
      ENDMODULE.
    ```	



    We can now call our screen from our main and take a look at it

    ``` abap
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

      CALL SCREEN 001.

      INCLUDE ZIMM_DOCUMENTATION_F01.
      INCLUDE zmm_documentation_saber_stao01.
      INCLUDE zmm_documentation_saber_usei01.
    ```

    We now have an empty screen. 

    ![Screen_Empty](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Screen_Empty.png?raw=true)



    ##### Step 2: Designing a custom container

    ##### Step 3: linking our custom screen and container to our source code

    ##### Step 4: Preparing our data for display

    ##### Step 5: Displaying the data 

    </details>

