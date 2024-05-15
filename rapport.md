## Rapport sur le Lab01

### Introduction

Le projet Lab01 a été conçu pour mettre en place une machine virtuelle Ubuntu Trusty 64 en utilisant Vagrant et VirtualBox. L'objectif principal était de configurer un environnement de développement reproductible et isolé. Ce rapport documente les étapes de réalisation du projet, les problèmes rencontrés, les solutions apportées et les perspectives d'amélioration.

### Étapes de Réalisation

1. **Initialisation de Vagrant**
   - **Commande utilisée** : `vagrant init ubuntu/trusty64`
   - **Détail** :
     - Cette commande a généré un fichier `Vagrantfile` dans le répertoire de travail (`C:\Users\STUXNET\OneDrive\Bureau\lab-01`).
     - Le fichier `Vagrantfile` contient les configurations nécessaires pour la machine virtuelle, telles que le fournisseur (VirtualBox) et l'image de la boîte (Ubuntu Trusty 64).

   ```plaintext
   PS C:\Users\STUXNET\OneDrive\Bureau\lab-01> vagrant init ubuntu/trusty64
   ```

2. **Démarrage de la Machine Virtuelle**
   - **Commande utilisée** : `vagrant up`
   - **Détail** :
     - Cette commande télécharge l'image de la boîte `ubuntu/trusty64` depuis Vagrant Cloud et l'importe dans VirtualBox.
     - Les configurations réseau sont appliquées, et les ports sont redirigés (port 22 du guest redirigé vers le port 2222 de l'hôte).
     - La machine est démarrée, et une clé SSH est générée pour sécuriser la connexion.

   ```plaintext
   PS C:\Users\STUXNET\OneDrive\Bureau\lab-01> vagrant up
   Bringing machine 'default' up with 'virtualbox' provider...
   ==> default: Box 'ubuntu/trusty64' could not be found. Attempting to find and install...
       default: Box Provider: virtualbox
       default: Box Version: >= 0
   ==> default: Loading metadata for box 'ubuntu/trusty64'
       default: URL: https://vagrantcloud.com/api/v2/vagrant/ubuntu/trusty64
   ==> default: Adding box 'ubuntu/trusty64' (v20190514.0.0) for provider: virtualbox
       default: Downloading: https://vagrantcloud.com/ubuntu/boxes/trusty64/versions/20190514.0.0/providers/virtualbox/unknown/vagrant.box
   Download redirected to host: cloud-images.ubuntu.com
       default:
   ==> default: Successfully added box 'ubuntu/trusty64' (v20190514.0.0) for 'virtualbox'!
   ==> default: Importing base box 'ubuntu/trusty64'...
   ==> default: Matching MAC address for NAT networking...
   ==> default: Checking if box 'ubuntu/trusty64' version '20190514.0.0' is up to date...
   ==> default: Setting the name of the VM: lab-01_default_1715783086197_45999
   ==> default: Clearing any previously set forwarded ports...
   ==> default: Clearing any previously set network interfaces...
   ==> default: Preparing network interfaces based on configuration...
       default: Adapter 1: nat
   ==> default: Forwarding ports...
       default: 22 (guest) => 2222 (host) (adapter 1)
   ==> default: Booting VM...
   ==> default: Waiting for machine to boot. This may take a few minutes...
       default: SSH address: 127.0.0.1:2222
       default: SSH username: vagrant
       default: SSH auth method: private key
       default: Warning: Connection reset. Retrying...
       default: Warning: Connection aborted. Retrying...
       default: Vagrant insecure key detected. Vagrant will automatically replace
       default: this with a newly generated keypair for better security.
       default:
       default: Inserting generated public key within guest...
       default: Removing insecure key from the guest if it's present...
       default: Key inserted! Disconnecting and reconnecting using new SSH key...
   ==> default: Machine booted and ready!
   ==> default: Checking for guest additions in VM...
       default: The guest additions on this VM do not match the installed version of
       default: VirtualBox! In most cases this is fine, but in rare cases it can
       default: prevent things such as shared folders from working properly. If you see
       default: virtual machine match the version of VirtualBox you have installed on
       default: your host and reload your VM.
       default:
       default: Guest Additions Version: 4.3.40
       default: VirtualBox Version: 7.0
   ==> default: Mounting shared folders...
       default: /vagrant => C:/Users/STUXNET/OneDrive/Bureau/lab-01
   ```

3. **Vérification de l'État de la Machine Virtuelle**
   - **Commande utilisée** : `vagrant status`
   - **Détail** :
     - Cette commande affiche l'état actuel de la machine virtuelle, confirmant qu'elle est en cours d'exécution.

   ```plaintext
   PS C:\Users\STUXNET\OneDrive\Bureau\lab-01> vagrant status
   Current machine states:

   default                   running (virtualbox)
   ```

4. **Connexion à la Machine Virtuelle**
   - **Commande utilisée** : `vagrant ssh`
   - **Détail** :
     - Cette commande établit une connexion SSH à la machine virtuelle, permettant d'interagir directement avec le système d'exploitation invité.

   ```plaintext
   PS C:\Users\STUXNET\OneDrive\Bureau\lab-01> vagrant ssh
   Welcome to Ubuntu 14.04.6 LTS (GNU/Linux 3.13.0-170-generic x86_64)
   ```

5. **Arrêt de la Machine Virtuelle**
   - **Commande utilisée** : `vagrant halt`
   - **Détail** :
     - Cette commande arrête la machine virtuelle de manière ordonnée.

   ```plaintext
   PS C:\Users\STUXNET\OneDrive\Bureau\lab-01> vagrant halt
   ==> default: Attempting graceful shutdown of VM...
   ```

6. **Destruction de la Machine Virtuelle**
   - **Commande utilisée** : `vagrant destroy`
   - **Détail** :
     - Cette commande supprime la machine virtuelle et toutes les ressources associées après confirmation.

   ```plaintext
   PS C:\Users\STUXNET\OneDrive\Bureau\lab-01> vagrant destroy
       default: Are you sure you want to destroy the 'default' VM? [y/N] y
   ==> default: Destroying VM and associated drives...
   ```

### Problèmes Rencontrés et Solutions

1. **Téléchargement de la Boîte**
   - **Problème** : Le téléchargement initial de la boîte `ubuntu/trusty64` peut être lent en raison de la redirection vers le serveur d'images cloud.
   - **Solution** : Utilisation de miroirs locaux ou de serveurs proxy pour accélérer le téléchargement des images.

2. **Problèmes de Connexion SSH**
   - **Problème** : Des messages d'avertissement concernant la réinitialisation et l'abandon de la connexion SSH ont été observés.
   - **Solution** : Ajustement des paramètres de délai d'attente et vérification de la configuration réseau pour assurer une connexion stable.

3. **Incohérence des Additions Invitées VirtualBox**
   - **Problème** : Les Additions Invitées de la VM ne correspondaient pas à la version installée de VirtualBox, ce qui peut causer des problèmes de synchronisation des dossiers partagés.
   - **Solution** : Mettre à jour les Additions Invitées pour correspondre à la version de VirtualBox installée sur l'hôte.

### Perspectives d'Amélioration

1. **Automatisation des Tests**
   - Intégrer des scripts de test pour vérifier automatiquement l'intégrité de la machine virtuelle après son démarrage.

2. **Gestion des Versions**
   - Utiliser des outils de gestion de versions pour le `Vagrantfile` et les scripts associés afin de suivre les modifications et faciliter la collaboration.

3. **Documentation Améliorée**
   - Ajouter des commentaires détaillés dans le `Vagrantfile` et créer une documentation utilisateur complète pour guider les nouveaux utilisateurs à travers le processus de configuration et de gestion des environnements virtuels.

### Conclusion

Le projet Lab01 a permis de configurer un environnement de développement virtuel reproductible en utilisant Vagrant et VirtualBox. Malgré quelques défis techniques, les solutions appropriées ont été mises en place pour assurer le bon fonctionnement de la machine virtuelle. Les perspectives d'amélioration identifiées pourront renforcer la robustesse et la convivialité du processus de déploiement.