# Serveur DNS de Laboratoire avec Docker et Technitium

Ce projet met à disposition un serveur DNS complet et simple à gérer, destiné à un environnement de laboratoire pédagogique. Il est basé sur [Technitium DNS Server](https://technitium.com/dns/) et déployé via Docker pour une installation rapide et isolée.

## Contexte

Dans le cadre d'activités pédagogiques, les étudiants ont régulièrement besoin d'un service DNS fonctionnel pour, par exemple :

  - Générer des certificats TLS pour leurs serveurs web.
  - Mettre en place des services qui requièrent une résolution de noms (ex: API, bases de données).
  - Comprendre les relations entre les différents types d'enregistrements DNS (`A`, `CNAME`, `MX`, `NS`).

Ce projet permet de déployer un serveur DNS centralisé pour tout le laboratoire, où les étudiants peuvent gérer leurs propres domaines en toute autonomie via une interface web, sans que l'installation et la configuration d'un serveur DNS (comme BIND9) ne soient un prérequis ou un objectif de l'activité.

## Solution Technique

  - **Serveur DNS** : **Technitium DNS Server**, pour son interface web complète et sa simplicité d'utilisation.
  - **Conteneurisation** : **Docker** et **Docker Compose**, pour un déploiement rapide, reproductible et isolé du reste du système.

## Prérequis

  - [Docker](https://docs.docker.com/get-docker/) installé sur le serveur hôte.
  - [Docker Compose](https://docs.docker.com/compose/install/) installé.

## Installation et Déploiement

1.  Clonez ce projet :
    ```bash
    git clone https://github.com/manastria/docker-dns.git
    ````
    
2.  Lancez le serveur en arrière-plan depuis le répertoire `dns-lab` :

    ```bash
    docker compose up -d
    ```

3.  Vérifiez que le conteneur est bien en cours d'exécution :

    ```bash
    docker compose ps
    ```

## Première Configuration

Une fois le serveur démarré, une configuration initiale via l'interface web est nécessaire.

1.  Ouvrez un navigateur et accédez à l'interface d'administration : `http://<IP_DU_SERVEUR>:5380`.
2.  Un assistant de configuration vous demandera de **définir un mot de passe administrateur**. Choisissez-en un et conservez-le.
3.  **(Recommandé)** Configurez des résolveurs DNS ("Forwarders") pour que votre serveur puisse résoudre les adresses Internet publiques.
      - Allez dans **Settings \> Forwarders**.
      - Ajoutez des serveurs DNS publics fiables. Exemples :
          - **Cloudflare** : `1.1.1.1` et `1.0.0.1`
          - **Quad9** : `9.9.9.9` et `149.112.112.112`
          - **Google** : `8.8.8.8` et `8.8.4.4`
      - Cliquez sur **Save**.

## Utilisation par les Étudiants

1.  Communiquez l'URL (`http://<IP_DU_SERVEUR>:5380`) et le mot de passe administrateur aux étudiants (`admin/netlab123`).
2.  Les étudiants peuvent se connecter et créer leurs propres zones DNS dans la section **Zones**.
3.  Pour chaque zone (ex: `projet-kevin.lab`), ils peuvent ajouter tous les enregistrements nécessaires (`A`, `AAAA`, `CNAME`, `TXT`, etc.), y compris des `NS` et `Glue Records` pour déléguer des sous-domaines.

## Persistance des Données

La configuration complète du serveur (zones, paramètres, etc.) est stockée dans le dossier `dns-config` sur la machine hôte, grâce au volume Docker.

  - **Sauvegarde** : Pour sauvegarder la configuration du serveur DNS, il suffit de copier le contenu du dossier `dns-config`.
  - **Mise à jour** : La configuration est conservée même si vous mettez à jour l'image Docker ou si vous redémarrez le conteneur.

## Commandes Utiles

```bash
# Démarrer le service
docker-compose up -d

# Arrêter le service
docker-compose down

# Consulter les logs en temps réel
docker-compose logs -f

# Lister les conteneurs en cours d'exécution
docker-compose ps
```


