**Subject:** GNUF Project – Server Stack and API Architecture Proposal<br>
**Affecting:** GNUF | Database, GNUF | Backend<br>
**Date:** 17-03-2025<br>
**To be discussed:** 20-03-2025<br>

In preparation for the upcoming report and exam, and to streamline the development and deployment process, the following proposals are submitted for consideration. These proposals focus on simplifying the current setup while retaining flexibility for future scalability and reference.<br>

# Introduction<br>

This document outlines the proposed architecture for the GNUF project's server stack and API. It aims to simplify the current setup while retaining flexibility for future scalability and reference.<br>

**IMPORTANT**<br>
Before proceeding, please perform a survey to gather intel on how many users can be expected on the final deployment. This information will help in determining the appropriate server configuration and resource allocation.<br>

# 1. Server Stack<br>
## Migration from K3s to Docker<br>

While K3s remains a preferred deployment option for its scalability and real-world relevance, the limited experience with Kubernetes among the team members has led to the proposal of migrating to a Docker-based setup, utilizing Portainer for GUI-based container management.<br>

This migration aims to simplify configuration and reduce overhead during development and testing.<br>

## Implementation of Nebula VPN<br>

To support secure and flexible access to the server infrastructure from any location, it is proposed to implement a Nebula VPN overlay network. This enables staff and testers to interact with the GNUF platform outside of campus constraints, enhancing development flexibility and user testing.<br>

# 2. API and Database Architecture<br>

There are two proposed options regarding the integration of SQLite and the API, each with its own set of advantages and trade-offs. The goal is to determine the most suitable setup for our current needs while documenting options for future scalability.<br>

**Option A: Maintain Current Setup – SQLite in a Separate Container with Network Access via API**<br>

    Description: SQLite resides in a dedicated container. The backend/API communicates with it over a network interface (port 9012), effectively exposing database operations over TCP/IP.<br>

  Pros:<br>

      Simulates a client-server architecture, useful for learning and demonstration.<br>
      Modular — easy to replace SQLite with a server-based DB in the future.<br>
      Clear separation of concerns between backend and database.<br>

  Cons:<br>
      SQLite is not optimized for network access — potential performance issues.<br>
      Increased complexity with container networking and port management.<br>
      Overhead for managing volumes or data persistence across containers.<br>
      Added complexity with C# pseudo-backend<br>

**Option B: Consolidate – Move SQLite Inside Backend Container (Local Access)**<br>

    Description: API would be be repurposed for frontend communication, endpoint management (database.cs) would be migrated to backend.cs, and SQLite would be moved inside the backend container. This consolidation simplifies the setup and improves performance by leveraging SQLite's local file access optimization.<br>

  Pros:<br>
      Simplified setup — no need for container networking or extra ports.<br>
      Better performance — local file access is what SQLite is optimized for.<br>
      Easier deployment and fewer moving parts.<br>
      Easier argumentation for Backend utilisation<br>

  Cons:<br>
      Tighter coupling — backend and database are no longer modular.<br>
      Requires more effort if we later decide to migrate to a networked DB (e.g., PostgreSQL).<br>
      Slightly less illustrative of scalable or enterprise-like architecture.<br>

**Option C: Postgres approach (only applicable for 30+ users)**<br>
  Pros:<br>
      Scalability — PostgreSQL is designed for high concurrency and large datasets.<br>
      Enterprise-grade features — advanced security, replication, and monitoring tools.<br>
      Better performance — PostgreSQL is optimized for network access.<br>

  Cons:<br>
      SQLite is not optimized for network access — potential performance issues.<br>
      Increased complexity with container networking and port management.<br>
      Overhead for managing volumes or data persistence across containers.<br>
      Added complexity with C# pseudo-backend<br>

  This option would allow for the DB to be scaled along K3S alowing for theoretically infinite users.<br>

**Option D: BEEFY HARDWARE**<br>
  Option D takes it's stands in A or B, along with proposal 1.A, but with a more beefy server than an rpi.4.b.<br>
    Pros:<br>
      Improved performance — a beefier server can handle more requests and data.<br>
      Easier maintenance — due to the simplicity of pure docker, over k3s.<br>

    Cons:<br>
      Higher cost — a beefier server is more expensive than a Raspberry Pi.<br>
