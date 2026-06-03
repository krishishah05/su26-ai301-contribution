# Contribution 1: Fix wrong time complexity in Lasers and Mirrors solution

**Contribution Number:** 1
**Student:** Krishi Shah
**Issue:** [cpinitiative/usaco-guide#6058 - Contact Form Submission - Mistake (Solution: USACO Gold 2016 December - Lasers and Mirrors)](https://github.com/cpinitiative/usaco-guide/issues/6058)
**Status:** Phase II Complete

---

## Why I Chose This Issue

I chose this issue because it is a clearly scoped content fix: a user reported that the time complexity listed in the Lasers and Mirrors solution page is incorrect, and linked to a forum thread confirming the mistake. The fix involves editing a single MDX file to correct the stated complexity, which makes it easy to verify and straightforward to submit. USACO Guide is a widely used resource for competitive programming students, so even a small correction has real impact on people learning from it.

I also wanted to start with something where I could focus on learning the contribution workflow rather than getting blocked on a complex codebase. This issue gives me a clean first PR to point to, and the project has an active maintainer community and clear setup docs, so I know I can get help if anything comes up.

---

## Understanding the Issue

### Problem Description

The time complexity displayed on the Lasers and Mirrors solution page is incorrect. A user flagged it via the contact form and linked to a USACO Guide forum thread with more details confirming the error.

### Expected Behavior

The solution page should display the correct time complexity for the algorithm.

### Current Behavior

The solution page shows an incorrect time complexity value.

### Affected Components

A single MDX solution file in the usaco-guide content directory for the Lasers and Mirrors problem.

---

## Reproduction Process

### Environment Setup

Cloned the fork locally using `git clone --depth 1 https://github.com/krishishah05/usaco-guide.git`. No additional setup was needed to verify the issue since this is a content fix in an MDX file. The repo runs on Node/Next.js but running the full dev server is not required to confirm the wrong time complexity annotation.

Created a working branch from `main`:
```
git checkout -b fix-issue-6058
git push origin fix-issue-6058
```

**Working branch:** https://github.com/krishishah05/usaco-guide/tree/fix-issue-6058

### Steps to Reproduce

1. Clone the fork and open `solutions/gold/usaco-671.mdx`.
2. Find line 279: `**Time Complexity:** $\mathcal{O}(N)$`
3. Read the BFS loop above it. When a node is dequeued, the code iterates over every point sharing the same coordinate using `lines[dir][coord]`.
4. Consider a worst-case input: N/2 fence posts all share x=5, and each is reachable from a different y-coordinate chain. All N/2 posts get added to the queue with direction 0. When each is later dequeued, it re-iterates the full N/2-element list for x=5, even though every entry is already marked visited. Total loop iterations: O(N/2 * N/2) = O(N^2).
5. The claimed O(N) is incorrect. The forum thread linked in the issue confirms this analysis and suggests a fix.

### Reproduction Evidence

- **Branch:** https://github.com/krishishah05/usaco-guide/tree/fix-issue-6058
- **File:** `solutions/gold/usaco-671.mdx`, line 279
- **My findings:** The BFS correctly avoids revisiting nodes (via `dist` check) but does not avoid re-iterating coordinate lists from different queue entries. The O(N) claim only holds if each coordinate list is processed at most once, which the current code does not guarantee.

---

## Solution Approach

### Analysis

The BFS loop in `solutions/gold/usaco-671.mdx` marks individual nodes as visited using the `dist` array, but it does not track which `(direction, coordinate)` pairs have already been fully processed. Because of this, multiple queue entries sharing the same coordinate will each trigger a full iteration over that coordinate's point list. The list iteration cost adds up to O(N^2) in the worst case, not O(N).

### Proposed Solution

Add a visited set tracking `(direction, coordinate)` pairs. Before running the inner for loop, check whether the current `(dir, coord)` pair is already in the set. If it is, skip the loop. If not, add it and proceed. This ensures each coordinate list is iterated at most once per direction, reducing total work to O(N) amortized with unordered_map.

### Implementation Plan

**Understand:** The time complexity annotation on line 279 of `solutions/gold/usaco-671.mdx` says O(N), but the actual worst-case cost is O(N^2). The fix is to skip redundant coordinate list iterations using a visited-coordinates set.

**Match:** The existing code already uses an `unordered_map` for coordinate lookups and a `dist` array to avoid revisiting nodes. The fix follows the same pattern but adds one more layer of "already handled" tracking at the coordinate level rather than the node level.

**Plan:**
1. In all three language implementations (C++, Java, Python) inside the BFS while loop, declare a set to track processed `(direction, coordinate)` pairs (e.g., `unordered_set` in C++, `HashSet` in Java, `set` in Python).
2. Before the inner for loop, check if the current `(dir, coord)` pair is already in the set. If yes, `continue` to the next queue entry.
3. If not in the set, insert it and run the for loop as normal.
4. Update the time complexity annotation on line 279 to reflect the correct complexity of the fixed code (O(N) amortized with unordered_map / HashSet).

**Implement:** https://github.com/krishishah05/usaco-guide/tree/fix-issue-6058 (code changes in Phase III)

**Review:** Will check CONTRIBUTING.md for commit message and PR format requirements before opening the PR. Will verify the MDX renders correctly by running the Next.js dev server locally.

**Evaluate:** After the fix, manually trace through the worst-case input (N points all sharing one coordinate) and confirm each coordinate list is iterated exactly once. Run any existing test suite and confirm no regressions. Check that the page at `/problems/usaco-671-lasers-and-mirrors/solution` renders the updated complexity correctly.

---

## Testing Strategy

### Unit Tests

- [ ] Test case 1: [Description]
- [ ] Test case 2: [Description]
- [ ] Test case 3: [Description]

### Integration Tests

- [ ] Integration scenario 1
- [ ] Integration scenario 2

### Manual Testing

[What you tested manually and results]

---

## Implementation Notes

### Week [X] Progress

[What you built this week, challenges faced, decisions made]

### Week [Y] Progress

[Continue documenting as you work]

### Code Changes

- **Files modified:** [List]
- **Key commits:** [Links to important commits]
- **Approach decisions:** [Why you chose certain approaches]

---

## Pull Request

**PR Link:** [GitHub PR URL when submitted]

**PR Description:** [Draft or final PR description - much of the content above can be adapted]

**Maintainer Feedback:**
- [Date]: [Summary of feedback received]
- [Date]: [How you addressed it]

**Status:** [Awaiting review / Iterating / Approved / Merged]

---

## Learnings & Reflections

### Technical Skills Gained

[What you learned technically]

### Challenges Overcome

[What was hard and how you solved it]

### What I'd Do Differently Next Time

[Reflection on your process]

---

## Resources Used

- [Link to helpful documentation]
- [Tutorial or Stack Overflow post that helped]
- [GitHub issues or discussions that helped]
