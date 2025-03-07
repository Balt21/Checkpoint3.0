# **Exercice 1 - VM Windows**

## **Partie 1 - Gestion des utilisateurs**

### **Q.1.1.1**
Pour Créer l'utilisateur Lionel Lemarchand je suis allé rechercher Kelly Rhameur :

![image](https://github.com/user-attachments/assets/7ffbf07d-e4ab-4932-acac-65caff255e5c)

Ensuite j'ai copié Kelly Rhameur pour creer Lionel Lemarchand :

![image](https://github.com/user-attachments/assets/7e666e71-bde8-4f1a-90fb-839f35b776a9)


### **Q.1.1.2**

J'ai cliqué sur Kelly puis move dans la bonne OU

![image](https://github.com/user-attachments/assets/896ec0e4-8a34-4f3b-b758-da8bbbc53799)


![image](https://github.com/user-attachments/assets/1e1c490e-d53b-45bb-8a5d-187000a0e00a)

### **Q.1.1.3**
Je suis allé dans l'OU direction RH puis clique droits GRPUsersDirection... -> propriété -> membre et remove Kelly

![image](https://github.com/user-attachments/assets/94ee4174-f346-41f0-a36d-8a6602bc4588)

### **Q.1.1.4**
J'ai rename le dossier de Kelly puis j'ai créé celui de Lionel

![image](https://github.com/user-attachments/assets/d93386c0-7068-49c2-9c29-5d01e2cfea84)


## **Partie 2 - Restriction utilisateurs**

### **Q.1.2.1** 
Pour faire en sorte que l'utilisateur **Gabriel Ghul** ne puisse se connecter que du lundi au vendredi j'ai dans l'AD : Recherche de Gabriel -> propriété -> Account -> Logon Hours

![image](https://github.com/user-attachments/assets/916d125a-e2ba-45d7-a363-9f4c497cdd2b)


![image](https://github.com/user-attachments/assets/af55566e-5ed3-4ef5-94b0-481a596a3d27)


### **Q.1.2.2**
Pour bloquer sa connexion il faut : allé dans Propriété -> Account -> Log On To... -> Sélectionné The following computers et rentrer "CLIENT01"

![image](https://github.com/user-attachments/assets/9050888d-407b-4281-b11a-b3a7caa2a142)


### **Q.1.2.3**
Pour mettre en place cette sécurité de mot de passe je suis partie sur une GPO :
pour la réaliser il faut aller dans : Group Policy management puis sélectionné LabUsers puis créer une nouvelle GPO et ensuite la modifier puis suivre : Configuration ordinateur > Polices > Paramètres Windows > Paramètres de sécurité > Stratégies de compte > Stratégie de mot de passe

J'ai ensuite modifier : 
- La longueur du MDP 
- La durée Maximal et minimal du MDP 
- La complexité 
- et l'historique des MDP pour ne pas qu'ils réutilisent les mêmes.

![image](https://github.com/user-attachments/assets/815b2772-a405-4e3c-823d-6913d9b4d3c5)


## **Partie 3 - Lecteurs réseaux**

### **Q.1.3.1**

Pour réaliser cette GPO j'ai Créé une GPO dans TSSR.lan

Configuration utilisateur > Préférences > Paramètres Windows > Lecteurs

J'ai mappé un nouveau drive et j'ai rentré les bonnes infos en récupérant sur les disques que j'ai partagé au Préalable.
![image](https://github.com/user-attachments/assets/23b91c4a-ba4f-44f8-835d-22701a584fe0)

![image](https://github.com/user-attachments/assets/ad1bdf0e-736d-440d-9b08-f8e0f0fea805)
