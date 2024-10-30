# Bonne pratiques du DevOps

## A quoi sert un OS ?

- Exécuter des processus

## Comment 2 process communiquent entre eux ?

- Signaux (Par exemple le ctrl c pour interrompre est le signal 2)
- Fichiers sockets (Trés utilisés pour la com rx)
- Mémoire partagée (Mémoire partagées par pls process)

## C’est quoi un shell ?

- Programe qui permet de choisir quel programe on veut lancer
- Les differentes falvour de sh sont du même types que les différents navigateurs

## Propriétées d’un process

- Un PID -> Identifiant unique du process
- PPID -> PID du process parent (process qui l’a créer)
- PID 1 -> Processus init -> Systemd
- UID -> User id
- GID: Group id -> Ne sert que pour les perms des fichiers qu’il créer
- Tout les fichiers sont créer à partir d’un fichier (par exemple /bin/bash)
- CWD -> Current working directory -> Répertoire sur lequel le process travail
- RTD -> Root directory -> Il indique au process sont répertoire racine

## Qu-est-ce qu’un user?

- Un user n’existe pas tant qu’il n’est pas lié à un fichier ou process
- Il s’agit SEULEMENT d’une propriété de ces derniers

## Comment on créer un process

- Je suis en train de faire un ls dans le bash
- Mon bash lance le syscall fork
- Il se met en sleep tant que le process enfant tourne
- Le process enfant modifie le chemin du fichier qu’il éxécute de /bin/bash à

## A quoi sert lsof

- Avec le -i permet de savoir quell process écoute les ports
- Avec le –p et le numéro de process il permet de lister tout les fichiers lu par ce process
- Sans flag il permet de savoir qui à un accés en lecture sur un fichier

## C’est quoi TTY

- Teletype -> Periphérique qui ressenvke bcp à une machine à écrire
- Il y a un clavier et une imprimente qui imprime ce qui à été tapper sur les autre télétypes du rx
- Des télétypes servaient aussi à communiquer avec les ordis sur lesquels ont été coder unix
- Un exemple de teletype est le ASR 33.
- Le combo écran / clavier souris en est le sucesseur (Il y avait des sortes d’ordi qui pouvaient seulement afficher du txt -> P)

## C’est quoi le CHROOT

- Appel system / commande qui permet d’enfermer un process d’en une partie de l’arboressence du filesystem
- Il change le rtd du process
- Les dépendances du process doivent être présentes dans le répertoire dans lequel on veux encapsuler le process.
- Le chroot n’isole que au niveau des fichier pas du tout niveau au niveau ressources et processs

## Commande lld

- Permet de lister les dépendances d’une commande
- Ne pas la faire sur un fichier potentielement vérolller
- 2 types de bibliotèques (les 2 sont listées par la commande)
  - Statique -> est ajouté dans le binaire
  - Dynamique -> est référencée dans le binaire mais doit être présente dans le file system. Les bibliotèques dynamiques ont un “=>” qui pointe vers leur chemin
- Il y a aussi une autrte dépense nomée loader qui permet de décrire au noyaux comment faire pour créer le process. C’est lui qui charge le binaire et ses dépendances en mémoire. Il y UN SEUL loader par os normalement (il peut y avoir un loader pour le 32bits et ou pour le 64)

## C’est quoi le loader

- C’est un fichier qui permet le lancer les binaires
- Il charge les dépendances en mémoire
- Il permet d’executer des fichier même si on a pas les droits d’exec si on a les droits de lecture

## Commande reset

- Permet de reset la config du terminal (utile si le shell à été pété par un cat de bunaire par exemple).
- Même si l’affichage est fucké les caractéres sont bien pris en compte

## Commande du -sh folder

 Permet de faire la somme de la taille de tous les fichiers d’un folders

## Debootstrap

- Premet de télécharger dans un répertoire les fichiers de base d’une distrib
- Cumulé avec un chroot ça peu “Simuler” d’avoir une VM
  - LE CHROOT N’ISOLE PAS LE RX
  - LE CHROOT N’ISOLE PAS LES PROCESS
  - Le CHROOT N’ISOLE PAS LES RESSOURCES

## Différence entre su et su –

- su tout seul ne change que le user mais ne met pas à jours certaine chose comme les variables d’env comme le path
- su – recharge complétmement le profil dont les variables d’env

## Jails

- Permet une meilleur isolation
- Pas seulement au niveau du filesystem
- Tourne comme un service
- Compliqué à utiliser sur le long terme

## Virtualisation vs emulation

- Virtualisation -> Les instruction CPU de la VM sont directement transmisent au proc de la machine physique
- Emulation -> Les instrution CPU doivent être réécrites pour être exécuter par la machine physique (Par exemple si la machine virtuelle est en intel alors que la machine physique est en apple silicon)

## Solaris Zones

- Un peu comme les jails sous BSD
- Créer par Sun microsystems
- Isolation dans le noyaux
- Faire des polls de ressources
- 8192 zones max
- 3 types de zone
  - Sparse:
    - Vient des sparse file de Unix (littéralement fichiers épapillés)
      - => Fichier dont l'espace disque n'est pas alloué à la création mais plus tard à l'écriture
    - Va utiliser les fichiers du système hôte en read only et ne stockera que les écritures.
    - Cela permet de créer des zones très rapidement
  - Big -> Prend tout l’espace disque à la création
  - Branded zone -> Zone qui a un OS différent de celui du système hôte
- Permet de faire des sortes de microservices

## OpenVZ / Virtuozzo

- OpenVZ -> Versio open source de virtuozzo
- Permet d'isoler au nuveau du noyau
- Permet de faire des environenements virtuels
- Isolation en utilisant des namespaces
- Isolation des fichier comme le chroot mais avec une isolation du /sys et du /proc en plus (Le chroot n'isole que le /)
- Chaque VE à sa propre liste de users / admins
- Chaque VE à son propre init (processus 1)
- Chaque VE à sa propre carte réseau virtualisée
- Chaque VE à ses propres mécanismes de communication inter processus
- Cheque VE à son propre quota
- Chaque VE à sa propre quantité de CPU allouée (Max % cpu alloué)
- Possibilité de faire en sorte que un périphérique soit visible que par un conteneur mais pas par un autre ou la machine hôte.
- Possibilé de faire des clusters (et aussi de passer un VE d'une machine à une autre à chaud)

## Cgroups

- Gestion des ressources
- Permet de limiter les ressources d'un processus
- Namespaces -> Isolation des processus
- Pas vraiment des containers mais plutôt des groupes de processus
- Permet de faire de la priorisation de processus

## Systemd-nspawn

- Permet de faire des containers sans le problème du double fork
- Permet de faire des containers avec un init propre au conteneur
