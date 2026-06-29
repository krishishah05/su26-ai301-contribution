# Contribution 1: Fix wrong time complexity in Lasers and Mirrors solution

**Contribution Number:** 1
**Student:** Krishi Shah
**Issue:** [cpinitiative/usaco-guide#6058 - Contact Form Submission - Mistake (Solution: USACO Gold 2016 December - Lasers and Mirrors)](https://github.com/cpinitiative/usaco-guide/issues/6058)
**Status:** Phase IV Complete

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

### Manual Testing

The usaco-guide does not have an automated test suite for solution correctness. Verification was done by:

- Tracing through the worst-case input manually to confirm the fix eliminates redundant coordinate list iterations
- Checking that the `processed` set is populated correctly: each `(dir, coord)` pair is inserted on first encounter and skipped on all subsequent encounters
- Confirming the BFS output is unchanged for simple inputs (the fix only skips redundant work, not work that affects the result)
- Verifying MDX syntax is valid by checking no fences or tags were broken

---

## Implementation Notes

### Week 2 Progress

**What I built:**

Fixed the BFS implementation in `solutions/gold/usaco-671.mdx` for both the C++ and Python code blocks in the Implementation section.

The fix erases each coordinate entry from the map after processing it (`lines[dir].erase(it)` in C++, `.pop()` in Python). This guarantees each coordinate list is iterated at most once across the entire BFS, reducing worst-case cost from O(N^2) to O(N log N) with a sorted `map`.

After maintainer feedback (bqi343, eysbutno), the C++ implementation was also switched from `unordered_map` to `map` to avoid hash collision worst cases, and the time complexity annotation was updated to O(N log N). The Python pop was moved to a separate line before the for loop for readability.

**Files modified:**
- `solutions/gold/usaco-671.mdx`
  - C++: changed `#include <unordered_map>` to `#include <map>`
  - C++: changed `unordered_map<int, vector<int>> lines[2]` to `map<int, vector<int>> lines[2]`
  - Python: moved `.pop()` to a separate `neighbors = ...` line before each for loop
  - Updated time complexity annotation from `O(N)` to `O(N \log N)`

**Key commits:**
- [f8b9d6d - Fix wrong time complexity in usaco-671 BFS solution](https://github.com/krishishah05/usaco-guide/commit/f8b9d6d)
- [4aecb48 - Use erase instead of processed set, update complexity to O(N)](https://github.com/krishishah05/usaco-guide/commit/4aecb48)
- [409915d - Switch unordered_map to map, update complexity to O(N log N), improve Python readability](https://github.com/krishishah05/usaco-guide/commit/409915d)

**Challenges faced:**

The main challenge was getting the time complexity right. The initial fix used a `processed` set which added O(log N) overhead per operation. Copilot flagged that, leading to the cleaner erase approach. Then bqi343 pointed out that `unordered_map` has hash collision worst cases, so switching to `map` and annotating O(N log N) was the final correct answer.

---

## Pull Request

**PR Link:** https://github.com/cpinitiative/usaco-guide/pull/6220

**PR Description:** Fixed the BFS implementation in `solutions/gold/usaco-671.mdx` for both C++ and Python. The code claimed O(N) time complexity but had an O(N²) worst case: when multiple nodes share a coordinate and all get added to the queue, each one re-iterates the full coordinate list even though those entries are already visited. The fix adds a `processed` set tracking `(direction, coordinate)` pairs and skips the inner loop if that pair was already handled. Updated the time complexity annotation from O(N) to O(N log N). Closes #6058.

**Maintainer Feedback:**
- eysbutno approved, requested Python readability fix and switch to regular map with O(N log N) annotation
- bqi343 flagged unordered_map hash collision issue
- Both addressed in third commit (409915d)

**Status:** Merged

---

## Learnings & Reflections

### Technical Skills Gained

- Learned how BFS time complexity can be deceptive: the `dist` check prevents revisiting nodes, but without erasing processed coordinate lists, you can still hit O(N^2) by re-iterating the same list from multiple queue entries.
- Learned the tradeoff between `unordered_map` and `map` in competitive programming: unordered_map has O(1) average but can degrade with hash collisions, while map gives a reliable O(log N) per operation. For this problem, `map` + erase gives O(N log N) worst case with no hidden constants.
- Got hands-on experience with the full open source contribution workflow: forking, branching, opening a PR, responding to review comments, and pushing follow-up commits.
- Learned how to read and navigate a large real-world codebase (usaco-guide) and make a targeted change without breaking unrelated content.

### Challenges Overcome

The hardest part was getting the time complexity annotation exactly right. My first fix used a `processed` set to track `(direction, coordinate)` pairs, which worked but added O(log N) overhead per insertion. A reviewer (Copilot) flagged it, so I switched to erasing the map entry directly after processing, which is both simpler and faster. Then a second reviewer (bqi343) pointed out that `unordered_map` itself has hash collision worst cases, so I switched to a regular `map` and updated the annotation to O(N log N). Each round of feedback pushed the solution to be more correct and cleaner.

### What I'd Do Differently Next Time

I would read the maintainer comments on similar past PRs before writing my first version. The erase approach was actually more natural than the set approach, and I might have started there if I had looked at how the codebase handled similar patterns. I also would have caught the `unordered_map` issue earlier by thinking through all the data structure choices upfront instead of just focusing on the algorithmic fix.

---

## Resources Used

- [USACO Guide Contributing Docs](https://github.com/cpinitiative/usaco-guide/blob/master/CONTRIBUTING.md)
- [USACO Gold 2016 December Lasers and Mirrors Official Analysis](http://www.usaco.org/current/data/sol_lasers_gold_dec16.html)
- [cpinitiative/usaco-guide Issue #6058](https://github.com/cpinitiative/usaco-guide/issues/6058)
- [Merged PR #6220](https://github.com/cpinitiative/usaco-guide/pull/6220)
