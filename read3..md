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
  Private Key
</p>



Dr. Perry Xiao
