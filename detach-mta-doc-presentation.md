---
type: slide
slideOptions:
  progress: true
  slideNumber: true
  showSlideNumber: 'all'
  display: block
  theme: black
  transition: 'slide'
---

<!-- ![](https://tenor.com/fr/view/orange-cat-smile-cat-smile-orenge-cat-smiling-gif-23133369.gif)-->

<!-- --- -->

**Projet Front Back - STACK**

# Don't tach my documents

- Etudiants : Yan IMENSAR, Adrien JALLAIS, Nicolas KIRCHHOFFER.
- Encadrant : Adrien LEBRE.

*29/11/2022*

Note:
Optimisation du stockage des pièces jointes associées aux emails

---

## Plan

1. Présentation du contexte
2. Enjeux DD & RS
3. Solutions techniques
4. Démonstration
5. Organisation / Prévisions

---

## Présentation du contexte

> Entre 2017 et 2021, près de 300 milliards de mails ont été envoyés dans le monde par jour.

*Statistica. 2021.*

----

### Problématique
*Comment réduire la quantité de données associée aux mails et plus exactement à ceux qui contiennent des pièces jointes ?*

Note:
- PUE (Power Usage Effectiveness) est lié à la consommation des serveurs

----

### Stockage des pièces jointes: X IMAP
![xIMAP](https://hot-objects.liiib.re/pad-faire-ecole-org/uploads/fdfe9d81-bd66-4b34-976d-a2e55c02f249.png)

----

### Stockage des pièces jointes: 1 IMAP
![oneIMAP](https://hot-objects.liiib.re/pad-faire-ecole-org/uploads/1a43c77b-d275-4c7b-a66a-ad9d4cb2048a.png)

----

### Stockage des pièces jointes: 1 CDN
![oneCDN](https://hot-objects.liiib.re/pad-faire-ecole-org/uploads/7d209ebb-eeb9-454e-95ca-b6d04aab395d.png)

Note:
Le principe c'est de mettre la pièce jointe dans un réseau de stockage (CDN), pour réduire l'utilisation de la bande passante sur la backbone lors de l'envoie de l'email (càd emprunter moins de router).

---

## 5 axes du label DD & RS
1. <span><!-- .element: class="fragment highlight-red" -->Environnemement</span>
2. <span><!-- .element: class="fragment highlight-red" -->Stratégie gouvernance</span>
4. Recherche innovation
5. Politique sociale
6. Enseignement & formation

![](https://hot-objects.liiib.re/pad-faire-ecole-org/uploads/b23d2f13-6869-48de-a010-a0b10e427515.png)

Note: 
- Axe : Stratégie gouvernance 
- Sensibiliser 
- Variable stratégique : Contribution DD&RS avec les parties prenantes

----

### Développement durable
- Réduire l'empreinte de <span><!-- .element: class="fragment highlight-red" -->stockage</span> des échanges de mails
- Réduire la <span><!-- .element: class="fragment highlight-red" -->bande passante</span> des systèmes de mails

----

### Responsabilité sociétale
- Sensibiliser l'utilisateur
  - évaluer l'impact du stockage
- Responsabiliser l'utilisateur
  - cycle de vie des pièces jointes
  - décentraliser le stockage
<!-- on ne souhaite pas aborder le point suivant car l'ipfs ne permet pas de supprimer un fichier-->
<!--    - Comment assurer la propriété des données des utilisateur·trice·s-->

Note:
- Sensibiliser l'user sur l'impact de ses fichiers
- Responsabiliser l'user sur le cycle de vie des pièces-jointes
- Décentraliser le stockage des pièces-jointes (actuellement les fichiers sont hébergés par les serveurs IMAP)

----

### Quels sont les impacts possibles ?

1. <span><!-- .element: class="fragment highlight-red" -->Serveurs de stockage mail (IMAP)</span>
2. Agents de transfert (MTA)

Note:
- IMAP : Internet Message Access Protocol
- MTA : Mail Transfert Agent

----

#### Focus sur le stockage 

![Simulation](https://hot-objects.liiib.re/pad-faire-ecole-org/uploads/e9ec3a31-d400-4a76-856b-656a2b8e9c7a.png)

Note:
- Poids d'un mail : 5Mo
- Axes : 
  - X : IMAP destinataires
  - Y : Stockage nécessaire 
- Permet de moins solliciter éviter de rajouter des serveurs de stockages

----

### Application numérique
*Sur les 300 milliards de mails quotidens, combien contiennent des pièces jointes ?*
> "Bonjour, j'ai bien tous les logs concernant les mails, mais ceux-ci ne concernent que l'enveloppe pas le contenu." 
> [Christian JANIN](https://www.imt-atlantique.fr/fr/user/288) - DSI de l'IMT

Note:
- Dans l'enveloppe, il y a la taille du mail et du contenu

---

## Solutions techniques

----
### Fonctionnement des mails

![](https://hot-objects.liiib.re/pad-faire-ecole-org/uploads/75106397-023e-45db-9179-13d399479e60.png)

Note:
- MUA : Mail User Agent
- MTA : Message Transfer Agent
- MDA : Mail Delivery Agent

----

### Choix des technologies

- Serveur SMTP : Postfix
- Stockage des pièces-jointes : Interplanetary File System (IPFS)
- Detach MTA : NodeJS

Note:
- Postfix est utilisé pour 35% des serveurs SMTP
- Facile de configuration (sur le papier)

----

### Architecture du projet

#### Solution 1 : MTA intermédiaire
![](https://hot-objects.liiib.re/pad-faire-ecole-org/uploads/d04042a8-2546-44c4-8474-7cb33c28ed55.png)

----

#### Solution 2 : Before-Queue Content Filter
![](https://hot-objects.liiib.re/pad-faire-ecole-org/uploads/d805407e-979c-43bf-86b4-169b7ac1a495.png)

----

### Interplanetary File System (IPFS)

- Stockage écentralisé
- Recherche par contenu =! par emplacement
- Contenu addressé par hash

```js
const locationBased = "https://bestsite.com/tree.structure/file"
const contentBased = "https://ipfs/afybeigdyrzt5sfhu76uh7y/file"
```

Note:
- Stockage distribué pour stocker et accéder à des fichiers, websites, apllications et données
- CID : (unique hash-based) Content Identifiers 

----

![](https://hot-objects.liiib.re/pad-faire-ecole-org/uploads/07009cbf-6507-48dc-a674-36bd1f606310.png)

----

#### Avantages d'IPFS

- Hash :
  - URL selon le contenu et non le nom du serveur
  - Pas de duplication
  - Intégrités des fichiers et immutabilité
- Stockage moins coûteux
- Haute performance, surtout en local
- Haute disponibilité

Note:
Hash : 
- recherche par contenu et pas par emplacement / nom
- Pas de duplication
- Stockage moins coûteux : On a besoin de moins de servers les utilisateurs qui voient le contenu aident à la distribution du fichier
- Haute performance, on est pas en HTTP un contenu est récupéré en 1 point, ici on peut avoir plusieurs noeuds comme sources pour un contenu
- Haute disponibilité et résiliant

----

#### Limites d'IPFS

- Protocole doit être massivement adopté
- Logiciel encore en version Alpha

![](https://hot-objects.liiib.re/pad-faire-ecole-org/uploads/7ffddd49-c795-4cf2-8b04-d7ead2bfe21a.png)

Note:


----

#### Ajout de données par IPFS
```js
const node = await IPFS.create()
const data = 'Hello IMT'

// add your data to IPFS - this can be a string, a Buffer,
// a stream of Buffers, etc
const results = node.add(data)

// we loop over the results because 'add' supports multiple 
// additions, but we only added one entry here so we only see
// one log line in the output
for await (const { cid } of results) {
  // CID (Content IDentifier) uniquely addresses the data
  // and can be used to get it again.
  console.log(cid.toString())
}
```

----

![](https://hot-objects.liiib.re/pad-faire-ecole-org/uploads/09fb9dd5-ac56-4aab-877b-23c9899a1d86.png)

Note:
- Nous avons créé un noeud IPFS sur un de nos serveurs avec l'implémentation go de IPFS
- Si on revient un peu avant sur le dessin, on a enfait créé un de ces différents noeuds

----

![](https://hot-objects.liiib.re/pad-faire-ecole-org/uploads/2238efe4-ea53-4d83-8acd-bf130170bf82.png)
Note:
- Dans notre application NodeJS on a une library IPFS HTTP qui nous permet de communiquer avec notre noeud en go
- A chaque fois un mail atteint le mta et contient une pièce jointe, on envoit tout sur le noeud IPFS
- On calcul et on récupère un Hash unique, CID (content identifier)
- On injecte les urls avec le hash dans le contenu du mail

---

## Démonstration

----

![](https://hot-objects.liiib.re/pad-faire-ecole-org/uploads/92e576e0-7866-4411-b33a-ad2b6e249970.gif)

---

## Organisation / Prévisions

----

### Répartition des tâches : Période 1

#### Mails : Nicolas (Yan et Adrien en support)
- Postfix/Dovecot/Rainloop
- Etude des choix d'architectures
- Proxy SMTP NodeJS
- Débugage Before-Queue Content Filter

Note:
Mails : 
- Configuration de Postfix/Dovecot (DKIM/DMARC/SPF) avec Rainloop pour envoyer des mails sans tomber dans les Spam (~ 3h)
- Documentation sur Postfix et réflexion sur les différentes architectures (~ 5h)
- Écriture du proxy SMTP sur NodeJS (~ 5h)
- Débugage sur le Before-Queue Content Filter : parcours de logs, recherche de documentation, etc. (~ 10h)

----

### Répartition des tâches : Période 1

#### IPFS: Yan et Adrien
- Découverte prise en main
- Étude de faisabilité
- Étude des avantages et inconvénients
- Premiers essais
- Implémentation du protocole sur l'application NodeJS

Note:
IPFS : 
- Découverte du protocole et prise en main (??)
- Premiers essais (??)
- Implémentation du protocole sur l'application NodeJS (??)

----

### Tâches restantes
- Refactoring du code
- Calcul de l'économie en bande passante/stockage
- Mise en place d'une BDD traçant les PJ envoyées (Vérif RGPD)
- Dashboard avec vue globale sur le dispositif
    - sensibilisation sur la consommation
    - système d'authentification
    - gestion du cycle de vie par l'expéditeur-trice

Note:
Reste à faire : 
- Refactoring du code pour améliorer la lisibilité (~ 1h)
- Calcul du poids économisé en bande passante/stockage avant l'envoi de mails (~ 1/2h)
- Mise en place d'une base de données recensant les PJ envoyées (vérif RGPD) (~ 1/2h)
- Réalisation d'un dashboard permettant d'avoir une vue globale sur le dispositif (aspect DDRS) (??h)
   - Sensibilisation des utilisateur·trice·s sur leur consommation de BP/stockage (aspect vitrine) (??h)
   - Système d'authentification via l'adresse mail de l'expéditeur·trice (??h)
   - Gestion du cycle de vie par l'expéditeur·trice (??h)

---

## Références
- [Before-queue Content Filter won't inject data back to Postfix](https://serverfault.com/questions/1116027/before-queue-content-filter-wont-inject-data-back-to-postfix)
- [The Postfix Homepage](https://www.postfix.org)
- [Zimbra - Collaboration Suite System Architecture](http://docs.zimbra.com/docs/os/6.0.10/administration_guide/images/2_Overview%20System%20Architecture.03.4.1.jpg)
- [RFC 5321: Simple Mail Transfer Protocol](https://www.rfc-editor.org/rfc/rfc5321.html)
- [Liu et al. 2021. - Who’s Got Your Mail?](https://dl.acm.org/doi/pdf/10.1145/3487552.3487820)
- [Trautwein et al. 2022. - Design and Evaluation of IPFS](https://dl.acm.org/doi/pdf/10.1145/3487552.3487820)
- [Statistica. 2021. - Number of sent and received e-mails per day worldwide from 2017 to 2024.](https://www.statista.com/statistics/456500/daily-number-of-e-mails-worldwide)
