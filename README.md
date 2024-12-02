# sapsfdctab

# Étape 1 : Configuration de la VM sur GCP
Création de la VM :

Connectez-vous à votre compte GCP.
Allez dans Compute Engine > Instances VM.
Créez une nouvelle instance avec les spécifications nécessaires (mémoire et processeurs adaptés aux traitements prévus).

Équivalent code de la création de la VM sur GCP

gcloud compute instances create etl-airflow-dbt-20241202-174853 --project=sapsfdctab --zone=europe-west9-c --machine-type=e2-micro --network-interface=network-tier=PREMIUM,stack-type=IPV4_ONLY,subnet=default --maintenance-policy=MIGRATE --provisioning-model=STANDARD --service-account=725160832093-compute@developer.gserviceaccount.com --scopes=https://www.googleapis.com/auth/devstorage.read_only,https://www.googleapis.com/auth/logging.write,https://www.googleapis.com/auth/monitoring.write,https://www.googleapis.com/auth/service.management.readonly,https://www.googleapis.com/auth/servicecontrol,https://www.googleapis.com/auth/trace.append --tags=http-server,https-server --create-disk=auto-delete=yes,boot=yes,device-name=etl-airflow-dbt,image=projects/ubuntu-os-cloud/global/images/ubuntu-2204-jammy-v20241119,mode=rw,size=10,type=pd-balanced --no-shielded-secure-boot --shielded-vtpm --shielded-integrity-monitoring --labels=goog-ec-src=vm_add-gcloud --reservation-affinity=any

Connexion à la VM depuis Visual Studio Code :

Installez l’extension Remote - SSH dans Visual Studio Code.
Configurez le fichier SSH pour inclure la clé privée de votre VM.
Ouvrez un terminal dans VS Code et connectez-vous à la VM.


# Étape 2 : Installation et configuration des outils sur la VM
Python et Airflow :

Installez Python 3.9+.
Installez Airflow avec pip install apache-airflow.
Configurez Airflow pour fonctionner avec GCP en ajoutant le module GCP et en configurant votre airflow.cfg.
dbt :

Installez dbt en suivant les instructions pour dbt CLI.
Configurez un projet dbt pour se connecter à BigQuery.
Connecteurs :

Installez les connecteurs pour SAP S/4HANA et Salesforce.
Pour Salesforce : utilisez simple-salesforce ou un connecteur ETL dédié comme celui d'Airflow.
Pour SAP : installez des bibliothèques compatibles (ex. SAP NCo si nécessaire).


# Étape 3 : Configuration du Data Warehouse (BigQuery)
Création du projet GCP :

Créez un projet dédié dans GCP.
Activez l’API BigQuery.
Définition des schémas :

Identifiez les tables sources dans SAP et Salesforce.
Créez des tables dans BigQuery avec des schémas alignés.
Connexion :

Configurez Airflow pour interagir avec BigQuery via des opérateurs dédiés (BigQueryOperator).


# Étape 4 : Transformation des données avec dbt
Configuration dbt :
Créez des modèles dbt pour transformer les données après leur chargement dans BigQuery.
Créez des fichiers YAML pour définir vos sources et modèles.
Testez vos modèles avec dbt run.


# Étape 5 : Configuration de Tableau pour la BI
Connexion à BigQuery :

Dans Tableau, ajoutez une nouvelle connexion et choisissez BigQuery.
Configurez les paramètres d'authentification avec un fichier JSON de clé de service.
Création des tableaux de bord :

Créez des visualisations interactives en utilisant les données préparées par dbt.


# Étape 6 : Flux ETL avec Airflow
Planification des workflows :

Configurez des DAGs (Directed Acyclic Graphs) dans Airflow pour automatiser :
L’extraction depuis SAP et Salesforce.
Le chargement dans BigQuery.
Les transformations avec dbt.
Supervision :

Activez des alertes pour surveiller l’exécution des workflows.


# Étape 7 : Test et Déploiement
Tests locaux :

Vérifiez que chaque composant fonctionne indépendamment.
Testez les connexions et les flux de bout en bout.
Déploiement :

Configurez des environnements staging et production.
Documentez votre architecture.
