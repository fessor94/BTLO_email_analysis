# Blue Team Lab Email Analysis Challenge

## Scenario
![Screenshot 2025-07-25 120919](https://github.com/user-attachments/assets/cabea8c3-2969-4096-adb7-5977736b768d)

## Email Authentication Status Check

- **SPF** - is set to FAIL, meaning the domain **microapple.com** does not permit the mail server of IP address **93.99.104.210**

![spf](https://github.com/user-attachments/assets/5a78514e-a7df-4ce7-8485-4b654ac0adf5)

## Other Headers

![r_and_f_headers](https://github.com/user-attachments/assets/7307eac0-ecf9-445d-854a-e09cf31d0a7e)

- The **Reply-To**  header is not the same as the **(@pashter.com)** is different to the **FROM** header of **@microapple.com**  which makes it suspicious

- There is also a discrapancy with **Time** and **Date**
    - The recipeint **Received** date is:
       Mon, 25 Jan 2021 22:41:10 -0800 (PST) convert to UTC we add 8 hours which is = 06:41:18 Jan 26, 2021

    - Sender time is Tue, 26 Jan 2021 01:41:18 -0500 (EST) we add 5 hours which is = 06:41:18 Jan 26, 2021 UTC
    - **High Suspicion of Forgery/Manipulation:** Since the UTC timestamps are exactly the same, it strongly suggests that the "Received" header from the sender's server has been forged or manipulated.


- Also checking the sender's **Received** header we can see that the malicious actor used **(emkei.cz)** email service which is a fake email sender service

![sender_mailserver](https://github.com/user-attachments/assets/6fa66e0d-b75c-4606-85c8-8464270b0c05)

![sender_mailserver1](https://github.com/user-attachments/assets/35480da2-d494-4a0f-b605-528b9a89b55e)

## Decoding base64

![base64](https://github.com/user-attachments/assets/d9a192c0-6a64-4295-a029-a2d0952d644b)

We see that when we decode the base64, it has a instructions telling us to solve a puzzple in an attachment. (which is also suspicious)

- we then save this file 

- Scrolling down the email we get the **Content-Type** header set to **PDF** which we cannot be sure, so we copy the base64 to CyberChef again.

![content_type](https://github.com/user-attachments/assets/f5a8d0ca-9972-4f1f-94ee-ed43cf7c9cf4)

We then convert the decoded base64 to **hex** to check for the **File Signature**

![hex](https://github.com/user-attachments/assets/c65041ad-cdd1-4f93-8727-b3fd92b3a5e1)

-Using **Gary Kessler's File Signature** we see that it is not a PDF but a ZIP file with a couple of files in it

![file_sig](https://github.com/user-attachments/assets/90d64b39-2ab0-496a-93dc-992d2d453c6a)

- We can then save the **ZIP** file to our virtual machine for further analysis
- Make sure the **Hidden Files** tab on windows is ticked to make sure our extracted ZIP file shows all the files inside

![hidden](https://github.com/user-attachments/assets/c58b3ee6-d99a-4b9c-bc4f-bfc0267d8d07)

- There is a file without a file extension inside the extracted **ZIP**, we can drag an drop the file inside **CyberChef** or **HxD** to get the file signature. After getting the file signature we realise that it a **.JPEG** file

![jpeg1](https://github.com/user-attachments/assets/630a6e18-969e-4c28-ba98-850367c510a4)

![jpeg2](https://github.com/user-attachments/assets/10b31c7f-f65e-43a5-a337-2f8418a6af7b)

- The other files are a **PDF** and **EXCEL** files

![sheet1](https://github.com/user-attachments/assets/c326cf0b-4a57-4be6-a2c0-706775a089a3)
 
- **There is also a Tool Call **Exiftools** that we can all use to check the metadata of all the files inside the **ZIP** folder.**

![exif](https://github.com/user-attachments/assets/56b04cd8-3fe2-467d-8351-0cb7c6654c1a)

- Using the **HxD** application we can drag and drop to see the **files signature** in hex, we then check the signature in **Gary Kessler's File Signature** to see its real .extension

![zipped_files](https://github.com/user-attachments/assets/a45a0dd0-5aa3-4d36-b95b-c9f79507ff95)

- We can also upload the file to CyberChef to see the file extension

![pdf](https://github.com/user-attachments/assets/0a9a4914-2dbe-4f23-9785-921714230425)


# **in Conclusion**

- This email analysis strongly indicates a phishing attempt due to multiple critical red flags. Key indicators include: an SPF authentication FAIL, mismatched Reply-To and FROM headers, and identical UTC timestamps for both sender and recipient "Received" headers, suggesting header forgery. Further, the sender used a known fake email service (emkei.cz), and a forensic examination of the attached file, despite being labeled as a PDF, revealed its true nature as a ZIP file through signature analysis. These combined elements highlight a deceptive and malicious email.

