# 🎮 Rollback-Core - Smooth online play for your games

[![Download Rollback-Core](https://img.shields.io/badge/Download-Release_Page-blue)](https://github.com/Outsized-flashboarding111/Rollback-Core/releases)

Rollback-Core provides a way to make online multiplayer games feel responsive. It keeps the game state consistent between players. This tool handles the complex math behind networking so you can focus on building your game world.

## 📦 How to get the software

You need to obtain the latest version of the toolkit from our repository.

1.  Visit the [official releases page](https://github.com/Outsized-flashboarding111/Rollback-Core/releases).
2.  Look for the section labeled "Assets" at the bottom of the newest release.
3.  Click the file ending in `.zip` to start the download.
4.  Once the file finishes, open your Downloads folder in Windows.
5.  Right-click the zip file and select "Extract All."
6.  Choose a location on your hard drive and click "Extract."

## ⚙️ Minimum system requirements

Your computer needs to meet these basic standards to run the simulation:

*   Operating System: Windows 10 or Windows 11.
*   Processor: Quad-core CPU at 3.0 GHz or faster.
*   Memory: 8 GB of RAM.
*   Graphics: DirectX 11 compatible video card.
*   Network: A stable broadband internet connection with latency below 100ms.

## 🚀 Setting up the plugin

Follow these steps to add the core functionality to your Unreal Engine 5 project:

1.  Open your existing Unreal Engine project folder.
2.  Create a folder named `Plugins` inside your main project directory if one does not exist.
3.  Copy the extracted `Rollback-Core` folder into this new `Plugins` folder.
4.  Restart Unreal Engine. The editor will prompt you to rebuild modules if necessary. Select "Yes" to compile the new code.
5.  Check the "Plugins" menu inside the editor. Ensure "Rollback-Core" shows as enabled.

## 🔧 Understanding rollback networking

This tool uses a specific method to handle lag. In older games, the screen would pause to wait for data from the other player. This causes jerky motion. 

Rollback-Core predicts what the other player will do. If the prediction is correct, the game continues without interruption. If the prediction is wrong, the game quickly rewinds to the last known correct state and reapplies the correct inputs. This happens in milliseconds, which makes the correction invisible to the human eye. 

This approach requires the game to be deterministic. This means the game must reach the same outcome every time a player makes the same input. Rollback-Core handles the state capture and frame verification to ensure this consistency.

## 📡 Configuring network settings

The tool communicates using the UDP protocol. This method sends data packets without waiting for a confirmation receipt for every single piece of information. This reduces the time it takes for data to travel across the internet.

To configure your network settings:

1.  Open the Project Settings menu.
2.  Locate the Rollback-Core section in the sidebar.
3.  Adjust the input buffer settings based on your average connection quality. A higher buffer provides more room for packet loss but adds slight delay. A lower buffer feels more responsive but shows more visual jitters if the connection is poor.
4.  Test your settings in a local simulation mode before testing over the internet.

## 💾 Managing game states

The software relies on SaveGame-reflection to track the progress of your game. Every frame, the system captures a snapshot of the current state of characters, items, and environments.

When a network correction occurs, Rollback-Core loads one of these previous snapshots. You must ensure that every moving part of your game relies on the internal state management provided by this plugin. Avoid using standard global variables for player health or positions. Instead, use the provided reflection interfaces. This allows the system to save and restore information predictably.

## 🛡️ Input redundancy

Network packets can go missing. If a packet gets dropped, the simulation could get out of sync. 

Rollback-Core utilizes input redundancy to solve this. Every packet sent includes the current input plus the inputs from the few seconds preceding it. If a packet is lost, the next packet contains the missing data, allowing the receiving computer to fill in the gaps without needing a request-retry cycle.

## 📋 Troubleshooting common issues

If you encounter difficulties, review these common scenarios:

*   The plugin fails to load: Ensure you extracted the files correctly and kept the folder structure intact. The `uplugin` file must reside in the root of the folder.
*   Input lag feels high: Increase the input buffer size in the settings menu. Test incrementally until the movement feels smooth.
*   Players get out of sync: Check your game loop. If your game uses random numbers, ensure you use the deterministic random number generator provided by the plugin. Standard system random functions will break the simulation.
*   Connection drops: Verify that your firewall allows traffic on the UDP ports configured in the project settings.

## 📜 Legal information

The source code and the compiled binaries fall under the MIT license. You may use this software in your own projects, whether they are free or commercial. You must include the original copyright notice in any distribution of the software. This license provides freedom to adapt and share the code as long as the terms of the MIT license are met.