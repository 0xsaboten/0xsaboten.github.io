---
title: Ash Record
description: 
date: 2026-07-25 12:56:00
categories: [CTF, HTB - Cyber Apocalypse 2026]
tags: [coding]
pin: false
image:
  path: /assets/img/2026-posts/cyber-apocalypse/ash-record/console.png 
  alt: Coding Challenge
---

## Challenge Summary
**Challenge Scenario**
> Aeron's scouts found a riverside hamlet that should have been starving. The ovens were still warm. The stew pot was fresh. The chairs were set, then abandoned mid-meal. Elowen Ashglass reads the scene like a staged confession: soot where it shouldn't cling, drag-marks that don't match panic, footprints that kept their spacing instead of scattering. This wasn't a raid — it was a procession, and someone wanted the hamlet preserved, not burned. Elowen has recovered residues across the site, each carrying a timestamp and a material type. She also holds the extraction sequence she suspects was followed — the order materials would appear in if hands moved people out calmly, stage by stage, from entry to compliance to departure. She needs the longest prefix of that sequence she can actually confirm: a subsequence of residues matching it in order, where no two consecutively matched residues are closer together than a minimum gap, because no stage of an orderly extraction happens faster than that.

**Challenge Info**

|:------:|:------:|
| Category | Coding
| Points | 900 |
| Difficulty | Medium
| Creator | 
| Release Date | 2026/07/24

## Solution

The goal of this challenge is to determine the maximum number of consecutive steps from the beginning of the extraction sequence until the end or when no residues remain all while satisfying the time gap constraint. The challenge provides the follow example input and output.

```
Input:
5 4 3
ash rope oil ash
1 ash
4 rope
7 oil
10 ash
11 rope

Expected output:
4
```

For this example there are `5` (N) different recovered residues, the extraction sequence is `4` (P) materials long, and the minimum gap between two consecutive steps is `3`. The challenge also provided an explanation for how the answer was derived:

```
Confirming the full sequence of 4 steps:
  Step 1: residue at timestamp 1, type ash.
  Step 2: residue at timestamp 4, type rope.  Gap from step 1: 3 >= 3.
  Step 3: residue at timestamp 7, type oil.   Gap from step 2: 3 >= 3.
  Step 4: residue at timestamp 10, type ash.  Gap from step 3: 3 >= 3.
```

Let's start by grabbing our `N`, `P`, and `min_gap` variables. These can be recovered by using `input()` and then indexing the individual variables out.

```python
line_one = input().split()  # Grabs the first line.
n = int(line_one[0])        # N is the number of recovered residues.
p = int(line_one[1])        # P is the length of the suspected extraction sequence.
min_gap = int(line_one[2])  # min_gap is the minimum time difference required between any two consecutively matched residues.
```

Next we're provided with a list (length `P`) which is the extraction sequence. This one-liner will collect the extraction sequence, in order, and store it in a list called `extraction_sequence`.

```python
extraction_sequence = [x for x in input().split()]
```

Lastly, we're provided `N` lines of two separated values: the timestamp and the material type. The following code snippet, saves the next `N` lines as a Python dictionary, using the timestamp as the key and the material type as the value.

```python
# Recovered Residues
recovered_residue = {}
for _ in range(n):
    line = input().split()
    timestamp = int(line[0])
    material = line[1]
    recovered_residue[timestamp] = material
```

Before calculating the solution we'll need some helper variables.

```python
steps = 0               # How many steps have been taken.
material_needed_idx = 0 # The current material needed in the extraction sequence.
prev_timestamp = 0      # The previous timestamp (Used to calculate the gap).
```

With everything in hand we're now ready to start coding the solution. We'll start with our for loop, looping over every timestamp provided.

```python
for timestamp in sorted(recovered_residue.keys()):
    material = recovered_residue[timestamp]
```

The challenge scenario stated "Residues are not necessarily given in timestamp order," which is why we used `sorted()`. `material = recovered_residue[timestamp]` grabs the material for the current timestamp.

The first thing we'll check for is if `prev_timestamp` is still zero. If that's the case, we're on the first iteration of the for loop and there is no previous gap to calculate.

```python
    if prev_timestamp != 0:
        gap = timestamp - prev_timestamp
    else:
        gap = timestamp
```

Next, we'll set a stopper in case our `material_needed_idx` goes past the length of `extraction_sequence` (Preventing an index out of bound error).

```python
    if material_needed_idx >= len(extraction_sequence):
        break
```

We'll also grab the current material we're looking for.

```python
    needed_material = extraction_sequence[material_needed_idx]
```

Finally, we'll put everything we've collected altogether. First we check if the `material` we have is equal to the needed material (`needed_material`) in the extraction sequence. If it is, we'll check if the gap calculated above is greater than or equal to the `min_gap` provided. If both checks come back true, we'll set the previous timestamp (`prev_timestamp`) equal to the current timestamp to calculate the gap for the next iteration. We'll also increment our index for the `extraction sequence` (we now need the next element in the sequence) and increment our step counter to keep track of our steps accurately.

```python
    if material == needed_material:
        if gap >= min_gap:
            prev_timestamp = timestamp
            material_needed_idx += 1
            steps += 1
```

Once the loop has concluded, we can `print(steps)` and click `Run Code` to obtain our flag.

![HTB Challenge Flag](/assets/img/2026-posts/cyber-apocalypse/ash-record/flag.png)

### Full Code
```python
line_one = input().split()  # Grabs the first line.
n = int(line_one[0])        # N is the number of recovered residues.
p = int(line_one[1])        # P is the length of the suspected extraction sequence.
min_gap = int(line_one[2])  # min_gap is the minimum time difference required between any two consecutively matched residues.

# Extraction Sequence
extraction_sequence = [x for x in input().split()]

# Recovered Residues
recovered_residue = {}
for _ in range(n):
    line = input().split()
    timestamp = int(line[0])
    material = line[1]
    recovered_residue[timestamp] = material

steps = 0               # How many steps have been taken.
material_needed_idx = 0 # The current material needed in the extraction sequence.
prev_timestamp = 0      # The previous timestamp (Used to calculate the gap).

for timestamp in sorted(recovered_residue.keys()):
    material = recovered_residue[timestamp]
    
    if prev_timestamp != 0:
        gap = timestamp - prev_timestamp
    else:
        gap = timestamp

    if material_needed_idx >= len(extraction_sequence):
        break

    needed_material = extraction_sequence[material_needed_idx]

    if material == needed_material:
        if gap >= min_gap:
            #print(f"Step {steps}: residue at timestamp {timestamp}, type {material}. Gap from step {steps}: {gap} >= {min_gap}") # For debugging.
            prev_timestamp = timestamp
            material_needed_idx += 1
            steps += 1
    
print(steps)
```