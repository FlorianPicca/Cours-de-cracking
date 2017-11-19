# Enoncé
Déterminer l'architecture pour laquelle le programme a été compilé.

Les symboles de débogage sont-ils encore présents ?

A-t-il été compilé avec le flag `--static` de gcc ?

Retrouver le nom du fichier source grâce à la commande `strings`.

Quelle est la version de gcc utilisé ? Sur quelle machine ?

Trouver la chaine qui s'affichera à l'écran lors de l'exécution.

Quelles sont les fonctions de la libc utilisées ?

Lancer `strace` et retrouver la ligne résponsable de l'affichage du message.

Lancer `ltrace` et retrouver les fonctions de la libc trouvées précédemment.
