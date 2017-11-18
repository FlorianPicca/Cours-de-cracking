# Le cracking pour les NULL
## Pré-Analyse du binaire:
La commande file
- Type/Archi
- dynamic/static link
- strip

La commande strings
- donne beaucoup d'infos:
  - fonctions de la libc
  - fonctions de l'utilisateur
  - chaines affichées
  - nom du fichier source (.c)
  
La commande objdump
- désassemble toutes les sections
- pleins d'options

La commande strace
- Affiche les appels aux syscall

La commande ltrace
- Affiche les appels aux fonctions de la libc

### Exercices
![Ex1](Exercices/Ex1), ![CrackMe1](Exercices/CrackMe1)

## Débugger le binaire:
Utiliser EDB (IHM) ou GDB (CLI).

Executer le programme sans débugger pour voir le comportement.

débug du programme sans passage d'arguments:

`edb --run <prog>`

débug du programme avec 2 arguments:

`edb --run <prog> <arg1> <arg2>`

Entrer dans la fonction main
- Le debugger commence un peu avant, il faut executer jusqu'à la fonction __libc_start_main

Essayer de se repérer grace au appels de fonctions
- Voir quelles fonctions sont appellée où et quand permet de se faire une idée plus visuelle de se qui se passe et de trouver les bout de codes à éviter/appeller
### Exercices
![Ex2](Exercices/Ex2)
## Rappels d'assembleur:
le registre EIP indique la prochaine instruction à executer
- Un debugger permet de le modifier et donc de changer le flux d'execution du programme à votre guise. Utilisez cette technique pour éviter les pièges que l'on vous tend.

Après un appel à une fonction qui renvoie une valeur (strlen, rand, ...), celle-ci se trouvera dans EAX.
