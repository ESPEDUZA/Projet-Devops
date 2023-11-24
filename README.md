# TP-CI-CD

## GitFlow Adopté

- main : La branche de production, contenant le code en état de déploiement.
- develop : La branche de développement, où toutes les fonctionnalités, corrections et autres branches sont fusionnées avant d'être déployées en production.
- feature/ : Des branches pour développer de nouvelles fonctionnalités. Chaque fonctionnalité a sa propre branche (par exemple, feature/nouvelle-fonctionnalité).
- release/ : Des branches pour préparer une prochaine version. Ces branches sont utilisées pour les ajustements finaux avant le déploiement sur main.


## Règles de Protection des Branches
Pour assurer la stabilité et la qualité du code, des règles de protection de branches ont été mises en place sur main :

- Les Pull Requests sont requises pour les merges.
- Les tests CI doivent passer avec succès avant tout merge.

## Version du workflow si le projet build 

```yml
name: Docker Image CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v3

    - name: Build the Docker image
      run: docker build . --file Dockerfile --tag espeduza/nangaparbat:$(date +%s)

    - name: Log in to Docker Hub
      if: github.event_name == 'push' && github.ref == 'refs/heads/main'
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKER_HUB_USERNAME }}
        password: ${{ secrets.DOCKER_HUB_ACCESS_TOKEN }}

    - name: Push the Docker image
      if: github.event_name == 'push' && github.ref == 'refs/heads/main'
      run: docker push espeduza/nangaparbat:$(date +%s)
```
