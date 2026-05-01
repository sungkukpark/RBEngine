# CLAUDE.md

This file defines the repository-local rules Claude must follow when working
in the `RBEngine` repository.

RBEngine is a long-lived rendering and game engine intended to survive many
hardware generations, graphics APIs, platforms, tools, and company projects.
The commit log is part of the engine architecture. Treat it as a permanent
engineering archive, not as a temporary chat log, changelog generator, or task
tracker.

RBEngine does **not** use Conventional Commits. RBEngine uses a
subsystem-first patch style derived from long-lived native-code projects:
small coherent patches, meaningful subsystem prefixes, imperative one-line
subjects, and commit bodies that explain the engineering reason behind the
change.

---

## 1. Non-Negotiable Rule

Claude must never create a commit unless the commit message satisfies this
file.

If the requested message is vague, incorrectly scoped, non-atomic, misleading,
not in English, or missing required rationale, Claude must rewrite it before
committing.

If the staged diff contains unrelated work, Claude must not hide that fact in a
broad commit message. Split the changes or ask for direction.

---

## 2. Commit Message Doctrine

Every commit must be useful to an engineer reading it years later while doing
one of the following:

- scanning `git log --oneline`;
- reviewing a patch;
- investigating a regression with `git bisect`;
- reading `git blame` on a subtle system;
- backporting a bug fix;
- understanding an architectural decision;
- porting RBEngine to a new platform, renderer, compiler, or asset pipeline.

A good RBEngine commit message answers two questions:

1. **What engine area changes, and what does applying this patch do?**
2. **Why is this change the correct engineering move?**

The diff shows what changed. The commit message must preserve the rationale
that the diff cannot show.

---

## 3. Required Format

Use this format for every non-merge commit:

```text
<Area>[/<Component>[/<Detail>]]: <Imperative summary>
```

Extended form:

```text
<Area>[/<Component>[/<Detail>]]: <Imperative summary>

<body>

<trailers>
```

Examples:

```text
Repo: Initialize RBEngine repository
Core/Memory: Add tagged allocation tracking
RHI/Vulkan: Recreate swapchain after surface resize
RenderGraph: Validate imported texture lifetimes
Renderer/Visibility: Cache frustum planes per camera
Shader/PBR: Clamp roughness before GGX evaluation
Asset/Pipeline: Track source hash in cooked texture metadata
Build/CMake: Add shader compilation target
```

---

## 4. Why RBEngine Rejects Conventional Commits

Do not use these forms:

```text
feat(renderer): add clustered lighting
fix(rhi): handle swapchain resize
refactor(core): clean up allocator
chore(repo): update files
```

Conventional Commits optimize for semantic versioning and generated release
notes. RBEngine optimizes for long-term subsystem ownership, architecture
review, regression diagnosis, backporting, and performance archaeology.

The change kind belongs in the verb, not in a fixed type taxonomy:

```text
Renderer/Lighting: Add clustered light culling
RHI/Vulkan: Fix swapchain recreation after resize
Core/Memory: Refactor allocator ownership tracking
Renderer/Visibility: Optimize frustum plane caching
Shader/PBR: Remove legacy Blinn-Phong evaluation path
```

---

## 5. Language Rule

Commit messages must be written in English.

Use precise technical English. Do not mix Korean and English unless the patch
changes Korean documentation, localization data, Korean comments, or
user-facing Korean text.

Good:

```text
RenderGraph: Add pass dependency validation
```

Bad:

```text
RenderGraph: 패스 의존성 검증 추가
```

---

## 6. Subject Line Rules

The first line is the subject. It must be independently meaningful because it is
shown by shortlog tools, review tools, IDE history views, and blame UIs.

The subject must:

- use the required `<Area>[/<Component>[/<Detail>]]: <Summary>` format;
- use an imperative verb after the colon;
- describe what applying the commit does;
- identify the actual engine concept being changed;
- avoid vague terms such as `update`, `cleanup`, `misc`, `changes`, `fix bug`,
  `work`, `stuff`, or `initial`;
- omit the final period;
- stay within 72 characters when practical;
- never exceed 90 characters without a strong readability reason.

Good:

```text
RenderGraph: Cull unused passes before allocation
RHI: Normalize image barrier descriptions
Shader/PBR: Clamp roughness before GGX evaluation
Platform/Windows: Route raw mouse input through event queue
```

Bad:

```text
Renderer: Update
RHI: Fix bug
Core: Cleanup
Shader: Changes
Build: Stuff
```

---

## 7. Prefix Rules

### 7.1 Area is required

Every commit must begin with a stable engine area prefix.

Good:

```text
RHI: Add backend-agnostic buffer descriptions
```

Bad:

```text
Add backend-agnostic buffer descriptions
```

### 7.2 Prefixes describe ownership, not file paths

The prefix should name the logical owner of the invariant being changed.
It is not necessarily the directory name.

Examples:

```text
RenderGraph: Track transient resource aliasing candidates
RHI/Vulkan: Convert image usage flags to VkImageUsageFlags
Asset/Pipeline: Store source import settings in resource metadata
```

### 7.3 Use the narrowest stable prefix

Prefer the most specific subsystem that still makes sense to a future engine
programmer.

Good:

```text
Renderer/Visibility: Compact visible mesh instance lists
```

Acceptable only for cross-cutting renderer orchestration:

```text
Renderer: Split scene extraction from draw submission
```

Too broad:

```text
Engine: Compact visible mesh instance lists
```

### 7.4 Prefer one primary prefix

If a change touches multiple areas, choose the area whose contract or invariant
is primarily changed. Explain secondary effects in the body.

Good:

```text
RHI: Add explicit queue ownership transfer metadata
```

Bad:

```text
RHI+Renderer+Vulkan: Add queue ownership transfer metadata
```

### 7.5 Prefix casing

Use stable engine-style casing for prefixes.

Good:

```text
RenderGraph: Add pass culling
RHI/Vulkan: Add image barrier conversion
Core/Memory: Add allocation tags
Shader/PBR: Fix Smith masking term
```

Bad:

```text
render-graph: Add pass culling
rhi(vulkan): Add image barrier conversion
core_memory: Add allocation tags
shader/pbr: Fix Smith masking term
```

---

## 8. Canonical Prefix Map

Claude should prefer these prefixes unless the repository architecture clearly
introduces a better stable name.

### Repository, documentation, and process

- `Repo`
- `Docs`
- `Docs/ADR`
- `CI`
- `Build`
- `Build/CMake`
- `Deps`
- `Tools`
- `Tests`

### Engine foundation

- `Core`
- `Core/Memory`
- `Core/Containers`
- `Core/String`
- `Core/Time`
- `Core/Config`
- `Core/Logging`
- `Core/Profiling`
- `Core/Debug`
- `Math`
- `Platform`
- `Platform/Windows`
- `Platform/Linux`
- `Platform/macOS`
- `Threading`
- `Jobs`
- `TaskGraph`
- `Reflection`
- `Serialization`

### Runtime and gameplay foundation

- `App`
- `Loop`
- `Scene`
- `World`
- `ECS`
- `Object`
- `Events`
- `Input`
- `UI`
- `Audio`
- `Physics`
- `Animation`
- `AI`
- `Scripting`
- `Network`

### Assets, resources, and tools

- `Asset`
- `Asset/Pipeline`
- `Asset/Importer`
- `Asset/Cook`
- `Resource`
- `Filesystem`
- `Cache`
- `Editor`
- `Editor/Viewport`
- `Editor/Inspector`

### Rendering architecture

- `Renderer`
- `Renderer/Visibility`
- `Renderer/Lighting`
- `Renderer/Frame`
- `Renderer/Scene`
- `Renderer/Debug`
- `RenderGraph`
- `RHI`
- `RHI/Vulkan`
- `RHI/D3D12`
- `RHI/Metal`
- `Shader`
- `Shader/PBR`
- `Shader/Lighting`
- `Shader/Shadow`
- `Material`
- `Texture`
- `Mesh`
- `GPUScene`
- `Shadow`
- `PostFX`
- `RayTracing`
- `GI`

---

## 9. Imperative Verb Rule

The summary after the prefix must read as an instruction to the codebase.

Recommended verbs:

- `Add`
- `Fix`
- `Remove`
- `Rename`
- `Move`
- `Split`
- `Merge`
- `Refactor`
- `Optimize`
- `Validate`
- `Clamp`
- `Track`
- `Cache`
- `Expose`
- `Hide`
- `Replace`
- `Defer`
- `Document`
- `Test`
- `Revert`

Prefer domain-specific verbs when they are clearer:

```text
RenderGraph: Cull unused passes before allocation
RHI/Vulkan: Transition swapchain images after acquire
Shader/Lighting: Normalize area light basis vectors
Renderer/Visibility: Compact visible mesh instance lists
Asset/Cook: Deduplicate identical texture payloads
```

---

## 10. Body Requirement

A body is required when the commit is not self-explanatory from the subject
alone.

A body is mandatory for commits that affect:

- architecture or subsystem ownership;
- public engine APIs;
- memory ownership, lifetime, allocators, or object identity;
- threading, jobs, synchronization, atomics, lock-free structures, or ordering;
- rendering output, visibility, lighting, shadows, materials, or shader math;
- RHI abstractions or backend behavior;
- asset formats, cooked data, import settings, or migration paths;
- serialization formats or resource identifiers;
- build system behavior, compiler flags, or dependency resolution;
- performance, memory footprint, loading time, frame time, or GPU cost;
- a subtle bug whose cause is not obvious from the diff;
- a temporary limitation or deliberate tradeoff.

For trivial commits, a one-line message is acceptable.

---

## 11. Body Structure

The body should explain the rationale, not repeat the diff.

Use this structure for non-trivial changes:

```text
Problem:
<What is wrong, missing, unstable, slow, misleading, or hard to maintain?>

Decision:
<What design or implementation direction does this patch take, and why?>

Impact:
<What changes for callers, assets, runtime behavior, performance, tools,
serialization, debugging, or future work?>

Validation:
<What build, test, capture, profile, replay, or manual check was run?>
```

The labels are optional for small commits, but mandatory for architectural,
performance-sensitive, concurrency-sensitive, asset-format, and RHI/backend
changes.

Do not fabricate validation. If no validation was run, either omit the
`Validation:` section for simple changes or state the truth when validation is
important.

---

## 12. Body Style Rules

The body must:

- wrap around 72 to 74 columns when practical;
- explain why the change exists;
- justify the chosen solution;
- mention alternatives rejected when they matter;
- mention tradeoffs, risks, assumptions, and follow-up constraints;
- use complete technical sentences;
- avoid marketing language;
- be understandable without requiring external links.

Good:

```text
RHI/Vulkan: Defer descriptor pool reset until frame retirement

Problem:
Descriptor pools are reset immediately after command submission, but command
buffers can still reference descriptors until the frame fence completes.

Decision:
Move pool reset into the frame retirement path so descriptor lifetimes match
GPU execution lifetime.

Impact:
This increases peak descriptor pool memory slightly, but removes a lifetime
hazard that can surface under GPU stalls.

Validation:
Ran the Vulkan smoke test and captured three frames in RenderDoc after a
forced swapchain stall.
```

Bad:

```text
RHI/Vulkan: Fix descriptors

Fixed descriptor bug.
```

---

## 13. Atomic Commit Rule

Each commit must represent one logical change.

A commit is atomic when a reviewer can answer yes to all of these:

- Does the subject describe the whole patch?
- Can the change be reviewed on its own?
- Can the change be reverted on its own?
- Can the change be bisected on its own?
- Does the repository remain in a coherent state after applying it?

Do not combine unrelated changes just because they were made in the same
working session.

Bad:

```text
Renderer: Add render graph and update editor icons
```

Good:

```text
RenderGraph: Add pass dependency validation
Editor/Viewport: Adjust toolbar icon spacing
```

---

## 14. Bisectability Rule

Every commit on a protected branch should leave the repository in a coherent
state.

For non-trivial code changes, the commit should preserve:

- build validity;
- test validity;
- shader compilation validity;
- runtime startup validity;
- backend initialization validity;
- serialization compatibility or a documented migration path;
- public API consistency.

A patch series may be split into multiple commits, but each commit should
still build and be explainable on its own. Do not hide a broken intermediate
state in the middle of a series.

---

## 15. Engine Architecture Commit Rules

### 15.1 Architecture changes

Architecture commits must describe ownership, lifetime, and dependency
direction.

Example:

```text
RenderGraph: Split pass declaration from execution

Problem:
Pass setup and command recording are coupled, which prevents the graph from
validating resource lifetimes before backend execution begins.

Decision:
Store pass declarations first, resolve dependencies, then execute backend
callbacks after graph validation.

Impact:
This prepares transient resource aliasing and async compute scheduling without
coupling validation to the Vulkan backend.
```

### 15.2 Public API changes

Public API changes must mention callers and migration.

```text
RHI: Replace raw buffer IDs with typed handles

Problem:
Raw integer buffer IDs can be mixed with texture and sampler IDs at API
boundaries.

Decision:
Introduce `BufferHandle` and return it from buffer creation.

Impact:
Callers must store typed handles directly instead of forwarding raw `u32`
identifiers.

Breaking-Change: Buffer creation now returns BufferHandle instead of u32.
Migration: Update call sites to store BufferHandle directly.
```

### 15.3 Rendering output changes

Rendering output changes must mention the visual invariant being changed and
how it was checked.

```text
Shader/PBR: Clamp roughness before GGX evaluation

Problem:
Materials with near-zero roughness can produce NaNs in the GGX visibility term
on some GPUs.

Decision:
Clamp perceptual roughness before converting it to alpha for BRDF evaluation.

Impact:
Mirror-like materials remain visually sharp while avoiding NaN propagation in
lighting accumulation.

Validation:
Compared the material test scene before and after the change on Vulkan.
```

### 15.4 Performance changes

Performance commits must quantify the result when possible and state the
test environment.

```text
Renderer/Visibility: Cache frustum planes per camera

Problem:
Visibility culling recomputes frustum planes for every mesh batch even when the
camera is unchanged for the frame.

Decision:
Cache the planes in the per-view visibility context and reuse them for all batch
culling jobs.

Impact:
The visibility phase drops from 0.42 ms to 0.31 ms in the Sponza stress scene
on Ryzen 9 7950X with 100k mesh instances.

Validation:
Profiled five warm frames with the built-in CPU profiler in Release.
```

### 15.5 Concurrency changes

Concurrency commits must describe ordering, lifetime, and synchronization
assumptions.

```text
Jobs: Defer worker sleep until queue drain is observed

Problem:
Workers can sleep after observing an empty local queue while another thread is
publishing work to the global queue.

Decision:
Require workers to observe the global queue drain counter before entering the
sleep path.

Impact:
This adds one atomic load to the idle path, but prevents missed wakeups during
burst submission.
```

### 15.6 Asset and serialization changes

Asset format, cooked data, and serialization changes must mention migration,
compatibility, and cache invalidation.

```text
Asset/Pipeline: Store source hash in cooked texture metadata

Problem:
Cooked textures do not record the source content hash, so the pipeline cannot
distinguish stale output from unchanged import settings.

Decision:
Write the source hash into the cooked metadata header and include it in cache
validation.

Impact:
Existing cooked texture caches must be regenerated once after this commit.

Migration: Delete `DerivedData/Textures` before the next full cook.
```

---

## 16. Trailer Rules

Use trailers for structured metadata that should be searchable or parseable.

Allowed trailers:

```text
Fixes: <12+ hex> ("<original subject>")
Refs: #123
Closes: #123
Reported-by: Name <email@example.com>
Reviewed-by: Name <email@example.com>
Tested-by: Name <email@example.com>
Co-authored-by: Name <email@example.com>
Signed-off-by: Name <email@example.com>
Breaking-Change: <description>
Migration: <description>
```

Rules:

- Use `Fixes:` only when the bug is traced to a specific earlier commit.
- Include both the hash and the original subject when referencing a commit.
- Use at least 12 hex characters for commit hashes.
- Do not wrap parseable trailer lines unless unavoidable.
- Do not add fake issue references.
- Do not add `Signed-off-by:` unless the project requires DCO sign-off or the
  user requests it.
- Do not add AI attribution trailers by default.

---

## 17. Revert Rule

Reverts must be explicit and must explain why the revert is necessary.

Format:

```text
Revert/<Area>: Revert <original summary>
```

Example:

```text
Revert/RenderGraph: Revert transient resource aliasing

This reverts commit abc1234def56 ("RenderGraph: Add transient resource aliasing").

Problem:
The aliasing pass can reuse a transient color target while it is still read by a
later async compute pass.

Decision:
Revert the aliasing implementation until cross-queue lifetime validation is in
place.
```

---

## 18. Merge and Squash Rules

RBEngine prefers clean, reviewable, bisectable history on protected branches.

- Do not create merge commits unless the user explicitly requests one.
- Do not squash unrelated changes into one broad commit.
- If GitHub squash merge is used, the final squash title must follow this file.
- Engine core, renderer, RHI, serialization, asset-format, and threading work
  should preserve meaningful commit boundaries whenever possible.
- Feature branch checkpoint commits may exist temporarily, but they must not be
  merged into protected branches as-is.

---

## 19. Checkpoint Commit Rule

Claude must not create WIP commits by default.

Prohibited:

```text
WIP
wip: render graph
Renderer: WIP
Temp commit
Save work
```

Checkpoint commits are allowed only when the user explicitly requests one.

Format:

```text
Checkpoint/<Area>: Save current work on <topic>
```

Example:

```text
Checkpoint/RenderGraph: Save current work on transient resources
```

Checkpoint commits should be squashed, rewritten, or replaced before merging
into a protected branch.

---

## 20. Initial Commit Rule

The first repository commit should be:

```text
Repo: Initialize RBEngine repository
```

If the first commit includes the commit doctrine only:

```text
Docs/Repo: Define RBEngine commit message doctrine
```

If the first commit includes a minimal engine skeleton:

```text
Core: Add initial engine skeleton
```

If both repository metadata and engine code are included, prefer splitting them:

```text
Repo: Initialize repository metadata
Core: Add initial engine skeleton
```

---

## 21. Prohibited Messages

Never use vague or throwaway messages.

Prohibited examples:

```text
update
fix
wip
misc
changes
cleanup
save
work in progress
commit
initial
stuff
asdf
```

Also prohibited:

```text
Renderer: Update
RHI: Fix bug
Core: Cleanup
Shader: Changes
Build: Stuff
Engine: Work
Repo: Initial
```

The subject must identify the actual engine change.

---

## 22. Claude Commit Workflow

Before committing, Claude must:

1. Inspect the staged diff.
2. Identify whether the staged files represent one logical change.
3. Select the narrowest stable prefix from the canonical prefix map.
4. Write an imperative subject that describes the effect of applying the patch.
5. Decide whether a body is required.
6. For non-trivial changes, explain the problem, decision, impact, and
   validation.
7. Add trailers only when they are true and useful.
8. Verify that the repository is not knowingly left in a broken state.
9. Verify that the final commit message satisfies this file.

Claude must not commit if:

- the staged diff contains unrelated changes;
- the prefix is missing;
- the prefix is too broad;
- the summary is vague;
- the message uses Conventional Commit syntax;
- the message is not in English;
- a non-trivial change lacks rationale;
- a breaking change lacks a `Breaking-Change:` trailer;
- validation is claimed but was not performed;
- the patch knowingly leaves a protected branch unbuildable.

---

## 23. Decision Table

| Change kind | Preferred subject |
|---|---|
| Repository creation | `Repo: Initialize RBEngine repository` |
| Commit doctrine | `Docs/Repo: Define RBEngine commit message doctrine` |
| New core system | `Core: Add application bootstrap layer` |
| Memory work | `Core/Memory: Add tagged allocation tracking` |
| Render graph capability | `RenderGraph: Add pass dependency validation` |
| Renderer orchestration | `Renderer: Split scene extraction from draw submission` |
| Visibility performance | `Renderer/Visibility: Cache frustum planes per camera` |
| Backend-agnostic RHI | `RHI: Add typed buffer handles` |
| Vulkan backend | `RHI/Vulkan: Recreate swapchain after surface resize` |
| D3D12 backend | `RHI/D3D12: Add descriptor heap allocator` |
| Shader math | `Shader/PBR: Clamp roughness before GGX evaluation` |
| Asset pipeline | `Asset/Pipeline: Track source hash in cooked texture metadata` |
| Serialization | `Serialization: Add version tag to resource headers` |
| Build system | `Build/CMake: Add shader compiler target` |
| Tests | `Tests/Math: Add matrix inverse precision cases` |
| Documentation | `Docs/Renderer: Explain frame lifecycle` |
| Revert | `Revert/Renderer: Revert render graph aliasing` |
| Checkpoint | `Checkpoint/Renderer: Save current work on pass culling` |

---

## 24. Final Validation Checklist

Before creating a commit, Claude must verify:

- [ ] The message is in English.
- [ ] The subject uses `<Area>[/<Component>[/<Detail>]]: <Imperative summary>`.
- [ ] The prefix is stable, specific, and architecture-aware.
- [ ] The summary uses an imperative verb.
- [ ] The summary describes the actual effect of applying the patch.
- [ ] The summary does not end with a period.
- [ ] The message does not use Conventional Commit syntax.
- [ ] The commit is atomic.
- [ ] The repository remains in a coherent state.
- [ ] A body exists when rationale is needed.
- [ ] The body explains why, not just what.
- [ ] Tradeoffs and alternatives are mentioned when relevant.
- [ ] Performance claims include numbers and environment when possible.
- [ ] Rendering-output changes include validation context when possible.
- [ ] Asset or serialization changes include migration when needed.
- [ ] Breaking changes use `Breaking-Change:`.
- [ ] Trailers are true, useful, and parseable.
- [ ] No validation is claimed unless it was performed.

If any item fails, Claude must fix the commit message before committing.
