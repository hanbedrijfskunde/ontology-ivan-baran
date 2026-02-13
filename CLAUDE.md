# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static, single-page educational briefing site (in Dutch) about ontology in the context of connected products and AI. It was created for an HBO Bedrijfskunde (Business Administration) student project advising a client on using ontologies to make machine/sensor data actionable for predictive maintenance.

## Architecture

The entire site lives in a single `index.html` file with no build tools, bundlers, or package managers. Everything is self-contained:

- **CSS**: Embedded in `<style>` within `<head>`. Uses CSS custom properties (`:root` variables) for theming (colors like `--primary`, `--success`, `--danger`, etc.).
- **JavaScript**: Inline `<script>` at bottom of `<body>`. Two features: accordion-style layer toggles (`toggleLayer`) and an interactive quiz (`selectOption`, `checkQuiz`).
- **External dependencies**: Google Fonts (Inter) and Lucide icons via CDN only.

## Running Locally

Open `index.html` directly in a browser. No server or build step required.

## Content Structure

The page has 7 numbered sections progressing from problem statement through ontology explanation to practical recommendations, followed by a 3-question quiz and resource links. The `background-material/` folder contains a markdown file with technical ontology examples (JSON Schema, RDF/Turtle, Python/Neo4j pseudo-code) used as reference material.

## Key Conventions

- All user-facing text is in **Dutch**.
- The design uses a component-based CSS approach with BEM-like class naming (`.compare-card.without`, `.quiz-option.correct`, `.layer.active`).
- Interactive state is managed via CSS class toggling (e.g., `.active`, `.selected`, `.correct`, `.incorrect`, `.answered`).
