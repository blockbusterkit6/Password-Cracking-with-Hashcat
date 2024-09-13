# Password Cracking with Hashcat and John the Ripper

**Overview:**  
In this project, I performed password cracking using **Hashcat** and **John the Ripper** to assess the strength of password policies. By utilizing both dictionary-based and brute-force attacks, I successfully cracked hashed passwords and evaluated the security posture of my target environment.

---

## Project Steps

### **1. Setting Up the Environment**

For this password cracking project, I used **Kali Linux** due to its pre-installed tools such as **Hashcat** and **John the Ripper**. These tools are highly effective for cracking hashed passwords and evaluating password strength.

- **Kali Linux Setup:**
  - Installed Kali Linux on VirtualBox, configured with **Bridged Mode** for network access.
  - Ensured both **Hashcat** and **John the Ripper** were up to date.

**Tools Used:**
- **Hashcat**: A powerful password-cracking tool that supports a variety of hash types.
- **John the Ripper**: A password-cracking tool designed for fast, customizable attacks on various encryption methods.

[Official Kali Linux Download](https://www.kali.org/get-kali/#kali-bare-metal)

---

### **2. Extracting Password Hashes**

Before starting the cracking process, I needed to obtain the password hashes from the target system. This can be done in various ways, such as accessing hashed passwords from a compromised machine, database dump, or even through penetration testing.

**Steps:**

1. **Linux System Password Hashes (Example):**
   On a Linux system, password hashes are often stored in the `/etc/shadow` file. I used the following command to extract the hashes:
   ```bash
   sudo cat /etc/shadow
   ```
   Here’s an example of a password hash:
   ```
   user:$6$Jd5YK/.F$0Nk4Ouh5MkQf3EER$:17938:0:99999:7:::
   ```

2. **Windows Password Hashes:**
   For Windows systems, I extracted password hashes from the **SAM** (Security Account Manager) file using a tool like **Mimikatz** or **SAMdump2**.

3. **Hash Format Identification:**
   Once I had the hashes, I used Hashcat’s `--example-hashes` to identify the format of the hash. For example, for Linux `SHA-512` hashes:
   ```bash
   hashcat --example-hashes | grep 1800
   ```

---

### **3. Cracking Passwords with Hashcat**

Next, I used **Hashcat** to crack the password hashes using both a **wordlist** attack and a **brute-force** attack. Hashcat is capable of cracking numerous hash types, including MD5, SHA-1, SHA-256, and more.

**Steps:**

1. **Prepare Wordlist:**
   I selected the popular `rockyou.txt` wordlist, which comes pre-installed in Kali Linux:
   ```bash
   /usr/share/wordlists/rockyou.txt
   ```

2. **Cracking Password Using Hashcat (Dictionary Attack):**
   To crack the password using a wordlist, I used the following command:
   ```bash
   hashcat -m 1800 -a 0 [hash file] /usr/share/wordlists/rockyou.txt
   ```
   - `-m 1800`: Specifies the hash type (1800 is for SHA-512).
   - `-a 0`: Specifies a dictionary attack.
   - `[hash file]`: The file containing the password hashes.

3. **Running Hashcat (Brute Force Attack):**
   If the dictionary attack didn’t yield results, I switched to a brute-force attack:
   ```bash
   hashcat -m 1800 -a 3 [hash file] ?a?a?a?a?a?a
   ```
   - `-a 3`: Specifies a brute-force attack.
   - `?a?a?a?a?a?a`: Specifies the character set to use (any characters, 6 positions).

**Sample Output:**
```bash
Session..........: hashcat
Status...........: Cracked
Hash.Type........: SHA-512
Hash.Target......: $6$Jd5YK/.F$0Nk4Ouh5MkQf3EER$...
Password.........: password123
```

**Result:**  
After running the command, Hashcat successfully cracked the password, showing the plaintext password `password123` for the given hash.

---

### **4. Cracking Passwords with John the Ripper**

After using Hashcat, I switched to **John the Ripper** for more flexible attack types and to test the hashes against additional rules and wordlists.

**Steps:**

1. **Prepare Hash File:**
   I created a text file containing the password hashes I wanted to crack:
   ```bash
   nano hashes.txt
   ```

2. **Running John the Ripper (Dictionary Attack):**
   I executed a simple dictionary attack using John’s default wordlist:
   ```bash
   john --wordlist=/usr/share/wordlists/rockyou.txt hashes.txt
   ```
   - This command uses the wordlist from **rockyou.txt** to test against the password hashes in `hashes.txt`.

3. **Running John the Ripper (Brute Force Attack):**
   I also ran a brute-force attack when the dictionary didn’t succeed:
   ```bash
   john --incremental hashes.txt
   ```

4. **Monitoring Progress:**
   I monitored the cracking process by running:
   ```bash
   john --show hashes.txt
   ```

**Result:**  
John the Ripper successfully cracked the password after some time, displaying the clear-text passwords associated with the cracked hashes.

---

### **5. Reporting and Analyzing Password Security**

After cracking the passwords, I performed an analysis of the results to determine the strength of the cracked passwords and provide recommendations for better security practices. This included:
- **Password Complexity Analysis**: Evaluating whether the cracked passwords were strong or weak (e.g., length, use of numbers/special characters).
- **Recommendations**: Advising to use longer, more complex passwords (e.g., 12+ characters, mixed case, special symbols), implement password managers, and enforce password expiration policies.

---

### **6. Conclusion and Remediation**

After completing the password-cracking process, I analyzed the passwords and provided the following key recommendations for improving password security:

- **Use Stronger Hashing Algorithms**: Recommend switching to stronger, more secure hashing algorithms like **bcrypt** or **Argon2**.
- **Increase Password Complexity**: Encourage the use of password policies that require longer, more complex passwords (e.g., at least 12 characters, including uppercase, lowercase, numbers, and symbols).
- **Implement Multi-Factor Authentication (MFA)**: Suggest using MFA wherever possible to add an extra layer of security beyond just password protection.
  
---

## **Tools Used:**

- **Hashcat**: Used for both dictionary-based and brute-force password cracking.
- **John the Ripper**: Used for flexible password-cracking techniques and testing custom rules.
- **Kali Linux**: The operating system environment used for this penetration test.

---

## **Lessons Learned:**

Through this project, I learned:
- The differences between dictionary-based and brute-force password-cracking techniques.
- How to identify and crack different types of password hashes.
- The importance of enforcing strong password policies and using secure hashing algorithms.
- Best practices for analyzing and reporting on password strength, along with providing actionable remediation steps.

---

This README serves as documentation for my experience using **Hashcat** and **John the Ripper** to crack password hashes, evaluate password strength, and provide security recommendations. Through this exercise, I gained hands-on experience in using powerful password-cracking tools and developed a deeper understanding of password security best practices.
