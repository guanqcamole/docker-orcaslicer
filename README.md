# QPrints

QPrints is a Docker-based 3D printing solution integrating OrcaSlicer, OctoPrint, and OctoEverywhere for browser-based slicing and remote printer management. It supports the Lulzbot Mini 2 with webcam monitoring via a Raspberry Pi.

## Setup Instructions

### 1. Build the Docker Image
1. Install Docker Desktop from [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/).
2. Build the QPrints image:
   ```bash
   docker build -t qprints .
   ```
3. Run the container:
   ```bash
   docker run -p 3000:3000 qprints
   ```
4. Access the OrcaSlicer web UI at `http://localhost:3000`.

### 2. Configure the Raspberry Pi with OctoPrint
1. Flash OctoPi from [https://octoprint.org/download/](https://octoprint.org/download/) onto the Pi’s SD card, noting credentials and ensuring network connectivity.
2. Connect a webcam (e.g., Logitech C270) to the Pi via USB.
3. Power the Pi with a >5V source (e.g., 15V Nintendo Switch charger) to avoid undervoltage issues.
4. Access the OctoPrint web UI at `http://<thenameyousetwhenflashing>.local` (e.g., `http://raspberrypi.local`) or the Pi’s IP. If it fails, reflash and follow the tutorial at [https://octoprint.org/download/](https://octoprint.org/download/).
5. In OctoPrint:
   - Note the Main API Key or generate instance-specific keys.
   - Add the printer (e.g., Lulzbot Mini 2) with parameters like bed dimensions and build height.
   - Install the OctoEverywhere plugin from settings. Check for undervoltage if installation fails.
6. Log into OctoEverywhere (15-day trial available), name the printer, and verify the webcam feed.

### 3. Connect OrcaSlicer, OctoEverywhere, and OctoPrint
#### a) OrcaSlicer and OctoPrint
- Ensure the client machine and Pi are on the same network (or use a reverse proxy).
- In OrcaSlicer’s Print Host section:
  - **HostType**: `Octo/Klipper`
  - **Hostname**: `http://raspberrypi.local` or `https://<your-pi-address>` (HTTPS needs a valid CA certificate)
  - **API Key**: Main or instance key from OctoPrint
- Save to enable direct slicing and printing.

#### b) OrcaSlicer, OctoEverywhere, and OctoPrint
- In OctoEverywhere, generate a sharing link via the Share Feed/Share Printer option.
- In OrcaSlicer’s Print Host section:
  - **HostType**: `Octo/Klipper`
  - **Hostname, IP, or URL**: Paste the sharing link
- Save, then upload files via the KasmVNC overlay to slice and print. Log into OctoEverywhere with any account if prompted, using the active sharing link.
