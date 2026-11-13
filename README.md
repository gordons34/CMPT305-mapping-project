# City of Edmonton Property Assessments

A JavaFX desktop app for exploring the City of Edmonton's property assessment data on an interactive map.

**Authors:** Charuni Liyanage, Olasubomi Badiru, Simon Gordon
**Course:** CMPT 305, taught by Dr. Indratmo Indratmo

---

## Overview

The City of Edmonton publishes an assessed value for every property in the city. That's over 420,000 records, and in raw form they're hard to make sense of. This app loads the full dataset and lets you look up a single address, a whole neighbourhood or any area on the map. It shows the results on an ArcGIS map, along with summary statistics.

**Built with:** Java 21, JavaFX (FXML + CSS), the ArcGIS Maps SDK for Java, ControlsFX and Maven.

---

## What it does

When the app starts, it reads the City's open-data CSV (`Property_Assessment_Data__Current_Calendar_Year__20240111.csv`) and turns each row into a `PropertyAssessment` object. Each object holds the property's address, neighbourhood, ward, assessed value, garage flag, assessment classes and location. You can then explore the data in three tabs:

### Find by Address
- An address box with autocomplete covering every property in the dataset.
- A table showing the matching property's address, assessed value, garage and assessment class.
- The property is marked on the map.

### Find by Neighbourhood
- A neighbourhood box with autocomplete.
- Statistics for the neighbourhood: property count, minimum, maximum, average and range of assessed values.
- The neighbourhood's outline is drawn on the map. The app works it out by computing a convex hull of all property locations in that neighbourhood.

### Find on Map
- Click anywhere on the map and use the slider to pick a radius in km.
- The app draws the radius and finds every property inside it, using the Haversine distance formula.
- Statistics for the selected area: count, min, max, range, mean and median.

### Project structure

- **App entry point (`Main.java`):** launches the JavaFX window and loads the UI layout from `main.fxml`.
- **UI controller (`Controllers/`):** handles everything the user interacts with. It sets up the ArcGIS map, runs the three search tabs and calculates the statistics shown on screen.
- **Data layer (`PropertyAssessmentHandler/`):** reads the CSV through a data access object (DAO) and turns each row into model classes: a property assessment and its address, location, neighbourhood and assessment class. It also has the filtering and statistics methods (min, max, mean, median) that the UI uses.
- **Helpers (`HelperClasses/`):** utility code, including the convex hull algorithm that draws neighbourhood boundaries and the autocomplete for the search boxes.
- **Resources (`src/main/resources/`):** the FXML layout, CSS stylesheets and image assets.


---

## Setup

### Prerequisites
- **JDK 21** or newer
- An **ArcGIS Developer API key**. You can get one for free at [developers.arcgis.com](https://developers.arcgis.com/).
- Optional: IntelliJ IDEA. The project was developed in it.

### 1. Clone the repository
git clone https://github.com/<your-username>/<repo-name>.git
cd <repo-name>

### 2. Add the dataset
The app looks for `Property_Assessment_Data__Current_Calendar_Year__20240111.csv` in the **project root**. If it isn't in the repo, download the "Property Assessment Data (Current Calendar Year)" dataset from the [City of Edmonton Open Data Portal](https://data.edmonton.ca/), rename it to match the filename above, and put it in the project root.

### 3. Set your ArcGIS API key
Open `src/main/java/com/example/cmpt305project/Controllers/MainController.java` and replace the value of `yourApiKey` in `initialize()` with your own key.

### 4. Run the app
Using the included Maven wrapper:

# macOS / Linux
./mvnw clean javafx:run

# Windows
mvnw.cmd clean javafx:run

**In IntelliJ:** open the project, let Maven import the dependencies, then run `com.example.cmpt305project.Main`.

> The first build can take a few minutes because the ArcGIS SDK and its native libraries are large. The app may also take a moment to start while it loads the full dataset.
