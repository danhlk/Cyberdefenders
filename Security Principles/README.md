> # Security Principles

## Summary
- [Summary](#summary)
  - [Task 2 - CIA](#task-2---cia)
  - [Task 3 - DAD](#task-3---dad)
  - [Task 4 - Fundamental Concepts of Security Models](#task-4---fundamental-concepts-of-security-models)
  - [Task 6 - ISO/IEC 19249](#task-6---isoiec-19249)

### Task 2 - CIA
1. Click on "View Site" and answer the five questions. What is the flag that you obtained at the end?<br>
    Complete all question in "View Site" to get the answer.<br>
    **Answer:** THM{CIA_TRIAD}

### Task 3 - DAD
1. The attacker managed to gain access to customer records and dumped them online. What is this attack?<br>
    > Disclosure: As in most modern countries, healthcare providers must maintain medical records’ confidentiality. As a result, if an attacker succeeds in stealing some of these medical records and dumping them online to be viewed publicly, the health care provider will incur a loss due to this data disclosure attack.

    **Answer:** Disclosure

1. A group of attackers were able to locate both the main and the backup power supply systems and switch them off. As a result, the whole network was shut down. What is this attack?<br>
    > Destruction/Denial: Consider the case where a medical facility has gone completely paperless. If an attacker manages to make the database systems unavailable, the facility will not be able to function properly. They can go back to paper temporarily; however, the patient records won’t be available. This denial attack would stall the whole facility.

    **Answer:** Destruction/Denial

### Task 4 - Fundamental Concepts of Security Models
1. Click on "View Site" and answer the four questions. What is the flag that you obtained at the end?<br>
    **Answer:** THM{SECURITY_MODELS}

### Task 6 - ISO/IEC 19249
1. Which principle are you applying when you turn off an insecure server that is not critical to the business?<br>
    > Attack Surface Minimisation: Every system has vulnerabilities that an attacker might use to compromise a system. Some vulnerabilities are known, while others are yet to be discovered. These vulnerabilities represent risks that we should aim to minimize. For example, in one of the steps to harden a Linux system, we would disable any service we don’t need.

    **Answer:** 2

1. Your company hired a new sales representative. Which principle are they applying when they tell you to give them access only to the company products and prices?<br>
    > Least Privilege: You can also phrase it informally as “need-to basis” or “need-to-know basis” as you answer the question, “who can access what?” The principle of least privilege teaches that you should provide the least amount of permissions for someone to carry out their task and nothing more. For example, if a user needs to be able to view a document, you should give them read rights without write rights.

    **Answer:** 1

1. While reading the code of an ATM, you noticed a huge chunk of code to handle unexpected situations such as network disconnection and power failure. Which principle are they applying?<br>
    > Preparing for Error and Exception Handling: Whenever we build a system, we should take into account that errors and exceptions do and will occur. For instance, in a shopping application, a customer might try to place an order for an out-of-stock item. A database might get overloaded and stop responding to a web application. This principle teaches that the systems should be designed to fail safe; for example, if a firewall crashes, it should block all traffic instead of allowing all traffic. Moreover, we should be careful that error messages don’t leak information that we consider confidential, such as dumping memory content that contains information related to other customers.

    **Answer:** 5
