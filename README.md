# John-the-Ripper
John the Ripper is a fast password cracker

John the Ripper: An Ethical Guide for Password Strength Auditing
This repository provides documentation and educational resources for using John the Ripper (JtR), a powerful open-source password security auditing and recovery tool. It is intended for students, cybersecurity professionals, and system administrators to learn about password security in a controlled and ethical manner.

⚠️ Ethical Warning & Disclaimer
This tool should only be used in legal and ethical scenarios such as penetration testing, educational labs, and authorized assessments. Unauthorized use can lead to severe legal consequences.

By using the information in this repository, you agree that you will only use John the Ripper on systems and files for which you have explicit, written permission. The author of this guide is not responsible for any misuse or damage caused by this tool. Always act responsibly.

What is John the Ripper?
John the Ripper is a fast password cracker, currently available for many flavors of Unix, Windows, DOS, and OpenVMS. Its primary purpose is to detect weak Unix passwords. Besides several crypt(3) password hash types most commonly found on various Unix systems, supported out of the box are Kerberos/AFS and Windows LM hashes, plus a number of other hashes and ciphers in the community-enhanced version.

Legitimate Use Cases
System Administrators: To audit their own systems for weak passwords and enforce stronger password policies.

Penetration Testers: To demonstrate the risk of weak passwords during an authorized security assessment.

Students & Educators: To understand the mechanics of password hashing, cracking, and the importance of cryptographic security in a lab environment.

Password Recovery: To recover lost passwords from your own encrypted files or systems when you have forgotten them.

Advanced Lab: Cracking a SHA512 Hash on Kali Linux
This example demonstrates a more realistic scenario. We will first change the default (and very strong) hashing algorithm on Kali Linux to a weaker one (SHA512) for educational purposes, create a test user, and then crack the user's password.

Step 1: Lab Setup - Downgrade Hashing Algorithm
Warning: This procedure makes your system less secure. Only perform these steps on a disposable virtual machine or a dedicated lab environment.

Modern Linux systems use strong hashing algorithms like yescrypt. For this lab, we will change it to SHA512, which is faster to crack.

Edit the login definitions file:

sudo nano /etc/login.defs

Find the line ENCRYPT_METHOD yescrypt and change it to ENCRYPT_METHOD SHA512.

Edit the common password file:

sudo nano /etc/pam.d/common-password

Find the line containing password [...] pam_unix.so obscure yescrypt and change yescrypt to sha512.

Nano Editor Tip: After editing a file in nano, save and exit by pressing Ctrl + O (Write Out), Enter (to confirm the file name), and Ctrl + X (to exit).

Step 2: Create a Test User and Set a Password
Now we will create a user that will have its password hashed using our new, weaker configuration.

Create the user:

sudo useradd -m testuser1

Set a simple password for the user:

sudo passwd testuser1

When prompted, enter a simple password, for example, 123456.

Step 3: Prepare the Password Hashes for JtR
We need to extract the user's password hash and prepare it in a format that John the Ripper can understand.

Verify the hash type (optional): You can see that the new user's hash uses SHA512 (indicated by the $6$ prefix).

sudo cat /etc/shadow | grep testuser1

Copy the password files: Never work on live system files.

sudo cp /etc/passwd /etc/shadow .

Combine the files using unshadow:

unshadow passwd shadow > mypasswords.txt

You now have a file named mypasswords.txt ready for auditing.

Step 4: Prepare the Wordlist
John the Ripper's power comes from using large lists of potential passwords (wordlists). We will use rockyou.txt, one of the most famous wordlists, which is included with Kali Linux but is often compressed.

Navigate to the wordlists directory:

cd /usr/share/wordlists/

Decompress rockyou.txt:

sudo gzip -d rockyou.txt.gz

Navigate back to your home directory:

cd ~

Step 5: Run the Audit
Now we will point John the Ripper at our hash file and tell it to use the rockyou.txt wordlist.

Start the cracking process:

john --wordlist=/usr/share/wordlists/rockyou.txt mypasswords.txt

JtR will start, load the hash, and try every password in rockyou.txt. For a simple password like 123456, it should find a match almost instantly.

Step 6: View the Results
Once JtR has cracked a password, it saves it to a file called john.pot. You can view all cracked passwords with the --show option.

Show cracked passwords:

john --show mypasswords.txt

The output will show the username and the cracked password: testuser1:123456.

Conclusion
This lab demonstrates a complete, practical workflow for auditing a password under controlled conditions. It highlights how system configurations can affect security and how quickly a simple password can be compromised with the right tools and a good wordlist.

Always remember to use your skills and knowledge responsibly and ethically.
