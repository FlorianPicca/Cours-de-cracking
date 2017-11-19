# Les bases du débogage

Avant d'ouvrir le programme avec un débugger, lancez-le pour voir s'il attend des paramètres et avoir un premier aperçu de son fonctionnment normal.

Maintenant qu'on a une meilleure compréhension du programme, on peut l'ouvrir avec son débogueur préféré. Par la suite j'utiliserai un débogueur graphique pour Linux qui s'appelle EDB. Si vous souhaitez l'installer, suivez [le guide d'installation](..Documentation/install%20EDB.md). Il est également possible d'utiliser GDB qui est très puissant mais entièrement en ligne de commande, ce qui est moins attirant pour les débutants.

## Déboguer avec EDB

Débogage d'un programme sans passage d'arguments:

`edb --run <prog>`

Débogage d'un programme nécessitant 2 arguments:

`edb --run <prog> <arg1> <arg2>`

A l'ouverture, on ne se trouve pas encore au début de la fonction _main()_. La première chose à faire est de s'y rendre. Pour cela, exécutez simplement toutes les instructions sans vous soucier de ce qu'elles font jusqu'à l'appel à la fonction ___libc_start_main()_. 

Utiliser les boutons en haut à gauche, il devrai ressembler à ceux-là:

![](../Images/stepButtons.png)

- Le bouton le plus à gauche entre dans les appels de fonction pour pouvoir déboguer plus en profondeur.
- Le deuxième exécute un instruction sans entrer dans les appels de fonction.
- Le troisième permet d'exécuter toutes les instructions jusqu'à sortir de la fonction courante.
- Le dernier exécute l'ensemble du programme.

On utilisera seulement le deuxième et parfois le premier, lorsque l'on souhaite aller voir des fonctions particulières.

Exécutez la fonction ___libc_start_main()_ sans entrer dedans (deuxième bouton) et vous allez vous retrouver au début de la fonction _main()_.

## Rappels d'assembleur:

Maintenant que le programme est chargé dans un débogueur, il est important d'avoir quelques notions d'assembleur (x86).

le registre EIP indique la prochaine instruction à executer
- Un debugger permet de le modifier et donc de changer le flux d'execution du programme à votre guise. Utilisez cette technique pour éviter les pièges que l'on vous tend.

Après un appel à une fonction qui renvoie une valeur (strlen, rand, ...), celle-ci se trouvera dans EAX.


Essayer de se repérer grace au appels de fonctions
- Voir quelles fonctions sont appellée où et quand permet de se faire une idée plus visuelle de se qui se passe et de trouver les bout de codes à éviter/appeller

### Exercices

[Ex2](../Exercices/Ex3)