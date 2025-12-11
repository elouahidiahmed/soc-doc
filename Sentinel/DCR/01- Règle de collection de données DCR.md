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

## Processus de collecte des données

Les DCR définissent comment Azure Monitor doit traiter les données entrantes : quelles données collecter, leur format, les transformations éventuelles et la destination d’ingestion. Elles fournissent un pipeline de traitement unifié pour tous les scénarios de collecte.

![alt text](images/01-processus-collecte-dcr.png)


## Associations des règles de collecte des données (DCRA)

Les DCRAs établissent le lien entre une DCR et une ressource. Comme la relation est many-to-many, une DCR peut servir plusieurs ressources et une ressource peut utiliser plusieurs DCR (jusqu’à 30).

``` mermaid

flowchart TB
    subgraph DCRAs [DCRAs]
        Ressource <--> DCRA <--> DCR
    end

style DCRAs color:#333,fill:#fff,stroke:#333,stroke-width:1px

style Ressource color:#333, fill:#fff, stroke:#333
style DCRA     color:#333, fill:#fff, stroke:#333
style DCR      color:#333, fill:#fff, stroke:#333

```

## Usage d'un DCR

une fois une DCR est créée, il existe plusieurs façons pour l'utiliser:


| Scenario                     | Method                                                              |
|------------------------------|---------------------------------------------------------------------|
| Azure Monitor agent (AMA)    | Data collection rule association (DCRA)                             |
| Event hubs                   | Data collection rule association (DCRA)                             |
| Platform metrics (preview)   | Data collection rule association (DCRA)                             |
| Direct ingestion             | DCR spécifié dans l'appel d'API Azure Monitor. |
| Workspace transformation DCR | La DCR est activée pour le workspace.            |


## Scénarios d'utilisation
Cette section décrit les scnénarios d'usage des DCR les plus communs.

### Azure Monitor agent (AMA)  

L’agent AMA collecte les données des VMs et clusters Kubernetes en suivant les instructions définies dans les DCR. Après installation, l’agent récupère les DCR associées, détermine quelles données collecter (événements, performances, métriques Prometheus) puis envoie ces données à Azure Monitor. Les transformations définies dans la DCR sont exécutées avant que les données ne soient stockées dans le workspace et la table cible.

![alt text](images/01-dcr-ama-azure_monitor.png)


### Event hubs

Azure Monitor peut ingérer des données provenant d’Event Hubs. À la réception des données, Azure Monitor applique les transformations définies dans les DCR associées, puis les transmet aux destinations configurées (Log Analytics workspace, tables…).

![alt text](images/01-event-hub-dcr-azure-monitor.png)

