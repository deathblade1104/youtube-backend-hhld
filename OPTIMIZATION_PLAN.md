
# 🎯 YouTube.AI — Engineering Optimization Roadmap (v2.0)

## 🚀 Overview

This roadmap outlines the next evolution of **YouTube.AI**, focusing on scalability, observability, and advanced GenAI integrations.
The foundation is strong — NestJS orchestrates workflow, Python handles AI-heavy workloads, and Kafka enables event-driven pipelines.
Next: improve efficiency, reliability, and intelligence across the stack.

---

## 🏗️ System Architecture Highlights

- Event-driven architecture using Kafka with Outbox & ProcessedMessage → ✅
- Dual-service design: NestJS (orchestration) + Python (AI microservice) → ✅
- Scalable storage: Postgres + MinIO + Redis + Elasticsearch → ✅
- FFmpeg-based transcoding + Whisper transcription + OpenAI summarization → ✅

---

## ⚙️ Backend Optimization Plan

### NestJS Backend

| Area | Optimization | Outcome |
|------|---------------|----------|
| FFmpeg concurrency | Move CPU-heavy jobs to BullMQ with concurrency limits | Prevents resource exhaustion |
| Event boilerplate | Auto-register processors using metadata scanning | Cleaner code & faster dev |
| Unified event emitter | Abstract Kafka/BullMQ event handling | Consistent cross-service pattern |
| Observability | Add OpenTelemetry tracing | End-to-end visibility |
| Testing | Add Jest + integration tests | CI/CD reliability |

---

## 🧠 Python AI Backend Optimization Plan

### Transcription (Whisper)

| Area | Optimization | Outcome |
|------|---------------|----------|
| Chunking | Split long audio via ffmpeg (-ss/-t) | Prevents OOM, faster parallel ASR |
| Model scaling | Use smaller model for short videos | Saves compute |
| Lazy loading | Reuse Whisper model per worker | Lowers reload latency |
| Parallelism | Multiple Celery workers or threads | Scales horizontally |
| Monitoring | Log duration & WER metrics | ASR quality tracking |

### Summarization (OpenAI)

| Area | Optimization | Outcome |
|------|---------------|----------|
| Token-aware chunking | Use `tiktoken` instead of char splits | Accurate cost & better summaries |
| Chunk caching | Cache map results in Redis | Saves tokens & retries |
| Parallel map | Use Celery group/chord pattern | Reduces total latency |
| Cost tracking | Log prompt/completion tokens & USD | Enables cost dashboards |
| Circuit breaker | Fallback to Ollama on outage | Resilient AI ops |
| Prompt registry | Store prompts in /prompts JSON | Easier iteration |

---

## 📊 Observability & Monitoring

### Add Metrics

| Component | Metric | Tool |
|------------|--------|------|
| Celery | Task latency, retry count | Prometheus + Grafana |
| Kafka | Consumer lag | Burrow / Conduktor |
| OpenAI | Token usage, cost per run | Custom collector |
| System | CPU, RAM, GPU | Node exporter |

### Logging Standards

- Add correlation IDs across Nest, Python, Kafka.
- Centralize logs using Loki or Elastic APM.
- Add DLQ tracking for failed Celery jobs.

---

## 🧩 Schema Enhancements

- Add `job_duration_ms` fields per stage (upload, transcode, transcribe, summarize, index).
- Add `model_info` JSONB for LLM version, latency, and cost.
- Create `video_embeddings` table for RAG search.
- Store chunk timestamps in transcript for UX linking.

---

## ☁️ Infra & Deployment Improvements

| Area | Change | Outcome |
|------|---------|----------|
| Container split | Separate ASR & GenAI workers | Optimized resource allocation |
| GPU support | Whisper.cpp CUDA container | 3× faster transcription |
| CI/CD | GitHub Actions build/test/push | Automated deployments |
| Config mgmt | Centralized `.env` per env | Simplified dev/prod ops |
| Health checks | FastAPI + Celery readiness probes | Stability assurance |

---

## 🧮 Advanced AI Roadmap

| Feature | Description | Stack |
|----------|--------------|--------|
| **Captions/Subtitles** | Auto-generate `.vtt` files from Whisper timestamps | Python |
| **Chapters/Highlights** | Segment via LLM summaries + timestamps | Python + OpenAI |
| **Topic Tagging** | GPT-based auto-tagging | Python + OpenAI |
| **Embeddings Search** | Semantic chunk retrieval | Elastic + OpenAI embeddings |
| **Video Q&A** | RAG-based chat over transcript | Python + LangChain |
| **Adaptive Summarization** | Prompt tuning by category | Python |

---

## 🔭 Long-Term Technical Goals

1. Add streaming summarization (parallel pipeline)
2. Implement ML lineage tracking (prompt/model versioning)
3. Add dynamic DAG execution (Kafka-based Airflow-lite)
4. Build internal cost intelligence dashboard
5. Implement self-healing Celery retry + DLQ processors

---

## 🧩 Development Workflow Enhancements

- Add unit/integration tests (pytest, Jest)
- Add replay CLI for reprocessing failed events
- Improve local dev via Docker Compose orchestration
- Add Swagger + event-doc generation for APIs
- Create `docs/` folder with architecture diagrams

---

## 🏁 Priorities (Q1 Roadmap)

| Priority | Task | Owner | ETA |
|-----------|-------|--------|------|
| P0 | Token-aware chunker + caching | Python | 1 week |
| P0 | FFmpeg queue concurrency cap | NestJS | 3 days |
| P1 | OpenAI cost tracking + metrics | Python | 1 week |
| P1 | DLQ + retry observability | Python | 1 week |
| P2 | Embeddings + semantic index | NestJS | 2 weeks |
| P2 | GPU-ready Whisper image | DevOps | 2 weeks |
| P3 | Prompt registry + A/B infra | Python | 3 weeks |

---

## 🧠 Summary

YouTube.AI is production-grade and already aligns with SDE-3 system design quality.
Next phase is about **operational excellence**, **AI cost optimization**, and **scalable parallelization**.
By implementing this roadmap, the system will become:
✅ Faster → parallelized map–reduce pipelines
✅ Cheaper → token caching + adaptive models
✅ Safer → DLQ + circuit breakers
✅ Observable → metrics + tracing

---

**Author:** Shahbaz Hasan Raja
**Reviewed by:** ChatGPT (GPT-5)
**Last Updated:** 2025-11-02
