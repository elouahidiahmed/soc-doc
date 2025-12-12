# MCN - CyberARK - New local account created

**Description :**  
Détecte la création de nouveaux comptes locaux dans CyberARK PVWA. Cet événement est très rare et nécessite des investigations approfondies car la création de comptes locaux peut indiquer une tentative d'établir une persistance non autorisée ou de contourner les contrôles de sécurité existants.

**Severity :** Medium  
**Enabled :** True  
**Start Time (UTC) :** None  

**Fichier source :** `MCN - CyberARK - New local account created.json`  
**Chemin :** `Gouv Qc\MCN-MCN Projet de surveillance gouvernementale - Documents\POC Validation règles\Règles Migrées\Scandium21-main\AnalyticRules\CyberArk\MCN - CyberARK - New local account created.json`  

---

## 🎯 Tactics / Techniques

**Tactics :**
- Persistence
- PrivilegeEscalation
- DefenseEvasion

**Techniques :**
- T1136
- T1098
- T1078

**Sub-techniques :**
- T1136.001
- T1098.001
- T1078.003

---

## ⚙️ Configuration de la règle

- **Query frequency :** PT5M  
- **Query period :** PT1H  
- **Trigger operator :** GreaterThan  
- **Trigger threshold :** 0  
- **Suppression enabled :** False  
- **Suppression duration :** PT1H  

---

## 📝 Query

```kusto
CommonSecurityLog
| where DeviceVendor =~ "CyberArk"
| where DeviceProduct has_cs "Vault"
| where Activity has_cs "Add User" or Message has_cs "Add User" or DeviceAction has_cs "Add User"
| extend NewUser = coalesce(DestinationUserName, extract(@"(?i)user[:\s]+([^\s,]+)", 1, Message), extract(@"(?i)added[:\s]+([^\s,]+)", 1, Message))
| extend AdminUser = coalesce(SourceUserName, "Unknown")
| project TimeGenerated, DeviceAddress, AdminUser, NewUser, Activity, DeviceAction, Message, LogSeverity
```

---

## 🧭 Entity mappings

- **Account**
  - `Name` → `NewUser`
- **Account**
  - `Name` → `AdminUser`
- **Host**
  - `HostName` → `DeviceAddress`

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
- Account
- Host

**Group by alert details :**  
_Aucune_

**Group by custom details :**  
- NewUser

---

## 🧾 Event grouping

- **Aggregation kind :** AlertPerResult  

---

## 🏷️ Custom details

| Clé | Valeur |
|-----|--------|
| VaultAddress | DeviceAddress |
| AdminUser | AdminUser |
| NewUser | NewUser |
| ActionType | Activity |

---

## 🔧 Alert details override (brut)

```json
{}
```

---

## 🧱 Raw JSON (resource)

```json
{
    "id": "[concat(resourceId('Microsoft.OperationalInsights/workspaces/providers', parameters('workspace'), 'Microsoft.SecurityInsights'),'/alertRules/0fe2d342-1e52-4da0-a778-24d2e15fdd5f')]",
    "name": "[concat(parameters('workspace'),'/Microsoft.SecurityInsights/0fe2d342-1e52-4da0-a778-24d2e15fdd5f')]",
    "type": "Microsoft.OperationalInsights/workspaces/providers/alertRules",
    "kind": "Scheduled",
    "apiVersion": "2023-12-01-preview",
    "properties": {
        "displayName": "MCN - CyberARK - New local account created",
        "description": "Détecte la création de nouveaux comptes locaux dans CyberARK PVWA. Cet événement est très rare et nécessite des investigations approfondies car la création de comptes locaux peut indiquer une tentative d'établir une persistance non autorisée ou de contourner les contrôles de sécurité existants.",
        "severity": "Medium",
        "enabled": true,
        "query": "CommonSecurityLog\n| where DeviceVendor =~ \"CyberArk\"\n| where DeviceProduct has_cs \"Vault\"\n| where Activity has_cs \"Add User\" or Message has_cs \"Add User\" or DeviceAction has_cs \"Add User\"\n| extend NewUser = coalesce(DestinationUserName, extract(@\"(?i)user[:\\s]+([^\\s,]+)\", 1, Message), extract(@\"(?i)added[:\\s]+([^\\s,]+)\", 1, Message))\n| extend AdminUser = coalesce(SourceUserName, \"Unknown\")\n| project TimeGenerated, DeviceAddress, AdminUser, NewUser, Activity, DeviceAction, Message, LogSeverity",
        "queryFrequency": "PT5M",
        "queryPeriod": "PT1H",
        "triggerOperator": "GreaterThan",
        "triggerThreshold": 0,
        "suppressionDuration": "PT1H",
        "suppressionEnabled": false,
        "startTimeUtc": null,
        "tactics": [
            "Persistence",
            "PrivilegeEscalation",
            "DefenseEvasion"
        ],
        "techniques": [
            "T1136",
            "T1098",
            "T1078"
        ],
        "subTechniques": [
            "T1136.001",
            "T1098.001",
            "T1078.003"
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
                    "Account",
                    "Host"
                ],
                "groupByAlertDetails": [],
                "groupByCustomDetails": [
                    "NewUser"
                ]
            }
        },
        "eventGroupingSettings": {
            "aggregationKind": "AlertPerResult"
        },
        "alertDetailsOverride": null,
        "customDetails": {
            "VaultAddress": "DeviceAddress",
            "AdminUser": "AdminUser",
            "NewUser": "NewUser",
            "ActionType": "Activity"
        },
        "entityMappings": [
            {
                "entityType": "Account",
                "fieldMappings": [
                    {
                        "identifier": "Name",
                        "columnName": "NewUser"
                    }
                ]
            },
            {
                "entityType": "Account",
                "fieldMappings": [
                    {
                        "identifier": "Name",
                        "columnName": "AdminUser"
                    }
                ]
            },
            {
                "entityType": "Host",
                "fieldMappings": [
                    {
                        "identifier": "HostName",
                        "columnName": "DeviceAddress"
                    }
                ]
            }
        ],
        "sentinelEntitiesMappings": null,
        "templateVersion": null
    }
}
```

