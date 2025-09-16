Distributed Multi-Client File Orchestration and Synchronization
===============================================================

Overview
--------

This is a simple Python-based distributed file orchestration and synchronization system that allows multiple clients to securely connect to a server over TLS/SSL and perform file system operations within their own isolated server-side directories.

Features
--------

- Secure client-server communication using TLS (self-signed certificates provided).
- Per-user isolated storage under `./server_storage/<username>`.
- Simple command set: ls, cat, get, put, rm, mkdir, rmdir, cd, exit.
- Network discovery to find available servers on the local network.

Requirements
------------

The project is written for Python 3.8+ and depends on a couple of third-party packages which are listed in `requirements.txt`:

- netifaces
- ipaddress

Install requirements with:

```sh
python3 -m pip install -r requirements.txt
```

Files
-----

- `server.py` - Server entry point. Starts the server and handles incoming client connections.
- `client.py` - Client entry point. Scans the local network, connects to a server, and accepts user commands.
- `net_utils.py` - Networking layers: `Server` and `Client` classes. Handles sockets, threading, and file transfer sockets.
- `fs_utils.py` - `FS` class implementing safe file system operations in each user's root directory.
- `handshake_utils.py` - Simple handshake protocol helpers and credential verification.
- `ssl_utils.py` - SSL wrapper helpers for client/server sockets. Expects `cert.pem` and `key.pem`.
- `ip_utils.py` - Helpers to list local IP addresses, present a selection UI, and compute the local IP range for discovery.
- `colors.py` - ANSI color constants used for terminal output.
- `user_credentials.txt` - Username:password pairs used for authentication (plain text in this repo).
- `ssl_certificates.sh` - Helper script to create `cert.pem` and `key.pem` if not present.

Quick start
-----------

1. (Optional) Generate self-signed certs if you don't have them:

```sh
bash ssl_certificates.sh
```

2. Install dependencies:

```sh
python3 -m pip install -r requirements.txt
```

3. Start the server (on the host):

```sh
python3 server.py
```

4. Start the client (on the same or another machine on the LAN):

```sh
python3 client.py
```

5. Follow prompts to connect, authenticate, and run commands.

Security notes
--------------

- The project uses TLS for encryption, but the provided certificates are self-signed. For production use, use certificates issued by a trusted CA.
- Credentials are stored in plain text in `user_credentials.txt`. Replace with a secure storage mechanism (hashed passwords, salted, etc.) for any real deployment.

Limitations and TODOs
--------------------

- Improve authentication (hash passwords, use a proper user store).
- Add client-side and server-side logging improvements and better error handling for network failures.
- Add retry/backoff for file transfers.


