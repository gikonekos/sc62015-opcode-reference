# Methodology

## Goal

The goal of this project is to build a clean and verifiable reference for the SC62015 instruction set.

## Basic rules

1. Follow the wording and notation of the primary printed source whenever possible.
2. Do not invent shorthand forms that are not supported by the source.
3. If a value or expression cannot be read with confidence, mark it unresolved.
4. Distinguish clearly between:
   - directly confirmed readings
   - reconstruction based on cross-checking
   - unresolved items

## Repository policy

This repository prefers final, reviewable results over unstable work-in-progress fragments.

Intermediate notes that depended on temporary scan conditions may be omitted from the public tree if they are not reproducible.

## Verification principle

Opcode tables should be published only after the relevant entries have been checked against the available source pages.

## Language policy

- `README.md` and `docs/*.md` in English are the main public-facing files.
- Japanese counterparts are provided when useful.
