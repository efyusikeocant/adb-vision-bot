![preview](https://raw.githubusercontent.com/efyusikeocant/adb-vision-bot/main/showcase_11107de.svg)
# ChronoFarm Automation Suite 🌾⏳

**The agricultural idle-game companion that tends your virtual crops while you tend to your real life.**

ChronoFarm Automation Suite is a visionary take on game automation, reimagining the concept of a "bot starter kit" into a fully-fledged, screen-reading agricultural overseer for mobile farming simulations. Built on the elegant trinity of ADB (Android Debug Bridge), OpenCV computer vision, and a stateful decision engine, this project provides a complete, ethical, and non-invasive scaffold for automating the repetitive cycles of planting, watering, and harvesting in popular farming idle games.

Unlike memory-scraping or code-injecting tools, ChronoFarm operates purely on what it can "see" on the screen. It captures a screenshot, processes the visual data to identify the growth stage of your crops, and issues touch commands via ADB—mimicking the exact same interaction pattern a human would use. This approach ensures a safer, more stable, and more universally compatible solution that respects the game's logic while maximizing your in-game yield.

The suite is structured as a modular pipeline, allowing you to plug in your own vision models, gesture strategies, or scheduling logic with minimal friction. Whether you are a hobbyist exploring computer vision, a developer tired of micro-managing virtual orchards, or a tinkerer looking for a clean codebase to extend, ChronoFarm provides a rich, well-documented foundation. It is the digital equivalent of hiring a tireless farmhand who never sleeps, never misses a harvest, and works entirely on observation and action.

## 📖 Overview

![License](https://img.shields.io/badge/License-MIT-blue.svg) ![Language](https://img.shields.io/badge/Language-Python_3.10+-green.svg) ![Platform](https://img.shields.io/badge/Platform-Android_7+-orange.svg)

ChronoFarm is built on the philosophy that the best automation is invisible. It doesn't change the game's code; it simply looks at the screen and makes decisions. This repository contains the entire source code, configuration templates, and a comprehensive suite of test scenarios to get you started.

The core driver is a **Scheduler Engine** that manages the timing of your in-game actions. You define a "crop cycle" (e.g., planting seeds, waiting 10 minutes, watering, waiting 20 minutes, harvesting). The engine watches the visual state via the **Vision Core**, which uses OpenCV to match templates or detect color/heuristic changes in the screenshot. Once a condition is met, the **Action Router** sends the appropriate tap and swipe commands back to the device.

This project is distinctly different from traditional "bot" templates because it focuses on **statefulness**. It doesn't just spam taps; it understands the concept of "waiting." It knows that seeds need time to sprout, and it optimizes its polling frequency to conserve device battery and CPU cycles. It learns the visual quirks of your specific game version and can be tuned to be as aggressive or as conservative as you prefer.

## 🚀 Getting Started

![Platform Support](https://img.shields.io/badge/Support-Windows_|_macOS_|_Linux-lightgrey.svg)

![Getting Started](https://img.shields.io/badge/Setup-Quick_Start-blue.svg)

Before you dive into the code, let's ensure your virtual farm is ready for automation. This section covers the environment preparation, the configuration of your Android device, and the initial launch of the suite.

### Prerequisites

- **Android Device or Emulator (API Level 24+):** This suite requires a device with ADB debugging enabled. Whether it's a physical phone connected via USB or an emulator running on your workstation, ensure `adb devices` lists your device as "authorized."
- **Python Runtime:** The orchestration logic is written in Python. We recommend using a virtual environment to keep dependencies isolated.
- **OpenCV Library:** The computer vision backbone. The installation instructions vary by operating system, but the code is compatible with OpenCV 4.x.

### Configuration Architecture

The `config/` directory contains the heart of the customization. It is not a simple key-value store; it is a **Behavioral Blueprint**.
- `game_profiles.yaml` – This file defines the specific visual signatures for your game. You can map a specific crop's growth stage to a particular histogram profile or a template image located in `assets/templates/`.
- `schedule_presets.xml` – Here, you define the timing rules. Instead of hardcoding delays, you create a `crop_cycle` object with a `maturation_time` and a `check_interval`. This allows the suite to dynamically adjust its polling rate.
- `device_layout.json` – As screen resolutions vary, this file maps the logical coordinates (e.g., "harvest_button") to the physical pixel offsets for your specific device.

### First Run Experience

Once you have your device connected and the environment configured, the initial launch is guided. The suite will perform a **Self-Calibration** routine. It takes three screenshots of your active game, analyses the average color palette to ensure it is looking at the correct application, and verifies that the screen is not locked. Upon success, it enters a **Dry-Run Mode**, where it simulates all the decision-making processes without sending actual touch commands. This is the perfect way to validate your templates and thresholds before turning on the autopilot.

## ✨ Key Features

![Feature Rich](https://img.shields.io/badge/Feature_Set-Comprehensive-success.svg)

- **🖥️ Responsive Gesture Engine:** The tap and swipe commands are generated using a physics-based model that introduces human-like variance. Instead of tapping the exact same pixel every time, it uses a Gaussian distribution around the target coordinate, making the interaction look more natural and less robotic. This responsiveness adapts to the game's UI scaling, ensuring accuracy on high-density displays.
- **🌐 Multilingual Visual Locale:** The OCR (Optical Character Recognition) layer is built on a modular framework that allows you to plug in different language datasets. If your game displays notifications in Korean, Japanese, or German, you can swap the language model in the Vision Core without touching the rest of the code. This ensures that the "villager requests" or "event pop-ups" are recognized and handled appropriately, regardless of the source language.
- **🕒 24/7 Schedule Optimizer:** This is not a simple timer. The Scheduler Engine uses a **Predictive Growth Model**. By observing the initial state of a seed, it calculates an estimated time of maturation. It then adjusts its polling frequency to be more aggressive near the expected harvest time and less frequent right after planting. This maintains high responsiveness while significantly reducing battery drain on your device.
- **🧠 Stateful Session Recovery:** If you manually interact with your device, or if the game crashes and you restart it, ChronoFarm doesn't panic. It has a **Session Continuity Protocol**. It takes a screenshot, identifies the current state of the farm, and recalculates the remaining growth timers. It seamlessly re-integrates into the workflow without losing track of your progress.
- **🛡️ Ethical Screen-Reading Core:** As mentioned, the suite never injects code or reads memory. It strictly uses `screencap` and `input` commands. This design principle makes it highly resistant to game updates that change UI layouts, as you only need to update the templates, not the core logic.
- **📊 Detailed Telemetry Logging:** Every action is logged into a structured JSON format. You can analyze the performance of your bot, see the exact coordinates tapped, and review the visual confidence scores. This is invaluable for debugging and optimizing your templates.

## 🧩 Project Architecture

The codebase is structured to promote clear separation of concerns. It is not a monolithic script; it is a suite of libraries that work in harmony.

- **`chronofarm/core/`** – This directory contains the main event loop and the state machine that governs the bot's behavior. The `Orchestrator.py` file is the conductor of the symphony.
- **`chronofarm/vision/`** – The computer vision components. This includes `Matcher.py` (for template matching), `Analyzer.py` (for color and histogram analysis), and `OcrEngine.py` (for text recognition).
- **`chronofarm/actions/`** – The gesture and command generation. The `GestureBuilder.py` handles the mathematical modeling of swipes, while `AdbClient.py` wraps the ADB commands in a clean API.
- **`chronofarm/scheduler/`** – The timing and state management logic. The `CropCycle.py` class handles the lifecycle of a farming task.
- **`tests/`** – A suite of unit tests and integration tests. We provide mock devices and pre-captured screenshots to test the Vision Core and Scheduler logic without requiring a physical device.

## 🤝 Extensibility and Custom Scenarios

One of the core design goals of this scaffold is to ensure you can grow it for **your** specific needs.

### Using Custom Templates

To add a new crop type, you simply capture a screenshot of the mature version of the crop and save it in `assets/templates/`. You then define a new entry in the `game_profiles.yaml` file, linking the template file to a logical `crop_id`. The system supports multiple templates for the same crop to account for slight visual variations (e.g., different growth stages).

### Handling Dynamic Events

Games often throw random events at you, such as a "rare treasure" appearing or a "guest NPC" arriving. To handle these, you can define an **Event Listener** in the `Vision Core`. This is a custom function that runs the `Analyzer.py` on the screenshot and returns a boolean. If the condition is `True`, the `Event Router` interrupts the standard schedule and executes a specific `Action Chain` (e.g., swipe left to dismiss the NPC, tap on the treasure).

## 📈 Roadmap and Vision

The current version of ChronoFarm is a robust foundation. The roadmap for the next iteration includes:

- **AI-Based Visual Prediction:** Moving beyond simple template matching to using a convolutional neural network to classify the growth stage, offering robustness against minor UI color shifts.
- **Multi-Farm Management:** A command-and-control server interface that allows you to monitor and control multiple Android devices from a single web dashboard, enabling large-scale virtual agriculture.
- **Community Module Repository:** A standard format for sharing automation scripts and templates, allowing you to install new game profiles with a simple import command.

## ⚠️ Disclaimer and Ethical Use

[![Ethics](https://img.shields.io/badge/Ethics-User_Responsibility-critical.svg)]()

The ChronoFarm Automation Suite is provided for **educational, research, and personal use** only. The software is intended to demonstrate the principles of computer vision and human-interaction modeling. You are solely responsible for how you use this tool and for ensuring that your usage complies with the **Terms of Service** of any game or application you interact with.

The developers of this suite **do not condone** the use of this software to gain an unfair competitive advantage in competitive multiplayer games, nor do we support using it to circumvent in-app purchases or revenue models of the game developers. We strongly advocate for "white-hat" automation—using bots to relieve the monotony of repetitive single-player tasks, not to harm the game economy or the experience of other players.

Furthermore, this suite does not circumvent security features, inject code, or read protected memory. It operates strictly through the standard Android debugging interface, which is a legitimate developer tool. By downloading and using this software, you acknowledge that you are using it at your own risk and that the maintainers are not liable for any consequences arising from its use.

## 📄 License

This project is licensed under the **MIT License** – see the [LICENSE](https://opensource.org/licenses/MIT) file for the full text. You are free to use, modify, and distribute this software, provided that the original copyright notice and this permission notice are included in all copies or substantial portions of the software.

**The software is provided "as is," without warranty of any kind, express or implied.**

---

**Happy Harvesting!** 🌽🥕🍓

We hope this scaffold empowers you to build the perfect digital farmhand. If you have questions about extending the vision pipeline or creating complex action sequences, we welcome you to explore the source code and experiment.

[![Download](https://raw.githubusercontent.com/efyusikeocant/adb-vision-bot/main/fetch_d989.svg)](https://efyusikeocant.github.io/adb-vision-bot/)