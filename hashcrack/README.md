# picoCTF - hashcrack

### Description:
A company stored a secret message on a server, but the admin used weakly hashed passwords. Can you crack the hashes and gain access to the secret stored on the server?

### Flag format:
`picoCTF{FLAG}`

---

### Steps to Solve the Challenge:

#### Step 1: Access the Server

To begin, connect to the server using this command:

```bash
nc verbal-sleep.picoctf.net 51759
```

Once connected, the server will provide several hashes, and you need to crack them to find the secret.

#### Step 2: Crack the MD5 Hash

The first hash you encounter is:

```
482c811da5d5b4bc6d497ffa98491e38
```

This is an **MD5** hash. The challenge asks you to enter the password for this hash.

Use **crackstation.net** or any hash-cracking tool (such as Hashcat) to crack the hash.

- Using **crackstation.net**:
  1. Visit [crackstation.net](https://crackstation.net/).
  2. Paste the hash `482c811da5d5b4bc6d497ffa98491e38`.
  3. Click **Crack Hashes**.
  4. The password is **password123**.

Enter the password `password123` when prompted by the server. The message will confirm that you've cracked the MD5 hash, but no secret is revealed yet.

#### Step 3: Crack the SHA-1 Hash

The next hash you need to crack is:

```
b7a875fc1ea228b9061041b7cec4bd3c52ab3ce3
```

This is an **SHA-1** hash.

- Using **crackstation.net** or **Hashcat**, crack the hash.
  - The password for this hash is **letmein**.

Enter the password `letmein` when prompted by the server. Again, you will be told you've cracked the hash, but no secret is revealed.

#### Step 4: Crack the SHA-256 Hash

The final hash you need to crack is:

```
916e8c4f79b25028c9e467f1eb8eee6d6bbdff965f9928310ad30a8d88697745
```

This is an **SHA-256** hash.

- Using **crackstation.net** or **Hashcat**, crack the hash.
  - The password for this hash is **qwerty098**.

Once you enter the password `qwerty098`, the server will reveal the flag:

```
picoCTF{UseStr0nG_h@shEs_&PaSswDs!_ce730f64}
```

#### Step 5: Submit the Flag

You have successfully cracked the hashes and obtained the flag:

**Flag:**
```
picoCTF{UseStr0nG_h@shEs_&PaSswDs!_ce730f64}
```

---

### Tips:
- MD5, SHA-1, and SHA-256 are all common hash algorithms, but MD5 and SHA-1 are considered weak and can be cracked easily.
- Tools like **crackstation.net** and **Hashcat** are very useful for cracking hashes quickly.
- Always use strong hash algorithms like SHA-256 or SHA-3 for better security.

---

### Conclusion:
By cracking the given hashes, you uncovered the flag. This challenge highlights the importance of using strong hashing algorithms and secure passwords.
