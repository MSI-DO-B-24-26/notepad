[]{#anchor}Les Bases des Conteneurs et de linux

[]{#anchor-1}Introduction

**Sujet :** Les bases des conteneurs et de linux\
**Professeur :** Yves Rougy\
**Chaîne YouTube :** yvesrougyFR

Ce rapport présente les notes du cours sur les bases des conteneurs,
organisées de manière plus lisible, avec des détails ajoutés et des
erreurs corrigées. L\'objectif est de fournir une compréhension
approfondie des concepts clés liés aux conteneurs, aux processus, aux
systèmes d\'exploitation et aux mécanismes de sécurité associés.

[]{#anchor-2}1. Drivers et Sécurité Système

[]{#anchor-3}1.1 Rôle des Drivers

Un **driver** (ou pilote) est un programme qui sert d\'interface entre
le système d\'exploitation (noyau) et le matériel. Il agit comme un
traducteur, permettant au système d\'exploitation de communiquer avec
les périphériques matériels tels que les claviers, les souris, les
imprimantes, etc.

[]{#anchor-4}1.2 Antivirus et Anti-Cheat

Les logiciels **antivirus** et les systèmes **anti-triche** s\'exécutent
souvent au niveau de la couche des drivers. En opérant à ce niveau bas,
ils peuvent surveiller et contrôler les opérations système critiques
pour détecter des activités malveillantes ou non autorisées.

Cependant, cette position privilégiée en fait une cible favorite pour
les hackers. Si un hacker parvient à compromettre un driver ou un
logiciel s\'exécutant à ce niveau, il peut potentiellement obtenir un
accès complet au système.

[]{#anchor-5}1.3 Vulnérabilités et Crashs Systèmes

Tout programme qui s\'exécute dans l\'espace des drivers et qui plante
peut entraîner le crash complet de la machine. Étant donné que les
drivers interagissent directement avec le matériel et le noyau, une
erreur non gérée peut causer des dysfonctionnements critiques, d\'où
l\'importance de la stabilité et de la sécurité des drivers.

[]{#anchor-6}2. Le Rôle du Système d\'Exploitation

Le système d\'exploitation (OS) a pour rôle principal d\'exécuter et de
gérer les processus. Il fournit un environnement où les applications
peuvent fonctionner, gère les ressources matérielles et assure la
communication entre les différents processus.

[]{#anchor-7}2.1 Propriétés des Processus

Chaque processus possède plusieurs propriétés essentielles :

-   **PID (Process ID)** : Identifiant unique du processus.

-   **PPID (Parent Process ID)** : Identifiant du processus parent.

-   **UID (User ID)** : Identifiant de l\'utilisateur propriétaire du
    > processus.

-   **GID (Group ID)** : Identifiant du groupe associé au processus.

-   **CWD (Current Working Directory)** : Répertoire de travail courant
    > du processus.

-   **RTD (Root Directory)** : Répertoire racine du processus, indiquant
    > la racine du système de fichiers pour ce processus.

[]{#anchor-8}2.2 Communication Inter-Processus (IPC)

Pour permettre aux processus de communiquer entre eux, on utilise des
mécanismes d\'**Inter-Process Communication (IPC)**. Les méthodes les
plus courantes sont :

-   **Signaux** : Mécanismes simples pour envoyer des notifications ou
    > des commandes entre processus.

-   **Pipes et Sockets** : Fichiers spéciaux permettant le transfert de
    > données entre processus.

-   **Mémoire partagée** : Segments de mémoire accessibles par plusieurs
    > processus pour un échange rapide de données.

[]{#anchor-9}3. Shells et Terminaux

[]{#anchor-10}3.1 Qu\'est-ce qu\'un Shell ?

Un **shell** est une interface qui permet aux utilisateurs d\'interagir
avec le système d\'exploitation. Il interprète les commandes entrées par
l\'utilisateur et exécute les programmes correspondants. Le shell est un
processus en cours d\'exécution qui sert d\'interface entre
l\'utilisateur et le noyau.

**Bash** (Bourne Again SHell) est un exemple courant de shell sous Linux
et macOS.

[]{#anchor-11}3.2 Rôle du Shell

Le shell sert principalement à :

-   Recevoir les commandes de l\'utilisateur.

-   Interpréter et exécuter ces commandes.

-   Gérer les entrées et sorties des processus.

-   Fournir des fonctionnalités de scripting pour automatiser des
    > tâches.

[]{#anchor-12}3.3 Terminaux et Émulateurs de Terminaux

Un **terminal** est historiquement un dispositif matériel permettant
d\'interagir avec un ordinateur (comme un teletype). De nos jours, le
terme désigne souvent un **émulateur de terminal**, qui est un logiciel
reproduisant le comportement d\'un terminal matériel.

Les émulateurs de terminaux écrivent sur le **port série** (ou son
équivalent virtuel), et le noyau lit depuis ce port. Les fichiers
spéciaux comme /dev/pts/1 représentent ces canaux de communication.

La commande tty permet d\'afficher le fichier de périphérique du
terminal actuel.

[]{#anchor-13}4. Processus en Profondeur

[]{#anchor-14}4.1 Création de Processus avec fork()

La création de nouveaux processus s\'effectue via l\'appel système
fork(). Cet appel duplique le processus appelant, créant ainsi un
processus enfant.

-   Le processus enfant est une copie presque identique du parent.

-   Le **PID** du processus enfant est différent, mais il hérite du
    > **PPID** du parent.

-   Le parent peut choisir d\'attendre (wait()) que l\'enfant se termine
    > ou continuer son exécution en parallèle.

[]{#anchor-15}4.2 Exécution de Programmes avec exec()

Après un fork(), le processus enfant peut remplacer son image mémoire
par un nouveau programme en utilisant l\'appel système exec(). Par
exemple, le shell peut forker puis exécuter la commande ls dans le
processus enfant.

[]{#anchor-16}4.3 Terminaison des Processus et Processus Zombies

Lorsque le processus enfant se termine, il appelle exit(). Le système
conserve des informations sur ce processus jusqu\'à ce que le parent
récupère le statut de sortie (via wait()). Si le parent ne récupère pas
ce statut, le processus enfant reste en état **zombie**.

Si le processus parent se termine avant l\'enfant, le processus enfant
est adopté par le processus init (PID 1) sous Linux.

[]{#anchor-17}4.4 Arborescence des Processus

La commande pstree affiche une arborescence des processus en cours,
montrant les relations parent-enfant.

[]{#anchor-18}5. Commandes Système Utiles

[]{#anchor-19}5.1 lsof (List Open Files)

La commande lsof permet de lister les fichiers ouverts par les
processus. Elle fournit de nombreuses informations, notamment :

-   Les processus écoutant sur des ports réseau.

-   Les fichiers ouverts par un processus spécifique (avec l\'option
    > -p).

-   Les permissions et types de fichiers.

Dans le résultat de lsof, on peut voir :

-   **0** : stdin (entrée standard)

-   **1** : stdout (sortie standard)

-   **2** : stderr (sortie d\'erreur standard)

-   **TYPE** : Type du fichier (par exemple, CHR pour Character Device).

[]{#anchor-20}5.2 ulimit

La commande ulimit est utilisée pour limiter les ressources utilisées
par le shell et les processus qu\'il lance, telles que :

-   La taille maximale des fichiers créés.

-   Le nombre maximum de processus.

-   La quantité de mémoire utilisée.

[]{#anchor-21}6. Systèmes de Fichiers et Périphériques

[]{#anchor-22}6.1 Fichiers de Périphériques

Les périphériques sont représentés par des fichiers spéciaux situés
généralement dans le répertoire /dev. Par exemple :

-   **/dev/pts/** : Représente les pseudo-terminaux (périphériques de
    > terminal).

-   **/dev/tty** : Représente le terminal contrôlant le processus.

Ces fichiers permettent aux programmes d\'interagir avec le matériel via
le système de fichiers.

[]{#anchor-23}6.2 Types de Périphériques

-   **CHR (Character Device)** : Périphérique de caractère qui transmet
    > les données caractère par caractère (ex : claviers, terminaux).

-   **BLK (Block Device)** : Périphérique de bloc qui transmet les
    > données par blocs (ex : disques durs).

[]{#anchor-24}7. Loader et Bibliothèques

[]{#anchor-25}7.1 Bibliothèques Statiques et Dynamiques

-   **Bibliothèques statiques** : Le code de la bibliothèque est inclus
    > dans l\'exécutable au moment de la compilation.

-   **Bibliothèques dynamiques** : Le code de la bibliothèque est chargé
    > au moment de l\'exécution, ce qui réduit la taille de
    > l\'exécutable et permet le partage de code.

[]{#anchor-26}7.2 Le Loader

Le **loader** est un programme responsable du chargement des exécutables
en mémoire et de la résolution des dépendances des bibliothèques
dynamiques.

-   Exemple de loader : /lib/ld-linux-aarch64.so.1 pour les systèmes
    > Linux sur architecture ARM 64 bits.

-   Le loader permet d\'exécuter tout programme pour lequel vous avez
    > les droits de lecture.

**Caractéristiques :**

-   Il y a généralement **un seul loader par système d\'exploitation**.
    > Toutefois, il peut y avoir des loaders distincts pour les
    > architectures 32 bits et 64 bits.

-   Le loader est lui-même un exécutable spécial.

-   Exemple de loader : /lib64/ld-linux-x86-64.so.2 pour les systèmes
    > Linux 64 bits sur architecture x86_64.

[]{#anchor-27}8. Utilisateurs et Permissions

[]{#anchor-28}8.1 Identifiants d\'Utilisateur et de Groupe

-   **UID (User ID)** : Numéro unique attribué à chaque utilisateur.

-   **GID (Group ID)** : Numéro unique attribué à chaque groupe.

Ces identifiants sont utilisés pour gérer les permissions d\'accès aux
fichiers et aux processus.

> **Un user** n'existe pas tant qu'il n'est pas lié à un fichier ou
> process, Il s'agit SEULEMENT d'une propriété de ces derniers

[]{#anchor-29}8.2 Le Fichier /etc/passwd

Le fichier /etc/passwd contient des informations sur les utilisateurs du
système :

-   Nom d\'utilisateur.

-   Mot de passe (historique, mais généralement remplacé par un x
    > indiquant que le mot de passe est stocké par default dans
    > /etc/shadow).

-   UID et GID.

-   Informations supplémentaires (nom réel, répertoire personnel, shell
    > par défaut).

[]{#anchor-30}8.3 Le Fichier /etc/shadow

Le fichier /etc/shadow contient les mots de passe chiffrés des
utilisateurs. Seul l\'administrateur système (root) a accès à ce fichier
pour des raisons de sécurité.

[]{#anchor-31}8.4 Propriétés des Fichiers

Les fichiers ont des propriétaires (UID) et des groupes (GID), ainsi que
des permissions définissant qui peut lire, écrire ou exécuter le
fichier. Si le système ne peut pas associer un UID à un nom
d\'utilisateur (par exemple, si l\'utilisateur a été supprimé), la
commande ls -l affichera le numéro UID au lieu d\'un nom.

[]{#anchor-32}9. Chroot et Jails et lld

[]{#anchor-33}9.1 Comprendre chroot

La commande chroot modifie le répertoire racine apparent (/) pour le
processus courant et ses descendants.

**chroot** est une commande Unix qui modifie le répertoire racine
apparent pour un processus spécifique et ses enfants.

En d\'autres termes, la commande chroot crée un environnement confiné
dans lequel le processus et ses enfants ne peuvent accéder qu\'à une
partie spécifique du système de fichiers, définie comme la nouvelle
racine.

[]{#anchor-34}9.2 Le Processus de Démarrage et pivot_root

Lors du démarrage, le système utilise un **ramdisk** initial contenant
les pilotes essentiels. Une fois que le système de fichiers principal
est monté, il utilise pivot_root pour changer le répertoire racine vers
le nouveau système, un processus similaire à chroot.

[]{#anchor-35}9.3 Les Jails

Les **jails** sont une technologie utilisée pour créer des
environnements virtuels isolés. Chaque jail a ses propres :

-   Système de fichiers.

-   Processus.

-   Comptes utilisateurs, y compris un compte root séparé.

Les jails offrent un niveau d\'isolation plus élevé que chroot, car ils
contrôlent également les ressources réseau, les IPC, etc.

[]{#anchor-36}9.4 Différences entre chroot, jail, sysctl et systemctl

-   **chroot** : Change le répertoire racine apparent pour un processus.

-   **jail** : Crée un environnement d\'exécution isolé avec un contrôle
    > accru sur les ressources.

-   **sysctl** : Utilitaire pour afficher ou modifier les paramètres du
    > noyau au moment de l\'exécution.

-   **systemctl** : Commande pour contrôler le système d\'initialisation
    > systemd, permettant de gérer les services, les cibles, etc.

[]{#anchor-37}9.5 La Commande ldd

La commande ldd (List Dynamic Dependencies) permet de lister les
dépendances dynamiques d\'un exécutable ou d\'une bibliothèque partagée.
Elle affiche les bibliothèques dynamiques requises par un programme pour
s\'exécuter.

**Utilisation :**

-   ldd \[options\] fichier

**Exemple :**

\$ ldd /bin/ls

**Sortie typique :**

linux-vdso.so.1 (0x00007ffd5b7f7000)

libc.so.6 =\> /lib/x86_64-linux-gnu/libc.so.6 (0x00007f6a7b3e9000)

/lib64/ld-linux-x86-64.so.2 (0x00007f6a7b5e7000)

**Interprétation :**

-   Les bibliothèques dynamiques requises sont listées avec un \"=\>\"
    > indiquant leur chemin sur le système de fichiers.

-   Le loader /lib64/ld-linux-x86-64.so.2 est également mentionné, car
    > il est nécessaire pour charger l\'exécutable.

**Précautions :**

-   **Sécurité :** **Ne pas exécuter ldd sur des fichiers
    > potentiellement vérolés ou compromis.** En effet, certains
    > exécutables malveillants peuvent exploiter ldd pour exécuter du
    > code arbitraire.

**Explication :**

-   Lorsque vous exécutez ldd sur un exécutable, il peut charger et
    > exécuter du code de l\'exécutable pour déterminer les dépendances,
    > ce qui peut être dangereux si le fichier est malveillant.

-   Les commandes objdump -p et readelf -d lisent directement les
    > informations depuis le fichier sans exécuter de code, offrant une
    > alternative plus sûre pour inspecter les dépendances.

[]{#anchor-38}10. Concepts Supplémentaires

[]{#anchor-39}10.1 WINE et WSL

-   **WINE** : Acronyme de \"Wine Is Not an Emulator\", c\'est un
    > logiciel qui permet d\'exécuter des applications Windows sur des
    > systèmes Unix/Linux en implémentant les API Windows.

-   **Proton GE** : Une version modifiée de WINE, utilisée pour
    > améliorer la compatibilité des jeux Windows sur Linux via la
    > plateforme Steam.

-   **WSL (Windows Subsystem for Linux)** : Permet d\'exécuter un
    > environnement Linux sous Windows. WINE peut être considéré comme
    > une sorte de reverse WSL.

[]{#anchor-40}10.2 Caractères de Contrôle

Les **caractères de contrôle** sont des caractères non imprimables
utilisés pour contrôler le flux de données dans les terminaux, tels que
:

-   Retour chariot (\\r).

-   Saut de ligne (\\n).

-   Tabulation (\\t).

[]{#anchor-41}10.3 Commande tty

La commande tty affiche le fichier de périphérique du terminal connecté
au processus standard d\'entrée.

[]{#anchor-42}11. Commandes Supplémentaires et Concepts

[]{#anchor-43}11.1 Commande du -sh folder

La commande du (Disk Usage) est utilisée pour estimer l\'espace utilisé
par des fichiers ou des répertoires.

-   **Syntaxe :** du -sh dossier

-   **Options :**

    -   -s : Affiche le total pour chaque argument (résumé).

    -   -h : Affiche les tailles dans un format lisible pour l\'humain
        > (par exemple, K, M, G).

**Utilisation :**

\$ du -sh /chemin/du/dossier

**Fonctionnalité :**

-   Calcule la taille totale de tous les fichiers dans un dossier donné.

-   Utile pour vérifier l\'espace disque utilisé par un répertoire
    > spécifique.

-   Aide à identifier les dossiers qui occupent beaucoup d\'espace sur
    > le disque.

[]{#anchor-44}11.2 debootstrap et Isolation avec chroot

**debootstrap** est un utilitaire Debian qui permet d\'installer un
système de base Debian dans un répertoire, sans avoir besoin d\'un
programme d\'installation traditionnel.

-   **Fonctionnalité :**

    -   Télécharge les fichiers de base d\'une distribution Debian (ou
        > dérivée) dans un répertoire spécifié.

    -   Crée un système minimal qui peut être utilisé pour des tests, du
        > développement ou comme base pour des conteneurs.

**Utilisation :**

**Installation d\'un système Debian minimal :\
**

\$ sudo debootstrap stable /chemin/vers/le/dossier
http://deb.debian.org/debian/

-   **Options :**

    -   stable : Version de Debian à installer (par exemple, stable,
        > testing, sid).

    -   /chemin/vers/le/dossier : Répertoire cible où le système sera
        > installé.

    -   URL du miroir Debian : Spécifie le dépôt à utiliser pour le
        > téléchargement.

[]{#anchor-45}Combinaison avec chroot

En combinant debootstrap avec chroot, vous pouvez créer un environnement
isolé qui simule un système Debian complet.

-   **Étapes :**

    -   Utiliser debootstrap pour installer le système de base dans un
        > répertoire.

    -   Utiliser chroot pour entrer dans cet environnement.

**Exemple :\
**\
\$ sudo debootstrap stable /srv/chroot/debian
http://deb.debian.org/debian/

\$ sudo chroot /srv/chroot/debian /bin/bash

-   **Avantages :**

    -   Permet de tester des configurations sans affecter le système
        > hôte.

    -   Utile pour le développement, le test de paquets, ou la
        > réparation de systèmes.

[]{#anchor-46}Limitations de chroot

**Important :** Le chroot ne fournit pas une isolation complète. Les
limitations incluent :

-   **Le chroot n\'isole pas le réseau :**

    -   Les processus dans le chroot peuvent toujours accéder au réseau
        > du système hôte.

    -   Les connexions réseau sortantes et entrantes ne sont pas
        > bloquées par défaut.

-   **Le chroot n\'isole pas les processus :**

    -   Les processus dans le chroot sont visibles depuis le système
        > hôte.

    -   Les processus du chroot peuvent interagir avec les processus du
        > système hôte (par exemple, envoyer des signaux).

-   **Le chroot n\'isole pas les ressources :**

    -   Les processus peuvent consommer les ressources du système hôte
        > (CPU, mémoire, stockage).

    -   Il n\'y a pas de limites ou de quotas imposés par défaut.

[]{#anchor-47}11.3 Différence entre su et su -

La commande su (substitute user) est utilisée pour changer d\'identité
d\'utilisateur dans un shell. Elle est souvent utilisée pour passer à
l\'utilisateur **root** ou à un autre utilisateur.

[]{#anchor-48}su (sans tiret)

-   **Usage :** su \[utilisateur\]

-   **Fonctionnalité :**

    -   Change l\'utilisateur courant vers l\'utilisateur spécifié (par
        > défaut, root).

    -   Ne modifie pas entièrement l\'environnement de l\'utilisateur
        > précédent.

    -   Ne recharge pas les variables d\'environnement de l\'utilisateur
        > cible.

    -   Les variables comme \$PATH peuvent rester celles de
        > l\'utilisateur initial, ce qui peut entraîner des problèmes
        > d\'accès aux commandes.

**Exemple :\
**\
\$ su root

Mot de passe :

root\$ echo \$PATH

\# Peut afficher le PATH de l\'utilisateur précédent.

[]{#anchor-49}su - (avec tiret)

-   **Usage :** su - \[utilisateur\] ou su -l \[utilisateur\]

-   **Fonctionnalité :**

    -   Le tiret (-) indique une connexion de type \"login shell\".

    -   Change l\'utilisateur courant vers l\'utilisateur spécifié.

    -   Recharge complètement l\'environnement de l\'utilisateur cible,
        > y compris les variables d\'environnement, le répertoire
        > personnel, le shell par défaut.

    -   Simule une nouvelle session de connexion pour l\'utilisateur
        > cible.

    -   Les scripts de démarrage (comme .bash_profile, .profile) sont
        > exécutés.

**Exemple :\
**\$ su - root

Mot de passe :

root\$ echo \$PATH
