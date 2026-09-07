# Bertrande, le registre date des parametres des services numeriques

Ce depot contient l'etat publie du registre, commite une fois par jour.

**L'historique git de ce depot est l'historique des prix.** Le fichier est trie
par identifiant de parametre, jamais par nom : le diff d'un commit est donc
exactement la liste des valeurs qui ont change ce jour-la.

    git log --oneline registry.csv
    git log -p -S "openai:gpt-5-mini#price_output" registry.csv

## Ce qu'il y a dedans

| Fichier | Contenu |
|---|---|
| `registry.csv` | 14037 valeurs publiees, chacune avec sa date d'observation, l'adresse de sa source, le niveau de cette source et l'empreinte SHA-256 du contenu lu |
| `changes.csv` | 96 changements confirmes, du plus recent au plus ancien |

Chaque ligne porte l'adresse de son **recu** : une page permanente qui montre la
valeur, sa date, sa source et son empreinte. L'identifiant du recu se recalcule
depuis ce fichier, par n'importe qui :

    sha256(f"{parameter_id}|{observed_at}|{content_hash}")[:12]

## Les trois regles du registre

1. **Aucun fait ne sort d'un modele de langage.** Chaque valeur vient d'une
   source lisible, avec son adresse, sa date et son empreinte.
2. **Toute donnee publiee est tracable.** D'ou elle vient, de quand elle date,
   ce qui a change depuis.
3. **Aucune identite humaine fabriquee.** Bertrande est un systeme editorial
   automatise, et le dit.

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
