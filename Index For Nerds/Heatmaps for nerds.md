### Activity Tracker (heatmaps for nerds)

```dataviewjs
dv.span("## SOC Operations Heatmap")

const TAG = "soc_operation"
const YEAR = moment().year()

const hue1 = 13
const hue2 = 132

// ------------------------------------
// HEATMAP CONFIG
// ------------------------------------

const currentDate = moment()  

const calendarData = {

    // IMPORTANT
    // DO NOT use "year"

    entries: [],

    showCurrentDayBorder: true,

    showMonthLabels: true,
    showWeekdayLabels: false,

    // Hide internal text rendering
    showText: false,

    cellSize: 5,
    cellGap: 1,

    intensityScaleStart: 1,
    intensityScaleEnd: 10,

    // HARD LIMIT RANGE
    startDate: `${YEAR}-01-01`,
    endDate: currentDate
        .endOf("month")
        .format("YYYY-MM-DD"),

    colors: {
        red2green: [
            `hsl(${hue1},100%,37%)`,
            `hsl(${hue1},100%,50%)`,
            `hsl(${hue1},100%,60%)`,
            `hsl(${hue1},100%,70%)`,
            `hsl(${hue1},100%,80%)`,
            `hsl(0,0%,85%)`,
            `hsl(${hue2*0.6},60%,75%)`,
            `hsl(${hue2*0.75},50%,60%)`,
            `hsl(${hue2*0.9},45%,45%)`,
            `hsl(${hue2},60%,25%)`,
        ]
    }
}

// ------------------------------------
// DAILY COUNTS
// ------------------------------------

const dailyCounts = {}

const pages = dv.pages()

for (const page of pages) {

    let hasSocTag = false

    // --------------------------------
    // CHECK FRONTMATTER TAGS
    // --------------------------------

    const pageTags = page.file.tags || []

    if (
        pageTags.some(
            t => t.replace("#", "") === TAG
        )
    ) {
        hasSocTag = true
    }

    // --------------------------------
    // CHECK RAW FILE CONTENT
    // --------------------------------

    if (!hasSocTag) {

        const content =
            await dv.io.load(page.file.path)

        if (
            content &&
            content.match(
                /#soc_operation\b/
            )
        ) {
            hasSocTag = true
        }
    }

    if (!hasSocTag)
        continue

    // --------------------------------
    // EXTRACT DAY FOLDER
    // --------------------------------

    // Example:
    // 2026/05-May/06/file.md

	const pathParts =  
	page.file.folder.split("/")  
	  
	// Need at least:  
	// Year/Month/Day  
	  
	if (pathParts.length < 3)  
	continue  
	  
	const dayFolder =  
	pathParts[pathParts.length - 1]  
	  
	const monthFolder =  
	pathParts[pathParts.length - 2]  
	  
	const yearFolder =  
	pathParts[pathParts.length - 3]  
	  
	// Validate values exist  
	  
	if (  
	!dayFolder ||  
	!monthFolder ||  
	!yearFolder  
	) continue

    // --------------------------------
    // BUILD DATE
    // --------------------------------

	const monthMatch =  
		String(monthFolder)  
			.match(/^(\d{2})/)

    if (!monthMatch)
        continue

    const month =
        monthMatch[1]

	const day =  
		String(dayFolder)  
			.replace(/\D/g, "")  
			.padStart(2, "0")

    const date =
        `${yearFolder}-${month}-${day}`

    if (!date.startsWith(`${YEAR}`))
        continue

    // --------------------------------
    // COUNT FILE
    // --------------------------------

    if (!dailyCounts[date]) {
        dailyCounts[date] = {
            count: 0,
            files: []
        }
    }

    dailyCounts[date].count++

    dailyCounts[date].files.push(
        page.file.link
    )
}

// ------------------------------------
// BUILD ENTRIES
// ------------------------------------

calendarData.entries =
    Object.entries(dailyCounts)

        .sort((a, b) =>
            a[0].localeCompare(b[0])
        )

        .map(([date, data]) => {

            return {

                date,

                intensity: Math.min(
                    data.count,
                    10
                ),

                content:""
            }
        })

// ------------------------------------
// RENDER
// ------------------------------------

renderHeatmapCalendar(
    this.container,
    calendarData
)

// ------------------------------------
// MONTHLY SUMMARY
// ------------------------------------

dv.header(2, "Monthly SOC Operations")

const monthly = {}

for (const [date, data]
    of Object.entries(dailyCounts)) {

    const month =
        moment(date).format("YYYY-MM")

    if (!monthly[month]) {
        monthly[month] = 0
    }

    monthly[month] += data.count
}

dv.table(
    ["Month", "SOC Operations"],

    Object.entries(monthly)
        .sort()
        .map(([m, c]) => [m, c])
)
```

```dataviewjs
dv.span("## Astreinte Heatmap")

const TAG = "astreinte"
const YEAR = moment().year()

const hue1 = 220
const hue2 = 280

const currentDate = moment()

// ------------------------------------
// HEATMAP CONFIG
// ------------------------------------

const calendarData = {

    entries: [],

    showCurrentDayBorder: true,

    showMonthLabels: true,
    showWeekdayLabels: false,

    showText: false,

    cellSize: 5,
    cellGap: 1,

    intensityScaleStart: 1,
    intensityScaleEnd: 10,

    startDate: `${YEAR}-01-01`,

    endDate: currentDate
        .endOf("month")
        .format("YYYY-MM-DD"),

    colors: {
        blue2purple: [
            `hsl(${hue1},100%,37%)`,
            `hsl(${hue1},100%,50%)`,
            `hsl(${hue1},100%,60%)`,
            `hsl(${hue1},100%,70%)`,
            `hsl(${hue1},100%,80%)`,
            `hsl(0,0%,85%)`,
            `hsl(${hue2*0.6},60%,75%)`,
            `hsl(${hue2*0.75},50%,60%)`,
            `hsl(${hue2*0.9},45%,45%)`,
            `hsl(${hue2},60%,25%)`,
        ]
    }
}

// ------------------------------------
// DAILY COUNTS
// ------------------------------------

const dailyCounts = {}

const pages = dv.pages()

for (const page of pages) {

    let hasTag = false

    // --------------------------------
    // FRONTMATTER TAGS
    // --------------------------------

    const pageTags = page.file.tags || []

    if (
        pageTags.some(
            t => t.replace("#", "") === TAG
        )
    ) {
        hasTag = true
    }

    // --------------------------------
    // INLINE TAGS
    // --------------------------------

    if (!hasTag) {

        const content =
            await dv.io.load(page.file.path)

        if (
            content &&
            content.match(/#astreinte\b/)
        ) {
            hasTag = true
        }
    }

    if (!hasTag)
        continue

    // --------------------------------
    // USE FILE CREATION DATE
    // --------------------------------

    const date =
        page.file.cday
            .toFormat("yyyy-MM-dd")

    if (!date.startsWith(`${YEAR}`))
        continue

    // --------------------------------
    // COUNT FILE
    // --------------------------------

    if (!dailyCounts[date]) {

        dailyCounts[date] = {
            count: 0
        }
    }

    dailyCounts[date].count++
}

// ------------------------------------
// BUILD ENTRIES
// ------------------------------------

calendarData.entries =
    Object.entries(dailyCounts)

        .sort((a, b) =>
            a[0].localeCompare(b[0])
        )

        .map(([date, data]) => {

            return {

                date,

                intensity: Math.min(
                    data.count,
                    10
                ),

                content: ""
            }
        })

// ------------------------------------
// RENDER
// ------------------------------------

renderHeatmapCalendar(
    this.container,
    calendarData
)

// ------------------------------------
// MONTHLY SUMMARY
// ------------------------------------

dv.header(2, "Monthly Astreinte")

const monthly = {}

for (const [date, data]
    of Object.entries(dailyCounts)) {

    const month =
        moment(date).format("YYYY-MM")

    if (!monthly[month]) {
        monthly[month] = 0
    }

    monthly[month] += data.count
}

dv.table(
    ["Month", "Astreinte"],

    Object.entries(monthly)
        .sort()
        .map(([m, c]) => [m, c])
)
```
