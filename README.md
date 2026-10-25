# Cryptography-note-cybersec101
Here are my notes after finishing Cryptography module in tryhackme
# Comprehensive Cryptography Notes

## Cryptography Basics

### Importance of Cryptography

* **Core Triad:** Used to protect **Confidentiality**, **Integrity**, and **Authentication/Non-repudiation**.
* **Compliance:** A standard requirement for the Payment Card Industry Data Security Standard (PCI DSS).

### Key Terms

* **Plaintext:** The original, readable message.
* **Ciphertext:** The encrypted, unreadable message.
* **Cipher:** The mathematical algorithm used to turn plaintext into ciphertext.
* **Key:** A string of bits used by the cipher to perform encryption or decryption.
* **Encryption:** The process of converting plaintext into ciphertext.
* **Decryption:** The reverse process, converting ciphertext back to plaintext.

### Historical Ciphers

* **Caesar Cipher:** Shifts the alphabet by a set number of slots defined by the key.
* **Other Examples:** Vigenère cipher, Enigma machine, One-Time Pad.

---

## Types of Encryption

### Symmetric Encryption

* **Concept:** Uses the **same key** to both encrypt and decrypt data. Fast, but requires secure key exchange.
* **Examples:** DES (Data Encryption Standard), 3DES (Triple DES), AES (Advanced Encryption Standard).

### Asymmetric Encryption (Public Key Cryptography)

* **Concept:** Uses a mathematically linked **pair of keys**: a **Public Key** (to encrypt) and a **Private Key** (to decrypt).
* **Examples:** RSA, Diffie-Hellman, ECC (Elliptic Curve Cryptography).

### Mathematical Foundations

* **XOR Operation ($\oplus$):**
* Property 1: XORing a value with itself results in 0 ($A \oplus A = 0$).
* Property 2: XORing a value with 0 leaves it unchanged ($A \oplus 0 = A$).
* If $P$ is plaintext and $K$ is the secret key, the ciphertext is $C = P \oplus K$.
* Because XOR is reversible, if we know $C$ and $K$, we can recover $P$ ($C \oplus K = P$).


* **Modulo Operation (mod):**
* The remainder of a division. It is a "one-way" mathematical function (not easily reversible).
* Example: If $x \pmod 5 = 4$, there are infinite possible values for $x$ ($9, 14, 19, \dots$).



---

## Public Key Cryptography Algorithms

### RSA Algorithm Example

RSA relies on the mathematical difficulty of factoring large prime numbers.

**The Variables:**

* $p$ and $q$: Large prime numbers.
* $n$: The product of $p \times q$.
* $m$ (or $x$): The original plaintext message.
* $c$ (or $y$): The encrypted ciphertext.
* **Public Key:** $(n, e)$
* **Private Key:** $(n, d)$

**The Process:**

1. **Key Generation:** Bob chooses two prime numbers ($p = 157$, $q = 199$).
* Calculates the modulus: $n = p \times q = 31243$.


2. **Calculate Totient:** $\phi(n) = (p-1) \times (q-1) = 30888$.
* Bob selects public exponent $e = 163$ (must be relatively prime to $\phi(n)$).
* Bob calculates private exponent $d = 379$, where $(e \times d) \pmod{\phi(n)} = 1$.
* *Public Key:* $(31243, 163)$. *Private Key:* $(31243, 379)$.


3. **Encryption:** Alice wants to send the message $x = 13$. She uses Bob's public key to calculate ciphertext $y$:
* $y = x^e \pmod n = 13^{163} \pmod{31243} = 16341$.


4. **Decryption:** Bob receives $y = 16341$ and decrypts it using his private key $d$:
* $x = y^d \pmod n = 16341^{379} \pmod{31243} = 13$.



> **CTF Tools:** Tools like `RsaCtfTool` and `rsatool` are commonly used to defeat weak RSA challenges.

### Diffie-Hellman Key Exchange

A method that allows two parties to establish a shared secret over an insecure channel without needing a pre-existing key.

**The Process:**

1. **Public Variables:** Alice and Bob agree on a large prime $p = 29$ and a generator $g = 3$. (These are sent publicly).
2. **Private Integers:** Alice chooses private key $a = 13$. Bob chooses private key $b = 15$. (These are kept secret).
3. **Calculate Public Keys:**
* Alice calculates $A = g^a \pmod p = 3^{13} \pmod{29} = 19$.
* Bob calculates $B = g^b \pmod p = 3^{15} \pmod{29} = 26$.


4. **Key Exchange:** Alice sends $A$ to Bob. Bob sends $B$ to Alice.
5. **Calculate Shared Secret:**
* Alice calculates: $B^a \pmod p = 26^{13} \pmod{29} = 10$.
* Bob calculates: $A^b \pmod p = 19^{15} \pmod{29} = 10$.
* *Both parties independently arrive at the shared secret: **10**.*



---

## Secure Shell (SSH) & Digital Signatures

### SSH Key Generation

* **Command:** `ssh-keygen -t ed25519`
* **Algorithms:**
* **DSA:** Digital Signature Algorithm (designed specifically for signatures).
* **ECDSA:** Elliptic Curve DSA (smaller keys, equivalent security).
* **Ed25519:** Uses Edwards-curve Digital Signature Algorithm (highly recommended for modern SSH).
* **-SK variants:** (e.g., `ECDSA-SK`, `Ed25519-SK`) incorporate hardware security keys (like YubiKey) for physical protection.



### Digital Signatures & Certificates

* **Purpose:** Verifies authenticity (who sent it) and integrity (it hasn't been altered).
* **Mechanism:** You **sign** with your *Private Key*. Anyone can **verify** the signature using your *Public Key*.

### PGP and GPG

* **PGP (Pretty Good Privacy):** Software implementing encryption for files and emails.
* **GPG (GnuPG):** The open-source implementation of the OpenPGP standard.
* **Commands:**
* Import a key: `gpg --import backup.key`
* Decrypt a file: `gpg --decrypt confidential_message.gpg`



---

## Hashing Basics

### Hash Functions

* A mathematical algorithm that takes input data of *any* size and creates a fixed-size summary/digest.
* **Properties:** It is a **one-way function** (irreversible). If you put the exact same data in, you always get the exact same hash out (useful for verifying file integrity).
* **Common Algorithms:** `md5sum`, `sha1sum`, `sha256sum`, `sha512sum` (outputs in hexadecimal).
* **Hash Collision:** Occurs when two completely different inputs mathematically produce the exact same hash output.
* **Total possible hashes:** $2^{(\text{number of bits})}$.

### Passwords & Salting

* **Rainbow Table:** A massive lookup table of pre-computed plaintext passwords and their corresponding hashes.
* **Salt:** A randomly generated value unique to each user, added to the password before hashing. This ensures two users with the same password do not produce the same hash, defeating basic rainbow tables.

### Linux Shadow File

* Passwords are stored in `/etc/shadow` (readable only by root).
* Format: `$prefix$options$salt$hash`
* *Prefix:* Identifies the hashing algorithm (e.g., `$y$` or `$6$` for SHA-512/yescrypt).
* *Salt & Hash:* Follows the prefix.



### HMAC (Keyed-Hash Message Authentication Code)

* Combines a cryptographic hash function with a secret key to verify data integrity and authenticity.
* **Formula:** $HMAC(K,M) = H((K \oplus opad) \parallel H((K \oplus ipad) \parallel M))$
* **Process:** The secret key is padded, XORed with constants, hashed with the message, and then hashed again.

### Base64 Encoding

* *Note: Base64 is encoding, not encryption or hashing. It is easily reversible and offers no security.*
* Encode: `base64 <plain_text_file>`
* Decode: `base64 -d <encoded_file>`

---

## Password Cracking: John the Ripper

John the Ripper (JtR) is a powerful, fast password cracker.

### Basic Usage & Hash Identification

* **Syntax:** `john [options] [file path]`
* **Auto-Detect & Crack:** `john --wordlist=[path to wordlist] [path to file]`
* **Specify Format:** `john --format=[format] --wordlist=[path to wordlist] [path to file]`
* *Note:* Standard hashes (like MD5) may need the `raw-` prefix (e.g., `--format=raw-md5`).


* **Identify Hashes:** Use tools like `hash-id.py` or online identifiers.
* **List Formats:** `john --list=formats` (or `john --list-formats | grep -iF "md5"`).

### Windows NTLM Hashes

* Modern Windows systems store passwords in **NThash** format (NTLM) inside the **SAM** (Security Account Manager) database or `NTDS.dit`.
* You can extract these using tools like Mimikatz.
* **JtR Format:** `--format=nt`

### Cracking Linux /etc/shadow

JtR needs both the `passwd` and `shadow` files combined to map usernames to hashes.

1. **Unshadow:** `unshadow [path to passwd] [path to shadow] > unshadowed.txt`
2. **Crack:** `john --wordlist=[path to wordlist] unshadowed.txt`

### Single Crack Mode

Uses the username (and variations of it) to quickly guess lazy passwords.

* **Format requirement:** The file must explicitly link the username to the hash (e.g., `mike:1efee03cdcb...`).
* **Command:** `john --single --format=[format] [path to file]`

### Custom Rules

You can write custom mutation rules to modify wordlist entries on the fly.
**Example Rule:**

```ini
[List.Rules:PoloPassword]
cAz"[0-9] [!£$%@]"

```

* `c`: Capitalizes the first letter.
* `Az`: Appends to the end of the word.
* `"[0-9] [!£$%@]"`: Appends a number (0-9) followed by a special character.
* **Command:** `john --wordlist=[path to wordlist] --rule=PoloPassword [path to file]`

### Auxiliary Tools

* **zip2john:** Extracts hashes from password-protected ZIP files.
* `zip2john [zip file] > hash.txt`


* **rar2john:** Extracts hashes from RAR archives.
* `rar2john [rar file] > hash.txt`


* **ssh2john:** Extracts hashes from encrypted SSH private keys (`id_rsa`).
* `ssh2john id_rsa > hash.txt`