# Baby Shark

Nombre de point : 50

Contexte : During my holiday in Bahamas, I met a baby shark. The shark wanted to sing me something but couldn't. Can you sing that for me?

Flag Format: KCTF{S0m3_T3xt_H3re}

Author: fazledyn

# Résolution

Dans un premier temps, on vérifie le type de fichier.
```bash
$ file babyshark.jar 
babyshark.jar: Java archive data (JAR)
```
Le fichier est bien de type .jar. On va donc essayer d'extraire les données contenues par le biais de la commande `jar xfv babyshark.jar`.
```bash
$ jar xfv babyshark.jar
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true
 inflated: META-INF/MANIFEST.MF
  created: META-INF/
  created: kctf/
  created: kctf/flag/
 inflated: kctf/flag/Flag.class
  created: kctf/constants/
 inflated: kctf/constants/Strings.class
 inflated: kctf/constants/Integers.class
 inflated: kctf/Sound.class
 inflated: kctf/Main.class
 inflated: audio.wav
```
Une fois ceci réalisé, on voit qu'on peut difficilement lire les .class. On va donc avoir besoin d'un décomplicateur pour pouvoir lire correctement les fichiers .class. Pour cela, j'utilise l'application [JD-GUI](http://java-decompiler.github.io/).
Pour lancer l'application, j'utilise la commande `java -jar jd-gui-1.6.6-min.jar `, puis j'ouvre le fichier babyshark.jar avec.

On voit très vite que le fichier `Flag.class` qui paraît prometteur, n'est pas intéressant :
```java
package kctf.flag;

public class Flag {
  private static final int count = 0;
  
  private static final String flag = "KCTF{this_is_not_the_flag}";
  
  private static final String comment = "You thought this was the flag? LOL";
}
```

Cependant on remarque une chaîne nommée `String_0xflag=S0NURns3SDE1X1dANV8zNDVZX1IxNkg3P30=` dans la class string. On se doute que c'est un base64, et après avoir décoder on récupère notre flag :
```bash
$ echo "S0NURns3SDE1X1dANV8zNDVZX1IxNkg3P30=" | base64 -d 
KCTF{7H15_W@5_345Y_R16H7?}  
```