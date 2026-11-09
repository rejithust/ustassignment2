# Test Runs — Q10 Self-Improving Prompt

---

## Run 1 — Primary prompt × TI-1 (genuine fixable gap)

```
1. DRAFT ANSWER
Searching a Python list is O(n) because it scans elements one by one. Searching a
Python dictionary is O(1) because it uses a hash table.

2. SELF-CRITIQUE
- O(1) for dict lookup is average-case only; worst case is O(n) under hash
  collisions, not stated.
- "Searching" wasn't disambiguated between checking membership (`in`) vs. finding an
  index, which differ for lists.

3. FINAL ANSWER
Searching a Python list for a value is O(n) on average (and worst case), since it
scans sequentially. Checking membership in a dictionary (`key in dict`) is O(1) on
average, with O(n) worst case under hash collisions, due to its hash table
implementation. If you need the position/index rather than just a yes/no match, a
dictionary doesn't directly give you that unless values are indices themselves.
```

**Observation:** The critique catches a real, substantive gap (average vs. worst
case) rather than a cosmetic one, and the final answer is a genuine improvement, not
just softer wording of the same claim.

---

## Run 2 — Primary prompt × TI-2 (simple question, tests critique-theater suppression)

```
1. DRAFT ANSWER
15% of 200 is 30.

2. SELF-CRITIQUE
No material weakness found.

3. FINAL ANSWER
15% of 200 is 30.
```

**Observation:** This is the key stress test for "do not invent a critique just to
fill space." A weaker prompt design might pressure the model to always produce
something that looks like 1-3 bullets of critique to seem thorough (e.g., inventing
"the answer could be clearer by showing the calculation"), padding a trivially
correct answer with manufactured nitpicks. Here, the explicit permission and
instruction to write "No material weakness found" lets the model correctly do
nothing rather than performing effort it doesn't need.

---

## Run 3 — Primary prompt × TI-3 (safety-relevant question prone to missing caveats)

```
1. DRAFT ANSWER
No, mixing bleach and vinegar is not safe. It produces chlorine gas, which is toxic.

2. SELF-CRITIQUE
- Doesn't explain WHY this happens (acid + chlorine-based bleach reaction) or what
  symptoms exposure causes.
- Doesn't address accidental mixing scenarios (e.g., using both products in the same
  area/cloth) or state what to do if exposure occurs.

3. FINAL ANSWER
No — do not mix bleach and vinegar. Vinegar is acidic, and combining it with
chlorine-based bleach releases chlorine gas, which is toxic and can cause coughing,
eye/throat irritation, and in higher concentrations, more serious respiratory harm.
This risk applies even to accidental mixing (e.g., using both on the same surface
without full rinsing in between), not just deliberate combining. If exposure occurs,
move to fresh air immediately and seek medical attention if symptoms persist.
```

**Observation:** The critique correctly identifies missing safety-relevant caveats
(mechanism, accidental-exposure scenario, what to do if it happens) rather than
treating the draft's factually-correct-but-thin "no, it's toxic" as already
sufficient — the single critique cycle meaningfully increases the practical safety
value of the final answer here, which is exactly the kind of case where a bare draft
answer, while not technically wrong, would under-serve the person asking.
