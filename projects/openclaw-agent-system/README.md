# OpenClaw — Personal AI Agent System

## Overview
OpenClaw is my self-hosted personal AI agent system, designed for daily automation and long-term use rather than one-time demos.

It runs on my own server and is used to automate information collection, reminders, health routines, and personal tracking tasks.

## What It Does
- Runs as a 7×24 personal AI assistant
- Supports Telegram push delivery
- Handles scheduled background tasks
- Uses long-term memory for persistent context
- Coordinates multiple AI models for different task types

## System Highlights
- 15 automated background tasks across information, motivation, health, and journaling
- Multi-model routing with Gemini 2.5 Pro as primary and DeepSeek as backup
- Automatic fallback when the primary model fails repeatedly
- Self-hosted deployment on personal server
- Low operating cost and practical daily usage

## Engineering Work
I independently handled deployment, operation, and troubleshooting for this system.

Key engineering points:
- Self-hosted deployment and configuration
- Multi-model routing and fallback logic
- Prompt design for scheduled tasks
- Long-term memory setup and maintenance
- Telegram delivery workflow
- System operation with restart / config update / log inspection

## Problems I Solved
During operation, I encountered and fixed several real issues:

- Model 503 failures and backup model switching
- Memory contamination / context interference
- Config reload problems after task updates
- File write and path-related issues
- Message sending failures
- Remote debugging and config modification

These issues gave me practical experience in maintaining an AI system after deployment, not just building it once.

## Why This Project Matters
This project shows that I can turn AI ideas into a running system, keep it working, and solve problems when it breaks.

It is not just a prompt demo or a chatbot screenshot — it is a self-hosted AI system with real tasks, real failures, and real maintenance work.

## Stack
- OpenClaw
- Gemini 2.5 Pro
- DeepSeek
- Telegram
- Personal VPS
- JSON config
- VSCode Remote

## Proof / Materials
The following materials will be added here:
- architecture notes
- task overview
- screenshots
- operation notes
- troubleshooting records
