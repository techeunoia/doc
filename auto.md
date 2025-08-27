# iTwist Auto-Start Setup

This guide explains how to configure **iTwist** to start automatically 
on openSUSE, either:

- When logging into the **IceWM desktop session**.
- Or at **system boot** using a `systemd` service.

---

## Option 1: Auto-Start in IceWM

### 1. Create IceWM config directory

Make sure the IceWM configuration directory exists:

```bash
mkdir -p ~/.icewm
cp -r /etc/icewm/* ~/.icewm/
```

---

### 2. Create or edit the startup file

Open the IceWM startup file:

```bash
nano ~/.icewm/startup
```

Add the following lines:

```bash
#!/bin/sh
/home/SAVIO/orion/iTwist/iTwist.sh &
```

Save and exit (`CTRL+O`, `Enter`, `CTRL+X`).

---

### 3. Make the startup file executable

```bash
chmod +x ~/.icewm/startup
```

---

### 4. Restart IceWM

Log out and back in, or reboot.  
IceWM will now automatically launch **iTwist** when the desktop starts.

---

## Option 2: Auto-Start at System Boot (systemd)

If you want **iTwist** to run even before logging in, create a `systemd` service.

### 1. Create a service file

Run:

```bash
sudo nano /etc/systemd/system/itwist.service
```

Paste the following:

```ini
[Unit]
Description=iTwist Auto Start
After=network.target

[Service]
ExecStart=/home/SAVIO/orion/iTwist/iTwist.sh
WorkingDirectory=/home/SAVIO/orion/iTwist/
Restart=always
User=SAVIO

[Install]
WantedBy=multi-user.target
```

> ⚠️ Replace `SAVIO` with your actual Linux username.

---

### 2. Reload systemd and enable service

```bash
sudo systemctl daemon-reload
sudo systemctl enable itwist.service
sudo systemctl start itwist.service
```

---

### 3. Check service status

```bash
systemctl status itwist.service
```

If everything is correct, iTwist will now start **automatically at boot**.

---

## Notes

- Ensure the iTwist script is executable:

  ```bash
  chmod +x /home/SAVIO/orion/iTwist/iTwist.sh
  ```

- Use **IceWM auto-start** if you want iTwist only when logged into IceWM.
- Use **systemd service** if you want iTwist always running in the background.

---
