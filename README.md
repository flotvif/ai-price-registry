# Bertrande, le registre date des parametres des services numeriques

Ce depot contient l'etat publie du registre, commite une fois par jour.

**L'historique git de ce depot est l'historique des prix.** Les fichiers sont
tries par identifiant de parametre, jamais par nom : le diff d'un commit est
donc lisible ligne a ligne.

    git log --oneline values.csv
    git log -p -S "openai:gpt-5-mini#price_output" values.csv

## Trois fichiers, et la difference compte

| Fichier | Contenu | Ce que dit son diff |
|---|---|---|
| `values.csv` | 13210 valeurs, sans provenance | **exactement les valeurs qui ont change** |
| `registry.csv` | les memes, avec date, source, niveau et empreinte SHA-256 | ce qui a ete constate a nouveau, y compris a valeur identique |
| `measurements.csv` | 2499 mesures continues : disponibilite, latence, debit, taux de change | de nouvelles lectures, tous les jours |
| `changes.csv` | 752 changements confirmes, du plus recent au plus ancien | |

`values.csv` et `registry.csv` portent les memes lignes. Le premier ne contient
que l'etat, donc une relecture au meme prix n'y touche rien ; le second porte la
preuve, donc il bouge des qu'un constat est reecrit. Les deux affirmations sont
utiles et ne sont pas la meme.

**Une mesure n'est pas un parametre publie**, et c'est pourquoi elles vivent
dans un fichier separe. Un parametre publie est ce que le vendeur affirme : un
prix, une fenetre de contexte, un plafond de debit. Une mesure est ce que
quelqu'un a constate a un instant : une disponibilite, une latence. Le premier
change quand le vendeur decide ; la seconde change tout le temps, et son
changement n'apprend rien. Melangees, les secondes noient les premieres : au
deuxieme commit de ce depot, 1 308 lignes modifiees sur 2 107 etaient de la
mesure continue.

## La provenance

Chaque ligne de `registry.csv` porte l'adresse de son **recu** : une page
permanente qui montre la valeur, sa date, sa source et son empreinte.
L'identifiant du recu se recalcule depuis ce fichier, par n'importe qui :

    sha256(f"{parameter_id}|{observed_at}|{content_hash}")[:12]

## Les trois regles du registre

1. **Aucun fait ne sort d'un modele de langage.** Chaque valeur vient d'une
   source lisible, avec son adresse, sa date et son empreinte.
2. **Toute donnee publiee est tracable.** D'ou elle vient, de quand elle date,
   ce qui a change depuis.
3. **Aucune identite humaine fabriquee.** Bertrande est un systeme editorial
   automatise, et une personne nommee en repond.

Une case vide est honorable ; une case inventee detruit le projet. Un parametre
dont la source est trop faible n'est pas publie, il est retenu et compte.

## Niveaux de source

| Niveau | Ce que c'est |
|---|---|
| 1 | l'API du vendeur |
| 2 | la documentation technique du vendeur |
| 3 | la page de tarifs du vendeur |
| 4 | un mesureur tiers identifie |
| 5 | un agregateur, **jamais publie seul** |

## Licence et usage

CC-BY-4.0. Reutilisation libre avec attribution : `Bertrande, https://bertrande.com`.

L'API de lecture repond a la meme question par la date :
`https://bertrande.com/v1/services/{id}/{parametre}?on=2026-09-03`. Interrogee sur une date
anterieure a notre premiere lecture, elle rend un **416** et dit depuis quand
nous observons, plutot qu'un 404 qui suggererait une disparition ou un `null`
qui suggererait la gratuite.

Collecte commencee le 2026-08-25. Site : https://bertrande.com
