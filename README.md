# Fiestaboard-WebApp
A self hosted website to interact with a Vestaboard as a live scoreboard

A lightweight, self-hosted web application designed to run on a Raspberry Pi. It provides a mobile-friendly UI to track player scores and instantly updates your Vestaboard in real-time using the Local API. 

## Features
* **Real-time Scoring:** Tap to add/subtract points or edit scores directly.
* **Multi-Board Support:** Dynamically adjusts layouts for Vestaboard Standard (up to 6 players) and Vestaboard Note (up to 3 players).
* **Custom Colors & Titles:** Assign specific Vestaboard color tiles to players and display a custom game title.
* **Fiestaboard Integration:** Easily pause/resume your Fiestaboard server during gameplay so it doesn't overwrite your scores.
* **Browser-Based Config:** Set your IP and API keys directly from the web UI (no terminal editing required).

<img width="200" alt="Scoreboard Setup" src="./screenshots/IMG_2600.jpeg" /> <img width="200" alt="Settings" src="./screenshots/IMG_2599.jpeg" /> <img width="200" alt="Player Details" src="./screenshots/IMG_2601.jpeg" /> <img width="200" alt="Score Entry" src="./screenshots/IMG_2604.jpeg" />

<img width="400" alt="Vestaboard Display" src="./screenshots/VB_display.png" />

## Prerequisites
* A Raspberry Pi (or any Linux-based machine)
* Python 3 installed
* Your Vestaboard's Local API enabled (you will need the IP address and Local API Key)

## Installation

**1. Clone the repository**
Open your terminal and clone this project into your home directory:
```bash
cd ~
git clone https://github.com/cordell25/Fiestaboard-WebApp.git
cd Fiestaboard-WebApp
```

**2. Create a Virtual Environment**
It is highly recommended to run this in an isolated Python environment to protect your system packages.
```
sudo apt update
sudo apt install python3-venv
python3 -m venv venv
source venv/bin/activate
```

**3. Install Dependencies**
```
pip install Flask requests
```

**First-Time Setup**
Before running it in the background, run the app manually to configure your settings:
```
python3 app.py
```

**Test that everything is working**
Open a web browser on your phone or computer and navigate to http://<YOUR_PI_IP_ADDRESS>:5000.
Click the ⚙️ Settings button on the setup screen.
Enter your Vestaboard IP, Local API Key, and Fiestaboard UUID (optional). Save the settings.
Go back to your terminal and press Ctrl+C to stop the server.

**Running as a Background Service (Recommended)**
To keep the scoreboard running 24/7 and automatically start when the Pi reboots, set it up as a system service.

**1. Create the service file:**
```
sudo nano /etc/systemd/system/fiestaboard-webapp.service
```

**2. Paste the following configuration:**
(Note: If your Raspberry Pi username is not _fiesta_, update the User, WorkingDirectory, and ExecStart paths below to match your actual username).
```
[Unit]
Description=Fiestaboard WebApp Hub
After=network.target

[Service]
User=fiesta
WorkingDirectory=/home/fiesta/Fiestaboard-WebApp
ExecStart=/home/fiesta/Fiestaboard-WebApp/venv/bin/python app.py
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```
Save and exit the editor (Press Ctrl+O, Enter, then Ctrl+X).

**3. Enable and start the service:**
Run these commands to apply the new service and turn it on:
```
sudo systemctl daemon-reload
sudo systemctl enable fiestaboard-webapp
sudo systemctl start fiestaboard-webapp
```

**Useful Troubleshooting Commands**
If you ever need to troubleshoot the background service, use these commands:
Check status: sudo systemctl status fiestaboard-webapp (press q to exit the log view)
Restart app: sudo systemctl restart fiestaboard-webapp
Stop app: sudo systemctl stop fiestaboard-webapp
View recent errors: sudo journalctl -u fiestaboard-webapp -n 20
