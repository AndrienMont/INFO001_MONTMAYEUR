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

&nbsp;

##### Question 10 : Rechercher tous les paramètres de la commande openssl pkeyutl -encrypt … pour chiffrer un message. Le message en clair sera dans un fichier nommé clair.txt. Ce fichier contiendra « Bonjour de votre_nom ». Le message chiffré devra s'appeler cipher.bin
Les paramètres de la commande openssl pkeyutl -encrypt sont les suivants :
- -in : le fichier contenant le message en clair.
- -out : le fichier contenant le message chiffré.
- -inkey : le fichier contenant la clé publique.
- -pubin : indique que la clé est une clé publique.
- -pkeyopt : les options de la clé publique.
Ce qui donne la commande :
```bash
openssl pkeyutl -encrypt -in clair.txt -out cipher.bin -inkey pub.montmayeur.pem -pubin -pkeyopt rsa_padding_mode:oaep
```

&nbsp;

##### Question 11 : Chiffrer plusieurs fois le même fichier clair.txt, et enregistrer le résultat dans les fichiers cipher.bin.1, cipher.bin.2. Comparer les contenus des différents messages chiffrés. Sont-ils identiques ? Est-ce normal ? Justifier.
Les contenus des différents messages chiffrés ne sont pas identiques. Ce n'est pas normal car le chiffrement RSA est déterministe, c'est-à-dire que le chiffrement d'un même message avec la même clé publique donne toujours le même message chiffré. Cela est dû à l'utilisation du padding OAEP qui ajoute un sel aléatoire à chaque chiffrement.

&nbsp;

##### Question :  Une fois que vous avez récupéré le fichier cipher.bin qui a été chiffré par votre voisin avec votre clé publique, déchiffrer le en complétant la commande suivante. 
```bash
openssl pkeyutl -decrypt -in cipher.bin -out clair2.txt -inkey rsa_keys.pem -pkeyopt rsa_padding_mode:oaep
```

&nbsp;

### 5. Analyse du contenu d'un certificat

#### 5.1 En ligne de commandes openssl s_client

##### Question 12 : Lire dans la page de manuel de s_client le rôle de l'option -showcerts. Expliquer le rôle de cette option. Après avoir parcouru le résultat de la commande s_client, indiquer le nombre de certificats qui ont été envoyés par le serveur. 
L'option -showcerts permet d'afficher les certificats du serveur. Après avoir parcouru le résultat de la commande s_client, on constate que le serveur a envoyé 3 certificats.

&nbsp;

##### Question 13 : Que signifie le x509 dans la commande saisie ? Quel est le sujet du certificat ? Expliquer complètement la notation utilisée dans le sujet (C=FR, ST=Auvergne…). Que signifie CN ? Quel est l'organisme qui a délivré le certificat ? 
Le x509 dans la commande saisie signifie que l'on souhaite afficher les informations du certificat. Le sujet du certificat est le suivant : 
```bash
subject= /
    C=FR
    ST=Auvergne-Rhone-Alpes
    O=Universite Grenoble Alpes
    CN=*.univ-grenoble-alpes.fr
```
La notation utilisée dans le sujet signifie que le pays est la France (FR), la région est Auvergne-Rhone-Alpes (Auvergne-Rhone-Alpes), l'organisme est l'Université Grenoble Alpes (Universite Grenoble Alpes), le nom commun est *.univ-grenoble-alpes.fr. CN signifie Common Name. L'organisme qui a délivré le certificat est  Sectigo RSA Organization Validation Secure Server CA.

&nbsp;

##### Question 14 : Que représente le « s », le « i » ? 
Le « s » représente le sujet du certificat et le « i » représente l'émetteur du certificat.

&nbsp;

##### Question 15 : On analyse le contenu du certificat de www.univ-grenoble-alpes.fr. Quelle partie d'une clé RSA, le certificat contient-il ? Quels algorithmes ont été utilisés pour signer le certificat ? Que contient l'attribut CN présent dans le « sujet » ? Quel attribut contient les autres noms de machine pour lequel le certificat peut être utilisé ? Quelle est la durée de validité du certificat ? Quel est l'utilité du lien pointant vers un fichier .crl ? 
Le certificat contient la clé publique RSA. Les algorithmes SHA256 et RSA ont été utilisés pour signer le certificat. L'attribut CN présent dans le « sujet » contient le nom commun du site. L'attribut Subject Alternative Name contient les autres noms de machine pour lequel le certificat peut être utilisé. La durée de validité du certificat est du 2024-04-08 au 2025-04-09. Le lien pointant vers un fichier .crl permet de révoquer le certificat.

&nbsp;

##### Question 16 : Par qui le certificat de www.univ-grenoble-alpes.fr a-t-il été signé ? Soit E l'algorithme de chiffrement RSA, donner la formule utilisée pour calculer la signature présente dans le certificat de www.univ-grenoble-alpes.fr .
Le certificat de www.univ-grenoble-alpes.fr a été signé par  Sectigo RSA Organization Validation Secure Server CA. La formule utilisée pour calculer la signature présente dans le certificat de www.univ-grenoble-alpes.fr est la suivante :
```bash
Signature = E(Hash(Certificat), Clé privée)
```

&nbsp;

##### Question 17 : Afficher (ou télécharger) le certificat de la CA qui a délivré le certificat de de www.univgrenoble-alpes.fr. Relever le sujet/objet de ce certificat. Quelle est la taille de la clé publique présente dans ce certificat ? Par quelle CA ce certificat a-t-il été signé ? 
Le sujet/objet du certificat de la CA qui a délivré le certificat de www.univgrenoble-alpes.fr est le suivant :
```bash
subject= /
    C=US
    ST=New Jersey
    L=Jersey City
    O=Sectigo Limited
    CN=Sectigo RSA Organization Validation Secure Server CA
```
La taille de la clé publique présente dans ce certificat est de 2048 bits. Ce certificat a été signé par USERTrust RSA Certification Authority.

&nbsp;

##### Question 18 : Pour chaque certificat de niveau n, vérifier rapidement qu'il contient les informations nécessaires pour valider le certificat de niveau n-1. On s'intéresse au certificat de dernier niveau (lorsque j'ai effectué la manipulation, il s'agissait du niveau n2). Quel certificat permet de valider ce certificat ? Où se trouve-t-il ?
Le certificat de dernier niveau (niveau n2) a été signé par USERTrust RSA Certification Authority. Ce certificat se trouve dans le certificat de niveau n1. 

&nbsp;

##### Rechercher, dans le fichier indiqué par la documentation ci-dessus, le certificat de l'autorité de certification racine. Puis l'extraire dans un fichier nommé root-ca1.pem.
La commande suivante permet d'extraire le certificat de l'autorité de certification racine dans un fichier nommé root-ca1.pem :
```bash
openssl s_client -connect www.univ-grenoble-alpes.fr:443 -showcerts < /dev/null | sed -n '/-----BEGIN CERTIFICATE-----/,/-----END CERTIFICATE-----/p' > root-ca1.pem
```

&nbsp;

##### Question 19 : Comparer les champs subject et issuer. Qu'en déduisez-vous ? Donnez la formule qui a permis de générer la signature de ce certificat ? Comment s'appelle ce type de certificat ?
Les champs subject et issuer sont identiques. Cela signifie que le certificat est auto-signé. La formule qui a permis de générer la signature de ce certificat est la suivante :
```bash
Signature = E(Hash(Certificat), Clé privée)
```

#### 5.2 Dans un navigateur

##### Question : Vérifier qu'on obtient les mêmes informations que précédemment. 
Les informations obtenues dans le navigateur sont les mêmes que celles obtenues précédemment.

&nbsp;

### 6. Mise en place d'une PKI (Public Key Infrastructure)

#### 6.2 Etude de la CA Racine (CN=Root Lorne)

&nbsp;

##### Question : Afficher le contenu de ce fichier. La ca par défaut est la ca nommée CA_Default. La variable dir contient le répertoire racine de cette CA.
```bash
# OpenSSL root CA configuration file.

[ ca ]
# `man ca`
default_ca = CA_default

[ CA_default ]
# Directory and file locations.
dir = /home/camanager/ca
certs = $dir/certs
crl_dir = $dir/crl
new_certs_dir = $dir/newcerts
database = $dir/index.txt
serial = $dir/serial
RANDFILE = $dir/private/.rand
# The root key and root certificate.
private_key = $dir/private/ca.key.pem
certificate = $dir/certs/ca.cert.pem
# For certificate revocation lists.
crlnumber = $dir/crlnumber
crl = $dir/crl/ca.crl.pem
crl_extensions = crl_ext
default_crl_days = 30
# SHA-1 is deprecated, so use SHA-2 instead.
default_md = sha256
name_opt = ca_default
cert_opt = ca_default
default_days = 375
preserve = no
policy = policy_strict

[ policy_strict ]
# The root CA should only sign intermediate certificates that match.
# See the POLICY FORMAT section of `man ca`.
countryName = match
stateOrProvinceName = match
organizationName = match
organizationalUnitName = optional
commonName = supplied
emailAddress = optional


[ policy_loose ]
# Allow the intermediate CA to sign a more diverse range of certificates.
# See the POLICY FORMAT section of the `ca` man page.
countryName = optional
stateOrProvinceName = optional
localityName = optional
organizationName = optional
organizationalUnitName = optional
commonName = supplied
emailAddress = optional


[ req ]
# Options for the `req` tool (`man req`).
default_bits = 2048
distinguished_name = req_distinguished_name
string_mask = utf8only
# SHA-1 is deprecated, so use SHA-2 instead.
default_md = sha256
# Extension to add when the -x509 option is used.
x509_extensions = v3_ca


[ req_distinguished_name ]
# See <https://en.wikipedia.org/wiki/Certificate_signing_request>.
commonName = Common Name
countryName = Country Name (2 letter code)
stateOrProvinceName = State or Province Name
localityName = Locality Name
0.organizationName = Organization Name
organizationalUnitName = Organizational Unit Name
emailAddress = Email Address
# Optionally, specify some defaults.
countryName_default = XX
stateOrProvinceName_default = MyState
localityName_default =
0.organizationName_default = MyOrg
organizationalUnitName_default =
emailAddress_default =


[ v3_ca ]
# Extensions for a typical CA (`man x509v3_config`).
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical, CA:true
keyUsage = critical, digitalSignature, cRLSign, keyCertSign

[ v3_intermediate_ca ]
# Extensions for a typical intermediate CA (`man x509v3_config`).
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical, CA:true, pathlen:0
keyUsage = critical, digitalSignature, cRLSign, keyCertSign
[ usr_cert ]
# Extensions for client certificates (`man x509v3_config`).
basicConstraints = CA:FALSE
nsCertType = client, email
nsComment = "OpenSSL Generated Client Certificate"
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer
keyUsage = critical, nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage = clientAuth, emailProtection
[ server_cert ]
# Extensions for server certificates (`man x509v3_config`).
basicConstraints = CA:FALSE
nsCertType = server
nsComment = "OpenSSL Generated Server Certificate"
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer:always
keyUsage = critical, nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth
[ crl_ext ]
# Extension for CRLs (`man x509v3_config`).
authorityKeyIdentifier=keyid:always
[ ocsp ]
# Extension for OCSP signing certificates (`man ocsp`).
basicConstraints = CA:FALSE
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer
keyUsage = critical, digitalSignature
extendedKeyUsage = critical, OCSPSigning
```

&nbsp;

##### Question : Une ligne du fichier openssl.cnf permet de localiser le certificat de la CA. En utilisant la commande openssl x509 associée à divers paramètres, afficher le certificat de la CA.
La commande suivante permet d'afficher le certificat de la CA :
```bash
openssl x509 -in /home/camanager/ca/certs/ca.cert.pem -noout -text
```

&nbsp;

##### Question 20 : Quelle est la taille de la clé ? Quelle est la durée de validité ? Qu'est-ce qui vous permet de dire qu'il s'agit d'un certificat « racine » ou auto-signé ? Pour quels « X509v3 Key Usage » cette autorité de certification peut-elle être utilisée ? 
La taille de la clé est de 2048 bits. La durée de validité est de 375 jours. Il s'agit d'un certificat « racine » ou auto-signé car le sujet et l'émetteur sont identiques. Cette autorité de certification peut être utilisée pour les X509v3 Key Usage suivants : digitalSignature, cRLSign, keyCertSign.

&nbsp;

##### Question : Une ligne du fichier openssl.cnf permet de localiser la clé privée de la CA. En utilisant la commande openssl rsa associée à divers paramètres, afficher la clé privée de la CA (clé de chiffrement florent).
La commande suivante permet d'afficher la clé privée de la CA :
```bash
openssl rsa -in /home/camanager/ca/private/ca.key.pem -noout -text
```

&nbsp;

#### 6.3 Création d'une autorité de certification intermédiaire

&nbsp;

##### Question 21 : Quelle valeur avez-vous mise dans le paramètre dir de la section CA_default ? Dans quel dossier et sous quel nom la clé privée de la CA devra-t-elle être stockée ? Dans quel dossier et sous quel nom le certificat de la CA devra-t-il être stocké ?
La valeur mise dans le paramètre dir de la section CA_default est /home/etudiant/ca . La clé privée de la CA devra être stockée dans le dossier /home/etudiant/ca/private sous le nom interm.key.pem. Le certificat de la CA devra être stocké dans le dossier /home/etudiant/ca/certs sous le nom interm.cert.pem.

&nbsp;

##### Question 22 : Relever la commande saisie pour créer la clé. 
La commande suivante permet de créer la clé :
```bash
cd private
openssl genrsa -aes128 -out interm.key.pem 3072
```

&nbsp;

##### Question 23 : On constate que la demande de certificat contient déjà une signature. Pourquoi est-ce que la présence de cette signature peut être qualifiée de bizarre ? Rechercher sur Internet en 2-3 minutes quel est l'intérêt de cette signature. 
La présence de cette signature peut être qualifiée de bizarre car la demande de certificat n'a pas encore été signée par l'autorité de certification. L'intérêt de cette signature est de permettre à l'autorité de certification de vérifier l'identité du demandeur.

&nbsp;

#### 6.4 Création du certificat du serveur

Fait

&nbsp;

### 7. Mise en place d'un reverse proxy

#### 7.1 Installation de docker et de git

Fait

&nbsp;

#### 7.2 Mise en place des conteneurs pour l'Infrastructure









