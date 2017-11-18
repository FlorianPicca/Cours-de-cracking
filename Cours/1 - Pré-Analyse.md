# Pré-Analyse du binaire:
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
![Ex1](../Exercices/Ex1), ![CrackMe1](../Exercices/CrackMe1)
