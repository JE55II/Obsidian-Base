<%*

let qcFileName = await tp.system.prompt("Note Title")

if (!qcFileName) {
    throw new Error("Note title cannot be empty")
}

let typeOptions = ["Mensuel", "Trimestriel", "Point_SOC", "lancement", "workshop", "workshop0", "workshop1", "workshop_ITP", "workshop_mobile", "workshop_mail"]
let noteType = await tp.system.suggester(typeOptions, typeOptions)

let fournisseurOptions = ["Operator1","Operator2"]
let fournisseur = await tp.system.suggester(fournisseurOptions, fournisseurOptions)

let titleName = tp.date.now("MMMM") + " - " + qcFileName
await tp.file.rename(titleName)

let baseFolder = "/🏢 Clients/📊 Points Mensuels/"
let year = tp.date.now('YYYY') 
let month = tp.date.now('MM-MMMM') 
let newFolder = `${baseFolder}${year}/${month}/` 

await tp.file.move(newFolder + titleName);

// ⚠️ DEFER MOVE (critical fix)
tp.hooks.on_all_templates_executed(async () => {
    try {
        await tp.file.move(`${fullPath}/${titleName}`)
    } catch (e) {
        console.error("Move failed:", e)
    }
})

// metadata
let currentMonth = tp.date.now("MMMM")
let creationDate = tp.date.now("YYYY-MM-DD HH:mm")

tR += `---
date: ${creationDate}
type: ${noteType}
fournisseur: ${fournisseur}

tags:
- ${currentMonth}
- ${qcFileName}

month: ${currentMonth}
client: ${qcFileName}
---

`;
-%>
<%*
let groups = {
    "Analystes": ["Name1","Name2"],
    "CSM": ["Name3","Name4"]],
    "Business": ["Name5","Name6"]
};

let selectedByGroup = {};
let output = "";

for (let [group, people] of Object.entries(groups)) {
    selectedByGroup[group] = [];
    let done = false;

    while (!done) {
        let displayList = ["[Done]"].concat(
            people.map(p => selectedByGroup[group].includes(p) ? `✅ ${p}` : p)
        );

        let choice = await tp.system.suggester(
            displayList,
            ["[Done]"].concat(people),
            false,
            `Select ${group} (multiple allowed, stop with [Done])`
        );

        if (choice === "[Done]") {
            done = true;
        } else if (!selectedByGroup[group].includes(choice)) {
            selectedByGroup[group].push(choice);
        }
    }
}

output += "## Participants\n\n";

for (let [group, chosen] of Object.entries(selectedByGroup)) {
    if (chosen.length > 0) {
        output += `**${group}**\n`;
        for (let name of chosen) {
            output += `- ${name}\n`;
        }
        output += `\n`;
    }
}

tR += output;
-%>
**Client** :
- 
  
## Agenda/Questions
- 

## To Do List

| Sujet | Décision/action | Responsable |
| ----- | --------------- | ----------- |
|       |                 |             |

## Notes
- 

## Remarques
- 
