

Passer au contenu principal



Architecture sur AWS – Atelier 2 – Création de votre infrastructure Amazon VPC
Informations sur l'atelier
Expire le 04 déc. à 03:24
1 heure 30 minutes
Commentaires
Ressources
Region


Contenu de l'atelier
    • Présentation de l'atelier
    • Commencer l'atelier
    • Tâche 1 : créer un Amazon VPC dans une région
    • Tâche 2 : créer des sous-réseaux publics et privés
    • Tâche 3 : créer une passerelle Internet
    • Tâche 4 : acheminer le trafic Internet du sous-réseau public vers la passerelle Internet
    • Tâche 5 : créer un groupe de sécurité public
    • Tâche 6 : lancer une instance Amazon EC2 dans un sous-réseau public
    • Tâche 7 : se connecter à une instance publique via HTTP
    • Tâche 8 : se connecter à l'instance Amazon EC2 dans le sous-réseau public par le biais de Session Manager
    • Tâche 9 : créer une passerelle NAT et configurer le routage dans le sous-réseau privé
    • Tâche 10 : créer un groupe de sécurité pour les ressources privées
    • Tâche 11 : lancer une instance Amazon EC2 dans un sous-réseau privé
    • Tâche 12 : se connecter à l'instance Amazon EC2 dans le sous-réseau privé
    • Tâche facultative 1 : dépanner la connectivité entre l'instance privée et l'instance publique
    • Tâche facultative 2 : récupérer les métadonnées de l'instance
    • Conclusion
    • Terminer l'atelier
    • Ressources supplémentaires
    • Solution de la tâche facultative

Atelier 2 : Création de votre infrastructure Amazon VPC
© 2024, Amazon Web Services, Inc. ou ses sociétés apparentées. Tous droits réservés. Ce cours ne peut être reproduit ou distribué, en partie ou dans son intégralité, sans l’autorisation écrite préalable d’Amazon Web Services, Inc. La copie, le prêt ou la vente à des fins commerciales sont interdits. Toutes les marques de commerce appartiennent à leurs propriétaires respectifs.
Remarque : n’incluez aucune information personnelle, d’identification ou confidentielle dans l’environnement de l’atelier. Les informations saisies peuvent être visibles par d’autres personnes.
Des corrections, commentaires ou questions ? Contactez-nous à l’adresse AWS Training and Certification.
Présentation de l’atelier
En tant que Solutions Architects AWS, il est important de comprendre la fonctionnalité et les capacités globales d’Amazon Web Service (AWS), ainsi que la relation entre les composants réseau AWS. Dans cet atelier, vous allez créer un Amazon Virtual Private Cloud (Amazon VPC), un sous-réseau public et privé dans une zone de disponibilité (AZ) unique, des acheminements publics et privés, une passerelle NAT, ainsi qu’une passerelle Internet. Ces services sont la base d’une architecture de mise en réseau à l’intérieur d’AWS. Cette conception d’architecture couvre les concepts d’infrastructure, de conception, de routage et de sécurité.
L’image suivante montre l’architecture finale pour l’environnement de cet atelier :

Objectifs
À la fin de cet atelier, vous saurez comment :
    • créer un Amazon VPC ; 
    • créer des sous-réseaux publics et privés ; 
    • créer une passerelle Internet ; 
    • configurer une table de routage et l’associer à un sous-réseau ; 
    • créer une instance Amazon Elastic Compute Cloud (Amazon EC2) et la rendre publiquement accessible ; 
    • isoler une instance Amazon EC2 dans un sous-réseau privé ; 
    • créer des groupes de sécurité et les affecter à des instances Amazon EC2 ; 
    • vous connecter à des instances Amazon EC2 à l’aide de Session Manager, une fonctionnalité d’AWS Systems Manager ; 
Signification des icônes
De nombreuses icônes sont utilisées dans cet atelier pour attirer votre attention sur différents types d’instructions et de remarques. La liste suivante explique la signification de chaque icône :
    • Commande : une demande que vous devez exécuter. 
    • Sortie attendue : un exemple de résultat que vous pouvez utiliser pour vérifier le résultat d’une commande ou d’un fichier modifié. 
    • Remarque : Un indice, une directive ou un conseil important. 
    • En savoir plus : Un emplacement où trouver davantage d’informations. 
    • Sécurité : une opportunité d’intégrer les bonnes pratiques en matière de sécurité. 
    • Attention : information présentant un intérêt spécial ou d’une importance particulière (pas assez importante pour causer des problèmes avec l’équipement ou les données si vous n’y faites pas attention, mais qui pourrait vous amener à devoir répéter certaines étapes). 
    • AVERTISSEMENT : une action qui est irréversible et susceptible d’avoir une incidence sur l’échec d’une commande ou d’un processus (y compris les avertissements concernant les configurations ne pouvant pas être modifiées après leur mise en place). 
Commencer l’atelier
    1. Pour lancer l’atelier, sélectionnez Start lab (Commencer l’atelier) en haut de la page. 
Vous devez attendre que les services AWS mis en service soient prêts avant de pouvoir continuer.
    2. Pour ouvrir l’atelier, sélectionnez Open Console (Ouvrir la console). 
Vous êtes automatiquement connecté à l’AWS Management Console dans un nouvel onglet de navigateur web.
Sauf stipulation contraire, ne modifiez pas la région.
Erreurs de connexion fréquentes
Erreur : vous devez d’abord vous déconnecter

Si vous voyez le message You must first log out before logging into a different AWS account (Vous devez d’abord vous déconnecter avant de vous connecter à un autre compte AWS) :
    • Cliquez sur le lien click here (Cliquez ici). 
    • Fermez votre onglet de navigateur web Amazon Web Services Sign In (Connexion à Amazon Web Services) et revenez à votre page d’atelier initiale. 
    • Sélectionnez à nouveau Open Console (Ouvrir la console). 
Erreur : la sélection de l’option Start Lab (Commencer l’atelier) n’a aucun effet
Dans certains cas, des extensions de navigateur web avec fenêtres contextuelles ou bloqueurs de scripts peuvent empêcher le bouton Start Lab (Commencer l’atelier) de fonctionner comme prévu. Si vous rencontrez un problème au démarrage de l’atelier :
    • Ajoutez le nom de domaine de l’atelier à la liste d’autorisation de votre fenêtre contextuelle ou de votre bloqueur de scripts. Ou bien, désactivez-les. 
    • Actualisez la page et réessayez. 

Scénario
Votre équipe est chargée de créer le prototype d’une architecture pour une nouvelle application basée sur le web. Afin de définir votre architecture, vous devez avoir une meilleure compréhension des sous-réseaux publics et privés, du routage et des options des instances Amazon EC2.
Services AWS non utilisés dans cet atelier
Les services AWS qui ne sont pas utilisés dans cet atelier sont désactivés dans l’environnement de l’atelier. En outre, les fonctionnalités des services utilisés dans cet atelier sont limitées aux stricts besoins de l’atelier. Des erreurs peuvent survenir si vous effectuez des actions ou tentez d’accéder à des services autres que ceux prévus dans le guide de cet atelier.

Tâche 1 : créer un Amazon VPC dans une région
Dans cette tâche, vous créerez un nouveau Amazon VPC dans le Cloud AWS.
Informations supplémentaires : Amazon VPC vous permet de mettre en service une section logiquement isolée du Cloud AWS dans laquelle vous pouvez lancer des ressources AWS dans un réseau virtuel que vous définissez. Vous conservez la totale maîtrise de votre environnement de mise en réseau virtuel, y compris pour la sélection de vos propres plages d’adresses IP, la création de sous-réseaux et la configuration de tables de routage et de passerelles réseau. Vous pouvez également utiliser les options de sécurité améliorées d’Amazon VPC pour assurer un accès plus granulaire aux instances Amazon EC2 et depuis celles-ci au sein de votre réseau virtuel.

    3. En haut de la Console de gestion AWS, dans la barre de recherche, recherchez et sélectionnez 
    3. VPC
       . 
Attention : vérifiez que la région affichée dans le coin supérieur droit de la console est identique à la valeur Region (Région) sur le côté gauche de la page de cet atelier.
Remarque : la console de gestion VPC propose un assistant VPC qui peut automatiquement créer plusieurs architectures VPC. Toutefois, dans cet atelier, vous créez manuellement les composants VPC.
    4. Dans le volet de navigation de gauche, sélectionnez Vos VPC. 
La console affiche une liste des VPC actuellement disponibles. Un VPC par défaut est fourni afin que vous puissiez lancer des ressources dès que vous commencez à utiliser AWS.
    5. Sélectionnez Create VPC (Créer un VPC) et configurez les éléments suivants : 
    • Ressources à créer : sélectionnez VPC uniquement. 
    • Balise Nom - facultatif : saisissez 
    • Lab VPC
    • IPv4 CIDR (CIDR IPv4) : saisissez 
    • 10.0.0.0/16
      . 
    6. Sélectionnez Créer un VPC. 
Le message You successfully created vpc-xxxxxxxxxx / Lab VPC (Vous avez créé le VPC d’atelier vpc-xxxxxxxxxx avec succès) s’affiche en haut de l’écran.
La page Détails du VPC s’affiche.
    7. Vérifiez l’état du Lab VPC (VPC d’atelier). 
Sortie attendue : la sortie suivante doit s’afficher :
    • État : Disponible 
Le VPC d’atelier dispose d’une plage CIDR (Classless Inter-Domain Routing) 10.0.0.0/16, qui inclut toutes les adresses IP commençant par 10.0.x.x. Cette plage contient plus de 65 000 adresses. Vous diviserez ensuite ces adresses en sous-réseaux distincts.
    8. Depuis cette même page, sélectionnez Actions , puis Modifier les paramètres VPC. 
La page Modifier les paramètres VPC s’affiche.
    9. À partir de la section Paramètres DNS, sélectionnez Activer les noms d’hôte DNS. 
Cette option attribue un nom de système de noms de domaine (DNS) convivial aux instances Amazon EC2 dans le VPC, par exemple :
ec2-52-42-133-255.us-west-2.compute.amazonaws.com
    10. Sélectionnez Enregistrer. 
Le message You have successfully modified the settings for vpc-xxxxxxxxxx / Lab VPC. (Vous avez modifié les paramètres du VPC d’atelier vpc-xxxxxxxxxx avec succès) s’affiche en haut de l’écran.
Toutes les instances Amazon EC2 lancées dans cet Amazon VPC reçoivent désormais automatiquement un nom d’hôte DNS. Vous pouvez également créer un nom DNS plus significatif (par exemple, app.company.com) en utilisant les registres d’Amazon Route 53.
Félicitations ! Vous avez créé votre propre VPC avez succès et vous pouvez désormais lancer les ressources AWS dans ce réseau virtuel défini.

Tâche 2 : créer des sous-réseaux publics et privés
Dans cette tâche, vous allez créer un sous-réseau public et un sous-réseau privé dans le VPC d’atelier. Afin d’ajouter un nouveau sous-réseau à votre VPC, vous devez spécifier un bloc d’adresse CIDR IPv4 pour le sous-réseau, parmi la plage de votre VPC. Vous pouvez spécifier la zone de disponibilité dans laquelle vous souhaitez placer le sous-réseau. Vous pouvez disposer de plusieurs sous-réseaux dans la même zone de disponibilité.

Remarque : Un sous-réseau est une sous-plage d’adresses IP au sein d’un réseau. Vous pouvez lancer des ressources AWS dans un sous-réseau spécifié. Utilisez un sous-réseau public pour les ressources qui doivent être connectées à Internet, et utilisez un sous-réseau privé pour les ressources qui doivent rester isolées d’Internet.
Tâche 2.1 : créer votre sous-réseau public
Le sous-réseau public est destiné aux ressources accessibles sur Internet.
    11. Dans le volet de navigation de gauche, sélectionnez Subnets (Sous-réseaux). 
    12. Sélectionnez Créer un sous-réseau et configurez les éléments suivants : 
    • ID de VPC : sélectionnez Lab VPC (VPC d’atelier) dans la liste déroulante. 
    • Nom du sous-réseau : saisissez 
    • Public Subnet
. 
    • Zone de disponibilité : sélectionnez la première zone de disponibilité de la liste. (Ne sélectionnez pas No Preference [Aucune preference].) 
    • IPv4 subnet CIDR block (Bloc d’adresse CIDR de sous-réseau IPv4) : saisissez 
    • 10.0.0.0/24
      . 
    13. Sélectionnez Créer un sous-réseau. 
Le message You have successfully created 1 subnet: subnet-xxxxxx (Vous avez créé un sous-réseau avec succès : subnet-xxxxxx) s’affiche en haut de l’écran.
    14. Vérifiez l’état. 
Sortie attendue : la sortie suivante doit s’afficher :
    • État : Disponible 
Remarque : le VPC comporte une plage CIDR 10.0.0.0/16, qui inclut toutes les adresses IP 10.0.x.x. Le sous-réseau que vous venez de créer possède une plage CIDR 10.0.0.0/24, qui inclut toutes les adresses IP 10.0.0.x. Ces plages peuvent se ressembler, mais le sous-réseau est plus petit que le VPC en raison du suffixe /24 dans la plage CIDR.
Vous allez à présent configurer le sous-réseau pour attribuer automatiquement une adresse IP publique à toutes les instances lancées dans celui-ci.
    15. Sélectionnez Public Subnet (Sous-réseau public).
    16. Sélectionnez Actions , puis Modifier les paramètres de sous-réseau.
La page Modifier les paramètres de sous-réseau s’affiche.
    17. À partir de la section Paramètres d’attribution automatique d’une adresse IP, sélectionnez Enable auto-assign public IPv4 address (Activer l’attribution automatique d’une adresse IPv4 publique).
    18. Sélectionnez Enregistrer.
Le message You have successfully changed subnet settings: Enable auto-assign public IPv4 address (Vous avez modifié avec succès le paramètre du sous-réseau : Activer l’attribution automatique d’une adresse IPv4 publique) s’affiche en haut de l’écran.
Remarque : bien que ce sous-réseau ait été nommé Public Subnet (Sous-réseau public), il n’est pas encore public. Un sous-réseau public doit disposer d’une passerelle Internet et d’un acheminement à la passerelle. Dans cet atelier, vous créerez et attacherez la passerelle Internet et les tables de routage.
Tâche 2.2 : créer votre sous-réseau privé
Le sous-réseau privé est dédié aux ressources qui doivent rester isolées d’Internet.
    19. Sélectionnez Créer un sous-réseau et configurez les éléments suivants : 
    • ID de VPC : sélectionnez Lab VPC (VPC d’atelier) dans la liste déroulante. 
    • Nom du sous-réseau : saisissez 
    • Private Subnet
. 
    • Zone de disponibilité : sélectionnez la première zone de disponibilité de la liste. (Ne sélectionnez pas No Preference [Aucune preference].) 
    • IPv4 subnet CIDR block (Bloc d’adresse CIDR de sous-réseau IPv4) : saisissez 
    • 10.0.2.0/23
      . 
    20. Sélectionnez Créer un sous-réseau. 
Le message You have successfully created 1 subnet: subnet-xxxxxx (Vous avez créé un sous-réseau avec succès : subnet-xxxxxx) s’affiche en haut de l’écran.
    21. Vérifiez l’état. 
Sortie attendue : la sortie suivante doit s’afficher :
    • État : Disponible 
Remarque : le bloc d’adresse CIDR 10.0.2.0/23 inclut toutes les adresses IP qui commencent par 10.0.2.x et 10.0.3.x. Ce sous-réseau est deux fois plus grand que le sous-réseau public, car la plupart des ressources doivent rester privées, sauf si elles doivent être spécifiquement accessibles depuis Internet.
Votre VPC possède désormais deux sous-réseaux. Toutefois, ces sous-réseaux sont isolés et ne peuvent pas communiquer avec des ressources en dehors du VPC. Vous allez ensuite configurer le sous-réseau public pour vous connecter à Internet par le biais d’une passerelle Internet.
Félicitations ! Vous avez créé un sous-réseau public et un sous-réseau privé dans le VPC d’atelier.

Tâche 3 : créer une passerelle Internet
Au cours de cette tâche, vous allez créer une passerelle Internet afin que le trafic Internet puisse accéder au sous-réseau public. Pour accorder l’accès à ou depuis Internet pour des instances d’un sous-réseau d’un VPC, vous allez créer une passerelle Internet et l’attacher à votre VPC. Ensuite, vous ajouterez un acheminement dans la table de routage de votre sous-réseau pour diriger le trafic Internet vers la passerelle Internet.
Informations supplémentaires : Une passerelle Internet a deux objectifs : fournir une cible dans les tables de routage de votre VPC pour le trafic routable sur Internet et effectuer une traduction d’adresses réseau (NAT) pour les instances auxquelles des adresses IPv4 publiques ont été attribuées.
    22. Dans le volet de navigation de gauche, sélectionnez Passerelles Internet.
    23. Sélectionnez Créer une passerelle Internet et configurez les éléments suivants :
    • Balise Nom : saisissez 
    • Lab IGW
      . 
    24. Sélectionnez Créer une passerelle Internet. 
Le message The following internet gateway was created: igw-xxxxxx - Lab IGW. You can now attach to a VPC to enable the VPC to communicate with the internet. (La passerelle Internet suivante a été créée avec succès :igw-xxxxxx - Lab IGW. Vous pouvez maintenant l’attacher à un VPC pour lui permettre de communiquer avec Internet) s’affiche en haut de l’écran.
Vous pouvez désormais attacher la passerelle Internet à votre VPC d’atelier.
    25. Depuis cette même page, sélectionnez Actions , puis Attacher au VPC.
    26. Pour VPC disponibles, sélectionnez Lab VPC (VPC d’atelier) dans la liste déroulante.
    27. Sélectionnez Attacher une passerelle Internet.
Le message Internet gateway igw-xxxxx successfully attached to vpc-xxxxx (La passerelle Internet igw-xxxxx a été attachée au vpc-xxxxx avec succès.) s’affiche en haut de l’écran.
    28. Vérifiez l’état. 
Sortie attendue : la sortie suivante doit s’afficher :
    • État : Attaché 
La passerelle Internet est à présent attachée à votre VPC d’atelier. Bien que vous ayez créé une passerelle Internet et que vous l’ayez attachée à votre VPC, vous devez également configurer la table de routage du sous-réseau public pour utiliser la passerelle Internet.
Félicitations ! Vous avez créé une passerelle Internet afin que le trafic Internet puisse accéder au sous-réseau public.

Tâche 4 : acheminer le trafic Internet du sous-réseau public vers la passerelle Internet
Dans cette tâche, vous créerez une table de routage et y ajouterez un acheminement pour diriger le trafic Internet vers votre passerelle Internet et associer vos sous-réseaux publics à votre table de routage. Chaque sous-réseau de votre VPC doit être associé à une table de routage. La table contrôle le routage du sous-réseau. Un sous-réseau peut uniquement être associé à une table de routage à la fois, mais vous pouvez associer plusieurs sous-réseaux à la même table de routage.
Informations supplémentaires : Une table de routage contient un ensemble de règles, appelées acheminements, qui permettent de déterminer la direction du trafic réseau. Pour utiliser une passerelle Internet, la table de routage de votre sous-réseau doit contenir un acheminement qui dirige le trafic Internet entrant vers la passerelle Internet. Vous pouvez appliquer l’acheminement à l’ensemble des destinations non explicitement connues de la table de routage (0.0.0.0/0 pour IPv4 ou ::/0 pour IPv6), ou vous pouvez appliquer l’acheminement à une plage réduite d’adresses IP. Si votre sous-réseau est associé à une table de routage comportant un acheminement vers une passerelle Internet, il est considéré comme un sous-réseau public.
    29. Dans le volet de navigation de gauche, sélectionnez Tables de routage. 
Il existe actuellement une table de routage par défaut associée au VPC, Lab VPC (VPC d’atelier). Elle achemine localement le trafic. Vous allez maintenant créer une autre table de routage pour acheminer le trafic public vers votre passerelle Internet.
    30. Sélectionnez Créer une table de routage et configurez les éléments suivants : 
    • Name - optional (Nom – facultatif) : saisissez 
    • Public Route Table
      .
    • VPC : sélectionnez Lab VPC (VPC d’atelier) dans la liste déroulante.
    31. Sélectionnez Créer une table de routage. 
Le message Route table rtb-xxxxxxx | Public Route Table was created successfully. (La table de routage rtb-xxxxxxx | Public Route Table a été créée avec succès) s’affiche en haut de l’écran.
    32. Sélectionnez l’onglet Routes dans la moitié inférieure de la page. 
Remarque : Un acheminement de votre table de routage autorise le trafic au sein du réseau 10.0.0.0/16 à circuler dans le réseau, mais il n’achemine pas le trafic en dehors du réseau.
Ajoutez maintenant un nouvel acheminement pour activer le trafic public.
    33. Choisissez Modifier des routes. 
    34. Sélectionnez Ajouter une route et configurez les éléments suivants : 
    • Destination : saisissez 
    • 0.0.0.0/0
      . 
    • Cible : sélectionnez Passerelle Internet dans le menu déroulant, puis sélectionnez l’ID de la passerelle Internet affichée. 
    35. Sélectionnez Enregistrer les modifications. 
Le message Updated routes for rtb-xxxxxxx / Public Route Table successfully (Les acheminements de la table de routage publique rtb-xxxxxxx ont été mises à jour avec succès) s’affiche en haut de l’écran.
    36. Sélectionnez l’onglet Associations de sous-réseau. 
    37. Sélectionnez Modifier des associations de sous-réseau. 
    38. Sélectionnez Sous-réseau public 
    39. Sélectionnez Enregistrer les associations. 
Le message You have successfully updated subnet associations for rtb-xxxxxxx / Public Route Table. (Vous avez mis à jour les associations de sous-réseau de la table de routage rtb-xxxxxxx / Public Route Table avec succès.) s’affiche en haut de l’écran.
Remarque : Le sous-réseau est désormais public, car il dispose d’un acheminement vers Internet par le biais de la passerelle Internet.
Félicitations ! Vous avez configuré la table de routage.

Tâche 5 : créer un groupe de sécurité public
Dans cette tâche, vous créerez un groupe de sécurité afin que les utilisateurs puissent accéder à votre instance Amazon EC2. Les groupes de sécurité dans un VPC spécifient le trafic autorisé vers ou depuis une instance Amazon EC2.
Informations supplémentaires : Vous pouvez utiliser des groupes de sécurité Amazon EC2 pour sécuriser les instances d’un VPC Amazon. Les groupes de sécurité d’un VPC vous permettent de spécifier le trafic réseau entrant et sortant autorisé vers ou depuis chaque instance Amazon EC2. Le trafic non explicitement autorisé vers ou depuis une instance est automatiquement refusé.
Sécurité : il est recommandé d’utiliser le protocole HTTPS pour renforcer la sécurité du trafic web. Toutefois, à des fins de simplicité, seul le protocole HTTP est utilisé dans cet atelier.
    40. Dans le volet de navigation de gauche, sélectionnez Groupes de sécurité. 
    41. Sélectionnez Créer un groupe de sécurité et configurez les éléments suivants : 
    • Nom de groupe de sécurité : saisissez 
    • Public SG
. 
    • Description : saisissez 
    • Allows incoming traffic to public instance
      . 
    • VPC : sélectionnez Lab VPC (VPC d’atelier) dans la liste déroulante. 
    42. Dans la section Règles entrantes, sélectionnez Ajouter une règle et configurez les éléments suivants : 
    • Type : sélectionnez HTTP dans le menu déroulant. 
    • Source : sélectionnez Anywhere-IPv4 dans le menu déroulant. 
    43. Dans la section Tags - optional (Balises – facultatif), sélectionnez Ajouter une nouvelle balise et configurez les éléments suivants : 
    • Clé : saisissez 
    • Name
. 
    • Valeur : saisissez 
    • Public SG
      . 
    44. Sélectionnez Créer un groupe de sécurité. 
Le message Security group (sg-xxxxxxx | Public SG) was created successfully (Le groupe de sécurité [sg-xxxxxxx | Public SG] a été créé avec succès) s’affiche en haut de l’écran.
Félicitations ! Vous avez créé un groupe de sécurité qui autorise le trafic HTTP. Vous en aurez besoin pour la prochaine tâche, lorsque vous lancerez une instance Amazon EC2 dans le sous-réseau public.

Tâche 6 : lancer une instance Amazon EC2 dans un sous-réseau public
Dans cette tâche, vous lancerez une instance Amazon EC2 dans un sous-réseau public. Pour activer la communication sur Internet pour IPv4, votre instance doit comporter une adresse IPv4 publique associée à une adresse IPv4 privée sur votre instance. Par défaut, votre instance prend uniquement en charge l’espace (interne) d’adresses IP privées défini dans le VPC et le sous-réseau.

Informations supplémentaires : La passerelle Internet que vous avez créée assure logiquement la NAT un-à-un au nom de votre instance. Lorsque le trafic quitte le sous-réseau de votre VPC pour se diriger vers Internet, le champ d’adresse de réponse est défini selon l’adresse IPv4 publique ou l’adresse IP élastique de votre instance, et non selon son adresse IP privée.
    45. En haut de la Console de gestion AWS, dans la barre de recherche, recherchez et sélectionnez 
    45. EC2
       . 
La Console de gestion Amazon EC2 s’affiche.
Tâche 6.1 : commencer la configuration de l’instance
    46. Dans le menu de navigation de la console à gauche, sélectionnez EC2 Dashboard (Tableau de bord EC2). 
    47. Dans la section Lancer des instances, choisissez Lancer des instances. 
La page Lancer une instance s’affiche.
Tâche 6.2 : ajouter des identifications à l’instance
Vous pouvez utiliser les identifications pour classer vos ressources AWS de différentes manières, par exemple, par objectif, par propriétaire ou par environnement. Vous pouvez appliquer des identifications à la majorité des ressources du Cloud AWS. Chaque balise se compose d’une clé et d’une valeur que vous définissez. Les identifications sont utilisées lorsque vous devez gérer plusieurs ressources du même type. Vous pouvez rapidement rechercher et identifier une ressource spécifique à l’aide de l’identification que vous y avez appliquée.
Dans cette tâche, vous ajouterez une identification à l’instance Amazon EC2.
    48. Localisez la section Name and tags (Nom et identifications). 
    49. Dans le champ Nom, saisissez 
    49. Public Instance
       . 
Remarque : Aucune balise d’instance supplémentaire n’est requise pour cet atelier.
Tâche 6.3 : sélectionner une AMI
Dans cette tâche, vous sélectionnerez une Amazon Machine Image (AMI). L’AMI contient une copie du volume de disque utilisé pour lancer l’instance.
    50. Localisez la section Application and OS Images (Amazon Machine Image) (Images d’application et de système d’exploitation [Amazon Machine Image]). 
    51. Assurez-vous que l’option Amazon Linux est sélectionnée comme système d’exploitation. 
    52. Assurez-vous que l’option Amazon Linux 2023 AMI est sélectionnée dans le menu déroulant. 
Tâche 6.4 : sélectionner le type d’instance Amazon EC2
Chaque type d’instance alloue une combinaison spécifique de CPU virtuelles (vCPU), de mémoire, de stockage sur disque et de performances réseau.
Pour cet atelier, utilisez un type d’instance t3.micro. Ce type d’instance possède 2 vCPU et 1 Gio de mémoire.
    53. Localisez la section Instance type (Type d’instance). 
    54. Dans le menu déroulant Instance type (Type d’instance), sélectionnez t3.micro. 
Tâche 6.5 : configurer une paire de clés à des fins de connexion
    55. Localisez la section Paire de clés (connexion). 
    56. Dans le menu déroulant Key pair name – required (Nom de la paire de clés – requis), sélectionnez Continuer sans paire de clés (déconseillé) . 
Tâche 6.6 : configurer le réseau de l’instance
    57. Localisez la section Paramètres réseau. 
    58. Sélectionnez Modifier. 
    59. Configurez les éléments suivants depuis les menus déroulants : 
    • VPC - required (VPC – requis) : sélectionnez Lab VPC (VPC d’atelier). 
    • Sous-réseau : sélectionnez Sous-réseau public. 
    • Attribuer automatiquement l’adresse IP publique : sélectionnez Activer. 
Tâche 6.7 : configurer des groupes de sécurité pour l’instance
Vous pouvez utiliser des groupes de sécurité pour définir le trafic autorisé/refusé et le trafic entrant/sortant de l’interface réseau élastique. L’interface réseau est attachée à une instance Amazon EC2. Le port 80 est le port par défaut du trafic HTTP ; le serveur web que vous lancez dans cet atelier en a besoin pour fonctionner correctement.
    60. Pare-feu (groupes de sécurité) : sélectionnez Sélectionner un groupe de sécurité existant. 
    61. Dans le menu déroulant Common security groups (Groupes de sécurité courants), sélectionnez le groupe de sécurité dont le nom ressemble à Public SG (SG public). 
Tâche 6.8 : ajouter un stockage
Vous pouvez utiliser la section Configure storage (Configurer le stockage) pour spécifier ou modifier les options de stockage pour l’instance et ajouter des volumes de disque Amazon Elastic Block Store (Amazon EBS) supplémentaires attachés à l’instance. La taille et les performances des volumes EBS peuvent être configurées.
Dans cet atelier, nous n’aurez besoin que des paramètres de stockage par défaut. Aucune modification n’est requise.
Tâche 6.9 : configurer les données utilisateur
    62. Localisez et développez la section Détails avancés. 
    63. Dans le menu déroulant Profil d’instance IAM, sélectionnez le rôle dont le nom ressemble à EC2InstProfile. 
Remarque : Pour installer et configurer la nouvelle instance en tant que serveur web, vous devez fournir un script de données utilisateur qui s’exécutera automatiquement au lancement de l’instance.
    64. Dans la section User data - optional (Données utilisateur – facultatif), copiez et collez ce qui suit : 
#!/bin/bash
# To connect to your EC2 instance and install the Apache web server with PHP
yum update -y
yum install -y httpd php8.1
systemctl enable httpd.service
systemctl start httpd
cd /var/www/html
wget  https://us-west-2-tcprod.s3.amazonaws.com/courses/ILT-TF-200-ARCHIT/v7.8.1.prod-29f8a5ae/lab-2-VPC/scripts/instanceData.zip
unzip instanceData.zip
Les valeurs par défaut des paramètres restants sur la page peuvent être conservées.
Tâche 6.10 : vérifier le lancement de l’instance
Prenez le temps de vérifier que la configuration de l’instance Amazon EC2 que vous êtes sur le point de lancer est correcte.
    65. Localisez la section Résumé. 
    66. Sélectionnez Lancer l’instance. 
La page Lancer une instance s’affiche.
Votre instance Amazon EC2 est à présent lancée et configurée telle que vous l’avez spécifiée.
    67. Sélectionnez Afficher toutes les instances. 
La console Amazon EC2 s’affiche.
    68. Cliquez de temps en temps sur le bouton d’actualisation de la console et attendez que la valeur Instance state (État de l’instance) soit Running (En cours d’exécution) pour la propriété Public Instance (Instance publique), puis attendez que la propriété Status check to pass (Vérification de l’état de réussite) soit 3/3 checks passed (3/3 vérifications réussies). 
Remarque : L’état de l’instance Amazon EC2 nommée Public Instance (Instance publique) est initialement En attente. L’état de l’instance passe ensuite à En cours d’exécution, ce qui indique que le démarrage de l’instance est terminé.
Félicitations ! Vous venez de lancer une instance Amazon EC2 dans un sous-réseau public.

Tâche 7 : se connecter à une instance publique via HTTP
Au cours de cette tâche, vous allez vous connecter à l’instance publique et lancer la page de base du serveur web Apache. Les règles entrantes précédemment ajoutées qui autorisent l’accès HTTP (port 80) vous permettront de vous connecter au serveur web exécutant Apache.
    69. Dans le volet de navigation de gauche, choisissez Instances. 
    70. Sélectionnez Public Instance (Instance publique). 
    71. Sélectionnez l’onglet Networking (Réseau) dans le volet inférieur. 
Remarque : si vous devez élargir une section de la console, vous pouvez redimensionner les bords horizontaux des conteneurs affichés sur la console.
    72. Localisez la valeur Public IPv4 DNS (DNS IPv4 public). 
    73. Copiez la valeur du DNS public. Ne sélectionnez pas l’option Open address (Adresse ouverte), car HTTPS n’est pas configuré pour cet environnement d’atelier. 
    74. Ouvrez un nouvel onglet de navigateur et collez la valeur du DNS public pour Public Instance (Instance publique) dans la barre de l’adresse URL. 
La page web hébergée sur l’instance Amazon EC2 s’affiche. La page affiche l’ID d’instance et la zone de disponibilité AWS dans laquelle se trouve l’instance Amazon EC2.
    75. Fermez l’onglet du navigateur et revenez à la console. 
Félicitations ! Vous venez de lancer un serveur web Apache dans le sous-réseau public et de tester la connexion HTTP. Vous pouvez fermer l’onglet en toute sécurité et retourner dans la console.

Tâche 8 : se connecter à l’instance Amazon EC2 dans le sous-réseau public par le biais de Session Manager
Dans cette tâche, vous vous connecterez à votre instance Amazon EC2 dans le sous-réseau public à l’aide de Session Manager.
Informations supplémentaires : Session Manager est une fonction AWS Systems Manager entièrement gérée qui vous permet de gérer vos instances Amazon EC2 par le biais d’un shell interactif basé sur un navigateur en un clic ou de l’Interface de la ligne de commande AWS (AWS CLI). Vous pouvez utiliser Session Manager pour démarrer une session avec une instance Amazon EC2 dans votre compte. Une fois la session démarrée, vous pouvez exécuter des commandes bash de la même façon que vous le feriez avec tout autre type de connexion.
    76. En haut de la Console de gestion AWS, dans la barre de recherche, recherchez et sélectionnez 
    76. EC2
       .
    77. Dans le volet de navigation de gauche, choisissez Instances.
    78. Sélectionnez Public Instance (Instance publique) et sélectionnez Se connecter.
La page Se connecter à l’instance s’affiche.
    79. Sélectionnez l’onglet Session Manager. 
Informations supplémentaires : Session Manager vous permet de vous connecter à des instances Amazon EC2 sans exposer le port SSH sur votre pare-feu ou le groupe de sécurité Amazon VPC. Pour plus d’informations, consultez AWS Systems Manager Session Manager.
    80. Sélectionnez Se connecter. 
Un nouvel onglet ou une nouvelle fenêtre du navigateur s’ouvre avec une connexion à la Public Instance (Instance publique).
Remarque : le service Session Manager n’est pas mis à jour en temps réel. Si vous rencontrez des erreurs avec Session Manager lors de la connexion à une instance Amazon EC2 que vous venez de lancer, patientez quelques minutes, le temps que l’instance se lance, effectue des vérifications d’état et communique avec le service Session Manager, avant de réessayer d’ouvrir une connexion de session.
    81. Commande : saisissez la commande suivante pour parcourir l’annuaire d’accueil (/home/ssm-user/) et testez la connectivité web à l’aide de la commande cURL. 
cd ~
curl -I https://aws.amazon.com/training/
Sortie attendue :
HTTP/2 200
content-type: text/html;charset=UTF-8
server: Server
date: Wed, 19 Apr 2023 14:43:47 GMT
x-amz-rid: 6HVPS1JY1XW2S1K34Q3Z
set-cookie: aws-priv=eyJ2IjoxLCJldSI6MCwic3QiOjB9; Version=1; Comment="Anonymous cookie for privacy regulations"; Domain=.aws.amazon.com; Max-Age=31536000; Expires=Thu, 18-Apr-2024 14:43:47 GMT; Path=/; Secure
set-cookie: aws_lang=en; Domain=.amazon.com; Path=/
x-frame-options: SAMEORIGIN
x-xss-protection: 1; mode=block
strict-transport-security: max-age=63072000
x-content-type-options: nosniff
x-amz-id-1: 6HVPS1JY1XW2S1K34Q3Z
last-modified: Thu, 30 Mar 2023 15:58:02 GMT
content-security-policy-report-only: default-src *; connect-src *; font-src * data:; frame-src *; img-src * data:; media-src *; object-src *; script-src *; style-src 'unsafe-inline' *; report-uri https://prod-us-west-2.csp-report.marketing.aws.dev/submit
vary: accept-encoding,Content-Type,Accept-Encoding,User-Agent
x-cache: Miss from cloudfront
via: 1.1 88c333921d5c405e037b84bb8c2dc33e.cloudfront.net (CloudFront)
x-amz-cf-pop: GRU3-P1
x-amz-cf-id: 89R1wtM9vYV0kIQXrEVkcoNzg_C3UfQJIEVkC5BA3xiIH3FD0nVnYw==
Félicitations ! Vous venez de vous connecter à votre instance publique à l’aide de Session Manager. Vous pouvez fermer l’onglet en toute sécurité et retourner dans la console.

Tâche 9 : créer une passerelle NAT et configurer le routage dans le sous-réseau privé
Dans cette tâche, vous créerez une passerelle NAT, ainsi qu’une table de routage pour acheminer le trafic non local vers la passerelle NAT. Vous attacherez ensuite la table de routage au sous-réseau privé. Vous pouvez utiliser une passerelle NAT pour autoriser les instances d’un sous-réseau privé à se connecter à Internet ou à d’autres services AWS, en empêchant Internet d’initier une connexion avec ces instances.
Remarque : pour créer une passerelle NAT, vous devez spécifier le sous-réseau public dans lequel la passerelle NAT devra se trouver. Vous devez également spécifier une adresse IP élastique à associer à la passerelle NAT lorsque vous la créerez. Vous ne pouvez pas modifier l’adresse IP élastique après l’avoir associée à la passerelle NAT. Lorsque vous avez créé une passerelle NAT, vous devez mettre à jour la table de routage associée à un ou plusieurs de vos sous-réseaux privés pour diriger le trafic Internet vers la passerelle NAT. Cela permet aux instances de vos sous-réseaux privés de communiquer avec Internet.
    82. Revenez à l’onglet du navigateur de la Console de gestion AWS.
    83. En haut de la Console de gestion AWS, dans la zone de recherche, recherchez et sélectionnez 
    83. VPC
       .
    84. Dans le volet de navigation de gauche, sélectionnez Passerelles NAT.
    85. Sélectionnez Créer une passerelle NAT et configurez les éléments suivants :
    • Name - optional (Nom – facultatif) : saisissez 
    • Lab NGW
      . 
    • Sous-réseau : sélectionnez Sous-réseau public dans le menu déroulant. 
    • Pour ID d’allocation d’adresses IP Elastic, sélectionnez Allouer une adresse IP Elastic. 
    86. Sélectionnez Créer une passerelle NAT. 
Le message NAT gateway nat-xxxxxxx | Lab NGW was created successfully. (La passerelle NAT nat-xxxxxxx | Lab NGW a été créée avec succès) s’affiche en haut de l’écran.
Lors de l’étape suivante, vous créerez une nouvelle table de routage pour un sous-réseau privé qui redirige le trafic non local vers la passerelle NAT.
    87. Dans le volet de navigation de gauche, sélectionnez Tables de routage. 
    88. Sélectionnez Créer une table de routage et configurez les éléments suivants : 
    • Name - optional (Nom – facultatif) : saisissez 
    • Private Route Table
      . 
    • VPC : sélectionnez Lab VPC (VPC d’atelier) dans la liste déroulante. 
    89. Sélectionnez Créer une table de routage. 
Le message Route table rtb-xxxxxxx | Private Route Table was created successfully. (La table de routage rtb-xxxxxxx | Private Route Table a été créée avec succès) s’affiche en haut de l’écran.
La table de routage privée est créée et sa page détaillée s’affiche.
    90. Sélectionnez l’onglet Routes (Acheminements). 
Un acheminement dirige actuellement l’ensemble du trafic localement.
Vous allez maintenant ajouter un acheminement pour envoyer du trafic Internet par le biais de la passerelle NAT.
    91. Choisissez Modifier des routes.
    92. Sélectionnez Ajouter une route et configurez les éléments suivants :
    • Destination : saisissez 
    • 0.0.0.0/0
      . 
    • Cible : sélectionnez Passerelle NAT dans le menu déroulant, puis sélectionnez l’ID de la passerelle NAT affichée. 
    93. Sélectionnez Enregistrer les modifications. 
Le message Updated routes for rtb-xxxxxxx / Private Route Table successfully (Les acheminements de la table de routage privée rtb-xxxxxxx ont été mis à jour avec succès) s’affiche en haut de l’écran.
    94. Sélectionnez l’onglet Associations de sous-réseau. 
    95. Sélectionnez Modifier des associations de sous-réseau. 
    96. Sélectionnez Sous-réseau privé. 
    97. Sélectionnez Enregistrer les associations. 
Le message You have successfully updated subnet associations for rtb-xxxxxxx / Private Route Table. (Vous avez mis à jour les associations de sous-réseau de la table de routage rtb-xxxxxxx / Private Route Table avec succès.) s’affiche en haut de l’écran.
Cet acheminement envoie le trafic Internet provenant du sous-réseau privé vers la passerelle NAT qui se trouve dans la même zone de disponibilité.
Félicitations ! Vous venez de créer la passerelle NAT et de configurer la table de routage privée.

Tâche 10 : créer un groupe de sécurité pour les ressources privées
Dans cette tâche, vous créerez un groupe de sécurité qui autorise le trafic HTTP entrant provenant de ressources attribuées au groupe de sécurité public. Dans une architecture à plusieurs niveaux, les ressources dans un sous-réseau privé ne devraient pas être accessibles directement depuis Internet. Cependant, un cas d’utilisation courant consiste à acheminer le trafic web depuis des ressources accessibles publiquement vers des ressources privées.
Informations supplémentaires : Lorsque vous spécifiez un groupe de sécurité comme source pour une règle, le trafic est autorisé depuis les interfaces réseau qui sont associées au groupe de sécurité source pour le protocole et le port spécifiés. Le trafic entrant est autorisé sur la base des adresses IP privées des interfaces réseau qui sont associées au groupe de sécurité source (et non aux adresses IP publiques ou adresses IP élastiques). Le fait d’ajouter un groupe de sécurité en tant que source n’ajoute pas des règles provenant du groupe de sécurité source.
    98. Dans le volet de navigation de gauche, sélectionnez Groupes de sécurité. 
    99. Sélectionnez Créer un groupe de sécurité et configurez les éléments suivants : 
    • Nom de groupe de sécurité : saisissez 
    • Private SG
. 
    • Description : saisissez 
    • Allows incoming traffic to private instance using public security group
      . 
    • VPC : sélectionnez Lab VPC (VPC d’atelier) dans la liste déroulante. 
    100. Dans la section Règles entrantes, sélectionnez Ajouter une règle et configurez les éléments suivants : 
    • Type : sélectionnez HTTP. 
    • Source : sélectionnez Personnalisé. 
        ◦ Dans la zone à droite de Personnalisée, saisissez 
        ◦ sg
          . 
        ◦ Sélectionnez Public SG (SG public) dans la liste. 
    101. Dans la section Tags - optional (Balises – facultatif), sélectionnez Ajouter une nouvelle balise et configurez les éléments suivants : 
    • Clé : saisissez 
    • Name
. 
    • Valeur : saisissez 
    • Private SG
      . 
    102. Sélectionnez Créer un groupe de sécurité. 
Le message Security group (sg-xxxxxxx | Private SG) was created successfully (Le groupe de sécurité [sg-xxxxxxx | Private SG] a été créé avec succès) s’affiche en haut de l’écran.
Félicitations ! Vous venez de créer le groupe de sécurité privé.

Tâche 11 : lancer une instance Amazon EC2 dans un sous-réseau privé
Dans cette tâche, vous lancerez une instance Amazon EC2 dans un sous-réseau privé.
Informations supplémentaires : les instances privées peuvent acheminer leur trafic par le biais d’une passerelle NAT ou d’une instance NAT pour accéder à Internet. Les instances privées utilisent l’adresse IP publique de la passerelle NAT ou de l’instance NAT pour parcourir Internet. La passerelle NAT ou l’instance NAT autorise la communication entrante, mais n’autorise pas les machines sur Internet à initier une connexion aux instances dont les adresses sont privées.
    103. En haut de la Console de gestion AWS, dans la barre de recherche, recherchez et sélectionnez 
    103. EC2
       . 
La console Amazon EC2 s’affiche.
Tâche 11.1 : commencer la configuration de l’instance
    104. Sélectionnez EC2 Dashboard (Tableau de bord EC2) dans le menu de navigation de la console sur la gauche. 
    105. Dans la section Lancer une instance, choisissez Lancer une instance. 
La page Lancer une instance s’affiche. Dans cette tâche, vous ajouterez une identification à l’instance Amazon EC2.
    106. Localisez la section Name and tags (Nom et identifications). 
    107. Saisissez 
    107. Private Instance
       dans le champ Nom. 
Remarque : Aucune balise d’instance supplémentaire n’est requise pour cet atelier.
Tâche 11.3 : sélectionner une AMI
Dans cette tâche, vous choisirez une AMI. L’AMI contient une copie du volume de disque utilisé pour lancer l’instance.
    108. Localisez la section Application and OS Images (Amazon Machine Image) (Images d’application et de système d’exploitation [Amazon Machine Image]). 
    109. Assurez-vous que l’option Amazon Linux est sélectionnée comme système d’exploitation. 
    110. Assurez-vous que l’option Amazon Linux 2023 AMI est sélectionnée dans le menu déroulant. 
Tâche 11.4 : sélectionner le type d’instance Amazon EC2
Chaque type d’instance alloue une combinaison spécifique de vCPU, de mémoire, de stockage sur disque et de performances réseau.
Pour cet atelier, utilisez un type d’instance t3.micro. Ce type d’instance possède 2 vCPU et 1 Gio de mémoire.
    111. Localisez la section Instance type (Type d’instance). 
    112. Sélectionnez t3.micro dans le menu déroulant Instance type (Type d’instance). 
Tâche 11.5 : configurer une paire de clés à des fins de connexion
    113. Localisez la section Paire de clés (connexion). 
    114. Sélectionnez Proceed without a key pair (Not recommended) (Continuer sans paire de clés [Non recommandé]) dans le menu déroulant Key pair name – required (Nom de la paire de clés – requis). 
Tâche 11.6 : configurer le réseau de l’instance
    115. Localisez la section Paramètres réseau. 
    116. Sélectionnez Modifier et configurez les éléments suivants depuis le menu déroulant : 
    • VPC - required (VPC – requis) : sélectionnez Lab VPC (VPC d’atelier). 
    • Sous-réseau : sélectionnez Sous-réseau privé. 
    • Attribuer automatiquement l’adresse IP publique : sélectionnez Désactiver. 
Tâche 11.7 : configurer des groupes de sécurité pour l’instance
    117. Pour Pare-feu (groupes de sécurité) : sélectionnez Sélectionner un groupe de sécurité existant. 
    118. Sélectionnez le groupe de sécurité dont le nom ressemble à Private SG (SG privé) dans le menu déroulant Common security groups (Groupes de sécurité courants). 
Tâche 11.8 : ajouter un stockage
Vous pouvez utiliser la section Configure storage (Configurer le stockage) pour spécifier ou modifier les options de stockage pour l’instance et ajouter des volumes de disque Amazon Elastic Block Store (Amazon EBS) supplémentaires attachés à l’instance. La taille et les performances des volumes EBS peuvent être configurées.
Dans cet atelier, nous n’aurez besoin que des paramètres de stockage par défaut. Aucune modification n’est nécessaire.
Tâche 11.9 : configurer le profil d’instance IAM
    119. Localisez et développez la section Détails avancés. 
    120. Sélectionnez le rôle EC2InstProfile dans le menu déroulant Profil d’instance IAM. 
Les valeurs par défaut des paramètres restants sur la page peuvent être conservées.
Tâche 11.10 : configurer les données utilisateur
    121. Localisez et développez la section Détails avancés. 
    122. Dans le menu déroulant Profil d’instance IAM, sélectionnez le rôle dont le nom ressemble à EC2InstProfile. 
Remarque : Pour installer et configurer la nouvelle instance en tant que serveur web, vous devez fournir un script de données utilisateur qui s’exécutera automatiquement au lancement de l’instance.
    123. Dans la section User data - optional (Données utilisateur – facultatif), copiez et collez ce qui suit : 
#!/bin/bash
# To connect to your EC2 instance and install the Apache web server with PHP
yum update -y
yum install -y httpd php8.1
systemctl enable httpd.service
systemctl start httpd
cd /var/www/html
wget  https://us-west-2-tcprod.s3.amazonaws.com/courses/ILT-TF-200-ARCHIT/v7.8.1.prod-29f8a5ae/lab-2-VPC/scripts/instanceData.zip
unzip instanceData.zip
Les valeurs par défaut des paramètres restants sur la page peuvent être conservées.
Tâche 11.11 : vérifier le lancement de l’instance
Prenez un moment pour vérifier que la configuration de l’instance Amazon EC2 que vous êtes sur le point de lancer est correcte.
    124. Localisez la section Résumé. 
    125. Sélectionnez Lancer l’instance. 
La page Lancer une instance s’affiche.
Votre instance Amazon EC2 est à présent lancée et configurée telle que vous l’avez spécifiée.
    126. Sélectionnez Afficher toutes les instances. 
La console Amazon EC2 s’affiche.
L’état de l’instance Amazon EC2 nommée Private Instance (Instance privée) est initialement En attente. L’état passe ensuite à En cours d’exécution, ce qui indique que le démarrage de l’instance est terminé.
    127. Cliquez de temps à autre sur le bouton d’actualisation de la console et attendez que la valeur État de l’instance passe à En cours d’exécution. 
Félicitations ! Vous venez de lancer une instance Amazon EC2 dans un sous-réseau privé.

Tâche 12 : se connecter à l’instance Amazon EC2 dans le sous-réseau privé
Dans cette tâche, vous vous connecterez à l’instance Amazon EC2 dans le sous-réseau privé à l’aide de Session Manager.
    128. Dans le volet de navigation de gauche, choisissez Instances. 
    129. Sélectionnez Private Instance (Instance privée), puis sélectionnez Se connecter. 
La page Se connecter à l’instance s’affiche.
    130. Sélectionnez l’onglet Session Manager. 
    131. Sélectionnez Se connecter. 
Un nouvel onglet ou une nouvelle fenêtre du navigateur s’ouvre avec une connexion à Private Instance (Instance privée).
Remarque : le service Session Manager n’est pas mis à jour en temps réel. Si vous rencontrez des erreurs avec Session Manager lors de la connexion à une instance Amazon EC2 que vous venez de lancer, patientez quelques minutes, le temps que l’instance se lance, effectue des vérifications d’état et communique avec le service Session Manager, avant de réessayer d’ouvrir une connexion de session.
    132. Commande : saisissez la commande suivante pour parcourir l’annuaire d’accueil (/home/ssm-user/) et testez la connectivité web à l’aide de la commande cURL. 
cd ~
curl -I https://aws.amazon.com/training/
Sortie attendue :
HTTP/2 200
content-type: text/html;charset=UTF-8
server: Server
date: Wed, 19 Apr 2023 14:59:09 GMT
x-amz-rid: AZPXJ57K93ERATZV588Z
set-cookie: aws-priv=eyJ2IjoxLCJldSI6MCwic3QiOjB9; Version=1; Comment="Anonymous cookie for privacy regulations"; Domain=.aws.amazon.com; Max-Age=31536000; Expires=Thu, 18-Apr-2024 14:59:08 GMT; Path=/; Secure
set-cookie: aws_lang=en; Domain=.amazon.com; Path=/
x-frame-options: SAMEORIGIN
x-xss-protection: 1; mode=block
strict-transport-security: max-age=63072000
x-content-type-options: nosniff
x-amz-id-1: AZPXJ57K93ERATZV588Z
last-modified: Thu, 30 Mar 2023 15:58:02 GMT
content-security-policy-report-only: default-src *; connect-src *; font-src * data:; frame-src *; img-src * data:; media-src *; object-src *; script-src *; style-src 'unsafe-inline' *; report-uri https://prod-us-west-2.csp-report.marketing.aws.dev/submit
vary: accept-encoding,Content-Type,Accept-Encoding,User-Agent
x-cache: Miss from cloudfront
via: 1.1 fb6a4eca9caced7b791557c24b8c6606.cloudfront.net (CloudFront)
x-amz-cf-pop: GRU3-P1
x-amz-cf-id: Tjphb1UhSXmtyHvybuq4QIFwzTurEI0g_saLB2nLjlYRiBbHbqn85Q==
    133. Fermez l’onglet de Session Manager et revenez à la console. 
Félicitations ! Vous vous êtes connecté à une instance privée à l’aide de Session Manager.

Tâche facultative 1 : dépanner la connectivité entre l’instance privée et l’instance publique
Dans cette tâche facultative, vous utiliserez le protocole ICMP (Internet Control Message Protocol) pour valider l’accessibilité réseau d’une instance privée depuis l’instance publique.
Remarque : Cette tâche est facultative, au cas où il vous resterait du temps avant la fin de l’atelier. Vous pouvez effectuer cette tâche, ou passer directement à la fin de l’atelier.
    134. Revenez à l’onglet du navigateur de la Console de gestion AWS. 
    135. Dans le volet de navigation de gauche, choisissez Instances. 
    136. Sélectionnez Private Instance (Instance privée). 
    137. Dans l’onglet Détails, copiez la valeur de Private IPv4 address (Adresse IPv4 privée) dans le presse-papier. 
Remarque : pour copier l’adresse IPv4 privée, passez votre souris dessus et sélectionnez l’icône de copie .
    138. Décochez Private Instance (Instance privée). 
    139. Sélectionnez Public Instance (Instance publique). 
    140. Sélectionnez Se connecter. 
La page Se connecter à l’instance s’affiche.
    141. Sélectionnez l’onglet Session Manager. 
    142. Sélectionnez Se connecter. 
Un nouvel onglet ou une nouvelle fenêtre du navigateur s’ouvre avec une connexion à la Public Instance (Instance publique).
Tout d’abord, utilisez une commande curl pour récupérer un fichier d’en-tête et vérifier que l’application web hébergée sur l’instance privée est accessible depuis l’instance publique.
    143. Commande : copiez la commande suivante dans votre bloc-notes. Remplacez PRIVATE_IP par la valeur de la Private IPv4 address (Adresse IPv4 privée) pour la Private Instance (Instance privée) : 
curl PRIVATE_IP
Sortie attendue :
<html><body><h1>It works!</h1></body></html>
    144. Commande : copiez la commande suivante dans votre bloc-notes. Remplacez PRIVATE_IP par la valeur de la Private IPv4 address (Adresse IPv4 privée) pour la Private Instance (Instance privée) : 
ping PRIVATE_IP
    145. Commande : copiez et collez la commande mise à jour dans votre terminal et appuyez sur Entrée. 
Cette commande sert uniquement à titre d’exemple. N’utilisez pas la commande suivante.
ping 10.0.2.131
    146. Après quelques secondes, mettez fin à la requête ping ICMP en appuyant sur CTRL + C. 
The ping request to the private instance fails (Échec de la requête ping à l’instance privée). Votre défi est d’utiliser la console et de déterminer la règle entrante appropriée requise dans le Private SG (SG privé) pour pouvoir envoyer une requête ping à l’instance privée.
En cas de difficulté à effectuer la tâche facultative, consultez la section Solution de la tâche facultative à la fin de l’atelier.

Tâche facultative 2 : récupérer les métadonnées de l’instance
Dans cette tâche facultative, vous exécuterez les commandes de métadonnées d’instance sur AWS CLI à l’aide d’un outil tel que cURL. Les métadonnées de l’instance sont disponibles depuis votre instance Amazon EC2 en cours d’exécution. Cela peut être utile lorsque vous écrivez des scripts à exécuter depuis votre instance Amazon EC2.
Remarque : Cette tâche est facultative, au cas où il vous resterait du temps avant la fin de l’atelier. Vous pouvez effectuer cette tâche, ou passer directement à la fin de l’atelier.
    147. Revenez à l’onglet de navigateur dans lequel la console de gestion AWS est ouverte.
    148. Dans le volet de navigation de gauche, choisissez Instances.
    149. Sélectionnez Public Instance (Instance publique).
    150. Sélectionnez Se connecter.
La page Se connecter à l’instance s’affiche.
    151. Sélectionnez l’onglet Session Manager. 
    152. Sélectionnez Se connecter. 
Un nouvel onglet ou une nouvelle fenêtre du navigateur s’ouvre avec une connexion à la Public Instance (Instance publique).
    153. Commande : pour afficher toutes les catégories de métadonnées d’instance depuis une instance en cours d’exécution, exécutez la commande suivante. 
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` \
&& curl -H "X-aws-ec2-metadata-token: $TOKEN" -v http://169.254.169.254/latest/meta-data/
    154. Commande : exécutez la commande suivante pour récupérer le public-hostname (l’un des éléments de métadonnées de niveau supérieur obtenus par la commande précédente). 
curl http://169.254.169.254/latest/meta-data/public-hostname -H "X-aws-ec2-metadata-token: $TOKEN"
Remarque ! l’adresse IP 169.254.169.254 est une adresse locale de liaison et n’est valide que depuis l’instance.
Vous avez appris à récupérer des métadonnées d’instance depuis votre instance Amazon EC2 en cours d’exécution.

Conclusion
La création d’un VPC avec des sous-réseaux publics et privés vous permet de lancer, avec flexibilité, des tâches et des services dans un sous-réseau public ou privé. Les tâches et les services des sous-réseaux privés peuvent accéder à Internet par le biais d’une passerelle NAT.
Félicitations ! Vous avez réussi à :
    • créér un Amazon VPC ; 
    • créer des sous-réseaux publics et privés ; 
    • créer une passerelle Internet ; 
    • configurer une table de routage et l’associer à un sous-réseau ; 
    • créer une instance Amazon EC2 et la rendre accessible publiquement ; 
    • isoler une instance Amazon EC2 dans un sous-réseau privé ; 
    • créer des groupes de sécurité et les affecter à des instances Amazon EC2 ; 
    • vous connecter aux instances Amazon EC2 à l’aide de Session Manager. 
Terminer l’atelier
Suivez ces étapes pour fermer la console et mettre fin à votre atelier.
    155. Revenez à AWS Management Console.
    156. Dans la barre de navigation, choisissez AWSLabsUser, puis choisissez Sign Out (Se connecter).
    157. Choisissez End lab (Terminer l’atelier) puis confirmez que vous souhaitez mettre fin à votre atelier.
Ressources supplémentaires
    • Qu’est-ce qu’Amazon VPC ? 
    • Sous-réseaux de votre VPC 
    • Connecter à l’Internet à l’aide d’une passerelle Internet 
    • Configuration des tables de routage 
    • Contrôler le trafic vers les ressources à l’aide de groupes de sécurité 
    • Passerelles NAT 
    • Adresses IPv4 publiques 
    • Comprendre les bases du réseau d’adresses IPv6 sur AWS 
Solution de la tâche facultative
    158. Revenez à l’onglet du navigateur de la Console de gestion AWS.
    159. En haut de la Console de gestion AWS, dans la zone de recherche, recherchez et sélectionnez 
    159. EC2
       .
    160. Dans le volet de navigation de gauche, sélectionnez Groupes de sécurité.
    161. Sélectionnez Private SG (SG privé).
    162. Sélectionnez Actions , puis Modifier les règles entrantes.
    163. Sur la page Modifier les règles entrantes, dans la section Règles entrantes, sélectionnez Ajouter une règle et configurez les éléments suivants :
    • Type : sélectionnez Custom ICMP - IPV4 (ICMP – IPV4 personnalisée). 
    • Source : sélectionnez Personnalisé. 
        ◦ Dans la zone à droite de Personnalisé, saisissez 
        ◦ sg
          . 
        ◦ Sélectionnez Public SG (SG public) dans la liste. 
    164. Sélectionnez Enregistrer les règles. 
    165. Sélectionnez le lien Optional Task (Tâche facultative) pour accédez à la Optional Task (Tâche facultative) et répétez les étapes. La Public Instance (Instance publique) doit désormais pouvoir envoyer une requête à la Private Instance (Instance privée). 
Pour plus d’informations sur AWS Training and Certification, consultez https://aws.amazon.com/training/.
Vos commentaires sont les bienvenus et sont appréciés. Si vous souhaitez partager des commentaires, des suggestions ou des corrections, veuillez fournir des détails dans notre Formulaire de contact AWS Training and Certification.
Beau travail ! Aidez-nous à améliorer le Builder Labs en seulement 2 minutes.
Démarrer l'enquête


ConfidentialitéConditions d'utilisation du site
Commentaires

