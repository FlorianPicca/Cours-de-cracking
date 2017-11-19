# Techniques d'offuscation

On a vu dans le premier chapitre qu'il est possible de retrouver les mots de passe cachés dans un programme avec la commande `strings` et parfois même avec `ltrace`. Nous allons voir comment il est possible de les dissimuler pour éviter qu'ils soient trouvé aussi simplement.

Certaines des techniques que nous allons voir ont déjà été utilisées dans les exercices des chapitres précédants.

## Ne pas déclarer son mot de passe

Le moyen le plus simple pour qu'une chaine de caractère n'aparaisse pas lors d'un `strings` c'est de ne pas avoir de chaine de caractère ! A la place vous pouvez comparer chaque lettre du mot entré avec celles du mot de passe attendu.

Au lieu de faire quelque chose comme ça:

```C
if (strcmp(pass, "s3cr3t") == 0) {
// Le mot de passe est déclaré en tant que chaine de caractère.
// Il sera donc placé dans la section .data et visible avec un strings
	printf("Good pass!\n");
}
```

Faites plutôt ça:

```C
if (pass[0] == 's' && pass[1] == '3' && pass[2] == 'c' && pass[3] == 'r' pass[4] == '3' && pass[5] == 't') {
// Pas de déclaration de chaine de caractère, rien dans la section .data
// Les caractères seront remplacés au moment de la compilation par leur code ASCII directement dans la section .text
	printf("Good pass!\n");
}
```

## Ne pas stocker son mot de passe en clair

La technique précédente est certes simple mais pas pratique du tout. Une meilleure alternative serait de garder une version chiffré du mot de passe. Bien souvent on rencontre un chiffrement très simple à base de XOR.

Dans le cas d'un chiffrement nécessitant une clé, il faudra aussi s'assuré que la clé ne soit pas trop simple à trouver.

### Exercices

[Ex3](../Exercices/Ex3), [Ex6](../Exercices/Ex6), [Ex7](../Exercices/Ex7), [Ex8](../Exercices/Ex8) 
