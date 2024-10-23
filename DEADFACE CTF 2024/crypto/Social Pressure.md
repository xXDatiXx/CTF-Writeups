# Social Pressure
## Challenge
- Cryptography
- DEADFACE CTF
- Beginner

### Description

>We intercepted this chat log between luciafer and lilith. We believe they’re discussing who or what they will target for their social engineering campaign.
>Decode the message and submit the name of the target. The key is to remember that sometimes the simplest solutions are just waiting to be reflected upon.
>Submit the flag as flag(First_Last).

```
luciafer 1:19 PM
Svb ororgs, yrt mvdh! Dv'iv tlrmt zugvi Wv Nlmmv Urmzmxrzo mvcg. Gsvri hvxfirgb nvzhfivh szev hlnv slovh gszg dv'iv tlmmz vckolrg yrt grnv! R'ev yvvm klprmt zilfmw zmw ulfmw hlnv HJO efomvizyrorgrvh dv xzm oveviztv uli nzcrnfn xszlh.

Ivnvnyvi gszg RG tfb R nvmgrlmvw yvuliv? Gfimh lfg, sv’h z ivzo xszggb Xzgsb lm hlxrzo nvwrz. Gsrmp dv xzm fhv hlnv tllw lo' hlxrzo vmtrmvvirmt gl lfi zwezmgztv. Dv’oo tvg srn hkvdrmt kzhhdliwh orpv z ovzpb uzfxvg. Kofh, drgs blfi LHRMG hprooh zmw nb HJO nztrx, gsvb dlm’g hvv dszg srg 'vn.

lilith 1:20 PM
R'ev zoivzwb hgzigvw hlnv LHRMG ivxlm zmw tfvhh dszg? Ulfmw hlnv qfrxb wvvgh zylfg gsvri RG gvzn lm OrmpvwRm. Kvlkov levihsziv hl nfxs, rg'h kizxgrxzoob z tlownrmv. Hlxrzo vmtrmvvirmt gszg xszggb wfwv hslfow yv z yivvav; R'oo xizug z ovtvmw gszg'oo szev srn hkroormt vevibgsrmt.

luciafer 1:21 PM
Bzzzh, gsrh rh tlmmz yv ovtvmwzib! Olermt gsv vmgsfhrzhn. Zmw Voilb Lmtzil? Gszg tfb'h kizxgrxzoob iloormt lfg gsv ivw xzikvg uli fh drgs sld nfxs sv hszivh lmormv. Xzm'g yvorvev sld vzhb hlnv lu gsvhv gzitvgh nzpv rg.

lilith 1:22 PM
Zyhlofgvob! Voilb Lmtzil szh ml rwvz dszg'h xlnrmt srh dzb. R'ev zoivzwb tlg z uvd zmtovh rm nrmw gl tvg srn gzoprmt. Hlxrzo vmtrmvvirmt gsvhv gbkvh rh zodzbh z gsiroo.

R'oo hgzig wizugrmt hlnv kvihlmzh zmw hxirkgh. Lmxv sv'h fmwvi lfi rmuofvmxv, dv xzm lixsvhgizgv gsv HJO vckolrg hvznovhhob. Blfi vckvigrhv rm gszg zivz rh tlrmt gl yv xifxrzo.
```
## Analysis

First you had to know with which algorithm the file was encrypted. So I entered the https://www.dcode.fr/identificador-cifrado page where you can add the encrypted text, parses it, and tells you which algorithms are likely to have been encrypted.

## Solution
Having found the encryption algorithm (Atbash Cipher) I entered the https://www.dcode.fr/atbash-cipher page where I added the text and deciphered it.

![image](https://github.com/user-attachments/assets/9fbcb8c1-6e6d-48c0-b94b-ebde4577cc78)

```
ofxrzuvi 1:19 KN

Hey lilith, big news! We're going after De Monne Financial next. Their security measures have some holes that we're gonna exploit big time! I've been poking around and found some SQL vulnerabilities we can leverage for maximum chaos.

Remember that IT guy I mentioned before? Turns out, he’s a real chatty Cathy on social media. Think we can use some good ol' social engineering to our advantage. We’ll get him spewing passwords like a leaky faucet. Plus, with your OSINT skills and my SQL magic, they won’t see what hit 'em.

ororgs 1:20 KN

I've already started some OSINT recon and guess what? Found some juicy deets about their IT team on LinkedIn. People overshare so much, it's practically a goldmine. Social engineering that chatty dude should be a breeze; I'll craft a legend that'll have him spilling everything.

ofxrzuvi 1:21 KN

Yaaas, this is gonna be legendary! Loving the enthusiasm. And Elroy Ongaro? That guy's practically rolling out the red carpet for us with how much he shares online. Can't believe how easy some of these targets make it.

ororgs 1:22 KN

Absolutely! Elroy Ongaro has no idea what's coming his way. I've already got a few angles in mind to get him talking. Social engineering these types is always a thrill.

I'll start drafting some personas and scripts. Once he's under our influence, we can orchestrate the SQL exploit seamlessly. Your expertise in that area is going to be crucial.
```
## How to avoid
Use more complex and secure encryption algorithms instead. Some recommended ciphers are:
- AES (Advanced Encryption Standard): A widely used and robust symmetric encryption.
- RSA (Rivest – Shamir – Adleman): Asymmetric encryption that is used in many modern security applications.
## Full solution
Reading the deciphered text in the penultimate line we find the name `Elroy Ongaro`.

`flag{Elroy_Ongaro}`
