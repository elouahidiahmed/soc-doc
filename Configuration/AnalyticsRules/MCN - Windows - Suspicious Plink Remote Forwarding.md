# MCN - Windows - Suspicious Plink Remote Forwarding

**Description :**  
Détecte l'utilisation de Plink avec l'option -R pour créer des tunnels SSH inversés, souvent utilisés pour contourner les pare-feu et établir des canaux de communication cachés.

**Severity :** High  
**Enabled :** True  
**Start Time (UTC) :** None  

**Fichier source :** `MCN - Windows - Suspicious Plink Remote Forwarding.json`  
**Chemin :** `C:\Users\eloah01\Gouv Qc\MCN-MCN Projet de surveillance gouvernementale - Documents\POC Validation règles\Règles Migrées\Scandium21-main\AnalyticRules\Windows\MCN - Windows - Suspicious Plink Remote Forwarding.json`  

---

## 🎯 Tactics / Techniques

**Tactics :**
- CommandAndControl
- DefenseEvasion

**Techniques :**
- T1572
- T1090

**Sub-techniques :**
- T1572.001

---

## ⚙️ Configuration de la règle

- **Query frequency :** PT5M  
- **Query period :** PT5M  
- **Trigger operator :** GreaterThan  
- **Trigger threshold :** 0  
- **Suppression enabled :** False  
- **Suppression duration :** PT1H  

---

## 📝 Query

```kusto
SecurityEvent

| where EventID == 4688

| where NewProcessName has_any (dynamic(["plink.exe", "plink64.exe"]))

| where CommandLine has "-R"

| where not(CommandLine has "deeplink")

| where not(NewProcessName matches regex @"(?i)(Teams\.exe|TortoiseGitPlink\.exe)")

| extend LowerCommandLine = tolower(CommandLine)

| where LowerCommandLine contains "-r"
```

---

## 🧭 Entity mappings

- **Host**
  - `FullName` → `Computer`
- **Account**
  - `FullName` → `Account`
- **Process**
  - `CommandLine` → `CommandLine`

---

## 🧷 Sentinel entities mappings

_Aucune_

---

## 📦 Incident grouping

- **Enabled :** True  
- **Reopen closed incident :** False  
- **Lookback duration :** PT1H  
- **Matching method :** Selected  

**Group by entities :**  
- Host
- Account

**Group by alert details :**  
_Aucune_

**Group by custom details :**  
_Aucune_

---

## 🧾 Event grouping

- **Aggregation kind :** AlertPerResult  

---

## 🏷️ Custom details

| Clé | Valeur |
|-----|--------|
| ProcessName | NewProcessName |
| CommandLine | CommandLine |

---

## 🔧 Alert details override (brut)

```json
{}
```

---

## 🧱 Raw JSON (resource)

```json
{
    "id": "[concat(resourceId('Microsoft.OperationalInsights/workspaces/providers', parameters('workspace'), 'Microsoft.SecurityInsights'),'/alertRules/4b8d3f2e-7c9a-4d6b-8e5f-9a2c3d4e5f6a')]",
    "name": "[concat(parameters('workspace'),'/Microsoft.SecurityInsights/4b8d3f2e-7c9a-4d6b-8e5f-9a2c3d4e5f6a')]",
    "type": "Microsoft.OperationalInsights/workspaces/providers/alertRules",
    "kind": "Scheduled",
    "apiVersion": "2023-12-01-preview",
    "properties": {
        "displayName": "MCN - Windows - Suspicious Plink Remote Forwarding",
        "description": "Détecte l'utilisation de Plink avec l'option -R pour créer des tunnels SSH inversés, souvent utilisés pour contourner les pare-feu et établir des canaux de communication cachés.",
        "severity": "High",
        "enabled": true,
        "query": "SecurityEvent\r\n| where EventID == 4688\r\n| where NewProcessName has_any (dynamic([\"plink.exe\", \"plink64.exe\"]))\r\n| where CommandLine has \"-R\"\r\n| where not(CommandLine has \"deeplink\")\r\n| where not(NewProcessName matches regex @\"(?i)(Teams\\.exe|TortoiseGitPlink\\.exe)\")\r\n| extend LowerCommandLine = tolower(CommandLine)\r\n| where LowerCommandLine contains \"-r\"",
        "queryFrequency": "PT5M",
        "queryPeriod": "PT5M",
        "triggerOperator": "GreaterThan",
        "triggerThreshold": 0,
        "suppressionDuration": "PT1H",
        "suppressionEnabled": false,
        "startTimeUtc": null,
        "tactics": [
            "CommandAndControl",
            "DefenseEvasion"
        ],
        "techniques": [
            "T1572",
            "T1090"
        ],
        "subTechniques": [
            "T1572.001"
        ],
        "alertRuleTemplateName": null,
        "incidentConfiguration": {
            "createIncident": true,
            "groupingConfiguration": {
                "enabled": true,
                "reopenClosedIncident": false,
                "lookbackDuration": "PT1H",
                "matchingMethod": "Selected",
                "groupByEntities": [
                    "Host",
                    "Account"
                ],
                "groupByAlertDetails": [],
                "groupByCustomDetails": []
            }
        },
        "eventGroupingSettings": {
            "aggregationKind": "AlertPerResult"
        },
        "alertDetailsOverride": null,
        "customDetails": {
            "ProcessName": "NewProcessName",
            "CommandLine": "CommandLine"
        },
        "entityMappings": [
            {
                "entityType": "Host",
                "fieldMappings": [
                    {
                        "identifier": "FullName",
                        "columnName": "Computer"
                    }
                ]
            },
            {
                "entityType": "Account",
                "fieldMappings": [
                    {
                        "identifier": "FullName",
                        "columnName": "Account"
                    }
                ]
            },
            {
                "entityType": "Process",
                "fieldMappings": [
                    {
                        "identifier": "CommandLine",
                        "columnName": "CommandLine"
                    }
                ]
            }
        ],
        "sentinelEntitiesMappings": null,
        "templateVersion": null
    }
}
```

