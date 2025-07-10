# FantaCrypt

AO1 originally encrypted the headers of the packets sent by the client to the server.

- FantaCrypt seeds a PRNG with a known value (the decryption key), where the lower two bytes are used in the PRNG, and the upper byte of those two bytes is used in the cipher.
- The current key is XOR'ed with the current byte.
- To get the key value for the next byte, the byte and key are added together.
- Then, some multiplication and addition is done to that value. (This PRNG behavior is similar to a [linear congruential generator](https://en.wikipedia.org/wiki/Linear_congruential_generator).)
- Finally, everything but the lower two bytes are discarded.

However, there was a major flaw in the implementation of FantaCrypt: the server supplies the client with the initial key to use for every packet. Thus, it is totally vulnerable to a replay attack.

Many clients do not actually even implement the FantaCrypt algorithm, but instead use a hardcoded key of 5, which is sent as 0x34 in the 'decryptor' packet. This is because the 'decryptor' packet argument is the key to be used, but encrypted. The decryptor value is always encrypted with a magic number key value: 322 decimal. Moreover, FantaCrypt is only used in messages sent by the client to the server and not vice versa.

The encryption algorithm comes from this [Stack Overflow answer](https://stackoverflow.com/questions/6798188/delphi-simple-string-encryption).
