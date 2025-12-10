# Revue des règles de collection des données DCR


**Version** : 1.0
**Date de production**: 2025-12-10


## Table des matières
1. Introduction
2. Principe de fonctionnement
3.

## 1. Introduction
Les Data Collection Rules (DCR) introduisent une approche normalisée de collecte des données dans Azure Monitor, basée sur un pipeline ETL (extraction, transformation, chargement). Elles permettent une configuration uniforme des différentes sources de données, offrent la possibilité d’appliquer des transformations avant l’ingestion et s’intègrent facilement dans des pratiques IaC/DevOps. Dans la majorité des cas, Azure Monitor crée automatiquement les DCR, mais leur gestion manuelle permet une personnalisation avancée du flux de collecte.


## DCR sur Monitor

Pour afficher les DCR existantes, allez dans:   
Azure > Monitor > Settings > Data Collection Rules 

![alt text](images/01-dcr%20sur%20azure%20monitor.png)