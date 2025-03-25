﻿# docker-node-cicd

Afin d'ajouter la publication de l'image docker dans github actions, il m'a suffit d'ajouter quelques lignes au fichier cicd.yml se trouvant dans le dossier .github/workflow.

La première étape fut d'ajouter un nouveau step permettant de se connecter à Hub Docker
``` sh
- name: Log in to Docker Hub
        uses: docker/login-action@f4ef78c080cd8ba55a85445d5b36e214a81df20a
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_TOKEN }}
```

afin d'ajouter mes identifiants : username (DOCKER_USERNAME) et password (DOCKER_TOKEN), j'ai du récupérer mon username Docker ainsi que de créer un nouveau access token.
Pour se faire je suis aller dans mon profil docker, ensuite dans l'onglet 'Personal access token', je me suis généré un nouveau token en lui donnant les permissions suivantes 'Read Write et Delete'

Ensuite sur mon répo github, je suis aller dans la partie 'Settings' puis dans l'onglet 'Secrets and variables' (Actions) j'ai ajouté mes repository secret (DOCKER_USERNAME et DOCKER_TOKEN)
