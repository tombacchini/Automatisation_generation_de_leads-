# Automatisation-de-generation-de-leads
À partir d'un simple déclenchement manuel, le système identifie automatiquement des entreprises cibles, évalue leur potentiel commercial grâce à l'IA, rédige des messages LinkedIn personnalisés pour chacune, et enregistre tout dans un CRM Airtable — sans aucune intervention humaine.

## Ce que fait le système

À partir d'un simple déclenchement manuel, le système identifie automatiquement des entreprises cibles, évalue leur potentiel commercial grâce à l'IA, rédige des messages LinkedIn personnalisés pour chacune, et enregistre tout dans un CRM Airtable — sans aucune intervention humaine.

---

## Comment ça fonctionne

### Étape 1 — Collecte des entreprises
Le système interroge l'API publique `recherche.entreprises.data.gouv.fr` pour récupérer des entreprises selon des critères précis : code NAF 4941Z (transport routier de fret), localisation en Île-de-France, effectif entre 20 et 80 salariés. La pagination est gérée automatiquement pour traiter de grands volumes.

### Étape 2 — Filtrage et structuration
Chaque entreprise récupérée est vérifiée : si le code NAF ne correspond pas, elle est écartée. Les données sont ensuite nettoyées et restructurées (raison sociale, SIREN, poste du dirigeant, adresse…) pour être exploitables.

### Étape 3 — Qualification par l'IA
Un modèle GPT-4 analyse chaque entreprise et lui attribue un **score de 0 à 100** ainsi qu'une **qualification** (`Qualifié` ou `À vérifier`). L'IA évalue le potentiel commercial du prospect selon des critères métier : volume probable de livraisons, structure multi-sites, type d'activité B2B, etc.

### Étape 4 — Enrichissement web
Pour chaque lead qualifié, un agent IA autonome (GPT Mini + Serper) effectue des recherches web en temps réel sur l'entreprise : actualités récentes, croissance, ouvertures de sites, recrutements… Ces informations servent à personnaliser les messages.

### Étape 5 — Rédaction des messages LinkedIn
Claude Sonnet (Anthropic) rédige automatiquement deux messages par lead :
- Un **message de connexion** court et personnalisé, ancré sur une information concrète trouvée sur l'entreprise
- Un **message après acceptation** qui présente la valeur de la solution en lien direct avec le contexte de l'entreprise

### Étape 6 — Enregistrement dans Airtable
Tout est sauvegardé dans une base Airtable structurée comme un CRM : score IA, qualification, messages rédigés, statut pipeline, température du lead, date de contact.

---

## Résultat

Chaque ligne du CRM contient un prospect identifié, scoré et prêt à contacter, avec deux messages LinkedIn déjà rédigés et personnalisés. Le commercial n'a plus qu'à copier-coller et envoyer.

---

## Stack technique

- **n8n** — orchestration des workflows
- **API Data Gouv** — source de données entreprises
- **OpenAI GPT-4o** — qualification et scoring
- **Claude Sonnet** (Anthropic) — rédaction des messages
- **Serper API** — recherche web temps réel
- **Airtable** — CRM et suivi pipeline
