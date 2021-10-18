## dependencies:
nodejs
npm
yarn (optional but recommended)
react-native
expo-cli

And the npm dependencies will show in `packages.json`!


## Overview
The app stores **notes** in **notebooks**.
A user can create multiple notes, as well as notebooks.
Each note will be a document storing either tabular data (similar to excel) or markdown documents.

## Stack description
We will be using **react native** (frontend) to develop a cross-platform app supported by a **flask** (backend) with **postgres** (database) on heroku.

## Considerations
**How should we deal with tabular data? (especially for for exercise tracking)**
- Are there specific features that we want to input in each cell?
  - if so, should we make those features generic (user settable) to allow for other uses 

**What should we use to render the markdown for notes?**
- for users not familar with markdown, should we also offer gui formatting menu for lists, checkboxes etc? (md can be hard to write on phone)
- the libarry we use for markdown has to be fast since we should refresh at every word. 
  - can we find patterns on where to refresh? 
  - is there a way to refresh the markdown partially/ avoid the user screen flashing when we refresh the document?

**Should we create extra functions dedicated to exercise mode?** 
- calculations, statistics etc
- Or we could also allow users to create 'templates' of calculations, (like excel formula) and provide the users with some pre-set configurations? (exercise pattern, eating habits, sleeping habits etc)

**How should we sync the data of the app with apps on other devices?**
- could just use the database with keys
- we can also consider syncing through dropbox/google cloud, but that might require 'authentication' from user every time.

**THE APP SHOULD WORK OFFLINE**
- we need to download the entire data on startup and keep it in our cache. 
- For now, since we are developing the frontend only we can store the note as a json file or something instead of making requests

## Design
- The application should be a single page app with a side menu.
- The main screen will always be showing a 'note' (unless in setting or user page). If there is none, a new note screen will show
- The user can use the side menu to choose a note to render. 
- The side menu will show the notes in a tree view, as well as user account icon and general settings button
- When showing the note, we will show different menu for markdown type or tabular type.


**Quick draft of the component/pages**

```yml
App:
    type: our entry point!
    input: nothing
    does: grabs the entire user note. Starts MainView with Sidebar on side.

Mainview:
    type: page
    input: notes (entire collection of notes), cur_note_id(what to display), settings
    does: searches up the note by the cur_note_id, determines the type of the note and calls Note or Table.

Note:
    type: component
    input: raw text of the note, settings
    does: renders the given note in markdown format following user set settings

Table:
    type: component
    input: raw text of the note (format tbd, we can either use json or csv), settings
    does: renders the given text in tabular format following user settings

SideBar:
    type: component
    input: notes (entire collection of notes), settings # for now, we might need user id or somethign later
    does: can be expanded to show the NoteView (notes in tree view) and settings and user button, which lead to Setting and UserPage

NoteView:
    type: component
    input: notes (entire collection of notes), settings
    does: displays the structure of the notes in tree view and the note/notebook titles as nodes

Setting:
    type: page
    input: settings
    does: takes in the given settings and allows user to make changes! the setings should be updated real time

UserPage:
    type: page
    input: settings
    does: some user related stuff... log in/log out, sync settings, tbd.
```

i'll try to make the rendering later

## Good reads:
[setting up react native with yarn](https://reactnative.dev/docs/environment-setup)
[what is markdown](https://www.markdownguide.org/getting-started/)
[what is react](https://www.w3schools.com/whatis/whatis_react.asp) &larr; still relevant because react native is a child of react!