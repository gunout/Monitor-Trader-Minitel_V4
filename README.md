# 📊 Trading Monitor V4

<div align="center">

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-2.x-000000?style=for-the-badge&logo=flask&logoColor=white)
![Socket.IO](https://img.shields.io/badge/Socket.IO-Real--Time-010101?style=for-the-badge&logo=socket.io&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-ML-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-Scientific-013243?style=for-the-badge&logo=numpy&logoColor=white)

![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Active-success?style=for-the-badge)
![Version](https://img.shields.io/badge/Version-4.0.0-blue?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Web-informational?style=for-the-badge)
![PRs](https://img.shields.io/badge/PRs-Welcome-brightgreen?style=for-the-badge)

**Plateforme de monitoring boursier en temps réel avec intelligence artificielle intégrée**

*Indicateurs techniques • Prédictions IA • Données fondamentales • Multi-actifs*

[Fonctionnalités](#-fonctionnalités) • [Installation](#-installation) • [Utilisation](#-utilisation) • [API](#-api-rest) • [Architecture](#-architecture)

</div>

---

## 🎯 À propos

**Trading Monitor V4** est une application web complète de surveillance des marchés financiers. Elle combine :

- 📈 **Analyse technique** avancée (RSI, MACD, Bollinger, Stochastique, ATR, SMA/EMA)
- 🤖 **Intelligence Artificielle** (Random Forest entraîné sur 2 ans de données historiques)
- 📊 **Données fondamentales** (P/E, dividende, capitalisation, beta, marges)
- 🔄 **Mise à jour en temps réel** via WebSocket (Socket.IO)
- 🌍 **Multi-actifs** : Actions US/Paris, Crypto, Forex, Commodités, ETF, Indices, Space

---

## ✨ Fonctionnalités

### 🧠 Intelligence Artificielle

- Modèle **Random Forest Classifier** entraîné automatiquement sur 10 actifs majeurs
- Extraction de **11 features** techniques (volatilité, RSI, MACD, Bollinger, Stochastic, ATR, momentum…)
- Prédiction **ACHAT / NEUTRE / VENTE** avec probabilités de confiance
- Sauvegarde/chargement du modèle (`trading_model.pkl` + `scaler.pkl`)
- Route API de ré-entraînement à la demande

### 📊 Indicateurs Techniques

| Indicateur | Période | Signal |
|-----------|---------|--------|
| RSI | 14 | Survente / Surachat |
| MACD | 12/26/9 | Croisement haussier/baissier |
| SMA | 20/50/200 | Golden Cross / Death Cross |
| Bollinger | 20, 2σ | Bandes de volatilité |
| Stochastic | 14/3 | Survente / Surachat |
| ATR | 14 | Stop-loss / Take-profit |
| Momentum | 10 | Tendance |
| Volatilité annualisée | 20 | Risque |

### 💰 Données Fondamentales

P/E ratio, PEG, dividende, payout ratio, beta, EPS, marges, ROE, ROA, dette/equity, cash-flow, short ratio, 52-week high/low…

### 🖥️ Frontend

- Graphiques **candlestick** interactifs
- Panneau **signaux IA** en temps réel
- Comparateur d'actifs (jusqu'à 5)
- Top performers
- Watchlist dynamique
- Export **CSV**
- Statut du marché US (ouvert/fermé)

### 🔌 Temps Réel

- WebSocket via **Socket.IO**
- Push automatique des insights
- Cache intelligent (30 secondes)

---

## 🚀 Installation

### Prérequis

- **Python 3.10+**
- pip / virtualenv

### Cloner le projet

```bash
git clone https://github.com/gunout/Monitor-Trader-Minitel_V4.git
cd Monitor-Trader-Minitel_V4
```

### Environnement virtuel

```bash
python3 -m venv venv

# Linux / macOS
source venv/bin/activate

# Windows
venv\Scripts\activate
```

### Installer les dépendances

```bash
pip install -r requirements.txt
```

**Ou manuellement :**

```bash
pip install flask flask-cors flask-socketio yfinance pandas numpy \
            scikit-learn pytz eventlet
```

---

## ⚙️ Configuration

Le fichier principal est `app.py` (ou `#!usrbinenv python3.py`).

Modifiez si besoin :

```python
app.config['SECRET_KEY'] = 'trading-monitor-secret-key'   # Clé secrète Flask
CACHE_DURATION = 30                                        # Cache en secondes
port = 5001                                                # Port du serveur
```

---

## ▶️ Utilisation

### Lancer le serveur

```bash
python app.py
```

### Accéder à l'application

```
🌐 http://localhost:5001
```

### Au démarrage

- Le modèle IA est **chargé** s'il existe
- Sinon, il est **entraîné automatiquement** sur 2 ans de données
- Les dossiers `templates/`, `static/js/`, `static/css/` sont créés si absents

---

## 🌐 API REST

| Méthode | Endpoint | Description |
|---------|----------|-------------|
| `GET` | `/` | Interface web principale |
| `GET` | `/api/trading/<symbol>` | Données OHLCV + indicateurs (toutes périodes) |
| `GET` | `/api/fundamental/<symbol>` | Données fondamentales |
| `GET` | `/api/insights/<symbol>` | Analyse complète + signaux IA |
| `GET` | `/api/watchlist` | Watchlist avec prix temps réel |
| `GET` | `/api/top-performers` | Top 10 des performances |
| `GET` | `/api/compare?symbols=AAPL,MSFT` | Comparateur d'actifs |
| `GET` | `/api/export-csv/<symbol>` | Export CSV des données historiques |
| `GET` | `/api/market-status` | Statut du marché US |
| `GET` | `/api/train-ai` | Ré-entraîner le modèle IA |
| `GET` | `/api/clear-cache` | Vider le cache |

### 🔌 Événements WebSocket

```javascript
// Connexion
socket.on('connected', (data) => { ... });

// Demander les insights
socket.emit('request_insights', { symbol: 'AAPL' });

// Recevoir les mises à jour
socket.on('insights_update', (data) => { ... });

// Erreurs
socket.on('error', (data) => { ... });
```

---

## 🏗️ Architecture

```
Monitor-Trader-Minitel_V4/
│
├── app.py                      # Serveur Flask + logique métier
├── trading_model.pkl           # Modèle IA (généré)
├── scaler.pkl                  # Scaler IA (généré)
├── requirements.txt            # Dépendances Python
├── README.md
│
├── templates/
│   └── monitor.html            # Interface utilisateur
│
└── static/
    ├── js/                     # Scripts frontend
    └── css/                    # Styles
```

### 🔄 Flux de données

```
yfinance → Cache → Indicateurs techniques → IA Random Forest → Signaux
                ↓
         WebSocket (Socket.IO)
                ↓
         Frontend (graphiques + panneaux)
```

### 🧩 Composants clés

- **`TradingAI`** : classe gérant le modèle Random Forest
- **`calculate_all_indicators()`** : calcule tous les indicateurs techniques
- **`get_fundamental_data()`** : récupère les données fondamentales via yfinance
- **`FUNDAMENTAL_FALLBACK`** : données par défaut si l'API échoue
- **`ASSETS`** : dictionnaire des actifs disponibles

---

## 📦 Actifs supportés

| Catégorie | Exemples |
|-----------|----------|
| 🥇 **Commodités** | GLD, SLV, USO, UNG, CORN, WEAT |
| ₿ **Crypto** | BTC-USD, ETH-USD, SOL-USD, ADA-USD |
| 💱 **Forex** | EURUSD=X, GBPUSD=X, USDJPY=X |
| 🇺🇸 **US Stocks** | AAPL, MSFT, NVDA, TSLA, META |
| 🇫🇷 **Paris Stocks** | ML.PA, SAN.PA, BNP.PA, OR.PA |
| 🌊 **Eau** | PHO, FIW, CGW, AWK, XYL |
| 🌍 **BRICS & EM** | EEM, VWO, FXI, EWZ, EPI |
| 📊 **FUNDS / ETF** | SPY, QQQ, VTI, VOO, SCHD |
| 📈 **Indices** | ^GSPC, ^DJI, ^IXIC, ^FCHI |
| 🚀 **Space** | SPCE, RKLB, ASTS, PL, ARKX |

---

## 🛠️ Stack technique

| Couche | Technologie |
|--------|-------------|
| Backend | Python 3, Flask |
| Temps réel | Flask-SocketIO, Socket.IO |
| Data | yfinance, pandas, numpy |
| IA/ML | scikit-learn (Random Forest, StandardScaler) |
| Frontend | HTML5, JS, CSS |
| Cache | Dictionnaire in-memory (30s TTL) |

---

## 📊 Précision du modèle IA

Le modèle est entraîné sur **10 actifs majeurs** (AAPL, MSFT, GOOGL, NVDA, TSLA, AMZN, META, JPM, GLD, SPY) avec :

- **n_estimators** : 150
- **max_depth** : 12
- **min_samples_split** : 5
- **min_samples_leaf** : 2
- **Test size** : 20 %

La précision est loggée au démarrage et affichée via `/api/train-ai`.

---

## ⚠️ Avertissement

> **Ce projet est fourni à des fins éducatives et informatives uniquement.**
> Il ne constitue **pas un conseil en investissement**. Les performances passées ne préjugent pas des performances futures. Utilisez ces signaux avec prudence et à vos propres risques.

---

## 🤝 Contribution

Les contributions sont les bienvenues !

```bash
# Fork → Clone → Branche
git checkout -b feature/ma-fonctionnalite

# Commit
git commit -m "feat: ajout de ma fonctionnalité"

# Push
git push origin feature/ma-fonctionnalite
```

Puis ouvrez une **Pull Request**.

---

## 📝 Licence

Distribué sous licence **MIT**. Voir `LICENSE` pour plus d'informations.

---

## 👤 Auteur

**gunout**

- GitHub : [@gunout](https://github.com/gunout)

---

<div align="center">

### ⭐ Si ce projet vous plaît, n'oubliez pas de lui donner une étoile !

</div>

---

<div align="center">

### 🇫🇷 Gunout · 2026

![Made in France](https://img.shields.io/badge/Made_in-France-002395?style=flat-square&labelColor=FFFFFF&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA5MDAgNjAwIj48cmVjdCB3aWR0aD0iOTAwIiBoZWlnaHQ9IjYwMCIgZmlsbD0iIzAwMjM5NSIvPjxyZWN0IHdpZHRoPSI5MDAiIGhlaWdodD0iNDAwIiB5PSIxMDAiIGZpbGw9IiNmZmYiLz48cmVjdCB3aWR0aD0iOTAwIiBoZWlnaHQ9IjIwMCIgeT0iNDAwIiBmaWxsPSIjZWQyOTM5Ii8+PC9zdmc+)
![GitHub](https://img.shields.io/badge/GitHub-gunout-181717?style=flat-square&logo=github&logoColor=white)
![Year](https://img.shields.io/badge/2026-ED2939?style=flat-square&labelColor=FFFFFF)

<sub>© 2026 <strong>Gunout</strong> — Tous droits réservés.</sub>

</div>
