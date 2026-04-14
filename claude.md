Build a full-stack web application called **"DJ Transition Assistant"** that allows users to load and mix multiple tracks (like a DJ console) while intelligently suggesting the best next tracks for smooth transitions.

## 🧰 Tech Stack

* Framework: Next.js (App Router) + TypeScript
* Styling: Tailwind CSS
* UI Library: shadcn/ui (Radix primitives)
* State Management: Zustand (for deck + audio state)
* Data Fetching: React Query (TanStack Query)
* Audio Engine: Web Audio API
* Playback Source: SoundCloud API (or fallback to direct audio URLs)
* Waveform Visualization: Wavesurfer.js
* Animations: Framer Motion
* Charts: Recharts (for energy visualization)
* Icons: Lucide React

## 🎧 Core Concept

Create a **multi-deck DJ interface (2–4 decks)** where:

* Each deck can independently load and play a track
* Users can control playback and mix between decks
* The system suggests the best next tracks for transitions

## 🎛️ Deck System

Each deck should have:

* Track loader (search + select from SoundCloud)
* Play / Pause
* Seek / scrub
* Volume (GainNode)
* Waveform display
* BPM (if available or estimated)
* Track metadata (title, artist, artwork)

Define a deck structure:

type Deck = {
id: string
track: Track | null
isPlaying: boolean
gain: number
audioBuffer?: AudioBuffer
bpm?: number
}

## 🔊 Audio Engine (Web Audio API)

* Create a shared AudioContext
* Each deck:

  * AudioBufferSourceNode
  * GainNode (volume control)
* Global:

  * Crossfader between Deck A and Deck B
  * Master output node

Implement:

* Smooth crossfade logic
* Ability to play multiple tracks simultaneously
* Sync playback timing across decks (basic version)

## 🔁 Transition Recommendation Engine

Implement:

getTransitionScore(currentTrack, candidateTrack)

Since SoundCloud does not provide rich metadata:

* Estimate BPM (optional, basic version can skip or approximate)
* Use heuristics:

  * Genre similarity
  * Track duration similarity
  * Energy proxy (RMS amplitude or waveform intensity if possible)

Return:

* Score (0–100)
* Transition type:

  * Smooth crossfade
  * Quick fade
  * Hard cut
* Suggested crossfade duration

## 🔍 Track Discovery

* Use SoundCloud search API to fetch tracks
* Allow users to:

  * Search manually
  * Load tracks into any deck
* Generate “Suggested Next Tracks”:

  * Based on currently playing deck
  * Ranked using transition score

## 🎛️ UI / UX (DJ-themed)

Design a dark, neon, club-style interface:

### Theme

* Background: black / deep purple gradient
* Accent: neon blue / pink
* Style: glassmorphism + glow effects

### Layout

1. **Top Bar**

   * App name: DJ Transition Assistant
   * Search input (SoundCloud track search)

2. **Deck Grid**

   * 2-deck (default) or 4-deck layout
   * Each deck card includes:

     * Waveform
     * Controls
     * Track info

3. **Crossfader Section**

   * Horizontal slider (Deck A ↔ Deck B)
   * Visual feedback

4. **Suggestions Panel**

   * List of recommended tracks
   * Each item shows:

     * Track name
     * Compatibility score
     * Suggested transition type
   * Button: “Load into Deck”

5. **Energy / Flow Visualization**

   * Chart showing energy across decks

## ✨ Advanced Features (Optional but preferred)

* Auto DJ Mode:

  * Automatically load best next track into an empty deck
* Beat sync (basic alignment, optional)
* Keyboard shortcuts (like DJ software)
* Visual indicators:

  * Green = smooth transition
  * Yellow = moderate
  * Red = risky
* Tooltip explaining why a transition is good

## ⚙️ Architecture Notes

* Use client components for audio engine (Web Audio API runs in browser)
* Use server components for initial page structure
* Cache track data locally to reduce API calls
* Handle cleanup of AudioBufferSourceNodes properly

## 🚫 Constraints

* Do NOT rely on Spotify
* Do NOT require external backend or AWS
* Keep everything runnable locally

## 🎯 Output

* Clean folder structure
* Modular components:

  * Deck
  * Crossfader
  * Waveform
  * Suggestions Panel
* Reusable hooks:

  * useAudioEngine
  * useDeckManager
  * useTransitionEngine

Make the app feel like a **professional DJ console in the browser** — smooth, responsive, and visually impressive.
