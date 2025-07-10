# Documentation FitnessConnect Kubernetes - docs.page

## 🚀 Mise en place de la documentation avec docs.page

### 1. Structure des fichiers

```
docs/
├── docs.json              # Configuration docs.page
├── index.mdx             # Page d'accueil
├── overview.mdx          # Vue d'ensemble
├── architecture.mdx      # Architecture détaillée
├── setup/
│   ├── prerequisites.mdx
│   ├── deployment.mdx
│   └── dns.mdx
├── components/
│   ├── frontend.mdx
│   ├── api.mdx
│   ├── postgres.mdx
│   ├── keycloak.mdx
│   ├── adminer.mdx
│   └── uptime-kuma.mdx
├── security/
│   ├── secrets.mdx
│   ├── network-policies.mdx
│   └── https.mdx
├── devops/
│   ├── cicd.mdx
│   ├── monitoring.mdx
│   └── scaling.mdx
├── tests/
│   ├── high-availability.mdx
│   ├── persistence.mdx
│   └── validation.mdx
├── bonus.mdx
├── troubleshooting.mdx
└── images/
    ├── dashboard.png
    ├── pods-running.png
    └── uptime-kuma.png
```

### 2. Déploiement sur GitHub

#### Option A: Repository dédié

```bash
# Créer un nouveau repository
git init fitness-k8s-docs
cd fitness-k8s-docs

# Ajouter tous les fichiers de documentation
cp -r docs/* .
git add .
git commit -m "Initial documentation"

# Pousser sur GitHub
git remote add origin https://github.com/[votre-username]/fitness-k8s-docs.git
git push -u origin main
```

#### Option B: Dans le repository existant

```bash
# Dans votre projet existant
mkdir docs
cp -r [fichiers-docs] docs/
git add docs/
git commit -m "Add docs.page documentation"
git push
```

### 3. Activation sur docs.page

1. Aller sur [https://docs.page](https://docs.page)
2. Se connecter avec GitHub
3. Sélectionner votre repository
4. La documentation sera automatiquement disponible à:
   - `https://docs.page/[username]/[repo-name]`
   - Ou avec domaine custom si configuré

### 4. Configuration du domaine personnalisé (optionnel)

Dans `docs.json`, ajouter:
```json
{
  "domain": "docs.fitnessconnect.fr"
}
```

Puis configurer un CNAME dans votre DNS:
```
docs.fitnessconnect.fr CNAME docs.page
```

### 5. Fonctionnalités docs.page utilisées

- **MDX Support**: Composants React dans la documentation
- **Syntax Highlighting**: Pour tous les blocs de code
- **Mermaid Diagrams**: Diagrammes d'architecture
- **Navigation**: Sidebar et navigation automatique
- **Search**: Recherche intégrée
- **Dark Mode**: Support automatique
- **Mobile Responsive**: Adaptatif sur tous les écrans
- **Tabs & Cards**: Pour une meilleure organisation
- **Callouts**: Pour les informations importantes
- **API Documentation**: Intégration avec Swagger/OpenAPI

### 6. Scripts de déploiement

```bash
#!/bin/bash
# deploy-docs.sh

# Build images si nécessaire
echo "📸 Taking screenshots..."
./scripts/take-screenshots.sh

# Commit et push
git add docs/
git commit -m "Update documentation"
git push

echo "✅ Documentation updated at https://docs.page/[username]/[repo]"
```

### 7. Intégration CI/CD

Ajouter dans `.gitlab-ci.yml`:

```yaml
update-docs:
  stage: deploy
  script:
    - echo "Documentation auto-updated via docs.page"
  only:
    changes:
      - docs/**/*
    refs:
      - main
```

## 📚 Pages principales à créer

### `/setup/prerequisites.mdx`
```mdx
---
title: Prérequis
description: Configuration nécessaire avant le déploiement
---

import { Callout, Code, Steps } from '@docs.page/ui';

# Prérequis

<Callout type="warning">
  Assurez-vous d'avoir tous les outils installés avant de commencer
</Callout>

<Steps>
  <Steps.Step>
    ### Installer gcloud CLI
    ```bash
    curl https://sdk.cloud.google.com | bash
    exec -l $SHELL
    gcloud init
    ```
  </Steps.Step>
  
  <Steps.Step>
    ### Installer kubectl
    ```bash
    gcloud components install kubectl
    ```
  </Steps.Step>
  
  <Steps.Step>
    ### Authentification GCP
    ```bash
    gcloud auth login
    gcloud config set project k8s-frist-test
    ```
  </Steps.Step>
</Steps>
```

### `/components/frontend.mdx`
```mdx
---
title: Frontend React
description: Configuration et déploiement du frontend
---

import { Tabs, Tab, Code } from '@docs.page/ui';

# Frontend React

## Configuration

<Tabs>
  <Tab title="Deployment">
    <Code>
    ```yaml
    apiVersion: apps/v1
    kind: Deployment
    metadata:
      name: frontend
      namespace: annual-project
    spec:
      replicas: 3
      selector:
        matchLabels:
          app: frontend
      template:
        metadata:
          labels:
            app: frontend
        spec:
          containers:
          - name: frontend
            image: europe-west1-docker.pkg.dev/k8s-frist-test/annual-project-repo/frontend:latest
            ports:
            - containerPort: 80
    ```
    </Code>
  </Tab>
  
  <Tab title="Service">
    <Code>
    ```yaml
    apiVersion: v1
    kind: Service
    metadata:
      name: frontend-service
      namespace: annual-project
    spec:
      selector:
        app: frontend
      ports:
      - port: 80
        targetPort: 80
      type: LoadBalancer
    ```
    </Code>
  </Tab>
</Tabs>

## Variables d'environnement

| Variable | Description | Valeur |
|----------|-------------|--------|
| `REACT_APP_API_URL` | URL de l'API backend | `https://api.fitnessconnect.fr` |
| `REACT_APP_KEYCLOAK_URL` | URL Keycloak | `https://login.fitnessconnect.fr` |
| `REACT_APP_KEYCLOAK_REALM` | Realm Keycloak | `MY-REALM` |
```

## 🎯 Avantages de docs.page

1. **Zéro configuration**: Fonctionne directement depuis GitHub
2. **MDX natif**: Composants React dans la documentation  
3. **Recherche intégrée**: Sans configuration supplémentaire
4. **Versioning**: Suit les branches Git
5. **Analytics**: Intégré si souhaité
6. **SEO optimisé**: Meta tags automatiques
7. **Performance**: CDN global
8. **Collaboration**: PR preview automatique

## 📝 Pour le professeur

La documentation complète est accessible à:
- **URL publique**: `https://docs.page/[username]/fitness-k8s-docs`
- **Navigation intuitive**: Sidebar avec toutes les sections
- **Recherche globale**: Pour trouver rapidement une information
- **Mode sombre**: Pour le confort de lecture
- **Export PDF**: Possible via le bouton d'impression

Cette documentation interactive permet de:
- Naviguer facilement entre les concepts
- Copier/coller les commandes
- Voir les diagrammes d'architecture
- Accéder aux exemples de code
- Comprendre chaque composant en détail