### Exercice 4 : Display Optimization

- Context 
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
            D -- If the user wants to leave the screen -->E;
            D -- Program return to PROCESS BEFORE OUTPUT every time -->B;
            
    ```

    ##### Step 2: Designing a custom container

    ##### Step 3: linking our custom screen and container to our source code

    ##### Step 4: Preparing our data for display

    ##### Step 5: Displaying the data 

    </details>

