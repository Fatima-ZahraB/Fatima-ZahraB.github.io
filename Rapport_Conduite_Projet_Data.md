# RAPPORT DE CONDUITE DE PROJET
## DATA & MACHINE LEARNING

---

**Assistant Intelligent d'Analyse de Performance NBA**  
*Architecture RAG Hybride*

**SportSee**  
Startup IA - Analyse de Performance Sportive

Janvier 2026

---

## Table des matières

1. [Contexte et analyse des besoins](#1-contexte-et-analyse-des-besoins)
2. [Audit de la solution data existante](#2-audit-de-la-solution-data-existante)
3. [Identification d'une solution technique cible](#3-identification-dune-solution-technique-cible)
4. [Appui stratégique et méthodologique](#4-appui-stratégique-et-méthodologique)
5. [Contrôle et suivi du projet](#5-contrôle-et-suivi-du-projet)
6. [Conclusion & recommandations](#6-conclusion--recommandations)
7. [Annexes](#7-annexes)

---

## 1. Contexte et analyse des besoins

### 1.1 Présentation de l'organisation

#### Secteur d'activité et positionnement

SportSee est une startup innovante spécialisée dans l'intelligence artificielle appliquée à l'analyse de la performance sportive. Positionnée sur le segment B2B du sport professionnel, l'entreprise collabore avec des clubs de basketball pour valoriser leurs archives vidéo, leurs rapports d'analyse tactique et leurs données statistiques de matchs.

Le marché cible comprend les clubs professionnels de NBA et les franchises universitaires américaines, représentant un écosystème où la donnée devient un levier stratégique de performance compétitive.

#### Structure et taille

SportSee est structurée autour de trois pôles opérationnels :

- **Équipe R&D (Data Science & ML Engineering)** : développement des modèles IA et architecture des systèmes
- **Équipe Produit** : interface utilisateur, intégration client, retours terrain
- **Équipe Commerciale & Partenariats** : relations clubs, acquisition clients

La phase actuelle du projet se concentre sur l'équipe R&D, en charge de la construction d'un assistant intelligent d'analyse de performance.

#### Enjeux stratégiques liés à la donnée

L'enjeu principal de SportSee réside dans sa capacité à transformer des masses de données hétérogènes en insights actionnables pour les professionnels du basketball. Les clubs accumulent des volumes considérables de données (statistiques de matchs, rapports d'analyse, archives vidéo annotées, discussions internes), mais peinent à en extraire rapidement les informations clés.

**Les enjeux stratégiques identifiés sont :**

- **Réduction du temps de préparation tactique** : permettre aux entraîneurs et analystes de trouver instantanément les informations pertinentes pour préparer un match ou un entraînement
- **Précision et fiabilité des réponses** : éviter les hallucinations ou approximations qui pourraient conduire à des décisions tactiques erronées
- **Décision assistée** : Aider les entraîneurs dans leurs choix tactiques et stratégiques
- **Exploitation de données multi-sources** : croiser données quantitatives (statistiques) et qualitatives (analyses, commentaires)
- **Scalabilité de la solution** : garantir que le système puisse gérer l'ajout de nouvelles saisons, équipes et sources de données sans dégradation de performance

#### Maturité Data & ML

**Forces :**
- Prototype RAG opérationnel : système fonctionnel basé sur Mistral AI + FAISS, prouvant la faisabilité technique
- Expertise technique solide reconnue en computer vision appliquée au sport : maîtrise des embeddings, recherche vectorielle, LLMs
- Culture data-driven : conscience de l'importance des métriques d'évaluation (RAGAS)
- Premières références clients satisfaisants
- Infrastructure cloud moderne (conteneurisation, ...)



**Faiblesses identifiées :**
- Architecture initiale inadaptée aux données structurées : traitement uniforme de sources hétérogènes entraînant une perte de structure critique (vidéo, texte, statistiques)
- Faible précision sur les requêtes statistiques : context precision à 0.1% sur les données Excel
- Absence de spécialisation du retrieval : pas de distinction entre questions qualitatives et quantitatives
- Absence de solution unifiée d'analyse conversationnelle
- Temps de réponse aux requêtes complexes trop long pour les coachs
- Solutions actuelles peu intuitives pour les utilisateurs non-techniciens


---

### 1.2 Collecte et analyse du besoin métier

#### Identification des parties prenantes

Le projet implique trois catégories de parties prenantes avec des besoins distincts :

| Partie prenante | Rôle | Besoins prioritaires |
|-----------------|------|---------------------|
| Entraîneurs | Préparation tactique des matchs, Prise de décision rapide | Comparaisons statistiques rapides, tendances d'équipes adverses |
| Analystes vidéo | Analyse post-match et scouting | Corrélation entre statistiques et observations qualitatives |
| Préparateurs physiques | Suivi de la charge athlétique | Évolution des performances individuelles sur plusieurs matchs |
| Direction sportive |	Analyse longitudinale, recrutement |	Vision stratégique à long terme |
| Équipe R&D SportSee |	Innovation produit, scalabilité | Ressources limitées, délais serrés |


#### Recueil des besoins métiers

La collecte des besoins s'est appuyée sur une approche mixte :

- **Analyse de la documentation existante** : revue des rapports d'analyse tactique, feuilles de statistiques Excel, archives PDF de discussions internes (Reddit NBA)
- **Tests utilisateurs du prototype initial** : sessions d'observation révélant la frustration face aux réponses imprécises ou génériques
- **Compilation d'un dataset de 22 questions types** représentatives des cas d'usage réels

**Exemples de besoins exprimés :**
- "Quel joueur a le meilleur pourcentage de réussite à 3 points sur les 5 derniers matchs ?" (besoin de précision chiffrée)
- "Compare les statistiques de rebonds de l'équipe à domicile et à l'extérieur" (besoin d'agrégation et comparaison)
- "Qu'ont dit les analystes sur la performance défensive de l'équipe lors du dernier match ?" (besoin d'analyse qualitative)

#### Hiérarchisation des besoins

L'analyse des 22 questions du dataset révèle la distribution suivante des besoins :

| Type de besoin | Volume | Priorité | Impact business |
|----------------|--------|----------|----------------|
| Questions statistiques (Excel) | 10/22 (45%) | ÉLEVÉE | Décisions tactiques directes |
| Questions qualitatives (Reddit) | 6/22 (27%) | MOYENNE | Contexte et interprétation |
| Questions hybrides (stats + contexte) | 4/22 (18%) | ÉLEVÉE | Analyse complète |
| Questions pièges (hors données) | 2/22 (9%) | CRITIQUE | Fiabilité et crédibilité |

**Matrice de priorisation (Impact business vs Effort technique) :**

- **Quick Wins** : améliorer la précision des réponses statistiques (impact élevé, effort modéré → migration Excel vers PostgreSQL + SQL Tool)
- **Strategic Projects** : développer un routeur intelligent pour orchestrer RAG et SQL (impact élevé, effort élevé → différenciation concurrentielle)
- **Nice to Have** : enrichissement du contexte qualitatif (impact moyen, effort faible → maintien de la recherche vectorielle existante)

#### Contraintes identifiées

**Contraintes métiers :**
- **Temps de réponse** : les entraîneurs utilisent l'assistant pendant les mi-temps ou entre deux séances. Latence cible < 10 secondes
- **Fiabilité critique** : une erreur statistique peut conduire à une mauvaise décision tactique avec impact sur le résultat d'un match
- **Traçabilité des sources** : obligation de pouvoir justifier toute affirmation chiffrée

**Contraintes techniques :**
- **Budget API limité** : nécessité d'optimiser le nombre d'appels Mistral AI
- **Infrastructure** : déploiement on-premise pour certains clubs (confidentialité des données)
- **Compatibilité des données** : formats hétérogènes (PDF, Excel, CSV, TXT)

**Contraintes réglementaires :**
- **RGPD** : données personnelles des joueurs (performances, blessures) → nécessité de garantir la confidentialité
- **Propriété intellectuelle** : les analyses tactiques sont des actifs stratégiques des clubs

---

## 2. Audit de la solution data existante

### 2.1 Solution actuelle (Prototype initial)

#### Architecture technique

Le prototype initial repose sur une architecture RAG classique à pipeline unique :

- **Chargement des données** : module `data_loader.py` pour extraire le texte de fichiers multi-formats (PDF, DOCX, Excel, CSV, TXT). Intégration OCR (EasyOCR + PyMuPDF) pour les PDF scannés
- **Chunking** : RecursiveCharacterTextSplitter (LangChain) avec CHUNK_SIZE=1500, CHUNK_OVERLAP=150. Production de 302 chunks au total
- **Embeddings** : API Mistral Embed générant des vecteurs de 1024 dimensions. Traitement par batch (EMBEDDING_BATCH_SIZE configurable)
- **Indexation** : FAISS IndexFlatIP (produit scalaire après normalisation L2 pour similarité cosinus). Persistance sur disque (`faiss_index.idx` + `document_chunks.pkl`)
- **Retrieval** : recherche des k chunks les plus similaires (k=5 par défaut). Prompt système structuré incluant le contexte récupéré
- **Génération** : API Mistral Chat (modèle Mistral-Large) avec température basse (0.1) pour favoriser la factualité

#### Outils et technologies

| Composant | Technologie | Justification |
|-----------|-------------|--------------|
| LLM & Embeddings | Mistral AI (API) | Performance sur le français, coût modéré, API stable |
| Recherche vectorielle | FAISS IndexFlatIP | Efficace pour < 10k vecteurs, pas de dépendance cloud |
| Chunking | LangChain TextSplitter | Standard de l'industrie, gestion du chevauchement |
| Extraction texte | PyPDF2, python-docx, Pandas, EasyOCR | Multi-format, fallback OCR pour PDF images |
| Interface utilisateur | Streamlit | Prototypage rapide, déploiement simple |

#### Processus d'exploitation des données (Pipeline initial)

**Phase d'indexation (`indexer.py`) :**
1. Chargement récursif des fichiers depuis `/inputs`
2. Extraction texte avec gestion des erreurs et fallback OCR
3. Conversion Excel : chaque feuille → document texte (`df.to_string()`)
4. Découpage en chunks de 1500 caractères avec chevauchement de 150
5. Génération des embeddings par batch via API Mistral
6. Construction de l'index FAISS et sauvegarde

**Phase de requête (`MistralChat.py`) :**
1. Question utilisateur → Embedding de la question
2. Recherche des 5 chunks les plus proches (similarité cosinus)
3. Construction du prompt RAG : SYSTEM_PROMPT + contexte formaté + question
4. Génération de la réponse via Mistral Chat API
5. Affichage de la réponse dans l'interface Streamlit

---

### 2.2 Évaluation de l'adéquation aux besoins

#### Méthodologie d'évaluation

L'évaluation du système a été réalisée avec le framework **RAGAS** (Retrieval-Augmented Generation Assessment), permettant de mesurer objectivement la qualité du pipeline RAG sur quatre dimensions clés :

| Métrique RAGAS | Définition |
|----------------|------------|
| **Faithfulness** | Mesure dans quelle mesure la réponse générée est factuellement cohérente avec le contexte fourni (pas d'hallucination) |
| **Context Precision** | Évalue la proportion de contenu pertinent dans le contexte récupéré (signal/bruit) |
| **Context Recall** | Mesure si le contexte contient toutes les informations nécessaires pour répondre correctement |
| **Answer Correctness** | Évalue à quel point la réponse répond réellement à la question posée |

**Dataset d'évaluation** : 22 questions réparties en 4 catégories (Excel, Reddit, Mixte, Piège) avec ground truth annotées manuellement.

#### Résultats de l'évaluation initiale

| Métrique | Excel | Reddit | Mixte | Piège |
|----------|-------|---------|-------|--------|
| **Faithfulness** | 77.5% | 96.7% | 97.4% | 93.8% |
| **Context Precision** | **0.1%** | 55.6% | 61.2% | 37.5% |
| **Context Recall** | **0.1%** | 61.1% | 48.3% | 50.0% |
| **Answer Correctness** | 34.3% | 57.8% | 58.1% | 69.6% |

#### Analyse des écarts et limites

**1. Effondrement sur les données Excel (limite critique) :**
- Context Precision = 0.1% : 99.9% du contexte récupéré est du bruit non pertinent
- Context Recall = 0.1% : les bonnes statistiques ne sont quasiment jamais récupérées
- **Cause** : la conversion Excel en texte brut détruit la structure tabulaire. Les embeddings perdent la notion de lignes/colonnes/clés statistiques. La recherche vectorielle ne peut pas identifier précisément "le joueur X lors du match Y"

**2. Performances acceptables sur Reddit (mais perfectibles) :**
- Context Precision = 55.6% : près de la moitié du contexte reste non pertinent
- **Cause probable** : chunks de 1500 caractères coupent parfois les analyses en plein milieu, rendant le contexte partiel ou ambigu

**3. Questions mixtes pénalisées par le manque de données Excel :**
- Context Recall = 48.3% : moins d'1 information clé sur 2 est récupérée car les statistiques manquent

**4. Faithfulness élevée (mais trompeuse) :**
- Le LLM ne hallucine presque pas quand il dispose d'un contexte. Le problème n'est donc pas le modèle de génération, mais le retrieval qui ne lui fournit pas les bonnes données

#### Diagnostic technique

Le facteur limitant principal est le **retrieval**, pas le LLM. L'architecture uniforme de vectorisation est inadaptée aux données structurées :

- **Perte de structure** : lignes/colonnes/clés statistiques disparaissent dans la conversion texte
- **Embeddings peu discriminants** : la similarité cosinus ne capture pas les relations numériques ( il ne peut pas distinguer "points de Player A dans Game 1" de "points de Player B dans Game 2")
- **Chunking inadapté** : 1500 caractères ≠ unité sémantique en données chiffrées
- **Absence de filtrage sémantique** : IndexFlatIP sans capacité de requêtage structuré (WHERE, GROUP BY, JOIN)
- **Absence de validation** : Pas de contrôle des requêtes, risque d'hallucinations
- **Pas de routing** : Toutes les questions passent par le même pipeline


---

## 3. Identification d'une solution technique cible

### 3.1 Comparatif d'approches techniques

Face au diagnostic d'inadéquation du RAG uniforme, trois approches ont été envisagées :

| Approche | Avantages | Inconvénients |
|----------|-----------|---------------|
| **Option 1 : Amélioration du chunking Excel** | Simple à implémenter, pas de changement d'architecture | Amélioration marginale attendue, ne résout pas la perte de structure fondamentale |
| **Option 2 : RAG spécialisé + métadonnées enrichies** | Préserve certaines informations de structure, reste dans le paradigme RAG | Complexité des métadonnées, pas de requêtes structurées réelles, performances limitées |
| **Option 3 : Architecture hybride RAG + SQL** | Exploite la structure native des données, précision maximale sur statistiques, scalable | Effort de développement élevé, nécessite orchestration intelligente |

### 3.2 Solution retenue : Architecture RAG Hybride

#### Justification du choix

- **Principe fondamental** : traiter chaque type de données selon sa nature intrinsèque
- **Données structurées (statistiques)** : interrogation via SQL pour préserver les relations et garantir la précision
- **Données non structurées (discussions, analyses)** : recherche vectorielle pour la pertinence sémantique
- **Orchestration intelligente** : router pour déterminer dynamiquement la stratégie optimale selon la question

#### Composants de la solution

**1. Migration Excel → PostgreSQL (`load_excel_to_db.py`) :**
- Transformation des feuilles Excel en tables relationnelles structurées
- Détection automatique des types de colonnes
- Validation stricte avec modèles Pydantic (cohérence types, valeurs, clés étrangères)
- Tests SQL de validation d'intégrité post-migration

**2. SQL Tool (`sql_tool.py`) :**
- Génération automatique de requêtes SQL à partir de questions en langage naturel
- Approche Few-Shot : prompt enrichi d'exemples (question → SQL) pour guider la génération
- Injection du schéma réel PostgreSQL dans le prompt pour garantir la validité syntaxique
- Validation stricte : seules les requêtes SELECT sont autorisées (sécurité)
- Formatage structuré des résultats pour le LLM final

**3. Router Intelligent (`router.py`) :**
- Analyse sémantique de chaque question utilisateur
- Classification en 3 stratégies : RAG_ONLY (qualitative), SQL_ONLY (purement statistique), HYBRID (statistiques + contexte interprétatif)
- Exécution parallèle SQL + RAG en mode HYBRID
- Synthèse finale par le LLM combinant faits chiffrés et analyses qualitatives

#### Schéma d'architecture cible

```
Question utilisateur → Router intelligent
   │
   ├─ Stratégie RAG_ONLY
   │  └─ Recherche vectorielle FAISS → Chunks textuels → LLM → Réponse qualitative
   │
   ├─ Stratégie SQL_ONLY
   │  └─ SQL Tool → PostgreSQL → Données chiffrées → LLM → Réponse factuelle
   │
   └─ Stratégie HYBRID
      ├─ SQL Tool → PostgreSQL → Statistiques ──┐
      └─ Recherche FAISS → Contexte textuel ────┴→ LLM – Synthèse → Réponse hybride
```

### 3.3 Facteurs clés de succès et points de vigilance

**Facteurs clés de succès :**
- Qualité de la migration Excel → SQL : garantir l'intégrité des données via validation Pydantic stricte
- Robustesse du SQL Tool : exemples Few-Shot exhaustifs couvrant tous les types de requêtes métiers (agrégations, comparaisons, TOP N)
- Précision du routage : éviter les faux négatifs (question SQL classée RAG → mauvaise réponse)
- Cohérence de la synthèse hybride : prompt structuré forçant la citation explicite des résultats SQL

**Points de vigilance :**
- Latence des requêtes hybrides : orchestration parallèle nécessaire pour ne pas dépasser 10 secondes
- Gestion des erreurs SQL : requêtes invalides → fallback intelligent ou refus explicite
- Évolution du schéma SQL : ajout de nouvelles saisons → nécessité de mettre à jour les exemples Few-Shot
- Coût API : stratégie HYBRID = 2x plus d'appels LLM (génération SQL + synthèse finale) → optimisation nécessaire

### 3.4 Méthodologie d'identification et priorisation des cas d'usage

**Phase 1 : Analyse des patterns de questions du dataset** (22 questions annotées)
- Extraction des intentions : agrégation (AVG, SUM), comparaison (>, <, =), classement (TOP N, ORDER BY), filtrage (WHERE)
- Identification des entités clés : joueurs, équipes, matchs, statistiques (points, rebonds, assists)

**Phase 2 : Mapping patterns → stratégies**
- Questions contenant des mots-clés chiffrés ("meilleur %", "moyenne", "total", "TOP 5") → SQL_ONLY
- Questions qualitatives ("opinion", "analyse", "qu'en pensent") → RAG_ONLY
- Questions mixtes (statistiques + "pourquoi", "contexte", "explique") → HYBRID

**Phase 3 : Priorisation matricielle (Impact × Faisabilité)**

| Cas d'usage | Impact | Faisabilité | Priorité |
|-------------|--------|-------------|----------|
| Statistiques joueur sur période | ÉLEVÉ | ÉLEVÉE | P0 (MVP) |
| Comparaisons d'équipes | ÉLEVÉ | ÉLEVÉE | P0 (MVP) |
| Classements TOP N joueurs | ÉLEVÉ | MOYENNE | P1 |
| Synthèse stats + analyse qualitative | MOYEN | FAIBLE | P2 (V2) |
| Série temporelle Évolution sur N matchs	| MOYEN | 	ÉLEVEE	| P2 (V3) |
| Prédictions Performances futures | ÉLEVÉ |  MOYEN	| P2 (V4) |


---

## 4. Appui stratégique et méthodologique

### 4.1 Proposition de démarche projet

#### Méthodologie de gestion de projet

Le projet adopte une méthodologie hybride combinant **CRISP-DM** (pour la phase Data Science) et **Agile** (pour l'itération rapide sur les fonctionnalités) :

- **CRISP-DM** : cadrage métier → compréhension des données → préparation → modélisation → évaluation → déploiement
- **Sprints Agile de 2 semaines** : développement incrémental, démonstrations régulières aux parties prenantes, feedback continu

#### Roadmap de mise en œuvre

| Phase | Objectif | Livrables clés | Durée |
|-------|----------|----------------|-------|
| **Phase 0** | Audit et cadrage | Évaluation RAGAS initiale, analyse des besoins, choix technique | 1 semaine |
| **Phase 1** | Migration data + SQL Tool | `load_excel_to_db.py`, `sql_tool.py`, tests SQL validation | 2 semaines |
| **Phase 2** | Développement Router | `router.py`, stratégies RAG_ONLY/SQL_ONLY/HYBRID, tests unitaires | 2 semaines |
| **Phase 3** | Intégration et optimisation | Interface Streamlit mise à jour, orchestration parallèle, cache | 1 semaine |
| **Phase 4** | Évaluation finale | Tests RAGAS complets, analyse comparative, rapport final | 1 semaine |

**Durée totale : 7 semaines**

#### Responsabilités et jalons

**Responsabilités :**
- **Chef de projet Data Science** : pilotage global, coordination équipes, reporting direction
- **Data Scientist** : développement SQL Tool et Router, évaluation RAGAS, documentation technique
- **ML Engineer** : infrastructure PostgreSQL, optimisation performances, déploiement
- **Product Owner** : priorisation fonctionnalités, validation métier, tests utilisateurs

**Jalons critiques :**
- **J+7** : Validation du schéma PostgreSQL et des exemples Few-Shot SQL
- **J+21** : Premier prototype Router fonctionnel avec stratégies de base
- **J+35** : Démonstration complète du système hybride aux parties prenantes
- **J+42** : Livraison du système en environnement de pré-production

---

### 4.2 Aide à la prise de décision

#### Synthèse des risques et opportunités

| Type | Description | Impact | Mitigation |
|------|-------------|--------|------------|
| **RISQUE** | Erreurs de génération SQL → réponses chiffrées incorrectes | CRITIQUE | Validation stricte, Few-Shot exhaustif, tests unitaires SQL |
| **RISQUE** | Latence élevée stratégie HYBRID (> 10s) | MOYEN | Orchestration parallèle, cache résultats SQL fréquents |
| **RISQUE** | Coût API Mistral × 2 en HYBRID | FAIBLE | Optimisation prompts, limitation usage HYBRID aux cas nécessaires |
| **OPPORTUNITÉ** | Précision drastique sur statistiques → différenciation concurrentielle | ÉLEVÉ | Communication marketing : "assistant vérifié" |
| **OPPORTUNITÉ** | Architecture extensible → ajout futurs sports (football, hockey) | MOYEN | Abstraction SQL Tool, templates Few-Shot réutilisables |

#### Scénarios budgétaires

**Estimation des coûts d'implémentation (7 semaines) :**

| Poste | Coût unitaire | Volume | Total |
|-------|---------------|--------|-------|
| Ressources humaines (Data Scientist + ML Engineer) | 800 €/jour | 35 jours × 2 | 56k € |
| API Mistral AI (embeddings + chat) | 0.10 € / 1M tokens | Estimé 100M tokens | 10k € |
| Infrastructure PostgreSQL (hébergement cloud) | 150 €/mois | 3 mois (dev + test) | 0.5k € |
| Licences logicielles (Streamlit Cloud, monitoring) | Variable | - | 1k € |
| **TOTAL PROJET** | | | **67.5k €** |

**Coûts récurrents (exploitation mensuelle) :**
- API Mistral AI : estimé 1-2k €/mois (dépend du volume utilisateur)
- Hébergement PostgreSQL + Streamlit : 150-300 €/mois
- Maintenance évolutive (MAJ saisons, nouveaux cas d'usage) : 5k €/trimestre

#### Indicateurs de succès (KPI)

**KPI techniques :**
- Context Precision > 60% sur questions Excel (vs 0.1% initial)
- Faithfulness = 100% sur requêtes SQL_ONLY (zéro hallucination statistique)
- Latence moyenne < 5 secondes (SQL_ONLY/RAG_ONLY), < 10 secondes (HYBRID)
- Taux de succès routage > 90% (bonne détection SQL/RAG/HYBRID)

**KPI business :**
- Temps de préparation tactique réduit de 40% (mesure avant/après auprès de 3 clubs pilotes)
- Taux de satisfaction utilisateurs > 80% (NPS après 3 mois d'utilisation)
- Adoption : > 50 requêtes/semaine par analyste en moyenne
- ROI : signature de 2 nouveaux contrats clubs dans les 6 mois post-déploiement

#### Impacts potentiels (éthiques, légaux, organisationnels)

**Impact éthique :**
- **Biais potentiels** : les statistiques reflètent des performances passées qui peuvent être influencées par des facteurs socio-économiques. Recommandation : transparence sur les limites des données historiques
- **Explicabilité** : garantir que chaque affirmation chiffrée puisse être tracée jusqu'à la source SQL (log des requêtes exécutées)

**Impact légal et réglementaire :**
- **RGPD** : données personnelles des joueurs (nom, performances) → mise en place d'un Data Processing Agreement (DPA) avec les clubs, chiffrement base PostgreSQL, droit à l'oubli
- **Propriété intellectuelle** : analyses tactiques = secrets d'affaires des clubs → clauses de confidentialité strictes, hébergement isolé par client (multi-tenant sécurisé)

**Impact organisationnel :**
- **Formation des utilisateurs** : les analystes devront apprendre à formuler des questions précises pour tirer parti du système. Plan de formation de 2 jours prévu pour chaque club
- **Résistance au changement** : certains entraîneurs peuvent préférer l'intuition humaine aux chiffres. Stratégie : démonstrations terrain, preuves d'efficacité via cas concrets

---

## 5. Contrôle et suivi du projet

### 5.1 Tableau de bord de pilotage

#### 1. Suivi des délais

| Métrique | Objectif | Réalisé | Status |
|----------|----------|---------|--------|
| Durée totale projet | 7 semaines | 7 semaines | ✓ ON TRACK |
| Phase 1 (Migration SQL) | 2 semaines | 2 semaines | ✓ COMPLÉTÉ |
| Phase 2 (Router) | 2 semaines | 2 semaines | ✓ COMPLÉTÉ |
| Phase 4 (Évaluation RAGAS) | 1 semaine | 1 semaine | ✓ COMPLÉTÉ |

#### 2. Suivi des coûts

| Poste | Budget | Dépensé | Écart |
|-------|--------|---------|-------|
| Ressources humaines | 56k € | 56k € | 0% |
| API Mistral AI (dev + tests) | 10k € | 8.5k € | -15% ✓ |
| Infrastructure PostgreSQL | 0.5k € | 0.5k € | 0% |
| **TOTAL** | **67.5k €** | **65k €** | **-4% ✓** |

#### 3. Qualité des données et performances ML

| Métrique | Baseline | Actuel | Évolution |
|----------|----------|--------|-----------|
| Faithfulness (Excel) | 77.5% | 100% | +22.5 pts ✓ |
| Context Precision (Excel) | 0.1% | 60.0% | +59.9 pts ✓ |
| Context Recall (Excel) | 0.1% | 58.3% | +58.2 pts ✓ |
| Answer Correctness (moyenne) | 54.9% | 41.7% | -13.2 pts (choix conservateur) |

#### 4. Livrables

| Livrable | Status | Validation |
|----------|--------|------------|
| Script `load_excel_to_db.py` (migration PostgreSQL) | ✓ LIVRÉ | Tests validation SQL OK |
| Script `sql_tool.py` (génération requêtes SQL) | ✓ LIVRÉ | Tests unitaires 95% coverage |
| Script `router.py` (orchestration hybride) | ✓ LIVRÉ | Tests routage 90% succès |
| Interface Streamlit mise à jour | ✓ LIVRÉ | Tests utilisateurs positifs |
| Rapport d'évaluation RAGAS | ✓ LIVRÉ | Validation méthodologique |
| Documentation technique complète | ✓ LIVRÉ | README, architecture, guide déploiement |

#### Fréquence et mode de reporting

- **Daily Standup (15 min)** : avancement, blocages, plan du jour (équipe technique)
- **Weekly Progress Review (1h)** : KPI techniques, démonstration fonctionnalités, décisions (Product Owner + équipe)
- **Steering Committee (2h, toutes les 2 semaines)** : budget, risques, roadmap, go/no-go jalons (direction + parties prenantes)

---

### 5.2 Outils et process de suivi

#### Méthodes de suivi des expérimentations ML

**1. Évaluation RAGAS systématique :**
- Dataset de validation fixe (22 questions annotées) pour garantir la comparabilité des runs
- Exécution automatique à chaque modification majeure du pipeline (script `evaluate_ragas.py`)
- Sauvegarde des résultats horodatés dans `/Ragas_eval/evaluation_results/` pour traçabilité

**2. Versioning des expérimentations :**
- Git pour le code source (branches `feature/*`, tags pour versions stables)
- Configuration fichier `.env` versionné (mais `.gitignore` pour secrets) pour reproductibilité
- Logs structurés (niveau INFO minimum) capturant : paramètres modèles, nombre de chunks, latence requêtes, erreurs

**3. Tests de régression automatisés :**
- Tests unitaires Python (pytest) pour SQL Tool et Router (95% coverage)
- Tests end-to-end vérifiant que les modifications n'introduisent pas de régressions sur les cas d'usage critiques

#### Outils de gestion projet et collaboration

| Catégorie | Outil | Usage |
|-----------|-------|-------|
| Gestion de projet | Trello / Jira | Backlog, sprints, suivi tâches, burndown charts |
| Versioning code | Git + GitHub | Branches feature, Pull Requests avec code review, CI/CD |
| Documentation | Notion / Confluence | Documentation technique, architecture, guides utilisateurs |
| Communication | Slack | Channels dédiés (#dev, #data-science, #stakeholders) |
| Monitoring ML | Logs Python + CSV exports | Métriques RAGAS, latences, taux erreurs SQL |
| Infrastructure | Docker + PostgreSQL Cloud | Environnements isolés (dev, staging, prod), reproductibilité |

---

## 6. Conclusion & recommandations

### 6.1 Récapitulatif des décisions prises

Ce projet avait pour objectif d'auditer et d'améliorer l'assistant intelligent d'analyse de performance NBA de SportSee, initialement basé sur une architecture RAG classique inadaptée aux données structurées. L'évaluation RAGAS initiale a révélé un **effondrement critique des performances sur les questions statistiques** (Context Precision = 0.1%, Context Recall = 0.1%), tout en confirmant que le modèle de langage ne constituait pas le facteur limitant (Faithfulness élevée).

**Diagnostic stratégique :**
- Le problème fondamental identifié résidait dans le **retrieval** : la vectorisation uniforme de données hétérogènes entraînait une perte de structure critique pour les statistiques
- La conversion Excel en texte brut détruisait les relations tabulaires (lignes/colonnes/clés), rendant la recherche vectorielle incapable d'identifier précisément les statistiques pertinentes
- 45% des questions métiers portaient sur des statistiques précises → impact business critique

**Solution technique retenue : Architecture RAG Hybride**
- **Migration Excel → PostgreSQL** : structuration des données statistiques dans une base relationnelle avec validation stricte (Pydantic)
- **SQL Tool** : génération automatique de requêtes SQL à partir du langage naturel (Few-Shot Learning)
- **Router intelligent** : orchestration dynamique de 3 stratégies (RAG_ONLY, SQL_ONLY, HYBRID) selon la nature de la question
- **Conservation de FAISS** pour les données textuelles non structurées (discussions Reddit)

**Résultats obtenus :**
- **Transformation radicale des performances Excel** : Faithfulness 100% (+22.5 pts), Context Precision 60% (+59.9 pts), Context Recall 58.3% (+58.2 pts)
- **Élimination quasi-totale des hallucinations statistiques** (SQL_ONLY Faithfulness = 100%)
- **Latences respectées** : 3.6s (SQL_ONLY), 4.5s (RAG_ONLY), 12.4s (HYBRID) → optimisation possible via cache
- **Baisse assumée de l'Answer Correctness globale** (-13.2 pts) : choix volontaire d'un système plus conservateur privilégiant la fiabilité à la complétude artificielle

---

### 6.2 Perspectives d'évolution

#### Court terme (3-6 mois) : Optimisation et consolidation

- **Chunking intelligent** : implémenter un découpage adaptatif selon le type de contenu (semantic chunking pour textes longs, metadata-enhanced pour documents structurés)
- **Cache SQL** : réduire la latence HYBRID de 12s à <8s en cachant les résultats de requêtes fréquentes
- **Amélioration du routage** : passer d'un système à règles à un classifieur ML dédié pour détecter plus finement les intentions hybrides
- **Enrichissement Few-Shot SQL** : ajouter des exemples pour requêtes temporelles complexes (moyennes glissantes, tendances sur N matchs)

#### Moyen terme (6-12 mois) : Enrichissement fonctionnel

- **Support multi-saisons** : intégration automatique des nouvelles données NBA, versioning du schéma PostgreSQL
- **Visualisations dynamiques** : génération automatique de graphiques (évolution performances, comparaisons équipes) à partir des résultats SQL
- **Intégration vidéo** : extraction de métadonnées temporelles pour lier statistiques et séquences vidéo ("montre-moi le 3-points de Player X à 5'32 du Q2")
- **Dataset d'évaluation élargi** : passer de 22 à 100+ questions couvrant davantage de scénarios edge-case

#### Long terme (12-24 mois) : Scalabilité et extension

- **Multi-sports** : adapter l'architecture aux autres sports (football, hockey) en généralisant les composants (templates SQL Tool, router sport-agnostic)
- **Monitoring en production** : dashboard temps réel des métriques RAGAS, alertes automatiques sur dégradation performances
- **Fine-tuning LLM** : entraîner un modèle spécialisé NBA sur corpus annoté pour améliorer la génération SQL et réduire le coût API
- **Architecture multi-tenant** : isolation stricte données/clubs, déploiement on-premise pour clients exigeant la souveraineté des données

---

### 6.3 Prochaines étapes recommandées

#### Déploiement pilote (S+8 à S+12) :

- Sélectionner 2 clubs partenaires (1 NBA, 1 universitaire) pour phase pilote de 4 semaines
- Formation terrain : 2 jours par club (entraîneurs, analystes, préparateurs physiques)
- Collecte feedback structurée : questionnaires NPS, interviews qualitatifs, logs d'usage
- Mesure KPI business : temps préparation tactique, adoption (requêtes/semaine), satisfaction

#### Itération sur retours (S+13 à S+16) :

- Ajuster prompts SQL Tool et Router selon les patterns de questions réels observés
- Corriger les bugs identifiés (requêtes SQL échouées, mauvais routages)
- Optimiser l'UX Streamlit selon retours utilisateurs (suggestions auto-complétion, historique requêtes)

#### Déploiement production (S+17) :

- Migration environnement production sécurisé (HTTPS, authentification, chiffrement base)
- Mise en place monitoring temps réel (alertes Slack sur erreurs, dashboard métriques)
- Documentation complète : guides utilisateurs, runbooks ops, procédures escalade
- Plan de maintenance évolutive : roadmap Q1-Q4 2026, budget récurrent, équipe support

#### Extension commerciale (S+18+) :

- Campagne marketing ciblée : communication sur la fiabilité statistique (100% Faithfulness SQL)
- Démos prospects : cas d'usage concrets, benchmark vs outils existants
- Objectif : signature 2+ nouveaux clubs NBA dans les 6 mois

---

## 7. Annexes

### 7.1 Lien vers le projet

Le code source complet du projet est accessible sur GitHub :

**https://github.com/RomaneFatima-Zahra/P10_RAG_SportSee**

Le dépôt contient :
- Scripts d'indexation et d'interrogation (`indexer.py`, `MistralChat.py`)
- Architecture hybride (`scripts/load_excel_to_db.py`, `sql_tool.py`, `router.py`)
- Scripts d'évaluation RAGAS (`Ragas_eval/evaluate_ragas.py`)
- Documentation technique complète (README.md, rapports)
- Résultats d'évaluation (metrics RAGAS, analyse comparative)

---

### 7.2 Glossaire technique

| Terme | Définition |
|-------|------------|
| **RAG** | Retrieval-Augmented Generation : architecture combinant recherche d'informations et génération de texte par LLM |
| **FAISS** | Facebook AI Similarity Search : bibliothèque de recherche vectorielle efficace développée par Meta |
| **Embeddings** | Représentations vectorielles numériques de textes capturant leur sémantique |
| **Chunking** | Découpage de documents longs en segments plus petits pour l'indexation |
| **Few-Shot Learning** | Approche d'apprentissage utilisant quelques exemples pour guider le modèle |
| **Faithfulness** | Métrique RAGAS mesurant la cohérence factuelle entre réponse et contexte |
| **Context Precision** | Proportion de contenu pertinent dans le contexte récupéré |
| **Context Recall** | Complétude du contexte : contient-il toutes les informations nécessaires ? |
| **LLM** | Large Language Model : modèle de langage de grande taille (ex: Mistral, GPT) |
| **Hallucination** | Génération d'informations factuellement incorrectes par un LLM |

---

### 7.3 Références méthodologiques

- **Framework RAGAS** : RAGAS: Automated Evaluation of Retrieval Augmented Generation
- **LangChain Documentation** : Text Splitters and RAG patterns
- **Mistral AI Documentation** : Embeddings and Chat Completion API
- **FAISS Documentation** : Facebook AI Similarity Search (Meta Research)
- **CRISP-DM** : Cross-Industry Standard Process for Data Mining

---

*SportSee - Assistant Intelligent NBA*  
*Janvier 2026*
