# WiFi Password Finder Script

## Introduction

This Python script helps you retrieve the WiFi passwords for networks that were previously connected to your system. If you’ve forgotten your WiFi password, this script can recover it for networks you've connected to in the past.

**Note:** The script only works for WiFi networks that have been connected to the system before. It will not work for unknown or nearby WiFi networks.

## Prerequisites

- Python (any version 3.x should work)
- No external libraries are required, as the script uses Python's built-in `subprocess` module.

## How It Works

The script runs two terminal commands to fetch the WiFi profiles and their associated passwords:

1. **`netsh wlan show profiles`**: Lists all WiFi networks your system has connected to.
2. **`netsh wlan show profile PROFILE-NAME key=clear`**: Retrieves the password for a specific network.

### Code Explanation

- The script first retrieves the list of WiFi profiles from your system.
- It then runs the second command for each profile to find the associated password.
- The password, if available, is displayed alongside the network name.

```python
import subprocess

# Running command to get WiFi profiles
command = subprocess.check_output(['netsh', 'wlan', 'show', 'profiles']).decode('utf-8').split('\n')
profiles = [i.split(":")[1][1:-1] for i in command if "All User Profile" in i]

# Loop through profiles and retrieve passwords
for i in profiles:
    results = subprocess.check_output(['netsh', 'wlan', 'show', 'profile', i, 'key=clear']).decode('utf-8').split('\n')
    results = [b.split(":")[1][1:-1] for b in results if "Key Content" in b]
    
    # Display the profile name and password (if available)
    try:
        print ("{:<30}|  {:<}".format(i, results[0]))
    except IndexError:
        print ("{:<30}|  {:<}".format(i, ""))
input("")
