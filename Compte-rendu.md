## INFO001 - Sécurité des applications
## Compte-rendu TP 1 - Cryptographie appliquée : TLS/SSL PKI

### MONTMAYEUR Andrien M2 INFO

&nbsp;

### 2. Préparation

##### Question 1 : Rappeler les calculs réalisés pour générer un couple de clés publique/privée RSA.
Pour générer un couple de clés publique/privée RSA, il faut suivre les étapes suivantes :
1. Choisir deux nombres premiers p et q.
2. Calculer n = p * q.
3. Calculer φ(n) = (p-1) * (q-1).
4. Choisir un entier e tel que 1 < e < φ(n) et que e soit premier avec φ(n).
5. Calculer d tel que d * e ≡ 1 (mod φ(n)).
6. La clé publique est (n, e) et la clé privée est (n, d).

&nbsp;

##### Question 2 : Soit un message M dont on souhaite assurer la confidentialité, rappeler le calcul réalisé pour chiffrer ce message en utilisant RSA. On notera C le message chiffré. Votre calcul doit faire intervenir les paramètres trouvés à la question 1. Rappeler comment l'opération inverse (déchiffrement) est réalisée.
Pour chiffrer un message M en utilisant RSA, il faut suivre les étapes suivantes :
1. Calculer C = M^e mod n.
Pour déchiffrer le message chiffré C, il faut suivre les étapes suivantes :
1. Calculer M = C^d mod n.

&nbsp;

##### Question 3 : Donner 4 informations importantes contenues dans un certificat.
Un certificat contient les informations suivantes :
1. Le nom du propriétaire du certificat.
2. La clé publique du propriétaire du certificat.
3. La signature du certificat.
4. La date de début de validité du certificat.

&nbsp;

##### Question 4 : Lorsque Bob se connecte en https au site www.alice.com, indiquer, en les détaillant un minimum, toutes les étapes pour l'authentification du site de Alice. On considère que le certificat d'Alice a été délivré par ca1 et que le certificat de ca1 a été délivré par root-ca.
Lorsque Bob se connecte en https au site www.alice.com,
1. Alice envoie son certificat à Bob.
2. Bob vérifie la signature du certificat d'Alice en utilisant la clé publique de ca1.
3. Bob vérifie la signature du certificat de ca1 en utilisant la clé publique de root-ca.
4. Bob vérifie que le certificat d'Alice est valide en vérifiant la date de début de validité et la date de fin de validité.
5. Bob vérifie que le nom du site www.alice.com est bien présent dans le certificat d'Alice.
6. Bob vérifie que le nom du site www.alice.com est bien présent dans le certificat de ca1.
7. Bob vérifie que le nom du site www.alice.com est bien présent dans le certificat de root-ca.
8. Bob vérifie que le certificat de root-ca est un certificat de confiance.


&nbsp;
&nbsp;

### 4. Etude du chiffrement RSA

#### 4.1 Génération de clés RSA 

##### Question : En utilisant la page de manuel de la commande openssl, trouver une commande qui permet de générer une « bi-clé » RSA ? Comment indique-t-on la taille de la clé que l'on souhaite créer ?
La commande suivante permet de générer une « bi-clé » RSA :
```bash
openssl genrsa -out key.pem 2048
```
La taille de la clé que l'on souhaite créer est indiquée par le paramètre 2048.

&nbsp;

##### Question : Créer une biclé RSA de 512 bits, et la stocker dans un fichier nommé rsa_keys.pem. 
La commande suivante permet de créer une biclé RSA de 512 bits et de la stocker dans un fichier nommé rsa_keys.pem :
```bash
openssl genrsa -out rsa_keys.pem 512
```

##### Question 5 : Quelle est la longueur des 2 nombres premiers choisis ? Quelle est la longueur du « n » généré ? Dans quel(s) calcul(s) le « publicExponant » et le « privateExponant » apparaissent-ils ? Le « publicExponant » est-il difficile deviner pour un pirate ? 
La longueur des 2 nombres premiers choisis est de 256 bits. La longueur du « n » généré est de 512 bits. Le « publicExponant » et le « privateExponant » apparaissent dans le calcul de la clé publique et de la clé privée. Le « publicExponant » n'est pas difficile à deviner pour un pirate car il est souvent égal à 65537.
```bash
Private-Key: (512 bit, 2 primes)
modulus:
    00:d2:5e:7e:f9:b7:46:90:e0:61:43:45:cb:c3:54:
    f2:57:76:0f:f6:e4:6a:4c:03:83:9f:f7:2f:dc:34:
    38:0c:6f:43:10:fc:5a:27:48:15:a9:9c:9f:82:be:
    95:32:0c:e7:f6:13:e6:f2:81:f5:ac:2a:48:25:ec:
    c0:58:d4:27:71
publicExponent: 65537 (0x10001)
privateExponent:
    05:0e:3e:50:f4:05:9f:1f:b5:56:af:8b:b9:13:06:
    8b:f1:8b:6a:ac:8c:9d:6d:0c:31:c0:f8:06:7b:be:
    c9:15:ac:41:6c:a3:48:00:26:75:84:9c:de:fd:e4:
    9f:c0:e0:45:a3:2c:b6:5c:14:50:a5:88:f8:22:b8:
    43:7e:65:91
prime1:
    00:f8:eb:cb:51:a2:75:c6:a1:af:1a:a3:66:4a:eb:
    44:a7:a1:9c:20:da:d5:64:e0:3d:18:d7:95:2e:c3:
    8c:14:bd
prime2:
    00:d8:5a:08:c5:72:df:3c:b7:c8:b8:c9:70:bb:c9:
    62:02:7b:3e:03:ee:e9:15:4f:9b:a6:fd:1f:6d:76:
    f1:1a:c5
exponent1:
    6d:b1:f3:c2:c0:f6:68:17:7e:84:1d:b8:09:92:0e:
    8a:55:04:e9:d1:a6:32:b3:43:19:7c:7f:c8:c0:f2:
    2d:7d
exponent2:
    6d:38:7e:a7:17:47:c5:82:4e:6d:a3:1c:2b:61:0d:
    fe:8c:b3:11:0f:42:52:04:df:62:5c:f4:c9:b1:3b:
    24:b5
coefficient:
    00:ae:cf:0c:f9:f5:18:77:61:d1:9e:7d:60:79:80:
    0c:9b:cd:51:ef:fe:89:a4:fe:e6:f6:2f:1f:56:21:
    dc:61:b9
```
&nbsp;

##### Question 6 : Y-a-t-il un intérêt à chiffrer une clé publique ? À chiffrer une clé privée ? 
Une clé publique est par définition accessible par tout le monde, il n'y a donc pas d'intérêt à la chiffrer. En revanche, une clé privée doit rester secrète, il est donc intéressant de la chiffrer.

&nbsp;

##### Question 7 :  Rechercher sur Internet quel est l'encodage utilisé pour la clé ? Quel est l'avantage de cet encodage ? 
L'encodage utilisé pour la clé est l'encodage PEM (Privacy Enhanced Mail). L'avantage de cet encodage est qu'il permet de stocker des données binaires dans un format texte.

&nbsp;

##### Question 8 : Retrouve-t-on bien les éléments attendus ? Pourquoi est-il intéressant de disposer d'un fichier à part contenant uniquement la clé publique ? 
On retrouve bien les éléments attendus, le module et l'exposant. Il est intéressant de disposer d'un fichier à part contenant uniquement la clé publique car cela permet de la partager avec d'autres personnes sans risquer de divulguer la clé privée.
```bash
Public-Key: (512 bit)
Modulus:
    00:d2:5e:7e:f9:b7:46:90:e0:61:43:45:cb:c3:54:
    f2:57:76:0f:f6:e4:6a:4c:03:83:9f:f7:2f:dc:34:
    38:0c:6f:43:10:fc:5a:27:48:15:a9:9c:9f:82:be:
    95:32:0c:e7:f6:13:e6:f2:81:f5:ac:2a:48:25:ec:
    c0:58:d4:27:71
Exponent: 65537 (0x10001)
```

&nbsp;

##### Question : Renommer le fichier pub.pem en pub.votre_login.pem.
La commande suivante permet de renommer le fichier pub.pem en pub.votre_login.pem :
```bash
mv pub.pem pub.montmayeur.pem
```

&nbsp;
&nbsp;

#### 4.2 Chiffrement asymétrique

&nbsp;

##### Question 9 : Quelle clé doit-on utiliser pour chiffrer un message RSA lorsqu'on veut l'envoyer de manière confidentielle ? 
Pour chiffrer un message RSA lorsqu'on veut l'envoyer de manière confidentielle, il faut utiliser la clé publique du destinataire.

##### Question : Expliquer les commandes suivantes.
La commande suivante permet d'installer le serveur web Apache :
```bash
sudo dnf install httpd
```
La commande suivante permet de démarrer le serveur web Apache :
```bash
sudo systemctl start httpd
```
La commande suivante permet d'ouvrir le port 80 en TCP :
```bash
sudo firewall-cmd --add-port 80/tcp --permanent
```
La commande suivante permet de recharger la configuration du pare-feu :
```bash
sudo firewall-cmd --reload
```
La commande suivante permet de copier le fichier pub.votre_login.pem dans le répertoire /var/www/html :
```bash
sudo cp pub.votre_login.pem /var/www/html/pub.votre_login.pem
```

&nbsp;

##### Question : Quelle commande curl ou wget utilisez-vous pour récupérer le fichier pub.votre_login.pem de votre voisin?
La commande curl suivante permet de récupérer le fichier pub.votre_login.pem de votre voisin :
```bash
curl http://IP_VOISIN/pub.votre_login.pem
```
