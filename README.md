During a chat in the ADSB Exchange (ADSBx) [Discord](https://discord.com/invite/ad8SSMpWvH), the topic of being able to filter and save lists of certain aircraft came up. Additionally, a separate topic about how to use the filtering options to see exactly what you want. It spurred the idea of creating a Github to reuse some of those links, and a space to store them all as the list grows!

If you want to add your own links, click this link to get to our ["How To" section](#how-to-add-links)

# Adding Links
Easy! You can add them yourself, or, join the Discord and someone will *probably* make it for you if you ask nicely. Maybe.  

# How To Add Links
This will require some Excel knowledge, some may call it witchcraft. I will include some links about the formulas needed below. If you don't have Excel, or aren't using an OS that supports Excel, you'll need some spreadsheet software that supports some version of `TEXTJOIN`or  `CONCATENATE`/`CONCAT`.

 - More about  `TEXTJOIN` [here](https://support.microsoft.com/en-us/office/textjoin-function-357b449a-ec91-49d0-80c3-0e8fc845691c)
 - More about `CONCATENATE`/`CONCAT` [here](https://www.w3schools.com/excel/excel_concat.php)  

If you're using a newer version of Excel, you may have access to `CONCAT`, which you should use if you don't care about backwards compatibility.  You can also use `TEXTJOIN` because that will allow you to add the delimiters as needed. I'm using `TEXTJOIN` for this example. 

## Getting the Data
**Step 1:** Download the FAA's Aircraft Registration information from their [website](https://www.faa.gov/licenses_certificates/aircraft_certification/aircraft_registry/releasable_aircraft_download). This information is updated nightly so it may be helpful to always download a fresh copy. 

| SCREENSHOT HERE | 

**Step 2:** Extract all of the files, and then edit the `MASTER.txt` file extension to `MASTER.csv`. Here's a link on how to do that for [Windows 10/11 and macOS](https://www.wikihow.com/Change-a-File-Extension). You may not need to rename it, you may be able to open it directly in Excel. Mine was being a little wonky, so I put both in here. 

## Manipulating the Data
**Step 3:** Fire up Excel, and [filter/sort](https://support.microsoft.com/en-us/office/filter-data-in-a-range-or-table-7fbe34f4-8382-431d-942e-41e9a88f6a96) how you want the information. For this example, let's say we want to see all of the aircraft owned by the [Georgia Department of Public Safety (GDPS)](https://en.wikipedia.org/wiki/Georgia_Department_of_Public_Safety). 
***Note:** You may get a popup where Excel asks you about whether or not you want to perform the typical stuff Excel does when you open a .CSV (remove trailing zeros, scientific notation, convert date strings to dates, etc.) Don't do this. Leave the file as is.*

**Step 4:** This part is where you get to have a little "fun", if there's something wrong with you (like me). *You* determine how to filter the data. For our current example in the previous step, I'm going to: 

 - Filter by state `Column K` aka `STATE`
 - Filter by the text string "Public Safety" in `Column G` aka `NAME`

|SCREENSHOT HERE|

**Step 5:** The data you're looking for is in `Column AH` or the column title `MODE S CODE HEX`

**Step 6:** 
For All Formulas:
Use `TRIM` to clear out any leading/trailing spaces. More about `TRIM` [here](https://www.w3schools.com/excel/excel_trim.php).

Using `TEXTJOIN` (easiest):
 1. Create a new tab (you don't have to do this, but it's my personal preference). 
 2.  In `A1` on the new tab, enter `=TEXTJOIN(",",CELL1,CELL2,CELL3,CELL4)`.
**Note:** `CELL1,CELL2,etc..`    are going to be the individual cells of the `TRIM` function column on the    `Master` tab. You can select them by clicking and holding  `CTRL` on your keyboard. If you use the `SHIFT` key the formula won't work.

Formula Example - you'll see I ran the `TRIM` function in a new column, `AI`

`=TEXTJOIN(",",TRUE,MASTER!AI51669,MASTER!AI113794,MASTER!AI131955,MASTER!AI266486,MASTER!AI266751,MASTER!AI267358,MASTER!AI273801,MASTER!AI273151,MASTER!AI274129,MASTER!AI274802,MASTER!AI276002,MASTER!AI293305)`

Using `CONCATENATE`/ `CONCAT` (more time-intensive):
 1. Create a new tab (you don't have to do this, but it's my personal preference). 
 2. In `A1` on the new tab, enter a single comma `,`
 3. In `B2`, enter `=CONCAT(A1,CELL1,A1,CELL2,A1,CELL3)`
 **Note:** `CELL1,CELL2,etc..`    are going to be the individual cells of the `TRIM` function column on the    `Master` tab. You can select them by clicking and holding  `CTRL` on your keyboard. If you use the `SHIFT` key the formula won't work.

Formula Example - note the need to add `A1` after every variable. 
KARV
`=CONCAT(MASTER!AI$51669,A4,MASTER!AI$113794,A4,MASTER!AI$131955,A4,MASTER!AI$266486,A4,MASTER!AI$266751,A4,MASTER!AI$267358,A4,MASTER!AI$273801,A4,MASTER!AI$273151,A4,MASTER!AI$274129,A4,MASTER!AI$274802,A4,MASTER!AI$276002,A4,MASTER!AI$293305)`

## Creating the URL string
***Note:** You can use this data for *any* site that supports this structure, but since I hang out with ADSBx geeks, I'm using ADSBx links. All you'd need to change is your base URL.* 

Using the output of this formulas, simply append them to the ADSBx base URL: `https://globe.adsbexchange.com/?icao=`

Example: 
`https://globe.adsbexchange.com/?icao=A208D5,A4DB92,A5B045,AC835D,AC8714,AC8E82,ACCEA4,ACC736,ACD25B,ACD9C9,ACE747,ADD775`
