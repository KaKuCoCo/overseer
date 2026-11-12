# Changelog

What changed in each Overseer release, newest first. Settings › Help & Diagnostics › Changelog shows the same.

## 2.5.1-windows.1 (2026-11-12)

Overseer 2.5.1 with the Windows port carried onto it.

- Removed the Projects status line button. Use Ctrl+Alt+Shift+L on Windows or the Show or hide Projects command.

## 2.5.1 (2026-11-11)

### Removed

- The Usage tab in Settings. It priced sessions at API rates, which says nothing about how much of a subscription you've used, and it counted forked sessions twice. Settings is a single page again

## 2.5.0 (2026-11-11)

### Added

- **Usage in Settings**: Settings has two tabs, General and Usage. Usage shows what your omp sessions cost Today, over 7 days or over 30 days, by project, worktree and model, with the most expensive sessions

### Changed

- Settings' first section is called Panel

## 2.4.5 (2026-11-11)

### Added

- **Update** beside the cog in the Projects header when a newer Overseer is out: it updates Overseer in a terminal tab (`tern plugin install … --force`, or `git pull --ff-only` in a linked checkout), and a notification says so once the new version loads. Overseer checks every hour, and on ↻

## 2.4.4 (2026-11-11)

### Changed

- An agent that finished while you were elsewhere shows a green check alone, no longer a white check in a green circle that looked like an open session's dot
- Settings is titled Overseer Settings

### Fixed

- Tab dots are round: they were drawn slightly taller than wide
- A working agent's tab dot glows instead of a ring that sat off its centre
- A busy terminal's tab dot is the small amber dot instead of a full-size one

## 2.4.3 (2026-11-11)

Requires Tern 0.9.15

### Added

- **Settings**: the cog in the Projects header, or Overseer settings in the palette, opens a tab with the Ice Box switch, each project's base branch, Linear workspace and agent coordination, and your GitHub and Linear sign-ins with Install and Connect buttons
- **Changelog** in Settings › Help & Diagnostics
- Buttons on Overseer's notifications while Projects is showing: Open on a created worktree, Answer on a setup that may need you, Show on a failed creation or deletion, Close session when a deletion left its session open, and Undo on a removed project
- The Ice Box can be turned off in Settings › General
- Diagnostics to copy for a bug report in Settings › Help & Diagnostics

### Changed

- The operation log opens over the tab you're in, as a table, newest first, instead of in `less` in a new tab
- The Projects panel redraws in about 4 ms, down from as much as 40 ms while agents were busy

## 2.4.2 (2026-11-09)

### Fixed

- The Projects panel keeps the width you gave it in tabs that already have splits, instead of arriving at the wrong width and forgetting changes; thanks to [@cxxzsh](https://github.com/cxxzsh) ([#5](https://github.com/mmnavarr/overseer/pull/5))

## 2.4.1-windows.1 (2026-11-08)

Overseer 2.4.1 with Malcolm Navarro's Windows port (`windows-support`) carried onto it. Install it from [KaKuCoCo/overseer](https://github.com/KaKuCoCo/overseer); see the README.

- **Overseer runs on Windows.** **+** opens Explorer's folder dialog; drive-letter and UNC paths work, and linked checkouts and junctions resolve to the same project. Worktrees are listed, created and deleted (Force delete included) through Worktrunk's `git-wt.exe`, never Windows Terminal's `wt.exe`, found on PATH or in Cargo's or Winget's folders. Creation and deletion run a PowerShell helper in a visible terminal with your hook approvals intact; its arguments are passed encoded, so paths and branch names with spaces, Unicode, apostrophes or `$` stay literal. Deletion rechecks the worktree's identity on physical paths and still keeps the branch. Sessions, agent status, sub-worktrees and their briefs, coordination and Visualize PR use Windows paths too.
- **Agents use `git-wt` on Windows.** The `overseer-subworktree` script runs Worktrunk as `git-wt` under Git Bash, and the skill and rule tell agents to type `git-wt` there.
- **Ctrl+Alt+Shift+L shows Projects on Windows.** Tern reads `cmd` in a chord as the Windows key on a PC keyboard, so ⌘⌥⇧P meant Win+Alt+Shift+P there, and Ctrl+Alt+Shift+P is Tern's own Picture in picture. macOS keeps ⌘⌥⇧P.
- **Hiding Projects gives focus back.** Closing the panel with the shortcut let Tern focus whichever pane it picked, often the tab's last one. Focus now returns to the pane you were in when you opened the panel, as Tern's own Files panel does.
- **Projects opens in one step.** The panel opened at half the focused pane's width, then moved to the left and resized, so the terminals beside it redrew two or three times. It now opens left of everything at its saved width in the same call (`cx.canvas:open` with `target`, `dir` and `ratio`).
- **Fixed: the Projects shortcut sometimes did nothing for one to three seconds.** The command only queued its work on a 0 ms timer, and Tern fires a queued plugin timer when the window next has an event, so after a lone keypress the toggle waited for the next mouse move or key (measured 1.6–3.1 s). The shortcut now toggles in the command itself; the toggle takes 1–7 ms.
- **Fixed: updating on Windows while Overseer runs.** `tern plugin install --force` failed with "file in use" (os error 32): Overseer's commands, among them the 5-second clock and an open folder picker, ran inside its own folder, and Windows won't replace a folder a process runs in. They now run in the Windows folder unless they need a repository's.
- **Fixed: Overseer relinked its omp half on every load on Windows.** omp lists the junction it made in its own plugins folder, not the folder it leads to, so the paths never matched. Overseer now reads where the junction leads before comparing.
- **Fixed: Show Overseer's operation log on Windows.** It ran `less` through your shell, and Git for Windows keeps `less` off PATH, so the tab only said the command wasn't found. Windows now opens the log in Tern's file view, at its newest line.
- **Fixed: Ctrl+A and Ctrl+C in Overseer's forms on Windows.** The form's text field took Cmd for select all and copy, which Tern reads as the Windows key on a PC, so Ctrl+A did nothing there. Off macOS they use Ctrl. Paste is Ctrl+Shift+V, as in Tern's terminals: Tern hands a plugin's form a paste only through its own paste command.
- **Fixed: deleting a worktree whose session is open on Windows.** Windows can't remove a folder a program runs in, and the session's shell runs in the worktree, so `wt remove` deleted the files and Git's record but stopped at the empty folder with "Permission denied": the row vanished and its session stayed open. On Windows Overseer now closes the worktree's session just before the removal starts.
- **Fixed: a worktree removed outside Overseer while its session was open on Windows.** `wt remove` in another terminal deletes the files and Git's record but can't remove the folder the session's shell runs in, and Overseer only flagged a worktree whose folder was gone, so the row vanished and its session was left with no way to close it from Projects. A worktree whose checkout is gone (no `.git` left) now shows **Worktree gone** with **Close session**, as on macOS.
- **Fixed: an empty quote in the pull request relay.** A sub-worktree agent's first turn can end before Tern has its last message, so its parent was told `Its last message: ""`. The relay now leaves the quote out when there is no message, as the waiting and failed relays already did.
- **Fixed: Delete all hid that it kept a sub-worktree.** Tern shows one toast at a time, and the **Kept sub-worktree** toast came about 20 ms before the parent's **Deleting worktree** toast, so it was replaced before you could read it. The family's **Worktree deleted** toasts now end with `kept <branch> (uncommitted changes)`.

## 2.4.1 (2026-11-06)

### Changed

- Waiting, finished and failed agents show as filled circles with a `?`, check or `!`, sized and centred like the other dots
- Deleting pulses red and creating pulses amber, like a working agent's green pulse, and the Deleting badge is red
- Labels start with a capital: Done, Failed, Overseer, Creating, Needs you?, Deleting, Checking, Conflicts, Worktree gone, Running, No pane
- The primary checkout's row shows just its folder, since its Primary badge already says what it is
- Confirmation cards ask their question as the title, and the agent coordination card lists what turning it on does
- The README tour, screenshots and social card show the new marks

## 2.4.0 (2026-11-06)

### Changed

- **Agent coordination is opt-in per repository**: turn it on or off from the project's right-click menu. Off, Overseer sends no wake-ups or relays and hides Brief sub-worktrees…, Make overseer session, N new, check-ins and the Overseer badge. Repositories that already used it stay on
- **No more pi-intercom or Bun**: Overseer hands its messages to agents through Tern, so wake-ups and briefs reach any agent CLI, and omp agents' messages go through a mailbox in the repository's Git directory
- An agent can no longer wait for another agent's answer; replies arrive as new messages
- pi-intercom can be uninstalled if nothing else uses it

## 2.3.3 (2026-11-06)

### Added

- **One-line install**: `tern plugin install github.com/mmnavarr/overseer`, no clone; `--force` updates it

### Changed

- A shorter README led by an animated tour; the full reference moved to AGENTS.md, with contributing notes and issue forms

### Fixed

- Coordination with Worktrunk 0.81, which renamed the fields for a worktree's commit and ahead/behind counts: parent-moved notices, finished-with-PR relays and tooltip counts work again ([#4](https://github.com/mmnavarr/overseer/issues/4))

## 2.3.2 (2026-11-06)

### Fixed

- ⌘T in a worktree's session follows Settings › New tabs and splits open: with Agent chosen it starts your agent in the worktree instead of a shell; thanks to [@sinkr](https://github.com/sinkr) ([#2](https://github.com/mmnavarr/overseer/pull/2))

## 2.3.1 (2026-11-05)

### Fixed

- Worktree creations and deletions no longer get stuck on Creating or Deleting: Overseer restarts its tick when it's more than 5 s late and logs `tick restarted`
- The Projects panel keeps following you while Tern has paused Overseer's timers, and finished jobs wrap up meanwhile
- Fewer pauses: the panel redraws only on changes and every 5 s, and reads the window's panes, tabs and agents once per draw; a draw that comes close to Tern's limit logs where its time went

## 2.3.0 (2026-11-04)

Requires Tern 0.9.9

### Added

- **Sub-worktrees**: a worktree can own others, nested under its row with tree lines and a ▾ N badge that folds them. Agents create them with the overseer-subworktree skill; Move under… and Make top-level rearrange them, and deleting an owner asks about its sub-worktrees
- **Coordination** between a worktree's sessions and its sub-worktrees': family news before each prompt, the overseer_family tool for messages, wake-ups when a sub-worktree's agent waits, fails or opens a pull request, and Brief sub-worktrees…
- **Operation log**: every creation, deletion, brief and relay with when, how long and how it ended; Show Overseer's operation log in the palette
- A new worktree shows while it's made: Creating with its time and step, and Needs you? when setup asks something or goes quiet for a minute
- omp gets Overseer's skill, rule and extension by itself: Overseer links its `overseer-omp` package into omp each time it loads
- The `overseer-worktrees` omp rule points agents that type a raw worktree command at the skill
- Worktrees removed outside Overseer show as Worktree gone while their session is still open

### Changed

- The panel uses Tern's own buttons, badges, cards and fields, and Tern's sans face for names and labels
- Right-click menus have icons and sections
- Delete worktree deletes a clean worktree at once, and lists uncommitted changes before Force delete; branches are always kept
- Close session closes without asking
- Loading, empty and error states are designed, with Refresh and Retry
- Shorter tab names: a GitHub pull request tab reads `#123 title`
- Tab states show as Done and Failed badges that a long name can't squeeze out

### Fixed

- Projects no longer fails on the temporary worktrees `fallow audit` creates

## 2.2.0 (2026-10-28)

Requires Tern 0.9.6

### Added

- **Agents that need you stand out**: a yellow ? when one asks you something, and a green check or red ! when one finished or failed while you were elsewhere
- **Remove project** on a project's right-click menu; adding the repository again brings back its names and Ice Box

### Changed

- Ticket and pull request links open in a tab of their own
- Overseer no longer reloads itself: Tern 0.9.6 pauses an overrunning hook for a moment instead of switching it off
- Processes start at once instead of one per timer callback

## 2.1.0 (2026-10-27)

### Changed

- Overseer reloads itself, at most every 30 s, when Tern switches part of it off after a slow callback; clicks, commands and tab changes only queue their work
- Callbacks of 30 ms or more are logged

### Fixed

- Forms no longer hang on Returning to Projects… or Working…, and a new worktree is never launched twice
- A finished job cut short at the wrong moment is no longer reported as failed

## 2.0.1 (2026-10-13)

### Fixed

- Projects no longer freezes, or answers clicks with `Nothing handles "…" here`, while Tern is busy: each background command starts in a callback of its own

## 2.0.0 (2026-10-09)

### Changed

- **One Projects panel per window**, following you to whichever tab you're on, instead of one per worktree session
- The width you drag it to applies to every tab and is remembered; it starts at 30%
- Forms move with the panel
- ⌘⌥⇧P shows the panel, or hides it when it has focus; the command is now Show or hide Projects
- Closing a tab's last terminal closes the tab, and the panel moves to the next one
- Close session moves the panel out first; if Tern closes the panel's session, it reopens in the next tab you see
- Each tab switch animates briefly and resizes both tabs' terminals, and in a top-and-bottom split the panel sits beside the top half
- Nothing to do when upgrading: the first run keeps one of your open panels and closes the rest

## 1.10.2 (2026-10-08)

### Changed

- Slightly smaller type across the panel and forms, a smaller Projects heading, and no divider under the header

## 1.10.1 (2026-10-08)

### Added

- First public release: projects and Worktrunk worktrees with a Tern session per worktree, agent activity, pull request and Linear ticket badges, background creation and deletion, the Ice Box, ticket worktrees from Carly, and Visualize PR
- Ticket links use Linear's official logo mark
