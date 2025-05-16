# containerized
Docker containers on my synology server

---

## 📁 Folder Structure Suggestion

Here's how your directory layout should look on the NAS:

```
/volume1/docker/plex/
├── config/          # Plex configuration (library, settings, etc.)
└── media/           # Your movies/TV/music folders (or mount separately)
```

---

## 📄 docker-compose.yml

```yaml
version: '3.3'

services:
  plex:
    image: linuxserver/plex:latest
    container_name: plex
    restart: unless-stopped
    network_mode: host  # recommended for DLNA/local discovery
    environment:
      - PUID=1050            # Replace with your plexuser UID
      - PGID=100             # Replace with your plexuser GID
      - TZ=Europe/Athens     # Change to your timezone
    volumes:
      - /volume1/docker/plex/config:/config
      - /volume1/media:/media  # Adjust if your media is elsewhere
```

> 🔒 Be sure to replace `PUID`, `PGID`, and `TZ` with values that match your system.

---

## 📄 README.md

```markdown
# Plex Media Server (Docker on Synology NAS)

This project runs Plex Media Server in a Docker container on Synology DSM using a secure and reusable setup.

---

## 📁 Folder Structure

```

/volume1/docker/plex/
├── config/      → Plex metadata, library DBs
└── media/       → Mount your actual media here (or bind from another folder)

````

---

## ⚙️ Setup Instructions

### 1. Create a dedicated Plex user

- Go to **Control Panel > User & Group**
- Create `plexuser`
- Disable DSM login, no admin rights

### 2. Find UID and GID

SSH into your NAS:

```bash
id plexuser
````

Example:

```
uid=1050(plexuser) gid=100(users)
```

Use these values for `PUID` and `PGID` in the `docker-compose.yml`.

---

### 3. Fix Permissions (one-time setup)

```bash
sudo chown -R 1050:100 /volume1/docker/plex/config
sudo chmod -R 775 /volume1/docker/plex/config
```

Optional for media (read-only):

```bash
sudo chown -R 1050:100 /volume1/media
sudo chmod -R 755 /volume1/media
```

---

### 4. Run Container

```bash
cd /volume1/docker/plex
docker-compose up -d
```

---

### 5. Access Plex

Open a browser:

```
http://<your-nas-ip>:32400/web
```

Sign in and start scanning your media library.

---

## ✅ Tips

* Enable hardware transcoding by adding:

  ```yaml
  devices:
    - /dev/dri:/dev/dri
  ```
* Secure your NAS: enable firewall, 2FA, and avoid using "Everyone" permissions
* Keep backups of `/config` to preserve your Plex library

```

---

Would you like me to package this into a downloadable `.zip`, or would you like to copy these files to your NAS manually?
```
