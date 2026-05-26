<%*
//Title
let titleName = await tp.system.prompt("Hor ticket")
let alertName = await tp.system.prompt("Alert name | [] break so remove")
let customerName = await tp.system.prompt("Customer name")
await tp.file.rename(titleName)
//location + Date
let baseFolder = "/♻️ DailyNotes/Soc Operation/SocTickets/"
let year = tp.date.now('YYYY') 
let month = tp.date.now('MM-MMMM')
let day = tp.date.now('DD')
let newFolder = `${baseFolder}${year}/${month}/${day}/` 
await tp.file.move(newFolder + titleName);
// metadata
let creationDate = tp.date.now("YYYY-MM-DD HH:mm")
tR += `---
Date: ${creationDate}
Client: ${customerName}
Alert name: ${alertName}
tags:
- ${titleName}
- soc_operation 
---
`;
-%>

---
Duplicated/Link of 

Date et heure : 
Utilisateur : 
Endpoint : 
OS Version : 
Local Admin : 
Source Endpoint : 
Destination Endpoint : 
Ticket : 

Target Username : 
Groupe/Domaine : 
Nom de fichier : 
APK identifier : 
Adresse IP Source : 
Adresse IP Poste : 
Hote distant : 
Adresse IP Destination : 
Adresse MAC Poste : 
###### Chemin d'accès : 
```

```
###### Commande : 
```

```
###### Script : 
```

```
###### Hash (SHA256) :
```

```
###### Reg Key : 
```

```
###### Lien (désactivé par sécurité) :
```

```
###### paramètres de lien :
```

```
---
### Invest

Bonjour,
Nous avons reçus une alerte suite à 









Nous fermons l'alerte en Vrai positif Légitime.



Nous fermons en Faux positif.

Cette activité est-elle légitime ?
Devons placer une exception sur cet utilisateur ?

#justify
`JUSITIFY: SLA restarted — at unrelated question from intermediary`

---
#### more

Nom de compte : 
Local Admin : 
Adresse IP locale :  
Host address : 
Compte de Domaine : 
Processus : 
Sujet de mail : 
Expéditeur : 
Destinataires : 

Adresses IP Source : 
Titre de Mail : 