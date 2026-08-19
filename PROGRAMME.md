# Programme d'entraînement Git & Gitflow

**Profil** : bases git connues mais rouillées · rythme intensif (quasi quotidien) · suivi via ce document, mis à jour au fil des sessions.

**Environnement** : dépôt local sous Linux + dépôt distant sur GitHub. Chaque jour = un défi concret avec des critères de réussite vérifiables (sortie de commandes, `git log --graph`, lien de la PR, etc.).

**Comment ça marche** : tu fais le défi du jour en local, Claude Code corrige, commente ce qui aurait pu être fait autrement, et coche l'étape ici. Rien n'est chronométré à la seconde près — l'idée est d'enchaîner les jours sans trop de blancs pour que ça reste engageant.

---

## Phase 0 — Mise en place (Jour 1)

Objectif : un terrain de jeu propre et un vrai dépôt GitHub relié.

- [x] Vérifier/installer `git`, configurer `user.name`, `user.email`, éditeur par défaut.
- [x] Configurer une clé SSH et l'ajouter à ton compte GitHub (pas de HTTPS + mot de passe).
- [x] Créer un dépôt GitHub vide `gitflow-dojo` (ou autre nom), le cloner en local.
- [x] Configurer `git config --global init.defaultBranch main`.
- [x] Créer un `.gitignore` de base et un premier commit "Initial commit".

**Défi du jour** : m'envoyer la sortie de `git remote -v`, `git config --list --local` (sans infos sensibles) et le lien du repo GitHub.

---

## Phase 1 — Rafraîchir les fondamentaux Git (Jours 2 à 4)

### Jour 2 — Le cycle de base
- `git status`, `git add` (fichier par fichier vs `-p` en mode interactif), `git commit`, `git log` (`--oneline`, `--graph`, `--stat`).
- Défi : faire 5 commits atomiques (un changement logique = un commit) sur un petit projet fictif (ex. un README qui évolue + 2-3 fichiers texte).
- Livrable : `git log --oneline --graph --all`.

### Jour 3 — Branches et fusion
- Créer/supprimer des branches, `git switch` vs `git checkout`, `git merge` (fast-forward vs merge commit), résoudre un conflit volontaire.
- Défi : créer une branche `test-conflit`, modifier la même ligne que sur `main`, fusionner, résoudre le conflit à la main.
- Livrable : capture ou copie du conflit résolu + `git log --graph`.

### Jour 4 — Historique et rattrapage
- `git diff`, `git stash`, `git rebase -i` (squash, reorder, reword), `git reflog`, `git revert` vs `git reset` (soft/mixed/hard).
- Défi : faire 3 commits "brouillon" (messages type "wip", "fix typo") puis les nettoyer en un seul commit propre via rebase interactif, sans perdre de contenu.
- Livrable : `git log` avant/après.

---

## Phase 2 — Comprendre Gitflow (Jour 5)

Objectif : théorie avant pratique, pour ne pas exécuter des commandes sans comprendre le modèle.

- Lire/comprendre le modèle original de Vincent Driessen : branches `main`, `develop`, `feature/*`, `release/*`, `hotfix/*` (et `support/*` en option).
- Comprendre pourquoi `develop` existe (séparer "stable en prod" de "prochaine version en cours"), le rôle des tags de version (SemVer : `v1.2.0`), et où gitflow diffère du GitHub Flow / trunk-based (à connaître aussi, pour savoir choisir).
- Installer l'extension `git-flow` (AVH edition) — utile pour aller vite, mais on fera aussi tout à la main au moins une fois pour vraiment comprendre ce qu'elle automatise.

**Défi du jour** : m'expliquer avec tes mots (2-3 phrases) à quoi sert chacune des 5 branches, et dans quel cas tu choisirais gitflow plutôt que trunk-based dans un vrai projet.

---

## Phase 3 — Gitflow en pratique, à la main (Jours 6 à 9)

On simule le cycle de vie complet d'un projet, sans l'outil `git-flow`, pour maîtriser les commandes sous-jacentes.

### Jour 6 — Initialisation
- Créer `develop` à partir de `main`, la pousser sur GitHub, la définir comme branche par défaut pour les PR (branche de travail courante).
- Défi : `main` et `develop` existent en local et sur GitHub, avec un commit "chore: init develop branch".

### Jour 7 — Feature branches
- Créer `feature/login`, `feature/navbar` (deux features en parallèle), commits dessus, fusion dans `develop` en `--no-ff` (pour garder la trace de la feature dans l'historique).
- Ouvrir une vraie Pull Request GitHub pour au moins une des deux features (même en solo), et la merger depuis l'interface GitHub.
- Défi : 2 features mergées dans `develop`, avec un historique lisible en `--graph`.

### Jour 8 — Release branch
- Créer `release/1.0.0` depuis `develop`, y faire uniquement des corrections mineures (pas de nouvelles features), merger dans `main` **et** dans `develop`, taguer `v1.0.0` sur `main`.
- Défi : `main` contient le tag `v1.0.0`, `develop` a bien récupéré les corrections de la release.

### Jour 9 — Hotfix branch
- Simuler un bug critique découvert en prod (sur `main`, après la release) : créer `hotfix/1.0.1` depuis `main`, corriger, merger dans `main` **et** `develop`, taguer `v1.0.1`.
- Défi : montrer que le hotfix est bien présent des deux côtés, avec les tags `v1.0.0` et `v1.0.1` visibles dans `git log --graph --all --decorate`.

---

## Phase 4 — Scénarios réalistes et outillage (Jours 10 à 12)

### Jour 10 — Conflits croisés
- Deux features qui touchent le même fichier, développées en parallèle, avec conflit à résoudre au moment du merge dans `develop`.
- Bonus : tester `git rebase develop` sur une feature avant de merger, pour comparer avec un merge direct.

### Jour 11 — GitHub avancé
- Branches protégées sur `main` (pas de push direct, PR obligatoire, au moins une review "requise").
- Écrire une PR description correcte (contexte, changements, comment tester), utiliser les GitHub Issues liées à une PR (`Closes #12`).

### Jour 12 — L'outil `git-flow`
- Refaire un mini-cycle feature → release → hotfix, mais cette fois avec les commandes `git flow feature start/finish`, `git flow release start/finish`, `git flow hotfix start/finish`.
- Défi : comparer le résultat avec ce que tu as fait à la main les jours 6-9 — qu'est-ce que l'outil automatise exactement ?

---

## Phase 5 — Défi final (Jours 13-14)

Un scénario complet, enchaîné sans étapes détaillées cette fois — à toi de dérouler le bon workflow :

> Tu pars de `main` à jour. Il faut livrer une v2.0.0 avec deux nouvelles features développées en parallèle. Pendant que la release est en préparation, un bug critique est signalé en prod sur la v1.x. Tu dois livrer le hotfix sans bloquer ni casser la release en cours, puis terminer et taguer la v2.0.0.

- Livrable : le repo GitHub final (lien), `git log --graph --all --decorate`, et un court résumé de tes choix.
- Revue complète comme si c'était une review de code réelle.

---

## Journal de progression

*(mis à jour au fil des sessions Claude Code)*

- **Jour 1** — 2026-08-16 : dépôt GitHub `gitflow-dojo` créé et relié en local (SSH déjà configuré au préalable), branche par défaut `main`, `.gitignore` ajouté, premier commit poussé. Remarque : git, SSH et `init.defaultBranch` étaient déjà configurés globalement sur la machine, donc étapes rapides.
- **Jour 2** — 2026-08-16 : 5 commits atomiques sur un projet fictif (README qui évolue, TODO.md, NOTES.md), historique vérifié avec `git log --oneline --graph --all` et `git log --stat`. Remarque : `git add -p` testé mais pas utilisable dans ce shell non interactif — session interrompue sans dégât, repris avec `git add` classique.
- **Jour 3** — 2026-08-16 : branche `test-conflit` créée, modification concurrente de la même ligne sur `main` et `test-conflit`, conflit provoqué puis résolu à la main lors du `git merge`, merge commit créé, branche supprimée après fusion. Historique divergent/réuni visible avec `git log --graph --oneline --all`.
- **Jour 4** — 2026-08-16 : 3 commits brouillon (`wip`, `fix typo`, `wip again`) squashés en un seul commit propre via `git rebase -i HEAD~3`, sans perte de contenu. `git reflog` utilisé pour montrer le filet de sécurité en cas de rebase raté. Commits non encore poussés au moment du rebase, donc push normal (pas de `--force` nécessaire).
- **Jour 5** — 2026-08-16 : théorie Gitflow discutée (rôle de `main`, `develop`, `feature/*`, `release/*`, `hotfix/*`). Bonne compréhension du figement d'état par `release` et du rôle de `hotfix`. Point à retravailler si besoin : le critère de choix Gitflow vs trunk-based repose sur la cadence de release et le nombre de versions actives en production simultanément, pas sur l'historique/possibilité de revert (possible aussi en trunk-based).
- **Jour 6** — 2026-08-16 : branche `develop` créée depuis `main`, commit `chore: init develop branch` (vide, `--allow-empty`), poussée sur GitHub et définie comme branche par défaut du repo pour les PR.
- **Jour 7** — 2026-08-16 : deux features en parallèle (`feature/login`, `feature/navbar`) depuis `develop`. `feature/login` mergée en local avec `git merge --no-ff`, `feature/navbar` mergée via une vraie PR GitHub (option "Create a merge commit" pour rester cohérent avec `--no-ff`). Historique `--graph` confirmant les deux merges dans `develop`.
