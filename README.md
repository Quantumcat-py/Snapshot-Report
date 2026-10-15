# Snapshot-Report

A small Python script I wrote while teaching myself cybersecurity. It
builds a timestamped "state of this machine" report you can save,
compare later, or hand to whoever helps you with your computer.

## What it does

The script runs a batch of system commands (mostly `os.popen` calls)
and writes a timestamped text report: system info, processes,
connections, and a basic check for suspicious-looking processes. The
idea is a triage snapshot — capture what the machine looks like *now*,
so future-you (or your more technical friend) has something concrete
to compare against when something feels off.

## Run it

```bash
python3 snapshot_report
```

## Honest notes

- The "suspicious process" check is a **substring search** through the
  `ps aux` output for names like `nc` — which also matches the "nc"
  inside innocent words like "sync". It can absolutely cry wolf. It's
  a beginner's first idea of detection, and it taught me why real
  detection is harder than it looks.
- Every command runs bare, so if one fails, you'll hear about it.
  There's no graceful degradation in this version.

## This grew up → the QuantumCat SOC Toolkit

This script is one of three I wrote in May 2026 while learning. I
eventually rebuilt all three properly and combined them into a single
desktop app — the **QuantumCat SOC Toolkit**. Of the three, this one's
idea held up best: the one-click report is the heart of the app.

What the app version adds:

- **One click** in a GUI tab builds the timestamped report: hostname,
  uptime, memory, disk, listening ports, active connections, top
  processes — plus a computed connection summary (active count,
  listening count, unique external peers)
- The flagged-process check now compares actual **process names**
  instead of substrings, and it's labelled in the app as what it is:
  a basic name heuristic, not detection. Dual-use tools are often
  legitimate, and the report says so
- Every section degrades to an "(unavailable)" line instead of
  crashing the whole report
- The other two tools built in: a live connection monitor with
  LOCAL/EXTERNAL tagging and top talkers, and a threaded port scanner
- An **AI Analyst** tab that reads the report's highlights and explains
  them in plain English — VERDICT → what looks normal → what deserves
  a second look → suggested next steps — running 100% locally via
  Ollama, with a built-in rule engine if you don't have Ollama
- Real installers: a Windows `.msi` that gives you a desktop shortcut
  and a Start Menu entry, plus a Linux `.deb` and a portable
  `.tar.gz`. Click the cat, the app opens — no terminal

It's $12, one-time. And it's honest about what it is: a
beginner-friendly triage tool, not professional security software and
not a Wireshark replacement. 100% local, no accounts, no telemetry.
The full Python source is included in the download.

→ https://quantumcatsoc.gumroad.com/l/quantumcat-soc-toolkit

The other two originals:
[Network-Monitor](https://github.com/Quantumcat-py/Network-Monitor) ·
[Port_scanner](https://github.com/Quantumcat-py/Port_scanner)
