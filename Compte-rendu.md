## INFO001 - Sécurité des applications
## Compte-rendu TP 1 - Cryptographie appliquée : TLS/SSL PKI

### MONTMAYEUR Andrien M2 INFO

&nbsp;

#### 2. Préparation

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

#### 4. Etude du chiffrement RSA