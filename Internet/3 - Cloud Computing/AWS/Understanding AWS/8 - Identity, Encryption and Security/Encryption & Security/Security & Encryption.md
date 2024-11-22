## Encryption in flight (TLS / SSL)
- Data is encrypted before sending and decrypted after receiving 
- SSL certificates help with encryption **(HTTPS)**
- Encryption in flight ensures no MITM (man in the middle attack) can happen
![[Encryption in flight TLS SSL.png]]
## Server-side encryption at rest
- Data is exncrypted after being received by the server, and decrypted before sent
- It is stored in an encrypted form thanks to a key (usually a data key)
- The encryption / decryption keys must be managed somewhere and the server must have access to it
![[Server-side encryption at rest.png]]

## Client-side encryption
- Data is encrypted by client and never decrypted by the server but by a receiving client
- Could leverage Envelope Encryption
- With Client-Side Encryption, the server doesn't need to know any information about the encryption scheme being used, as the server will not perform any encryption or decryption operations.
![[Client-side encryption.png]]

