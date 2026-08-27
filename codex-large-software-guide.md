# Guide de developpement d'un logiciel complet avec Codex

> Statut : guide de reference optionnel.
> Derniere revue : 2026-08-20.
> Ce fichier n'est pas charge automatiquement par Codex et n'a pas a etre lu a
> chaque evolution. `AGENTS.md` reste l'instruction projet obligatoire. Pour le
> travail courant, Codex inspecte les specs, le code et les tests directement
> concernes. Ce guide sert surtout a l'onboarding, au cadrage d'une evolution
> structurante ou a la revision du processus de developpement.

Ce guide decrit comment piloter Codex pour construire, maintenir et faire evoluer un logiciel large, critique et durable. Il est ecrit pour un projet professionnel de gestion patrimoniale, mais la methode s'applique a tout produit logiciel complexe.

L'idee centrale est simple : Codex ne doit pas etre traite comme une memoire magique ni comme un generateur de code ponctuel. Il doit etre pilote comme un membre technique de l'equipe, avec des sources de verite explicites, des workflows courts, des criteres d'acceptation, une discipline Git, et une documentation vivante.

## 1. Principes directeurs

### 1.1 Le depot est la source de verite

Pour un grand logiciel, la memoire fiable ne doit pas vivre uniquement dans la conversation. Elle doit vivre dans le depot :

- les exigences fonctionnelles dans `specs/`;
- les decisions structurantes dans `specs/decisions/`;
- les regles de collaboration agentique dans `AGENTS.md`;
- les instructions utilisateur dans `README.md`;
- le modele de donnees dans `prisma/schema.prisma`;
- les scripts et validations dans `package.json`, `scripts/` et les tests;
- les exemples de prompts dans `specs/prompts/`.

La conversation avec Codex sert a travailler. Les fichiers du depot servent a transmettre, verifier et reprendre le travail.

### 1.2 Codex travaille mieux par taches bornees

Une demande vague comme "termine le module fiscalite" pousse Codex a faire trop d'hypotheses. Une demande robuste contient quatre blocs :

- objectif : ce qui doit changer;
- contexte : les fichiers, specs, erreurs ou captures pertinentes;
- contraintes : architecture, UX, securite, performance, compatibilite;
- definition of done : ce qui doit etre vrai a la fin.

Exemple :

```text
Lis AGENTS.md, README.md, specs/modules/taxation-currencies-performance.md et prisma/schema.prisma.

Objectif : ajouter la prise en charge d'un taux fiscal specifique par enveloppe et par type de revenu.

Contraintes :
- la base interne reste la source de verite;
- ne pas hard-coder de donnees fiscales dans la logique applicative;
- conserver une UX compacte, adaptee a un cabinet;
- mettre a jour les specs concernees avant ou pendant l'implementation.

Done when :
- le schema Prisma represente la regle fiscale;
- les calculs utilisent la regle depuis la base;
- l'UI permet de visualiser et modifier la regle;
- npm run lint et npm run build passent.
```

### 1.3 Les specs accompagnent les changements fonctionnels

Pour un logiciel patrimonial, le risque principal n'est pas seulement le bug technique. C'est l'ambiguite metier : fiscalite, devises, performance, consolidation familiale, imports bancaires, auditabilite.

Pour une fonctionnalite significative ou ambigue, clarifier la spec avant de figer l'implementation. Pour une correction locale ou un comportement deja clairement specifie, la spec peut etre mise a jour pendant l'implementation. Il n'est pas necessaire d'imposer une phase documentaire formelle a chaque petite modification.

Quand une clarification prealable est necessaire :

```text
Avant de coder, lis les specs existantes et propose la modification minimale de spec pour cette feature.
Ne touche pas au code tant que la coherence fonctionnelle, le modele de donnees, les impacts UX et les criteres d'acceptation ne sont pas explicites.
```

### 1.4 La verification fait partie de la demande

Codex produit de meilleurs resultats quand il sait comment prouver son travail. Chaque prompt d'implementation doit indiquer les validations attendues :

- `npm run lint`;
- `npm run build`;
- `npm test` lorsque la logique metier, les validations ou les droits changent;
- tests unitaires ou integration supplementaires selon le risque;
- verification visuelle pour les changements UI;
- revue du diff;
- `git diff --check` avant commit;
- controle des specs et du README.

Dans ce projet, `AGENTS.md` impose deja `npm run lint` et `npm run build` apres implementation.

## 2. Les surfaces de configuration Codex

### 2.1 `AGENTS.md`

`AGENTS.md` est la couche la plus importante pour les regles durables du depot. Codex le lit avant de travailler. Il doit rester court, normatif et actionnable.

Y mettre :

- commandes de build, lint, test;
- conventions non negociables;
- structure du projet;
- regles de documentation;
- contraintes de securite;
- attentes de revue;
- criteres de fin de tache.

Ne pas y mettre :

- des specs metier detaillees;
- de longs journaux de decisions;
- des informations secretes;
- des informations temporaires liees a une seule feature.

Bonne regle : si Codex fait deux fois la meme erreur, ajouter une instruction concise dans `AGENTS.md` ou dans un `AGENTS.md` plus proche du sous-dossier concerne.

### 2.2 `specs/`

`specs/` est la memoire fonctionnelle centrale du produit. Dans ce projet, elle contient deja :

- `specs/modules/core-model.md`;
- `specs/modules/dashboard.md`;
- `specs/modules/envelopes.md`;
- `specs/modules/imports.md`;
- `specs/modules/investments.md`;
- `specs/modules/projects-reporting.md`;
- `specs/modules/revenues.md`;
- `specs/modules/security-deployment.md`;
- `specs/modules/simulations.md`;
- `specs/modules/taxation-currencies-performance.md`;
- `specs/modules/transactions.md`;
- `specs/modules/ux-navigation.md`;
- `specs/audits/module-scorecard-2026-08.md`;
- `specs/prompts/`;
- `specs/decisions/`.

Les specs doivent repondre a ces questions :

- pourquoi la fonctionnalite existe;
- quelles entites sont concernees;
- quelles regles metier s'appliquent;
- quelles donnees sont persistantes;
- quels calculs sont attendus;
- quelles contraintes UX s'appliquent;
- quels criteres d'acceptation valident le resultat;
- quelles migrations ou imports sont impactes.

### 2.3 `README.md`

`README.md` s'adresse aux humains qui lancent, utilisent ou decouvrent l'application. Il doit etre mis a jour quand une evolution change :

- les capacites utilisateur;
- les pages disponibles;
- les scripts de lancement;
- les variables d'environnement;
- le workflow local;
- les limites connues.

Le README n'est pas le lieu des details d'architecture profonds. Il doit rester utile a l'onboarding.

### 2.4 `.codex/config.toml`

Une configuration projet `.codex/config.toml` peut definir des preferences Codex propres au depot, par exemple des options de sandbox, des hooks ou des MCP. Elle ne doit pas contenir de secrets. Ce depot n'en depend pas actuellement pour fonctionner.

Les preferences personnelles restent dans `~/.codex/config.toml`. Les worktrees geres par l'application sont conserves sous `$CODEX_HOME/worktrees`. Les noms de fichiers, options et fonctions Codex evoluent : verifier la documentation officielle avant d'ajouter une configuration projet durable.

### 2.5 Memoires Codex

Les memoires Codex, quand elles sont activees, servent a rappeler des preferences stables et du contexte utile entre conversations. Elles ne remplacent jamais les fichiers du depot.

Utiliser les memoires pour :

- preferences personnelles de communication;
- habitudes recurrentes;
- rappels de contexte non critiques;
- observations de workflow.

Ne pas utiliser les memoires comme seule source pour :

- exigences metier;
- regles de securite;
- decisions d'architecture;
- politiques de donnees;
- conventions d'equipe;
- secrets.

Pour controler les memoires lorsqu'elles sont disponibles, utiliser `/memories`. Pour reduire un long contexte, utiliser `/compact`, une conversation secondaire avec `/side`, un fork avec `/fork`, ou demarrer un nouveau chat selon le besoin. Les commandes disponibles dependent de la surface Codex utilisee : taper `/` dans le composeur reste la source immediate la plus fiable.

### 2.6 Skills, plugins et MCP

Les skills decrivent des workflows reutilisables. Les plugins distribuent des skills, des MCP, des hooks ou des integrations. Les MCP connectent Codex a des systemes externes : GitHub, Figma, Google Drive, documentation interne, navigateur, observabilite.

Regle de choix :

- prompt : contrainte ponctuelle;
- `AGENTS.md` : regle durable du depot;
- spec : verite fonctionnelle;
- skill : workflow repetable;
- plugin : workflow a distribuer;
- MCP : acces a un outil ou systeme externe;
- hook : enforcement mecanique;
- memory : rappel local non critique.

## 3. Structure actuelle du depot

Au 2026-08-20, la structure de reference est celle qui existe reellement dans le depot :

```text
AGENTS.md
README.md
TODO_AUTHENTIFICATION.md
app/                         # pages, routes API, erreurs et etats de chargement
components/                  # composants UI reutilisables
lib/                         # calculs, actions serveur, securite et services metier
prisma/
  schema.prisma
  migrations/
scripts/                     # initialisation, sauvegarde et restauration SQLite
tests/                       # tests Node/TypeScript
docs/
  production-runbook.md
specs/
  README.md
  codex-large-software-guide.md
  modules/
  decisions/
  prompts/
  audits/
.github/workflows/ci.yml
Dockerfile
docker-compose.yml
```

Les repertoires `specs/product/`, `specs/architecture/`, `specs/security/` ou `specs/operations/` restent des extensions possibles, pas des emplacements obligatoires. Ils ne doivent etre crees que lorsqu'ils apportent une separation utile. En attendant :

- les comportements fonctionnels vivent dans `specs/modules/`;
- les choix structurants vivent dans `specs/decisions/`;
- la securite et le deploiement vivent dans `specs/modules/security-deployment.md`;
- l'exploitation vit dans `docs/production-runbook.md`;
- les travaux d'authentification restants vivent dans `TODO_AUTHENTIFICATION.md`.

## 4. Relation entre les fichiers

### 4.1 Changement fonctionnel

Quand une feature change le comportement metier :

1. mettre a jour la spec du module dans `specs/modules/`;
2. mettre a jour le schema ou les calculs si necessaire;
3. implementer dans `app/`, `components/`, `lib/`, `prisma/` ou `scripts/`;
4. mettre a jour `README.md` si l'utilisateur voit un changement;
5. ajouter une decision dans `specs/decisions/` si le choix est structurant;
6. executer les validations.

### 4.2 Changement UX

Quand une page, navigation ou interaction change :

1. mettre a jour `specs/modules/ux-navigation.md` ou la spec module concernee;
2. implementer les composants et pages;
3. verifier responsive, densite, lisibilite, et absence de formulaires lourds sur le dashboard;
4. mettre a jour `README.md` si les pages ou workflows utilisateur changent.

### 4.3 Changement de donnees

Quand le modele persistant change :

1. documenter l'intention dans la spec;
2. modifier `prisma/schema.prisma`;
3. mettre a jour les seeds uniquement comme donnees de demonstration;
4. adapter les imports;
5. adapter les calculs;
6. verifier que la logique ne depend pas de donnees hard-codees.

### 4.4 Changement d'architecture

Quand une decision affecte plusieurs modules :

1. mettre a jour les specs modules impactees;
2. ajouter une ADR dans `specs/decisions/` si le choix doit etre conserve dans le temps;
3. documenter les impacts d'exploitation dans `docs/production-runbook.md` si necessaire;
4. faire valider les impacts avant une implementation risquee;
5. implementer par increments courts.

### 4.5 Changement de securite

Quand une evolution touche des donnees sensibles, roles, imports, fichiers ou audit :

1. mettre a jour `specs/modules/security-deployment.md` et la spec fonctionnelle concernee;
2. decrire les risques et controles;
3. ajouter une ADR si le modele de confiance ou d'autorisation change;
4. eviter tout secret de production dans le repo, les prompts ou les memoires;
5. conserver les identifiants de test uniquement dans `.env` ou un fichier local explicitement ignore;
6. verifier logs, exports, fichiers temporaires et erreurs utilisateur;
7. demander une revue securite dediee lorsque le risque le justifie.

## 5. Workflow Git par feature

Un grand logiciel doit avancer par increments reviewables. La regle pratique : une branche, une intention, une spec, une validation.

### 5.1 Cycle recommande

```text
main
  -> codex/tax-rules-by-envelope
     -> spec
     -> implementation
     -> tests/build
     -> review
     -> merge
```

Etapes :

1. creer une branche courte et nommee par intention, en respectant le nom demande par l'utilisateur ou le prefixe `codex/` par defaut;
2. demander un plan seulement si la tache est complexe, risquee ou doit etre validee avant execution;
3. faire modifier ou creer la spec;
4. implementer la tranche minimale;
5. lancer lint/build/tests;
6. demander a Codex une revue du diff;
7. corriger;
8. commit avec message clair;
9. ouvrir une PR ou fusionner selon le workflow equipe.

### 5.2 Prompts Git utiles

Demander une preparation de branche :

```text
Lis AGENTS.md et specs/README.md. Je veux developper la feature X.
Propose le nom de branche, les fichiers de spec a modifier, les risques et un plan en 5 etapes maximum.
Ne code pas encore.
```

Demander une revue avant commit :

```text
Review les changements non committes comme une PR.
Priorise les bugs, regressions, incoherences specs/code, risques de donnees et tests manquants.
Ne fais pas de refactor non demande.
```

Demander un commit propre :

```text
Inspecte le diff. Propose un decoupage en commits logiques.
Ne stage pas tout aveuglement. Explique les groupes de fichiers puis commit si le decoupage est coherent.
```

### 5.3 Worktrees Codex

Les worktrees permettent a Codex de travailler sur plusieurs taches sans perturber le checkout local. Ils sont utiles pour :

- lancer une exploration en parallele;
- deleguer une correction secondaire;
- isoler une refactorisation;
- eviter que deux threads modifient les memes fichiers.

Precautions :

- ne pas lancer deux threads Codex sur les memes fichiers;
- les worktrees geres demarrent normalement en `detached HEAD`; creer une branche avant de publier le travail;
- utiliser Handoff pour ramener un travail en local;
- les fichiers ignores ne sont pas transmis automatiquement; utiliser `.worktreeinclude` uniquement pour les fichiers locaux necessaires, sans y exposer de secret dans Git;
- nettoyer ou archiver les worktrees inutiles.

## 6. Nettoyer le contexte sans perdre Codex

Le contexte d'une conversation est limite. Codex peut compacter automatiquement, mais la discipline projet reste indispensable.

### 6.1 Quand garder le meme thread

Garder le meme thread si :

- la tache est encore la meme;
- Codex vient d'implementer et doit corriger;
- le contexte accumule est utile;
- les fichiers touches restent limites.

### 6.2 Quand demarrer un nouveau thread

Demarrer un nouveau thread si :

- on change de feature;
- la conversation contient beaucoup d'explorations abandonnees;
- Codex confond anciennes contraintes et nouvelle demande;
- le diff est deja merge ou abandonne;
- une revue independante est souhaitee.

Prompt de relance propre :

```text
Nouveau thread pour la feature X.
Ignore les hypotheses de conversations precedentes.
Lis AGENTS.md, README.md, specs/README.md et les specs suivantes : ...
Etat actuel : ...
Objectif : ...
Contraintes : ...
Done when : ...
```

### 6.3 Commandes utiles dans l'application desktop

Les commandes varient selon la surface et les droits. Taper `/` dans le composeur permet de voir la liste effectivement disponible. Au 2026-08-20, les commandes utiles a ce workflow incluent notamment :

- `/compact` : compacter le contexte du chat courant;
- `/side` : ouvrir une conversation secondaire temporaire;
- `/fork` : copier le chat dans un nouveau chat local ou worktree;
- `/plan` : activer le mode plan pour une tache multi-etapes;
- `/review` : lancer une revue de code;
- `/status` : afficher l'identifiant du chat, le contexte et les limites;
- `/memories` : controler l'usage des memoires lorsqu'elles sont disponibles;
- `/worktree` : demarrer le travail dans un nouveau worktree Git.

Ne pas transformer cette liste en dependance projet : la documentation officielle et le menu du composeur priment si les commandes evoluent.

### 6.4 Ecrire des handoffs

Avant de fermer un long thread, demander :

```text
Redige un handoff court pour reprendre cette feature dans un nouveau thread.
Inclue : objectif, fichiers modifies, decisions prises, validations deja faites, validations restantes, risques connus.
```

Ensuite, coller ce handoff dans une issue, une PR, une ADR, ou le nouveau prompt.

## 7. Prompts types pour developper avec Codex

### 7.1 Creer une spec de feature

```text
Lis AGENTS.md, README.md et specs/README.md.

Je veux ajouter : [description].

Avant toute implementation :
- identifie les modules specs concernes;
- propose les entites, calculs, UX, criteres d'acceptation;
- liste les risques metier, donnees et securite;
- propose les fichiers a modifier.

Ne code pas tant que la spec n'est pas coherente.
```

### 7.2 Implementer depuis une spec

```text
Lis AGENTS.md, README.md et [specs concernees].

Implemente uniquement ce qui est decrit dans la spec.
Contraintes :
- pas de donnees metier hard-codees;
- la base interne est la source de verite;
- UX compacte et professionnelle;
- pas de refactor hors scope.

Mets a jour les specs et README si necessaire.
Termine par npm run lint et npm run build.
```

### 7.3 Debugger un bug

```text
Bug : [description].
Reproduction : [etapes].
Comportement attendu : [attendu].
Comportement observe : [observe].

Lis les fichiers pertinents, identifie la cause racine, propose un correctif minimal, implemente-le, puis verifie avec les commandes adequates.
Si le bug revele une ambiguite de spec, mets la spec a jour.
```

### 7.4 Refactoriser sans regression

```text
Je veux refactoriser [zone] pour [raison].

Contraintes :
- comportement fonctionnel identique;
- pas de changement UX sauf mention explicite;
- commits ou changements faciles a reviewer;
- tests ou validations avant/apres.

Commence par cartographier les dependances et risques.
```

### 7.5 Ajouter une page UX

```text
Ajoute une page [nom] pour [utilisateur cible].

Lis specs/modules/ux-navigation.md et la spec module concernee.
UX attendue :
- cabinet/professionnel;
- compacte;
- tableaux et pages detail plutot que gros ecran unique;
- dashboard reserve au pilotage.

Verifie le rendu responsive et mets a jour README.md si la navigation change.
```

### 7.6 Revue securite

```text
Fais une revue securite de cette feature.
Cherche en priorite :
- exposition de donnees sensibles;
- logs ou erreurs trop bavards;
- imports non valides;
- absence de controle cote serveur;
- secrets dans code, docs, prompts ou fichiers temporaires;
- incoherence entre spec et implementation.

Donne les findings par severite avec fichiers/lignes.
```

### 7.7 Revue architecture

```text
Review l'architecture de cette implementation.
Verifie :
- separation UI / actions serveur / calculs / persistence;
- duplication;
- dependances circulaires;
- coherence Prisma et specs;
- maintenabilite pour de futures features.

Ne propose pas de refactor cosmetique. Priorise les risques reels.
```

## 8. Securite et gouvernance

### 8.1 Sandbox et approvals

Codex agit selon le sandbox et le profil de permissions actifs pour la session. Il peut modifier les racines autorisees et demande une approbation lorsque l'action sort de ce perimetre ou presente un risque particulier.

Regles recommandees :

- conserver un perimetre d'ecriture limite au projet pour le developpement courant;
- demander confirmation pour les commandes reseau, installations, actions destructrices ou acces hors workspace;
- eviter `danger-full-access` sauf environnement isole et intention claire;
- ne jamais accepter une regle d'approbation trop large sans comprendre sa portee.

### 8.2 Secrets

Ne jamais mettre de secret de production dans les prompts, specs, README, seeds ou memoires :

- cles API;
- tokens;
- mots de passe;
- extraits de donnees client sensibles;
- exports patrimoniaux reels non anonymises.

Utiliser `.env` local et ne documenter que les noms de variables dans `.env.example` ou README. Les comptes de test saisis manuellement peuvent etre notes dans un fichier local explicitement ignore, comme `MEMO_COMPTES_TEST.local.md`, a condition qu'ils ne donnent acces a aucun environnement partage ou de production.

### 8.3 Donnees patrimoniales

Pour une application wealth-management :

- distinguer donnees de demonstration et donnees reelles;
- interdire le hard-code de donnees metier dans l'application;
- documenter les imports comme sources, pas comme source de verite;
- centraliser les calculs sensibles;
- garder une trace des decisions fiscales et de performance;
- verifier les exports PDF/Excel pour eviter les fuites.

## 9. Relation avec l'architecture applicative

Dans le stack actuel Next.js 16 App Router, TypeScript, Tailwind, Prisma, SQLite, Recharts, Auth.js et Server Actions, respecter ces frontieres :

- `app/` : pages, Server Components, routes API, erreurs et etats de chargement;
- `components/` : composants UI reutilisables;
- `lib/*-actions.ts` et `lib/actions.ts` : mutations serveur organisees par domaine;
- `lib/calculations.ts`, `lib/performance.ts`, `lib/ledger.ts`, `lib/simulations.ts` : calculs metier testables;
- `lib/auth.ts`, `lib/require-auth.ts`, `lib/workspace.ts`, `lib/server-validation.ts` : authentification, autorisation, isolation workspace et validation serveur;
- `lib/fx*.ts`, `lib/tax*.ts`, `lib/import*.ts`, `lib/snapshots.ts` : services metier specialises;
- `lib/format.ts`, `lib/labels.ts`, `lib/table.ts` : presentation et utilitaires sans donnees business dynamiques;
- `lib/reference-data.ts` : referentiels techniques ou enums, pas donnees business dynamiques;
- `prisma/schema.prisma` et `prisma/migrations/` : modele persistant et evolution versionnee;
- `proxy.ts` : protection HTTP preliminaire; les controles definitifs restent cote serveur;
- `scripts/` : initialisation, sauvegarde et restauration locales;
- `tests/` : preuves unitaires et d'integration locale;
- `.github/workflows/ci.yml` : validation automatisee;
- `docs/production-runbook.md`, `Dockerfile` et `docker-compose.yml` : exploitation mono-instance;
- `specs/` : source fonctionnelle et decisionnelle.

Avant de modifier une API ou une convention Next.js, lire le guide pertinent dans `node_modules/next/dist/docs/`, comme l'impose `AGENTS.md`, car cette version contient des changements incompatibles avec les conventions historiques.

Quand une demande traverse plusieurs couches, demander explicitement a Codex de suivre le flux :

```text
Spec -> Prisma -> actions serveur -> calculs -> UI -> README -> validations.
```

## 10. Best practices operationnelles

Ces pratiques doivent devenir des reflexes de travail avec Codex. Elles ne remplacent pas le jugement humain, mais elles reduisent fortement les risques de derive sur un logiciel long.

### 10.1 Demander un plan avant les taches complexes

Pour une migration, un changement de modele de donnees, une refonte UX ou une correction risquee, un plan valide peut reduire les mauvaises hypotheses. Pour le travail courant clairement borne, Codex peut continuer a explorer, implementer et verifier directement comme aujourd'hui. La lecture du present guide n'est jamais un prealable systematique.

Prompt recommande :

```text
Passe en mode plan.
Lis AGENTS.md, README.md, specs/README.md et les fichiers que tu juges necessaires.

Avant toute modification :
- resume le besoin;
- liste les fichiers probablement concernes;
- propose un plan en 5 a 8 etapes;
- indique les risques metier, techniques, UX et securite;
- precise les validations finales.

Attends ma validation avant de coder.
```

Cette pratique est utile quand l'enjeu est reel ou quand l'utilisateur demande explicitement un plan. Pour une correction bornee ou une modification documentaire simple, un plan formel est inutile.

### 10.2 Valider le plan avant execution

Le plan n'est pas une formalite. Il sert a corriger les hypotheses avant que Codex modifie le code. La validation humaine doit verifier :

- le bon perimetre fonctionnel;
- les specs concernees;
- les donnees persistantes touchees;
- les impacts UX;
- les commandes de validation;
- les risques de regression;
- ce qui est explicitement hors scope.

Prompt de validation :

```text
Le plan est valide avec ces ajustements : [ajustements].
Implemente uniquement ce perimetre.
Ne fais pas de refactor hors scope.
Mets a jour les specs/README si necessaire.
Termine par les validations prevues.
```

### 10.3 Mettre a jour la spec a chaque changement fonctionnel

Une feature qui n'est pas dans la spec devient difficile a maintenir. Une implementation qui contredit la spec est un bug de gouvernance, meme si le code compile.

Regle :

- nouvelle capacite metier : spec obligatoire;
- changement de calcul : spec obligatoire;
- changement de modele de donnees : spec obligatoire;
- changement d'import/export : spec obligatoire;
- changement UX visible : spec module ou UX obligatoire;
- correction purement technique sans impact utilisateur : spec souvent inutile, sauf si elle clarifie une ambiguite.

Prompt utile :

```text
Avant de modifier le code, verifie si cette demande necessite une mise a jour de spec.
Si oui, modifie d'abord la spec concernee avec les criteres d'acceptation.
Ensuite seulement, implemente le changement.
```

### 10.4 Nettoyer le contexte regulierement

Un contexte trop long peut melanger anciennes hypotheses, explorations abandonnees et contraintes encore valides. Il faut donc nettoyer le contexte volontairement.

Bonnes pratiques :

- une feature importante = un thread dedie;
- une revue independante = un nouveau thread;
- un changement de perimetre = nouveau prompt de cadrage;
- une longue session encore utile = `/compact`;
- une session confuse = nouveau chat ou fork depuis un point propre;
- une reprise longue = handoff ecrit.

Prompt de handoff :

```text
Prepare un handoff pour reprendre dans un nouveau thread.
Inclue :
- objectif;
- decisions prises;
- fichiers modifies;
- specs impactees;
- validations effectuees;
- validations restantes;
- risques connus;
- prochaine action recommandee.
```

### 10.5 Travailler avec Git comme garde-fou

Git doit cadrer le travail de Codex. La branche protege le perimetre, le diff permet la revue, et le commit rend l'avancement auditable.

Best practices :

- une branche par feature ou correction;
- pas deux threads sur les memes fichiers;
- commits petits et coherents;
- revue du diff avant commit;
- PR pour toute evolution importante;
- message de commit qui mentionne l'intention, pas seulement les fichiers;
- pas de commit de secrets, exports reels, bases locales ou fichiers temporaires.

Prompt de fin de feature :

```text
Inspecte le diff comme une PR.
Verifie la coherence avec les specs, les risques de regression, les oublis README, et les fichiers qui ne devraient pas etre committes.
Propose ensuite un decoupage de commit en groupes logiques.
```

### 10.6 Preparer le deploiement comme une feature

Le deploiement ne doit pas etre traite comme une commande de fin de session. Pour un logiciel complet, il faut une spec operationnelle ou au minimum une checklist versionnee.

Avant un deploiement, demander a Codex de verifier :

- environnement cible : local, staging, production;
- variables d'environnement;
- schema Prisma et migrations;
- sauvegarde ou restauration de la base;
- donnees de demonstration vs donnees reelles;
- procedure de rollback;
- commandes build;
- controles post-deploiement;
- impact utilisateur;
- securite des imports/exports;
- logs et observabilite.

Prompt recommande :

```text
Prepare le plan de deploiement de cette version.
Lis README.md, package.json, prisma/schema.prisma, scripts/ et les specs concernees.

Produis :
- pre-requis;
- variables d'environnement;
- commandes;
- verification avant deploiement;
- verification apres deploiement;
- rollback;
- risques et points d'attention.

Ne lance aucune commande destructive sans validation.
```

Le projet dispose maintenant de `docs/production-runbook.md`, d'un build Docker standalone, de scripts de sauvegarde/restauration SQLite, de healthchecks et de routes cron. SQLite reste adapte a une seule instance avec volume persistant; PostgreSQL est requis avant un deploiement multi-replica. Avant la production, il faut egalement terminer la configuration des providers de connexion decrite dans `TODO_AUTHENTIFICATION.md`.

### 10.7 Demander une revue finale systematique

Avant d'accepter le resultat, demander a Codex une revue finale. Cette revue doit etre critique, pas decorative.

Prompt recommande :

```text
Fais une revue finale stricte.
Priorise :
- bug fonctionnel;
- incoherence spec/code;
- risque de donnees;
- risque securite;
- regression UX;
- test ou validation manquante;
- fichier qui ne devrait pas etre modifie.

Donne les findings par severite avec fichiers/lignes quand possible.
```

## 11. Definition of done standard

Pour une feature significative, le travail est termine seulement si :

- la spec concernee est a jour;
- l'implementation correspond a la spec;
- aucun comportement non demande n'a ete ajoute;
- la base reste la source de verite;
- le README est a jour si l'utilisateur est impacte;
- les calculs critiques sont verifies;
- `npm test` passe lorsque la logique, la securite ou les validations changent;
- `npm run lint` passe;
- `npm run build` passe;
- les migrations sont testees sur une base propre lorsqu'elles changent;
- les parcours desktop et mobile sont verifies pour une evolution UX;
- `git diff --check` ne remonte pas d'erreur avant commit;
- Codex a resume les fichiers modifies, les validations et les limites restantes.

Prompt final utile :

```text
Avant de conclure, fais une revue finale :
- liste les fichiers modifies;
- confirme les specs/README mis a jour;
- indique les commandes lancees et leur resultat;
- signale les risques restants ou tests non executes;
- verifie qu'aucune donnee metier n'a ete hard-codee.
```

## 12. Anti-patterns

Eviter :

- demander plusieurs features independantes dans un seul prompt;
- laisser Codex coder sans spec pour une regle metier complexe;
- utiliser la memoire Codex comme source de verite;
- accepter des changements non relus;
- ignorer les validations;
- melanger refactor et feature sans raison;
- faire travailler deux threads sur les memes fichiers;
- hard-coder des donnees business;
- mettre des secrets dans les prompts;
- traiter ce guide optionnel comme une lecture obligatoire a chaque petite tache;
- creer un dashboard qui devient un ecran massif de saisie;
- lancer un deploiement sans checklist ni rollback;
- valider un plan sans verifier le perimetre hors scope;
- continuer dans un thread devenu confus au lieu de repartir proprement.

## 13. Cadence recommandee pour un produit long

Pour construire un logiciel complet avec Codex, organiser le travail en cycles courts :

1. cadrage produit;
2. spec module;
3. architecture et securite;
4. implementation tranchee;
5. verification;
6. revue;
7. documentation;
8. merge;
9. retrospective;
10. amelioration de `AGENTS.md`, specs ou prompts si une friction revient.

Cette boucle transforme Codex en systeme d'acceleration durable : chaque feature ameliore non seulement le code, mais aussi la qualite des instructions, des specs et de la memoire projet.

## 14. Sources Codex utilisees

La partie Codex a ete revue le 2026-08-20 a partir de la documentation OpenAI officielle. Ces pages priment sur les exemples de commandes de ce guide si le produit evolue :

- [Custom instructions with AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md);
- [Slash commands](https://learn.chatgpt.com/docs/reference/slash-commands);
- [Worktrees](https://learn.chatgpt.com/docs/environments/git-worktrees);
- [Developer commands](https://learn.chatgpt.com/docs/developer-commands).

Les principes metier et d'architecture restent propres au depot Wealth Management et doivent etre verifies contre `AGENTS.md`, `specs/README.md`, `prisma/schema.prisma`, `package.json` et le code courant.
