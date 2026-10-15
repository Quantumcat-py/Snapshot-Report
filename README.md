# Snapshot-Report

A small Python script I wrote while teaching myself cybersecurity. It
builds a timestamped "state of this machine" report you can save,
compare later, or hand to whoever helps you with your computer.

## What it does

Run it and it writes a timestamped text report
(`triage_report_YYYY-MM-DD_HH-MM-SS.txt`): hostname, current user,
local IP, uptime, memory, disk, a connection summary (active count,
listening count, unique external peers), listening ports, active
connections, top processes by memory, and a basic check for
suspicious-looking processes. The idea is a triage snapshot — capture
what the machine looks like *now*, so future-you (or your more
technical friend) has something concrete to compare against when
something feels off.

```bash
pip install psutil   # the one dependency; everything else is stdlib
python3 snapshot_report            # report lands in the current folder
python3 snapshot_report ~/reports  # or pick a folder
```

## Updated

The original (May 2026) gathered everything with bare `os.popen` calls
to Linux tools, and its process check had a bug I learned a lot from.
This version keeps the same sections and the same purpose, rebuilt
properly:

- **The process check compares actual process names now.** The
  original was a **substring search** through the `ps aux` output for
  names like `nc` — which also matches the "nc" inside innocent words
  like "sync". It could absolutely cry wolf. Names are now matched
  exactly, the list is a bit broader (netcat variants, common
  scanners/sniffers), and the section is labelled in the report as
  what it is: a basic name heuristic, not detection. Dual-use tools
  are often legitimate, and the report says so.
- **A computed connection summary** — active connections, listening
  sockets, unique external peers — instead of just dumping raw
  command output.
- **Gathered with `psutil`**, so the same script works on Linux,
  Windows, and macOS.
- **Graceful degradation.** Every section is independent: if one
  can't be gathered it becomes an "(unavailable)" line instead of
  crashing the whole report. The original ran every command bare, so
  if one failed, you heard about it.

## This grew up → the QuantumCat SOC Toolkit

This script is one of three I wrote in May 2026 while learning. I
eventually rebuilt all three properly and combined them into a single
desktop app — the **QuantumCat SOC Toolkit**. Of the three, this one's
idea held up best: the one-click report is the heart of the app.

What the app version adds:

- The same report from a **GUI button**, saved to a proper per-user
  data folder, with the highlights fed straight to the analyst
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
