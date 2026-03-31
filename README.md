# Titanic Dashboard — Projet DevOps Data/IA

Dashboard Streamlit interactif sur le dataset Titanic avec pipeline CI/CD complet.


## Démarrage rapide (sans Docker)

### 1. Prérequis

- **Python 3.11** installé ([python.org](https://www.python.org/downloads/))
- **VS Code** ([code.visualstudio.com](https://code.visualstudio.com/))
- **Git** installé ([git-scm.com](https://git-scm.com/))

>  **Vous venez de Jupyter/Anaconda ?** Pas de panique ! Voici les équivalences :
> - Jupyter cell → fonctions Python dans des fichiers `.py`
> - `!pip install` → `pip install` dans le terminal
> - `%matplotlib inline` → les graphiques Plotly s'affichent dans le navigateur
> - Markdown dans Jupyter → `st.markdown()` dans Streamlit

### 2. Cloner le dépôt

# Dans le terminal VS Code (Ctrl+` pour ouvrir)
git clone https://github.com/VOTRE-ORG/titanic-dashboard.git
cd titanic-dashboard


### 3. Créer un environnement virtuel

# Créer l'environnement (équivalent Anaconda env)
python -m venv .venv

# Activer l'environnement
# Sur Windows :
.venv\Scripts\activate
# Sur Mac/Linux :
source .venv/bin/activate

# Vérifier que le venv est actif (vous verrez (.venv) dans le terminal)

### 4. Installer les dépendances

pip install -r requirements.txt


> Cette commande installe : `streamlit`, `pandas`, `plotly`, `seaborn`, `pytest`, etc.

### 5. Récupérer le dataset Titanic

Le dataset est chargé **automatiquement** via seaborn :

import seaborn as sns
df = sns.load_dataset('titanic')  # 891 lignes, 15 colonnes


**Pas besoin de télécharger un fichier CSV !** Seaborn le télécharge depuis GitHub au premier appel.

> Si vous êtes hors ligne, seaborn cherchera dans son cache local (~/.cache/seaborn).

### 6. Lancer l'application

streamlit run app.py


Le navigateur s'ouvre automatiquement sur **http://localhost:8501** 



## Lancer les tests

# Tous les tests avec détail
pytest tests/ -v

# Avec rapport de couverture
pytest tests/ -v --cov=data --cov-report=term-missing

Tous les tests doivent afficher `PASSED` 


## Lancer avec Docker

### Build et run manuel

# Construire l'image
docker build -t titanic-dashboard .

# Lancer le conteneur
docker run -p 8501:8501 titanic-dashboard

# Accéder à : http://localhost:8501

### Avec docker-compose (App + ELK)

# Lancer toute la stack
docker-compose up -d

# Vérifier que tout tourne
docker-compose ps

# Arrêter
docker-compose down

**URLs :**
- Dashboard : http://localhost:8501
- Kibana : http://localhost:5601
- Elasticsearch : http://localhost:9200

---

##  Workflow Git (3 membres)

### Convention de branches

main          ← branche de référence (code validé)
membre-alice  ← branche de Alice
membre-bob    ← branche de Bob
membre-carol  ← branche de Carol

### Workflow quotidien

# 1. Créer votre branche depuis main
git checkout main
git pull origin main
git checkout -b membre-VOTRE-NOM

# 2. Travailler, puis committer
git add .
git commit -m "feat: ajout de la page X"

# 3. Pousser votre branche
git push origin membre-VOTRE-NOM

# 4. Créer une Pull Request vers main sur GitHub
# Les tests CI s'exécutent automatiquement !

### Convention de commits

feat: nouvelle fonctionnalité
fix: correction de bug
test: ajout/modification de tests
docs: documentation
style: formatage, pas de changement logique
refactor: refactoring
ci: modification pipeline CI/CD

## Configuration GitHub Actions

Ajoutez ces **secrets** dans votre repo GitHub (Settings → Secrets → Actions) :

| Secret | Description |
|--------|-------------|
| `DOCKERHUB_USERNAME` | Votre nom d'utilisateur DockerHub |
| `DOCKERHUB_TOKEN` | Token d'accès DockerHub (pas votre mot de passe) |
| `NEXUS_URL` | URL de votre instance Nexus |
| `NEXUS_USER` | Utilisateur Nexus |
| `NEXUS_PASSWORD` | Mot de passe Nexus |


## Pages du Dashboard

| Page | Description |
|------|-------------|
| 📊 Vue Générale | KPIs : nb passagers, taux survie, âge moyen + 2 graphiques |
| 🏅 Analyse de Survie | Bar chart sexe, pie chart classe, histogramme âge, heatmap |
| 🎛️ Filtres Interactifs | Résultats en temps réel selon les filtres sidebar |
| 📋 Données Brutes | Tableau paginé + export CSV |

**Sidebar** : filtres par classe (1/2/3), genre, tranche d'âge — toutes les pages se mettent à jour.


## Logs utilisateur

Chaque interaction est enregistrée en JSON dans `app.log` :

{"timestamp": "2024-01-15T10:30:00Z", "event_type": "page_visit", "details": {"page": "Vue Générale"}}
{"timestamp": "2024-01-15T10:30:05Z", "event_type": "filter_apply", "details": {"classes": [1, 2], "sexes": ["female"]}}
{"timestamp": "2024-01-15T10:30:10Z", "event_type": "export_csv", "details": {"rows_exported": 245}}
```

Ces logs alimentent le dashboard Kibana via Filebeat → Elasticsearch.


## Ressources utiles

- [Documentation Streamlit](https://docs.streamlit.io)
- [Plotly Express](https://plotly.com/python/plotly-express/)
- [pytest](https://docs.pytest.org)
- [Docker](https://docs.docker.com)
- [GitHub Actions](https://docs.github.com/en/actions)