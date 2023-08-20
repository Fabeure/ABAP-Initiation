### Exercise 7 - Formatting

- Contenxt \
    Before moving forward, lets make our screen displays more efficient by using up the entirety of the screen we made.
- Problem \ 
    Fit the data displayed on our ALV report to the screen. You shouldnt have to scroll left or right to see all the data.

    - Hints 
    <details>
    <summary>Show hints</summary>
    * Find out more about the fields available in the layout varibale we our assigning our ALV report by using CTRL+SPACE. See if you can find any fields relating to optimization.
    </details>

- Solution
    <details>
    <summary>Show solution</summary>
    Currently, our ALV report overflow and we have to scroll to view all the data displayed. 

    ![Scroll](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Scroll.png?raw=true)

    Let's go back to our DISPLAY_DATA form, and right before calling the SET_TABLE_FOR_FIRST_DISPLAY method, let's modify the **CWIDTH_OPT** field of our **GS_LAYOUT1** variable

    ```abap
        GS_LAYOUT1-CWIDTH_OPT = 'X'.
    ```

    Now if we run our program again, we should be able to see our entire internal table without having to scroll

    ![No_Scroll](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/No_Scroll.png?raw=true)

    </details>