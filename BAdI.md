### Exercise 12 - BAdI

- Context

    In SAP ABAP programming, Business Add-Ins (BAdIs) provide a powerful mechanism to extend and customize the behavior of standard applications without modifying the original source code. This allows developers to tailor SAP applications to meet specific business requirements. BAdIs act as predefined hooks within the SAP codebase, enabling the insertion of custom code at specific execution points. In this section, we will explore the concept of BAdIs and address a common challenge in utilizing them effectively.

- Problem

    Implement a classic BAdI in a standard transaction of your choosing.

- Solution

    <details>
    <summary> Show solution </summary>

    The first step of implementing a BAdI is choosing a transaction that best fits the need for a BAdI.

    For our example, we will use the transaction **XK02**. This transaction is used to modify supplier data. We will use a BAdI to implement additional logic to this standard sap functionality.

    More specifically, we will make sure that when choosing to modify a vendor, we will run a few checks on the address to make sure it is coherent.

    The first step is to find the BAdI that best fits our needs. To do this, let's head over to the **SE24** transaction and find the **CL_EXITHANDLER**. This class will gives us a list of all the BAdIs that are called when executing a transaction.

    After displayuing the **CL_EXITHANDLER** class, lets display the **GET_INSTANCE** method
    
    ![Get_Instance](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Get_Instance.png?raw=true)

    Lets add a breakpoint to the **CALL METHOD cl_exithandler=>get_class_name_by_interface** method call

    ![Break](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Break.png?raw=true)

    Lets open the **XK02** transaction and see what happens 

    ![Exit_Name](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Exit_Name.png?raw=true)

    double clicking on the **exit_name** variable will allow us to get a list of the BAdIs being called 

    ![Name](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Name.png?raw=true)

    We can already see that we get a call for a BAdI named **ADDR_PRINTFORM_SHORT**

    > **_NOTE:_**  Pressing F8 and executing the code further will give you the rest of the BAdIs that are called. Multiple different BAdIs are usually called in a single transaction.

    Now that we have the BAdI name, let's make sure this is the BAdI that actually has access to the values we want to modify.

    We can do this by heading over to the **SE18** transaction, entering the BAdI name and investigating what variables are passed

    ![BAdI](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/BAdI.png?raw=true)

    Heading over to the **Interface** tab and looking at the methods provided, we can find a handy method called **ADDR_PRINTFORM_SHORT**

    ![Input](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Input.png?raw=true)


    > **_NOTE:_**  Sometimes the interface names in a BAdI are not very descriptif of it's actual functionality, so to be certain of what a method in a BAdI does, you can double click on the interface name to see the input and output parameters of a particular method. These will give you a clear idea of what that method does.
    ![Parametres](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Parametres.png?raw=true)


    Now that we have found the appropriate BAdI interface, we can add our custom implementation as follows

    ![Implement](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Implement.png?raw=true)

    We can now choose the interface method we want to implement.

    ![Method](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Method.png?raw=true)

    Double clicking on the method name will take us to the source code file of that method

    ![Code](https://github.com/Fabeure/ABAP-Initiation/blob/main/Images/Code.png?raw=true)

    We can now add our logic that will execute when saving any modifications to the address

    ```abap
        method IF_EX_ADDR_PRINTFORM_SHORT~ADDR_PRINTFORM_SHORT.

            if DESTINATION_COUNTRY_TEXT-LAND1 = 'TN' and RECEIVER_LANGUAGE <> 'F'.
            MESSAGE 'Language doit etre en francais' TYPE 'I'.

            ENDIF.
        endmethod.
    ```

    we now run a check to make sure the **RECEIVER_LANGUAGE** field is correctly set.


    


    </details>

    

