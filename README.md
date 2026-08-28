# Codex Instructions

Repository de référence pour **travailler efficacement avec Codex sur des projets logiciels**, en particulier lorsque le projet devient long, complexe ou doit pouvoir être repris après une interruption de contexte.

L’objectif n’est pas de fournir des instructions propres à une stack particulière, mais une méthode **agnostique** applicable quel que soit le langage, le framework, l’ORM, la base de données, le cloud ou la CI/CD.

> **Principe central :** la conversation sert à travailler ; le repository sert à mémoriser, transmettre, vérifier et reprendre le travail.

---

## Contenu du repository

| Fichier | Rôle |
|---|---|
| [`AGENTS.md`](./AGENTS.md) | Instructions permanentes destinées à Codex, notamment pour la reprise des tâches longues. |
| [`prompt.md`](./prompt.md) | Prompts réutilisables pour démarrer, reprendre et organiser une tâche Codex. |
| [`codex-large-software-guide-agnostic.md`](./codex-large-software-guide-agnostic.md) | Guide complet et agnostique pour développer et maintenir un logiciel avec Codex. |
| `README.md` | Vue d’ensemble et point d’entrée du repository. |

---

## 1. Philosophie générale

Pour un projet logiciel important, la connaissance durable ne doit pas dépendre uniquement de l’historique d’une conversation.

Elle doit être versionnée dans le repository :

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

Codex peut intervenir à chaque étape, mais **le repository reste la source de vérité**.

Les conséquences pratiques sont simples :

- donner à Codex des tâches clairement bornées ;
- conserver les règles permanentes dans `AGENTS.md` ;
- documenter les comportements significatifs dans des specs ;
- versionner les décisions structurantes ;
- maintenir les tests et la documentation avec le code ;
- vérifier systématiquement le diff et les validations du projet ;
- utiliser `TASK_STATE.md` pour les travaux longs ou interrompables.

---

## 2. Utilisation recommandée de `AGENTS.md`

`AGENTS.md` constitue la couche d’instructions permanente d’un projet pour Codex.

Il doit rester :

- court ;
- normatif ;
- stable ;
- actionnable.

On peut y mettre notamment :

- les commandes de build, test, lint et validation ;
- les conventions obligatoires ;
- les règles de sécurité ;
- les contraintes architecturales importantes ;
- les règles de documentation ;
- le comportement attendu avant de terminer une tâche.

Il ne doit pas devenir une documentation exhaustive du produit.

Le `AGENTS.md` fourni dans ce repository ajoute surtout une règle importante pour les tâches longues : maintenir un fichier `TASK_STATE.md` permettant de reprendre le travail sans recommencer ce qui a déjà été réalisé.

---

## 3. Tâches longues et reprise après interruption

Pour une tâche susceptible de dépasser une session ou la fenêtre de contexte, Codex doit maintenir :

```text
TASK_STATE.md
```

Ce fichier doit contenir au minimum :

```markdown
# Task State

## Objectif

## Terminé

## En cours

## À faire

## Décisions prises

## Fichiers modifiés / importants

## Validations effectuées

## Risques / problèmes connus

## Prochaine étape
```

Lors d’une reprise, Codex doit reconstruire l’état réel du travail à partir de :

1. `TASK_STATE.md` ;
2. `git status` ;
3. `git diff` ;
4. les fichiers du repository ;
5. l’historique de conversation encore disponible.

Puis identifier ce qui est déjà terminé, ce qui reste à faire et **ne pas refaire les tâches terminées**.

Les prompts prêts à l’emploi sont disponibles dans [`prompt.md`](./prompt.md).

---

## 4. Organisation recommandée d’un projet piloté avec Codex

Pour un logiciel significatif, le guide recommande une structure conceptuelle de ce type :

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

Cette structure est une référence et non une obligation. Un petit projet peut utiliser beaucoup moins de fichiers.

### Specs fonctionnelles

Les specs de modules décrivent **ce que le logiciel doit faire**, indépendamment de la technologie choisie :

- objectif ;
- acteurs ;
- concepts métier ;
- règles métier ;
- données manipulées ;
- parcours principaux ;
- cas limites ;
- permissions ;
- critères d’acceptation.

### Spec d’architecture

Fichier recommandé :

```text
specs/architecture/system-architecture.md
```

Elle décrit uniquement les choix structurants : composants principaux, responsabilités des couches, frontières importantes, flux structurants, dépendances critiques et contraintes techniques majeures.

### Spec de sécurité

Fichier recommandé :

```text
specs/security/security-model.md
```

Elle décrit notamment :

- authentification ;
- autorisation ;
- rôles et permissions ;
- isolation des utilisateurs / tenants / workspaces ;
- données sensibles ;
- auditabilité ;
- gestion des secrets ;
- principaux risques et contrôles de sécurité.

### Spec du modèle de données

Fichier recommandé :

```text
specs/data-model/domain-model.md
```

Cette spec décrit le **modèle métier conceptuel**, indépendamment de Prisma, Hibernate, Entity Framework, SQLAlchemy, Django ORM ou de toute autre implémentation.

La relation recommandée est :

```text
specs/data-model/domain-model.md
        ↓
modèle ORM / schéma SQL / documents / collections
        ↓
migrations
        ↓
base réelle
```

L’ORM implémente le modèle ; il ne doit pas devenir l’unique documentation du modèle métier.

---

## 5. Workflow recommandé avec Codex

Pour une nouvelle feature :

```text
Besoin
  ↓
Spec fonctionnelle
  ↓
Impacts architecture / sécurité / données
  ↓
Plan si nécessaire
  ↓
Implémentation incrémentale
  ↓
Tests et validations
  ↓
Review du diff
  ↓
Documentation
  ↓
Commit / PR
```

Pour préparer une demande importante, [`prompt.md`](./prompt.md) propose également le workflow suivant :

1. brainstormer dans ChatGPT ;
2. générer le plan ;
3. générer le prompt ;
4. préparer les inputs nécessaires ;
5. exécuter ensuite le travail dans Codex.

Une pratique proposée est de considérer :

- **un projet Codex** pour un produit ;
- **un nouveau chat/thread** pour une feature ou fonctionnalité distincte.

---

## 6. Definition of Done

Une feature significative n’est pas terminée lorsque le code est simplement écrit.

Selon le projet, il faut vérifier :

- conformité avec les specs ;
- tests unitaires et d’intégration ;
- tests end-to-end si pertinents ;
- build / compilation ;
- lint ;
- validation des types ;
- migrations ;
- sécurité ;
- parcours UI importants ;
- cohérence architecture / code ;
- absence de secrets ;
- revue du `git diff` ;
- absence de changements hors scope.

Les commandes concrètes propres au projet doivent être définies dans son `AGENTS.md`.

---

## 7. Principaux anti-patterns

Éviter notamment de :

- considérer la mémoire conversationnelle de Codex comme la source de vérité ;
- demander plusieurs features indépendantes dans une seule tâche ;
- utiliser le schéma ORM comme seule documentation métier ;
- transformer `README.md` en documentation architecturale exhaustive ;
- transformer `AGENTS.md` en manuel de plusieurs dizaines de pages ;
- hard-coder des données métier ;
- lancer des refactors hors scope ;
- ignorer les migrations, tests ou revues de diff ;
- versionner des secrets ou des données de production sensibles ;
- sur-architecturer un besoin simple ;
- refaire du travail déjà terminé après une reprise de contexte.

---

## 8. Guide complet

Pour les détails, patterns, prompts, workflows Git, ADR, revues architecture/sécurité/data-model, déploiement et exemples de structures, consulter :

**[`codex-large-software-guide-agnostic.md`](./codex-large-software-guide-agnostic.md)**

Le guide est volontairement plus complet et sert de **référence optionnelle**. Il n’a pas vocation à être relu intégralement par Codex à chaque tâche : les instructions permanentes doivent rester dans `AGENTS.md`, tandis que Codex doit surtout inspecter les specs, le code et les tests directement concernés.

---

## 9. Résumé

Ce repository fournit une méthode pour rendre l’utilisation de Codex plus fiable sur des projets logiciels réels :

```text
Règles permanentes     → AGENTS.md
Specs métier           → specs/modules/
Architecture           → specs/architecture/
Sécurité               → specs/security/
Modèle de données      → specs/data-model/
Décisions durables     → specs/decisions/
Prompts réutilisables  → specs/prompts/ ou prompt.md
État temporaire        → TASK_STATE.md
Implémentation réelle  → code + migrations + tests
```

Le but final est qu’un humain ou un nouveau thread Codex puisse reprendre le projet en relisant le repository, sans dépendre d’une conversation précédente.