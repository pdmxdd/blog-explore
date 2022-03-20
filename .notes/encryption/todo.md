# TODO

## Concepts

- encryption
  - decryption
    - decryption key
  - symmetrical
  - asymmetrical
  - one way: hashing
    - password storage and checking
      - rainbow table
      - dictionary attack
      - salt
      - pepper
    - signing
    - verify integrity
- RSA PKI
  - public key
    - anyone can encrypt
  - private key
    - only the owner of the private key can decrypt
  - prime number problem
  - RSA problem

## Activities
  
- hash things
  - bash
  - hashing libraries
    - Python
    - Java
    - Go
    - Node
  - bcrypt libraries (and other encryption libraries)
    - Python
    - Java
    - Go
    - Node
- encrypt something --> decrypt with key
  - bash
  - bcrypt libraries
  - creating your own elementary, non-production grade encryption library
    - Caesar Cypher
- utilize bash openSSH options:
  - ssh-keygen
  - ssh
    - signing into remote servers with pub/private keys `-i /path/to/private.key`
  - sshd
  - create your own elementary, non-production grade ssh-keygen utility
- signing GH commits!
  - how we can create an RSA pub/private keypair and provide to our local git account to get verified commits
- verifying integrity of downloaded files
  - download some files
  - hash the files
  - check our hash agains the verification hash --> if they are the same we **know** what we downloaded is **identical** (unmessed with) to the files on the server we downloaded from
- hashing, salting, peppering, rainbow tables / dictionary attacks
  - plain text passwords (dump)
    - no additional knowledge necessary
  - hashed passwords (dump)
    - must know about rainbow tables and how to create/implement a dictionary attack
  - salted passwords (dump)
    - must also know server salt
  - peppered passwords (dump)
    - must also know server pepper
  - creating your own elementary, non-production grade password hashing/salting/peppering and authentication library
