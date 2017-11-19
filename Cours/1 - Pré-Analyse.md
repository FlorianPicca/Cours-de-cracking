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

L'architecture indique s'il s'agit d'un programme 32 ou 64 bit. Un programme 64 bit ne pourra pas être exécuté sur une machine 32 bit, tandis qu'une machine 64 bit pourra exécuter un programme 32 bit. Si vous n'arrivez pas à exécuter un programme 32 bit, il vous faudra installer un paquet supplémentaire:

`sudo apt-get install libc6-i386`

Le processeur sur un ordinateur est généralement Intel. Les téléphones portables ont un processeur ARM. Il existe d'autres types de processeurs comme par exemple MIPS. Vous ne pourrez exécuter un binaire que si le processeur correspond à celui de votre machine. Si ce n'est pas le cas et que vous avez affaire à un executable ARM, par exemple, il faudra passer par un émulateur, comme qemu.

_Dynamically linked_ est ce que vous rencontrerez le plus souvent, ça veut dire que les appels aux fonctions de la libc (printf, scanf, ...) se feront par référence. Le cas contraire serait _Statically linked_. Dans ce cas, les fonctions de la libc utilisées dans le programme seraient copiées et compilées dans l'éxécutable. Cela ferait qu'il pèse plus lourd mais peut être utile parfois.

Dans tout exécutable il y a des symboles de débogage. C'est ce qui permet à un débogueur d'afficher le nom de vos fonctions/variables ou de retrouver à quelle ligne du code C originel correspond une instruction en language d'assemblage après compilation. Un programme _stripped_ est un programme auquel on a supprimé ces symboles avec la commande `strip`:

`strip <executable>`

## La commande strings

`strings <filename>`

La commande `strings` permet d'afficher toutes les possibles chaines de caractères présentes dans un fichier. Cela permet d'avoir un aperçu de tout ce que le programme peux afficher à l'écran et même plus car cela affichera aussi les chaines de caractères déclarées dans le programme mais pas forcément affichées (mot de passe, clé cryptographique, ...).

Dans le cas d'un binaire possédant encore tout ses symboles de débogage, la commande `strings` va pouvoir nous fournir encore plus d'informations ! En effet, on pourra voir le nom des fonctions déclarées par le programmeur, le nom des fonctions de la libc et même, bien qu'inutile, le nom du fichier source originel.


```
$ strings crackme 
/lib/ld-linux.so.2
__gmon_start__							<- Fonction de la libc
libc.so.6
_IO_stdin_used							<- Fonction de la libc
puts								<- Fonction de la libc
realloc								<- Fonction de la libc
getchar								<- Fonction de la libc
__errno_location						<- Fonction de la libc
malloc								<- Fonction de la libc
stderr								<- Fonction de la libc
fprintf								<- Fonction de la libc
strcmp								<- Fonction de la libc
strerror							<- Fonction de la libc
__libc_start_main						<- Fonction de la libc
GLIBC_2.0
PTRh@
[^_]
%s : "%s"							<- Chaine déclarée par le programmeur
Allocating memory
Reallocating memory
############################################################	<- Chaine déclarée par le programmeur
##        Bienvennue dans ce challenge de cracking        ##	<- Chaine déclarée par le programmeur
############################################################	<- Chaine déclarée par le programmeur
Veuillez entrer le mot de passe : 				<- Chaine déclarée par le programmeur
Bien joue, vous pouvez valider l'epreuve avec le pass : %s!	<- Chaine déclarée par le programmeur
Dommage, essaye encore une fois.				<- Chaine déclarée par le programmeur
GCC: (GNU) 4.1.2 (Gentoo 4.1.2 p1.0.2)
GCC: (GNU) 4.1.2 (Gentoo 4.1.2 p1.0.2)				<- Information sur le compilateur utilisé
GCC: (Gentoo 4.3.4 p1.0, pie-10.1.5) 4.3.4
GCC: (Gentoo 4.3.4 p1.0, pie-10.1.5) 4.3.4
GCC: (GNU) 4.1.2 (Gentoo 4.1.2 p1.0.2)
GCC: (Gentoo 4.3.4 p1.0, pie-10.1.5) 4.3.4
GCC: (GNU) 4.1.2 (Gentoo 4.1.2 p1.0.2)
.symtab								<- Les sections du binaire commences avec un point
.strtab
.shstrtab
.interp
.note.ABI-tag
.gnu.hash
.dynsym
.dynstr
.gnu.version
.gnu.version_r
.rel.dyn
.rel.plt
.init
.text
.fini
.rodata
.eh_frame
.ctors
.dtors
.jcr
.dynamic
.got
.got.plt
.data
.bss
.comment
crackme.c							<- Le nom du fichier source
_GLOBAL_OFFSET_TABLE_
__init_array_end
__init_array_start
_DYNAMIC
data_start
__errno_location@@GLIBC_2.0
strerror@@GLIBC_2.0						<- On retrouve ici les fonctions de la libc utilisées (devant les @@GLIBC_2.0)
__libc_csu_fini
_start
getchar@@GLIBC_2.0
__gmon_start__
_Jv_RegisterClasses
_fp_hw
realloc@@GLIBC_2.0
_fini
__libc_start_main@@GLIBC_2.0
_IO_stdin_used
__data_start
getString
stderr@@GLIBC_2.0
__dso_handle
__DTOR_END__
__libc_csu_init
printf@@GLIBC_2.0
fprintf@@GLIBC_2.0
__bss_start
malloc@@GLIBC_2.0
_end
puts@@GLIBC_2.0
_edata
strcmp@@GLIBC_2.0
__i686.get_pc_thunk.bx
main								<- Fonction déclarée par l'utilisateur
_init
printError							<- Fonction déclarée par l'utilisateur
```

## TODO

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
