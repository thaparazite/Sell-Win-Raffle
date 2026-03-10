# Sell & Win Raffle

Desktop raffle ticket management system built with JavaFX.

## Overview

Sell & Win Raffle is a local-first desktop application for managing raffle items, ticket sales, player records, and winner selection in one workflow. It is designed for operators who need a lightweight setup without a database server or web deployment.

The application stores its data on the local machine using CSV files and item-specific image folders under the user's home directory. That keeps setup simple, backups straightforward, and the operating model easy to understand.



## Key Features

- Add raffle items with title, description, ticket count, and ticket price.
- Store item images locally and browse them in a dedicated gallery view.
- Sell tickets to players and assign ticket IDs automatically.
- Reuse ticket IDs when player records are removed.
- Track remaining ticket inventory per raffle item.
- Search player status by name, phone number, or ticket ID.
- Run a live draw and display the winner or show when a selected ticket was not sold.
- Package the application as a JAR and a Windows executable.

## Tech Stack

| Area | Details |
|------|---------|
| Language | Java 21 |
| UI | JavaFX 21, FXML, CSS |
| Build | Maven |
| Testing | JUnit 5 |
| Storage | Local CSV files |
| Packaging | JavaFX Maven Plugin, Launch4j artifacts in `out/artifacts/` |

## Application Flow

### 1. Add a raffle item

Create a new item with:

- Title
- Description
- Number of tickets
- Ticket price

When an item is created, the app prepares:

- A folder for the item's images
- A ticket record CSV for that item
- An entry in the main `data.csv` catalog

### 2. Add item images

Each item gets its own image folder under the application data directory. The app lets the operator set a default image for the main dashboard and browse all images for the item in the item viewer.

### 3. Sell tickets

For a selected item, the operator can add players by entering:

- Player name
- Phone number
- Number of tickets to purchase

The app randomly assigns available ticket IDs and writes the updated player state back to the item's record file.

### 4. Check player status

The player status screen supports searching by:

- Ticket ID
- Phone number
- Player name

### 5. Run the draw

The draw screen animates ticket number generation and then stops on the selected ticket. If the ticket belongs to a player, the winner is displayed. If the ticket has not been sold, the app shows that clearly.

## Data Storage

At runtime, the application writes data to the user's home directory:

```text
~/Sell & Win Raffle/
  data/
    data.csv
  records/
    <item-title>.csv
  <item-title>/
    image files...
```

### `data/data.csv`

Stores the master item catalog with:

- Image path
- Title
- Description
- Available tickets
- Ticket price

### `records/<item-title>.csv`

Stores the ticket ledger for a single raffle item with:

- Ticket ID
- Player name
- Phone number
- Number of tickets associated with that buyer

## Project Structure

```text
src/
  main/
    java/
      raffle/
        controllers/
        main/
        models/
        utils/
    resources/
      fxml_files/
      icons/
      stylesheets/
  test/
    raffle/
      controllers/
      main/
      models/
      utils/
```

## Architecture

### Main application layer

- `raffle.main.App`
  Loads the JavaFX application, shows the loading screen, and navigates between views.
- `raffle.main.EntryPoint`
  Thin entry point class used to launch the app.

### Controllers

The UI is split into focused JavaFX controllers:

- `MainViewController`
- `AddItemController`
- `AddPlayerController`
- `DrawController`
- `ViewItemController`
- `PlayerStatusController`
- `LoadingController`

### Domain models

- `Item`
- `Player`

### Persistence utilities

- `ItemDataReaderAndWriter`
- `PlayerDataReaderAndWriter`

These utility classes handle CSV parsing and serialization.

## Validation and Safety

The application already includes several operator-facing safeguards:

- Empty-field validation on item and player forms
- Positive-number validation for ticket counts and ticket price
- Basic phone number validation
- File accessibility checks before writing CSV files
- Confirmation dialogs before deleting items or player records

## Requirements

- Java 21
- Maven 3.9 or later
- Windows is the primary packaged target for the included `.exe` artifact

## Running the Application

### Start in development mode

```bash
mvn clean javafx:run
```

### Run tests

```bash
mvn test
```

### Build the project

```bash
mvn clean package
```

## Testing

The repository contains automated tests for:

- Controllers
- Models
- CSV reader/writer utilities
- Main application entry points

Current repository test coverage signals include:

- 12 test classes
- 47 JUnit tests

## Included Artifacts

The repository currently includes generated application artifacts in:

```text
out/artifacts/Sell_and_Win_Raffle_jar/
```

That folder contains:

- `Sell_and_Win_Raffle.jar`
- `Sell & Win Raffle.exe`

## Screens in the Application

The JavaFX UI currently includes these views:

- Loading view
- Main view
- Add item view
- Add player view
- Draw view
- View item view
- Player status view

## Current Limitations

- Persistence is CSV-based rather than database-backed.
- The app is designed for local use, not multi-user access.
- Images are managed through the local file system.
- The draw can stop on a ticket that has not been sold, and the UI reports that outcome explicitly.

## Future Improvements

- Replace CSV storage with a relational database for stronger concurrency support.
- Add sales reports and export features.
- Add installer-based distribution for non-technical operators.
- Track draw history and operational audit logs.
- Improve image management and default image workflows.

