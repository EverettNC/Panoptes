# Panoptes

A search that finds what you meant, not just what you typed, and always tells you what's true right now.

## What it is

Most searches match letters. Type the wrong word, or ask about something that used to be true, and they hand it right back to you.

Panoptes looks across everything at once: the repos, the vault, the working folders, the chat record, and what is actually running. It understands what is being asked for, checks the real thing before it answers, and knows when an old answer has been replaced by a newer one. It shows exactly why it matched something or missed it. And it never stops learning.

## Why it exists

On 2026-10-19 a search for the 3D in the Athanor came back saying it didn't exist. It found an October 3 note saying so. The 3D campus was pushed that same evening and has been running ever since. Nothing marked the old note as out of date. Panoptes is built so that never happens again.

## Status

Not built yet. The build order is written: see [BUILD.md](BUILD.md). Nothing in this README describes working code until the Progress section of BUILD.md says it does.

## The rules it lives under

- Runs on the Mac, bound to 127.0.0.1 only. No one else's key, no cloud, no telemetry.
- Its own matching algorithm, not a standard edit-distance measure.
- The fault catalog and the query parser live side by side at the root, so the logic is always visible.
- Breathing: actively alive and ever evolving, always searching for the learn.
- Every answer carries when it was true, and every search is written to an append-only ledger.

The Christman AI Project and Robotics Division Inc
