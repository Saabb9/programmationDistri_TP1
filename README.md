## Installation de Minikube

https://minikube.sigs.k8s.io/docs/start/

Minikube propose un tableau de bord (portail web). Accédez-y à l'aide de la commande suivante :
```
minikube dashboard
```

## Télécharger ce projet

Ce projet contient un service web codé en Java Spring Boot. Ce projet a déjà été compilé et la version binaire est disponible.

Clonez le projet :
`git clone https://github.com/charroux/kubernetes-minikube`

Ensuite, accédez au répertoire `cd kubernetes-minikube/MyService` où se trouve le fichier Dockerfile.

## Testez ce projet avec Docker

Créer l'image Docker :
```
docker build -t myservice .
```

Consultez l'image :
```
docker images
```

Démarrer le conteneur :
```
docker run -p 4000:8080 -t myservice
```

Le port 8080 est celui du service web, tandis que le port 4000 permet d'accéder au conteneur.
Ctrl+C pour arrêter le service web.

Vérifiez l'ID du conteneur :
```
docker ps
```

Arrêter le conteneur :
```
docker stop containerID
```

## Publiez l'image sur Docker Hub

Étiquetez l'image Docker :
```
docker tag imageID sabrinabela/myservice:latest
```

Exemple : `docker tag d5264707930a sabrinabela/myservice:latest`

Connectez-vous à Docker Hub :
```
docker login
```

Envoyer l'image vers Docker Hub :
```
docker push sabrinabela/myservice:latest
```

## Créer un déploiement Kubernetes à partir d'une image Docker
```
kubectl get nodes
```
```
kubectl create deployment myservice --image=sabrinabela/myservice:latest
```

L'image utilisée provient de Docker Hub : https://hub.docker.com/r/sabrinabela/myservice

Vérifiez le pod :
```
kubectl get pods
```

Vérifiez si l'état est en cours d'exécution.

Obtenir les journaux complets d'un pod :
```
kubectl describe pods
```

Récupérez l'adresse IP, mais notez que cette adresse IP est éphémère puisqu'un pod peut être supprimé et remplacé par un nouveau.

Ensuite, récupérez le déploiement dans le tableau de bord minikube. En réalité, le conteneur Docker s'exécute dans un pod Kubernetes (consultez le pod dans le tableau de bord).

Vous pouvez également entrer dans le conteneur en mode interactif avec :
```
kubectl exec -it myservice-5f7bbd49f6-wh6k9 -- /bin/bash
```

où podname est le nom des pods obtenus avec :
```
kubectl get pods
```

Énumérez le contenu du conteneur avec :
```
ls
```

N'oubliez pas de quitter le conteneur avec :
```
exit
```
