#Steganographic Message System

Steganographic pipeline that encrypts text with AES, wraps the AES key with RSA, and hides ciphertext inside a PNG using least significant bit (LSB) embedding.

##Highlights

- End-to-end pipeline: plaintext -> AES encryption -> RSA key wrapping -> LSB steganography
- Lossless PNG embedding with exact recovery of hidden ciphertext
- Simple, repeatable CLI workflow for encryption and decryption

##Tech Stack

- Python 3
- cryptography (AES-CBC, PBKDF2, PKCS7)
- rsa (public/private key encryption)
- OpenCV (pixel-level image manipulation)

##How It Works

1. Message input: write your plaintext message in input.txt.
2. Encryption and embedding: run encryptor.py, which:
   - AES-encrypts the message.
   - RSA-encrypts the AES key.
   - Embeds the ciphertext bits into input.png using LSB replacement.
   - Produces output.png and supporting artifacts in content/ and key/.
3. Extraction and decryption: run decryptor.py, which:
   - Extracts the embedded bits from output.png.
   - Recovers the AES key via RSA decryption.
   - Decrypts the ciphertext into output.txt.

##Local Usage

Install dependencies:

```bash
pip install -r requirements.txt
```

###Encrypt a message:

```bash
python encryptor.py
```

###Outputs:
- output.png (image with hidden ciphertext)
- content/encrypted_output.txt
- content/binary_output.bin
- content/size.txt
- key/pki_encrypted.txt

###Decrypt a message:

```bash
python decryptor.py
```

###Requires:
- output.png
- key/private.pem
- key/pki_encrypted.txt

###Outputs:
- output.txt (decrypted plaintext)

##Example

1) input.txt

```
This is the hidden message, Hi how are you?
```

2) Run

```bash
python encryptor.py
python decryptor.py
```

3) output.txt

```
This is the hidden message, Hi how are you?
```

**Notes**

- Use a lossless PNG for input.png so the LSBs remain intact.
- The RSA key pair is only generated if missing; existing keys are preserved.
