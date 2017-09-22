---
layout: post
title: "Maximize your Yubikey NEO"
date: 2015-10-19 10:05
comments: true
published: true
categories: Development
tags: [Security,Tutorial]
---
# Maximize your YubiKey
**TODO: Brief introduction to the article**

## PGP
**TODO: Brief introduction to ppg**

### Generating a GPG Key Pair

**TODO: Flesh out this section.**

We are going to generate a new master key.
Because this key will be kept in storage, it doesn't need signing or encryption capabilities.

It is going to be 4096 bits long.

It will not expire (controversial)

Enter your name and email address.

Enter a strong passphrase

```bash
❯ gpg2 --expert --gen-key
gpg (GnuPG) 2.0.29; Copyright (C) 2015 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

gpg: directory `/Users/mike/.gnupg' created
gpg: new configuration file `/Users/mike/.gnupg/gpg.conf' created
gpg: WARNING: options in `/Users/mike/.gnupg/gpg.conf' are not yet active during this run
gpg: keyring `/Users/mike/.gnupg/secring.gpg' created
gpg: keyring `/Users/mike/.gnupg/pubring.gpg' created
Please select what kind of key you want:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (7) DSA (set your own capabilities)
   (8) RSA (set your own capabilities)
Your selection? 8

Possible actions for a RSA key: Sign Certify Encrypt Authenticate
Current allowed actions: Sign Certify Encrypt

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? s

Possible actions for a RSA key: Sign Certify Encrypt Authenticate
Current allowed actions: Certify Encrypt

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? e

Possible actions for a RSA key: Sign Certify Encrypt Authenticate
Current allowed actions: Certify

   (S) Toggle the sign capability
   (E) Toggle the encrypt capability
   (A) Toggle the authenticate capability
   (Q) Finished

Your selection? q
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048) 4096
Requested keysize is 4096 bits       
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 0
Key does not expire at all
Is this correct? (y/N) y

GnuPG needs to construct a user ID to identify your key.

Real name: Mike Kusold
Email address: hello@mikekusold.com
Comment: https://mikekusold.com    
You selected this USER-ID:     
    "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O
You need a Passphrase to protect your secret key.    

We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.
gpg: /Users/mike/.gnupg/trustdb.gpg: trustdb created
gpg: key 9D5F9D08 marked as ultimately trusted
public and secret key created and signed.

gpg: checking the trustdb
gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
pub   4096R/9D5F9D08 2015-11-14
      Key fingerprint = 92F3 103D F728 A154 5611  C490 04A2 5CE8 9D5F 9D08
uid       [ultimate] Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>


❯
```

Set your hash preferences.

```bash
❯ gpg2 --edit-key hello@mikekusold.com
gpg (GnuPG) 2.0.29; Copyright (C) 2015 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

pub  4096R/9D5F9D08  created: 2015-11-14  expires: never       usage: C   
                     trust: ultimate      validity: ultimate
[ultimate] (1). Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>

gpg> setpref SHA512 SHA384 SHA256 SHA224 AES256 AES192 AES CAST5 ZLIB BZIP2 ZIP Uncompressed
Set preference list to:                                                                     
     Cipher: AES256, AES192, AES, CAST5, 3DES
     Digest: SHA512, SHA384, SHA256, SHA224, SHA1
     Compression: ZLIB, BZIP2, ZIP, Uncompressed
     Features: MDC, Keyserver no-modify
Really update the preferences? (y/N) y

You need a passphrase to unlock the secret key for
user: "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>"
4096-bit RSA key, ID 9D5F9D08, created 2015-11-14


pub  4096R/9D5F9D08  created: 2015-11-14  expires: never       usage: C   
                     trust: ultimate      validity: ultimate
[ultimate] (1). Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
```

Add any secondary uids. I'll be adding my keybase account.

```bash
gpg> adduid
Real name: Mike Kusold
Email address: mike@keybase.io
Comment: https://keybase.io/mike
You selected this USER-ID:      
    "Mike Kusold (https://keybase.io/mike) <mike@keybase.io>"

Change (N)ame, (C)omment, (E)mail or (O)kay/(Q)uit? O

You need a passphrase to unlock the secret key for
user: "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>"
4096-bit RSA key, ID 9D5F9D08, created 2015-11-14


pub  4096R/9D5F9D08  created: 2015-11-14  expires: never       usage: C   
                     trust: ultimate      validity: ultimate
[ultimate] (1)  Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
[ unknown] (2). Mike Kusold (https://keybase.io/mike) <mike@keybase.io>
```

Optional: Add a photo
Photo should be smaller than 240x288 and 6KB

```bash

gpg> addphoto

Pick an image to use for your photo ID.  The image must be a JPEG file.
Remember that the image is stored within your public key.  If you use a
very large picture, your key will become very large as well!
Keeping the image close to 240x288 is a good size to use.

Enter JPEG filename for photo ID: ~/Pictures/PersonalPhoto-lo.jpg
Is this photo correct (y/N/q)? y

You need a passphrase to unlock the secret key for
user: "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>"
4096-bit RSA key, ID 9D5F9D08, created 2015-11-14


pub  4096R/9D5F9D08  created: 2015-11-14  expires: never       usage: C   
                     trust: ultimate      validity: ultimate
[ultimate] (1)  Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
[ unknown] (2). Mike Kusold (https://keybase.io/mike) <mike@keybase.io>
[ unknown] (3)  [jpeg image of size 5884]
```

Set your primary uid
The first uid selects which uid. The second deselects.

```bash
gpg> uid 1

sec  4096R/9D5F9D08  created: 2015-11-14  expires: never     
(1)* Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
(2). Mike Kusold (https://keybase.io/mike) <mike@keybase.io>
(3)  [jpeg image of size 5884]

gpg> primary
Please use the command "toggle" first.

gpg> primary

You need a passphrase to unlock the secret key for
user: "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>"
4096-bit RSA key, ID 9D5F9D08, created 2015-11-14


pub  4096R/9D5F9D08  created: 2015-11-14  expires: never       usage: C   
                     trust: ultimate      validity: ultimate
[ultimate] (1)* Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
[ unknown] (2)  Mike Kusold (https://keybase.io/mike) <mike@keybase.io>
[ unknown] (3)  [jpeg image of size 5884]

gpg> uid 1

pub  4096R/9D5F9D08  created: 2015-11-14  expires: never       usage: C   
                     trust: ultimate      validity: ultimate
[ultimate] (1). Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
[ unknown] (2)  Mike Kusold (https://keybase.io/mike) <mike@keybase.io>
[ unknown] (3)  [jpeg image of size 5884]
```

Add Keyserver. You will upload your key to this address later.

```bash
gpg> keyserver
Enter your preferred keyserver URL: https://mikekusold.com/assets/hello_mikekusold_com.public.gpg    

You need a passphrase to unlock the secret key for
user: "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>"
4096-bit RSA key, ID 9D5F9D08, created 2015-11-14


pub  4096R/9D5F9D08  created: 2015-11-14  expires: never       usage: C   
                     trust: ultimate      validity: ultimate  
[ultimate] (1). Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
[ unknown] (2)  Mike Kusold (https://keybase.io/mike) <mike@keybase.io>
[ unknown] (3)  [jpeg image of size 5884]
```

Create an encryption subkey

```bash
gpg> addkey
Key is protected.

You need a passphrase to unlock the secret key for
user: "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>"
4096-bit RSA key, ID 9D5F9D08, created 2015-11-14

Please select what kind of key you want:
   (3) DSA (sign only)
   (4) RSA (sign only)
   (5) Elgamal (encrypt only)
   (6) RSA (encrypt only)
Your selection? 6
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048) 2048
Requested keysize is 2048 bits       
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 1y
Key expires at Sun Nov 13 16:37:17 2016 MST
Is this correct? (y/N) y
Really create? (y/N) y  
We need to generate a lot of random bytes. It is a good idea to perform
some other action (type on the keyboard, move the mouse, utilize the
disks) during the prime generation; this gives the random number
generator a better chance to gain enough entropy.

pub  4096R/9D5F9D08  created: 2015-11-14  expires: never       usage: C   
                     trust: ultimate      validity: ultimate
sub  2048R/05ECB8BF  created: 2015-11-14  expires: 2016-11-13  usage: E   
[ultimate] (1). Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
[ unknown] (2)  Mike Kusold (https://keybase.io/mike) <mike@keybase.io>
[ unknown] (3)  [jpeg image of size 5884]
```

Save your changes

```bash
gpg> save

❯
```

Generate revocation certificate

```bash
❯ gpg2 --output hello@mikekusold.com.gpg-revocation-certificate.asc --gen-revoke hello@mikekusold.com

sec  4096R/9D5F9D08 2015-11-14 Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>

Create a revocation certificate for this key? (y/N) y
Please select the reason for the revocation:         
  0 = No reason specified
  1 = Key has been compromised
  2 = Key is superseded
  3 = Key is no longer used
  Q = Cancel
(Probably you want to select 1 here)
Your decision? 1
Enter an optional description; end it with an empty line:
> This key has been lost or stolen
>                                 
Reason for revocation: Key has been compromised
This key has been lost or stolen
Is this okay? (y/N) y

You need a passphrase to unlock the secret key for
user: "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>"
4096-bit RSA key, ID 9D5F9D08, created 2015-11-14

ASCII armored output forced.
Revocation certificate created.

Please move it to a medium which you can hide away; if Mallory gets
access to this certificate he can use it to make your key unusable.
It is smart to print this certificate and store it away, just in case
your media become unreadable.  But have some caution:  The print system of
your machine might store the data and make it available to others!

❯

```

Export your secret keys

--armor to allow easy  printing  of  the  key  for  paper backup;

```bash
❯ gpg2 --export-secret-keys --armor hello@mikekusold.com > hello@mikekusold.com.private.gpg
```

Delete the secret keys then reimport them to validate our backup

It also ensures that we are working with the correct private key

```
~
❯ gpg2 --delete-secret-keys hello@mikekusold.com
gpg (GnuPG) 2.0.29; Copyright (C) 2015 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.


sec  4096R/9D5F9D08 2015-11-14 Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>

Delete this key from the keyring? (y/N) y
This is a secret key! - really delete? (y/N) y

~
❯ gpg2 --import hello@mikekusold.com.private.gpg
gpg: key 9D5F9D08: secret key imported
gpg: key 9D5F9D08: "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>" 2 new signatures
gpg: Total number processed: 1
gpg:         new signatures: 2
gpg:       secret keys read: 1
gpg:   secret keys imported: 1
```

Plug in your Yubikey
Add a signing key to your yubikey

The default admin yubikey pin: 12345678
The default user yubikey pin: 123456

```
~
❯ gpg2 --edit-key hello@mikekusold.com
gpg (GnuPG) 2.0.29; Copyright (C) 2015 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Secret key is available.

gpg: checking the trustdb
gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
pub  4096R/9D5F9D08  created: 2015-11-14  expires: never       usage: C   
                     trust: ultimate      validity: ultimate
sub  2048R/05ECB8BF  created: 2015-11-14  expires: 2016-11-13  usage: E   
[ultimate] (1). Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
[ultimate] (2)  Mike Kusold (https://keybase.io/mike) <mike@keybase.io>
[ultimate] (3)  [jpeg image of size 5884]

gpg> addcardkey
scdaemon[20907]: pcsc_control failed: invalid parameter (0x80100004)
scdaemon[20907]: pcsc_vendor_specific_init: GET_FEATURE_REQUEST failed: 65538
Signature key ....: 8C28 FDFC 95B5 EB01 012F  40CA 5144 4FE4 2685 649E
Encryption key....: 3720 ADC4 256C 5D0B 3D86  B7C7 1501 851A 730A 9EB3
Authentication key: 1BFB 0AF0 447A 48CC C5DC  83E4 6781 6067 28FE DF79

Please select the type of key to generate:
   (1) Signature key
   (2) Encryption key
   (3) Authentication key
Your selection? scdaemon[20907]: updating slot 0 status: 0x0000->0x0007 (0->1)
1

gpg: WARNING: such a key has already been stored on the card!

Replace existing key? (y/N) y
scdaemon[20907]: 3 Admin PIN attempts remaining before card is permanently locked
scdaemon[20907]: DBG: asking for PIN '|A|Please enter the Admin PIN'
scdaemon[20907]: DBG: asking for PIN '||Please enter the PIN'
Key is protected.

You need a passphrase to unlock the secret key for
user: "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>"
4096-bit RSA key, ID 9D5F9D08, created 2015-11-14

Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 1y
Key expires at Sun Nov 13 16:46:30 2016 MST
Is this correct? (y/N) y
Really create? (y/N) y  
scdaemon[20907]: existing key will be replaced
scdaemon[20907]: please wait while key is being generated ...
scdaemon[20907]: key generation completed (44 seconds)
scdaemon[20907]: signatures created so far: 0
scdaemon[20907]: signatures created so far: 1

pub  4096R/9D5F9D08  created: 2015-11-14  expires: never       usage: C   
                     trust: ultimate      validity: ultimate
sub  2048R/05ECB8BF  created: 2015-11-14  expires: 2016-11-13  usage: E   
sub  2048R/DE88F708  created: 2015-11-14  expires: 2016-11-13  usage: S   
[ultimate] (1). Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
[ultimate] (2)  Mike Kusold (https://keybase.io/mike) <mike@keybase.io>
[ultimate] (3)  [jpeg image of size 5884]
```

Add authentication key

```bash
gpg> addcardkey
Signature key ....: 262A C9E6 0ECD 718E CB07  19CA 39CA 1316 DE88 F708
Encryption key....: 3720 ADC4 256C 5D0B 3D86  B7C7 1501 851A 730A 9EB3
Authentication key: 1BFB 0AF0 447A 48CC C5DC  83E4 6781 6067 28FE DF79

Please select the type of key to generate:
   (1) Signature key
   (2) Encryption key
   (3) Authentication key
Your selection? 3

gpg: WARNING: such a key has already been stored on the card!

Replace existing key? (y/N) y
Key is protected.            

You need a passphrase to unlock the secret key for
user: "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>"
4096-bit RSA key, ID 9D5F9D08, created 2015-11-14

Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 1y
Key expires at Sun Nov 13 16:47:32 2016 MST
Is this correct? (y/N) y
Really create? (y/N) y  
scdaemon[20907]: existing key will be replaced
scdaemon[20907]: please wait while key is being generated ...
scdaemon[20907]: key generation completed (23 seconds)

pub  4096R/9D5F9D08  created: 2015-11-14  expires: never       usage: C   
                     trust: ultimate      validity: ultimate
sub  2048R/05ECB8BF  created: 2015-11-14  expires: 2016-11-13  usage: E   
sub  2048R/DE88F708  created: 2015-11-14  expires: 2016-11-13  usage: S   
sub  2048R/A621B8EC  created: 2015-11-14  expires: 2016-11-13  usage: A   
[ultimate] (1). Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
[ultimate] (2)  Mike Kusold (https://keybase.io/mike) <mike@keybase.io>
[ultimate] (3)  [jpeg image of size 5884]
```

Send encryption key to yubikey

```bash
gpg> toggle

sec  4096R/9D5F9D08  created: 2015-11-14  expires: never     
ssb  2048R/05ECB8BF  created: 2015-11-14  expires: never     
ssb  2048R/DE88F708  created: 2015-11-14  expires: 2016-11-13
                     card-no: 0006 03632643
ssb  2048R/A621B8EC  created: 2015-11-14  expires: 2016-11-13
                     card-no: 0006 03632643
(1)  Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
(2)  Mike Kusold (https://keybase.io/mike) <mike@keybase.io>

gpg> key 1

sec  4096R/9D5F9D08  created: 2015-11-14  expires: never     
ssb* 2048R/05ECB8BF  created: 2015-11-14  expires: never     
ssb  2048R/DE88F708  created: 2015-11-14  expires: 2016-11-13
                     card-no: 0006 03632643
ssb  2048R/A621B8EC  created: 2015-11-14  expires: 2016-11-13
                     card-no: 0006 03632643
(1)  Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
(2)  Mike Kusold (https://keybase.io/mike) <mike@keybase.io>

gpg> keytocard
Signature key ....: 262A C9E6 0ECD 718E CB07  19CA 39CA 1316 DE88 F708
Encryption key....: 3720 ADC4 256C 5D0B 3D86  B7C7 1501 851A 730A 9EB3
Authentication key: 7828 7349 3D70 DE94 93D5  74CA BA4D 6686 A621 B8EC

Please select where to store the key:
   (2) Encryption key
Your selection? 2

gpg: WARNING: such a key has already been stored on the card!

Replace existing key? (y/N) y

You need a passphrase to unlock the secret key for
user: "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>"

scdaemon[20907]: existing key will be replaced

sec  4096R/9D5F9D08  created: 2015-11-14  expires: never     
ssb* 2048R/05ECB8BF  created: 2015-11-14  expires: never     
                     card-no: 0006 03632643
ssb  2048R/DE88F708  created: 2015-11-14  expires: 2016-11-13
                     card-no: 0006 03632643
ssb  2048R/A621B8EC  created: 2015-11-14  expires: 2016-11-13
                     card-no: 0006 03632643
(1)  Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>
(2)  Mike Kusold (https://keybase.io/mike) <mike@keybase.io>
```

Save your changes

```bash
gpg> save
scdaemon[20907]: scdaemon (GnuPG) 2.0.29 stopped

❯
```

Export your public key

```bash
❯ gpg2 --export --armor hello@mikekusold.com > hello@mikekusold.com.public.gpg
```

Back up your .gnupg directory
```
❯ mv .gnupg .gnupg.bak
```

Import your public key

```
❯ gpg2 --import hello@mikekusold.com.public.gpg
gpg: directory `/Users/mike/.gnupg' created
gpg: new configuration file `/Users/mike/.gnupg/gpg.conf' created
gpg: WARNING: options in `/Users/mike/.gnupg/gpg.conf' are not yet active during this run
gpg: keyring `/Users/mike/.gnupg/secring.gpg' created
gpg: keyring `/Users/mike/.gnupg/pubring.gpg' created
gpg: /Users/mike/.gnupg/trustdb.gpg: trustdb created
gpg: key 9D5F9D08: public key "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
```


❯ gpg2 --keyserver hkps://hkps.pool.sks-keyservers.net --send-key 9D5F9D08
gpg: sending key 9D5F9D08 to hkps server hkps.pool.sks-keyservers.net

~/dotfiles master*
❯ echo "Test Encrypt And Decrypt" | gpg2 --encrypt --recipient hello@mikekusold.com > test.asc

~/dotfiles master*
❯ gpg2 -d test.asc
scdaemon[30005]: pcsc_control failed: invalid parameter (0x80100004)
scdaemon[30005]: pcsc_vendor_specific_init: GET_FEATURE_REQUEST failed: 65538
scdaemon[30005]: DBG: asking for PIN '||Please enter the PIN'
gpg: encrypted with 2048-bit RSA key, ID 05ECB8BF, created 2015-11-14
      "Mike Kusold (https://mikekusold.com) <hello@mikekusold.com>"
Test Encrypt And Decrypt

~/dotfiles master* 10s
❯ scdaemon[30005]: scdaemon (GnuPG) 2.0.29 stopped



--------------------------
Now we are going to remove the master key from our keychain. GPG doesn't provide an easy way, so we are going to export the subkeys, delete our keys from our keychain, then reimport them.

Validate that our master key is no longer in the keychain.
Notice the pound sign in `sec#`. This means that the masterkey's private key is not located in the keychain.

```
~
❯ gpg --list-secret-keys
gpg: checking the trustdb
gpg: 3 marginal(s) needed, 1 complete(s) needed, PGP trust model
gpg: depth: 0  valid:   1  signed:   0  trust: 0-, 0q, 0n, 0m, 0f, 1u
/Users/mike/.gnupg/secring.gpg
------------------------------
sec#  4096R/6653E2A0 2015-11-14
uid                  Mike Kusold <hello@mikekusold.com>
uid                  Mike Kusold (https://keybase.io/mike) <mike@keybase.io>
ssb   4096R/EAA103A0 2015-11-14
ssb   2048R/B6AFD7D5 2015-11-14
ssb   2048R/D4C94508 2015-11-14


~
❯
```

# TODO: Explain how to set up the yubikey. Mode 84 (most likely)
# TODO: Probably didn't want to create subkeys.
