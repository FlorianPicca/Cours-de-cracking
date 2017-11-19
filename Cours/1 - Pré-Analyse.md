# Pré-Analyse du binaire
Lorsque l'on a affaire à un binaire, il est bien de commencer par l'analyser pour voir quelles informations on peut en tirer.
## La commande `file`

`file <filename>`

La commande `file` permet de déterminer le type du fichier. Dans notre cas ce sera un exécutable de type ELF. Mais ce n'est pas la seule information utile que nous pouvons en tirer. Voici un exemple de ce que donne cette commande sur un binaire:

```
$ file example 
example: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 2.6.32, BuildID[sha1]=3ba4e2952af627b035a7e69ad4d1b379c8c85051, not stripped
```

Les informations importantes sont:
- Le type du binaire: **ELF**
- L'architecture: **32-bit**
- Le processeur: **Intel 80386**
- Le mode de lien pour les dépendances: **dynamically linked**
- La disponibilité des symboles de débogage: **not stripped**

Le type du binaire indique s'il s'agit d'un exécutable pour Linux (**E**xecutable and **L**inkable **F**ormat), pour Windows (**P**ortable **E**xecutable) ou encore Mac OS (Mach-O).

L'architecture indique s'il s'agit d'un programme 32 ou 64 bit. Un programme 64 bit ne pourra pas être exécuté sur une machine 32 bit tandis qu'une machine 64 bit pourra exécuter un programme 32 bit. Si vous n'arrivez pas à exécuter un programme 32 bit il vous faudra installer un paquet suplémentaire:

`sudo apt-get install libc6-i386`

Le processeur pour un exécutable sur PC est généralement Intel. Les téléphones portables ont un processeur ARM. Il existe d'autres types de processeurs comme par exemple MIPS. Vous ne pourrez exécuter un binaire que si le processeur correspond à celui de votre machine. Si ce n'est pas le cas et que vous avez affaire à un executable ARM, par exemple, il faudra passer par un émulateur type qemu.

_Dynamically linked_ est ce que vous rencontrerez le plus souvent, ça veut dire que les appels aux fonctions de la libc (printf, scanf, ...) se feront par référence. Le cas contraire serait _Statically linked_. Dans ce cas, les fonctions de la libc utilisées dans le programme seraient copié et compilées dans l'éxécutable. Cela ferait qu'il pèse plus lourd mais peut être utile parfois.

Dans tout exécutable il y a des symboles de débogage. C'est ce qui permet à un débugger d'afficher le nom de vos fonctions/variables ou de retrouver à quelle ligne du code C originel correspond une instruction assembleur après compilation. Un programme _stripped_ est un programme auquel on a supprimé ces symboles avec la commande `strip`:

`strip <executable>`



## TODO
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
[Ex1](../Exercices/Ex1), [CrackMe1](../Exercices/CrackMe1)
