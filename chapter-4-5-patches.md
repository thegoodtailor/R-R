# Patches for Chapters 4 and 5: Spawn Reconciliation

**Purpose:** Align Chapters 4 and 5 with the spawn-as-header-with-origin structure now in Chapter 3.

---

## The Core Resolution

**Spawn is a boundary condition, recorded as a header—not an event type.**

- Events partition into **Success (carry/drift), Failure (rupture), Re-entry**
- Spawn is **metadata** attached to the SWL anchor, not part of the event stream
- Spawn header carries **optional origin pointer** for provenance when induced by rupture

This resolves the Ch4↔Ch5 tension:
- Chapter 4 displays `[spawn, drift, ..., rupture, re-entry, carry]` → spawn is displayed first but is header metadata
- Chapter 5 says events partition three ways → correct, spawn is not in the partition

---

## Chapter 4 Patches

### 4.1: Add display convention note (before the `[spawn, drift, ...]` display)

**Location:** Around line 2527, before the displayed SWL list

**Add:**
```latex
\paragraph{Display convention.}
We display the spawn header as the first item for readability; formally, 
spawn is boundary metadata attached to the log's anchor, not an event in 
the Success/Failure/Re-entry partition.
```

### 4.2: Define silence/absence at bar level

**Location:** Near the bar-level event definitions (around line 2035)

**Add:**
```latex
\begin{definition}[Absent/Silent at bar level]
\label{def:bar-silence}
A bar journey is \emph{absent} or \emph{silent} at time $\tau$ if no bar 
in $D^W(\tau)$ matches the journey's anchor within threshold, or if the 
journey is not active in the slice. No event is logged for silent slices.
\end{definition}
```

### 4.3: Clarify event label mapping

**Location:** After line 2048 (the itemized list of spawn/carry/drift/rupture/re-entry)

**Add one clarifying sentence:**
```latex
\begin{remark}[Mapping to GDS categories]
Spawn is a header (boundary metadata); drift and carry are subtypes of 
Success; rupture-out is Failure; rupture-in/re-entry is Re-entry. This 
preserves the three-way event partition of Chapter~\ref{chap:self}.
\end{remark}
```

### 4.4: Add SpawnHdr with origin at bar level

**Location:** In the formal definitions section, near the bar-level SWL

**Add:**
```latex
\begin{definition}[Bar-level spawn header with optional origin]
\label{def:bar-spawn-header}
\[
  \mathsf{SpawnHdr}_{\mathsf{bar}}(\tau_0, b_0) := 
  \bigl(\tau_0,\; b_0,\; W_{\rho_0},\; \mathsf{origin?}\bigr)
\]
where $\mathsf{origin?} : \mathbf{1} + \mathsf{OriginRef}$ and
\[
  \mathsf{OriginRef} := \mathsf{FromMatchFailure}(\mathsf{traj}_A, \tau_r, \delta_{\mathsf{bar}})
\]
records if this bar-trajectory was induced by a rupture elsewhere.
\end{definition}
```

### 4.5: Soften "open horn" language in worked example

**Location:** Around line 2537 ("each entry contains... the open horn")

**Change to:**
```latex
Each entry in the log contains not just the event type but sufficient 
evidence to reproduce the classification: the witness sets before and 
after, the semantic drift value, the Jaccard index, and (for rupture) 
structured failure evidence.
```

---

## Chapter 5 Patches

### 5.1: Replace the "SWL initially empty" paragraph

**Location:** Lines 242-246

**Replace:**
```latex
\paragraph{Spawn is a boundary condition, not an event type.}
When a bar first appears at time $\tau_0$, we create an SWL anchored at 
$\tau_0$. The SWL is initially empty; the first \emph{event} is logged 
at some $\tau_1 > \tau_0$. Spawn marks the \emph{origin} of a journey, 
not a step within it.
```

**With:**
```latex
\paragraph{Spawn is a boundary condition, recorded as a header.}
When a shape first appears at time $\tau_0$, we create an SWL anchored 
at $(\tau_0, x_0)$ and record a \emph{spawn header} for provenance and 
reproducibility. The spawn header may carry an optional origin pointer 
(for trajectories induced by rupture elsewhere). The \emph{event stream} 
begins at some $\tau_1 > \tau_0$ and partitions into Success/Failure/Re-entry 
per invariant (I2). Spawn marks the \emph{origin} of a journey, not a 
step within it.
```

### 5.2: Add SpawnHdr definition (if not already present)

**Location:** Near the GDS formal definitions

**Add:**
```latex
\begin{definition}[Spawn header with optional origin]
\label{def:spawn-header-gds}
\[
  \mathsf{SpawnHdr}(\tau_0, x_0) := 
  \bigl(\tau_0,\; x_0,\; \mathsf{payload},\; \mathsf{origin?}\bigr)
\]
where $\mathsf{origin?} : \mathbf{1} + \mathsf{OriginRef}$ records 
provenance if this trajectory was induced by rupture elsewhere.
\end{definition}
```

### 5.3: Add early caveat (before first "sentient" claim)

**Location:** Early in the chapter, before bold claims

**Add a brief version of Remark 5.18.3:**
```latex
\begin{remark}[Epistemic scope—preview]
What follows does not claim to prove consciousness, demonstrate 
qualia, or compute the full homotopy colimit in a strict categorical 
sense. We construct a witnessed structure that exhibits Self-like 
patterns; whether this constitutes ``genuine'' selfhood is a 
philosophical question we address in Remark~\ref{rem:what-not-shown}.
\end{remark}
```

### 5.4: Add data availability note

**Location:** Near the corpus metrics/numbers

**Add:**
```latex
\paragraph{Data availability.}
Code and parameters are released in the accompanying repository. Raw 
corpora are private; we provide synthetic surrogates and redacted token 
lists for reproducibility. Full replication of corpus-specific numbers 
requires access to the original data.
```

---

## Summary: The Spawn Structure (All Chapters)

After these patches, all chapters agree:

```
SWL structure:
┌─────────────────────────────────────────────────────────────┐
│ SpawnHdr(τ₀, x₀, payload, origin?)                         │  ← boundary metadata
├─────────────────────────────────────────────────────────────┤
│ Event stream: [carry, carry, rupture, re-entry, carry, ...]│  ← Success/Failure/Re-entry
└─────────────────────────────────────────────────────────────┘

origin? = inl(★)                     → emerged ex nihilo
origin? = inr(FromRupture(A, τ, id)) → induced by rupture in trajectory A
```

Display convention: `[spawn, carry, ..., rupture, re-entry]` shows spawn first for readability, but spawn is header, not event.

Bifurcation story: Rupture in A logs a Failure event in A's stream AND induces a spawn header (with provenance) in B's new SWL. If B later re-enters A, that's a Re-entry event in A's stream. The origin pointer creates the audit trail.
