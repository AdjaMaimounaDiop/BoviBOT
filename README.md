# BoviBot — Gestion d'Élevage Bovin avec IA et PL/SQL
**Projet L3 — ESP/UCAD**

## Démarrage rapide

### 1. Base de données MySQL
```bash
mysql -u root -p < schema.sql
```
Cela crée la base `bovibot` avec toutes les tables, procédures, fonctions, triggers et events.

### 2. Clé OpenAI
Obtenir une clé gratuite sur https://platform.openai.com/api-keys  
Éditer le fichier `.env` :
```
OPENAI_API_KEY=sk-ta-cle-ici
DB_PASSWORD=ton_mot_de_passe_mysql
```

### 3. Installer les dépendances
```bash
pip install -r requirements.txt
```

### 4. Lancer le backend
```bash
python app.py
# Ou : uvicorn app:app --host 0.0.0.0 --port 8002 --reload
```

### 5. Ouvrir l'interface
Aller sur **http://localhost:8002**  
(Le backend sert directement le dashboard et le chat IA)

---

## Architecture

| Composant | Technologie |
|-----------|-------------|
| Backend   | Python FastAPI (port 8002) |
| Base      | MySQL 8.x + PL/SQL |
| IA        | OpenAI gpt-4o-mini (Text-to-SQL + actions) |
| Frontend  | HTML/CSS/JS (servi par FastAPI) |

## Éléments PL/SQL

| Type | Nom | Rôle |
|------|-----|------|
| Procédure | `sp_enregistrer_pesee` | Pesée + MAJ poids + calcul GMQ + alerte |
| Procédure | `sp_declarer_vente` | Vente + changement statut animal |
| Fonction | `fn_age_en_mois(id)` | Âge de l'animal en mois |
| Fonction | `fn_gmq(id)` | Gain Moyen Quotidien (kg/jour) |
| Trigger | `trg_historique_statut` | Log des changements de statut |
| Trigger | `trg_alerte_vaccination` | Alerte si rappel vaccin dépassé |
| Trigger | `trg_alerte_poids_faible` | Alerte critique si veau < 60 kg avant 6 mois |
| Event | `evt_alerte_velages` | Vêlages dans les 7 prochains jours (quotidien) |
| Event | `evt_rapport_croissance` | Rapport hebdomadaire du troupeau |

## Fonctionnalités LLM

**Consultation (Text-to-SQL) :**
- "Liste tous les animaux actifs avec leur âge et GMQ"
- "Quels animaux ont un GMQ inférieur à 0.3 kg/jour ?"
- "Quelles femelles vêlent dans les 30 prochains jours ?"

**Action avec confirmation :**
- "Enregistre une pesée de 325 kg pour TAG-001 aujourd'hui"
- "Déclare la vente de TAG-003 à Oumar Ba pour 280 000 FCFA"

## Vérification santé
```
GET http://localhost:8002/health ou bien http://bovibot.mouhadev.com/
```