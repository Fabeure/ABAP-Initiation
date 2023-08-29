### Exercice 1.4 : Using the Debugger

- Context 

    Before moving forward, we're going to learn how to use the debugger. 

    The Debugger is a powerful tool that can help you, well, debug your code and understand exactly where something is going wrong when your code doesn't work as intended.

- Problem 

    Try fixing this piece of code using the debugger to find out where exactly the code breaks.

    ```abap
    DATA: num1 TYPE i VALUE 10,
        num2 TYPE i VALUE 5,
        result TYPE i.

    DO 5 TIMES.
    num1 = num1 - 1.
    num2 = num2 - 1.
    ENDDO.

    result = num1 / num2.

    WRITE: 'Result:', result.
    ```

    - Hint
        <details>
        <summary>Show hints</summary>

        - Analyse the code and try understanding what the **predicted** result is, and then run it to see the actual result. 

        </details>

- Solution

    Lets first run our code and see the result it gives us

    ![Dump](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Dump.png?raw=true)

    Our program encountered a **runtime error** and has given us a **dump**

    We can see that the short description of the dump tells us that we are divising by 0 which isnt allowed.

    Most of the time the error descriptions in dumps are not as straight forward, so to be sure we understand where our code is breaking, lets take a look at the debugger

    To do so, let's first add a breakpoint to our code by **double-clicking** to the left of the line where we want out code to stop and our debugger to take over.

    ![Breakpoint](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Breakpoint.png?raw=true)

    Running our code will now give us the following screen

    ![Debugger](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Debugger.png?raw=true)

    Let's analyse the screen and understand what the debugger is composed of

    The 4 buttons to the top left of the screen are the buttons used to **step into the code**.

    stepping into the code just means executing it line by line

    We can execute entire chunks of code using **F6**, or execute single lines using **F5**

    The window right below that is our code that we are executing. The small arrow indicates where the program is currently stopped. 

    We can **double-click on a variable name** is our code to show what value that variable holds.

    ![Variable](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Variable.png?raw=true)

    Next, let's **step through** our code line by line and see what is happening

    ![Step](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Step.gif?raw=true)


    We can now see that our **num2** variable reaches 0 and then we try divising by it.

    We can now fix the problem in different ways.

    The main takeaway here is that the debugger is a very powerful tool for understanding where code isn't working as expected, especially when a program doesn't dump but instead works but gives a wrong result.


    
