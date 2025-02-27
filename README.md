# FreeSWITCH Installation Guide



## **Introduction**

Installing FreeSWITCH on **Ubuntu 20.04 LTS**.

---

## **Table of Contents**

- [Prerequisites](#prerequisites)
- [Installing Required Libraries](#installing-required-libraries)
- [Installing FreeSWITCH](#installing-freeswitch)
- [Post-Installation Setup](#post-installation-setup)
- [Running FreeSWITCH as a Service](#running-freeswitch-as-a-service)
- [Using FreeSWITCH CLI](#using-freeswitch-cli)
- [Conclusion](#conclusion)

---

## **Prerequisites**

Ensure your **Ubuntu 20.04 LTS** system has all required dependencies installed:

### **For Ubuntu 20.04 LTS**

```sh
sudo apt install --yes build-essential pkg-config uuid-dev zlib1g-dev libjpeg-dev libsqlite3-dev libcurl4-openssl-dev \
    libpcre3-dev libspeexdsp-dev libldns-dev libedit-dev libtiff5-dev yasm libopus-dev libsndfile1-dev unzip \
    libavformat-dev libswscale-dev libavresample-dev liblua5.2-dev liblua5.2-0 cmake libpq-dev \
    unixodbc-dev autoconf automake ntpdate libxml2-dev libpq-dev libpq5 sngrep
```

---

## **Installing Required Libraries**

### **Install **``

```sh
sudo git clone https://github.com/signalwire/libks.git /usr/local/src/libks
cd /usr/local/src/libks
sudo cmake .
sudo make && sudo make install
```

Verify installation:

```sh
sudo ldconfig -p | grep libks
```

### **Install **``

```sh
sudo git clone https://github.com/signalwire/signalwire-c.git /usr/local/src/signalwire-c
cd /usr/local/src/signalwire-c
sudo cmake .
sudo make && sudo make install
```

Verify installation:

```sh
sudo ldconfig -p | grep signalwire
```

---

## **Installing FreeSWITCH**

```sh
sudo wget -c https://files.freeswitch.org/releases/freeswitch/freeswitch-1.10.7.-release.tar.gz -P /usr/local/src
cd /usr/local/src
sudo tar -zxvf freeswitch-1.10.7.-release.tar.gz
cd freeswitch-1.10.7.-release
sudo ./configure --enable-core-odbc-support --enable-core-pgsql-support
sudo make
sudo make install
```

Install sound files:

```sh
sudo make cd-sounds-install
sudo make cd-moh-install
```

Verify installation:

```sh
sudo freeswitch -nonat
```

---

## **Running FreeSWITCH as a Service**

Create a systemd service file:

```sh
sudo nano /etc/systemd/system/freeswitch.service
```

Paste the following:

```ini
[Unit]
Description=FreeSWITCH open source softswitch
After=network.target network-online.target

[Service]
Type=forking
PIDFile=/usr/local/freeswitch/run/freeswitch.pid
ExecStart=/usr/local/freeswitch/bin/freeswitch -u freeswitch -g freeswitch -ncwait
Restart=always

[Install]
WantedBy=multi-user.target
```

Reload and start FreeSWITCH:

```sh
sudo systemctl daemon-reload
sudo systemctl start freeswitch
sudo systemctl enable freeswitch
```

Check status:

```sh
sudo systemctl status freeswitch
```

---

## **Using FreeSWITCH CLI**

```sh
fs_cli
```

To check system status:

```sh
status
```

---
