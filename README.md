# Sell & Win Raffle 🎟️

[![Java](https://img.shields.io/badge/Java-21-ED8B00?logo=openjdk&logoColor=white)](https://openjdk.org/)
[![JavaFX](https://img.shields.io/badge/JavaFX-21-2C2255?logo=java&logoColor=white)](https://openjfx.io/)
[![Build](https://img.shields.io/badge/Build-Maven-C71A36?logo=apachemaven&logoColor=white)](https://maven.apache.org/)
[![Tests](https://img.shields.io/badge/Tests-JUnit%205-25A162?logo=junit5&logoColor=white)](https://junit.org/junit5/)

Desktop raffle ticket management system built with **JavaFX**. Designed for local raffle campaigns with an operator-friendly workflow for managing items, selling tickets, tracking players, and running a live draw.

---

## 🧾 About

**Sell & Win Raffle** is a local-first desktop application for organizing and running raffle campaigns end-to-end:

- Manage raffle items (prizes)
- Sell tickets and assign ticket IDs automatically
- Track players and ticket inventory
- Run a live draw and announce winners

Data is stored locally using **CSV files** and item-specific image folders under the user’s home directory, keeping setup simple and backups straightforward.

---

## ✨ Key Features

- 🏷️ Create raffle items with title, description, ticket count, and ticket price
- 🖼️ Store and browse item images (with a configurable default image)
- 👤 Sell tickets to players and auto-assign ticket IDs
- ♻️ Reuse ticket IDs when player records are removed
- 📦 Track remaining ticket inventory per item
- 🔎 Search player status by name, phone number, or ticket ID
- 🎲 Run a live draw and show the winner (or indicate when a selected ticket was not sold)
- 🧰 Package as a **JAR** and a Windows **.exe** (Launch4j)

---

## 🧱 Tech Stack

| Area | Details |
|------|---------|
| 💻 Language | Java 21 |
| 🧩 UI | JavaFX 21, FXML, CSS |
| 🏗️ Build | Maven |
| ✅ Testing | JUnit 5 |
| 💾 Storage | Local CSV files |
| 📦 Packaging | JavaFX Maven Plugin, Launch4j artifacts in `out/artifacts/` |

---

## 🔄 Application Flow

### 1) Add a raffle item

Create a new item with:

- Title
- Description
- Number of tickets
- Ticket price

When an item is created, the app prepares:

- A folder for the item’s images
- A ticket record CSV for that item
- An entry in the main `data.csv` catalog

### 2) Add item images

Each item gets its own image folder under the application data directory. Operators can set a default image for the main dashboard and browse all images for an item in the item viewer.

### 3) Sell tickets

For a selected item, the operator can add players by entering:

- Player name
- Phone number
- Number of tickets to purchase

The app randomly assigns available ticket IDs and writes the updated player state back to the item’s record file.

### 4) Check player status

The player status screen supports searching by:

- Ticket ID
- Phone number
- Player name

### 5) Run the draw

The draw screen animates ticket number generation and stops on the selected ticket:

- If the ticket belongs to a player, the winner is displayed.
- If the ticket has not been sold, the UI clearly indicates that outcome.

---

## 🗂️ Data Storage

At runtime, the application writes data to the user’s home directory:

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

---

## 📁 Project Structure

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

---

## 🏗️ Architecture

### Main application layer

- `raffle.main.App` — loads the JavaFX application, shows the loading screen, and navigates between views
- `raffle.main.EntryPoint` — thin entry point class used to launch the app

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

---

## ✅ Validation and Safety

Operator-facing safeguards include:

- Empty-field validation on item and player forms
- Positive-number validation for ticket counts and ticket price
- Basic phone number validation
- File accessibility checks before writing CSV files
- Confirmation dialogs before deleting items or player records

---

## 📋 Requirements

- Java 21
- Maven 3.9+
- Windows is the primary packaged target for the included `.exe` artifact

---

## 🚀 Running the Application

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

---

## 🧪 Testing

The repository contains automated tests for:

- Controllers
- Models
- CSV reader/writer utilities
- Main application entry points

Current repository test coverage signals include:

- 12 test classes
- 47 JUnit tests

---

## 📦 Included Artifacts

The repository includes generated application artifacts in:

```text
out/artifacts/Sell_and_Win_Raffle_jar/
```

That folder contains:

- `Sell_and_Win_Raffle.jar`
- `Sell & Win Raffle.exe`

---

## 🖥️ Screens in the Application

The JavaFX UI currently includes these views:

- Loading view
- Main view
- Add item view
- Add player view
- Draw view
- View item view
- Player status view

---

## ⚠️ Current Limitations

- CSV-based persistence (not database-backed)
- Designed for local use (not multi-user)
- Images managed through the local file system
- Draw can stop on a ticket that has not been sold (and the UI reports that explicitly)

---

## 🛣️ Future Improvements

- Replace CSV storage with a relational database for stronger concurrency support
- Add sales reports and export features
- Add installer-based distribution for non-technical operators
- Track draw history and operational audit logs
- Improve image management and default image workflows
