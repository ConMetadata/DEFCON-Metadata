# DEF CON Metadata Scraper & Kiosk

This project provides an automated, zero-touch metadata scraper for DEF CON videos, alongside a pre-configured Jellyfin media server (Kiosk) optimized for offline viewing and conference environments. 

## Prerequisites
* Docker and Docker Compose installed.

---

## Deployment Option 1: The Full Kiosk (Default)
This spins up both the automated scraper and a fully configured Jellyfin instance. You do not need to clone this repository to run it.

1. Create a new directory for the deployment and enter it:
   `mkdir defcon-kiosk && cd defcon-kiosk`

2. Download the deployment configuration file:
   `curl -O https://raw.githubusercontent.com/ConMetadata/DEFCON-Metadata/main/docker-compose.yml`

3. Start the stack:
   `docker compose up -d`

4. Access the Jellyfin Kiosk at `http://localhost:8096`.
   * **Admin login:** `admin` / `ChangeMeAdmin`
   * **Guest login:** `guest` / `ChangeMeGuest`

*Note: The media and configuration files will be automatically contained within a `data/` folder inside your `defcon-kiosk` directory.*

**Troubleshooting Permissions:** If the scraper container logs show "Permission denied" errors when downloading files, your host machine likely created the `./data` directories as the `root` user. Run `sudo chown -R 1000:1000 ./data` from your deployment folder to fix this.

---

## Deployment Option 2: Using an Existing Jellyfin Server
If you already have a Jellyfin server running and just want the scraper to inject the DEF CON metadata and speaker images into your existing setup:

1. Download the `docker-compose.yml` file using the `curl` command above.
2. Open the file in a text editor.
3. **Delete** the entire `jellyfin-cons` service block.
4. In the `infocon-scraper` block, remove the `depends_on:` section.
5. Update the `volumes:` section of the scraper to point to the absolute paths of your existing server:
   
   volumes:
     - /your/existing/jellyfin/config:/config
     - /your/existing/media/shows:/data/shows

6. Run `docker compose up -d`.
