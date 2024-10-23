# Ides-le Talk
## Challange
- Cryptography
- Beginner
- DEADFACE CTF

>We intercepted a file from one of DEADFACE’s more amateur members. They claim that they hid a password in a
>text file. When we looked at the file, it’s indecipherable jibberish. We believe a common cipher was used to
>hide the message (and the password). As far as possible keys goes, maybe it can be determined by the author of the original text?
>
>Submit the flag as flag{flag-text}

```
Gur Yvsr naq Qrngu bs Whyvhf Pnrfne
Funxrfcrner ubzrcntr | Whyvhf Pnrfne | Ragver cynl
NPG V
FPRAR V. Ebzr. N fgerrg.
Ragre SYNIVHF, ZNEHYYHF, naq pregnva Pbzzbaref
SYNIVHF
Urapr! ubzr, lbh vqyr perngherf trg lbh ubzr:
Vf guvf n ubyvqnl? jung! xabj lbh abg,
Orvat zrpunavpny, lbh bhtug abg jnyx
Hcba n ynobhevat qnl jvgubhg gur fvta
Bs lbhe cebsrffvba? Fcrnx, jung genqr neg gubh?
Svefg Pbzzbare
Jul, fve, n pnecragre.
ZNEHYYHF
Jurer vf gul yrngure nceba naq gul ehyr?
Jung qbfg gubh jvgu gul orfg nccnery ba?
Lbh, fve, jung genqr ner lbh?
Frpbaq Pbzzbare
...
```
## Analysis
First you had to know with which algorithm the file was encrypted. So I entered the https://www.dcode.fr/identificador-cifrado page where you can add the encrypted text and tells you which algorithms are likely to have been encrypted.
## Solution
Having found the encryption algorithm (mono-alphabetical substitution rotation 13) I entered the https://www.dcode.fr/rot-13-cipher page where I added the text and deciphered it.
The encrypted text is The Life and Death of Julius Caesar Shakespeare in which the flag was hidden among the text:
```
...
Are levying powers: we must straight make head: Therefore let our alliance be combined, Our best friends made,
our means stretch'd And let us presently go sit in council, How covert matters may be best disclosed, And open
perils surest answered. OCTAVIUS Let us do so: for we are at the stake, And bay'd about with many enemies; And
some that smile have in their hearts, I fear, Millions of mischiefs. Exeunt flag: L3t_The#Mi$chiefs^8361n SCENE
II. Camp near Sardis. Before BRUTUS's tent. Drum. Enter BRUTUS, LUCILIUS, LUCIUS, and Soldiers; TITINIUS and PINDARUS
meeting them BRUTUS Stand, ho! LUCILIUS Give the word, ho! and stand. BRUTUS What now, Lucilius! is Cassius near?
LUCILIUS He is at hand; and Pindarus is come To do you salutation from his master. BRUTUS He greets me well. Your master, 
...
```
## How to avoid
Use more complex and secure encryption algorithms instead of simple encryption like ROT13 or César encryption. Some recommended ciphers are:
- AES (Advanced Encryption Standard): A widely used and robust symmetric encryption.
- RSA (Rivest – Shamir – Adleman): Asymmetric encryption that is used in many modern security applications.
## Solution
`flag{L3t_The#Mi$chiefs^8361n}`


