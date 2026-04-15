# LAB INFRA

```
┌──────────────────────────────────────────────────────────────┐
│                   CI/CD Layer                                │
│                                                              │
│  lab-worker  (192.168.1.200 | 4/4)                           │
│  └── Jenkins Controller (Docker, on-demand)                  │
│      ├── Orkestrasi pipeline                                 │
│      ├── Web UI :8080                                        │
│      └── Komunikasi ke agent via SSH / JNLP :50000           │
│                                                              │
│  lab-worker-1 (192.168.1.201 | 2/2)                          │
│  └── Jenkins Agent (ephemeral Docker container)              │
│      ├── npm build / docker build                            │
│      ├── push image ke GHCR                                  │
│      └── kubectl apply ke k3s cluster                        │
└──────────────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│                   Kubernetes Layer                           │
│                                                              │
│  lab2  (192.168.1.102 | 2/4) — k3s Control Plane             │
│  └── API server, scheduler, etcd                             │
│                                                              │
│  lab1  (192.168.1.101 | 1/2) ──┐                             │
│  lab3  (192.168.1.103 | 1/2) ──┼── k3s Worker Nodes          │
│  lab4  (192.168.1.104 | 1/2) ──┘  └── Running Pods           │
│                                       ├── App Next.js #1     │
│                                       ├── App Next.js #2     │
│                                       └── Nginx Ingress LB   │
└──────────────────────────────────────────────────────────────┘
```

| VM | IP | Spec | Role |
|---|---|---|---|
| lab2 | .102 | 2/4 | k3s Control Plane |
| lab1 | .101 | 1/2 | k3s Worker |
| lab3 | .103 | 1/2 | k3s Worker |
| lab4 | .104 | 1/2 | k3s Worker |
| lab-worker | .200 | 4/4 | Jenkins Controller |
| lab-worker-1 | .201 | 2/2 | Jenkins Build Agent |
