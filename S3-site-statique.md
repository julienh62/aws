# Configuration du site web statique avec S3, CloudFront et HTTPS

Ce document décrit les étapes de configuration pour héberger un site web statique sur **Amazon S3** avec un certificat SSL (HTTPS) et **Amazon CloudFront** pour la distribution de contenu. Le domaine utilisé pour ce site est **jhennebo.fr**.

## 1. Création et configuration du Bucket S3  (region europe)

1. **Créer un bucket S3** nommé `jhennebo.fr`.
2. Activer l'hébergement statique sur le bucket :
   - **Properties > Static Website Hosting > Enable**.
   - **Index document** : `index.html`.
   - **Error document** (facultatif) : `error.html`.
3. **Télécharger les fichiers du site** dans le bucket.




2. Création d'une Hosted Zone dans Route 53

    Créer une Hosted Zone pour votre domaine dans Route 53 :
        Allez dans Route 53 > Hosted Zones > Create Hosted Zone.
        Entrez votre nom de domaine jhennebo.fr.
        Laissez le type de zone comme Public Hosted Zone.

    Une fois la zone créée, copiez les serveurs de noms (NS) générés par AWS :
        Exemple : ns-1061.awsdns-04.org, ns-959.awsdns-55.net, etc.

3. Mettre à jour les serveurs de noms chez IONOS

    Connectez-vous à IONOS, votre fournisseur de domaine.
    Allez dans les paramètres DNS de votre domaine jhennebo.fr.
    Remplacez les serveurs de noms existants par les serveurs de noms (NS) fournis par AWS dans la Hosted Zone Route 53.
    Sauvegardez les modifications et attendez que la propagation DNS prenne effet (cela peut prendre jusqu'à 48 heures).

4. Création d'un certificat SSL dans ACM  region us-east-1
   #  creer certicat dans certificate manager choisir region us-east-1
    Aller sur AWS Certificate Manager (ACM) et demander un certificat public pour les domaines :
        jhennebo.fr et www.jhennebo.fr (si nécessaire).
    Valider le certificat en ajoutant un enregistrement CNAME dans la zone DNS (Route 53).

5. Création de la distribution CloudFront

    Aller sur CloudFront > Create Distribution.
    Sélectionner Web et configurer les paramètres suivants :
        Origin Domain Name : Choisir le bucket S3 jhennebo.fr.s3-website.eu-west-3.amazonaws.com.
        Viewer Protocol Policy : Choisir Redirect HTTP to HTTPS.
        SSL Certificate : Sélectionner le certificat SSL créé dans ACM.
        Alternate Domain Names (CNAMEs) : Ajouter jhennebo.fr et www.jhennebo.fr comme CNAMEs pour la distribution.
        Default Root Object", entrez index.html
        Cache Behavior : Laisser par défaut ou personnaliser selon vos besoins.

    toujours dans cloudfront dans  security cliquer sur origin access et  créer OAC


    Créer la distribution et attendre qu'elle soit déployée (cela peut prendre 15-20 minutes).

## 6. Sécurisation de l'accès au Bucket S3

1. Dans **Permissions > Block Public Access** :
   - Décochez la case **Block Public Access (bucket settings)** pour permettre un accès public aux fichiers via CloudFront (mais pas directement).
   
2. Ajouter une **Bucket Policy** pour restreindre l'accès au seul CloudFront (via OAC) :

   Exemple de politique :
   ```json
   {
       "Version": "2012-10-17",
       "Statement": [
           {
               "Sid": "AllowCloudFrontAccess",
               "Effect": "Allow",
               "Principal": {
                   "Service": "cloudfront.amazonaws.com"
               },
               "Action": "s3:GetObject",
               "Resource": "arn:aws:s3:::jhennebo.fr/*",
               "Condition": {
                   "StringEquals": {
                       "AWS:SourceArn": "arn:aws:cloudfront::<CloudFront-Distribution-ID>"
                   }
               }
           }
       ]
   }



7. Configuration DNS dans Route 53 (ou IONOS)

    Aller dans Route 53 (ou IONOS) et mettre à jour les enregistrements DNS pour pointer vers CloudFront :
        Créer un enregistrement A (Alias) pour jhennebo.fr et www.jhennebo.fr, et sélectionnez votre distribution CloudFront comme Alias Target.
    Exemple de configuration dans Route 53 :
        Name : jhennebo.fr.
        Type : A - IPv4 address (Alias).
        Alias Target : Sélectionnez votre distribution CloudFront.

8. Test et vérification
   curl https://d3bp3m54wut6tl.cloudfront.net/index.html
    Une fois la distribution CloudFront déployée, vérifiez que le site est accessible via HTTPS :
        Testez l'URL https://jhennebo.fr dans votre navigateur.
    Assurez-vous que :
        Les requêtes HTTP sont redirigées vers HTTPS.
        Le certificat SSL est correctement appliqué.

9. Sécurisation de l'accès avec CloudFront

    Origin Access Control (OAC) est utilisé pour restreindre l'accès direct au bucket S3, afin que les fichiers ne soient accessibles qu'à travers CloudFront.
    Configurer OAC (Origin Access Control) lors de la création de la distribution CloudFront pour une sécurité accrue.
    Pour renforcer la sécurité, configurez une Bucket Policy afin que le bucket S3 autorise uniquement les requêtes venant de CloudFront, via OAC.

10. Notes supplémentaires

    N'oubliez pas d'ajouter le nom de domaine jhennebo.fr sous Alternate Domain Name (CNAME) dans la configuration de CloudFront. Cela garantit que les fichiers servis par votre distribution CloudFront utilisent votre domaine personnalisé.
    Si vous utilisez des fichiers dans un autre bucket ou avez des besoins spécifiques, ajustez la configuration de CloudFront et S3 en conséquence.


    pour vider le cache du CloudFront ;  Sélectionne ta distribution CloudFront :

    Dans le tableau de bord de CloudFront, tu verras une liste de distributions. Sélectionne la distribution associée à ton site web.
    Clique sur le ID de la distribution pour ouvrir les détails.

Invalider le cache :

    Une fois dans les détails de la distribution, va dans l'onglet Invalidations (c'est dans le menu en haut).
    Clique sur Create Invalidation (Créer une invalidation).

Choisis les fichiers à invalider :

    Dans le champ "Object Paths", tu peux spécifier les fichiers ou les répertoires que tu veux invalider.
        Si tu veux invalider tout le cache, tu peux entrer /* (cela invalidera tous les fichiers en cache de la distribution).
        Si tu veux invalider des fichiers spécifiques, entre les chemins d'accès relatifs de ces fichiers, par exemple : /images/*.jpg ou /css/styles.css.
