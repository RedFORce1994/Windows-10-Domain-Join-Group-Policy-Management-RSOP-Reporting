# Windows-10-Domain-Join-Group-Policy-Management-RSOP-Reporting

This home lab walkthrough covers joining a Windows 10 machine to a domain as a local user, applying Group Policy configurations, and running RSOP reports to verify policy enforcement across the domain environment.

<h2>Goals</h2>

- **Domain-join a Windows 10 PC** using a local user account.
- Create and apply **GPOs** to enforce settings on domain-joined machines.
- Generate and review **RSOP** reports to troubleshoot policy application.
- Manage GPO configurations through the **Group Policy Management Console (GPMC)**

<h2>Walkthrough</h2>



A second Windows 10 virtual machine, Desktop2, will be set up to represent a typical employee workstation in the lab. This allows us to simulate real-world scenarios, such as using the Help Desk account on our primary Windows 10 VM to reset a user's password when needed.

To get started, go to Machine → New, name the machine Desktop2, select the Windows ISO from your downloads, choose Skip Unattended Installation, and click Finish."

1. <p align="center">
   <img width="1023" height="767" alt="1bfea619-5378-46e1-abf7-e11f5de36d65" src="https://github.com/user-attachments/assets/f7c24024-28a9-49f1-86f1-45d1a18ca1f1" />
   <br />
   <br />
</p>

The procedure will mirror what we followed for our other Windows 10 virtual machine associated with the Helpdesk account. Click Next → Install now → I do not possess a product key.

2. <p align="center">
   <img width="1020" height="698" alt="oBHlqRW" src="https://github.com/user-attachments/assets/7ac23fc8-8cb8-4fc7-8abb-88ea5aa20ec4" />
   <br />
   <br />
</p>

Choose Windows 10 Pro, then proceed by clicking Next → Custom: Install Windows only → Next.

3. <p align="center">
   <img width="1017" height="700" alt="ulddbO0" src="https://github.com/user-attachments/assets/6dbc17ca-6532-4a67-af7d-6679fc043509" />
   <br />
   <br />
</p>

Proceed with the same settings as previously for the Windows 10 Helpdesk account. Choose Personal Use, then input User for the name and bypass the password creation.

4. <p align="center">
   <img width="1022" height="670" alt="otxMPmi" src="https://github.com/user-attachments/assets/1eef15d5-f705-4e16-bf2a-806cb4693026" />
   <br />
   <br />
</p>

Now that **Desktop2** has been established, we can proceed to create a user for this computer. To accomplish this, launch your **Helpdesk PC** virtual machine and log in as **Helpdesk**. Please note that **Windows Server 2022** must be operational in the background to enable access to **Active Directory Users and Computers** on our **Desktop1** lab machine.

Keep in mind, we currently have three virtual machines active: **Windows Server 2022**, **Windows 10 Helpdesk**, and our recently created **Windows 10 local user account**.

5. <p align="center">
   <img width="1023" height="762" alt="p22jMTB" src="https://github.com/user-attachments/assets/03938d36-1f22-4eb9-8762-063e1e608ff7" />
   <br />
   <br />
</p>

To begin, access Active Directory Users and Computers on the Windows 10 Helpdesk machine. Right-click on our domain SimoTech.com, then choose New → Organizational Unit. Label the first Organizational Unit as HR. Follow the same steps to create a second Organizational Unit, which should be named IT. At this point, you should have two OUs: HR and IT.

6. <p align="center">
   <img width="1023" height="760" alt="KUl5pTD" src="https://github.com/user-attachments/assets/922c192a-6f67-4238-8a6a-2a6a89bbe8e8" />
   <br />
   <br />
</p>

Next, we will create a user in Active Directory. Begin by right-clicking on Users within the domain, then choose New → User. Enter the name Bob (both first and last name) and assign the Logon Name as Bob. Ensure that all options are unchecked, then establish a password for Bob and click Finish to finalize the user creation.

7. <p align="center">
   <img width="430" height="375" alt="0smzO7q" src="https://github.com/user-attachments/assets/75b1356d-68e9-4021-bfdd-c24aec0d9519" />
   <br />
   <br />
</p>



8. <p align="center">
   <img width="435" height="379" alt="HYeyzoo" src="https://github.com/user-attachments/assets/012025e1-0e28-40fe-ac91-8c01812177e8" />
   <br />
   <br />
</p>

Now, we will move the newly created user Bob into the HR organizational unit (OU). When prompted to confirm your decision, click Yes to proceed. This will successfully transfer Bob into the HR OU.

9. <p align="center">
   <img width="752" height="528" alt="a03e23da-38bb-4034-9585-c4bffb486d5a" src="https://github.com/user-attachments/assets/954d6218-f5f4-4217-9d8b-ffa709d69e14" />
   <br />
   <br />
</p>

Next, transfer the 'Helpdesk' user to the IT OU. Once this is done, you should find 'Bob' in the HR OU and 'Helpdesk' in the IT OU. To confirm the locations of the users, activate 'Advanced Features' by clicking on the 'View' tab at the top, followed by selecting 'Advanced Features'.

10. <p align="center">
   <img width="750" height="528" alt="bTqJd6N" src="https://github.com/user-attachments/assets/d489b4f3-07af-443b-9d7c-b26176bffd3a" />
   <br />
   <br />
</p>

To find the user 'Bob,' begin by right-clicking on the domain SimoTech.com and selecting 'Find.' In the 'Find' window, right-click on 'Users' and opt for 'Properties.' Next, choose 'Entire Directory' and enter 'Bob' in the search field. When his name shows up, double-click on it and go to the 'Objects' tab to see his details.

11. <p align="center">
   <img width="750" height="526" alt="bGiK3Hw" src="https://github.com/user-attachments/assets/c77d6ea9-47c9-476c-af2a-139e0650cc1a" />
   <br />
   <br />
</p>


12. <p align="center">
   <img width="750" height="524" alt="NPU5xXV" src="https://github.com/user-attachments/assets/b39dab8f-06e0-4b68-91ea-0dec6d099fa3" />
   <br />
   <br />
</p>


13. <p align="center">
   <img width="759" height="581" alt="cO8Bgal" src="https://github.com/user-attachments/assets/b1114a51-3e5b-4de5-bde2-fda707600c3d" />
   <br />
   <br />
</p>

In the Objects tab, you will notice that Bob is included in the HR organizational unit, identified as SimoTech.com/HR/Bob.

14. <p align="center">
   <img width="400" height="508" alt="XcrE8uf" src="https://github.com/user-attachments/assets/203a0811-2f1a-4902-bdf9-22d720648030" />
   <br />
   <br />
</p>

We can verify this with the Helpdesk.

15. <p align="center">
   <img width="797" height="501" alt="peuepfS" src="https://github.com/user-attachments/assets/7d5a1ae2-7553-4a4b-85a0-4ba6df5d40a4" />
   <br />
   <br />
</p>

Let's access Group Policy Management through Server Manager with our Helpdesk account. After that, click on "Tools" and choose "Group Policy Management."

16. <p align="center">
   <img width="817" height="640" alt="OxGZv11" src="https://github.com/user-attachments/assets/3662c478-e967-4b28-805a-e3eb0f05d195" />
   <br />
   <br />
</p>

This will present the group policy for our domain controller. Choose "Domains" → "RedForce.com" → "Default Domain", and then click on "Generate Report." After that, navigate to "Settings" and select "Show All" located in the top-right corner.

17. <p align="center">
   <img width="1011" height="740" alt="h0VJVUq" src="https://github.com/user-attachments/assets/a4a4758c-aa69-4a96-a7db-e5bdd72fdfc2" />
   <br />
   <br />
</p>

This report provides an extensive overview of different policies, such as account policies, password policies, and account lockout policies. It serves as a crucial resource for comprehending the policies that affect a user. For example, we can see that the Account Lockout Policy is set with a limit of 0 invalid logon attempts. This configuration presents a security threat, as it permits unlimited login attempts, thereby exposing the system to brute-force attacks.

18. <p align="center">
   <img width="1201" height="937" alt="XHtTya4" src="https://github.com/user-attachments/assets/7232f6e3-2612-4aab-93b7-019399e284ca" />
   <br />
   <br />
</p>

To change this policy, right-click on "Default Domain Policy" and choose "Edit."

19. <p align="center">
   <img width="271" height="538" alt="ndd2jiO" src="https://github.com/user-attachments/assets/037e2f46-d693-4caf-9789-28c3d363bc82" />
   <br />
   <br />
</p>

Choose "Policy," then proceed to "Windows Settings," followed by "Security Settings," and double-click on "Account Policies."

20. <p align="center">
   <img width="783" height="560" alt="WtLokPE" src="https://github.com/user-attachments/assets/579ba6ee-aee3-4f64-bb5c-38a0dec1db22" />
   <br />
   <br />
</p>

Double-click on "Account lockout duration," activate the "Define this policy setting" option, and adjust it to 30 minutes. Furthermore, if you wish, you can change the "Account lockout threshold" by double-clicking it and lowering the number of invalid logon attempts from 5 to 4.

21. <p align="center">
   <img width="684" height="695" alt="fuqdKNk" src="https://github.com/user-attachments/assets/d2fac517-f67a-4fb9-b78c-36129fe96e8d" />
   <br />
   <br />
</p>



22. <p align="center">
   <img width="880" height="655" alt="4IzMWsg" src="https://github.com/user-attachments/assets/36979e8d-498c-47b8-a457-4ce1fffcdc9c" />
   <br />
   <br />
</p>

Now, let's set up some configurations in the Password Policy section. Change the Maximum password age to 90 days.

23. <p align="center">
   <img width="837" height="580" alt="e86b25f9-fbaf-48b6-84b1-1486c7417182" src="https://github.com/user-attachments/assets/26db1dc9-68a2-4630-bda7-255d0ee72998" />
   <br />
   <br />
</p>

Once the policies have been configured, we can enforce them by right-clicking on "Default Domain Policy" and choosing "Enforced."

24 <p align="center">
   <img width="814" height="673" alt="5QyfFqc" src="https://github.com/user-attachments/assets/ef552ca8-5420-4e85-aace-fa39725908e4" />
   <br />
   <br />
</p>

To ensure our policies are current, access Server Manager and proceed to Tools → Group Policy Management → Default Domain Policy. Create a report, navigate to Settings, and choose Show All. Verify that all modifications are in effect: the Maximum password age is established at 90 days, and the Account lockout threshold along with the Account lockout duration correspond to the settings we implemented.

25. <p align="center">
   <img width="782" height="577" alt="at4ycrW" src="https://github.com/user-attachments/assets/d3b123e3-5aba-4d99-8865-85f6d1213495" />
   <br />
   <br />
</p>

Let's go back to the Desktop2 virtual machine and launch File Explorer. Right-click on This PC and choose Properties. Click on Rename this PC (Advanced) → Change, modify the existing computer name to "Desktop2," and then click OK to implement the change. Lastly, restart the system to finalize the process.

26. <p align="center">
   <img width="924" height="713" alt="SLN7oZN" src="https://github.com/user-attachments/assets/8d0aac06-dfa3-4338-a037-8749d35751a9" />
   <br />
   <br />
</p>

Following the restart, we should activate the Administrator account. Begin by opening File Explorer, right-clicking on This PC, and choosing Manage. In the Computer Management window, go to Local Users and Groups → Users. Right-click on Administrator, select Properties, and deselect "Disable account". Finally, click Apply and OK to activate the account.

27. <p align="center">
   <img width="398" height="452" alt="WuFVDw2" src="https://github.com/user-attachments/assets/e390f3c8-259c-493d-9667-4720c50a4d0e" />
   <br />
   <br />
</p>

Right-click on the Administrator account once more and choose Set Password. Input the preferred password for the account and confirm it. After that, click OK to implement the changes.

28. <p align="center">
   <img width="600" height="598" alt="jooNPWx" src="https://github.com/user-attachments/assets/caa37961-42cf-4a55-89b7-38ff07a28052" />
   <br />
   <br />
</p>

Subsequently, log out of the PC and sign in as the administrator.

29. <p align="center">
   <img width="1020" height="765" alt="acc36fad-5688-49be-8812-247c56279d9a" src="https://github.com/user-attachments/assets/c1e96287-839f-418b-b27b-256d660de811" />
   <br />
   <br />
</p>

To eliminate the **User** login screen, adhere to the following steps:

1. Launch **File Explorer** and perform a right-click on **This PC**.
2. Choose **Properties** and subsequently click on **Advanced system settings**.
3. In the **User Profiles** section, click on **Settings**.
4. Locate and select **Desktop\User** from the list, then click **Delete** to erase the user profile.

30. <p align="center">
   <img width="444" height="474" alt="RWoIggH" src="https://github.com/user-attachments/assets/f2427ec0-990c-4844-a2e8-f3a729b294a4" />
   <br />
   <br />
</p>

Next, access the **Control Panel** and navigate to **View Network Status and Tasks**.
Click on **Change adapter settings**, then right-click the **Ethernet** connection and choose **Properties**.

Double-click on **Internet Protocol Version 4 (TCP/IPv4)**, and set the static IP configurations as follows:

- **IP Address:** 12.1.10.4
- **Subnet Mask:** 255.0.0.0
- **Default Gateway:** 12.1.10.1
- **Preferred DNS Server:** 12.1.10.2
- **Alternate DNS Server:** 12.1.10.1

Press **OK** to save the changes.

31. <p align="center">
   <img width="395" height="450" alt="DOZhtEd" src="https://github.com/user-attachments/assets/1605ca0b-3618-4b02-b57f-4df33b2a8ed0" />
   <br />
   <br />
</p>

Next, on our virtual machine, navigate to "Devices" → "Network" → "Network settings" → and modify "NAT" to "Host-only Adapter".

32. <p align="center">
   <img width="761" height="442" alt="4vsudaV" src="https://github.com/user-attachments/assets/e1658407-7d65-4c12-a96a-acb43ba88cb4" />
   <br />
   <br />
</p>

Now, let's launch Command Prompt and ping our domain, RedForce.com.

33. <p align="center">
   <img width="961" height="296" alt="QeVdaOx" src="https://github.com/user-attachments/assets/a9bd5139-b258-4e25-9917-bfbe0322b41d" />
   <br />
   <br />
</p>

To add this PC to our domain, we will open File Explorer, right-click on This PC, choose Properties, then click on Rename this PC (Advanced), and ultimately select Change.

34. <p align="center">
   <img width="321" height="389" alt="Wq7yGhk" src="https://github.com/user-attachments/assets/6fcef7fd-76e5-42d6-a484-26812595a4e2" />
   <br />
   <br />
</p>

Next, we can utilize our Helpdesk administrator account to connect to the domain. Subsequently, restart VirtualBox. After the restart, verify Active Directory Users and Computers → Computers under our Helpdesk account. You should observe that Desktop2 has been successfully incorporated into our domain, RedForce.com.

35. <p align="center">
   <img width="749" height="524" alt="0b1e7955-9f00-4a7e-ad4b-75c5bca6f4a2" src="https://github.com/user-attachments/assets/4e021b03-2e7c-4649-a93f-8596093a969e" />
   <br />
   <br />
</p>

Now that our PC has finished restarting, let's log in as Bob on our local user account, Desktop2.

36. <p align="center">
   <img width="1017" height="759" alt="3b192364-c83d-46e0-990b-f0e4f4a8e344" src="https://github.com/user-attachments/assets/ed4bd4c2-7f62-4cdf-aafa-d1ad8d7cd90c" />
   <br />
   <br />
</p>

Ultimately, we will execute several essential command-line tools to verify that everything is operating as intended.
Using ping RedForce.com, we check the network connectivity with the domain controller. The ipconfig command validates the correct network configuration, while net use Bob /domain assesses our capability to access domain resources using valid credentials. Through these verifications, we can assuredly affirm that our setup is functioning as anticipated.

37. <p align="center">
   <img width="1148" height="835" alt="1e360acd-6bca-4a80-928e-8c4a89045d48" src="https://github.com/user-attachments/assets/5b882d32-a43f-45b5-a89f-d98a1cca7298" />
   <br />
   <br />
</p>



38. <p align="center">
   <img width="1146" height="844" alt="z8U0rIv" src="https://github.com/user-attachments/assets/4a9830f9-2f3a-4aeb-b519-c66188197feb" />
   <br />
   <br />
</p>


Congratulations! We have successfully added **Desktop2** to the domain as a local user on a Windows 10 PC, configured and assessed policy settings, and examined **Group Policy Management**.

For administration and troubleshooting purposes, we employed **Command Line Tools (CMD)** and produced **Resultant Set of Policy (RSOP)** reports to evaluate the policies that have been implemented.


