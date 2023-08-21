### Exercice 12 - Selection Screen Enhancement

- Context

    To make our application more user friendly, let's give the user the option to choose which selection parameter they would like to use.

- Problem

    Modify the selection screen to make it dynamically change according to a the selected options i.e if the user selects search by (name, surname), only that option should show up on the selection screen.

    - Hints 
        <details>
        <summary>Show hints</summary>

        * Instead of directly using **SELECT OPTIONS**, use **SELETCION SCREEN** in conjunction with **BLOCKS** and **RADIO-BUTTON GROUPS** 
        </details>

- Solution

    Let's start off by making a selection screen **box** that will contain all our radio buttons that control the selection parameters

    Let's this piece of code in our main program (don't forget to remove the **SELECT OPTIONS** portion of our code)

    ```abap
        SELECTION-SCREEN BEGIN OF BLOCK B1 WITH FRAME TITLE TEXT-B01.
        PARAMETERS : R_RAD1 TYPE BOOLE_D RADIOBUTTON GROUP G1 USER-COMMAND RAD DEFAULT 'X',
                    R_RAD2 TYPE BOOLE_D RADIOBUTTON GROUP G1,
                    R_RAD3 TYPE BOOLE_D RADIOBUTTON GROUP G1.

        SELECTION-SCREEN END OF BLOCK B1.
    ```	

    Let's now add the three selection screen boxes that will contain our selection options

    ```abap
    SELECTION-SCREEN BEGIN OF BLOCK B2 WITH FRAME TITLE TEXT-B02.
        SELECT-OPTIONS:
            S_ID FOR S_IDSAL MODIF ID ID1.
        SELECTION-SCREEN END OF BLOCK B2.

    SELECTION-SCREEN BEGIN OF BLOCK B3 WITH FRAME TITLE TEXT-B03.
        SELECT-OPTIONS:
            S_NOM FOR S_NOMSAL NO INTERVALS MODIF ID ID2,
            S_PRENOM FOR S_PRENOMSAL NO INTERVALS MODIF ID ID2.
        SELECTION-SCREEN END OF BLOCK B3.

    SELECTION-SCREEN BEGIN OF BLOCK B4 WITH FRAME TITLE TEXT-B04.
        SELECT-OPTIONS:
            S_DAT FOR S_DATNAISSANCESAL MODIF ID ID3.
        SELECTION-SCREEN END OF BLOCK B4.
    ```

    Finally, let's code the logic of our selection screen in a **AT SELECTION-SCREEN OUTPUT** statement

    ```abap
        AT SELECTION-SCREEN OUTPUT.
            LOOP AT SCREEN.
                IF ( R_RAD1 <> 'X' AND SCREEN-GROUP1 = 'ID1' )
                OR   ( R_RAD2 <> 'X' AND SCREEN-GROUP1 = 'ID2' )
                OR   ( R_RAD3 <> 'X' AND SCREEN-GROUP1 = 'ID3' ).
                SCREEN-ACTIVE = 0.
                SCREEN-INPUT = 0.
                MODIFY SCREEN.
                ENDIF.
            ENDLOOP.
    ```

    This is now the result of our new selection screen

    Selection_Screen_Move

    ![Selection_Screen_Move](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Selection_Screen_Move.gif?raw=true)

