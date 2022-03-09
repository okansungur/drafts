### Encryption


__Encryption__ is one of the oldest security techniques; it dates back at least to the Roman era, when a cipher encryption, now known as the Caesar cipher, was named after the emperor Julius Caesar. In this simple shift cipher, letters in the text are shifted a fixed number of places down the alphabet to make them unreadable.
For example, if you shift each letter three places to the left in the alphabet, the following clear text: 

attack london

will become the following:

xqqxzh ilkalk

Modern encryption techniques can be classified as either **private key encryption** (also called symmetric key encryption) or
**public key encryption** (also called asymmetric key encryption). The technical field of study underlying encryption and secure communication is known as cryptography.


###### Private Key Encryption
In private key encryption, the sender and the receiver use the same key to encrypt the message and to decrypt the message
You can imagine the private key as simply a sequence of random numbers. Because both parties use the same key, this is also called symmetric key encryption.

The most popular private key encryption is Advanced Encryption Standard (AES). In AES, plain text is divided into 128-bit blocks. The
secret key, which can be 128, 192, or 256 bits, is expanded into several round keys through a key expansion algorithm



<p align="center">
  <img  src="https://github.com/okansungur/drafts/blob/main/Misc/private.png"><br/>
   Private key encryption
</p>



######  Public Key Encryption
In contrast to private key encryption, with public key encryption the sender and receiver use different keys to encrypt the message and to decrypt the message.

First, everyone needs to generate a pair of their own keys, one private key and one public key. They will keep the private key to themselves and give the public key to everyone else. Then, if X wants to send a secret message to Y only, X will use Y’s public key to encrypt the message, and Y will use Y’s private key to decrypt the message. In this case, only Y can decrypt the message. Because senders and receivers use different keys, this is also called asymmetric key encryption. The beauty of this encryption method is
that there is no key distribution issue; others can never figure out what your private key is from your public key.


Public key encryption can also be used for authentication purposes. For example, to prove that the message is coming from X, sender X can encrypt
the message using X’s private key, and receiver Y will know it is indeed coming from X if Y can decrypt the message using X’s public key

The most popular public key encryption is Rivest–Shamir–Adelman (RSA), designed by Ron Rivest, Adi Shamir, and Leonard Adelman in 1977. The RSA
algorithm involves three key steps: key generation, encryption, and decryption



<p align="center">
  <img  src="https://github.com/okansungur/drafts/blob/main/Misc/public.png"><br/>
  Public key encryption
</p>



Dr. Perry Xiao
