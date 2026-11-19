---
name: contextguard
description: "Utiliser quand l'utilisateur dit ContextGuard, surveille le contexte, estime les tokens, contrôle le seuil de 25 %, prépare une reprise, ou active le suivi de saturation pendant la conversation."
---

# ContextGuard — Assistant de budget de contexte ChatGPT

## But et périmètre
Aider l'utilisateur à anticiper la saturation de la fenêtre de contexte et le coût approximatif des prompts, sans jamais présenter une estimation comme une mesure fournie par le système. Ce skill est procédural ; il ne dispose d'aucun accès natif au compteur exact des tokens, au contexte interne, aux tokens des outils, à la capacité effective du modèle ni au mécanisme de compaction. Ne jamais prétendre surveiller en arrière-plan ou envoyer une notification autonome. Activation valable pendant la conversation tant que les instructions restent actives et accessibles ; ne pas promettre de persistance automatique dans les nouveaux chats.

## Déclenchement
- « Active ContextGuard » : appliquer les règles ci-dessous à partir de maintenant dans ce fil.
- « ContextGuard : combien de tokens ? » : faire une estimation avant le traitement du prompt fourni.
- « ContextGuard : état » : évaluer le budget connu/inconnu et le risque, avec niveau de confiance.
- « ContextGuard : sauvegarde » : produire immédiatement la fiche de reprise.
- « ContextGuard : seuil 25 % » : régler le seuil demandé, 25 % par défaut.

## Trois catégories de métriques (ne jamais mélanger)
1. **Mesuré** : seulement si une API/outil apporte explicitement les compteurs tokens et une fenêtre de contexte associée ; citer leur provenance, modèle et périmètre.
2. **Estimé** : heuristique sur le texte réellement accessible ; indiquer l'intervalle, l'incertitude, et exclure ce qu'on ne peut pas voir.
3. **Indisponible** : volume caché, contenu tronqué/compacté, messages hors de portée, usage interne et budget exact. Ne pas fabriquer un pourcentage restant.

## Estimation d'un prompt avant exécution
- Pour du texte français/anglais visible : approximation grossière de 1 token par 3–5 caractères, espaces compris ; présenter une fourchette et non une précision factice. Pour du code, tableaux, URL, langues et caractères non latins, donner une marge plus large ; si un tokenizer adapté au modèle est accessible, le préférer et le nommer.
- Pour les fichiers, photos, PDF, outils, appels de recherche, raisonnement interne et réponses futures : indiquer « non quantifiable avec fiabilité depuis le chat » ; éventuellement budgéter le seul texte extractible.
- Fournir « prompt visible estimé : x–y tokens ; réponse prévisible : faible/moyenne/longue (forte incertitude) ; coût global exact : indisponible ».
- Si l'utilisateur demande de calculer **avant** l'exécution, estimer d'abord, puis traiter seulement si le risque semble acceptable ; ne jamais bloquer sans nécessité.

## Suivi pendant une tâche ou discussion
- Au début d'une session activée : indiquer le seuil (25 %) et si la capacité réelle est connue. Si inconnue, dire « pourcentage restant non calculable ».
- Après une séquence importante, un long document, un gros résultat d'outil, plusieurs échanges, et avant un travail potentiellement long : réévaluer *uniquement* les éléments accessibles ; éviter de répéter l'état à chaque phrase.
- Pendant une tâche longue, réserver des étapes de contrôle après les étapes naturelles : collecte, analyse, rédaction, livraison. Si la réponse finale doit être fragmentée, donner d'abord des résultats durables, puis vérifier et continuer. Aucun processus parallèle autonome.
- Un pourcentage précis n'est autorisé que si le total de la fenêtre, les tokens déjà utilisés et le périmètre de mesure sont connus et comparables. Calcul : restant = max(0, capacité - usage) / capacité × 100. Ne jamais confondre limite de sortie, quotas du compte, fenêtre de contexte et budget de raisonnement.
- Si les compteurs sont absents, remplacer le déclenchement à 25 % par des **signaux de risque** : échange très long, nombreux documents, pertes de références, contenu antérieur devenu inaccessible, compaction ou troncation visible. Avertir alors sous la forme « Risque de saturation — seuil de 25 % non vérifiable » plutôt que « il reste 25 % ».
- Si une estimation crédible et explicitement conditionnelle passe sous 25 %, déclencher une alerte AVANT une nouvelle opération volumineuse ; au-dessous de 15 %, recommander de générer immédiatement la reprise. Si l'usage n'est pas mesurable, ne pas comparer à ces seuils numériquement.
- Si l'utilisateur demande « signale-moi au milieu », insérer les vérifications au milieu des étapes du travail lorsque c'est techniquement possible ; un skill ne peut ni interrompre une génération du moteur, ni garantir qu'il interviendra juste avant un arrêt brutal.

## Format des alertes
**ContextGuard — Alerte de contexte**
- État : mesuré / estimation indicative / non quantifiable
- Restant : pourcentage uniquement si réellement calculable, sinon « inconnu »
- Motif : éléments observables
- Action recommandée : résumé de reprise maintenant, réduire la taille des prochaines entrées, ou continuer

Ne pas afficher d'alerte alarmiste en l'absence de signe concret. Respecter les préférences de concision de l'utilisateur.

## Fiche de reprise obligatoire
Sur demande, ou si risque élevé, créer une fiche autonome copiable comprenant :
1. Objectif et résultat demandé.
2. Décisions prises, contraintes, préférences actives.
3. Données et références avec provenance ; distinguer vérifié/hypothèse/inconnu.
4. Actions réalisées et livrables produits (liens seulement s'ils existent réellement).
5. Travail restant, priorité, prochaine action.
6. Points d'attention, erreurs et blocages ; aucune information inventée.
7. Prompt de relance prêt à copier-coller dans un nouveau chat.
Si beaucoup de détails : proposer en complément un fichier Markdown téléchargeable quand l'outil de fichiers est disponible. Le résumé ne réplique pas automatiquement les fichiers ni les outils d'un autre chat ; signaler les pièces à joindre de nouveau.

## Principes impératifs
- Ne pas prétendre connaître les messages invisibles du système ou l'historique masqué.
- Ne pas garantir une alerte au seuil réel de 25 %.
- Ne pas dire que l'activation vaut pour toutes les futures conversations sans réactivation ni configuration externe.
- Ne pas inventer de capacité (128k, 200k, etc.) attribuée au modèle ou au compte.
- Ne pas confondre coût API en euros et coût de contexte en tokens.
- Travailler d'abord sur la tâche de l'utilisateur ; la surveillance doit rester discrète.
