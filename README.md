# permisdeconduireqc
Génération/validation partielle de numéros de permis de conduire du Québec

## Comment l'utiliser

Ce fichier est une implémentation simple en single-page javascript de l'algorithme qui permet de générer la première partie du numéro de permis de conduire au Québec. L'algorithme n'est pas officiel mais s'est anecdotiquement révélé exact. Vous n'avez qu'à enregistrer localement (ou hoster) cette page. Elle ne fait aucun appel serveur, ne charge aucune librairie distante, et tout est exécuté en local, puisqu'on parle ici d'informations sensibles ici (nom, date de naissance). 

### Pré-requis. 

Un browser. 

## L'algorithme lui-même

Ce algo d'encodage n'est pas à ma connaissance disponible de façon publique, et a été trouvé sur un forum. (voir section remerciements ci-dessous). 

Le numéro est constitué de 5 "champs" 
1. (une lettre). La première lettre du nom de famille. 
1. (trois chiffres). Trois chiffres codifiés selon la correspondance ci-dessous pour les lettres suivantes du nom de famille. 
    - Les lettres AEIOUYHW ne sont pas codifiées et sont retirées du nom (VEILLEUX devient VLLX)
    - Les lettres doublées sont retirées (VLLX devient VLX)
    - S'il n'y a plus de consonnes à codifier, les positions restantes sont comblées avec des 0 (par exemple, KIM deviendra KM -> 250)
    ```
    1- B, F, P, V
    2- C, G, K, J, Q, S, X, Z
    3- D, T
    4- L
    5- M, N
    6- R
    ```
1. (un chiffre). Un chiffre correspondant à la première lettre du prénom selon la correspondance ci-dessous.
      ```
      Prénom
      1- A, B
      2- C
      3- D, E, F
      4- G, H, I
      5- J, K, L
      6- M, N, O
      7- P, Q, R
      8- S, T
      9- U, V, W, X, Y, Z
      ```
1. (six chiffres). La date de naissance, en format JJMMAA
1. (deux chiffres). Ces chiffres ne sont pas codifiés, et pourraient être séquentiels. Leur amplitude n'est pas connue, mais semble basse. 

Le numéro de permis de conduire de JEAN TREMBLAY, né le 1980-12-25, serait donc le T6515-251280-XX (en correspondance: TRMBJ).

## Conséquences et implications

Le numéro de permis de conduire est souvent utilisé comme information permettant d'authentifier un utilisateur. Cette pratique est dangereuse puisque la première partie du numéro se décèle à partir du nom/prénom seulement, la seconde à partir de la date de naissance (une information souvent facile à obtenir), et que seuls les deux dernier chiffres du numéro de permis, qui semblent à priori non-codifiés et potentiellement séquentiels sur des critères, font office de "secret", et anecdotiquement, seraient sur une quantité de valeurs relativement basses et limitées (et non, par exemple, aléatoire entre 00 et 99). 

## Scénarios d'attaques possibles faisant usage de cette information

Le fait que ce numéro semble semi-aléatoire et sans "pattern" reconnaissable pourrait servir dans des scénarios d'ingéniérie sociale afin d'instaurer un climat de confiance avec la cible. 

> Bonjour M. X, je suis Y de la banque Z, et je vous appelle pour [insérer transaction éventuellement frauduleuse]. Comme preuve que je suis légitime, voici 5 premiers chiffres/lettres de celui-ci. Maintenant, pour prouver votre identité, veuillez me donner les deux derniers numéros. 

Il est aussi évidemment possible que certains services à la clientèle utilisent ces combinaisons pour valider l'identité de quelqu'un, ou que des mécanismes de réinitialisation de mots de passes utilisent cette information comme validation pour une réinitialisation.

## Limitations

L'algorithme lui-même, même s'il est anecdotiquement fonctionnel, n'est pas exhaustif. Je n'ai pas validé ce que donneraient des cas limite, par exemple:
* quelqu'un nommé "Aaron Aa" ou autre noms qui n'ont pas de consonnes (présumément que les trois chiffres seraient 0 pour un numéro commençant par A0001). 
* la 101e personne née la même journée sur une combinaison très commune de nom ("J Tremblay"), où les chiffres séquentiels des dernières positions dépasseraient les combinaisons possibles. Sachant qu'il naît en moyenne au "pic" de l'année environ 250 enfants au Québec, il s'agit tout de même d'une situation improbable.

## Contribution

Ce repository ne me semble pas propice aux contributions, mais si vous le croyez pertinent n'hésitez pas à me contacter. 

## Auteur

* **Alexandre Major** - *Publication Initiale* - [LexMajor](https://github.com/LexMajor)

## License

Ce projet n'est pas sous licence particulière. 

## Remerciements

Un merci tout particulier à l'utilisateur "michaelmc" du Club Civic Québec, qui semble rapidement être un policier amateur de voitures de sport et qui a levé le voile sur l'algorithme de façon très détaillée dans un de ses posts.  https://forum.clubcivicquebec.com/topic/305634-numeros-de-permis-de-conduire/ 
