# **Exercice 1 - VM Windows**

## **Partie 1 - Gestion des utilisateurs**

### **Q.1.1.1**
Pour Créer l'utilisateur Lionel Lemarchand je suis allé rechercher Kelly Rhameur :

![](media/image1.png)

Ensuite j'ai copié Kelly Rhameur pour creer Lionel Lemarchand :

![](media/image2.png)

### **Q.1.1.2**

J'ai cliqué sur Kelly puis move dans la bonne OU

![](media/image3.png)

![](media/image4.png)

### **Q.1.1.3**
Je suis allé dans l'OU direction RH puis clique droits GRPUsersDirection... -> propriété -> membre et remove Kelly

![](media/image5.png)

### **Q.1.1.4**
J'ai rename le dossier de Kelly puis j'ai créé celui de Lionel

![](media/image6.png)

## **Partie 2 - Restriction utilisateurs**

### **Q.1.2.1** 
Pour faire en sorte que l'utilisateur **Gabriel Ghul** ne puisse se connecter que du lundi au vendredi j'ai dans l'AD : Recherche de Gabriel -> propriété -> Account -> Logon Hours

![](media/image7.png)

![](media/image8.png)

### **Q.1.2.2**
Pour bloquer sa connexion il faut : allé dans Propriété -> Account -> Log On To... -> Sélectionné The following computers et rentrer "CLIENT01"

![](media/image9.png)

### **Q.1.2.3**
Pour mettre en place cette sécurité de mot de passe je suis partie sur une GPO :
pour la réaliser il faut aller dans : Group Policy management puis sélectionné LabUsers puis créer une nouvelle GPO et ensuite la modifier puis suivre : Configuration ordinateur > Polices > Paramètres Windows > Paramètres de sécurité > Stratégies de compte > Stratégie de mot de passe

J'ai ensuite modifier : 
- La longueur du MDP 
- La durée Maximal et minimal du MDP 
- La complexité 
- et l'historique des MDP pour ne pas qu'ils réutilisent les mêmes.

![](media/image10.png)

## **Partie 3 - Lecteurs réseaux**

![](media/image11.png)

### **Q.1.3.1**

Pour réaliser cette GPO j'ai Créé une GPO dans TSSR.lan

Configuration utilisateur > Préférences > Paramètres Windows > Lecteurs

J'ai mappé un nouveau drive et j'ai rentré les bonnes infos en récupérant sur les disques que j'ai partagé au Préalable.

![](media/image12.png)

# **Exercice 2 - VM Linux**

## **Partie 1 - Gestion des utilisateurs**

### **Q.2.1.1**

Pour créer le User il faut faire la commande "useradd -m -s "nomUser"
Ensuite il faut ajouter un mot de passe de votre choix complexe.

![](media/image13.png)

### **Q.2.1.2**

Je préconise d'ajouter sudo au compte.

![](media/image14.png)

## **Partie 2 - Configuration de SSH**

### **Q.2.2.1**
Pour désactivé l'acces il faut se rendre dans : /etc/ssh/sshd_config puis décommentez Permit RootLogin et ajouter NO à la fin. Ensuite il faut restart SSH.

![](media/image15.png)

### **Q.2.2.2**
Pour autoriser l'acces il faut se rendre dans : /etc/ssh/sshd_config et ajouter AllowUsers "Nom"

![](media/image16.png)

### **Q.2.2.3**
décommentassions des deux lignes en jaune et sur la ligne PasswordAuthentification mettre NO

![](media/image17.png)

## **Partie 3 - Analyse du stockage**

### **Q.2.3.1**

Pour voir les systèmes de fichiers monté j'ai fais un df -hT et un lsblk -f
Les système de fichier monter sont : tmpfs (4)(- udev -

![](media/image18.png)

### **Q.2.3.2**

![](media/image19.png)

Les systemes de stockage utilisé sont :

udev - tmpfs - /dev/mapper/cp3--vf-root - /dev/md0p1

### **Q.2.3.3**

J'ai créer un nouveau disque de 8g sur la vm puis je l'ai Partitionné et j'ai ajouter le raid

![](media/image20.png)

![](media/image21.png)

### **Q.2.3.4**
Pour ajouter le LVM et le monter automatiquement voici les commande :

```
lvcreate -L 2G -n backup cp3-vg
mkfs.ext4 /dev/cp3-vg/backup
mkdir -p /var/lib/bareos/storage
mount /dev/cp3-vg/backup /var/lib/bareos/storage
```

Ajoutez la ligne suivante dans le fichier /etc/fstab :
```
/dev/cp3-vg/backup /var/lib/bareos/storage ext4 defaults 0 2
```

### **Q.2.3.5**
Il reste exactement 1.79G de libre.
Il y a deux type de commande possible soit vgdisplay "nom du vg" soit vgs qui donne directement la taille restante

![](media/image22.png)

![](media/image23.png)

## **Partie 4 - Sauvegardes**

### **Q.2.4.1**

bareos-dir c'est le composant principal du systeme de souvegarde il permet de gèrer et planifier les sauvegardes.
bareos-sd permet d'enregistrer et conserver les fichiers sauvegardés.
bareos-fd permet d'envoyer les fichiers à sauvegarder et permet la restauration.

## **Partie 5 - Filtrage et analyse réseau**

### **Q.2.5.1**

![](media/image24.png)

Les règles actuellement appliquées sont :

- ct state established,related accept
- ct state invalid drop
- iifname "lo" accept
- tcp dport 22 accept
- ip protocol icmp accept
- ip6 nexthdr ipv6-icmp accept

### **Q.2.5.2**

Les règles pour les types de communication autorisées sont :

- ct state established,related accept
- iifname "lo" accept
- tcp dport 22 accept
- ip protocol icmp accept
- ip6 nexthdr ipv6-icmp accept

### **Q.2.5.3**

Les règles des types interdits sont :

- ct state invalid drop -- Les packets sont considérés comme invalide

### **Q.2.5.4**

Pour ajouter les règles nécessaires il faut faire ces commandes (à l'aide d'internet) :
Avant d'ajouter les règles il faut activé NFTables avec un "systemctl start nftables"

Les regles :

- nft add rule inet filter input ip saddr 192.168.1.0/24 tcp dport {9101, 9102, 9103} accept
- nft add rule inet filter output ip daddr 192.168.1.0/24 tcp sport {9101, 9102, 9103} accept

![](media/image25.png)

## **Partie 6 - Analyse de logs**

### **Q.2.6.1**

ok : /24

nr :

faux :
