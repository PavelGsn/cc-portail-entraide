# EXPLICATIONS — CC Git A2 S2

**GUISSANI Pavel** · dépôt : https://github.com/PavelGsn/cc-portail-entraide
Identité Git : `Pavel Guissani <233691912+PavelGsn@users.noreply.github.com>`

## Départ
- Commit initial : `4a266e1`. Commit de départ de main : `76184a8`. `feature/tutorat` (`a43f314`) part aussi de `76184a8`.
- État initial : arbre propre, aucun dépôt distant. Test initial : 2 clics → 400 min au lieu de 40 (durée à 200, introduite par `b88cc98`).

## Branches
| Branche | Rôle | Départ |
|---|---|---|
| feature/accueil | accueil libre le jeudi à 16 h | `76184a8` |
| feature/contact | contact cliquable | `76184a8` |
| feature/tutorat | contribution fournie | `76184a8` |
| hotfix/duree | annulation de la durée erronée | `0328b21` |

## Mes changements
- `3f1df58` (accueil) : paragraphe de permanence → « Accueil libre sans inscription le jeudi à 16 h. »
- `5a271f2` (contact) : lien `mailto:entraide@example.org`.
- Dans main : avance rapide vers `3f1df58`, puis fusion `b174e81` de feature/contact (sans conflit).

## Conflit
- Accueil et tutorat partent tous les deux de `76184a8` et modifient la même ligne `#permanence` → conflit dans `index.html`.
- Décision : deux paragraphes, l'accueil libre à 16 h (`#permanence`) et le tutorat sur inscription à 16 h 30 (`#tutorat`). Le contact cliquable est conservé.
- Fusion `0328b21`, parents `b174e81` (mes deux évolutions) et `a43f314` (tutorat). Aucun marqueur, `git status` propre.

## Annulation
- Commit annulé : `b88cc98` (durée 20 → 200 dans `app.js`).
- `git revert b88cc98` sur hotfix/duree → `6a2d678` (200 → 20, rien d'autre), puis avance rapide de main.
- `b88cc98` reste dans l'historique ; le conseil du README (`76184a8`) est conservé.

## Tests (navigateur : 2 clics puis Recommencer)
| | Attendu | Avant (`76184a8`) | Après (`6a2d678`) |
|---|---|---|---|
| Durée affichée | 20 min | 200 ❌ | 20 ✔ |
| Après 2 clics | 2 · 40 min | 2 · 400 ❌ | 2 · 40 ✔ |
| Recommencer | 0 · 0 | 0 · 0 ✔ | 0 · 0 ✔ |

Après : accueil 16 h, tutorat 16 h 30 et contact cliquable présents.

## Publication
- Branches poussées : main, feature/accueil, feature/contact, feature/tutorat, hotfix/duree.
- Dernier main vérifié identique en local et sur GitHub avant ce commit : `9b5a435`. L'identifiant du commit de ce fichier est dans mon message de remise.

## Difficultés
- Conflit attendu : voir plus haut.
- Premier essai déjà publié : dossier local supprimé, TP refait depuis le zip, `git push --force` des 5 branches, dépôt renommé (ancien nom `cc-portail-entraide-entrainement`). Vérifié avec `git ls-remote origin`.
- Aucune autre difficulté.

## Aides utilisées
- Outil : Claude Code (Claude Opus 5.5).
- Aide reçue : l'assistant a expliqué les commandes et rédigé ce fichier puis sa version courte.
- L'assistant m'a aussi aidé à commiter et pousser ce fichier : version courte (`9b5a435`), puis le commit qui ajoute cette précision. Vérifié ensuite avec `git ls-remote origin`.

