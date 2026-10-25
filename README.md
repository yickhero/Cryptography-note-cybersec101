# Cryptography-note-cybersec101
Here are my notes after finishing Cryptography module in tryhackme
Cryptography
Cryptography basics:
Importance of Cryptography:
Used to protect confidentiality, integrity and confidentiality
Standard requirement for payment card industry data security standard (PCI DSS)
Terms:
Plaintext: original message
Ciphertext: encrypted, unreadable message
Cipher: algorithm to turn plaintext to ciphertext
Key: a string of bits the cipher uses
Encryption: the process of converting plaintext into ciphertext
Decryption: the reverse process from ciphertext to plaintext
Historical ciphers:
Caesar ciphers: you shift the alphabet to the amount of slots provided on the key
There are other types of encryption e.g. vigenere cipher, enigma machine, and one time pad
Types of Encryption:
Symmetric encryption: it uses the same key to encrypt and decrypt data
Examples are data encryption standard (DES), triple DES (3DEs) and advanced encryption standard (AES)
Asymmetric encryption: uses a pair of keys, one to encrypt and one to decrypt, called private key and public key
Examples are RSA, Diffie-Hellman, and Elliptic Curve cryptography (ECC) 
Mathematical operations:
Mathematical operations used for cryptography includes: XOR operation, Modulo operation
Properties includes: applying XOR to a value with itself results in 0, applying XOR to any value with 0 leaves it unchanged
If P is plaintext and K is secret key. The ciphertext is C=P⊕ K
If we know C and K, we can recover P
Modulo means the remainder of a division, it is not reversible, if we are given the equation x%5 = 4 any number can be x
Public Key Cryptography Basics
RSA:
The RSA algorithm example:
1. Bob chooses two prime numbers: p = 157 and q = 199. He calculates n = p × q = 31243.
2. With ϕ(n) = n − p − q + 1 = 31243 − 157 − 199 + 1 = 30888, Bob selects e = 163 such that e is          relatively prime to ϕ(n); moreover, he selects d = 379, where e × d = 1 mod ϕ(n), i.e., e × d = 163 × 379 = 61777 and 61777 mod 30888 = 1. The public key is (n,e), i.e., (31243,163) and the private key is $(n,d), i.e., (31243,379).
3. Let’s say that the value they want to encrypt is x = 13, then Alice would calculate and send y = xe mod n = 13163 mod 31243 = 16341.
4. Bob will decrypt the received value by calculating x = yd mod n = 16341379 mod 31243 = 13.  This way, Bob recovers the value that Alice sent.
Some tools to defeat RSA challenges includes: RsaCtfTool and rsatool
The variables includes:
p and q are large prime number
n is the product of p and q
The public key is n and e
The private key is n and d
m is used to represent the original message
c represents the encrypted text
Diffie-Hellman key exchange:
It is a method that allows two parties to establish a shared secret over an insecure communication channel without requiring a pre-existing shared secret and without an observer being able to get this key
The method is explained below:
1. Alice and Bob agree on the public variables: a large prime number p and a generator g, where 0 < g < p. These values will be disclosed publicly over the communication channel. Although insecurely small, we will choose p = 29 and g = 3 to simplify our calculations.
2. Each party chooses a private integer. As a numerical example, Alice chooses a = 13, and Bob chooses b = 15. Each of these values represents a private key and must not be disclosed.
3. It is time for each party to calculate their public key using their private key from step 2 and the agreed-upon public variables from step 1. Alice calculates A = ga mod p = 313 mod 29 = 19 and Bob calculates B = gb mod p = 315 mod 29 = 26. These are the public keys.
4. Alice and Bob send the keys to each other. Bob receives A = ga mod p = 19, i.e., Alice’s public key. And Alice receives B = gb mod p = 26, i.e., Bob’s public key. This step is called the key exchange.
5. Alice and Bob can finally calculate the shared secret using the received public key and their own private key. Alice calculates Ba mod p = 2613 mod 29 = 10 and Bob calculates Ab mod p = 1915 mod 29 = 10. Both calculations yield the same result, gab mod p = 10, the shared secret key.
SSH
ssh-keygen is the program used to generate key pairs. Command: ssh-keygen -t ed25519
Different algorithms that ssh-keygen can generate includes:
DSA (digital signature algorithm) is a public key cryptography algorithm specifically designed for digital signatures
ECDSA (Elliptic Curve Digital Signature Algorithm) is a variant of DSA that uses elliptic curve cryptography to provide smaller key sizes for equivalent security
ECDSA-SK (ECDSA with Security Key) is an extension of ECDSA. It incorporates hardware-based security keys for enhanced private key protection
Ed25519 is a public-key signature system using EdDSA (Edwards-curve Digital Signature Algorithm) with Curve25519
Ed25519-SK (Ed25519 with Security Key) is a variant of Ed25519. It uses a hardware based security key for improved private key protection
To generate private key:
Ssh-ed25519 [publickey] [servername]
Digital signature and certificates
Provides a way to verify the authenticity and integrity of a digital message and document
You produce a signature with your private key and can be verified using your public key
PGP and GPG:
PGP stands for pretty good privacy. It’s software that implements encryption for encrypting files, performing digital signing and more. GnuPG or GPG is an open-source implementation of the OpenPGP standard
GPG is commonly used in email to protect the confidentiality of the email messages, also used to sign an email message and confirm its integrity
Private key is usually protected with passphrase just like in SSH
To import and decrypt your received message do: 
gpg –import backup.key 
Gpg –decrypt confidential_message.gpg
Hashing basics:
Hash Function:
A hash function takes some input data of any size and creates a summary or digest of that data
md5sum, sha1sum, sha256sum and sha512sum produce their outputs in hexadecimal format
Hash collision is when two different input provides the same output
Total number of hash values is 2^number of bits
A rainbow table is a lookup table of hashes to plaintext
A salt is a randomly generated value stored in the database and should be unique to each user
This ensures that when a plaintext is hashed they don’t produce the same result
On linux, password hashes are stored in /etc/shadow, which is only readable by root. They used to be stored in /etc/passwd, which was readable by everyone
The shadow file contains the password information. Each line contains nine fields, separated by colons (:). The first two fields are the login name and the encrypted password
The encrypted password field contains the hashed passphrase with four components: prefix (algorithm id), options (parameters), salt, and hash. It is saved in the format $prefix$options$salt$hash. The prefix makes it easy to recognise Unix and Linux-style passwords; it specifies the hashing algorithm used to generate the hash. For prefix, $y$ is the best.
The prefix shows what type of hash it is
Hashcat syntax:
Hashcat -m <hash_type> -a <attack_mode> hashfile wordlist (to know which hash_type to use-search online)
Hashing can also be used to check if a file has been changed as if you put the same data in you always get the same data out
HMAC (Keyed-hash message authentication code) is a type of message authentication code that uses a cryptographic hash function in combination with a secret key to verify the authenticity and integrity of data
How HMAC works:
1. The secret key is padded to the block size of the hash function
2. The padded key is XORed with a constant (usually a block of zeros or ones)
3. The message is hashed using the hash function with the XORed key
4. The result from Step 3 is then hashed again with the same hash function but using the padded key XORed with another constant
5. The final output is the HMAC value, typically a fixed-size string
Overall, HMAC is calculated by doing: HMAC(K,M) = H((K⊕opad)||H((K⊕ipad)||M)) 
Note: hashing is irreversible
Base64 can be used to encrypt and decrypt a plaintext as well
To decode: base64 -d <encrypted_text> 
To encode: base64 <plain_text>
John the ripper: The basics:
The basic syntax of john the ripper commands are:
john [options] [file path]
To automatically detect what type of hash its being given do:
John –wordlist=[path to wordlist] [path to file]
To identify the hash use an online hash identifier or use a tool called hash-identifier
To use hash-identifier, you can use wget or curl to download the python file hash-id.py from its gitlab page. To launch it do: python3 hash_id.py
If you have identified the hash you can do:
john –format=[format] –wordlist=[path to wordlist] [path to file]
If you’re dealing with a standard hash type e.g. md5, you have to prefix it with raw-. That might now always be the case
You can list all of John’s format by doing: john –list=formats (then check manually)
Or you can grep for your hash type using something like: john –list-formats | grep -iF “md5”
NThash is the hash format modern windows operating system machines use to store user and service passwords. Commonly refers to NTLM
In windows, SAM (security account manager) is used to store user account information, including usernames and hashed passwords. You can acquire NThash hashes by dumping the SAM database on a windows machine, using a tool like Minikatz, or using the active dictionary database: NTDS.dit
The format flag for NThash in john is –format=nt
To crack /etc/shadow passwords, you must combine it with the /etc/passwd file for john to understand. To do this, use a tool called unshadow. The syntax for that is:
Unshadow [path to passwd] [path to shadow] 
E.g. unshadow local_passwd local_shadow > unshadowed.txt
Local password contains the /etc/passwd line for the root user:
root:x:0:0::/root:/bin/bash
Local shadow line for the root user:
root:$6$2nwjN454g.dv4HN/$m9Z/r2xVfweYVkrr.v5Ft8Ws3/YYksfNwq96UL1FX0OJjY1L6l.DS3KEVsZ9rOVLB/ldTeEL/OIhJZ4GMFMGA0:18576:::::: 
Single crack mode in john, you can use the information provided in the username and guess the password
To use single crack mode do:
john –single –format=[format] [path to file]
Also you have to change the file that contains the hash
From: 1efee03cdcb96d90ad48ccc7b8666033 
To: mike:1efee03cdcb96d90ad48ccc7b8666033 
When mike is the username
To create custom rules:
The first line: [List.Rules:THMRules] (is used to define the name of your rule, this is what you will use to call your custom rule a John argument
We then use a regex style pattern match to define where the word will be modified
Az: Takes the word and appends it with the characters you made
A0: Takes the word and prepends it with the characters you define
c: Capitalises the character positionally
To define what characters should be appended, prepended or otherwise included. We do this by adding character sets in square brackets [] where they should be used. These follow the modifier patterns inside double quotes “”, examples:
[0-9]: will include numbers 0-9
[0]: will include only the number 0
[A-z]: will include both upper and lowercase
[A-Z]: will include only uppercase
[a-z]: will include only lowercase 
[a]: will include only a
[!”£$%]: will include only the symbols !”£$%
Adding all together a rule entry will looks like this:
[List.Rules:PoloPassword]
cAz”[0-9] [!£$%@]”
meaning: 
c: capitalizes the first letter
Az: Appends to the end of the world
[0-9]: A number in the range 0-9
[!£$%@]: the password is followed by one of these symbols
To use this rule:
john –wordlist=[path to wordlist] –rule=Polopassword [path to file]
Zip2john is a tool that converts the zip files into a hash format that John can understand and hopefully crack. The primary usage is like:
zip2john [option] [zip file] > [output file]
[options]: Allows you to pass specific checksum options to zip2john; this shouldn’t be necessary
For rar folders to decompress the file you can use rar2john 
Rar2john [rar file] > [output file]
To use john to crack the ssh private key password of id_rsa files
You need to use ssh2john, example:
ssh2john [id_rsa private key file] > [output file]
