# st2scl

## Car rental service

https://github.com/charroux/st2scl/tree/main/rentalService

```
curl --header "Content-Type: application/json" --request GET http://localhost:8080/cars
```

```
curl --header "Content-Type: application/json" --request PUT --data '{"begin":"4/11/2024","end":"20/11/2024"}' 'http://localhost:8080/cars/11AA22?rent=true'
```

### Launch a workflow when the code is updated

The script is there: https://github.com/charroux/st2scl/blob/main/.github/workflows/acttions.yml

Create a new branch:
```
git branch newcarservice
```
Move to the new branch:
```
git checkout newcarservice
```
Update the code and commit changes:
```
git commit -a -m "newcarservice"
```
Move to the main branch:
```
git checkout main
```
Push the changes to GitHub:
```
git push -u origin newcarservice
```
Create a Pull request on GitHub and follow the workflow.

Merge the branch on Github is everything is OK.

Then pull the new main version:

```
git checkout main
```
```
git pull origin main
```

If necessary delete the branch:

```
git branch -D newcarservice
```
```
git push origin --delete newcarservice
```

### Docker

Create a Dockerfile in the code folder: https://github.com/charroux/st2scl/blob/main/rentalService/Dockerfile

Build a Docker image:
```
docker build -t rentalservice .      
```
Run the container:
```
docker run -p 4000:8080 rentalservice    
```
Then check in your browser:
```
http://localhost:4000/cars
```

### Publish the Docker image to the Docker Hub

Tager l'image :
```
docker tag 4da2332370c7 votreIdDocherHub/rental:1
```
où le numéro est l'identifiant de l'image donné par docker images, et 1 est un numéro de version

Se connecter au Docker Hub : 
```
docker login
```

Publier l'image :
```
docker push votreIdDocherHub/rental:1      
```

### Installer Minikube

https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Fx86-64%2Fstable%2Fbinary+download

### Démarrer Minikube
```
minikube start --driver=docker      
```

Combien de noeuds dans le cluster?
```
kubectl get nodes      
```
Désployer votre image Docker :
```
kubectl create deployment rentalservice --image=charroux/rentalservice:1      
```
Attention d'utiliser votre image.

Vérifier que le procesus fonctionne bien :
```
kubectl get pods      
```
Scale :
```
kubectl scale --replicas=2 deployment/rentalservice          
```     

Tuer un pod pour constater son redémarrage:
```
kubectl delete pod rentalservice-5b746d6f65-t5m8v               
```    
Ajouter un load balancer :
```
kubectl expose deployment rentalservice --type=LoadBalancer              
```    
Récupérer l'adresse du service :
```
minikube service rentalservice --url                      
```    
Tester dans votre navigateur :

http://127.0.0.1:50784/cars

En adaptant l'URL.
