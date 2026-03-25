# 🎛️ ZabbixTune — Zabbix Server Config Simulator

Simulateur interactif de configuration `zabbix_server.conf` — génère une config optimisée selon ton infrastructure (hosts, items, proxies, BDD).

![HTML](https://img.shields.io/badge/HTML-single--file-orange?logo=html5&logoColor=white)
![Zabbix](https://img.shields.io/badge/Zabbix-7.0%20%2F%207.4-red?logo=zabbix&logoColor=white)
![License](https://img.shields.io/badge/license-MIT-green)
![Deploy](https://img.shields.io/badge/deploy-Vercel-black?logo=vercel&logoColor=white)
![No deps](https://img.shields.io/badge/dependencies-none-brightgreen)

---

## 🚀 Demo

👉 **[zabbixtune.vercel.app](https://zabbixtune.vercel.app)**

---

## ✨ Fonctionnalités

- **5 profils prédéfinis** — Lab / Small / Medium / Large / XLarge
- **Support proxy actif/passif** — calcul StartTrappers / StartProxyPollers automatique
- **3 backends BDD** — MySQL/MariaDB, PostgreSQL, PostgreSQL + TimescaleDB
- **Calcul NVPS** — répartition collecte directe vs ingestion depuis proxies
- **Sizing caches** — CacheSize, HistoryCacheSize, ValueCacheSize, TrendCacheSize...
- **Alertes contextuelles** — RAM insuffisante, partitionnement requis, bonne pratiques proxy
- **Export `.conf`** prêt à coller sur le serveur
- **Copie diff** — uniquement les variables modifiées par rapport aux défauts
- Zéro dépendance — un seul fichier HTML

---

## 🧱 Stack

| Composant | Détail |
|-----------|--------|
| Frontend | HTML / CSS / JS vanilla |
| Fonts | IBM Plex Mono + IBM Plex Sans (Google Fonts) |
| Hébergement | Vercel (Hobby — statique) |
| Zabbix cible | 7.0 / 7.4 |
| BDD supportées | MySQL/MariaDB · PostgreSQL · PostgreSQL + TimescaleDB |

---

## 🖥️ Utilisation locale

Aucune installation requise. Ouvrir directement dans le navigateur :

```bash
# Cloner le repo
git clone https://github.com/Bisuuuu/zabbixtune.git
cd zabbixtune

# Ouvrir dans le navigateur
open index.html        # macOS
xdg-open index.html    # Linux
start index.html       # Windows
```

---

## ⚙️ Paramètres simulés

| Paramètre | Rôle |
|-----------|------|
| `StartPollers` | Basé sur hosts backend directs |
| `StartTrappers` | Calculé selon nb proxies actifs × 2 |
| `StartPreprocessors` | Basé sur NVPS total ingéré ≥ CPU cores |
| `StartDBSyncers` | Adapté au NVPS ingéré en BDD |
| `CacheSize` | Contient toute la config (hosts + items) |
| `HistoryCacheSize` | Buffer write avant flush BDD |
| `ValueCacheSize` | Cache lecture historique — critique si plein |
| `TrendCacheSize` / `TrendFunctionCacheSize` | Tendances + fonctions trend |
| `StartProxyPollers` | Proxies passifs uniquement |
| `StartPingers` | ICMP — minimum maintenu même avec proxies |

---

## 📊 Métriques Zabbix à surveiller après déploiement

```
zabbix[wcache,history,pfree]    → % libre HistoryCacheSize   (cible > 50%)
zabbix[wcache,trend,pfree]      → % libre TrendCacheSize
zabbix[rcache,buffer,pfree]     → % libre CacheSize          (cible > 40%)
zabbix[vcache,buffer,pfree]     → % libre ValueCacheSize
zabbix[vcache,cache,mode]       → 0 = normal  |  1 = LOW MEMORY CRITIQUE
zabbix[process,<type>,avg,busy] → % busy par type de processus
zabbix[vps,written]             → NVPS réels écrits en BDD
```

---

## 📁 Structure

```
zabbixtune/
└── index.html    # Application complète — single file
```

---

## 🔗 Projets liés

- [ansible-zabbix-deploy](https://github.com/Bisuuuu/ansible-zabbix-deploy) — Déploiement automatisé Zabbix 7.x via Ansible (AlmaLinux 9)
