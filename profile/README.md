# venantvr.pubsub

Ecosysteme complet de messaging Pub/Sub temps reel en Python et Rust. Zero dependance a un broker externe : WebSocket + SQLite.

## Repos

### Systeme Pub/Sub

| Repo | Stack | Description |
|------|-------|-------------|
| `Python.Publisher.Subscriber` | Python | Systeme pub/sub monolithique originel (Flask + Socket.IO + SQLite) |
| `Python.PubSub.Server` | Python | Serveur pub/sub standalone : topics, broadcast, persistence SQLite, monitoring |
| `Python.PubSub.Client` | Python | Client pub/sub (PyPI: `python-pubsub-client`), reconnexion auto, queuing |
| `Python.PubSub.Example` | Python | Demo complete multi-topics (orders, inventory, shipping) |

### Librairies de support

| Repo | Stack | Description |
|------|-------|-------------|
| `Python.SQLite.Async` | Python | Wrapper SQLite non-bloquant thread-safe (PyPI: `python-sqlite-async`) |
| `Python.JSONL.Async` | Python | Writer JSONL non-bloquant thread-safe (PyPI: `python-jsonl-queue`) |
| `Python.Threadsafe.Logger` | Python | Logger business events (SQLite/TinyDB), non-bloquant |
| `Python.PubSub.DevTools.Consumers` | Python | Proxies record/replay pour DevTools |
| `Python.PubSub.Templator` | Python | Generateur de squelettes de projets consommateurs |

### Ports Rust (haute performance)

| Repo | Stack | Description |
|------|-------|-------------|
| `Rust-PubSub-Server` | Rust | Port du serveur (Axum + Tokio), binaire unique, purge auto |
| `Rust-PubSub-Client` | Rust | Port du client (tokio + rust_socketio + dashmap), benchmarks Criterion |
| `Rust-SQLite-Async` | Rust | Port du wrapper SQLite async (r2d2 pool, back-pressure) |
| `Rust-JSONL-Async` | Rust | Port du writer JSONL async (batch, generique serde::Serialize) |

## Architecture

```
PubSub.Templator -----> Genere des projets consommateurs
                              |
PubSub.Client (Py/Rust) <--> PubSub.Server (Py/Rust) <--- DevTools.Consumers
                              |
                    +---------+---------+
                    |                   |
              SQLite.Async        JSONL.Async
              (Py / Rust)         (Py / Rust)
                    |
              Threadsafe.Logger
```

## Principes

- **Dual-language** : chaque composant existe en Python et en Rust
- **Async-first** : tous les backends de stockage utilisent des threads d'ecriture dedies
- **Thread-safe** : concu pour les environnements concurrents
- **Self-contained** : aucun broker externe requis
