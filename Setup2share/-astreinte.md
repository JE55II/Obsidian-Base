<%*
/* ---------- INPUTS ---------- */
let titleName = await tp.system.prompt("Hor ticket")
if (!titleName) {
    new Notice("Title required")
    return
}
let alertName = await tp.system.prompt("Alert name") || ""
let customerName = await tp.system.prompt("Customer name") || "unknown"
/* ---------- CLEAN TITLE ---------- */
titleName = titleName.replace(/[\\/:*?"<>|]/g, "")
/* ---------- DATE / PATH ---------- */
let baseFolder = "🔎⏰Astreintes"
let year = tp.date.now("YYYY")
let month = tp.date.now("MM-MMMM")
let newFolder = `${baseFolder}/${year}/${month}`
/* ---------- ENSURE FOLDER ---------- */
async function ensureFolder(path) {
    const parts = path.split("/")
    let current = ""

    for (let part of parts) {
        if (!part) continue
        current += (current ? "/" : "") + part

        try {
            if (!app.vault.getAbstractFileByPath(current)) {
                await app.vault.createFolder(current)
            }
        } catch (e) {
            if (!e.message.includes("Folder already exists")) {
                throw e
            }
        }
    }
}
await ensureFolder(newFolder)
/* ---------- METADATA (WRITE FIRST) ---------- */
let creationDate = tp.date.now("YYYY-MM-DD HH:mm")
tR += `---
Date: ${creationDate}
Client: ${customerName}
Alert name: ${alertName}
tags:
  - ${titleName.replace(/\s+/g, "_").toLowerCase()}
  - soc_operation
  - astreinte
---
`
/* ---------- MOVE + RENAME IN ONE STEP ---------- */
await tp.file.move(`${newFolder}/${titleName}`)
-%>
### Heure de fin : [HH:mm]

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
