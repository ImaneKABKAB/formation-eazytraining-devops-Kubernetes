#  🧾Project theme
-A travers ce projet , on vise à réaliser un cluster K8s mono-node pour déployer une application 2 tiers avec wordpress et mysql .

-On doit :

✔ Assurer la communication entre les 2 tiers à travers des services.

 ✔ Assurer la persistance des données du cluster.
 
 ✔ Sécuriser les données avec les objets secrets de K8s.

-Le schéma suivant présente le cluster :

# Isolation d'environnement de travail
-Pour réaliser ce projet, on doit isoler l'environnement de travail autrement dit, on doit créer un objet namespace rassemblant tous les ressources K8s indispensables au projet .

-Pour cela, on crée un namespace nommé **wordpress-mysql** avec le manifest [wordpress-namespace.yml](https://github.com/ImaneKABKAB/formation-eazytraining-devops-Kubernetes/blob/main/mini-project/wordpress-namespace.yml).

# 🔒Création de secrets:
-Pour utiliser les images docker de wordpress et mysql , on doit fournir des variables d'environnement telles que le **WORDPRESS_DB_PASSWORD** et **MYSQL_ROOT_PASSWORD** etc .

-On ne doit pas mettre ce type de variables d'env en clair dans les manifest d'où l'utilisation de l'objet K8s secret.

-On crée l'objet secret avec le manifest [wordpress-secret.yml](https://github.com/ImaneKABKAB/formation-eazytraining-devops-Kubernetes/blob/main/mini-project/wordpress-secret.yml).

# Déployer une base de données mysql
-Dans ce projet , on déploie un seul réplicas mysql  avec le manifest [mysql-deployment.yml](https://github.com/ImaneKABKAB/formation-eazytraining-devops-Kubernetes/blob/main/mini-project/mysql-deployment.yml) .

-La stratégie de mise à jour implémentée est la stratégie **Recreate**.

  Elle sert à supprimer le pod et le recréer en conformité avec les changements au niveau du manifest .
  
# Déployer wordpress:
-Dans ce projet , on déploie un seul réplicas wordpress  avec le manifest [wordpress-deployment.yml](https://github.com/ImaneKABKAB/formation-eazytraining-devops-Kubernetes/blob/main/mini-project/wordpress-deployment.yml).

-On utilise la stratégie  de type **Recreate** pour les mises à jour .

# Connectivité et disponibilité des pods
-Après avoir crée les déploiements, on doit connecter les pods et rendre le cluster accessible à l'utilisateur .

-Pour cette raison, on peuple le cluster avec 2 services:

 - Service **ClusterIP**: 
   
 1. Pour exposer le pod mysql à l'intérieur du cluster 
 2. Rendre le pod joignable à travers le port 3306 du service dirigeant vers le port 3306 du pod mysql .
 
 Voici une description du service implémenté:
 
![private](https://github.com/ImaneKABKAB/formation-eazytraining-devops-Kubernetes/blob/main/mini-project/image/word9.png)

 Au niveau du champs **Endpoint** ,on voit bien que le service détecte l'@ IP du pod .
 
 - Service **NodePort**:
 
  1. Pour exposer le pod wordpress à l'extérieur du cluster.
  2. Rendre le pod accessible à l'utilisateur à travers le port 30008 dirigeant vers le port 80 du pod wordpress.
  
Voici une description du service implémenté:

 ![private](https://github.com/ImaneKABKAB/formation-eazytraining-devops-Kubernetes/blob/main/mini-project/image/word8.png)
 
Au niveau du champs **Endpoint** ,on voit bien que le service détecte l'@ IP du pod .


# Persistance des données
-On utilise un stockage local avec des volumes crées au niveau du node: **/data/mysql** et **/data/wordpress** .

-On monte les 2 volumes respectivement aux paths : **/var/lib/mysql** et **/var/www/html** aux niveau des conteneurs .

-On voit bien que les données sont stockées dans les volumes crées :

![private](https://github.com/ImaneKABKAB/formation-eazytraining-devops-Kubernetes/blob/main/mini-project/image/word10.png)


![private](https://github.com/ImaneKABKAB/formation-eazytraining-devops-Kubernetes/blob/main/mini-project/image/word11.png)



#  Test et résultat
-Pour tester , on se rend à l'@ IP du nœud et on précise le port 3OOO8.

![private](https://github.com/ImaneKABKAB/formation-eazytraining-devops-Kubernetes/blob/main/mini-project/image/word1.PNG)


![private](https://github.com/ImaneKABKAB/formation-eazytraining-devops-Kubernetes/blob/main/mini-project/image/word3.PNG)


![private](https://github.com/ImaneKABKAB/formation-eazytraining-devops-Kubernetes/blob/main/mini-project/image/word5.PNG)
