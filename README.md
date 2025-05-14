# 💱 DirhamWatch – Suivi des cours de change

https://github.com/DARIF-YS/dirham-exchange-monitor/blob/main/assets/intro.mkv

**DirhamWatch** est un projet de scraping, transformation et visualisation des taux de change du dirham marocain (MAD) face aux principales devises étrangères sur une periode de d'une annee.  
Les données sont extraites quotidiennement depuis le site officiel de [Bank Al-Maghrib](https://www.bkam.ma/Marches/Principaux-indicateurs/Marche-des-changes/Cours-de-change/Cours-des-billets-de-banque-etrangers).

## 🎯 Objectif du projet

Créer une pipeline **ETL automatisée** pour :

### 📥 Extraction

- Scraping quotidien des taux de change depuis Bank Al-Maghrib :
  - **Données extraites** : `Monnaie`, `Date`, `Taux d’achat`, `Taux de vente`
  - **Fichier de sauvegarde brut** : `raw_taux_change.csv`

### 🔄 Transformation

- Nettoyage et enrichissement avec **Pandas** :
  - Interpolation des valeurs manquantes
  - Normalisation des noms de devises
  - Conversion des dates au format `mm/dd/yyyy`
  - Casting des taux et nettoyage des chaînes de texte
  - Calcul de :
    - `Taux_moyen = (Taux_achat + Taux_vente) / 2`
    - `Écart = Taux_vente - Taux_achat`
  - **Fichier de sauvegarde clean** : `clean_taux_change.csv`

### 🧱 Chargement

- Stockage des données propres dans une base **PostgreSQL** :

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


## 📊 Visualisation

Connexion de la base PostgreSQL (via **Aiven Cloud**) à **Looker Studio** pour :

- Comparaison des Taux de Change par Devise : Vente, Achat et Moyen  
- Répartition des Taux : Vente vs. Achat avec Écart Dynamique  
- Évolution du Taux de Vente dans le Temps  
- Évolution du Taux d’Achat dans le Temps  
- Variation du Taux Moyen de Change  
- Écart Achat/Vente – Suivi Temporel  
- Heatmap des fluctuations hebdomadaires  


## 🔗 Exemple d’URL utilisée pour le scraping

```bash
https://www.bkam.ma/Marches/Principaux-indicateurs/Marche-des-changes/Cours-de-change/Cours-des-billets-de-banque-etrangers?date=06%2F05%2F2025&block=98a86bd3205c8223897bbd8d87e3788d
```

## ⚙️ Technologies utilisées

- **Python** – pour l’automatisation, le traitement et le scraping  
- **Pandas** – pour la transformation des données  
- **Airflow** – pour l’orchestration de la pipeline  
- **PostgreSQL** – pour le stockage des données  
- **Looker Studio** – pour la visualisation des données  
- **Aiven Cloud** – base de données PostgreSQL managée


## 🚀 Exécution de la pipeline

1. **Installer les dépendances** :
   ```bash
   pip install -r requirements.txt
   ```

2. **Lancer Airflow** :
   ```bash
   airflow db init
   airflow webserver --port 8080
   airflow scheduler
   ```

3. **Activer le DAG `etl_dag` dans l'interface Airflow**.

## 📫 Contact

For any questions, please contact me via [GitHub Issues](https://github.com/DARIF-YS) or reach out to me on LinkedIn.