# Guide de développement d’un logiciel complet avec Codex

> **Statut :** guide de référence optionnel.  
> Ce fichier n’a pas vocation à être lu intégralement à chaque tâche. `AGENTS.md` reste la couche d’instructions projet permanente. Codex doit surtout inspecter les specs, le code et les tests directement concernés par le travail courant.

Ce guide décrit une méthode générique pour construire, maintenir et faire évoluer un logiciel complexe avec Codex.

Il est volontairement **agnostique** :

- du langage ;
- du framework frontend ou backend ;
- du type d’application ;
- de la base de données ;
- de l’ORM ;
- du cloud ;
- du système de CI/CD ;
- de l’outil de déploiement.

Les exemples de chemins et de commandes sont donc indicatifs et doivent être adaptés au dépôt réel.

L’idée centrale est simple :

> La conversation sert à travailler.  
> Le dépôt sert à mémoriser, transmettre, vérifier et reprendre le travail.

---

# 1. Principes directeurs

## 1.1 Le dépôt est la source de vérité

Pour un logiciel important, la connaissance durable ne doit pas vivre uniquement dans une conversation avec Codex.

Elle doit être versionnée dans le dépôt.

Structure conceptuelle recommandée :

```text
AGENTS.md
README.md

specs/
  modules/
  architecture/
  security/
  data-model/
  decisions/
  prompts/

docs/

src/ ou équivalent
tests/
scripts/
```

Les responsabilités sont différentes :

- `AGENTS.md` : règles permanentes de travail pour les agents ;
- `README.md` : onboarding et utilisation du projet ;
- `specs/modules/` : comportement fonctionnel ;
- `specs/architecture/` : architecture structurante ;
- `specs/security/` : modèle et exigences de sécurité ;
- `specs/data-model/` : modèle de données indépendant de l’implémentation ;
- `specs/decisions/` : décisions d’architecture durables ;
- `specs/prompts/` : prompts réutilisables ;
- `docs/` : exploitation, procédures, documentation technique complémentaire ;
- code, migrations et tests : implémentation réelle.

Les noms exacts peuvent être adaptés au projet.

---

## 1.2 Codex travaille mieux sur des tâches bornées

Éviter :

```text
Termine tout le module de facturation.
```

Préférer une demande structurée avec :

1. objectif ;
2. contexte ;
3. contraintes ;
4. définition de terminé.

Exemple générique :

```text
Lis AGENTS.md, les specs concernées et le code existant.

Objectif :
Ajouter la possibilité d’associer plusieurs devises à un compte.

Contraintes :
- conserver les invariants métier existants ;
- ne pas hard-coder les données métier ;
- respecter l’architecture existante ;
- éviter les refactors hors scope ;
- mettre à jour les specs concernées.

Done when :
- le modèle de données représente correctement le besoin ;
- les règles métier sont implémentées ;
- les interfaces concernées sont adaptées ;
- les tests pertinents passent ;
- les validations projet passent.
```

---

## 1.3 Les specs accompagnent le logiciel

Une fonctionnalité significative doit être décrite indépendamment de son implémentation.

La spec doit répondre principalement à :

- pourquoi la fonctionnalité existe ;
- qui l’utilise ;
- quelles entités sont impliquées ;
- quelles règles métier s’appliquent ;
- quelles données doivent être persistées ;
- quels calculs ou états sont attendus ;
- quelles contraintes UX existent ;
- quels cas d’erreur doivent être gérés ;
- quels critères d’acceptation prouvent que le besoin est couvert.

Pour une correction locale déjà parfaitement spécifiée, une phase documentaire préalable n’est pas obligatoire.

---

## 1.4 La vérification fait partie de l’implémentation

Une tâche n’est pas terminée lorsque le code a simplement été écrit.

Selon le projet, Codex doit exécuter ou vérifier :

- tests unitaires ;
- tests d’intégration ;
- tests end-to-end si pertinents ;
- compilation ou build ;
- lint ;
- validation des types ;
- migrations ;
- analyse statique ;
- contrôle du diff ;
- validation visuelle des changements UI ;
- vérifications sécurité ;
- `git diff --check` ou équivalent.

Les commandes exactes doivent être définies dans `AGENTS.md`.

---

# 2. Les surfaces de configuration et de connaissance

## 2.1 `AGENTS.md`

`AGENTS.md` est la couche de règles durables du dépôt.

Il doit être :

- court ;
- normatif ;
- actionnable ;
- stable.

Y mettre notamment :

- commandes de build, lint et test ;
- conventions obligatoires ;
- architecture générale du dépôt ;
- règles de sécurité ;
- règles de documentation ;
- restrictions importantes ;
- comportement attendu avant de terminer une tâche.

Ne pas y mettre :

- specs métier détaillées ;
- longues explications d’architecture ;
- secrets ;
- journal de projet ;
- contexte temporaire d’une feature.

Bonne règle :

> Si Codex répète régulièrement la même erreur, transformer la correction en règle durable dans `AGENTS.md` ou dans un `AGENTS.md` local au sous-répertoire concerné.

---

## 2.2 `specs/modules/`

Les specs fonctionnelles vivent dans `specs/modules/`.

Exemple :

```text
specs/
  modules/
    authentication.md
    users.md
    billing.md
    reporting.md
    imports.md
    notifications.md
```

Une spec module peut contenir :

```markdown
# Module

## Objectif

## Acteurs

## Concepts métier

## Règles métier

## Données manipulées

## Parcours principaux

## Cas limites

## Permissions

## Critères d’acceptation
```

Ces fichiers doivent décrire **ce que fait le logiciel**, pas comment un framework particulier le réalise.

---

# 3. Les trois specs transverses minimales

En plus des specs fonctionnelles, un logiciel significatif devrait disposer de trois descriptions transverses.

L’objectif n’est pas de produire une documentation lourde.

Quelques pages bien maintenues valent mieux qu’une architecture de 100 pages jamais relue.

---

## 3.1 Spec d’architecture structurante

Fichier recommandé :

```text
specs/architecture/system-architecture.md
```

Cette spec décrit uniquement les choix structurants qui influencent plusieurs modules.

Elle peut rester courte.

Structure recommandée :

```markdown
# Architecture système

## Objectifs architecturaux

Exemples :
- maintenabilité ;
- simplicité ;
- scalabilité ;
- résilience ;
- portabilité ;
- observabilité.

## Vue globale

Décrire les grands composants du système.

Exemple conceptuel :

Client
  -> API / application
  -> services métier
  -> persistence
  -> systèmes externes

## Responsabilités des couches

Décrire où doivent vivre :
- présentation ;
- logique métier ;
- orchestration ;
- accès aux données ;
- intégrations ;
- traitements asynchrones.

## Frontières importantes

Exemples :
- l’UI ne contient pas de logique métier critique ;
- la persistence n’est pas appelée directement depuis toutes les couches ;
- les intégrations externes sont isolées ;
- les règles métier importantes restent testables indépendamment du framework.

## Flux structurants

Documenter uniquement les flux importants :
- authentification ;
- création d’une transaction ;
- traitement asynchrone ;
- import ;
- génération d’un rapport.

## Dépendances externes critiques

Lister les catégories :
- base de données ;
- stockage ;
- identité ;
- messaging ;
- paiement ;
- services tiers.

Éviter d’y dupliquer la configuration détaillée.

## Contraintes techniques majeures

Exemples :
- mono-tenant ou multi-tenant ;
- traitement synchrone ou asynchrone ;
- cohérence transactionnelle ;
- contraintes de volumétrie ;
- disponibilité cible.

## Décisions structurantes

Pointer vers les ADR concernées.
```

Cette spec ne doit pas devenir un catalogue de classes, de fichiers ou de composants techniques.

---

## 3.2 Spec de sécurité

Fichier recommandé :

```text
specs/security/security-model.md
```

Elle décrit le **modèle de sécurité du produit**, indépendamment d’un fournisseur d’identité ou d’un framework particulier.

Structure recommandée :

```markdown
# Modèle de sécurité

## Périmètre

## Types d’utilisateurs

## Authentification

Décrire les principes :
- identité vérifiée ;
- gestion des sessions ;
- MFA si nécessaire ;
- récupération de compte.

## Autorisation

Décrire le modèle :
- rôles ;
- permissions ;
- ownership ;
- séparation par organisation / tenant / workspace ;
- politiques particulières.

## Données sensibles

Identifier :
- PII ;
- secrets ;
- données financières ;
- données réglementées ;
- fichiers sensibles.

## Contrôles essentiels

Exemples :
- contrôle serveur obligatoire ;
- validation des entrées ;
- protection contre l’accès horizontal ;
- principe du moindre privilège ;
- filtrage des exports ;
- protection des logs ;
- gestion des secrets.

## Auditabilité

Décrire ce qui doit être traçable :
- connexions ;
- changements sensibles ;
- modifications de permissions ;
- imports / exports ;
- actions administratives.

## Gestion des secrets

Définir les principes :
- aucun secret dans Git ;
- variables d’environnement ou secret manager ;
- rotation ;
- séparation par environnement.

## Risques principaux

Lister les risques propres au produit.

## Critères de validation sécurité
```

Une feature qui modifie le modèle de confiance, les rôles ou l’isolation doit mettre à jour cette spec et éventuellement produire une ADR.

---

## 3.3 Spec du modèle de données

Fichier recommandé :

```text
specs/data-model/domain-model.md
```

Cette spec est volontairement **indépendante de Prisma, Hibernate, Entity Framework, SQLAlchemy, Django ORM ou de tout autre outil**.

Elle représente le modèle de référence dont l’implémentation physique devra s’inspirer.

Exemple :

```markdown
# Modèle de données

## Principes

- identifier les entités métier ;
- décrire leurs relations ;
- définir les invariants importants ;
- éviter les détails propres à un ORM.

## Entités

### User

Responsabilité :
Représente un utilisateur du système.

Attributs principaux :
- id
- email
- displayName
- status
- createdAt

Relations :
- appartient éventuellement à une ou plusieurs organisations.

Contraintes :
- email unique dans le périmètre défini par le produit.

### Organization

Responsabilité :
Représente un espace métier isolé.

Attributs principaux :
- id
- name
- status

Relations :
- contient des utilisateurs ;
- contient des ressources métier.
```

La spec peut également contenir un diagramme Mermaid :

```mermaid
erDiagram
    USER ||--o{ MEMBERSHIP : has
    ORGANIZATION ||--o{ MEMBERSHIP : contains
    ORGANIZATION ||--o{ RESOURCE : owns
```

Pour chaque entité importante, documenter au minimum :

- responsabilité ;
- attributs métier principaux ;
- relations ;
- cardinalités ;
- contraintes ;
- règles d’unicité ;
- cycle de vie ;
- règles de suppression ;
- données sensibles éventuelles.

Éviter de figer inutilement :

- type SQL précis ;
- nom d’index spécifique ;
- syntaxe ORM ;
- annotation framework ;
- stratégie physique détaillée.

Ces éléments appartiennent à l’implémentation ou aux migrations.

La relation recommandée est donc :

```text
specs/data-model/domain-model.md
        ↓
modèle ORM / schéma SQL / documents / collections
        ↓
migrations
        ↓
base réelle
```

Le modèle ORM doit **implémenter** la spec de données, pas devenir l’unique documentation conceptuelle.

---

# 4. ADR : décisions structurantes

Les décisions qui ont un impact durable doivent être conservées dans :

```text
specs/decisions/
```

Exemple :

```text
001-use-relational-database.md
002-separate-domain-services.md
003-multi-tenant-isolation.md
```

Format simple :

```markdown
# ADR-003 — Isolation multi-tenant

## Contexte

## Décision

## Alternatives considérées

## Conséquences

## Date
```

Créer une ADR uniquement lorsqu’une décision mérite réellement d’être conservée.

Ne pas transformer chaque choix de code en ADR.

---

# 5. README

Le `README.md` s’adresse principalement à un humain qui découvre ou exécute le projet.

Il doit généralement contenir :

- objectif du projet ;
- prérequis ;
- installation ;
- configuration ;
- démarrage ;
- tests ;
- structure générale ;
- variables d’environnement ;
- liens vers les specs.

Éviter d’y déplacer toute la documentation métier ou architecture.

---

# 6. Relation entre les fichiers

## 6.1 Changement fonctionnel

Quand le comportement métier change :

```text
spec module
    ↓
modèle de données si nécessaire
    ↓
implémentation
    ↓
tests
    ↓
README si impact utilisateur
```

Créer ou modifier une ADR uniquement si une décision structurante apparaît.

---

## 6.2 Changement de données

Quand une donnée persistante change :

1. mettre à jour la spec fonctionnelle si nécessaire ;
2. mettre à jour `specs/data-model/domain-model.md` si le modèle conceptuel change ;
3. modifier le modèle ORM ou le schéma physique ;
4. créer la migration si nécessaire ;
5. adapter les services métier ;
6. adapter les imports/exports ;
7. vérifier les tests et migrations.

---

## 6.3 Changement d’architecture

Quand une décision touche plusieurs modules :

1. mettre à jour la spec d’architecture ;
2. créer une ADR si la décision mérite d’être conservée ;
3. identifier les modules impactés ;
4. implémenter par incréments ;
5. vérifier l’absence de dérive architecture/code.

---

## 6.4 Changement de sécurité

Quand une évolution touche :

- authentification ;
- autorisation ;
- rôles ;
- données sensibles ;
- isolation ;
- imports ;
- exports ;
- audit ;
- secrets ;

alors :

1. mettre à jour la spec sécurité ;
2. mettre à jour la spec fonctionnelle concernée ;
3. créer une ADR si le modèle de confiance change ;
4. ajouter ou adapter les tests sécurité ;
5. revoir logs, erreurs et données temporaires.

---

# 7. Workflow Git recommandé

Un logiciel important doit évoluer par changements reviewables.

Principe :

> Une branche = une intention principale.

Cycle typique :

```text
main
  -> feature/...
      -> spec
      -> implementation
      -> tests
      -> review
      -> commit
      -> PR
      -> merge
```

Étapes :

1. créer une branche courte ;
2. lire les instructions et specs concernées ;
3. clarifier le besoin ;
4. mettre à jour les specs nécessaires ;
5. implémenter la tranche minimale ;
6. exécuter les validations ;
7. revoir le diff ;
8. corriger ;
9. commit ;
10. PR ou merge selon le workflow de l’équipe.

---

# 8. Revue du diff

Prompt générique utile :

```text
Review les changements non committés comme une Pull Request.

Priorise :
- bugs ;
- régressions ;
- incohérences specs / code ;
- problèmes de modèle de données ;
- risques sécurité ;
- tests manquants ;
- changement hors scope.

Ne propose pas de refactor cosmétique non demandé.
```

---

# 9. Travailler sur les tâches longues

Le contexte conversationnel est temporaire.

Pour une tâche longue, utiliser un fichier de reprise.

Exemple :

```text
TASK_STATE.md
```

Structure :

```markdown
# Task State

## Objectif

## Terminé

## En cours

## À faire

## Décisions prises

## Fichiers importants

## Validations effectuées

## Risques / blocages

## Prochaine étape
```

Codex doit mettre ce fichier à jour régulièrement pour les tâches longues ou interrompables.

Ce fichier peut être supprimé à la fin si sa conservation n’a plus de valeur.

---

# 10. Reprendre après une interruption

Prompt recommandé :

```text
Continue.

Avant de poursuivre, reconstruis précisément l’état du travail à partir de :

1. TASK_STATE.md
2. git status
3. git diff
4. les fichiers du repository
5. l’historique encore disponible

Identifie ce qui est déjà terminé et ce qui reste à faire.

Ne refais pas les tâches terminées.

Puis continue la tâche initiale jusqu’à son terme.
```

La reprise doit s’appuyer en priorité sur l’état réel du dépôt.

---

# 11. Changer de thread sans perdre le contexte utile

Garder le même thread lorsque :

- la tâche reste identique ;
- le contexte récent est encore utile ;
- Codex doit corriger sa propre implémentation ;
- le nombre de fichiers reste raisonnable.

Créer un nouveau thread lorsque :

- on change de feature ;
- le contexte contient trop d’explorations abandonnées ;
- des contraintes anciennes polluent la tâche ;
- une revue indépendante est souhaitée ;
- la feature précédente est terminée.

Prompt de reprise propre :

```text
Nouveau thread pour [feature].

Lis :
- AGENTS.md ;
- README.md ;
- les specs concernées ;
- TASK_STATE.md s’il existe ;
- le diff courant si nécessaire.

État actuel :
[...]

Objectif :
[...]

Contraintes :
[...]

Done when :
[...]
```

---

# 12. Handoff

Avant de quitter une longue session :

```text
Rédige un handoff court permettant de reprendre cette tâche dans un nouveau thread.

Inclure :
- objectif ;
- état actuel ;
- décisions prises ;
- fichiers modifiés ;
- validations réalisées ;
- validations restantes ;
- risques connus ;
- prochaine étape.
```

Un handoff peut ensuite être placé dans :

- `TASK_STATE.md` ;
- une issue ;
- une PR ;
- une note temporaire ;
- le prochain prompt.

---

# 13. Prompts réutilisables

## 13.1 Cadrer une feature

```text
Lis AGENTS.md, README.md et les specs existantes.

Je veux ajouter :
[description]

Avant l’implémentation :

- identifie les specs concernées ;
- clarifie les règles métier ;
- identifie les impacts architecture, données et sécurité ;
- propose les critères d’acceptation ;
- liste les fichiers probablement concernés.

Ne crée pas de complexité qui n’est pas nécessaire au besoin.
```

---

## 13.2 Implémenter depuis une spec

```text
Lis AGENTS.md et les specs concernées.

Implémente uniquement ce qui est décrit.

Contraintes :
- respecter l’architecture existante ;
- ne pas hard-coder les données métier ;
- ne pas introduire de dépendance inutile ;
- ne pas refactorer hors scope ;
- préserver les comportements existants non concernés.

Mets à jour les specs et README si nécessaire.

Termine par les validations définies dans AGENTS.md.
```

---

## 13.3 Debugger

```text
Bug :
[description]

Reproduction :
[étapes]

Attendu :
[...]

Observé :
[...]

Identifie la cause racine avant de modifier le code.

Implémente ensuite le correctif minimal.

Si le bug révèle une ambiguïté de spec, mets la documentation correspondante à jour.

Exécute les validations pertinentes.
```

---

## 13.4 Refactoriser

```text
Je veux refactoriser [zone] pour [objectif].

Contraintes :
- comportement fonctionnel inchangé ;
- modèle de données inchangé sauf demande explicite ;
- pas de changement UX ;
- changements faciles à reviewer ;
- validations avant et après.

Commence par cartographier les dépendances et les risques.
```

---

## 13.5 Revue architecture

```text
Review l’architecture de cette implémentation.

Compare-la avec :
- specs/architecture/ ;
- specs/data-model/ ;
- les ADR pertinentes.

Vérifie notamment :
- séparation des responsabilités ;
- dépendances incorrectes ;
- duplication de logique métier ;
- couplage excessif ;
- dépendances circulaires ;
- cohérence avec le modèle de données ;
- maintenabilité.

Priorise les risques réels.
```

---

## 13.6 Revue sécurité

```text
Fais une revue sécurité de cette feature.

Compare avec specs/security/.

Cherche en priorité :
- contrôle d’accès manquant ;
- validation uniquement côté client ;
- accès horizontal possible ;
- exposition de données sensibles ;
- secrets ;
- logs trop bavards ;
- erreurs révélant des informations internes ;
- imports non validés ;
- exports trop permissifs ;
- incohérence entre spec et implémentation.

Classe les findings par sévérité.
```

---

## 13.7 Revue du modèle de données

```text
Review le changement de modèle de données.

Compare :
- la spec fonctionnelle ;
- specs/data-model/domain-model.md ;
- le modèle ORM ou schéma physique ;
- les migrations.

Vérifie :
- cardinalités ;
- contraintes ;
- unicité ;
- nullable / obligatoire ;
- ownership ;
- cycle de vie ;
- suppression ;
- compatibilité avec les données existantes.

Ne propose pas d’optimisation physique sans besoin démontré.
```

---

# 14. Plan avant tâche complexe

Pour une migration, une refonte importante ou un changement à risque :

```text
Passe en mode plan.

Lis les instructions, specs et fichiers pertinents.

Avant toute modification :

- résume le besoin ;
- identifie les specs impactées ;
- identifie les impacts architecture, sécurité et données ;
- liste les fichiers probablement concernés ;
- propose un plan en 5 à 8 étapes ;
- indique les principaux risques ;
- précise les validations finales.

N’implémente rien avant validation du plan.
```

Pour une correction locale simple, un plan formel est inutile.

---

# 15. Développement incrémental

Éviter de demander à Codex de modifier simultanément de grandes parties indépendantes du système.

Préférer :

```text
spec
  ↓
modèle
  ↓
backend / domaine
  ↓
interface
  ↓
tests
  ↓
review
```

Chaque incrément doit être suffisamment petit pour être compris et revu.

---

# 16. Sécurité des secrets

Ne jamais versionner :

- clés API ;
- mots de passe ;
- tokens ;
- certificats privés ;
- credentials cloud ;
- données personnelles réelles inutiles ;
- exports clients sensibles.

Utiliser selon l’environnement :

- variables d’environnement ;
- secret manager ;
- vault ;
- credentials temporaires.

Le dépôt peut contenir les **noms** des variables requises, mais pas leurs valeurs réelles.

---

# 17. Données de démonstration et données réelles

Séparer clairement :

```text
données de démonstration
≠
données de développement
≠
données de test
≠
données de production
```

Éviter que :

- les seeds deviennent une source de vérité métier ;
- les tests dépendent de données réelles ;
- des exports production soient committés ;
- les exemples contiennent des données personnelles.

---

# 18. Architecture : principes génériques

Quel que soit le framework, chercher à conserver des frontières claires entre :

```text
présentation
application / orchestration
domaine métier
persistence
intégrations externes
infrastructure
```

Ce découpage n’implique pas nécessairement six dossiers différents.

Le principe important est que les responsabilités restent compréhensibles et testables.

Exemple de flux :

```text
UI / API
   ↓
use case / application service
   ↓
domain logic
   ↓
repository / persistence
```

Pour une architecture plus simple, plusieurs couches peuvent être fusionnées.

Ne pas introduire de complexité architecturale sans justification.

---

# 19. Modèle de données : source conceptuelle vs implémentation

Le modèle conceptuel décrit le métier.

L’ORM ou le schéma physique décrit son implémentation.

Exemples d’implémentations possibles :

```text
specs/data-model/domain-model.md
        ↓
Prisma
Hibernate
Entity Framework
SQLAlchemy
Django ORM
Drizzle
TypeORM
SQL natif
MongoDB schema
autre
```

La spec ne doit donc pas contenir d’hypothèse inutile propre à l’outil choisi.

Lorsqu’un ORM change, le modèle métier doit rester compréhensible.

---

# 20. Déploiement

Le déploiement doit être traité comme une opération contrôlée.

Avant une mise en production, vérifier au minimum :

- environnement cible ;
- configuration ;
- secrets ;
- migrations ;
- compatibilité des données ;
- stratégie de rollback ;
- build ;
- tests ;
- observabilité ;
- health checks ;
- sauvegarde si nécessaire ;
- changements de sécurité ;
- changements d’infrastructure ;
- impacts utilisateurs.

Prompt :

```text
Prépare le plan de déploiement de cette version.

Produis :
- prérequis ;
- configuration ;
- migrations ;
- commandes nécessaires ;
- vérifications avant déploiement ;
- vérifications après déploiement ;
- rollback ;
- risques.

Ne lance aucune action destructive sans validation explicite.
```

---

# 21. Definition of Done standard

Pour une feature significative, le travail est terminé lorsque :

- la spec fonctionnelle est à jour ;
- la spec architecture est à jour si un choix structurant change ;
- la spec sécurité est à jour si le modèle de sécurité change ;
- la spec data-model est à jour si le modèle conceptuel change ;
- les ADR nécessaires existent ;
- l’implémentation correspond aux specs ;
- aucun comportement non demandé n’a été ajouté ;
- les tests pertinents passent ;
- le build passe ;
- le lint passe ;
- les migrations sont validées si elles changent ;
- les parcours UI importants sont vérifiés si nécessaire ;
- le diff a été relu ;
- aucun secret ou fichier indésirable n’est introduit ;
- les risques ou validations non effectuées sont explicitement signalés.

---

# 22. Anti-patterns

Éviter :

- mettre toute la connaissance dans les conversations ;
- utiliser la mémoire de Codex comme source de vérité ;
- écrire des specs dépendantes d’un framework sans nécessité ;
- utiliser le schéma ORM comme seule documentation métier ;
- mettre toute l’architecture dans `README.md` ;
- transformer `AGENTS.md` en documentation exhaustive ;
- demander plusieurs features indépendantes dans un même prompt ;
- lancer un refactor non lié pendant une feature ;
- coder une règle métier ambiguë sans clarification ;
- faire travailler deux agents sur les mêmes fichiers sans coordination ;
- hard-coder des données métier ;
- committer des secrets ;
- ignorer les migrations ;
- ignorer les tests ;
- accepter un diff sans revue ;
- créer des abstractions prématurées ;
- sur-architecturer un besoin simple ;
- conserver un thread devenu confus alors que le dépôt permet de repartir proprement.

---

# 23. Structure de référence recommandée

Structure générique :

```text
AGENTS.md
README.md

specs/
  README.md

  modules/
    module-a.md
    module-b.md

  architecture/
    system-architecture.md

  security/
    security-model.md

  data-model/
    domain-model.md

  decisions/
    001-example.md

  prompts/
    feature.md
    review.md

docs/
  runbook.md

src/
tests/
scripts/
```

Cette structure est une **référence**, pas une obligation.

Un petit projet peut utiliser :

```text
specs/
  product.md
  architecture.md
  security.md
  data-model.md
```

L’objectif est la clarté, pas le nombre de fichiers.

---

# 24. Cadence recommandée pour un produit long

Cycle conseillé :

1. besoin ;
2. spec fonctionnelle ;
3. vérification architecture / sécurité / données ;
4. plan si nécessaire ;
5. implémentation incrémentale ;
6. tests ;
7. revue ;
8. documentation ;
9. commit / PR ;
10. merge ;
11. retour d’expérience.

Après chaque friction récurrente, demander :

> Est-ce qu’une nouvelle règle doit être ajoutée dans `AGENTS.md`, une spec, une ADR ou un prompt réutilisable ?

Le dépôt devient ainsi progressivement meilleur pour les humains comme pour les agents.

---

# 25. Règle de décision : où documenter quoi ?

| Information | Emplacement |
|---|---|
| Règle permanente pour Codex | `AGENTS.md` |
| Fonctionnalité métier | `specs/modules/` |
| Architecture structurante | `specs/architecture/` |
| Sécurité | `specs/security/` |
| Modèle conceptuel / logique de données | `specs/data-model/` |
| Décision durable | `specs/decisions/` |
| Installation / utilisation | `README.md` |
| Exploitation | `docs/` |
| Prompt répétable | `specs/prompts/` |
| État temporaire d’une longue tâche | `TASK_STATE.md` |
| Implémentation réelle | code + migrations + tests |

---

# 26. Principe final

Codex ne doit pas être utilisé comme une mémoire magique capable de reconstruire indéfiniment un logiciel depuis une conversation.

Le système fiable est :

```text
Intentions humaines
      ↓
Specs
      ↓
Architecture / Security / Data Model
      ↓
Code
      ↓
Tests
      ↓
Git
      ↓
Review
```

Codex intervient dans chacune de ces étapes, mais la source de vérité reste le dépôt.

La meilleure utilisation de Codex sur un logiciel long consiste donc à construire progressivement un projet où :

- les règles sont explicites ;
- les décisions importantes sont versionnées ;
- les specs restent proches du code ;
- le modèle métier n’est pas lié à un framework ;
- les changements sont petits et vérifiables ;
- Git protège l’historique ;
- les tests prouvent les comportements ;
- une nouvelle conversation peut reprendre le travail en relisant le dépôt.
