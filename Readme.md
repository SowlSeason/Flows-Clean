Netflow Log Monitor

Centralized Network Security Monitoring System

Netflow Log Monitor is a comprehensive, centralized system designed for capturing, analyzing, and visualizing network traffic data in real-time. It enables organizations to monitor network behavior, detect suspicious activities (Critical Ports, Unapproved Domains), and manage logs efficiently using a scalable architecture.

Key Features

Sniffer Agent (Client)

- Packet Sniffing: Real-time network packet capturing using Scapy.

- Security: Encrypts data using Fernet before transmission.

- Threat Detection.

- Detects critical port usage (SSH, RDP, Telnet).

- Identifies access to suspicious IPs or unapproved domains.

- Resilience: Buffers logs locally and sends them as encrypted ZIP files hourly.

- Easy Installation: Automated setup via install.bat (installs Npcap, configures startup task).

Backend Infrastructure (Server)

- High Performance: Powered by FastAPI running in 3 replicas for load distribution.

- Scalable Architecture: Uses Nginx as a Load Balancer and Reverse Proxy.

- No-Database Bottleneck: Logs are stored in structured .jsonl files (partitioned by date/branch) instead of a traditional relational database, ensuring high write throughput.

- Archive Manager: Background service for auto-archiving old logs (ZIP) and cleanup.

- User Management: SQLite database for handling Authentication (OAuth2 + JWT) and RBAC.

Web Dashboard (Frontend)

- Modern UI: Built with React (Vite) and Ant Design.

- Real-time Monitoring: Dashboard for viewing critical events within the last 7 days.

- Log Search: Advanced filtering by Branch, Date, IP, and Domain.

- Archive Access: Download historical log files directly from the UI.

System Architecture

The system follows a microservices-like architecture deployed via Docker Compose:

     Client [Client Side]
        [Sniffer Agent] -->|Encrypted Traffic| (Nginx Load Balancer)
    

     Server [Server Infrastructure]
         --> {FastAPI Replicas}
         -->|Write| [JSONL Log Files]
         -->|Read/Write| [(SQLite - Auth Only)]
        [Archive Manager] -->|Compress/Cleanup|


     Frontend [Web Dashboard]
        [React App] -->|HTTP Requests|


Technology Stack

Sniffer Agent

 - Python 3, Scapy, Cryptography, Windows Task Scheduler

Backend

 - Python 3 (FastAPI), Uvicorn, Pydantic

Frontend

 - React, Vite, Ant Design, Axios

Infrastructure

 - Docker, Docker Compose, Nginx

Storage

 - File System (.jsonl), SQLite (Users)

Getting Started

Prerequisites

- Server: Docker & Docker Compose

- Client: Windows 10/11 (64-bit), Administrator privileges

1. Server Deployment (Backend & Frontend)

Clone the repository:

Configure Environment Variables:
Create a .env file in the root directory (or Backend/ depending on structure):

SECRET_KEY=your_very_strong_secret_key_here
ACCESS_TOKEN_EXPIRE_MINUTES=1440  # 24 Hours


Run with Docker Compose:

docker-compose up -d --build


The Web Dashboard will be available at: http://localhost:8080 (or your server IP).

The API documentation will be available at: http://localhost:8080/docs.

2. Sniffer Agent Installation (Client)

Prepare the Installer:

Navigate to the Sniffer/ directory.

Ensure installer.exe, install.bat, and the config folder are present.

Update config.json or .env in the client package to point to your Server IP:

{
  "server_url": "http://<YOUR_SERVER_IP>:8080",
  "secret_key": "your_very_strong_secret_key_here"
}


Install on Client Machine:

Copy the installer folder to the client machine.

Right-click install.bat and select Run as administrator.

The script will:

Install Npcap (if missing).

Create necessary directories in C:\ProgramData\NetflowMonitor.

Set up a scheduled task to run the agent on startup.

Project Structure

Netflow-Log-Monitor/
├── Backend/
│   ├── app/                # FastAPI Application code
│   ├── logs/               # Storage for .jsonl files
│   ├── Dockerfile          # Multi-stage build for Backend
│   └── requirements.txt
├── Frontend/
│   ├── src/                # React Source code
│   ├── Dockerfile          # Build script for Nginx serving React
│   └── package.json
├── Sniffer/
│   ├── netflow_sniffer.py  # Main Agent Logic
│   ├── install.bat         # Installation Script
│   └── requirements.txt
├── docker-compose.yml      # Orchestration config
├── nginx.conf              # Load Balancer & Proxy config
└── README.md


Troubleshooting

CORS Issues: If the browser blocks requests, ensure nginx.conf is correctly handling Access-Control-Allow-Origin.

Agent Not Sending Data: Check C:\ProgramData\NetflowMonitor\logs\agent.log for errors. Verify the SECRET_KEY matches the server.

Docker Containers Exiting: Check memory usage. The server requires at least 4GB RAM recommended for running 3 API replicas.

License

This project is licensed under the MIT License - see the LICENSE file for details.