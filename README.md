# Q3 Inference Service

## 🚀 Quick Start (NVIDIA GPU)

```bash
docker run -d --gpus all -p 8001:8001 sophiacloud/inf-q3-service:cuda
```

## 🚀 Quick Start (AMD GPU)

```bash
docker run --device=/dev/kfd --device=/dev/dri --group-add video sophiacloud/inf-q3-service:rocm
```

## 🚀 Quick Start (CPU)

```bash
docker run -d -p 8001:8001 sophiacloud/inf-q3-service:cpu
```

> 💡 **CPU build generica** — per +10-25% performance, scaricare il tag ottimizzato per la propria CPU se disponibile.

---

## 🖥️ Piattaforme supportate

| Piattaforma | Tag | Stato | Note |
|-------------|-----|-------|------|
| NVIDIA GPU (CUDA) | `:cuda` | ✅ Stabile | Build generica x86-64-v3, ~99% performance |
| CPU x86_64 | `:cpu` | 💡 Generic | Preferire build dedicata |
| CPU ARM64 | `:cpu` | 💡 Generic | Preferire build dedicata |
| Jetson | — | 🔜 Coming soon | |
| AMD ROCm | — | 🟢 Beta | Build generica x86-64-v3, ~99% performance |
| Apple Silicon | — | 🔜 Coming soon | |

---

## 📦 Zero Config

Il servizio **non richiede configurazione**:

- **Auto-download**: al primo avvio scarica automaticamente il modello da `Sophia-AI/Qwen3-4B-Instruct-2507-GGUF`
- **Auto-tuning**: rileva automaticamente CPU/GPU e applica le ottimizzazioni migliori
- **Pronto all'uso**: basta runnare il container

---

## 🎮 Requisiti CUDA

Per usare l'immagine `:cuda` servono:

| Requisito | Versione |
|-----------|----------|
| GPU NVIDIA | Compute Capability 5.0+ (GTX 750 o superiore) |
| Driver NVIDIA | ≥ 550.x |
| NVIDIA Container Toolkit | Installato |
| Docker | 19.03+ |
| CPU | x86_64 con AVX2 (qualsiasi CPU dal 2015) |

---

## 💢 Requisiti ROCm

Per usare l'immagine `:rocm` servono:

| Requisito | Versione |
|-----------|----------|
| GPU AMD | RX 6000/7000 series (RDNA2/RDNA3) |
| Sistema operativo | Linux nativo (WSL2 non supportato) |
| Driver AMD | amdgpu (incluso nel kernel Linux) |
| ROCm | ≥ 6.0 (opzionale, i runtime sono nel container) |
| Docker | 19.03+ |
| CPU | x86_64 con AVX2 (qualsiasi CPU dal 2015) |

### GPU supportate

| Serie | Modelli | Target |
|-------|---------|--------|
| RX 7000 (RDNA3) | 7900 XTX, 7900 XT, 7900 GRE, 7800 XT, 7700 XT, 7600 | ✅ |
| RX 6000 (RDNA2) | 6950 XT, 6900 XT, 6800 XT, 6800, 6700 XT, 6600 XT, 6600 | ✅ |
| RX 5000 (RDNA1) | 5700 XT, 5700, 5600 XT | ❌ Non supportate |
| Vega / Polaris | Vega 64, RX 580, ecc. | ❌ Non supportate |

> ⚠️ **Nota:** ROCm non funziona su WSL2. È richiesto Linux nativo (Ubuntu, Fedora, ecc.)

---

## 📊 Modelli disponibili

| Quantizzazione | Size | Note |
|----------------|------|------|
| Q2_K | 1.67 GB | Minimo, qualità ridotta |
| Q3_K_M | 2.08 GB | Leggero |
| **Q4_K_M** | 2.5 GB | **Default** — bilanciato |
| Q5_K_M | 2.89 GB | Buona qualità |
| Q6_K | 3.31 GB | Alta qualità |
| Q8_0 | 4.28 GB | Quasi lossless |
| F16 | 8.05 GB | Full precision |

---

## ⚙️ Override opzionali

Il servizio funziona senza configurazione, ma puoi personalizzare:

| Variabile | Default | Descrizione |
|-----------|---------|-------------|
| `Q3_PORT` | `8001` | Porta server |
| `Q3_QUANT` | `Q4_K_M` | Quantizzazione modello |
| `Q3_N_CTX` | `auto` | MAX_CONTEXT_TOKEN |
| `Q3_HF_REPO_ID` | Sophia-AI/Qwen3-4B-Instruct-2507-GGUF | Repo Hf |

Esempio con quant diverso:

```bash
docker run -d --gpus all -p 8001:8001 \
  -e Q3_N_CTX=8192 \
  sophiacloud/inf-q3-service:cuda
```

---

## 🔌 API Endpoints

| Endpoint | Metodo | Descrizione |
|----------|--------|-------------|
| `/v1/chat/completions` | POST | Chat completions (OpenAI-compatible) |
| `/v1/models` | GET | Lista modelli |
| `/health` | GET | Stato + device |

---

## 🧠 Caratteristiche

- OpenAI-compatible API
- Streaming token-by-token
- Tool / Function calling
- JSON mode / JSON schema
- Auto-download modello
- Auto-detect CPU / GPU
- Zero configurazione richiesta

---

## 📄 License

Il modello "Qwen3-4B-Instruct-2507-GGUF" è una versione convertita di un modello base licenziato sotto Apache License 2.0.

Il codice del servizio è licenziato sotto PolyForm Noncommercial License 1.0.0.
