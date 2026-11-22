# 🚀 QFlow-AI — Autonomous Quantum Computing Scheduler

> **"AI-powered autonomous scheduler for quantum workloads with intelligent qubit allocation and error estimation."**

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg?style=for-the-badge&logo=python)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.100+-green.svg?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com)
[![React](https://img.shields.io/badge/React-18+-61DAFB.svg?style=for-the-badge&logo=react)](https://react.dev)
[![Gemini](https://img.shields.io/badge/Google-Gemini_AI-orange.svg?style=for-the-badge&logo=google)](https://ai.google.dev)
[![SQLite](https://img.shields.io/badge/SQLite-3-003B57.svg?style=for-the-badge&logo=sqlite)](https://sqlite.org)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

---

## 📖 Table of Contents
1. [Introduction](#-introduction)
2. [The Problem It Solves](#-the-problem-it-solves)
3. [Key Features](#-key-features)
4. [System Architecture](#-system-architecture)
5. [Tech Stack](#-tech-stack)
6. [Database Schema](#-database-schema)
7. [AI Agents & Algorithms](#-ai-agents--algorithms)
8. [Project Folder Structure](#-project-folder-structure)
9. [Installation & Setup Guide](#-installation--setup-guide)
10. [API Endpoints](#-api-endpoints)
11. [Screenshots](#-screenshots)
12. [Future Scope](#-future-scope)
13. [Author](#-author)

---

## 🧠 Introduction

**QFlow-AI** is a full-stack, multi-agent AI system that intelligently schedules quantum computing workloads across simulated quantum hardware. As quantum computers remain scarce, noisy, and expensive, manual scheduling of quantum jobs often leads to poor hardware utilization, high error rates, and inefficient prioritization. 

QFlow-AI acts as an **"Air Traffic Control for Quantum Jobs"**. It combines **Google Gemini AI** with deterministic fallback algorithms to create a robust hybrid architecture. It autonomously decides *which quantum job runs when*, *which qubits are assigned to each job*, and *what the expected error probability is* — all visualized in a modern, real-time React dashboard.

---

## 🎯 The Problem It Solves

Today, quantum jobs are often scheduled manually or with naive FIFO (First-In-First-Out) queues. This leads to:
- ❌ **Low hardware utilization:** Expensive qubits sit idle while jobs wait in queue.
- ❌ **High error rates:** Jobs run on poor-quality qubits, causing circuit failures.
- ❌ **Poor prioritization:** Critical research jobs wait behind trivial test jobs.
- ❌ **No error awareness:** Users don't know the failure probability until the job crashes.

**QFlow-AI solves this** by introducing an AI agent that continuously optimizes scheduling decisions, similar to how an expert quantum engineer would — but autonomously and in milliseconds.

---

## ✨ Key Features

### 🤖 Autonomous AI Agents (Powered by Gemini)
- **Scheduler Agent:** Determines execution order based on priority, qubit requirements, and hardware state.
- **Allocation Agent:** Picks optimal qubits using coherence time + gate error data.
- **Error Agent:** Predicts total execution error using a physics-inspired model.
- **Orchestrator:** Coordinates all agents + provides **fallback to classical algorithms** if AI fails.

### ⚛️ Quantum-Aware Scheduling
- Priority queue with weighted scoring (urgency × complexity × error tolerance).
- Qubit allocation with **coherence-time matching**.
- Error estimation using **decoherence × gate-error model**.
- Hardware utilization tracking in real time.

### 📊 Beautiful Real-Time Dashboard
- Live utilization charts (Recharts).
- Interactive Gantt chart of schedule timeline.
- Qubit grid with animated state indicators (free / busy / errored).
- Agent activity feed with AI reasoning transparency.
- Chat interface to talk to the scheduler agent.

### 🔄 Live Updates via WebSocket
- Every agent decision, allocation change, and metric update streams live to the UI.
- No polling — pure real-time experience.

### 🛡️ Graceful Degradation (Hybrid Architecture)
- If Gemini API fails or hits rate limits → falls back to **deterministic classical scheduler**.
- The system never leaves the queue stuck.
- AI is a **bonus layer**, not a strict dependency.

---

## 🏗️ System Architecture

```text
┌─────────────────────────────────────────────────────────┐
│                    REACT FRONTEND                        │
│  Dashboard │ Jobs │ Schedule │ Qubits │ Agent Chat       │
└──────────────────────┬──────────────────────────────────┘
                       │ REST + WebSocket
┌──────────────────────▼──────────────────────────────────┐
│                  FASTAPI BACKEND                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │              AGENT ORCHESTRATOR                  │   │
│  │   ┌────────────┐ ┌─────────────┐ ┌────────────┐  │   │
│  │   │ Scheduler  │ │ Allocation  │ │   Error    │  │   │
│  │   │   Agent    │ │   Agent     │ │   Agent    │  │   │
│  │   └─────┬──────┘ └──────┬──────┘ └─────┬──────┘  │   │
│  │         │               │              │         │   │
│  │         └───────────────┼──────────────┘         │   │
│  │                         │                        │   │
│  │                  ┌──────▼───────┐                │   │
│  │                  │ Gemini API   │                │   │
│  │                  └──────────────┘                │   │
│  │                         │                        │   │
│  │   ┌─────────────────────▼──────────────────────┐ │   │
│  │   │  Fallback Algorithms (deterministic)       │ │   │
│  │   │  priority_queue │ qubit_allocator │ error  │ │   │
│  │   └────────────────────────────────────────────┘ │   │
│  └──────────────────────────────────────────────────┘   │
│                                                          │
│  ┌──────────────────────────────────────────────────┐   │
│  │              SQLite / SQLAlchemy                 │   │
│  │  jobs │ qubits │ schedules │ errors │ agent_logs │   │
│  └──────────────────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────┘


qflow-ai/
│
├── frontend/                  # React + Vite + Tailwind UI
│   ├── src/
│   │   ├── api/               # Axios configurations
│   │   ├── components/        # Reusable UI components
│   │   ├── pages/             # Dashboard, Jobs, Schedule, etc.
│   │   ├── hooks/             # Custom React hooks
│   │   └── context/           # Global state
│   └── package.json
│
├── backend/                   # FastAPI + SQLAlchemy
│   ├── app/
│   │   ├── api/               # API routes
│   │   ├── agents/            # Gemini AI agents
│   │   ├── algorithms/        # Fallback algorithms
│   │   ├── models/            # SQLAlchemy models
│   │   ├── schemas/           # Pydantic schemas
│   │   └── services/          # Business logic
│   ├── main.py
│   └── requirements.txt
│
├── database/                  # SQL schema and seed data
│   ├── schema.sql
│   └── seed_data.sql
│
├── docs/                      # Documentation
│   ├── API_DOCUMENTATION.md
│   └── PROJECT_REPORT.md
│
├── .env.example
├── .gitignore
└── README.md


git clone https://github.com/vishakha2121/quantum-scheduler-ai.git
cd quantum-scheduler-ai


cd backend
python -m venv venv
# Activate virtual environment
# Windows:
venv\Scripts\activate
# Mac/Linux:
source venv/bin/activate

pip install -r requirements.txt

cd frontend
npm install
npm run dev