# venantvr.pubsub

Écosystème complet de messaging Pub/Sub temps réel en Python et Rust. Zéro dépendance à un broker externe : WebSocket + SQLite uniquement.

## Repos

### Système Pub/Sub

[![Python.Publisher.Subscriber](https://img.shields.io/badge/Python.Publisher.Subscriber-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://github.com/venantvr-pubsub/Python.Publisher.Subscriber) <sup>public</sup>
*Python / Flask / Socket.IO* — Système pub/sub monolithique originel : Flask + Socket.IO + eventlet, persistance SQLite, base du protocole WebSocket temps réel.

[![Python.PubSub.Server](https://img.shields.io/badge/Python.PubSub.Server-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://github.com/venantvr-pubsub/Python.PubSub.Server) <sup>public</sup>
*Python / Flask* — Serveur pub/sub standalone extrait du monolithe : gestion de topics, broadcast, persistance SQLite via thread dédié, monitoring intégré, hot-reload de configuration.

[![Python.PubSub.Client](https://img.shields.io/badge/Python.PubSub.Client-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://github.com/venantvr-pubsub/Python.PubSub.Client) <sup>public</sup>
*Python* — Client pub/sub publié sur PyPI (`python-pubsub-client`) : reconnexion automatique infinie, queuing de messages, validation Pydantic, handlers typés, dashboard de monitoring HTML intégré.

[![Python.PubSub.Example](https://img.shields.io/badge/Python.PubSub.Example-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://github.com/venantvr-pubsub/Python.PubSub.Example) <sup>public</sup>
*Python* — Démo complète multi-topics (orders, inventory, shipping) illustrant l'utilisation du client comme dépendance Git.

### Librairies de support

[![Python.SQLite.Async](https://img.shields.io/badge/Python.SQLite.Async-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://github.com/venantvr-pubsub/Python.SQLite.Async) <sup>public</sup>
*Python* — Wrapper SQLite non-bloquant thread-safe publié sur PyPI (`python-sqlite-async`) : écritures dans un thread dédié, zéro dépendance externe.

[![Python.JSONL.Async](https://img.shields.io/badge/Python.JSONL.Async-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://github.com/venantvr-pubsub/Python.JSONL.Async) <sup>public</sup>
*Python* — Writer JSONL non-bloquant thread-safe publié sur PyPI (`python-jsonl-queue`) : écriture asynchrone dans des fichiers `.jsonl`, zéro dépendance.

[![Python.Threadsafe.Logger](https://img.shields.io/badge/Python.Threadsafe.Logger-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://github.com/venantvr-pubsub/Python.Threadsafe.Logger) <sup>public</sup>
*Python* — Logger métier thread-safe multi-backends (SQLite via `python-sqlite-async` + JSONL via `python-jsonl-queue` + TinyDB), non-bloquant.

[![Python.PubSub.DevTools.Consumers](https://img.shields.io/badge/Python.PubSub.DevTools.Consumers-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://github.com/venantvr-pubsub/Python.PubSub.DevTools.Consumers) <sup>public</sup>
*Python* — Proxies génériques record/replay pour DevTools : interception transparente des événements, rejeu configurable.

[![Python.PubSub.Templator](https://img.shields.io/badge/Python.PubSub.Templator-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://github.com/venantvr-pubsub/Python.PubSub.Templator) <sup>public</sup>
*Python* — Générateur de squelettes de projets consommateurs : templates YAML-driven (Makefile, Docker Compose, agents, events, tests), scaffolding interactif.

### Ports Rust (haute performance)

[![Rust-PubSub-Server](https://img.shields.io/badge/Rust--PubSub--Server-B7410E?style=for-the-badge&logo=rust&logoColor=white)](https://github.com/venantvr-pubsub/Rust-PubSub-Server) <sup>public</sup>
*Rust / Axum / Tokio* — Port du serveur : Axum + socketioxide + Tokio, binaire unique optimisé (LTO, strip), purge automatique, feature flags (parallel/sequential emit).

[![Rust-PubSub-Client](https://img.shields.io/badge/Rust--PubSub--Client-B7410E?style=for-the-badge&logo=rust&logoColor=white)](https://github.com/venantvr-pubsub/Rust-PubSub-Client) <sup>public</sup>
*Rust / Tokio* — Port du client : tokio + rust_socketio + dashmap (lock-free), benchmarks Criterion, reconnexion automatique.

[![Rust-SQLite-Async](https://img.shields.io/badge/Rust--SQLite--Async-B7410E?style=for-the-badge&logo=rust&logoColor=white)](https://github.com/venantvr-pubsub/Rust-SQLite-Async) <sup>public</sup>
*Rust* — Port du wrapper SQLite : pool r2d2 + rusqlite, canal crossbeam borné pour contre-pression (back-pressure), thread d'écriture dédié.

[![Rust-JSONL-Async](https://img.shields.io/badge/Rust--JSONL--Async-B7410E?style=for-the-badge&logo=rust&logoColor=white)](https://github.com/venantvr-pubsub/Rust-JSONL-Async) <sup>public</sup>
*Rust* — Port du writer JSONL : batch générique `serde::Serialize`, SmallVec pour éviter les allocations heap, canal crossbeam borné.

## Architecture

```mermaid
graph TD
    T[PubSub.Templator<br/><i>Génère des projets consommateurs</i>] --> S

    C[PubSub.Client<br/><i>Py / Rust · PyPI · Pydantic</i>] <--> S[PubSub.Server<br/><i>Py / Rust · WebSocket · Topics</i>]
    D[DevTools.Consumers<br/><i>Record / Replay</i>] --> S

    S --> SQ[SQLite.Async<br/><i>Py / Rust</i>]
    S --> JL[JSONL.Async<br/><i>Py / Rust</i>]

    SQ --> L[Threadsafe.Logger<br/><i>Multi-backends</i>]
    JL --> L
```

## Principes

- **Dual-language** : chaque composant existe en Python et en Rust avec la même API
- **Async-first** : tous les backends de stockage utilisent des threads d'écriture dédiés
- **Thread-safe** : conçu pour les environnements concurrents (dashmap, crossbeam, locks)
- **Self-contained** : aucun broker externe requis (ni RabbitMQ, ni Kafka, ni Redis)
- **PyPI-ready** : client, SQLite.Async et JSONL.Async publiés comme packages installables
