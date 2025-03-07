# **Exercice 2 - VM Linux**

## **Partie 1 - Gestion des utilisateurs**

### **Q.2.1.1**

Pour créer le User il faut faire la commande "useradd -m -s "nomUser"
Ensuite il faut ajouter un mot de passe de votre choix complexe.

![image](https://github.com/user-attachments/assets/09da7029-9f81-4b03-b2b2-134e70bff80f)


### **Q.2.1.2**

Je préconise d'ajouter sudo au compte.

![image](https://github.com/user-attachments/assets/b5714933-6f97-4ee5-9c39-3eaab54ed747)


## **Partie 2 - Configuration de SSH**

### **Q.2.2.1**
Pour désactivé l'acces il faut se rendre dans : /etc/ssh/sshd_config puis décommentez Permit RootLogin et ajouter NO à la fin. Ensuite il faut restart SSH.

![image](https://github.com/user-attachments/assets/00e3ed0c-3c56-4f8d-8869-9373722730e4)


### **Q.2.2.2**
Pour autoriser l'acces il faut se rendre dans : /etc/ssh/sshd_config et ajouter AllowUsers "Nom"

![image](https://github.com/user-attachments/assets/20bd2d8d-2b66-4752-8629-f2a2d4e1f999)


### **Q.2.2.3**
décommentassions des deux lignes en jaune et sur la ligne PasswordAuthentification mettre NO

![image](https://github.com/user-attachments/assets/6602cd96-3fde-47b4-b719-a3c902d39405)


## **Partie 3 - Analyse du stockage**

### **Q.2.3.1**

Pour voir les systèmes de fichiers monté j'ai fais un df -hT et un lsblk -f
Les système de fichier monter sont : 
⦁ udev ⦁ tmpfs(4) ⦁ /dev/mapper/cp3–vf-root ⦁ /dev/md0p1

![image](https://github.com/user-attachments/assets/2e7abf57-fc56-4ae3-b2c7-47c6d66f7ccf)

### **Q.2.3.2**

![image](https://github.com/user-attachments/assets/6629e79e-84ef-451d-8643-2a7c143a4f35)

Les systemes de stockage utilisé sont :

udev - tmpfs - /dev/mapper/cp3--vf-root - /dev/md0p1

### **Q.2.3.3**

J'ai créer un nouveau disque de 8g sur la vm puis je l'ai Partitionné et j'ai ajouter le raid

![image](https://github.com/user-attachments/assets/daa6213a-75ee-4968-8f08-4139c6e6bc52)


![image](https://github.com/user-attachments/assets/53ca2a30-2c8b-4ef5-ac89-0bebe1acff86)


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

![image](https://github.com/user-attachments/assets/1b86e069-7735-45f6-ac2d-6b0d8c93fdac)


![image](https://github.com/user-attachments/assets/674fbbbe-0b2b-4d6b-9ea9-db5aef3a3bc0)


## **Partie 4 - Sauvegardes**

### **Q.2.4.1**

bareos-dir c'est le composant principal du systeme de souvegarde il permet de gèrer et planifier les sauvegardes.
bareos-sd permet d'enregistrer et conserver les fichiers sauvegardés.
bareos-fd permet d'envoyer les fichiers à sauvegarder et permet la restauration.

## **Partie 5 - Filtrage et analyse réseau**

### **Q.2.5.1**

![image](https://github.com/user-attachments/assets/c761ead9-2373-470d-bbf9-63ae70bd8c81)


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

![image](https://github.com/user-attachments/assets/04e94043-b523-4195-9358-4bc173e7e50e)


## **Partie 6 - Analyse de logs**

### **Q.2.6.1**

Malheureusement après plusieurs tentative de plusieur commande différente aucune log de connexion n'apparait.
