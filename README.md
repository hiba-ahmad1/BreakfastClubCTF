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
For the remaining 7 values, it would be too time-consuming to guess the characters manually. Therefore, with the help of the 'hashlib' module in Python, I created a Python script which outputs the hash digest of the following characters: 'a-z', 'A-Z', '0-9', and special characters. This is also based off the assumption that each hash value represents a single character, following the current trend of digests. 
 <br/> 
 <br/>
 <img src="https://i.imgur.com/A6mnCvI.png" height="90%" width="90%" alt="Hash Cracker Script"/>
 <br/>
 <br/>
This script takes a string input and uses the 'hashlib.algorithm()' function, where 'algorithm' is replaced by the supported hash algorithm, to return the hash digest. Out of the remaining 7 hash values, 5 are supported by hashlib: SHA3_224, SHA3_256, SHA3_384, SHA3_512, and BLAKE2s. The characters are then provided and iterated through. The provided hash from 'BreakfastPasswords.txt' is compared to the iterations, and if a match is found, it is outputted. This process was repeated for all 5 algorithms to retrieve the following values:
 <br/>
 <br/>
 <img src="https://i.imgur.com/0XbhTE4.png" height="100%" width="100%" alt="Value"/>
  <br/>
 <br/>
 <img src="https://i.imgur.com/3gVjejm.png" height="150%" width="150%" alt="Value"/>
  <br/>
 <br/>
  <img src="https://i.imgur.com/w5wx9hq.png" height="250%" width="150%" alt="Value"/>
  <br/>
 <br/>
 <img src="https://i.imgur.com/vNOEbz4.png" height="150%" width="150%" alt="Value"/>
  <br/>
 <br/>
  <img src="https://i.imgur.com/abzgqQu.png" height="150%" width="150%" alt="Value"/>
  <br/>
 <br/>
This script provided an additional 5 hash digest values. There are now 2 values remaining to be decrypted. The current digest is: <strong>'PCTF{H@5H_8R0--S}'</strong> 
 <br/>
 <br/>
The remaining 2 hash values use the algorithms TupleHash128 and TupleHash256, which are not supported by 'hashlib'. After conducting some research, I found the Python module 'Crypto.Hash', which supports both of these algorithms. For more information on the 'Crypto.Hash' algorithm, refer to the following link: https://pycryptodome.readthedocs.io/en/latest/src/hash/hash.html. I then wrote the following script: 
 <br/>
 <br/>
 <img src="https://i.imgur.com/KdHDIQi.png" height="80%" width="80%" alt="Tuple Hash Script"/>
 <br/>
 <br/>
This script is essentially identical to the previous one, but it utilizes a different module, resulting in slightly different syntax. It starts by generating the digest based on an input string and then iterates through the provided characters to display the results. Here are the script's output values:
 <br/>
 <br/>
<img src="https://i.imgur.com/eKOUJEb.png" height="150%" width="150%" alt="TupleHash128"/>
<br />
<br />
 <img src="https://i.imgur.com/kw8A9fV.png" height="150%" width="150%" alt="TupleHash256"/>
<br />
<br />
All of the hash digests have now been retrieved! The resulting flag is <strong>PCTF{H@5H_8R0WNS}</strong>
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
