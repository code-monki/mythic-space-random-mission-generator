# Mythic Space Random Mission Generator

## Requirements

- [ ] The application shall provide the capability to create a Mission Deck.
- [ ] The application shall provide the capability to modify the Mission Deck.
- [ ] The application shall provide the capability to create a trio of random missions based on the Mission Deck.
- [ ] The application shall provide the capability to modify missions.
- [ ] The application shall provide the capability to store the Mission Deck and the current missions in the browser's local storage.
- [ ] The application shall provide the capability to store multiple Mission Decks & missions.
- [ ] The application shall provide the capability to export the Mission Deck and it's associated missions in ASCII format.
- [ ] The application shall provide the capability to export the Mission Deck and it's associated missions in a comma-separated values (CSV) format.
- [ ] The application shall provide the capability to export the Mission Deck and its associated missions in a Javascript Object Notation (JSON) format.

## Workflow

1. Generate Mission Deck
   1. Select four Factions
   2. Assign weights to Mission Types (Combat, Diplomacy, Exploration, Infiltration) to determine number of missions per type
2. Generate Missions
   1. Generate a Mission Type
   2. Generate an associated Faction
   3. Determine Mission Difficulty
   4. Determine Mission Objectives
   5. Determine Complications
   6. Determine Mission Rewards

## Analysis

The application is in essence a single-page application (SPA) that runs in the browser. Unfortunately, browser security precludes loading the Mission Deck from the user's local file storage and the author is not inclined at this point to run a free storage service. To work around these issues, the application will store the Mission Deck in the Browser's local storage and offer an option to export a representation of the deck as a plain text file, a comma-separated values (CSV) file, and as a JSON object.

At this time, the application will not function as a progressive web application (PWA) but that may change in the future.

## Design

### User Interface - Main Page

![Main User Interface](/design/img/main-page.png)<br />
**Figure 1:** Main User Interface

The Main User interface displays the Mission Table and its associated Missions. The interface is divided into two section:

- Mission Table and associated controls
- Missions and associated controls

#### Mission Table and Associated Controls

The _Mission Table_ dropdown list above the table allows the user to select an existing Mission Table by name. Once selected, the contents of the selected Mission Table will be displayed in the Mission Table.

The _New Mission Table_ (document icon with the plus sign) allows the user to create a new Mission Table. When clicked, the Mission Table Builder dialog will be displayed and allow the user to select the mission type distribution and the factions.

The _Edit_ button (document with the pencil) allows the user to edit the currently selected Mission Table.

The _Export_ button (document icon with the right-pointing arrow) allows the user to export the currently selected Mission Table and its associated missions.

The currently selected Mission Table contents are displayed as a two-column 'table' with the first column containing the mission categories and the second containing the factions.

#### Missions and Associated Controls

The Missions are display in a collection of three summary cards.

A _Mission_ card contains a brief summary of the mission including _mission type_, _faction_, _difficulty_, _objective_, and _rewards_ as well as a link named _More..._ that appears in the lower right hand corner. The _More..._ link opens the selected mission in a Mission Edit dialog allowing the user to edit the existing mission or randomly generate a single new mission to replace the existing mission.

The button above the mission cards with the die icon allows the user to randomly generate three missions.

### User Interface - Mission Table Builder Dialog

![Mission Table Builder](/design/img/mission-table-builder.png)<br />
**Figure 2:** Mission Table Builder Dialog

The Mission Table Builder dialog has the following controls:

- Mission Table Name
- Faction dropdown lists and randomizer controls
- Number of players spincontrol
- Mission Type weightings

#### Mission Table Name

The _Mission Table Name_ edit field allows the user to assign a name to the Mission Table that will be used as a key in the browser local storage. The edit field has a control to erase its contents and another control in the shape of a trashcan that allows the user to delete the Mission Table. If this option is chosen, the form will be reset to its defaults and the Mission Table will be removed from the main user interface as well as from local storage. **_This field is required for saving changes to the mission._**

#### Faction Dropdown List and Randomizer Controls

The Factions section of the dialog allows the user to specifically set the desired factions or to have them randomly selected by clicking the button with the die icon.

#### Number of Players Spin Control

The _Number of Players_ spin control allows the user to select the number of players ranging from 1 to 12 and populates that number of input fields per mission category. The maximum number of points per player per category is 6.

Each category has an additional input field to the right that will have the average of the player weightings. The maximum total of the averages is 13 and will be enforced by the application. For example, if the Combat category averages 6 points, the Diplomacy category averages 3 points, and the Exploration category averages 3 points, then the Infiltration category will have a maximum of 1 point (6 + 3 + 3 + 1 = 13).

At the bottom of the dialog are the _Cancel_ and _Save_ buttons. The _Cancel_ button has the following functions:

- New table - The dialog will discard any changes and close without saving the table.
- Existing table - The dialog will save the changes under the name given in the _Mission Table Name_ field. The dialog will not allow a save to occur unless there is something in the _Mission Table Name_ field and will prompt the user for confirmation if the table will be overwriting an existing table.

### Mission Card

![Mission Card](/design/img/mission-card.png)<br />
**Figure 3:** Mission Card Dialog

The _Mission Card_ dialog displays all of the details of the mission. Clicking on the Edit button (document with a pencil icon), will allow the fields to be edited by the user. Likewise, clicking on the delete button (_trashcan_) will delete the mission and close the dialog.

The first field is the _mission name_. This is a text input field with a quick erase button (circle with X icon). **_This field is required for saving changes to the mission._**

The _Mission Category_ allows the user to change the category of the mission.

The _Faction_ dropdown list allows the user to change the faction associated with the mission.

The _Difficulty_ dropdown allows the user to change the difficulty of the mission.

The _Objective_ dropdown allows the user to change the objective of the mission.

The _Complications_ dropdown allows the user to change the complication associated with the mission.

The _Rewards_ dropdown allows the user to change the rewards associated with the mission.

The _Cancel_ and _Save_ button allow the user to cancel or save any changes that have occurred.

### Data Storage Format

The Mission Table and its associated missions are converted to a JSON string and stored in the browser's local storage. The JSON object format is:

```json

{
  "Name of Mission Table" : {
    "categories" [
      "category 1", "category 2", "category 3", "category 4", "category 5",
      "category 6", "category 7", "category 8", "category 9", "category 10",
      "category 11", "category 12", "category 13"
      ],
    "factions": [ "faction 1", "faction 2", "faction 3", "faction 4"],
    "num_players": 3,
    "category_scores": [
      {
        "combat": [4, 2, 3]
      },
      {
        "diplomacy": [3, 4, 1]
      },
      {
        "exploration": [2, 2, 5]
      },
      {
        "infiltration": [1, 4, 4]
      }
    ],
    "missions": [
      {
        "name": "Mission 1",
        "category": "Combat",
        "faction": "Brachinids",
        "difficulty": "Easy",
        "objective": "Group",
        "complications": "There is a secondary objective.",
        "rewards": "Credits"
      },
      {
        "name": "Mission 2",
        "category": "Infiltration",
        "faction": "Singularity",
        "difficulty": "Hard",
        "objective": "Information",
        "complications": "New Experience",
        "rewards": "Exotic Material"
      },
      {
        "name": "Mission 3",
        "category": "Exploration",
        "faction": "Empire of Man",
        "difficulty": "Hard",
        "objective": "Object",
        "complications": "Another faction wants the same objective.",
        "rewards": "Weapon Mod"
      }
    ]
    }

```

**_Note that the *num_players* item can range from 1-12 and the associated player weights will have the same number as the number of players._**

This same format will be used for the JSON export.

### Text Export Format for Mission Table and Missions

The _Text Export_ format shall be:

```
+------------------------------------+
| Name of Mission Table              |
+----+-------------+-----------------+
|  2 | category 1  | ♠️ Brachinids    |
|  3 | category 2  | ♥️ Singularity   |
|  4 | category 3  | ♦️ Empire of Man |
|  5 | category 4  | ♣️ Irumai        |
|  6 | category 5  |                 |
|  7 | category 6  |                 |
|  8 | category 7  |                 |
|  9 | category 8  |                 |
| 10 | category 9  |                 |
|  J | category 10 |                 |
|  Q | category 11 |                 |
|  K | category 12 |                 |
|  A | category 13 |                 |
+----+-------------+-----------------+
```

**NOTE: The _Text Export_ omits the number of players and the category weightings.**

### CSV Export Format for Mission Table and Missions

The _CSV Export_ Format shall use the table name as the file name and have the following body:

```
"category 1","category 2","category 3","category 4","category 5","category 6","category 7","category 8","category 9","category 10","category 11","category 12","category 13",
"♠️ Brachinids","♥️ Singularity","♦️ Empire of Man","♣️ Irumai"
"Mission 1","Combat","Brachinids","Easy","Group","There is a secondary objective.","Credits"
"Mission 2","Infiltration","Singularity","Hard","Information","New Experience","Exotic Material"
"Mission 3","Exploration","Empire of Man","Hard","Object","Another faction wants the same objective.","Weapon Mod"
```

**NOTE: The _CSV Export_ omits the number of players and the category weightings.**

### JSON Export Format for Mission Table and Missions

The JSON format is the same as that used for the browser local storage and includes all of the components of the Mission Table, Missions, and the underlying weighting data.
