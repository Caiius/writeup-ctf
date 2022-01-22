# The Encoder

Nombre de point : 50

Contexte : A friend of mine sent me the following numbers and the binary and told me that he used that binary to encrypt something interesting for me. 1412 1404 1421 1407 1460 1452 1386 1414 1449 1445 1388 1432 1388 1415 1436 1385 1405 1388 1451 1432 1386 1388 1388 1392 1462

Can you find out what all these numbers mean?

Flag Format: KCTF{answer_here}

Author: NomanProdhan

# Résolution

Après avoir décompressé le fichier zip. On récupère un fichier ELF.
```bash
file the_encoder.out 
the_encoder.out: ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=23dbf7389437f227d2b0ea83251c2f719085f2a2, for GNU/Linux 4.4.0, not stripped
```
Le programme semble encoder les charactères qu'on lui donne en entrée :
```bash
./the_encoder.out 
Welcome to the encoder
Please give me a plain text of max 40 characters
abcd
1434 1435 1436 1437  
```
Après avoir tester plusieurs fois le programme on remaque que le comportement ne change pas.
On doit alors juste trouver la clé pour décoder le message du challenge donner dans le contexte :
`1412 1404 1421 1407 1460 1452 1386 1414 1449 1445 1388 1432 1388 1415 1436 1385 1405 1388 1451 1432 1386 1388 1388 1392 1462`.

On sait que ord('a') = 97, donc on a ajouté 1337 pour obtenir 1434. Notre clé est donc `1337`.
Plus qu'à faire une petit python :
```python
a = [1412,1404,1421,1407,1460,1452,1386,1414,1449,1445,1388,1432,1388,1415,1436,1385,1405,1388,1451,1432,1386,1388,1388,1392,1462]
for c in a:
    print(chr(c-1337),end="")
# Sortie
KCTF{s1Mpl3_3Nc0D3r_1337}
```