# ♟️ Java Chess Engine — Ubuntu Setup Guide

Complete installation and run guide for **Ubuntu 20.04 / 22.04 / 24.04**.

---

## 📋 Table of Contents

1. [System Requirements](#system-requirements)
2. [Install Java JDK on Ubuntu](#install-java-jdk-on-ubuntu)
3. [Verify Installation](#verify-installation)
4. [Extract the Project](#extract-the-project)
5. [Compile the Project](#compile-the-project)
6. [Run the Game](#run-the-game)
7. [Game Modes](#game-modes)
8. [How to Play](#how-to-play)
9. [Network Game (2 Players)](#network-game-2-players)
10. [Run Perft Test](#run-perft-test)
11. [Troubleshooting](#troubleshooting)
12. [Uninstall Java](#uninstall-java)

---

## 1. System Requirements

| Item | Requirement |
|---|---|
| OS | Ubuntu 20.04 / 22.04 / 24.04 (64-bit) |
| Java | JDK 17 or higher |
| RAM | 256 MB minimum |
| Disk | 50 MB free |
| Network | Only needed for 2-player network mode |

---

## 2. Install Java JDK on Ubuntu

Open your terminal (`Ctrl + Alt + T`) and run:

### Step 1 — Update package list

```bash
sudo apt update
```

### Step 2 — Install JDK 17

```bash
sudo apt install openjdk-17-jdk -y
```

> If you prefer JDK 21 (latest LTS):
> ```bash
> sudo apt install openjdk-21-jdk -y
> ```

### Step 3 — Set JAVA_HOME (recommended)

```bash
echo 'export JAVA_HOME=/usr/lib/jvm/java-17-openjdk-amd64' >> ~/.bashrc
echo 'export PATH=$JAVA_HOME/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

> For JDK 21, replace `java-17` with `java-21` in the path above.

---

## 3. Verify Installation

```bash
java -version
javac -version
```

Expected output:
```
openjdk version "17.0.x" 2024-xx-xx
OpenJDK Runtime Environment (build 17.0.x+x-Ubuntu-...)
OpenJDK 64-Bit Server VM (build 17.0.x+x-Ubuntu-..., mixed mode, sharing)

javac 17.0.x
```

Both `java` and `javac` must be found. If only `java` works (not `javac`), you installed the JRE instead of the JDK — re-run the install command above.

---

## 4. Extract the Project

### If you have the zip file:

```bash
# Install unzip if not already present
sudo apt install unzip -y

# Extract
unzip ChessEngine.zip

# Enter the project folder
cd ChessEngine
```

### Verify folder structure:

```bash
ls -R src/chess/
```

You should see:
```
src/chess/engine/   src/chess/model/   src/chess/network/
src/chess/ui/       src/chess/util/
```

---

## 5. Compile the Project

### Make the script executable and run it:

```bash
chmod +x compile.sh
./compile.sh
```

### Expected output:

```
Compiling Chess Engine...
Build successful!
  Local game:     java -cp out chess.ui.Main
  Perft test:     java -cp out chess.util.PerftTest
  Network server: java -cp out chess.network.ChessServer
  Network client: java -cp out chess.network.ChessClient
```

### If compile.sh doesn't work, compile manually:

```bash
mkdir -p out
find src -name "*.java" | sort > sources.txt
javac --release 17 -d out @sources.txt
echo "Done: $?"
```

---

## 6. Run the Game

```bash
chmod +x run.sh
./run.sh
```

Or directly:

```bash
java -cp out chess.ui.Main
```

### Startup menu:

```
┌─────────────────────────────────┐
│   Chess Engine v1.0 - Startup   │
├─────────────────────────────────┤
│  1) Local game                  │
│  2) Start network server        │
│  3) Connect as network client   │
└─────────────────────────────────┘
Choose:
```

Type `1` and press Enter for a local game.

---

## 7. Game Modes

### Mode 1 — Human vs Human (same keyboard)

```
Choose: 1

╔════════════════════════════════╗
║    Java Chess Engine v1.0      ║
╠════════════════════════════════╣
║  1) Human vs Human             ║
║  2) Human vs AI                ║
╚════════════════════════════════╝
Mode: 1
```

### Mode 2 — Human vs AI

```
Mode: 2
Play as (W/B): W
AI depth (1-5, default 3): 3
```

AI depth guide:

| Depth | Speed | Strength |
|---|---|---|
| 1 | Instant | Very easy |
| 2 | Instant | Easy |
| 3 | < 1 second | Medium |
| 4 | 1–5 seconds | Hard |
| 5 | 5–30 seconds | Very hard |

---

## 8. How to Play

### Move format

Moves use **UCI notation**: type the starting square then the destination square.

```
e2e4      ← move pawn from e2 to e4
g1f3      ← move knight from g1 to f3
e1g1      ← kingside castling (king moves two squares right)
e1c1      ← queenside castling (king moves two squares left)
e7e8q     ← promote pawn to queen (use q/r/b/n)
```

### The board display

```
  a b c d e f g h
8 r n b q k b n r 8
7 p p p p p p p p 7
6 . _ . _ . _ . _ 6
5 _ . _ . _ . _ . 5
4 . _ . _ . _ . _ 4
3 _ . _ . _ . _ . 3
2 P P P P P P P P 2
1 R N B Q K B N R 1
  a b c d e f g h
```

- **UPPERCASE** = White pieces
- **lowercase** = Black pieces
- `. _` = empty squares (alternating pattern)

### Piece symbols

| Symbol | Piece |
|---|---|
| K / k | King |
| Q / q | Queen |
| R / r | Rook |
| B / b | Bishop |
| N / n | Knight |
| P / p | Pawn |

### In-game commands

| Command | What it does |
|---|---|
| `e2e4` | Make a move |
| `moves` | Show all legal moves you can make |
| `fen` | Print the current board as a FEN string |
| `save` | Save game to a `.pgn` file |
| `resign` | Resign the current game |
| `help` | Show command reference |

---

## 9. Network Game (2 Players)

Play chess with a friend over the network.

### Step 1 — Open two terminal windows

Press `Ctrl + Alt + T` to open a terminal.  
Right-click the terminal and choose **Open New Tab** or **Open New Window** for the second one.

### Step 2 — Start the server (Terminal 1)

```bash
cd ChessEngine
java -cp out chess.ui.Main
# Choose: 2
```

You will see:
```
[Server] Listening on port 5555 ...
[Server] Waiting for Player 1 (WHITE) ...
```

### Step 3 — Connect as Player 1 (Terminal 2)

```bash
cd ChessEngine
java -cp out chess.ui.Main
# Choose: 3
```

You will see:
```
[Client] Connected to localhost:5555
[Client] You are playing as: WHITE
```

### Step 4 — Connect as Player 2 (Terminal 3)

```bash
cd ChessEngine
java -cp out chess.ui.Main
# Choose: 3
```

You will see:
```
[Client] Connected to localhost:5555
[Client] You are playing as: BLACK
```

### Step 5 — Play!

Both players type moves in their own terminal window. WHITE moves first.

### Network-only commands

| Command | What it does |
|---|---|
| `e2e4` | Make a move |
| `chat hello` | Send a message to your opponent |
| `draw` | Offer or accept a draw |
| `resign` | Resign the game |

### Playing over LAN (different computers)

1. Find the server machine's IP address:
   ```bash
   ip addr show | grep "inet " | grep -v 127.0.0.1
   # Example: inet 192.168.1.105
   ```

2. On the server machine, make sure port 5555 is allowed:
   ```bash
   sudo ufw allow 5555
   ```

3. On the **client machine**, edit `src/chess/network/ChessClient.java`:
   ```java
   // Change this line:
   private static final String HOST = "localhost";
   // To:
   private static final String HOST = "192.168.1.105";  // use actual server IP
   ```

4. Recompile and run on both machines.

---

## 10. Run Perft Test

The Perft test verifies that the move generator is 100% correct.

```bash
java -cp out chess.util.PerftTest
```

Expected output:
```
=== Perft Test - Starting Position ===
Expected: depth1=20  depth2=400  depth3=8902  depth4=197281

  depth 1 ->      20  (  0 ms)  PASS
  depth 2 ->     400  (  1 ms)  PASS
  depth 3 ->   8,902  (  8 ms)  PASS
  depth 4 -> 197,281  ( 45 ms)  PASS
```

All 4 lines must show **PASS**. These are internationally verified node counts for the starting chess position.

---

## 11. Troubleshooting

### ❌ `javac: command not found`

You have the Java runtime (JRE) but not the development kit (JDK).

```bash
sudo apt install openjdk-17-jdk -y
```

---

### ❌ `java: command not found`

Java is not installed or not in PATH.

```bash
sudo apt install openjdk-17-jdk -y
source ~/.bashrc
```

---

### ❌ `error: release version 17 not supported`

Your installed Java version is older than 17.

Check what you have:
```bash
java -version
```

Install JDK 17:
```bash
sudo apt install openjdk-17-jdk -y
sudo update-alternatives --config java
sudo update-alternatives --config javac
# Select the openjdk-17 option
```

---

### ❌ `java.lang.UnsupportedClassVersionError`

The `.class` files were compiled with a newer Java than you are running.

Delete the `out/` folder and recompile:
```bash
rm -rf out/
./compile.sh
```

---

### ❌ `Connection refused` in network mode

The server is not running or the port is blocked.

1. Make sure you started the server first (option 2 in main menu).
2. Check the port is not blocked:
   ```bash
   sudo ufw status
   sudo ufw allow 5555
   ```
3. Verify the server is listening:
   ```bash
   ss -tlnp | grep 5555
   ```

---

### ❌ Multiple Java versions installed

```bash
# See all installed versions
update-alternatives --list java

# Switch to JDK 17
sudo update-alternatives --config java
sudo update-alternatives --config javac
```

---

### ❌ `permission denied: ./compile.sh`

```bash
chmod +x compile.sh run.sh
./compile.sh
```

---

### ❌ AI is too slow

Reduce the AI depth:
```
AI depth (1-5, default 3): 2
```

Depth 2 is instant. Depth 3 is usually under 1 second.

---

## 12. Uninstall Java

If you no longer need Java after the project:

```bash
sudo apt remove openjdk-17-jdk -y
sudo apt autoremove -y
```

---

## 📬 Quick Reference Card

```bash
# Install Java
sudo apt update && sudo apt install openjdk-17-jdk -y

# Extract and build
unzip ChessEngine.zip && cd ChessEngine
chmod +x compile.sh && ./compile.sh

# Run local game
java -cp out chess.ui.Main

# Run perft test
java -cp out chess.util.PerftTest

# Run network server
java -cp out chess.network.ChessServer

# Run network client
java -cp out chess.network.ChessClient
```

---

*Pure Java 17 — no frameworks, no dependencies, works on any Ubuntu machine with JDK 17+.*
