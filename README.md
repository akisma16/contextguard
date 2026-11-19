# ContextGuard

**Un skill pour estimer le budget de contexte et préparer une reprise fidèle de vos conversations.**

Créé par **Adam KISMA** · Version **1.0.0** · Instructions en français.

ContextGuard aide un assistant compatible avec les Agent Skills à repérer les
signaux de saturation d'une conversation, estimer le volume du texte visible et
produire une fiche de reprise utilisable dans un nouveau chat.

## Fonctionnalités

- Estimation indicative du nombre de tokens du texte accessible, sous forme de fourchette.
- Distinction entre données mesurées, estimations et informations indisponibles.
- Vérifications du contexte aux étapes naturelles d'une tâche longue.
- Seuil d'alerte configurable, à 25 % par défaut lorsque la capacité et l'usage sont réellement connus.
- Fiche de reprise : objectif, décisions, références, résultats, travail restant et prompt de relance.

## Installation dans Codex

Demandez à Codex :

```text
Utilise skill-installer pour installer ce skill :
https://github.com/akisma16/contextguard/tree/main/skills/contextguard
```

Ou copiez le dossier `skills/contextguard` dans votre répertoire personnel
`~/.agents/skills/`, ou dans `.agents/skills/` à la racine de votre projet.
Conservez le dossier et son fichier `SKILL.md` ensemble.

Si le skill n'apparaît pas dans la liste des skills disponibles, redémarrez Codex.
La disponibilité dépend du produit, de sa version et de la configuration de votre environnement.

## Utilisation

Après installation, demandez explicitement l'utilisation du skill :

```text
Utilise le skill contextguard pour cette conversation.
Seuil d'alerte : 25 % de contexte restant si ce pourcentage est vérifiable.
```

Puis, selon votre besoin :

```text
ContextGuard : état
ContextGuard : combien de tokens pour ce prompt ?
ContextGuard : seuil 25 %
ContextGuard : sauvegarde
```

Dans les interfaces qui proposent un sélecteur de skills, sélectionnez
`contextguard` avec la syntaxe proposée par le produit (`$` ou `@`).

## Limites essentielles

Ce skill fournit une procédure à suivre par l'assistant. Il n'ajoute aucun accès
aux compteurs internes du modèle ni à sa capacité réelle de contexte.

- Un seuil de 25 % n'est vérifiable que si des compteurs comparables sont explicitement fournis.
- À défaut, ContextGuard signale les risques observables et indique que le pourcentage restant est inconnu.
- Il ne fonctionne pas comme un processus de surveillance en arrière-plan.
- Il ne peut pas garantir une alerte avant une coupure ou une compaction.
- L'activation d'un chat ne garantit pas sa persistance dans les suivants.
- Un résumé de reprise ne transfère pas automatiquement les fichiers, outils ou autorisations.
- Les estimations de tokens ne sont pas une mesure du coût facturé par une API.

L'installation locale dans Codex ne publie pas le skill dans l'annuaire ChatGPT.

## Contenu du dépôt

```text
skills/contextguard/SKILL.md   Instructions du skill
plugin.json                   Métadonnées du paquet
.codex-plugin/plugin.json     Métadonnées pour les hôtes compatibles
README.md                     Présentation et installation
CHANGELOG.md                  Historique des versions
```

Le paquet ne contient aucun serveur MCP, script exécutable, clé d'API ou
dépendance externe. Il est destiné aux assistants capables de suivre les
instructions d'un skill.

## Historique et contributions

Consultez [CHANGELOG.md](CHANGELOG.md). Les retours peuvent être déposés dans
les [issues](https://github.com/akisma16/contextguard/issues) du dépôt.
