Tech Stack: Python (Scapy / Raw Sockets), Scapy

Description: A standalone service or background process responsible for capturing raw network traffic in real-time. It filters relevant packets and extracts metadata before sending the structured data to the Backend for processing.

Key Features:

    Packet Inspection: Captures headers (IP, TCP/UDP) and payloads.

    Traffic Filtering: Ignores noise to focus on critical network events.

    Data Forwarding: Pushes captured event logs to the Backend API or database.