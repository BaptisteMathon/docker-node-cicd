# docker-node-cicd

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

La seconde étape est de générer les tags et labels pour notre image docker:

``` sh
- name: Extract metadata (tags, labels) for Docker
        id: meta
        uses: docker/metadata-action@9ec57ed1fcdbf14dcef7dfbe97b2010124a938b7
        with: 
          images: ${{ secrets.DOCKER_USERNAME }}/docker-node-cicd
```

Ces tags et labels serviront pour la prochaine étape qui est le build et le push de notre image:

Le build et le push se fait de la façon suivante:

``` sh
- name: Build and push Docker image
        id: push
        uses: docker/build-push-action@3b5e8027fcad23fda98b2e3ac259d8d67585f671
        with: 
          context: .
          file: ./Dockerfile
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```

Grâce à cette modification la publication de l'image Docker depuis github action est fonctionnel.

Lorsque l'on fait un push sur github, nous voyons bien que notre workflow s'est effectué sans problème:

<img width="1458" alt="Capture d’écran 2025-03-26 à 09 01 16" src="https://github.com/user-attachments/assets/8e1b46de-6471-415c-ab92-3694d002e26a" />

<img width="1458" alt="Capture d’écran 2025-03-26 à 09 03 33" src="https://github.com/user-attachments/assets/0791ab6c-b944-454f-baec-07c69d32e1db" />

