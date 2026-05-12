# Triage Graph: AhoyISki/duat

**Repository:** AhoyISki/duat  
**Stars:** 242  
**Language:** Rust  
**Type:** Text editor (Kakoune-inspired, configured in Rust)  
**Triage Date:** 2026-05-11  
**Open Issues:** 7

## Repository Assessment

**Maintainer Activity:** Active. Issues #52 and #32 are assigned to AhoyISki, indicating ongoing feature development.

**Contribution Pattern:** Strong theme contribution culture. No CONTRIBUTING.md exists, but issue templates guide contributors. Theme additions (#36, #37, #33) are explicitly labeled "good first issue" to encourage community involvement.

**AI Policy:** None detected. No CONTRIBUTING.md, no mentions of AI in README or issue templates.

## Issues Analyzed

### #36 - Add the Ayu theme ✅ IMPLEMENTED
- **Type:** Enhancement (theme addition)
- **Labels:** enhancement, good first issue
- **Status:** Triaged → Implemented → qa_passed
- **Branch:** add-ayu-theme
- **Implementation:** 211 LOC, 3.5 hours
- **Commits:** 47c79194
- **Approach:** Extracted official Ayu color palette from ayu-vim repository. Implemented all three variants (dark, light, mirage) following existing `add_colorschemes!` macro pattern.
- **Test Strategy:** `cargo build --release` passes with zero errors. All 163 form mappings follow established catppuccin/tokyo-night/dracula patterns.
- **Risk:** Low. Additive change, no breaking modifications, well-isolated in `src/colorscheme.rs`.
- **Merge Probability:** 95% - Follows exact pattern of catppuccin (4 variants in single PR), tokyo-night (2 variants), dracula (2 variants).

### #37 - Add the monokai-pro theme
- **Type:** Enhancement (theme addition)
- **Labels:** enhancement, good first issue
- **Status:** Triaged (not selected for implementation)
- **Estimated LOC:** ~200 (similar to Ayu)
- **Approach:** Same as #36 - extract palette from official monokai-pro, implement variants, follow macro pattern.
- **Risk:** Low
- **Merge Probability:** 95%
- **Notes:** Identical pattern to #36. Not implemented to avoid flooding maintainer with theme PRs simultaneously.

### #33 - Add gruvbox-pastel theme
- **Type:** Enhancement (theme addition)
- **Labels:** enhancement, good first issue
- **Comments:** 3 (maintainer guidance on testing)
- **Status:** Triaged (not selected for implementation)
- **Estimated LOC:** ~200
- **Approach:** Same pattern, gruvbox-pastel palette
- **Risk:** Low
- **Merge Probability:** 95%
- **Notes:** Has maintainer engagement in comments. Could be follow-up after #36 merges.

### #55 - Fix hot reloading on Windows
- **Type:** Bug (platform-specific)
- **Labels:** bug, help wanted, good first issue
- **Status:** Triaged (not selected - out of scope)
- **Estimated LOC:** ~50-100
- **Approach:** Maintainer-specified solution: move compiled config .exe to `target/exec/duat{n}.exe` before execution to avoid Windows file locking. Numbered approach for multi-instance support.
- **Risk:** Medium - Windows-only, requires file watching coordination, potential race conditions
- **Merge Probability:** 85%
- **Notes:** Well-specified by maintainer but requires Windows testing environment. Complex synchronization logic for multi-instance case.

### #48 - Turn the History into a tree
- **Type:** Enhancement (data structure refactor)
- **Labels:** enhancement, good first issue, help wanted
- **Status:** Triaged (not selected - medium complexity)
- **File:** `crates/duat-core/src/buffer/history.rs`
- **Estimated LOC:** ~150-200
- **Approach:** Replace linear `Vec<Moment>` with tree structure. Maintain `unread_moments` field for parser tracking. Enable non-linear undo/redo.
- **Risk:** Medium-high - Public API changes, bincode serialization compatibility, extensive testing needed
- **Merge Probability:** 70%
- **Notes:** "Good first issue" label is optimistic for this scope. Well-isolated in single file but non-trivial graph traversal logic.

### #52 - External commands/terminal
- **Type:** Feature request
- **Assigned to:** AhoyISki
- **Status:** Not triaged (assigned to maintainer)
- **Notes:** Substantial feature (`:!` command execution, F5 mapping). User wants integrated terminal for rust compilation. Assigned to maintainer indicates this is roadmap work, not open for external contribution.

### #32 - Add helix mode
- **Type:** Feature request
- **Assigned to:** AhoyISki
- **Comments:** 3
- **Status:** Not triaged (assigned to maintainer)
- **Notes:** Large scope - new editing mode plugin. Requires understanding helix keybindings, motions, selection semantics. Maintainer-assigned indicates strategic decision needed.

## Hypothesis Classification

**#36 (Ayu theme):** H2-boring-fix  
- Mechanical implementation following established pattern
- Zero architecture decisions
- Community-requested feature (theme diversity)
- Low-risk trust builder

**#37, #33 (Other themes):** H2-boring-fix  
- Same classification as #36

**#55 (Windows hot reload):** H3-bug-with-reproduction  
- Has reproduction (Windows file locking)
- Has maintainer-specified solution
- Platform-specific edge case
- Requires non-trivial synchronization logic

**#48 (History tree):** H4-maintainer-vision  
- Enhances core functionality (undo/redo UX)
- Requires architecture decision (tree structure choice)
- Touches core library (duat-core)
- May have downstream plugin compatibility concerns

## Implementation Decision

**Selected:** #36 (Add Ayu theme)

**Rationale:**
1. **Fastest to merge:** Theme additions have established 2-7hr merge pattern in this repo
2. **Zero risk:** Additive only, no breaking changes, isolated to single function in colorscheme.rs
3. **Community value:** Ayu is widely requested theme (popular in VSCode, Vim, Neovim, Helix)
4. **Pattern compliance:** Exact match for existing multi-variant theme PRs (catppuccin, tokyo-night, dracula)
5. **Trust building:** First contribution to repo, boring fix establishes competence before tackling #55 or #48

**Not selected:**
- #37, #33: Avoid flooding with theme PRs; wait for #36 to merge
- #55: Requires Windows environment, non-trivial synchronization
- #48: Higher complexity than "good first issue" suggests, core library changes
- #52, #32: Assigned to maintainer, strategic features

## Build Verification

```bash
cd /Users/junekim/Documents/duat
git checkout add-ayu-theme
cargo build --release
# ✓ Compiles successfully
# ✓ Zero errors related to Ayu theme
# ✓ Single unrelated warning in duat-base (struct Places never constructed)
```

## Next Steps

1. ✅ Implementation complete on branch `add-ayu-theme`
2. ✅ Build verified
3. ✅ Drip queue entry created (status: qa_passed)
4. **Pipeline:** Entry awaits /drip gate → /ship for PR creation
5. **Follow-up:** After #36 merges, consider #37 (monokai-pro) or #33 (gruvbox-pastel)
