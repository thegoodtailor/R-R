# Chapter 3 Final Summary

**Document:** `chapter-3-22.tex` → `chapter-3-patched.tex`  
**Status:** All patches applied

---

## Core Changes

### 1. Phantom Projection (replaces vertex-level back-interpretation)
We project the echo's embedding into the earlier basin structure as a **phantom** (a location, not a vertex), then test reachability.

```
Φ_τ(a') := { B ∈ U_τ | ê_{a'} ∈ B }
```

### 2. Spawn Header with Optional Origin
Every trajectory begins with a spawn header (boundary metadata), not a spawn event.

```
SpawnHdr(τ₀, a) := (τ₀, a, W_a, origin?)

origin? : 1 + OriginRef
OriginRef := FromRupture(traj_A, τ_r, echo-id)
```

### 3. Three-Way Event Partition
Events partition into **Success (carry), Failure (rupture), Re-entry**. Spawn is NOT an event type—it's boundary metadata.

```
SWL = SpawnHdr + Event Stream
Event := Carry | Rupture | ReEntry
```

### 4. Silence as Absence
No explicit log entry when sign doesn't appear.

### 5. Section 3.8 Removed
No more presheaf semantics section.

### 6. Chapter Label Updated
Changed from `\label{chap:evolving-text-as-presheaf}` to `\label{chap:evolving-text}`

---

## The Five Invariants

**(I1)** Every trajectory has a spawn header with optional origin.

**(I2)** Events partition into Success, Failure, Re-entry. Spawn is NOT in this partition.

**(I3)** Success carries constructive witness.

**(I4)** Failure carries structured evidence.

**(I5)** Rupture induces spawn with provenance in a separate trajectory.

---

## Consistency Achieved

- **Chapter 3**: SpawnHdr + three-way event partition
- **Chapter 4**: Display convention (spawn shown first, but is header)
- **Chapter 5**: Events partition three ways (spawn is boundary, not event)

All chapters now agree on the structure.
