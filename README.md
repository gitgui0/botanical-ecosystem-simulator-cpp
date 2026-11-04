# Turn-Based Botanical Ecosystem & Garden Engine (Modern C++20 & OOP Architecture)

An interactive, turn-based botanical ecosystem simulation engine developed in **Modern C++20**. The engine models a dynamic 2D grid terrain where autonomous biological entities (flora, invasive weeds, specialized plants) interact with environmental soil dynamics (moisture, nutrients, decay) and a user-controlled gardener entity through polymorphically dispatched commands and specialized tools.

Developed for the **Object-Oriented Programming (Programação Orientada a Objetos)** course in Computer Engineering at **Instituto Superior de Engenharia de Coimbra (ISEC)**.

---

## Architecture & Polymorphic Class Hierarchy

The project strictly emphasizes clean object-oriented design, strong encapsulation, and polymorphic inheritance hierarchies:

```
                            +-------------------+
                            |     EstaSolo      |
                            | (Abstract Base)   |
                            +---------+---------+
                                      |
                 +--------------------+--------------------+
                 |                                         |
       +---------v---------+                     +---------v---------+
       |      Planta       |                     |    Ferramenta     |
       |  (Abstract Flora) |                     |  (Abstract Tool)  |
       +---------+---------+                     +---------+---------+
                 |                                         |
     +-----------+-----------+                 +-----------+-----------+
     |           |           |                 |           |           |
+----v----+ +----v----+ +----v----+       +----v----+ +----v----+ +----v----+
|  Cacto  | | Roseira | | Orquídea|       | Regador | |  Adubo  | | Tesoura |
+---------+ +---------+ +---------+       +---------+ +---------+ +---------+
     |
+----v----+
|  Erva   |
| Daninha |
+---------+
```

### Key Engineering Features:
* **Polymorphic Class Hierarchies:** Clean separation between grid soil entities (`BocadoSolo`), flora lifecycle dynamics (`Planta` -> `Cacto`, `Roseira`, `Orquidea`, `ErvaDaninha`), and gardening tools (`Ferramenta` -> `Regador`, `Adubo`, `TesouraPoda`, `AceleradorCrescimento`).
* **Strict RAII & Memory Management:** Direct, leak-free memory management utilizing dynamic matrix allocations and strict Resource Acquisition Is Initialization (RAII) principles without external dependencies.
* **Command Pattern (`simulacao/Comando`):** Fully decoupled CLI parser and command dispatcher executing discrete simulation ticks, gardener movement, tool equipment, plant harvesting, and batch execution scripts.
* **Modern C++20 Standard:** Utilizes C++20 features for type safety, modular headers, and clean build automation via CMake.

---

## Interactive CLI Command Reference

The engine runs an interactive command loop supporting the following operations:

### Simulation Control
* `jardim <linhas> <colunas>`: Initializes a new grid with specified dimensions (1 to 26).
* `avanca [n]`: Advances the simulation by `n` discrete time steps (defaults to 1).
* `fim`: Terminates the engine session.

### Gardener Navigation
* `entra <linha><coluna>`: Spawns the gardener at the designated grid coordinate (e.g. `entra a b`).
* `c`: Moves gardener one tile North (up).
* `b`: Moves gardener one tile South (down).
* `e`: Moves gardener one tile West (left).
* `d`: Moves gardener one tile East (right).
* `sai`: Vacates the gardener from the garden terrain.

### Cultivation, Harvest & Inventory
* `planta <especie>`: Sows a plant species on the gardener's current tile (e.g. `cacto`, `roseira`, `orquidea`).
* `colhe`: Harvests or trims the flora on the current tile.
* `compra <ferramenta>`: Purchases a gardening tool into inventory (`regador`, `adubo`, `tesoura`, `acelerador`).
* `pega <ferramenta>`: Equips a tool from the inventory for immediate use.
* `larga`: Drops the currently equipped tool onto the terrain tile.

### State Inspection & Telemetry
* `lplantas`: Lists all living flora currently populating the grid.
* `lplanta <id>`: Displays detailed health, lifecycle, and nutrient metrics for a specific plant.
* `lsolo`: Outputs soil telemetry for the current tile (moisture, organic nutrients, compaction).
* `larea`: Inspects environmental metrics in the immediate perimeter around the gardener.
* `lferr`: Displays the gardener's active tool inventory and remaining charges.

### Snapshot Persistence & Script Automation
* `grava <nome>`: Serializes an in-memory snapshot of the simulation state.
* `recupera <nome>`: Restores a previously saved simulation snapshot.
* `apaga <nome>`: Deletes a saved snapshot from memory.
* `executa <ficheiro>`: Reads and executes a batch sequence of commands from an external script file.

---

## Tech Stack

* **Language:** C++20 (`set(CMAKE_CXX_STANDARD 20)`)
* **Build System:** CMake 3.30+
* **Compiler:** GCC / Clang / MSVC with modern C++ support

---

## Building & Running

### Prerequisites
* CMake (>= 3.20)
* C++20 compliant compiler (`g++ >= 11`, `clang++ >= 13`, or MSVC)

### Build with CMake
```bash
# Generate build configuration
cmake -B build -S .

# Compile target executable
cmake --build build --config Release
```

### Run
```bash
# Linux / macOS:
./build/TP

# Windows:
.\build\Release\TP.exe
```

---

## Repository Structure

```
├── include/
│   ├── jardim/         # Terrain, Soil (BocadoSolo), and Gardener models
│   ├── plantas/        # Flora base and derived classes (Cacto, Roseira, etc.)
│   ├── ferramentas/    # Gardening tools and dynamic modifiers
│   └── simulacao/      # Simulation loop, CLI interface, and Command parser
├── src/
│   ├── jardim/         # Implementation of grid and soil dynamics
│   ├── plantas/        # Flora growth, reproduction, and absorption logic
│   ├── ferramentas/    # Tool actions and resource modifications
│   └── simulacao/      # Main simulator engine and CLI parser
├── CMakeLists.txt      # Modern CMake configuration
├── main.cpp            # Entry point
└── README.md
```
