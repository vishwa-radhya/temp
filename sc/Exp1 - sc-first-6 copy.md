## Exp1 - Caesar cipher
```
function caesarCipher(text, shift) {
  let result = '';
  for (let i = 0; i < text.length; i++) {
    let char = text[i];
    if (char.match(/[a-z]/i)) { // Check if it's a letter
      const code = text.charCodeAt(i);
      let shiftedCode;

      if (char.match(/[a-z]/)) { // Lowercase
        shiftedCode = ((code - 97 + shift) % 26 + 26) % 26 + 97; // Ensure positive modulo
      } else { // Uppercase
        shiftedCode = ((code - 65 + shift) % 26 + 26) % 26 + 65; // Ensure positive modulo
      }
      result += String.fromCharCode(shiftedCode);
    } else {
      result += char; // Non-alphabetic characters remain unchanged
    }
  }
  return result;
}

// Example usage
const plaintext = "Hello, World!";
const shiftAmount = 3;
const ciphertext = caesarCipher(plaintext, shiftAmount);
console.log("Plaintext:", plaintext);
console.log("Ciphertext:", ciphertext);

const shiftedBack = caesarCipher(ciphertext, -shiftAmount);
console.log("Decrypted Text:", shiftedBack);
```
## Exp2 - Monoalphabetic cipher
```
function monoalphabeticCipher(text, key) {
  const alphabet = 'abcdefghijklmnopqrstuvwxyz';
  let result = '';

  for (let char of text.toLowerCase()) {
    if (alphabet.includes(char)) {
      const index = alphabet.indexOf(char);
      result += key[index];
    } else {
      result += char; // Non-alphabetic characters remain unchanged
    }
  }
  return result;
}

function monoalphabeticDecipher(ciphertext, key) {
  const alphabet = 'abcdefghijklmnopqrstuvwxyz';
  let result = '';

  for (let char of ciphertext.toLowerCase()) {
    const index = key.indexOf(char);
    if (index !== -1) {
      result += alphabet[index];
    } else {
      result += char;
    }
  }
  return result;
}

// Example usage
const plaintext = "hello world";
const key = "qwertyuiopasdfghjklzxcvbnm"; // Example key

const ciphertext = monoalphabeticCipher(plaintext, key);
console.log("Ciphertext:", ciphertext);

const decryptedText = monoalphabeticDecipher(ciphertext, key);
console.log("Decrypted Text:", decryptedText);
```
## Exp3 - Message Authentication code
```
import hashlib

def calculate_hashes(text):
    """Calculates and prints SHA hashes for the given text."""

    encoded_text = text.encode()

    # SHA-256
    sha256_hash = hashlib.sha256(encoded_text).hexdigest()
    print(f"The hexadecimal equivalent of SHA256 is: {sha256_hash}")

    # SHA-384
    sha384_hash = hashlib.sha384(encoded_text).hexdigest()
    print(f"The hexadecimal equivalent of SHA384 is: {sha384_hash}")

    # SHA-224
    sha224_hash = hashlib.sha224(encoded_text).hexdigest()
    print(f"The hexadecimal equivalent of SHA224 is: {sha224_hash}")

    # SHA-512
    sha512_hash = hashlib.sha512(encoded_text).hexdigest()
    print(f"The hexadecimal equivalent of SHA512 is: {sha512_hash}")

    # SHA-1
    sha1_hash = hashlib.sha1(encoded_text).hexdigest()
    print(f"The hexadecimal equivalent of SHA1 is: {sha1_hash}")

 Example usage
text = "GeeksforGeeks"
calculate_hashes(text)

 Example to verify data integrity:
original_text = "Data to be protected"
original_hash = hashlib.sha256(original_text.encode()).hexdigest()

 Simulate data transmission/storage (potentially altered)
received_text = "Data to be protected" # or "Data to be protected with a change"

received_hash = hashlib.sha256(received_text.encode()).hexdigest()

if original_hash == received_hash:
    print("\nData integrity verified: Hashes match.")
else:
    print("\nData integrity compromised: Hashes do not match.")
```
## Exp 4 - Data encryption standard
```
from Crypto.Cipher import DES
from Crypto.Util.Padding import pad, unpad

def des_encrypt(plaintext, key):
    """Encrypts plaintext using DES."""
    cipher = DES.new(key, DES.MODE_ECB)
    padded_plaintext = pad(plaintext.encode('utf-8'), DES.block_size)
    ciphertext = cipher.encrypt(padded_plaintext)
    return ciphertext

def des_decrypt(ciphertext, key):
    """Decrypts ciphertext using DES."""
    cipher = DES.new(key, DES.MODE_ECB)
    padded_plaintext = cipher.decrypt(ciphertext)
    plaintext = unpad(padded_plaintext, DES.block_size).decode('utf-8')
    return plaintext

 Example usage
key = b'abcdefgh'  # 8-byte key
plaintext = "This is a secret message."

ciphertext = des_encrypt(plaintext, key)
print("Ciphertext:", ciphertext.hex())

decrypted_plaintext = des_decrypt(ciphertext, key)
print("Decrypted plaintext:", decrypted_plaintext)
```
## Exp 5 - Advanced Encryption Standard
```
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad, unpad
import os

def aes_encrypt(plaintext, key):
    """Encrypts plaintext using AES-256-CBC."""
    iv = os.urandom(16)  # Generate a random 16-byte IV
    cipher = AES.new(key, AES.MODE_CBC, iv)
    padded_plaintext = pad(plaintext.encode('utf-8'), AES.block_size)
    ciphertext = iv + cipher.encrypt(padded_plaintext) # iv prepended to ciphertext
    return ciphertext

def aes_decrypt(ciphertext, key):
    """Decrypts ciphertext using AES-256-CBC."""
    iv = ciphertext[:16] # extract the IV
    cipher = AES.new(key, AES.MODE_CBC, iv)
    padded_plaintext = cipher.decrypt(ciphertext[16:]) # decrypt the ciphertext without the IV
    plaintext = unpad(padded_plaintext, AES.block_size).decode('utf-8')
    return plaintext

 Example usage (AES-256)
key = os.urandom(32)  # 32-byte key (256 bits)
plaintext = "This is a secret message."

ciphertext = aes_encrypt(plaintext, key)
print("Ciphertext (hex):", ciphertext.hex())

decrypted_plaintext = aes_decrypt(ciphertext, key)
print("Decrypted plaintext:", decrypted_plaintext)
```
## Exp 6 - Asymmetric Key Encryption
```
import math

def gcd(a, b):
    """Calculates the greatest common divisor of a and b."""
    while b:
        a, b = b, a % b
    return a

def generate_keypair(p, q):
    """Generates RSA public and private keys."""
    n = p * q
    t = (p - 1) * (q - 1)

    # Find public key e
    e = 2
    while e < t:
        if gcd(e, t) == 1:
            break
        e += 1

    # Find private key d
    d = 1
    while (d * e) % t != 1:
        d += 1

    return (e, n), (d, n)

def encrypt(plaintext, public_key):
    """Encrypts the plaintext using the public key."""
    e, n = public_key
    ciphertext = pow(plaintext, e, n)
    return ciphertext

def decrypt(ciphertext, private_key):
    """Decrypts the ciphertext using the private key."""
    d, n = private_key
    plaintext = pow(ciphertext, d, n)
    return plaintext

 Example usage
p = 53
q = 59
plaintext = 89

public_key, private_key = generate_keypair(p, q)

ciphertext = encrypt(plaintext, public_key)
print("Ciphertext:", ciphertext)

decrypted_plaintext = decrypt(ciphertext, private_key)
print("Decrypted plaintext:", decrypted_plaintext)
```