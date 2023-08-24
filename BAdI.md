### Exercise 12 - BAdI

- Context

    In SAP ABAP programming, Business Add-Ins (BAdIs) provide a powerful mechanism to extend and customize the behavior of standard applications without modifying the original source code. This allows developers to tailor SAP applications to meet specific business requirements. BAdIs act as predefined hooks within the SAP codebase, enabling the insertion of custom code at specific execution points. In this section, we will explore the concept of BAdIs and address a common challenge in utilizing them effectively.

- Problem

    Implement a classic BAdI in a standard transaction of your choosing.

- Solution

    The first step of implementing a BAdI is choosing the transaction that best fits the need for a BAdI.

    For our example, we will use the transaction **MM01**. This transaction is used to create materials. We will use a BAdI to implement additional logic to this standard sap functionality.

    More specifically, we will make sure that when choosing to create a mateiral of the **Beverage Industry** of type **Beverages**, a "default" description and unit of measurement will be set. 

    

