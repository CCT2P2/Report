**Subject:** GNUF Project – Server Stack and API Architecture Proposal  
**Affecting:** GNUF | Database, GNUF | Backend  
**Date:** 17-03-2025  
**To be discussed:** 20-03-2025  

In preparation for the upcoming report and exam, and to streamline the development and deployment process, the following proposals are submitted for consideration. These proposals focus on simplifying the current setup while retaining flexibility for future scalability and reference.  

# 1. Server Stack 
## Migration from K3s to Docker

While K3s remains a preferred deployment option for its scalability and real-world relevance, the limited experience with Kubernetes among the team members has led to the proposal of migrating to a Docker-based setup, utilizing Portainer for GUI-based container management.  

This migration aims to simplify configuration and reduce overhead during development and testing.  

## Implementation of Nebula VPN

To support secure and flexible access to the server infrastructure from any location, it is proposed to implement a Nebula VPN overlay network. This enables staff and testers to interact with the GNUF platform outside of campus constraints, enhancing development flexibility and user testing.  

# 2. API and Database Architecture

There are two proposed options regarding the integration of SQLite and the API, each with its own set of advantages and trade-offs. The goal is to determine the most suitable setup for our current needs while documenting options for future scalability.  
Option A: Maintain Current Setup – SQLite in a Separate Container with Network Access via API  

    Description: SQLite resides in a dedicated container. The backend/API communicates with it over a network interface (port 9012), effectively exposing database operations over TCP/IP.  

  Pros:  

      Simulates a client-server architecture, useful for learning and demonstration.  
      Modular — easy to replace SQLite with a server-based DB in the future.  
      Clear separation of concerns between backend and database.  

  Cons:  
      SQLite is not optimized for network access — potential performance issues.  
      Increased complexity with container networking and port management.  
      Overhead for managing volumes or data persistence across containers.  
      Added complexity with C# pseudo-backend  

Option B: Consolidate – Move SQLite Inside Backend Container (Local Access)  

    Description: API would be be repurposed for frontend communication, endpoint management (database.cs) would be migrated to backend.cs, and SQLite would be moved inside the backend container. This consolidation simplifies the setup and improves performance by leveraging SQLite's local file access optimization.  

  Pros:  
      Simplified setup — no need for container networking or extra ports.  
      Better performance — local file access is what SQLite is optimized for.  
      Easier deployment and fewer moving parts.  
      Easier argumentation for Backend utilisation  

  Cons:  
      Tighter coupling — backend and database are no longer modular.  
      Requires more effort if we later decide to migrate to a networked DB (e.g., PostgreSQL).  
      Slightly less illustrative of scalable or enterprise-like architecture.  

Summary  

No final decision is being made at this stage — these proposals are intended for discussion during the Thursday meeting. The objective is to align on a strategy that balances simplicity, educational value, and future flexibility.  
