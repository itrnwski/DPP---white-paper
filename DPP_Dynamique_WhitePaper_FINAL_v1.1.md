# Protocole DPP Dynamique
## White Paper – Version 1.1 (Décembre 2025)
### FINAL 

---

## Résumé exécutif

Le **Digital Product Passport (DPP)** est une obligation réglementaire européenne qui imposera progressivement à partir de 2027 la documentation numérique du cycle de vie des produits (vêtements, électronique, batteries, meubles, etc.).

**Le problème** : Les solutions actuelles documentent bien les données de fabrication (d'où vient le produit, comment il est fait). Mais personne ne documente **ce qui se passe réellement après l'achat** : réparations, nettoyage, vente de seconde main, recyclage.

Exemple réel : Un jean acheté neuf en 2024 est réparé en 2025 (aucune trace). Vendu en seconde main (aucun historique). Arrivé au recyclage en 2026 (impossible de vérifier si c'est vrai). À partir de 2027, le producteur doit prouver ses claims de durabilité, mais il n'a aucune donnée terrain.

**Notre solution** : Un protocole minimal (pas une plateforme) qui permet à n'importe qui (réparateur, atelier de nettoyage, collecteur, ressourcerie) de documenter un événement dans la vie du produit de manière vérifiée et interopérable.

**L'approche** :
- Données opérationnelles stockées en base de données classique (flexible, performant)
- Preuve cryptographique ancrée en blockchain (garantit qu'on ne peut pas mentir rétroactivement)
- Coûts minimes (batching : 100–500 événements en 1 ancrage)
- Interopérable : compatible avec standards qui arrivent (GS1, identifiants numériques, etc.)
- Indépendant : si l'UE impose un registre central, on l'alimente (on ne le concurrence pas)

**Viabilité financière** : Financements publics européens (Innoviris, Horizon Europe, gouvernances régionales) soutiennent déjà ce type de brique infrastructure.

---

## 1. Contexte réglementaire et le problème réel

### La directive ESPR : DPP obligatoire à partir de 2027

En mars 2024, l'UE a adopté l'**Ecodesign for Sustainable Products Regulation (ESPR)**, qui impose le DPP pour plusieurs secteurs :

| Secteur | Quand obligatoire |
|---------|------------------|
| Textile | 2027–2028 |
| Électronique | 2027–2028 |
| Batteries | 2027 |
| Meubles | 2028 |
| Chimie | 2028–2029 |
| Acier, aluminium | 2029+ |

Chaque secteur doit respecter des schémas de données spécifiques (ex. : pour le textile, composition, origines, certifications). Mais tous partagent l'objectif : prouver via un dossier numérique vérifiable que le produit est vraiment ce qu'on prétend.

### Le paradoxe : données amont vs. terrain

**Ce qui existe déjà en 2025** :
- Données de fabrication (matériaux, origine, usines, certifications)
- Supply chain initial (logistique, transport, douanes)
- État du produit au lancement

**Ce qui manque totalement** : la documentation de ce qui se passe après la vente.

**Exemples concrets** :
- Un jean réparé en 2025 : le réparateur n'enregistre rien. Aucune trace.
- Le même jean vendu d'occasion : le vendeur deuxième main n'a pas d'outil pour documenter son état, les réparations, les tests.
- En 2026, arrivée à la ressourcerie pour tri/recyclage : impossible de vérifier que le produit a vraiment été traité, par qui, comment.

**Conséquence en 2027–2028** :
Quand le textile DPP devient obligatoire, le producteur initial doit prouver « 80% de mes produits sont recyclés » ou « réparés ». Mais sans données terrain, c'est juste du greenwashing. Il n'a aucun moyen de vérifier.

### Trois risques majeurs si rien ne change

1. **Fragmentation silotée** : Chaque région, chaque secteur, chaque grande plateforme va développer son propre système. Coûts énormes pour les petits acteurs (réparateurs, collecteurs).

2. **Donnée non vérifiable** : Le DPP devient un document rempli de promesses sans preuve. Aucune crédibilité.

3. **Exclusion des petits acteurs** : Si seules les grandes plateformes centralisées peuvent documenter, les réparateurs indépendants, ressourceries, collecteurs disparaissent. La circularité réelle disparaît.

**Le besoin** : Une infrastructure publique, décentraisée, sobriette, qui permette à n'importe qui de documenter ce qu'il observe et fait. C'est exactement ce que le Protocole DPP Dynamique propose.

---

## 2. Notre hypothèse et positionnement

### L'hypothèse centrale

**Le DPP ne peut fonctionner que par une logique hybride (top-down + bottom-up).**

- **Top-down** : L'UE impose des règles, définit des schémas de données, crée éventuellement un registre central.
- **Bottom-up** : Les acteurs terrain (réparateurs, reconditionneurs, collecteurs) documentent ce qu'ils observent et font. Leurs données alimentent le registre.

La qualité du registre central dépendra entièrement de la qualité des données du bas. Sans une bonne infrastructure de terrain, le DPP sera un monument bureaucratique rempli de données vides ou fausses.

### Ce que nous construisons

Le **Protocole DPP Dynamique** est une brique d'infrastructure publique pensée pour :

1. **Être minimal** : Propose seulement l'essentiel (structure d'un événement, signature, preuve sur blockchain). Tout le reste (interface utilisateur, intégrations métier, détails sectoriels) vit en extension.

2. **Être décentralisée** : Fonctionne sans point central obligatoire. Chaque acteur terrain peut documenter indépendamment.

3. **Être vérifiable** : Chaque événement est signé par qui l'a créé, chaîné avec le précédent, ancré en blockchain pour preuve d'antériorité. Aucune confiance requise envers une autorité centrale.

4. **Être interopérable** : Compatible avec les standards qui arrivent (GS1 Digital Link pour les codes-barres, W3C pour les identifiants numériques, EIOPA pour les standards batterie, etc.). Jamais propriétaire.

5. **Être sobre** : Données en base de données classique (performante, flexible), blockchain uniquement comme couche de preuve. Coûts viables même pour un petit réparateur.

### Ce que nous ne faisons PAS

- On ne construit pas un "registre central du DPP" (ça, c'est le rôle potentiel de l'UE)
- On ne force pas un secteur (textile ≠ électronique ≠ batteries)
- On n'impose pas une gouvernance lourde dès le jour 1
- On ne promet pas « on va sauver 1000 tonnes de CO2 » (on mesure ce qui est vérifiable, c'est tout)

---

## 3. Architecture technique

### 3.1 Schéma global : Base de données + Blockchain

```
┌────────────────────────────────────┐
│  Base de données relationnelle      │
│  (Supabase, PostgreSQL, etc.)       │
│                                     │
│  - Fiche produit (jean-001)         │
│  - Événements                       │
│    (réparation le 15 janvier,       │
│     test le 20 janvier, etc.)       │
│  - Acteurs (qui a fait quoi)        │
│  - Timelines (historique complet)   │
└────────────────────────────────────┘
          ↓ (résumé, hash)
┌────────────────────────────────────┐
│  Blockchain (couche de preuve)      │
│  (Polygon, Gnosis, Arbitrum, etc.)  │
│                                     │
│  - Ancrage batch (100–500 événements)
│  - Preuve : "ces 500 événements     │
│    existaient le 15 janvier à 14h00"│
│  - Intégrité : "personne n'a        │
│    modifié après coup"              │
└────────────────────────────────────┘
```

**Pourquoi cette séparation ?**
- Les données sensibles (métadonnées, informations personnelles) **ne vont jamais en blockchain**
- Seul un résumé cryptographique (hash) va en chaîne
- C'est moins cher, plus rapide, compatible RGPD, et tout aussi sûr

### 3.2 Comment ça marche : append-only + contestation

**Append-only** = on ne peut que **ajouter**, jamais effacer ou modifier.

Chaque événement :
1. Référence le hash du précédent (chaînage)
2. Est signé par la personne qui l'a créé (authentification)
3. A un timestamp (quand c'est arrivé)
4. Est ancré en blockchain (preuve)

**Exemple concret** :

```
[15 jan, 14:00] Atelier Réparation Bruxelles crée événement :
  "Produit : jean-001"
  "Événement : réparation"
  "Détails : couture recousue, zip remplacé"
  "Signature : atelier-001-key-v1"
  → Hash produit : abc123

[20 jan, 10:00] Reconditionneurs "2nd Life" ajoute événement :
  "Produit : jean-001"
  "Événement : reconditionnement"
  "Détails : nettoyage, test : OK"
  "Signature : recondit-001-key"
  "Référence précédente : abc123"
  → Hash produit : def456

[25 jan, 09:00] Ressourcerie ajoute événement :
  "Produit : jean-001"
  "Événement : collecte fin de vie"
  "Détails : composition détectée : 80% coton, 20% polyester"
  "Signature : ressourcerie-001-key"
  "Référence précédente : def456"
  → Hash produit : ghi789

→ Timeline complète : réparation → reconditionnement → collecte
→ Chacun peut vérifier : "ce jean a vraiment eu ces 3 événements, dans cet ordre, à ces dates"
```

**Et si quelqu'un conteste ?**

Par exemple, la ressourcerie dit : « Non, ce jean ne montrait aucun défaut en arrivée » (contrairement à ce que l'atelier a dit).

Solution : on **ajoute un événement de contestation** signé par la ressourcerie :

```
[26 jan, 14:00] Ressourcerie ajoute événement de contestation :
  "Type : dispute / claim"
  "Je conteste l'événement abc123"
  "Raison : Les dégâts signalés n'étaient pas présents à réception"
  "Preuves : [photos hashées, certificat de réception]"
  "Signature : ressourcerie-001-key"
```

**Résultat** : La timeline reste **complète et auditable**. Un auditeur tiers lit :
- L'atelier dit : « dégâts signalés »
- La ressourcerie dit : « non, pas vus »
- Les deux événements sont là
- Au auditeur de juger

C'est honnête, transparent, et impossible à manipuler.

### 3.3 Modèle de données

#### Schéma obligatoire (le squelette minimal)

Tout événement a :
```json
{
  "product_id": "GS1-DL-ou-identifiant-interne",
  "event_id": "identifiant-unique",
  "timestamp": "2025-01-15T14:30:00Z",
  "previous_event_hash": "abc123...",
  "event_type": "repair | refurbish | collection | test | dispute | ...",
  "actor": {
    "id": "atelier-bruxelles-001",
    "name": "Atelier Couture Bruxelles",
    "role": "repairer"
  },
  "signature": {
    "algorithm": "HMAC-SHA256",
    "value": "sig_..."
  },
  "metadata": {
    // Les détails spécifiques à cet événement
    // (voir ci-dessous par secteur)
  },
  "blockchain_anchor": {
    "chain": "polygon",
    "tx_hash": "0x...",
    "proof_of_inclusion": "..."
  }
}
```

#### Extensions sectorielles (la flexibilité)

Chaque secteur ajoute ses propres détails sans modifier le squelette :

**Textile (réparation)** :
```json
"metadata": {
  "fabric_type": "coton",
  "damage_before": ["couture déchirée côté gauche", "zip bloqué"],
  "repair_performed": ["couture recousue", "zip remplacé"],
  "materials_used": ["fil polyester noir", "zip YKK"],
  "condition_after": "comme neuf",
  "estimated_extra_lifespan_months": 12
}
```

**Électronique (test)** :
```json
"metadata": {
  "device_type": "smartphone",
  "tests_performed": ["batterie", "écran", "audio", "connectivité"],
  "test_results": {
    "batterie": "85% de capacité",
    "écran": "OK",
    "audio": "OK",
    "4G": "OK"
  },
  "next_use": "resale"
}
```

**Fin de vie (collecte)** :
```json
"metadata": {
  "material_composition": {
    "plastique": "15%",
    "métal": "25%",
    "textile": "60%"
  },
  "sorting_destination": "recyclage-fibre",
  "condition_noted": "intact"
}
```

**Avantage** : Un réparateur textile remplit 5 champs. Un agent de collecte remplit 3 champs. Aucun n'est obligé de comprendre l'autre. Mais tous sont lisibles et vérifiables.

### 3.4 Interopérabilité : V1 vs. V2

#### V1 (maintenant, 2026) : Pragmatique

| Standard | Comment on l'utilise | Pourquoi |
|----------|----------------------|----------|
| **GS1 Digital Link** | Tous les produits ont un code-barres QR scannable qui renvoie à la timeline | Déjà utilisé dans retail, logistics, food. Les gens connaissent. On peut scanner avec n'importe quel téléphone. |
| **Signatures simples** | Clé cryptographique de l'atelier (pas complexe) | Compréhensible, auditable, pas besoin d'infrastructure PKI lourde |

#### V2 (2027+, si consortium) : Futur

| Standard | Quand on l'ajoute | Exemple |
|----------|------------------|---------|
| **DID** (Decentralized Identifiers) | Quand plusieurs organisations gèrent le protocole | Pour identifier chaque acteur de manière unique et souveraine |
| **W3C Verifiable Credentials** | Quand on formalise des attestations (« ce réparateur est certifié ») | Pour pouvoir vérifier « qui a le droit de dire quoi » |
| **Schémas sectoriels** (EIOPA pour batteries, etc.) | Quand un secteur le demande | Via un « mapping » (traduction entre nos événements et leurs schémas) |

**Posture** : On reste minimaliste. On ajoute des standards quand ça a du sens, pas avant.

### 3.5 Sécurité

#### Clés et signature

- Chaque atelier, collecteur, reconditeur gère sa propre clé (ou la délègue à quelqu'un de confiance)
- Quand ils créent un événement, ils le signent avec leur clé
- N'importe qui peut vérifier la signature en ayant la clé publique de cet acteur
- **Audit trail complet** : on sait qui a dit quoi, quand

#### Rotation de clés (renouvellement régulier)

- Tous les 6 mois, chaque acteur génère une nouvelle clé
- Les anciens événements restent vérifiables avec l'ancienne clé
- Si une clé est compromise (volée), l'acteur publie un événement spécial « cette clé n'est plus valide »
- Les événements signés avec cette clé restent dans l'historique (pas d'effacement), mais marqués comme « nécessitent re-audit »

#### Confidentialité

- Les données sensibles (info personnelle, secrets commerciaux) **ne vont jamais en blockchain**
- Seul un résumé cryptographique (hash) va en chaîne
- RGPD-compatible par design

---

## 4. Modèle économique

### 4.1 Coûts opérationnels

#### Blockchain : Coûts mineurs grâce au batching

**L'approche** :
- Au lieu de faire 1 ancrage par événement (cher), on batch : 100–500 événements → 1 ancrage
- Coût : quelques euros par mois même pour un petit acteur

**Réseau principal** :
- **Polygon** : ~0.001–0.01€ par transaction, très populaire, écosystème mature
- **Gnosis Chain** : encore moins cher, très « green » (pas de consommation énergétique)
- **Arbitrum ou Optimism** : fallback si les deux ci-dessus deviennent trop chers

**Coût par événement** : 0.01–0.1€ après batching. C'est-à-dire :
- Un petit réparateur avec 50 événements/mois = quelques euros/mois
- Une ressourcerie avec 1000 événements/mois = quelques dizaines d'euros/mois

**Stratégie si coûts montent** :
Si une blockchain devient trop chère, on bascule vers une autre. Zéro changement pour l'utilisateur ; juste une autre blockchain qui reçoit les hashes.

#### Infrastructure cloud

- Base de données + serveur : ~200–500€/mois en production (Supabase ou équivalent)
- Site web : ~50–100€/mois (Vercel ou équivalent)
- **Financé par** : grants publics (Innoviris, Horizon Europe), gouvernances sectorielles, ou petits abonnements utilisateur

### 4.2 Modèles de revenus (après V1, après validation)

#### Financement public (immédiat, 2026–2027)
- **Innoviris** : R&D (ce qu'on cherche là)
- **Horizon Europe / Digital Europe** : projets infrastructure 100K–500K€
- **Gouvernances régionales** : Wallonie, Bruxelles, Flandre cherchent des solutions DPP

#### Services et intégrations (après 2027)
- **Hébergement + SaaS** : petits acteurs payent un abonnement
- **Intégration ERP** : cabinets de conseil facturent accompagnement + connexion à Odoo, SAP, etc.
- **Data as service** : gouvernances sectorielles achètent des données vérifiées (ex. : « montrez-moi tous les vêtements recyclés cette année »)
- **Fondation** : un jour, le protocole vit sous une fondation financée par ses utilisateurs

### 4.3 Critères de viabilité à court terme (janvier–mars 2026)

**Pendant les 8 semaines de validation, on mesure** :

✅ **Usabilité** :
- Création d'événement : ≥ 80% des cas < 60 secondes
- Taux de complétion : ≥ 85% des événements ont tous les champs obligatoires remplis (pas d'abandons en cours)

✅ **Auditabilité** :
- Un tiers (qui n'a jamais vu le système) peut reconstituer la timeline à 100%
- Les hashes sont vérifiables via des outils publics (pas besoin d'accès interne)

✅ **Adoption pilotes** :
- ≥ 70% des 3 pilotes continuent après le test (ils utilisent vraiment ça)
- Chaque pilote crée ≥ 20 événements pendant le test

✅ **Architecture stable** :
- Schéma des événements ne change pas après test (c'est bon signe)
- Aucun problème technique majeur non-résolvable

---

## 5. Roadmap d'exécution : 8 semaines

### Phase 1 : Semaines 1–4 (Formalisation)

**Objectif** : Un schéma d'événements documenté, compris par les pilotes, prêt à tester.

#### Semaine 1–2 : Schéma core

- Finaliser la structure générique en JSON
- Produire 3 exemplaires concrets et réalistes :
  - **Textile / Réparation** : un atelier documente réellement une réparation (tissus, coutures, diagnostics, signature)
  - **Électronique / Reconditionnement** : tests fonctionnels, nettoyage, certificat d'état
  - **Fin de vie / Collecte** : tri, composition matériaux détectée, destination recyclage

**Livrable** : Fichier schéma JSON + 3 exemplaires JSON réalistes

#### Semaine 2–3 : Décisions techniques V1

Documenter et fixer :
- **Identifiant produit** : Comment lier un produit interne au code GS1. Format concret.
- **Signature** : HMAC ou RSA ou ECDSA ? Comment on génère les clés ? Comment on les rotate ?
- **Blockchain** : Polygon, Gnosis, ou Arbitrum ? Batch de 100 ou 500 ? Fréquence (quotidienne, hebdo) ?
- **Contrat blockchain** : Code minimal pour poster les hashes

**Livrable** : 3–4 documents techniques (1–2 pages chacun, clair et concis)

#### Semaine 2–3 (parallèle) : Tests UX et intégrations

- **Prototype interface** (mobile simple + web formulaire) : Tester avec 5–10 petits acteurs (pas pilotes) pour avoir du feedback rapide
- **Intégration Odoo** : Sketcher une API simple pour que le système parle à Odoo. Quel « bouton » les utilisateurs Odoo verraient-ils ?

#### Semaine 3–4 : Documentation publique

- README expliquant le protocole en 2 minutes
- Architecture diagram (cf. schéma ci-dessus)
- Décisions techniques documentées
- FAQ
- Mise à jour GitHub public

**Livrable** : Repo propre, docs claires, testable. Les pilotes peuvent commencer.

---

### Phase 2 : Semaines 5–8 (Validation terrain)

**Objectif** : Confirmer que ça marche réellement avec de vrais acteurs.

#### Semaine 5–6 : Embarquer les 3 pilotes

**Cas 1 : Fin de vie**
- **Cible** : Dufour Tournai (traitement déchets, accès familial)
- **Cas concret** : Documenter flux actuel (produits en arrivée, composition, destination)
- **Enjeu** : Tester schéma fin de vie, timeline auditable

**Cas 2 : Textile / Reconditionnement**
- **Cible** : Paradox (marque textile belge)
- **Cas concret** : Produits en seconde main ou reconditionnement
- **Enjeu** : Tester schéma textile, que timeline est comprise par un acteur externe

**Cas 3 : Réparation**
- **À identifier** : Petit atelier réparation (textile ou électronique)
- **Enjeu** : Compléter le cycle (amont réparation → aval reconditionnement → fin collecte)

**Approche** : Conversation informelle (« vous testez avec nous pendant 4 semaines »), pas contrat lourd. Formaliser une fois schéma finalisé.

#### Semaine 6–7 : Tests mécaniques

Avec chaque pilote :

1. **Création d'événement < 60s** : Un opérateur mesure le timing réel. Résultat : % qui passent < 60s, goulots identifiés.

2. **Timeline compréhensible** : Partager la timeline avec quelqu'un d'externe (pas l'émetteur). Question : « Peux-tu raconter ce qui s'est passé ? » Résultat : oui/non + temps + clarté.

3. **Append-only en charge** : Ajouter 10–20 événements rapidement. Vérifier : aucun n'est perdu, tous chaînés, hashes OK.

4. **Contestation** : Scénario simulé où deux acteurs contredisent. Vérifier : timeline complète conservée, audit possible.

**Livrable** : Logs de tests, timing, captures, résultats.

#### Semaine 7–8 : Apprentissages et ajustements

- Retours des pilotes (friction, suggestions)
- Ajustements mineurs au schéma (si consensus simple)
- Documentation des apprentissages (ce qui marche, ce qui bloque, hypothèses validées)

**Livrable** : Rapport par pilote (1–2 pages), schéma V1 finalisé.

---

### Phase 3 : Après 8 semaines (Décision)

**Critères de succès** :
- ✅ Schéma compris par 3 acteurs réels
- ✅ Création < 60s validée en pratique
- ✅ Timeline auditable par un tiers
- ✅ Append-only + contestation testés
- ✅ Aucun problème tech insurmontable

**Décisions** :
- **Succès complet** → Phase 2 : déploiement production + gouvernance consortium si demande
- **Succès avec ajustements** → 4 semaines itération supplémentaire
- **Problème découvert** → Diagnostiquer, corriger, relancer

---

## 6. POC live pour la présentation Innoviris (6 janvier)

### Ce qu'on montre en live (2–3 minutes)

**Situation** : Un produit (hoodie) avec 2 événements existants.

**Démo** :
1. **Charger la timeline** (appel API)
   → Affiche : hoodie-001 + ses événements en chronologie

2. **Ajouter un événement** (formulaire simple, 2 minutes)
   - Type : "repair"
   - Détails : « recoudre les coutures »
   - Click "Envoyer"

3. **L'événement apparaît** (automatiquement)
   → Timeline mise à jour
   → Hash chaîné visible
   → Bloc blockchain status : "pending" → "confirmé"

4. **Vérifier integrity** (bonus)
   → Click "Verify"
   → Affiche preuve blockchain
   → Lien vers explorer blockchain public (Polygon, etc.)

### Tech pour coder ça (simple, 1–2 jours)

- **Backend** : Node.js + Express, 2 endpoints (GET /products, POST /events), ~100 lignes
- **Frontend** : React simple, affiche timeline + form, ~150 lignes
- **Data** : JSON fichier ou mini-BD (pas besoin Supabase complet)
- **Blockchain** : Contrat test sur Polygon, déployé et fonctionnel

### Important

C'est pas un produit fini. C'est une preuve que l'architecture marche. Si ça crash, une vidéo pré-enregistrée en backup c'est OK.

---

## 7. Équipe

### Fondateur / Porteur du protocole

- **Rôle** : Vision, cohérence système, engagement pilotes
- **Profil** : Psychomotricien, observateur terrain, pense les cas limites
- **Compétence clé** : Refuser la complexité inutile, tenir l'intégrité

### Co-fondateur technique (À recruter)

- **Rôle** : Architecture code, scalabilité, bonnes pratiques
- **Profil** : Dev mid–senior, expérience blockchain PoS ou EVM (Polygon/Gnosis), PostgreSQL, Node/TypeScript
- **Timeline** : 50% temps, 8–12 semaines, mission : structurer V0 → V1

### Partenaires pilotes

- **Dufour Tournai** (fin de vie)
- **Paradox** (textile)
- **À identifier** : réparation ou collecte

---

## 8. Budget Innoviris

### 8–12 semaines

| Poste | Montant | Justification |
|-------|---------|---------------|
| **Co-fondateur technique** (50% FTE) | 20–25K€ | Structuration code V0 → V1 |
| **Validation terrain** | 10–15K€ | Facilitation pilotes, accompagnement |
| **Infrastructure cloud** | 3–5K€ | Supabase, RPC blockchain, Vercel |
| **Documentation + communication** | 3–5K€ | Docs finales, rapports, GitHub |
| **Buffer / imprévus** | 5K€ | 10% marge |
| **TOTAL** | **45–55K€** | |

### Timeline

- **Décembre 2025–janvier 2026** : Appel Innoviris, signature
- **Janvier–février 2026** : Phase 1
- **Février–mars 2026** : Phase 2
- **Mars 2026** : Décision escalade
- **Après mars** : Financement post (Horizon EU, régional)

---

## 9. Mesure de succès et impact attendu

### Court terme : Fin Q1 2026 (après pilotes)

**Métriques mesurables** :

✅ **Adoption pilotes** :
- 70% des 3 pilotes continuent post-test vers usage réel
- Chaque pilote crée ≥ 20 événements en 8 semaines

✅ **Usabilité** :
- 80% des cas : création < 60 secondes
- 85% des événements : champs core remplis

✅ **Auditabilité** :
- 100% des timelines reconstruites correctement par un tiers
- 100% des hashes vérifiables via RPC public

✅ **Architecture** :
- Schéma V1 stabilisé (zéro changements forcés après test)
- Zéro blocages techniques non-résolus

### Moyen terme : 2026–2027 (escalade)

✅ **Adoption** :
- 5–10 organisations utilisant le protocole
- 500+ événements/mois en production
- 3 secteurs représentés (textile, électronique, batterie/fin de vie)

✅ **Réduction greenwashing** :
- % de timelines vérifiées publiquement : ≥ 60% des produits ont au moins 1 événement vérifié non-producteur
- Cas où data terrain contredit claim producteur (détecté et documenté)

✅ **Financement** :
- 1+ contrat Horizon Europe obtenu
- 2+ gouvernances sectorielles finançant l'infra

### Long terme : 2027+ (pérennisation)

✅ **Scalabilité** :
- 50+ organisations EU-wide
- 100K+ événements/an
- Utilisé par 2+ gouvernances sectorielles officielles

✅ **Crédibilité réglementaire** :
- Intégration dans registre DPP EU (si créé) OU reconnaissance Commission EU

✅ **Modèle viable** :
- Revenus mixtes : subventions (40%), services (30%), data (20%), dues (10%)
- Coûts opérationnels couverts 3 ans minimum

---

## 10. Risques et mitigations

| Risque | Symptôme | Mitigation |
|--------|----------|-----------|
| **Adoption pilotes faible** | Dufour ou Paradox ne signe pas | Outreach précoce, 5–7 candidats, accepter premiers 3 oui |
| **Schéma trop complexe** | Pilotes disent « c'est impossible » | Tests UX semaine 2, exemplaires JSON réalistes |
| **UX trop lente** | Création > 60s en pratique | Mode draft (brouillon), intégration ERP, test early |
| **Coûts blockchain imprévisibles** | Frais explosent sur une blockchain | Tests réels semaine 3, plans B (autres chaînes) |
| **Manque co-founder tech** | Pas de candidat sérieux en janvier | Outreach dès janvier, freelancer short-term, réseau Bruxelles |

---

## 11. Conclusion

Le DPP est inévitable. Les solutions actuelles adressent l'amont. Ce qui manque est une **infrastructure de documentation terrain accessible à tous**.

Le **Protocole DPP Dynamique** propose une brique minimale, pragmatique, robuste. Il ne concurrence pas la régulation ; il en devient la fondation de crédibilité.

**L'opportunité** :
- Moment critique (2026 avant 2027 DPP mandatory)
- Marché clair (tous les secteurs DPP ont besoin de ça)
- Financement public disponible (Innoviris, Horizon EU, régional)
- Partenaires réels identifiés (Dufour, Paradox)
- Validation terrain faisable en 8 semaines

**Le pari** : Valider en 8 semaines que approche fonctionne. Après, escalader via financements publics et gouvernances sectorielles.

**Timing** : C'est maintenant ou jamais. En 2027, tout le monde sera en panic mode. En 2026, on peut valider tranquille.

---

## Annexes

### A. Schéma JSON core (exemple)

```json
{
  "product_id": "GS1-DL-ou-interne",
  "event_id": "unique-id",
  "timestamp": "2025-01-15T14:30:00Z",
  "previous_event_hash": "abc123...",
  "event_type": "repair",
  "actor": {
    "id": "atelier-bruxelles-001",
    "name": "Atelier Couture Bruxelles",
    "role": "repairer"
  },
  "signature": {
    "algorithm": "HMAC-SHA256",
    "value": "sig_..."
  },
  "metadata": {
    "item_type": "jeans",
    "damage_observed": ["couture gauche", "zip bloqué"],
    "repair_performed": ["couture recousue", "zip remplacé"],
    "condition_after": "comme neuf"
  },
  "blockchain_anchor": {
    "chain": "polygon",
    "tx_hash": "0x...",
    "block_number": 52345678
  }
}
```

### B. GS1 Digital Link : Comment ça marche

GS1 Digital Link = un standard qui lie un code QR à des données.

**Format** : `https://gs1.example.com/01/[code]/`

**Notre usage** :
- Chaque produit a un code GS1 (ou on mappe un interne)
- QR scannble par n'importe quel téléphone
- Redirige vers la timeline du produit
- Les acteurs voient directement les événements, peuvent ajouter les leurs

**Avantage** : Déjà industriel, zéro explication nécessaire.

---

## Historique

| Version | Date | Changements |
|---------|------|-----------|
| 0.1 | Oct 2025 | Soumission Innoviris initiale |
| 1.0 | Déc 2025 | Révision post-audit : architecture complète, roadmap 8 semaines |
| 1.1 | Déc 2025 | Merge updates V1–V7 : blockchain alternatives, Merkle proofs, UX tests, KPIs, gouvernance, POC live |

---

**Document à jour 24 décembre 2025.**  
**Prêt pour Innoviris 6 janvier 2026.**
