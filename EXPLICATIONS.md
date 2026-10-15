# EXPLICATIONS — A2 S2 · CC Git · Portail d'entraide

- **Nom / prénom :** GUISSANI Pavel
- **Dépôt public :** https://github.com/PavelGsn/cc-portail-entraide
- **Identité Git (configurée en local avant mes commits) :** `Pavel Guissani <233691912+PavelGsn@users.noreply.github.com>` (adresse de confidentialité GitHub)

## 1. Reprise et diagnostic initial

- Zip extrait entièrement dans un dossier neuf, dossier `cc-portail-entraide` ouvert avec son `.git`.
- `git status` : branche `main`, rien à valider. `git branch -v` : `main` et `feature/tutorat`. `git remote -v` : aucun dépôt distant.
- **Commit initial de main :** `4a266e1` *Créer le portail et le simulateur de créneaux* (aucun parent).
- **Commit de départ de main :** `76184a8` *Ajouter le conseil de vérification du compteur*.
- Histoire fournie : `4a266e1` → `b88cc98` (durée 20 → 200 dans `app.js`) → `76184a8` (conseil du README). `feature/tutorat` = `a43f314`, un commit au-dessus de `76184a8`, qui modifie le paragraphe `#permanence`.

## 2. Branches : rôle et point de départ

| Branche | Rôle | Point de départ |
|---|---|---|
| `main` | version intégrée | `76184a8`, gardé à son départ jusqu'à l'enregistrement des deux propositions |
| `feature/accueil` | ma contribution : accueil libre sans inscription | `76184a8` |
| `feature/contact` | ma contribution : contact cliquable | `76184a8` |
| `feature/tutorat` | contribution fournie par l'équipe (`a43f314`) | `76184a8` |
| `hotfix/duree` | annulation ciblée de la durée erronée | `0328b21` (main après intégration du conflit) |

Création : `git branch feature/accueil main` et `git branch feature/contact main` avant toute intégration. Vérification : `git merge-base feature/accueil feature/contact` → `76184a8`.

## 3. Mes changements

| Commit | Branche | Changement |
|---|---|---|
| **`3f1df58`** *Annoncer l'accueil libre sans inscription le jeudi à 16 h* | feature/accueil | `#permanence` : « Permanence le jeudi à 16 h. » → « Accueil libre sans inscription le jeudi à 16 h. » |
| **`5a271f2`** *Rendre l'adresse de contact cliquable avec un lien mailto* | feature/contact | `entraide@example.org` → `<a href="mailto:entraide@example.org">entraide@example.org</a>` |

Chaque modification a été relue avec `git diff` avant le commit (une seule ligne modifiée à chaque fois).

Intégration dans main :
1. `git merge feature/accueil` → **avance rapide** `76184a8` → `3f1df58` (main n'avait pas bougé, pas de commit de fusion).
2. `git merge feature/contact` → commit de fusion **`b174e81`** (parents `3f1df58` et `5a271f2`), sans conflit car les lignes modifiées sont différentes.

## 4. Conflit et décision de contenu

**Pourquoi les branches ont divergé :** `feature/accueil` (`3f1df58`) et `feature/tutorat` (`a43f314`) partent toutes les deux de `76184a8` et modifient **la même ligne** d'`index.html` (le paragraphe `#permanence`) avec deux textes différents. Git ne peut pas choisir seul : il signale un conflit.

- **État constaté :** `git merge feature/tutorat` → `CONFLICT (content): Merge conflict in index.html` ; `git status` → `both modified: index.html` ; marqueurs `<<<<<<< HEAD` (accueil) / `=======` / `>>>>>>> feature/tutorat` (tutorat).
- **Décision :** les deux informations sont des besoins différents, je garde donc les deux dans deux paragraphes distincts :
  - `<p id="permanence">Accueil libre sans inscription le jeudi à 16 h.</p>`
  - `<p id="tutorat">Tutorat individuel sur inscription le jeudi à 16 h 30.</p>` (nouvel id, car un id doit être unique)
- Le contact cliquable, hors de la zone de conflit, est conservé tel quel.
- **Fin de l'intégration :** recherche des marqueurs (aucun), `git add index.html`, puis `git commit`. `git status` → arbre propre, aucune fusion en cours.
- **Commit de fusion du conflit : `0328b21`**, avec deux parents : **`b174e81`** (main avec mes deux évolutions) et **`a43f314`** (feature/tutorat fournie). Vérifié avec `git log -1 --format=%p 0328b21` et dans `git log --graph --oneline --all`.

## 5. Commit annulé et commit d'annulation

- **Commit annulé : `b88cc98bf3fae11998895be8365305f22b2cc49c`** *Mettre à jour la durée de référence des créneaux* : remplace `const dureeCreneau = 20;` par `200` dans `app.js`.
- **Apport ultérieur conservé : `76184a8`** : section « Conseil de vérification » du README. Ce commit touche un autre fichier, et l'annulation ne le modifie pas.
- L'histoire est considérée comme partagée : pas de `reset`, pas de retour de toute la branche à une ancienne version. Sur `hotfix/duree` (créée depuis main `0328b21` avec `git switch -c hotfix/duree`), j'ai lancé `git revert b88cc98`. Le revert crée un **nouveau commit** qui applique l'inverse du changement.
- **Commit d'annulation : `6a2d678`** *Revert "Mettre à jour la durée de référence des créneaux"* (parent `0328b21`) : `200` → `20` dans `app.js`, et rien d'autre (`git show 6a2d678`).
- **Réunion dans main :** `git switch main` puis `git merge hotfix/duree` → avance rapide `0328b21` → `6a2d678`, puisque main n'avait pas bougé depuis la création du hotfix.
- **Vérifications :** `b88cc98` est toujours dans `git log` (histoire non réécrite) ; le README contient toujours « Conseil de vérification » ; `index.html` contient l'accueil (16 h), le tutorat (16 h 30) et le lien `mailto:`.

## 6. Tests avant / après

Méthode : ouverture d'`index.html` dans le navigateur, puis chargement → 2 clics sur « Ajouter un créneau » → « Recommencer ». Résultats relevés aussi par un petit script Node qui charge `app.js` et simule les mêmes clics.

| Moment | Action | Attendu | Observé |
|---|---|---|---|
| **Avant** (main `76184a8`) | chargement | durée 20 min | **200 minutes** ❌ |
| Avant | 2 clics | 2 créneaux · 40 min | **2 créneau(x) · 400 minute(s)** ❌ |
| Avant | Recommencer | 0 · 0 | 0 créneau(x) · 0 minute(s) ✔ |
| Contrôle au commit initial `4a266e1` | 2 clics | 2 · 40 | 2 créneau(x) · 40 minute(s) ✔ : l'erreur vient bien de `b88cc98` |
| **Après** (main `6a2d678`) | chargement | durée 20 min | 20 minutes ✔ |
| Après | 2 clics | 2 créneaux · 40 min | 2 créneau(x) · 40 minute(s) ✔ |
| Après | Recommencer | 0 · 0 | 0 créneau(x) · 0 minute(s) ✔ |
| Après | page | accueil 16 h + tutorat 16 h 30 + contact cliquable | les trois présents, le lien est un `mailto:` ✔ |

## 7. Publication

- `git remote add origin https://github.com/PavelGsn/cc-portail-entraide.git` (dépôt renommé sur GitHub après la première publication, puis adresse mise à jour avec `git remote set-url origin ...`)
- `git push -u origin main feature/accueil feature/contact feature/tutorat hotfix/duree`
- Vérification via l'API publique GitHub, sans connexion : les branches `main`, `feature/accueil` (`3f1df58`), `feature/contact` (`5a271f2`), `feature/tutorat` (`a43f314`) et `hotfix/duree` (`6a2d678`) sont visibles.
- **Dernier identifiant de main vérifié sur GitHub avant cette version du fichier : `a15c7349bd77617d85f9fd0ec53150e963feb001`** (ajout d'EXPLICATIONS.md), identique à `git rev-parse main` en local.
- Le commit qui enregistre cette version de ce fichier devient le dernier commit de main ; son identifiant est donné dans le message de remise.

## 8. Difficultés

1. **Conflit de fusion (attendu)** : voir section 4. Résultat : fusion `0328b21` terminée avec les deux informations. Vérification : aucun marqueur restant, `git status` propre, deux parents.
2. **Reprise complète du TP** :
   - *État constaté :* un premier essai avait déjà été publié sur ce même dépôt GitHub, et j'ai voulu tout refaire pas à pas pour suivre chaque commande.
   - *Ce que j'ai fait :* suppression du dossier local, nouvelle extraction du zip, toutes les étapes refaites. Le dépôt GitHub existant a été réutilisé : `git push --force` des cinq branches a remplacé le contenu du premier essai.
   - *Résultat :* les branches distantes pointent sur les nouveaux commits (`3f1df58`, `5a271f2`, `a43f314`, `6a2d678`).
   - *Vérification :* `git ls-remote origin` et l'API publique GitHub donnent les mêmes identifiants qu'en local.

Aucune autre difficulté.

## 9. Aides utilisées

- **Outil :** Claude Code (modèle Claude Opus 5.5), utilisé pour le compte rendu de ce TP et pour la validation des commandes.