# Analyse et suivi des taux de change

Ce projet permet de scraper, transformer et visualiser les taux de change du dirham marocain (MAD) face aux principales devises étrangères.  
Les données sont collectées quotidiennement depuis [Bank Al-Maghrib](https://www.bkam.ma/Marches/Principaux-indicateurs/Marche-des-changes/Cours-de-change/Cours-des-billets-de-banque-etrangers).

## Pipeline ETL

- **Extraction** : scraping quotidien (`Monnaie`, `Date`, `Taux d’achat`, `Taux de vente`)  
- **Transformation** : interpolation, normalisation, calcul du taux moyen et de l’écart  
- **Chargement** : stockage dans PostgreSQL (`clean_taux_change`)  

<!-- Image avec bordure et taille contrôlée -->
<img src="https://github.com/user-attachments/assets/a3671f7e-329a-4a94-9ff9-91a316c49e8d" alt="workflow_etl" style="border:2px solid #000; width:700px; height:470px;">

## Visualisation

Connexion de la base PostgreSQL (hébergée via Aiven Cloud) à Looker Studio pour :  

- Comparaison des taux par devise (vente, achat, moyen)  
- Répartition des taux avec écart dynamique  
- Évolution temporelle des taux d’achat et de vente  
- Variation du taux moyen de change  
- Écart achat/vente dans le temps  
- Heatmap des fluctuations hebdomadaires  

[🎥 Voir la vidéo de démonstration](https://github.com/user-attachments/assets/e90c35ab-4298-4cbf-808a-fbda0b0fc157)

## Technologies

Python | Pandas | Airflow | PostgreSQL | Looker Studio | Aiven Cloud  

**Auteur :** [Yassine Darif](https://www.linkedin.com/in/darif-yassine)
