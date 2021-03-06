# r3e-adaptive-ai-primer

Allows tweaking of the adaptive AI file for [R3E](http://game.raceroom.com/), based on a database that provides the track times the AI does at different levels.

The file can be found in 
```My Documents\My Games\SimBin\RaceRoom Racing Experience\UserData\Player1\aiadaptation.xml```

How the system works was reverse engineered [here](https://forum.sector3studios.com/index.php?threads/the-new-adaptive-ai.5013/page-4#post-70837) by the user "Cheerfullyinsane" on the sector3studios forums. This work is based upong the findings in that thread.

USE AT YOUR OWN RISK

![ui](https://raw.githubusercontent.com/pixeljetstream/r3e-adaptive-ai-primer/master/docs/ui.png)

## Work in progress

## How to use

### Resetting the AI times 

You can set the existing AI times to zero influence. This is particularly useful whenever the game's AI algorithms have changed a lot.
The benefit is that you will not loose your own existing laptimes, and the AI will use the old AI levels to start its adaptation search.

* Run the tool (it will print some command-line stuff)
* Press the "Reset all AI times" button

### Database External Seeds

The database will alwasy be built from your own aiadaptataion values. So the track & car combinations you are driving as well as how the AI rated the levels based on your driving will always be shown.

To enhance the database with results from other players, dump as many "aiadaptation.xml" files as you can find in the "/seeds" directory (the tool will look for all *.xml files). The input files should have content that fits the same version of R3E's AI (when the AI or physics changes a lot with an update, old results may be too slow or fast). Run the tool it will update the database and generate a "results/database.html" file similar to [this](http://htmlpreview.github.io/?https://github.com/pixeljetstream/r3e-adaptive-ai-primer/blob/master/docs/database.html).

The table gives you AI laptimes for different levels for each track/car combo found in the database. The lower number is the "variance" in seconds, when multiple times for the same AI level were found. The lower the variance the more reliable the number (unless zero then it means we only have one entry).

### Database AI Level Processing

When the tool is run, it also generates a "results/processed.html" file ([example](http://htmlpreview.github.io/?https://github.com/pixeljetstream/r3e-adaptive-ai-primer/blob/master/docs/processed.html)), which looks is similar to the database file, however this time all AI levels have timing values. These are generated by linear curve fitting to the original database. The fitting may not always work, especially as the database may have noisy data from different users, it can happen that low AI is faster than higher AI, the tool will therefore reject curves that were not plausible.

Check the "config.lua" file for some of the influence on the fitting/plausibility checks.

### Modifying your AI file via UI

* Run the tool (it will print some command-line stuff)
* Select the type of car and class you want to modify as well as the target ai level (not all are available, it depends on the database and how it was processed)
* **Warning:** The existing ai levels for the track/class you are editing will be overwritten in your aiadaptation.xml file, make backups! Double check on classid and trackid!
* The modification is mentioned in the label above the button. The actual number of ai levels and their "spacing" (say 92,94,96) are set via the config file at the bottom.
* By pressing the "Apply" button your settings for that track/class will be overwritten

### Modifying an AI file via command-line

* Run the tool (it will print some command-line stuff)
* Look at the "results/processed.html", find the AI levels you want to race against.
* Modify the "edits.lua" file to your liking. It does contain comments what to do. You will need the classid in "()" behind car class, and the trackid numbers and provide them as "strings" to the functions.
* **Warning:** The existing ai levels for the track/class you are editing will be overwritten in your aiadaptation.xml file, make backups! Double check on classid and trackid!
* pass the "edits.lua" file to the exe (command-line or drag and drop), it will then perform all edits you defined within it.
