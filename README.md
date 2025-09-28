## DirhamWatch – Suivi des cours de change

### Schéma de la pipeline

![Workflow DirhamWatch](https://github.com/user-attachments/assets/b6a9d926-8bcd-4bca-aa0f-50654fe73748)
<img width="714" height="477" alt="dirhamwatch_workflow" src="https://github.com/user-attachments/assets/a3671f7e-329a-4a94-9ff9-91a316c49e8d" />

### Vidéo de démonstration

https://github.com/user-attachments/assets/e90c35ab-4298-4cbf-808a-fbda0b0fc157

**DirhamWatch** est un projet de scraping, transformation et visualisation des taux de change du dirham marocain (MAD) face aux principales devises étrangères, sur une période d'une année.  
Les données sont extraites quotidiennement depuis le site officiel de [Bank Al-Maghrib](https://www.bkam.ma/Marches/Principaux-indicateurs/Marche-des-changes/Cours-de-change/Cours-des-billets-de-banque-etrangers).

### 1. Objectif du projet

Créer une pipeline ETL automatisée pour :

#### a. Extraction

- Scraping quotidien depuis Bank Al-Maghrib
- Données extraites : `Monnaie`, `Date`, `Taux d’achat`, `Taux de vente`
- Fichier brut : `raw_taux_change.csv`

#### b. Transformation (Pandas)

- Interpolation des valeurs manquantes
- Normalisation des noms de devises
- Conversion des dates au format `mm/dd/yyyy`
- Casting des taux
- Calcul :
  - `Taux_moyen = (Taux_achat + Taux_vente) / 2`
  - `Écart = Taux_vente - Taux_achat`
- Fichier transformé : `clean_taux_change.csv`

#### c. Chargement (PostgreSQL)

```sql
CREATE TABLE clean_taux_change (
    id SERIAL PRIMARY KEY,
    date DATE,
    nom_devise VARCHAR(10),
    taux_achat FLOAT,
    taux_vente FLOAT,
    taux_moyen FLOAT,
    var_journaliere FLOAT
);
```

### 2. Visualisation

Connexion de la base PostgreSQL (hébergée via Aiven Cloud) à Looker Studio pour :

- Comparaison des taux par devise (vente, achat, moyen)
- Répartition des taux avec écart dynamique
- Évolution temporelle des taux d’achat et de vente
- Variation du taux moyen de change
- Écart achat/vente dans le temps
- Heatmap des fluctuations hebdomadaires

### 3. Exemple d’URL utilisée

```bash
https://www.bkam.ma/Marches/Principaux-indicateurs/Marche-des-changes/Cours-de-change/Cours-des-billets-de-banque-etrangers?date=06%2F05%2F2025&block=98a86bd3205c8223897bbd8d87e3788d
```

### 4. Technologies utilisées

- Python
- Pandas
- Airflow
- PostgreSQL
- Looker Studio
- Aiven Cloud

### 5. Exécution de la pipeline

#### a. Installation des dépendances

```bash
pip install -r requirements.txt
```

#### b. Lancement d’Airflow

```bash
airflow db init
airflow webserver --port 8080
airflow scheduler
```

#### c. Activation du DAG

Via l’interface Airflow : `etl_dag`

---

**Auteur :** [Yassine Darif](https://www.linkedin.com/in/darif-yassine)


