challenge: __Crypto Stands For Cryptography__
Uses base64 encryption
`TWV0YUNURntiYXNlNjRfZW5jMGRpbmdfaXNfbjB0X3RoZV9zYW1lX2FzX2VuY3J5cHRpMG4hfQ==`

__Solution__ 
Use cyberchef and use From base64 recipe to decrypt.
output: `MetaCTF{base64_enc0ding_is_n0t_the_same_as_encrypti0n!}`

------------------------------------------------------

challenge: __Welcome to the Obfuscation Games!__

decrypt:
```
$s=New-Object IO.MemoryStream(,[Convert]::FromBase64String("H4sIAEFgjl8A/xXMMQrCQBCF4as8FltPIFaCnV3A8jFmn8ngupuYaUS8e5LyL77
//vHQcWxLIHWj8Cw2wBd4RWyp2resjMm+pVlOJxzmGWekm8Iu3fU3ScXrwIf1L26C+4CtijukBY3hb/3TCj2Ieh9qAAAA"));
IEX (New-Object IO.StreamReader(New-Object IO.Compression.GzipStream($s,[IO.Compression.CompressionMode]::Decompress))).ReadToEnd();
```
Solution:

Step 1: 
We grab the encrypted text and use the from base64 in cyberchef.
After doing this we get:
```
....A`._.ÿ.Ì1
Â@..á«<.[O V..]Àò1f.Éàº..iD¼{.ò/¾ÿþñÐqlK u£ð,6À.xEl©Ú·¬.É¾¥YN'.æ.g¤.Â.Ýõ7IÅëÀ.õ/n.û...;¤..áoýÓ
=.z.j...

```
Step 2: 
We notice that its being compressed into a gzip.
We use the extract files and notice that one file was found.

![file](https://i.imgur.com/viscGsT.png)

Step 3:
We extract using gunzip and we recive the flag

flag:`Write-host "The flag is in the encoded payload"; $qq = "MetaCTF{peeling_back_the_flag_one_code_at_a_time}"`

-------------------------------------------
challenge: __ROT 26__

decrypt: ```g!0{]n`7*+0y~+1|(!y.+0yKM9```

Step 1:
We notice that it uses ROT encryption
We can either use a python script or a simple online tool.

https://www.dcode.fr/rot-cipher


Step 2: 
We enter the encrypted text and run the 
94 PRINTABLE ASCII CHARACTERS FROM ! (33) TO ~ (126) (IE: ROT47) decryption
and the output will be the flag.

`MetaCTF{not_double_rot_13}`


