/usr/local/aws-cli/v2/current/bin/aws

#pour supprimer aws
sudo rm /usr/local/bin/aws sudo rm /usr/local/bin/[votre_chemin]

#créer un VPC dans l’interface de ligne de commande, exécutez cette commande :

aws ec2 create-vpc --cidr-block 192.168.0.0/16

   "VpcId": "vpc-03ca7d2b3b3f9dea9",
   
# créez deux sous-réseaux dont un public pour le rendre accessible à partir d’Internet. Pour cela, utilisez la commande ci-dessous :
  Remplacez VCPID par le VpcId généré à la création de votre VPC.

aws ec2 create-subnet --vpc-id VPCID --cidr-block 192.168.1.0/24
aws ec2 create-subnet --vpc-id vpc-03ca7d2b3b3f9dea9 --cidr-block 192.168.0.0/24

       sousreseau public
   "SubnetId": "subnet-0725ea5f5d3e9f85f",

#Créez maintenant un deuxième sous-réseau avec le bloc CIDR 10.0.0.0/24. 

 aws ec2 create-subnet --vpc-id VPCID --cidr-block 192.168.0.0/24


  "SubnetId": "subnet-0043c08ef8381e698",

 aws ec2 create-subnet --vpc-id VPCID --cidr-block 192.168.0.0/24
 
 #Création d’une passerelle Internet
aws ec2 create-internet-gateway
Pour permettre aux sous-réseaux de communiquer avec Internet, il est nécessaire de créer une passerelle internet* (_Internet Gateway_) et de *l'associer à notre VPC
"InternetGatewayId": "igw-077b51e06a168f7c5",

#attachez cette passerelle Internet au VPC
aws ec2 attach-internet-gateway --vpc-id VPCID --internet-gateway-id INTERNETGATEWAYID

aws ec2 attach-internet-gateway --vpc-id vpc-03ca7d2b3b3f9dea9 --internet-gateway-id igw-077b51e06a168f7c5

#Création d’une table de routage
route allant du VPC vers la passerelle internet
aws ec2 create-route-table --vpc-id VPCID
aws ec2 create-route-table --vpc-id vpc-03ca7d2b3b3f9dea9

aws ec2 create-route --route-table-id ROUTETABLEID --destination-cidr-block 0.0.0.0/0 --gateway-id INTERNETGATEWAYID
aws ec2 create-route --route-table-id rtb-0cf61e671b431dee2 --destination-cidr-block 0.0.0.0/0 --gateway-id igw-077b51e06a168f7c5


aws ec2 describe-route-tables --route-table-id ROUTETABLEID
aws ec2 describe-route-tables --route-table-id rtb-0cf61e671b431dee2

route-table-id rtb-0cf61e671b431dee2


aws ec2 describe-subnets --filters "Name=vpc-id,Values=VPCID" --query "Subnets[*].{ID:SubnetId,CIDR:CidrBlock}"
aws ec2 describe-subnets --filters "Name=vpc-id,Values=vpc-03ca7d2b3b3f9dea9" --query "Subnets[*].{ID:SubnetId,CIDR:CidrBlock}"

#Association de la table de routage et modification du sous-réseau
 aws ec2 associate-route-table --subnet-id SUBNETID --route-table-id ROUTETABLEID
  aws ec2 associate-route-table --subnet-id subnet-0725ea5f5d3e9f85f --route-table-id rtb-0cf61e671b431dee2
  #Pour affecter une adresse IP publique IPv4 aux machines 
  Pour que les machines EC2 que nous allons instancier dans ce sous-réseaux soient affectées automatiquement à une adresse IP publique*, nous allons utiliser la commande 
 aws ec2 modify-subnet-attribute --subnet-id SUBNETID --map-public-ip-on-launch
 aws ec2 modify-subnet-attribute --subnet-id subnet-0725ea5f5d3e9f85f --map-public-ip-on-launch 
 aws ec2 modify-subnet-attribute --subnet-id subnet-0043c08ef8381e698 --map-public-ip-on-launch
  
  Les applications Web, les réseaux VPC auront au moins 2 sous-réseaux :

    Un sous-réseau public* : Ce réseau servira d'interface entre le réseau VPC et internet. C'est dans ce réseau en général que nous instancierons les *serveurs web* pour *communiquer avec internet.

    Un sous-réseau privé* : Ce réseau permet aux serveurs web du réseau public de *communiquer avec des serveurs de bases de données ou des serveurs back-end de nos applications* qui doivent être *protégés du trafic externe. Le seul client de ces serveurs devrait être le sous-réseau public.

#Création d’une paire de clés et d’un groupe de sécurité (fichier .pem)

L’étape la plus importante consiste à créer une paire de clés SSH pour accéder à notre nouvelle machine. Cette paire de clés doit être conservée précieusement par l’utilisateur afin que seules les personnes autorisées puisse accéder à l’instance EC2.
aws ec2 create-key-pair --key-name datascientest_keypair --query "KeyMaterial" --output text > "$(pwd)/datascientest_keypair.pem"


Un groupe de sécurité permet de définir les ports des machines que nous allons ouvrir pour autoriser les connexions SSH. Pour configurer le groupe de sécurité, utilisez les commandes ci-dessous :

aws ec2 create-security-group --group-name datascientest-SG --description "Groupe de securite pour acces SSH instance" --vpc-id vpc-03ca7d2b3b3f9dea9


"GroupId": "sg-0ada0509f7149da4a"

Le protocole et port que nous utiliserons pour nous connecter aux machines est TCP/22. 
aws ec2 authorize-security-group-ingress --group-id GROUPID --protocol tcp --port 22 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-id sg-0ada0509f7149da4a --protocol tcp --port 22 --cidr 0.0.0.0/0
Le bloc CIDR 0.0.0.0/0 est utilisé pour autoriser la connexion à ce port à toutes les adresses IPs IPv4.

#Exécution de l’instance EC2

AMI ID  recupere dans EC2 images catalogue des AMI 
ami-07db896e164bc4476 

recuperer nom  paires de clés EC2 pour se connecter aux instances via SSH.
paire de clé crée precedement
aws ec2 describe-key-pairs --query 'KeyPairs[*].KeyName' --output table
verifier permission du fichier cle
chmod 400 cle-datascientest-learn.pem




Récupérez le nom de votre paire de clés, l’ID du groupe de sécurité et l’ID du sous-réseau VPC public et utilisez-les au bon endroit dans la commande ci-dessous. Notez également l’InstanceId qui sera créé.

aws ec2 run-instances
  --image-id AMI_ID
  --count 1
  --instance-type t2.micro
  --key-name datacientest_keypair
  --security-group-ids SECURITYGROUPID
  --subnet-id SUBNETID
  --tag-specifications ResourceType=instance,Tags=[{Key=Name,Value=Datascientest}]
  
aws ec2 run-instances \
  --image-id ami-07db896e164bc4476 \
  --count 1 \
  --instance-type t2.micro \
  --key-name datascientest_keypair \
  --security-group-ids sg-0ada0509f7149da4a \
  --subnet-id subnet-0725ea5f5d3e9f85f \
  --tag-specifications 'ResourceType=instance,Tags=[{Key=Name,Value=Datascientest}]' \
  --dry-run


--dry-run       permet de ne pas reelemnt effectuer la commande
cela permet de verifier d'abord si cela fonctionne
faire ensuit ela meme commande sans dry run





--count 1* pour instancier qu'une seule machine 
et l'argument *--instance-type t2.micro pour définir la configuration CPU, mémoire et stockage de l'instance parmi les types d'instances disponibles.


#Affichage de l'instance   
aws ec2 describe-instances \
  --filters "Name=tag:Name,Values=Datascientest" \
  --query "Reservations[].Instances[].[InstanceId, PublicIpAddress, PrivateIpAddress]" \
  --output table

-----------------------------------------------------------
|                     DescribeInstances                    |
+---------------------+------------------+-----------------+
|  i-0f433a2ce79b50803|  35.180.196.104  |  192.168.1.243  |
+---------------------+------------------+-----------------+
IP publique : 35.180.196.104
IP privée : 192.168.1.243

aws ec2 describe-instances --instance-id INSTANCEID
aws ec2 describe-instances --instance-id i-0f433a2ce79b50803

sudo chmod 400 datascientest_keypair.pem
ssh -i "path to/datacientest_keypair.pem" ubuntu@[IP_public]
ssh -i "/home/ubuntu/aws/datascientest_keypair.pem" ubuntu@35.180.196.104


Si vous souhaitez l’arrêter sans la supprimer, vous pouvez utiliser l’instruction aws ec2 stop-instances --instance-ids INSTANCEID". 

i-0f433a2ce79b50803
aws ec2 terminate-instances --instance-ids INSTANCE_ID
aws ec2 terminate-instances --instance-ids i-0f433a2ce79b50803

Exo
aws ec2 create-key-pair --key-name hennebo --query "KeyMaterial" --output text > "$(pwd)/hennebo.pem"

aws ec2 create-security-group --group-name julien-SG --description "Mon groupe de securite julien"

aws ec2 authorize-security-group-ingress --group-name julien-SG --protocol tcp --port 22 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-name julien-SG --protocol tcp --port 80 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --group-name julien-SG --protocol tcp --port 443 --cidr 0.0.0.0/0


Créez une instance à partir d'une AMI Amazon Linux et lui associer le groupe de sécurité et la paire de clés créés plus haut. Le type de l'instance sera t2.micro.
ami-02d0b1ffa5f16402d   

aws ec2 run-instances --image-id ami-02d0b1ffa5f16402d --count 1 --instance-type t2.micro --key-name hennebo --security-group-ids sg-077726c48852234a2 --dry-run

sans dry run si ok  

#affichez l'adresse IP publique de l'instance en utilisant la commande aws ec2 describe-instances avec le filtre approprié.

--reservation-ids r-0d211ba8159f310f1 : Filtre les instances basées sur l'ID de réservation.
--reservation-ids:   r-0d211ba8159f310f1

aws ec2 describe-instances --filters "Name=reservation-id,Values=r-0d211ba8159f310f1" --query "Reservations[*].Instances[*].PublicIpAddress" --output text
ip publique
13.38.10.148

aws ec2 describe-instances --filters "Name=ip-address,Values=13.38.10.148" --query "Reservations[*].Instances[*].InstanceId" --output text
instance Id     i-073b015428da0ac9b


aws ec2 describe-instances --instance-ids <instance-id>
aws ec2 describe-instances --instance-ids i-073b015428da0ac9b

aws ec2 describe-security-groups
sg-08f9764547c1789dc

Ajouter une règle pour permettre le SSH (port 22) : Si la règle pour le port 22
aws ec2 authorize-security-group-ingress --group-id sg-08f9764547c1789dc --protocol tcp --port 22 --cidr 0.0.0.0/0


Connectez-vous via SSH à l'instance EC2
ec2-user est le nom de l'utilisateur dans une image Amazon Linux :
ssh -i "path to/hennebo.pem" ubuntu@[IP_public]
ssh -i "/home/ubuntu/aws/hennebo.pem" ec2-user@13.38.10.148

cd /var/www/html
aws ec2 authorize-security-group-ingress --group-id sg-08f9764547c1789dc --protocol tcp --port 80 --cidr 0.0.0.0/0
aws ec2 authorize-security-group-ingress --region eu-west-3 --group-id sg-08f9764547c1789dc --protocol tcp --port 80 --cidr 0.0.0.0/0   


#Installer Apache :
sudo yum install httpd -y

Démarrer Apache :
sudo systemctl start httpd
Activer Apache pour démarrer automatiquement au démarrage du système :
sudo systemctl enable httpd
Copier votre fichier index.html :
sudo cp /home/ec2-user/html/index.html /var/www/html/

# configure aws   à nouveau
  


    Vous pouvez créer une nouvelle image AMI à partir de l'instance EC2 que vous venez de créer afin de la répliquer dans la suite. Pour cela, utilisez la commande aws ec2 create-image avec les arguments :

        --instance-id suivi de l'ID de l'instance pour spécifier l'instance à copier.

        --name suivi d'une chaîne de caractères pour spécifier le nom que nous voulons à donner à cette image. Par exemple : "image-prenom-httpd".

        --no-reboot pour éviter de redémarrer l'instance actuelle lors de la création de l'image.

  aws ec2 create-image --instance-id INSTANCEID --name "image-prenom-httpd" --no-reboot --region eu-west-3
        
aws ec2 create-image --instance-id i-073b015428da0ac9b --name "image-julien-httpd" --no-reboot --region eu-west-3


aws ec2 describe-images
aws ec2 describe-images --filters "Name=name,Values=image-julien-httpd" --owners self --region eu-west-3


maintenant que vous avez créé une image AMI de votre instance, vous pouvez supprimer cette instance si vous n'en avez plus besoin. La suppression de l'instance ne supprimera pas l'AMI ou les volumes associés à cette image.

aws ec2 terminate-instances --instance-ids INSTANCEID
aws ec2 stop-instances --instance-ids i-073b015428da0ac9b --region eu-west-3

git remote set-url origin https://github.com/julienh62/aws.git

git remote set-url origin https://github.com/votre-utilisateur/votre-depot.git


