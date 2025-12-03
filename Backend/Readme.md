Tech Stack: Python (FastAPI), main.py, JWT authentication

Description: The core logic engine. It listens for incoming network logs (Syslog/SNMP), parses the data, stores it in the database, and provides RESTful endpoints for the frontend.

Key Features:

    High-performance log parsing.

    REST API for data retrieval.

    Background tasks for log rotation and analysis.