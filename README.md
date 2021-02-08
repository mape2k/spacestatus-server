# Spacestatus Server
Flask/Connexion based web application providing a hackspace status website and JSON frontend for [spaceAPI](http://spaceapi.net).

## Known limitations
* Application not usable in multiprocess environment
(gunicorn will work in threading mode)

## Dependencies
### System (Debian-related)
* git
* python3 (>=3.7)
* python3-virtualenv

### Python modules
* see [requirements.txt](requirements.txt)
* wheel

## Installation

You should install this application using a dedicated user.

After your installation spacestatus-server serves on port 5000 on all interfaces. Configure your preferred http proxy (apache, nginx, traefik, ...) to allow access via http/https.

### System requirements on Debian

1. Install system requirements
    ```shell
    sudo apt-get update
    sudo apt-get install python3-virtualenv
    ```

2. Create spacestatus user
    ```shell
    sudo useradd --comment "Spacestatus" --create-home  --user-group spacestatus
    ```

### Spacestatus server

1. Change to spacestatus user
    ```shell
    sudo su - spacestatus
    ```

2. Clone repository
    ```shell
    git clone https://github.com/Bytespeicher/spacestatus-server
    ```
3. Initialize virtual environment
    ```shell
    python3 -m venv virtualenv3
    ```
4. Install python requirements in virtual environment
    ```shell
    . virtualenv3/bin/activate
    pip3 install wheel
    pip3 install -r spacestatus-server/requirements.txt
    pip3 install gunicorn
    deactivate
    ```
5. Copy example configuration files
    ```shell
    cd ~/spacestatus-server
    cp config/config.example.yaml config/config.yaml
    cp config/apidata/status.example.org.json config/apidata/status.your-domain.org.json
    ```

6. Adjust configuration file config/config.yaml

8. Adjust space api json file config/apidata/status.your-domain.org.json

### Install systemd unit

1. Copy systemd unit file
    ```shell
    sudo cp ~/spacestatus-server/contrib/spacestatus-server.service /etc/systemd/system/spacestatus-server.service
    ```

2. Adapt /etc/systemd/system/spacestatus-server.service if service should not listen on all interfaces

3. Reload systemd daemon to reload unit file and start and enable service
    ```shell
    sudo systemctl daemon-reload
    sudo systemctl enable spacestatus-server.service --now
    ```
