# How to Download the Official Celestia Snapshot with Screen

## Introduction
Downloading the official Celestia snapshot can take several hours, especially on remote servers. This process often requires you to maintain an active connection to your remote server throughout the entire download, which can be inconvenient and prone to disconnection. 

A recommended solution is to use `screen`, a terminal multiplexer that allows you to run long-lived processes in the background. This guide will walk you through the steps to download the official Celestia mainnet snapshot using `screen`.

The official snapshot for the Celestia mainnet can be found here: [Celestia Mainnet Snapshot](https://docs.celestia.org/how-to-guides/consensus-node#option-3-quick-sync)

## Steps to Download the Snapshot with Screen

### 1. Install Screen
Ensure that `screen` is installed on your system. If not, you can install it using the following commands:

#### On Ubuntu/Debian:
```bash
sudo apt-get install screen
```

### 2. Create a New Screen Session
Start a new screen session with the following command:
```bash
screen
```

### 3. Download the Snapshot within the Screen Session
Once inside the screen session, run the following commands to download the snapshot:

#### Commands for Celestia Mainnet:
```bash
cd $HOME
rm -rf ~/.celestia-app/data
mkdir -p ~/.celestia-app/data
SNAP_NAME=$(curl -s https://snaps.qubelabs.io/celestia/ | \
    egrep -o ">celestia.*tar" | tr -d ">")
aria2c -x 16 -s 16 -o celestia-snap.tar "https://snaps.qubelabs.io/celestia/${SNAP_NAME}"
tar xf celestia-snap.tar -C ~/.celestia-app/data/
```

### 4. Detach from the Screen Session
To disconnect from the screen session without stopping the download process, press `Ctrl + A` followed by `D`. This will return you to your regular shell, and the download process will continue running in the background.

### 5. Reconnect to the Screen Session
If you want to reconnect to your screen session later, use the following command:
```bash
screen -r
```

### 6. Verify the Snapshot Download
Once the download is complete, verify that the snapshot has been successfully extracted into the `~/.celestia-app/data/` directory.

### Notes
- If you close your SSH session, the screen session will keep running in the background.
- To list all active screen sessions, use:
```bash
screen -ls
```

---
By following these steps, you can efficiently download the Celestia snapshot without worrying about maintaining a continuous connection to your server.
