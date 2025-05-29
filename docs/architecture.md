# Architecture — Mirror of Discernment (MoD)

## 1. System Overview
Mirror of Discernment (MoD) is a modular, cloud-native platform for real-time content analysis and gating. It consists of four main components, each containerized and orchestrated via Kubernetes:

1. **Core Analysis API (CAA)**  
   - Performs deep NLP and alignment operations on incoming content.  
   - Exposes a gRPC interface for low-latency calls.

2. **AR Client**  
   - Web/mobile SDK that captures user inputs and forwards them to MoD.  
   - Handles local pre-validation and batching.

3. **Gateway**  
   - Central ingress point for all client traffic.  
   - Implements authentication, rate-limiting, and routing to CAA or Dashboard.

4. **QBSM Dashboard**  
   - Web UI for monitoring pipeline metrics, audit logs, and training data.  
   - Retrieves data via REST from the Gateway.

## 2. High-Level Data Flow

```mermaid
flowchart LR
  subgraph Client Layer
    AR[AR Client SDK]
  end
  subgraph Edge
    GW[Gateway Service]
  end
  subgraph Core
    CAA[Analysis API (CAA)]
    DB[(PostgreSQL)]
  end
  subgraph UI
    QBSM[QBSM Dashboard]
  end

  AR -->|HTTPS| GW
  GW -->|gRPC calls| CAA
  CAA -->|Reads/Writes| DB
  GW -->|REST API| QBSM

