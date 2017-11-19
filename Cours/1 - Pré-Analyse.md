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
.symtab								<- Les sections du binaire commencent avec un point
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
getString							<- Fonction déclarée par l'utilisateur
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

## La commande objdump

`objdump -d <filename>`

Bien que personnelement je ne l'ai jamais utilisée pour résoudre un challenge de craking, il est bon de savoir qu'elle existe et ce qu'elle fait. `objdump` permet, entre autres, de désassembler un binaire. Cela permet de voir le code assembleur de chaque fonctions ainsi que le contenu des autres sections. Cela ne nous intéresse pas vraiment puisqu'un débogueur permet de faire la même chose. Pour plus d'informations vous pouvez consulter le manuel d'utilisation.

## La commande strace

`strace ./<exécutable> [args]`

`strace` est un petit débogueur qui va afficher tous les appels système que fait le programme lors de son exécution (write, open, read, ...). La sortie est plutôt verbeuse car il y a une première phase d'initialisation avant d'arriver dans le coeur du programme:

```
$ strace ./challJF 
execve("./challJF", ["./challJF"], [/* 73 vars */]) = 0
strace: [ Process PID=2349 runs in 32 bit mode. ]
brk(NULL)                               = 0x87a3000
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
mmap2(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xf778d000
access("/etc/ld.so.preload", R_OK)      = -1 ENOENT (No such file or directory)
open("/etc/ld.so.cache", O_RDONLY|O_CLOEXEC) = 3
fstat64(3, {st_mode=S_IFREG|0644, st_size=105077, ...}) = 0
mmap2(NULL, 105077, PROT_READ, MAP_PRIVATE, 3, 0) = 0xf774d000
close(3)                                = 0
access("/etc/ld.so.nohwcap", F_OK)      = -1 ENOENT (No such file or directory)
open("/lib32/libc.so.6", O_RDONLY|O_CLOEXEC) = 3
read(3, "\177ELF\1\1\1\3\0\0\0\0\0\0\0\0\3\0\3\0\1\0\0\0\300\207\1\0004\0\0\0"..., 512) = 512
fstat64(3, {st_mode=S_IFREG|0755, st_size=1775464, ...}) = 0
mmap2(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xf774c000
mmap2(NULL, 1784348, PROT_READ|PROT_EXEC, MAP_PRIVATE|MAP_DENYWRITE, 3, 0) = 0xf7598000
mprotect(0xf7745000, 4096, PROT_NONE)   = 0
mmap2(0xf7746000, 12288, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_DENYWRITE, 3, 0x1ad000) = 0xf7746000
mmap2(0xf7749000, 10780, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_FIXED|MAP_ANONYMOUS, -1, 0) = 0xf7749000
close(3)                                = 0
mmap2(NULL, 4096, PROT_READ|PROT_WRITE, MAP_PRIVATE|MAP_ANONYMOUS, -1, 0) = 0xf7597000
set_thread_area({entry_number:-1, base_addr:0xf7597700, limit:1048575, seg_32bit:1, contents:0, read_exec_only:0, limit_in_pages:1, seg_not_present:0, useable:1}) = 0 (entry_number:12)
mprotect(0xf7746000, 8192, PROT_READ)   = 0
mprotect(0x804a000, 4096, PROT_READ)    = 0
mprotect(0xf778e000, 4096, PROT_READ)   = 0
munmap(0xf774d000, 105077)              = 0
clock_gettime(CLOCK_PROCESS_CPUTIME_ID, {8076355287646208, -614467463509926468}) = 0
ptrace(PTRACE_TRACEME, 0, 0x1, NULL)    = -1 EPERM (Operation not permitted)
fstat64(1, {st_mode=S_IFCHR|0620, st_rdev=makedev(136, 7), ...}) = 0
brk(NULL)                               = 0x87a3000
brk(0x87c4000)                          = 0x87c4000
write(1, "[FAIL #734] Debugger spotted\n", 29[FAIL #734] Debugger spotted			<- Le programme commence ici
) = 29
write(1, "\n", 1
)                       = 1
exit_group(0)                           = ?
+++ exited with 0 +++
```

Dans l'exemple, on voit que tout ce que le programme a fait, c'est afficher un message à l'écran (write) indiquant qu'il a détecté un débogueur. Tout ce qui se trouve avant ne nous intéresse pas car ça fait partie de l'initialisation du programme.


## La commande ltrace

`ltrace ./<exécutable> [args]`

`ltrace` fonctionne de la même manière que `strace` sauf que cette commande va afficher tous les appels aux fonctions (strcmp, printf, ...) ainsi que leur arguments ! C'est la commande que l'on va le plus souvent utiliser car elle donne d'importantes informations sur ce qui se passe à l'intérieur d'un programme avant même de devoir l'ouvrir à la main avec un débogueur. Si un programme fait une comparaison entre un mot de passe et l'entrée utilisateur en utilisant la fonction strcmp(), on verra facilement le mot de passe attendu.

La sortie de `ltrace` pour le même programme que précédemment:

```
$ ltrace ./challJF 
__libc_start_main(0x8048a95, 1, 0xff9f3674, 0x8048b40 <unfinished ...>
clock(0x8000, 0xf7754000, 0xf7752244, 0x804841d)                                  = 604
ptrace(0, 0, 1, 0)                                                                = 0xffffffff
puts("[FAIL #734] Debugger spotted\n"[FAIL #734] Debugger spotted

)                                            = 30
exit(0 <no return ...>
+++ exited (status 0) +++
```

C'est beaucoup plus concis et lisible. Dans ce cas on apprend qu'il y a un appel à ptrace(), c'est ce qui lui a permit de détecter le débogueur mais on verra cela plus en détail dans les prochains cours.

### Exercices
[Ex1](../Exercices/Ex1), [CrackMe1](../Exercices/CrackMe1)
