Ragnar (Headless ARM/Linux Edition)
DZAZ method.

This guide explains how to run Ragnar on a Hackberry Pi Zero, uConsole CM4, Raspberry Pi Zero 2W, Pi 5, Hackberry pi CM5 or any ARM-based Linux system without the e-paper display. The modified Ragnar.py used for headless operation is stored in this repository:
https://github.com/DezusAZ/hbp0_ragnar/Ragnar.py

1. Clone the Original Ragnar Repository.
Clone Pierre Gode’s original Ragnar project:
git clone https://github.com/PierreGode/Ragnar.git
cd Ragnar

2. Replace the Original Ragnar.py
Download or copy the modified Ragnar.py from:
https://github.com/DezusAZ/hbp0_ragnar/Ragnar.py
mv Ragnar.py Ragnar.py.old
cp /path/to/downloaded/Ragnar.py /Ragnar.py
(Or manually paste it into Ragnar/Ragnar.py after deleting the original)
chmod +x Ragnar.py

some systems require different things so to ensure everything is installed on your system before continuing:
sudo apt install python3-venv python3-dev python3-pip bluetooth libbluetooth-dev python3-bluez -y

4. Create and Activate a Python Virtual Environment
This is required on Kali Linux and recommended for all systems:
python3 -m venv venv
source venv/bin/activate
a small "(venv)" will appear at the start of your command line.
pip install --upgrade pip

6. Install Python Dependencies
Install all required Python packages inside the virtual environment:
pip install -r requirements.txt
not sure why but "requests" didnt properly install so i had to do that one manually
pip install requests

7. Install Bluetooth Support (Recommended)
The PyBluez pip package does not install correctly on modern Python. Install the correct system packages instead:
sudo apt install python3-bluez bluetooth libbluetooth-dev -y
(doing this inside the venv for safe measure, it should have installed when we ran the command at the start but I had some issues)

If these packages cannot be installed, Ragnar will still run, but Bluetooth features will be limited.

6. Run Ragnar
Ensure the logs directory is writable
If you are running Ragnar inside Downloads or another restricted folder, set permissions:
sudo chmod -R 777 data/logs
Or simply run the entire project from a folder you own (root or home)
mv ~/Downloads/Ragnar ~/Ragnar
Start Ragnar using the Python interpreter inside the virtual environment:
If you need root (required for some network scans, run with sudo):
sudo ./venv/bin/python3 Ragnar.py

8. Access the Web Interface
On the device:
open browser navigate to http://127.0.0.1:8000
or from another device on the same network http://<device-ip>:8000

9. Stopping Ragnar
sometimes I have found that a simple Ctrl+c will not work but I still want it to shut down cleanly, in that case I will
Open another terminal and run:
sudo pkill -f Ragnar.py

10. Data Storage Locations
Ragnar saves all scan results in the following paths:
data/ragnar.db        (SQLite database containing all scan history and hosts)
data/netkb.csv        (network knowledge base)
data/livestatus.csv   (live status information)
data/logs/            (runtime logs)
to inspect the database:
sqlite3 data/ragnar.db

11. Network Switching
Ragnar is not tied to any specific network like its predicessor Bjorn. Whenever the device (hackberry, uCon, etc.) connects to a new wired or wireless network, Ragnar automatically detects the new IP range and begins scanning it without requiring any configuration changes.

ALL CREDIT FOR ORIGINAL CODE GOES TO Pierre Gode! I simply wanted a way to run this on my cyber decks hahaha.
