# Blue Team Lab Email Analysis Challenge

## Scenario
[]image 

## Email Authentication Status Check

- **SPF** - is set to FAIL, meaning the domain **microapple.com** does not permit the mail server of IP address **93.99.104.210**

## Other Headers

- The **Reply-To**  header is not the same as the **(@pashter.ocm)** is different to the **FROM** header of **@microapple.com**  which makes it suspicious

- The is also a discrapancy with **Time** and **Date**
    - The recipeint **Received** date is:
       Mon, 25 Jan 2021 22:41:10 -0800 (PST) convert to UTC we add 8 hours which is = 06:41:18 Jan 26, 2021

    - Sender time is Tue, 26 Jan 2021 01:41:18 -0500 (EST) we add 5 hours which is = 06:41:18 Jan 26, 2021 UTC
    - **High Suspicion of Forgery/Manipulation:** Since the UTC timestamps are exactly the same, it strongly suggests that the "Received" header from the sender's server has been forged or manipulated.

[]image

- Also checking the sender's **Received** header we can see that the malicious actor used **(emkei.cz)** email service which is a fake email sender service

## Decoding base64

[]image

We see that when we decode the base64, it has a instructions telling us to solve a puzzple in an attachment. (which is also suspicious)

- we then save this file 

- Scrolling down the email we get the **Content-Type** header set to **PDF** which we cannot be sure, so we copy the base64 to CyberChef again. We then convert the decoded base64 to **hex** to check for the **File Signature**

[]image

-Using **Gary Kessler's File Signature** we see that it is not a PDF but a ZIP file

[]image

- Using the **HxD** application we can drag and drop to see the **files signature** in hex, we then check the signature in **Gary Kessler's File Signature** to see its real .extension

[]image

- We can also upload the file to CyberChef to see the file extension

[]image

# **in Conclusion**

- This email analysis strongly indicates a phishing attempt due to multiple critical red flags. Key indicators include: an SPF authentication FAIL, mismatched Reply-To and FROM headers, and identical UTC timestamps for both sender and recipient "Received" headers, suggesting header forgery. Further, the sender used a known fake email service (emkei.cz), and a forensic examination of the attached file, despite being labeled as a PDF, revealed its true nature as a ZIP file through signature analysis. These combined elements highlight a deceptive and malicious email.

