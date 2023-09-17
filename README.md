<p align="center">
<img src="https://i.imgur.com/9Ab7VoK.png" height="80%" width="80%" alt="Patriot CTF"/>
<p/>
<h1>PatriotCTF2023: Breakfast Club (Cryptography)</h1>
PatriotCTF 2023 is a Jeopardy-style CTF competition organised by the MasonCC (Mason Competitive Cyber) club at George Mason University.
<br/>
<br/>
<h2>Challenge Description:</h2> 
<p align="center">
"As the sysadmin for your college, you're responsible for overseeing the security of all the clubs. One of the on campus orginizations is a breakfast club with their own personal website that the leader assured you was "unhackable". He was so sure of this, that he sent you an example of how hashes are stored in the database, something about "changing the hash type multiple times for each password" or something like that. Can you crack the password and prove him wrong?"
<br/>
<br/>
Flag format: PCTF{}
<h2>Solution:</h2> 
<p align="center">
Along with the prompt provided above, the challenge also includes a file called 'BreakfastPasswords.txt':  
<br/>
<br/>
<img src="https://i.imgur.com/w4RmUX8.png" height="100%" width="100%" alt="BreakfastPasswords.txt"/>   
<br/>
<br/>
I began by attempting to decrypt some of the hashes provided using https://crackstation.net/. CrackStation is a website offering a variety of online tools such as hash cracking. I started by entering the hashes provided that have compatible algorithms with site: SHA, SHA1, MD2, MD4, MD5, SHA224, SHA256, SHA384, and SHA512: 
<br />
<br />
<img src="https://i.imgur.com/PhUwmX1.png" height="90%" width="90%" alt="CrackStation Input"/> 
<br />
<br />
These hashes then outputted the following result: 
<br />
<br />
<img src="https://i.imgur.com/2VCCqLn.png" height="90%" width="90%" alt="CrackStation Result"/> 
<br />
<br />
This tool provided the first 9 out of 17 values: <strong>'PCTF{H@5H--------'</strong> 
<br/>
<br/>
The remaining 8 hash values would need to be cracked using a different method. Because the flag format is 'PCTF{}', and the current digest retrieved is 'PCTF{H@5H', I assumed that the last character would be '}'. In order to test this out, I created a Python script using the Python library 'hashlib': 
<br/>
<br/>
 <img src="https://i.imgur.com/BpPhxw6.png" height="70%" width="70%" alt="BLAKE2B Script"/>
 <br/>
 <br/>
This script takes the string value of '}' and converts it into bytes for the hash calculation. The 'hashlib.blake2b()' built-in function then calculates the hash value of the string. The program then prints out the hash digest. Here is the output value of the script:
 <br/>
 <br/>
 <img src="https://i.imgur.com/k9FGKnn.png" height="200%" width="200%" alt="BLAKE2B Script Output"/>
<br/>
<br/>
The value outputted matches the BLAKE2B hash value in 'BreakfastPasswords.txt', therefore the script provided an additional hash digest value. There are now 7 values remaining to be decrypted. 
<br/>
<br/>
  The current digest is: <strong>'PCTF{H@5H-------}'</strong> 
 <br/>
 <br/>
 <img src="https://i.imgur.com/25bx2Ph.png" height="30%" width="30%" alt="Uncredentialed Scan Results"/>     <img src="https://i.imgur.com/WjAcm6L.png" height="30%" width="30%" alt="Credentialed Scan Results"/>
  <br/>
 <br/>
For further analysis, I have downloaded a deprecated version of Firefox on the Windows 10 virtual machine. When installing deprecated software, it is vital to ensure that it is done in a sandboxed environment. I then relaunched the scan and got the following results:
 <br/>
 <br/>
 <img src="https://i.imgur.com/nFye7ok.png" height="60%" width="60%" alt="Credentialed Scan Results w/ Firefox"/>
  <br/>
 <br/>
 <img src="https://i.imgur.com/KLqflvy.png" height="50%" width="50%" alt="Credentialed Scan Detailed Results w/ Firefox"/>
  <br/>
 <br/>
Rather than having 7 'Medium' level vulnerabilities as in the first credentialed scan, there are now 23. The 'High' severity vulnerabilities increased from 34 to 108, and the 'Critical' level vulnerabilities increased from 8 to 86. These scan results highlight the importance of ensuring that all third-party software are fully patched and up-to-date. Depicted below is a comparison of the first credentialed scan (left) to the second credentialed scan with the deprecated version of Firefox installed (right):"
 <br/>
 <br/>
 <img src="https://i.imgur.com/WjAcm6L.png" height="30%" width="30%" alt="Credentialed Scan Results"/>     <img src="https://i.imgur.com/FcqpzOC.png" height="30%" width="30%" alt="Credentialed Scan Results w/ Firefox"/>
<h2>Remediation:</h2> 
 <p align="center">
Prior to running a vulnerability scan, it is essential to ensure that both the operating system and all third-party software are fully up-to-date. This will significantly reduce the number of vulnerabilities present in the scan results and allow the analyst to have more efficient vulnerability management. To further maximize efficiency, setting up automatic updates on your operating system and third-party software is very beneficial.
 <br/>
 <br/>
<img src="https://i.imgur.com/aMYixCT.png" height="60%" width="60%" alt="Uninstall Firefox"/>
<br />
<br />
 <img src="https://i.imgur.com/2aNsH9T.png" height="60%" width="60%" alt="Windows Updates"/>
 <br />
<br />
After uninstalling the deprecated Firefox and running Windows updates, I recieved the following scan result:
<br />
<br />
<img src="https://i.imgur.com/a1LVKQZ.png" height="60%" width="60%" alt="Windows Update Scan"/>
<br />
<br />
 <img src="https://i.imgur.com/XqWuev6.png" height="60%" width="60%" alt="Windows Update Scan"/>
<br />
<br />
I then investigated more specific vulnerabilities for remediation. I found a few more vulnerabilities related to out-of-date software and installed the necessary software updates.
<br />
<br />
<img src="https://i.imgur.com/lj5Xcv0.png" height="70%" width="70%" alt="3D Viewer Vulnerability"/>
<br />
<br />
 <img src="https://i.imgur.com/vY8a8LB.png" height="70%" width="70%" alt="3D Viewer Update"/>
<br />
<br />
<img src="https://i.imgur.com/Nv8j19h.png" height="70%" width="70%" alt="Onedrive Update"/>
<br />
<br />
Missing or misconfigured registry keys can also cause vulnerabilities on a system. When configuring registry keys, it is crucial to ensure that the changes made align with the intended system or application settings and do not introduce unintended consequences or conflicts with existing configurations. To address the WinVerifyTrust Signature Validation Vulnerability (CVE-2013-3900), refer to the following link: https://msrc.microsoft.com/update-guide/vulnerability/CVE-2013-3900.
<br />
<br />
 <img src="https://i.imgur.com/Zrx2GOj.png" height="70%" width="70%" alt="WinVerifyTrust Vulnerability"/>
<br />
<br />
<img src="https://i.imgur.com/0eXXMzV.png" height="70%" width="70%" alt="WinVerifyTrust Remediation"/>
<br />
<br />
I also addressed the 'SMB Signing not required' vulnerability, which has a 'Medium' severity level. This involved navigating to the 'Local Security Policy' and making the necessary configuration changes. The following link provides more information on how to resolve this vulnerability: https://learn.microsoft.com/en-us/windows/security/threat-protection/security-policy-settings/microsoft-network-server-digitally-sign-communications-always.
<br />
<br />
<img src="https://i.imgur.com/AtNAkOd.png" height="70%" width="70%" alt="SMB Signing Vulnerability"/>
<br />
<br />
 <img src="https://i.imgur.com/lOaujty.png" height="70%" width="70%" alt="SMB Signing Remediation"/>
<br />
<br />
<img src="https://i.imgur.com/3zNJDy1.png" height="70%" width="70%" alt="SMB Signing Remediation"/>
<br />
<br />
After addressing these more specific vulnerabilities on the target system, I launched the final Nessus scan:
<br />
<br />
<img src="https://i.imgur.com/X4tXd1e.png" height="60%" width="60%" alt="Final Scan Results"/>
<br />
<br />
 <img src="https://i.imgur.com/0rygP8U.png" height="60%" width="60%" alt="Final Scan Results"/>
<br />
<br />
Rather than having 5 'Medium' level vulnerabilities as in the previous scan, there are now 0. The 'High' severity vulnerabilities decreased from 16 to 3, and the 'Critical' level vulnerabilities decreased from 2 to 1. Depicted below is a comparison of the previous scan after Firefox was uninstalled and Windows updates were run (left) to the most recent scan where more specific remediations were also made (right):
<br />
<br />
<img src="https://i.imgur.com/cVK2O89.png" height="35%" width="35%" alt="Update Scan Results"/>     <img src="https://i.imgur.com/229wO2C.png" height="35%" width="35%" alt="Remediation Scan Results"/>
<h2>Key takeaways:</h2>
<p align="center">
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
