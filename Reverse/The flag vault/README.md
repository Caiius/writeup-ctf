# The Flag Vault

Nombre de point : 25

Contexte : Unlock the vault & grab the flag.

Flag Format: KCTF{pla1n_t3xt_here}

Author: fazledyn

# Résolution 

Après avoir décompressé le fichier zip. On récupère un fichier ELF. On le lance pour voir son fonctionnement et on remarque qu'il nous demande un mot de passe.
```bash

$ file The_Flag_Vault 
The_Flag_Vault: ELF 64-bit LSB pie executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=8db1599916703d131749a488d7c16fd334f6a84d, for GNU/Linux 3.2.0, not stripped

$ ./The_Flag_Vault 

Hi there!

Please enter the password to unlock the flag vault: test

Sorry!

You have entered a wrong password! 

Please try with a valid one!

If you don't have the password, you can buy that here at https://knightsquad.org
                                                                                
```

On va donc devoir décompiler l'application pour trouver le mot de passe qui doit être entré. Pour cela on utilise `gdb`. Je recommande d'installer [gdb-peda](https://github.com/longld/peda) pour plus de faciliter.
```bash
# Lancement de GDB
$ gdb The_Flag_Vault 
# On pose un breakpoint au début le main
gdb-peda$ b main
# Puis on lance l'application
gdb-peda$ r
# On désassemble le main pour voir un peu plus clair et on remarque une chaîne intéressante
0x00005555555552bb <+242>:   call   0x5555555550c0 <strcmp@plt> 
# On pose un b sur l'appelle du strcmp qui doit être le moment ou le mot de passe est vérifié
gdb-peda$ b *0x00005555555552bb
# On avance jusqu'à ce point puis on entre un mot de passe bidon
gdb-peda$ c
Continuing.

Hi there!

Please enter the password to unlock the flag vault: toto
# A ce moment là les arguments de la fonction strcmp sont les suivantes :
arg[0]: 0x7fffffffdb60 ("abracadabrahahaha")
arg[1]: 0x7fffffffdb40 --> 0x6f746f74 ('toto')
arg[2]: 0x7fffffffdb40 --> 0x6f746f74 ('toto')
# On stop le programme pour tester la chaine abracadabrahahaha en tant que mot de passe
gdb-peda$ c
gdb-peda$ r
gdb-peda$ c
Continuing.

Hi there!

Please enter the password to unlock the flag vault: abracadabrahahaha
# Et bingo on obtient le mot de passe
gdb-peda$ c
Continuing.

Congratulations! Here is your flag:

KCTF{welc0me_t0_reverse_3ngineering}

[Inferior 1 (process 15370) exited normally]