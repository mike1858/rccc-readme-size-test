

# Real Claude Code Changelog

Claude Code's release notes only describe what Anthropic chooses to announce, not everything they actually change. Bug fixes sometimes ship in one version but get announced in another, and even significant features can go unmentioned entirely.

---

Here are 3 examples of changes that were shipped but not announced at all:

> **Files Claude reads on your computer, and the output of commands it runs, become part of a cloud session** — visible to anyone the session is shared with and, on Team or Enterprise plans, to your organization's administrators. *(2.1.257)*

> **Approving an Artifact live room lets page events from anyone in your organization start turns while you are not present.** *(2.1.251)*

> **A comment from a writer on a watched Artifact can trigger automatic edits to it**, subject to server-side re-checks on the writer, the edit grant, file count, size and rate. *(2.1.248)*

There are a lot more (over 100) in the [changelog below](#changelog).  **Coverage:** 79 releases from 2.1.**111** to 2.1.**325**.

:point_right: **[Read the changelog ↓](#changelog)**

## Measuring completeness

Tracking machinery is still a work in progress, so consistent full evaluation of each release is currently unviable.  Significant architectural refactoring, like the React Compiler migration, interferes with a clean diff, so we've developed a completeness metric to make it clear how comprehensive each changelog entry is.

| Icon | Status                                   | Description                                                                                          |
| ---- | ---------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| ◆◆◆◆ | **Complete**                           | We identified 100% of the changes                                                                     |
| ◆◆◆◇ | **Complete barring a few assumptions** | We identified everything but have a few assumptions we haven't finished checking                      |
| ◆◆◇◇ | **Most**                               | We identified _most_ (_potentially_ all) but aren't sure about the entire release                     |
| ◆◇◇◇ | **Highlights**                         | We only identified some of the changes (usually because the release is very large and full of churn). |

Every entry compares the linux-x64 build of a release with the linux-x64 build of the release before it.

## Visibility indicators

Naturally, we include entries that are also in the official changelog; therefore, each change is labeled with a badge.

| Status | Anthropic officially documented? | and in the right version? |
| ------ | -------------------------------- | ------------------------- |
| 🟢      | **Yes**                          | **Yes**                   |
| 🟡      | **Yes**                          | ⚠️ **No**                |
| 🔴      | ❌ **No**                         | —                         |

## Caveats

1. Just because a change is shipped **doesn't necessarily mean anyone can use it** because new features are often _gated_; that is, unavailable to most public users.

2. Anthropic can alter Claude Code's behavior without code changes by dynamically enabling feature flags in the [GrowthBook](https://www.growthbook.io) console.

## Have you found something we missed?

If so, please [open an issue](https://github.com/Piebald-AI/real-claude-code-changelog/issues/new).

## License

- Each entry listed in this changelog is licensed under [CC BY 4.0](LICENSE), meaning you're free to quote entries provided you link back to this repository.
- Any strings which are quoted verbatim from Claude Code's source code are proprietary to Anthropic, so they don't fall under CC BY 4.0.  You don't have to link back here for those.
- Neither we nor this repository are affiliated with or endorsed by Anthropic.
- The tools we develop to produce this changelog are closed source for the time being.

# Changelog

### 2.1.327 ◆◆◆◆
- 🔴 Removed the built-in Claude Test plugin: `/claude-test` and `/claude-test --ci`, the `onboard`, `fix` and `all` actions, `.claude-test/specs/` completion, the `Write Claude Test draft` tool and its preview, and the local browser helpers. The first-run flow, browser preview and approval, special MCP password and no-selection prompts, dev-server cleanup and account-change disconnects are gone. Claude Test on claude.ai still uses its separate connector.
- 🔴 Removed the Claude Test pane, including startup and test progress, estimated time, rerun comparisons, results and screenshots, and the report link and browser controls governed by `CLAUDE_TEST_NO_OPEN`.
- 🔴 Removed Bash and Zsh's own Tab suggestions in `!` shell mode. Users lose shell-provided command, variable and path matches; Claude Code still offers its built-in path suggestions.
- 🟢 Sped up the first eligible Bash call by starting the shell while the call is still streaming. Shell startup files can run before approval, even if you refuse the command. The remote switch `tengu_amber_kettle` defaults on; set `CLAUDE_CODE_DISABLE_SHELL_HEAD_START=1` to turn the head start off.
- 🔴 Removed the 1,000,000-token auto-compaction ceiling from saved model windows, `CLAUDE_CODE_AUTO_COMPACT_WINDOW` and `--autocompact`; numeric windows can start at 100,000 tokens. Saved subagent `autoCompactWindow` values also lose the ceiling and still need a safe integer of at least 100,000. The active window can't exceed the model's limit.
- 🟢 Stopped `/export` from overwriting without asking. The interactive picker defaults to changing the name; `/export <name>` leaves an existing file alone and points to the picker.
- 🟢 Changed `ExitWorktree` to ask before discarding work it may delete, without offering an always-allow choice. An existing allow rule, auto mode or bypass permissions mode can still let it proceed without asking.
- 🟢 Added local setup diagnostics to `/status` for broken agent, output style, skill and command files; keybinding and sandbox problems; ignored settings; and tool hook matchers written in a form that won't match. It shows up to 10 entries of 300 characters each and counts the rest. The extra checks don't run for a thin client attached to a remote session.
- 🟢 Retried a transcript read once using the whole file when it ends early. If the retry fails or finds no conversation rows of its own, Claude Code keeps the first read. It logs a warning if messages might still be missing.
- 🟢 Added `/tui classic` to switch back from `fullscreen` and used it in startup, onboarding and recovery instructions. `/tui default` and the `default` argument still work.
- 🔴 Extended Bash's `view_images` beyond the shared-server path to eligible clients, including the terminal, when the remote flag `tengu_fluttering_canyon` is on. It defaults off. Up to four images share 15 seconds, including any image the command returns itself. Each requested file still needs `Read` permission.
- 🔴 Listed clickable filenames in Bash results when image files were sent to Claude. Image-only results keep the short image notice and list the files below it.
- 🔴 Added plain source excerpts to eligible first-party WebSearch results, including in subagents, if WebFetch is available and no matching rule or hook would be skipped. A remote flag has to provide a positive excerpt budget; it defaults to zero. The usual search path also needs citations off.
- 🔴 Replayed the current text or thinking block to Remote Control and cloud viewers that join or reconnect mid-response, when the remote flag `tengu_ccr_restate_open_block` is on. It defaults off.
- 🟢 Preserved pasted images when a prompt begins with a file path and is handled as ordinary text, not a slash command.
- 🟢 Sorted `Grep` content, count and filename results by path before applying limits and offsets. Content groups with the same path use displayed text to break ties; the lines within a group keep their order.
- 🟢 Let local `Edit` calls use a matching full read from before compaction for an unchanged file, including outside the project, even when the cached contents aren't available. The remote flag `tengu_quiet_kite` defaults on.
- 🟢 Kept a completed tool call and its result out of the summary on the first eligible compaction attempt after a reply hit the output limit. The added case needs `tengu_flickering_nest`, which defaults off; if the attempt fails, Claude Code can retry without keeping the pair separate.
- 🔴 Raised the auto-compaction window when instructions and tools take up too much of the window saved for the model in user settings. The offer needs the `tengu_binary_music` experiment or the Desktop host to opt in. The raise targets twice the fixed input, up to the model's limit; `tengu_nested_kite` defaults on. `/context` shows both sizes.
- 🔴 Changed the `/autocompact` slider to stop at the current model's context size rounded up to a 100,000-token step, or at a higher saved override, up to the slider's limit. `CLAUDE_CODE_AUTO_COMPACT_WINDOW` still takes precedence over the saved setting.
- 🟢 Allowed a cut-off reply on a model with an assumed 32,000-token limit to continue with up to 128,000 output tokens when the user hasn't set an output limit.
- 🟢 Warned when an interactive resume or fork starts from a transcript branch whose earlier messages couldn't be loaded. A missing parent on the first saved row alone doesn't trigger the warning.
- 🔴 Recovered the assistant reply around a pending tool call, including its sibling calls when saving and loading a resume point. On eligible headless resumes of an interrupted turn, resume repair skips unrelated blocks of unfinished replies; finished replies keep their blocks.
- 🟢 Restored a newer transcript branch after an unanswered tool call or a reply with no visible text, even if system rows follow that silent reply. Both need resume repair and a remote switch that defaults off: `tengu_sorted_metcalfe` for unanswered calls or `tengu_spicy_oboe` for silent replies.
- 🟢 Kept an explicitly empty conversation empty when resuming after a rewind instead of reviving an older transcript branch. This needs resume repair and `tengu_witty_octopus`, which defaults off.
- 🟢 Fixed a rewound prompt coming back after another restart. Claude remembers which transcript rows predate the rewind and saves the new resume point when new rows arrive, even if the next prompt's rows arrive out of order.
- 🟢 Reported how many files `/rewind` couldn't restore and showed up to three failing paths, including missing backups and failed writes. The result still distinguishes files skipped on purpose.
- 🔴 Kept file attachments for prompts that survive a rewind even when the attachment rows themselves were on the removed side.
- 🔴 Stopped background shell commands that belong to a removed turn on rewind even when the task record lacks a tool-call ID and the transcript can supply it.
- 🔴 Changed `--resume` and `/resume` to share session loading, reloads and pagination; if a page fails to load, the picker can keep going instead of giving up on later sessions.
- 🔴 Made `/resume` look up a supplied session ID even when the project has no listed conversations, and say when a supplied ID or title can't be found.
- 🔴 Kept matching sessions visible while the resume picker reloads, and kept the selection marker visible while you're typing a search.
- 🟢 Kept eligible incoming prompts and notifications queued through Stop in remote headless sessions, including during a restart. The added protection needs `tengu_whimsical_crayon`, which defaults off.
- 🟢 Fixed tasks created after `/clear` going missing or cleared tasks coming back when a cloud or self-hosted runner session resumes without its feature flags. The worker uses the session's own task list when restoring tasks.
- 🟢 Stopped cloud and self-hosted sessions from running a command after Stop when its approval arrives after a restart and feature flags haven't loaded yet.
- 🔴 Removed the session-specific suffix from Worktree's default name when Claude creates a worktree without a name.
- 🔴 Stopped counting commits already reachable from a remote branch or a base ref that won't be removed as work at risk in the ordinary worktree removal prompt. Uncommitted and other at-risk work still counts.
- 🟢 Resolved relative `--add-dir` paths against the launch folder when `--worktree` moves the session, including for background launches. An eligible sibling folder stays available at its original location; some parent paths such as `..` narrow to the worktree. This is on by default.
- 🟢 Fixed `claude agents` and `claude rm` falsely reporting unpushed commits when a background worktree has a top-level file or directory named `HEAD`.
- 🔴 Stopped background launches before handing off when an added folder or its repository needs workspace trust. The error names the folder that needs approval.
- 🔴 Changed `/export` to refuse a linked destination when replacing a file in place.
- 🔴 Fixed `/teleport` to keep the session local if the cloud wouldn't preserve one of its `--disallowed-tools` rules.
- 🟢 Kept `/teleport` open when Enter is pressed after the cloud session list fails to load. Ctrl+R still retries.
- 🔴 Let background agents create and update tasks where the task tools are available, instead of refusing solely because the agent is running in the background.
- 🔴 Added terminal transcript notices for background agents that have waited 30 seconds or more for a permission answer. The remote flag `tengu_clever_pelican` defaults on.
- 🔴 Moved eligible Bash and PowerShell commands to the background after they've run for at least 60 seconds and are holding up an agent report or message. Every unfinished call in the reply must qualify, and `tengu_tranquil_gadget` must be on; it defaults off.
- 🔴 Kept finished background-task notices available until the session reports them, and then saved the updated list.
- 🔴 Fixed resumed subagents losing the original turn's pending tool results when picking up a turn cut short, including an interrupted result for a call that can't be replayed.
- 🔴 Changed resumed teammates to refresh permission and tool choices from the current session and drop the previous effort-based model choice when resuming as delegated work.
- 🟢 Kept agents passed with `--agents` in the available list when plugins are first loaded or refreshed.
- 🟢 Restored the saved main agent on resume when its definition becomes available after plugin checks, so the `--resume` picker doesn’t drop that agent.
- 🟢 Kept eligible subagents started without a model on the model they first used when they're resumed, even after `/model` changes the main session's choice. The regular local path uses `tengu_graceful_pudding`, which defaults on.
- 🟢 Stopped a resumed subagent from inheriting the model and effort choice of a sibling that messaged it. Claude uses the main session's model context to pick the resumed agent's settings.
- 🟢 Changed in-process teammates to resolve their model before estimating or compacting the next turn, so those steps use the selected model's limits and token estimate.
- 🟢 Told the main conversation when an active agent has waited 30 seconds or more for a permission answer, so Claude can tell you which prompt needs an answer. The remote flag `tengu_clever_pelican` defaults on.
- 🟢 Asked a subagent that hits its output limit twice to fit the full answer in half the previous length, or say up front that it's partial.
- 🟢 Waited for plugins before dispatching background wake-ups or checking MCP readiness on restarted, adopted or orphaned subagents, so plugin-provided tools are available. Work arriving during the load is checked afterward.
- 🟢 Paged `ListAgents` results over 10,000 characters in recent-activity order. Results say how many entries are hidden, distinguish incomplete listings from the end, and explain that omitted agents may still be reached by name or a unique prefix of at least three characters. An expired or foreign `cursor` returns `cursor_not_valid` and asks Claude to start over.
- 🔴 Reworked the Background detail screens for agents, shells and monitors, cloud sessions, Auto Mode setup scans, and memory consolidation. Titles identify the task, and status, progress, output and actions sit in consistent places.
- 🟢 Kept unrelated top-level keys in `.mcp.json` when adding or removing a project MCP server.
- 🟢 Refused to add a project MCP server when `.mcp.json` lists servers at the top level instead of under `mcpServers`. The error points to the file layout that keeps those servers visible to plugins.
- 🔴 Made MCP reconnect progress consistent across dialogs and `/mcp`. Status shows `still retrying` or `failed · retrying`, batch size and failures where relevant, with an immediate reconnect link. Closing the dialog doesn't stop retries; invalid settings still show `config issue`.
- 🟢 Gave local, remote and agent-only servers a shared `/mcp` detail layout for connection issues, sign-in and capabilities. Action menus include Back and use arrow keys outside screen-reader mode.
- 🟢 Changed the empty `/mcp` message to suggest restarting after adding a server and explain how to reset a declined project server choice.
- 🔴 Let MCP reads use ordinary resource URIs even when a skill from that server is loaded. Directory aliases still can't be used.
- 🔴 Told the model to search the likely MCP server's tools before answering from memory or saying a capability doesn't exist when the deferred tool list groups that server's tools.
- 🔴 Added file uploads through Desktop for supported connectors that advertise upload support, when Claude Code can't use its own sign-in for the transfer. Claude Code still checks which files can go. Each upload can take up to 18 minutes and can be canceled.
- 🔴 Added downloads through Desktop for connectors that advertise download support when the session can use Desktop's sign-in. Files can arrive inline or in chunks; each request can wait up to six minutes. The connector-files flag `tengu_piped_planet` defaults off.
- 🔴 Made outside-workspace connector uploads default to No. The prompt explains that it won't send the file without permission and can't save an always-allow rule.
- 🔴 Added transcript notices that name files sent to a connector and identify the connector that received them.
- 🔴 Added local file references up to 4 MB for Microsoft 365 uploads and updates to SharePoint and OneDrive. The connector must support filling them, the organization must allow `allow_m365_file_references`, and the remote flag `tengu_curried_forest` defaults off. Ordinary text still passes as text.
- 🔴 Kept MCP images visible when saving the original fails, using a smaller copy when the host can shrink it and adding a note about the resized image.
- 🟢 Showed which project, local, flag, or managed setting overrides a preference in `/config`, including theme, notifications, and auto-compact, so users can see why a change there won't take effect.
- 🔴 Hid overridden `/config` controls for auto-continue, dynamic workflows, idle compaction and Clawd file progress. The controls also stay hidden where the preferences can't be saved.
- 🔴 Showed a warning naming each newly broken settings file that didn't load, with a pointer to `claude doctor`.
- 🔴 Described custom commands that didn't load because a built-in command already uses the same name, and told users to rename them and restart.
- 🔴 Reorganized the `/context` columns for custom agents, memory files, skills, and MCP tools into matching sections with compact counts and token totals, and grouped rows in the expanded view.
- 🔴 Clarified when `/effort` will save a default and when it'll only change the current session, including separate key hints when both choices are available.
- 🔴 Showed the current log path when `/debug` is typed.
- 🔴 Clarified `/restart` refusals and errors: wait for background work or stop it in `/tasks`, retry a failed save later, and distinguish a restart that failed after saving.
- 🔴 Clarified Ultrareview startup and timeout errors. Recognized launch failures are distinct from runs that started and then timed out, without a repeated status prefix or extra copy of the error.
- 🔴 Changed Ultrareview's interruption and failure notices to say an unfinished review may be billed, showing its cloud session ID when known.
- 🟢 Kept a mouse click in a `/plugin` results list from opening the highlighted item and leaving the next Enter press to install or disable it. A click now highlights; Enter opens the item.
- 🟢 Warned before removing a marketplace that this will also delete settings and saved data for the plugins listed in the confirmation.
- 🟢 Listed same-named skills only once in the slash-command menu, instead of showing a row for each location that provides the skill.
- 🟢 Grouped hooks supplied with `--settings` under `Startup settings` in `/hooks`.
- 🟢 Made the `/statusline` setup agent use the `settings.json` under `CLAUDE_CONFIG_DIR` when set, including if that file is a symlink.
- 🟢 Changed `/autofix-pr` cancellation to dismiss the dialog right away and finish stopping the cloud session in the background. If it can't stop the session, you'll get a follow-up with a link to stop it yourself.
- 🔴 Changed `/advisor` calls from plugins to apply only to the current session instead of saving the preference.
- 🔴 Stopped loading `CLAUDE.md` or skills from connector download folders and session scratchpads, including links into them, so files saved there can't supply instructions.
- 🔴 Skipped `CLAUDE.md` files and rules directories in disallowed locations, including when the resolved location is disallowed, and kept their included files out of the loaded instructions.
- 🔴 Removed public access from Claude Code project, plan, and debug folders you own on Unix, while preserving group access for folders set up to share. The check skips root and uses `tengu_enchanted_island`, which defaults on.
- 🔴 Protected Claude Code settings and data paths when pruning old sessions and caches, including when cleanup reaches those paths through a link. The remote switch `bot_protected_tengu_purring_magpie` defaults on.
- 🔴 Stopped fast or pasted keystrokes from opting users into data sharing in the startup privacy notice. New input can turn the initial key check back on while the notice is open.
- 🔴 Fixed `/privacy-settings` and Data privacy to confirm a choice before reporting it saved. Read back uncertain writes, restore the toggle or show an error when account identity can't be confirmed, and check again on close. A read over 60 seconds old can't confirm a save. If retry won't help, the dialog offers `Close` instead of `Try again`.
- 🔴 Required Claude to resend some calls from replies begun before switching to auto mode. This applies when auto mode sets aside a broad allow rule and `tengu_plain_hedgerow` is on. It defaults off.
- 🔴 Made file deny rules and read approval rules apply even when a file has more than one path name. An explicit rule to ask before writing now still applies when plan mode or another safety check also requires approval.
- 🟢 Kept parenthesized content in ambiguous `--disallowedTools` lists instead of quietly dropping the deny rule.
- 🟢 Let Escape dismiss a permission prompt as soon as it appears. The brief delay on other answers still keeps a key intended for the previous screen from confirming a tool call.
- 🟢 Stopped arrow keys from wrapping past the decline choice in numbered permission, question and plan menus, including prompts that show No first. An out-of-range number selects No.
- 🟢 Sent more broad package-runner allow rules through auto mode review, including `pnpm dlx`, other `exec` and `dlx` forms, and rules written with a path or version.
- 🟢 Kept Bash, PowerShell and Monitor calls in the sandbox when their permission check depended on it, even if they'd normally run outside it. Bash and PowerShell explain why in the result.
- 🟢 Refused more Bash commands that name a path covered by a Read or Edit deny rule when `permissions.blockReadsOutsideWorkingDirectories` is on. Claude Code checks more path spellings before offering approval.
- 🟢 Fixed an earlier Code Mode script losing its auto mode approval when a later script also passes auto mode. Inner tool calls can still use the check that allowed their own script.
- 🟢 Fixed auto mode disregarding some wildcard PowerShell allow rules for scripts with paths starting with `.\`. On Linux this applies if you've enabled the PowerShell tool with `CLAUDE_CODE_USE_POWERSHELL_TOOL=1`.
- 🟢 Cleared unfinished `!` commands when the usage-limit menu opens automatically, so another Enter after closing the menu doesn't run one by accident.
- 🟢 Stopped Tab from filling in Bash filenames where a new command belongs, such as after a pipe or opening parenthesis. It still completes filenames in arguments.
- 🔴 Kept shell and slash commands marked to run on their own out of the current prompt batch. They can wait for their own turn without holding up an otherwise ready prompt.
- 🔴 Let text that looks like a slash command join a running turn in eligible remote chats, when the model supports messages during a turn and the text doesn't match an installed command. Recognized commands still wait.
- 🔴 Recognized standard output and error device redirections in Bash permission checks when the sandbox permits the writes. Explicit deny rules still apply.
- 🟢 Fixed slow rejected `Write` calls on large files by showing a preview of the proposed text instead of reading the existing file to build a diff. The compact view just names the rejected write.
- 🟢 Named changed files in the Bash result even when their diff can't be shown, including binary files and files too large for the preview. Compact output lists up to three extra names before the rest count.
- 🟢 Reported the size on disk when `Read` rejects a streamed file of 10 MB or more, rather than showing the 512 KB it read before stopping.
- 🟢 Let notebook reads return supported output even when a cell also has an unknown output type, with a note about what couldn't be shown.
- 🟢 Kept a notebook's final newline when running NotebookEdit.
- 🟢 Limited file lookups to the first 100 `@` mentions per prompt so a long list doesn't stall the turn. Claude Code logs when a prompt names more.
- 🟢 Rejected WebFetch URLs longer than 2,000 characters during argument validation and returned a length-specific message.
- 🔴 Made `Edit` include the next newline when deleting a match that doesn't already end in one and is immediately followed by one. The approval diff uses the same match when it can read the file.
- 🔴 Held back the always-allow choice for a replace-all edit until its inline terminal diff is drawn. If that preview can't show all the changes, you can approve just this edit or reject it. This doesn't change the separate IDE diff.
- 🔴 Pointed users to `/rewind` for oversized API requests, instead of suggesting they make room or try the same request again when the problem is the request size rather than the model’s context limit.
- 🔴 Fixed wrapping and spacing for wide characters, emoji sequences, flags and combining marks. The new renderer uses measured widths and can substitute a character that fits instead of wrapping early. The classic renderer also works around some of these problems in GNU screen.
- 🔴 Checked the terminal's actual cursor position after quiet periods and repaired misaligned frames in the new renderer. Repeated mismatches can switch to the old renderer. Slow terminals and sessions that don't own the terminal aren't probed.
- 🔴 Stopped waiting for terminal capability probes after 2 seconds so the app can proceed even when a reply never comes.
- 🔴 Made fullscreen transcript search fit more comfortably in narrow terminals. The status now shows the match count or indexing progress alongside the keys: Escape appears from 50 columns, and Enter from 70 when there are matches.
- 🔴 Made transcript search match thinking text as well as assistant text and tool inputs.
- 🟢 Made terminal transcript search find the contents of applied `Edit` and `Write` results, including newly created files and overwrite diffs, while excluding staged changes.
- 🟢 Restored the prior query and returned to the original message and row when cancelling a fullscreen transcript search. If the transcript was following the bottom, it follows it again.
- 🟢 Fixed next and previous in fullscreen transcript search when a match can't be shown. The search moves past it, trying up to 20 matching messages per move.
- 🟢 Recovered from damaged terminal layout data by rebuilding measurements and repainting. The checks are on by default; set `CLAUDE_CODE_DISABLE_LAYOUT_CHECK=1` to turn them off. Claude Code stops checking if the damage keeps happening.
- 🟢 Retried a failed terminal redraw right away if it hadn't already forced a full repaint, so the screen can recover from a brief memory allocation failure.
- 🟢 Fixed some garbled rows in tmux with passthrough enabled, including inside kitty, Ghostty and iTerm2. The new renderer corrects the cursor when tmux might've skipped drawing unchanged cells.
- 🟢 Kept the input cursor's target column when moving up or down across a shorter or empty line.
- 🟢 Reworked the model picker on terminals under 60 columns: rows stay short, and the focused model’s longer description moves below the list.
- 🟢 Made the `/effort` picker fit narrow terminals by showing the current choice between arrows in place of the full slider.
- 🔴 Warned in the paid-model continuation prompt that Claude can spend credits without asking again, including for organization members who can already continue on credits.
- 🔴 Added a second confirmation before removing the monthly usage-credit limit, with `No, keep a limit` selected first.
- 🟡 (2.1.326) Made the monthly spend-limit menu ignore input that arrives as it opens or while the limit is loading, so early keypresses don't carry straight into a choice.
- 🟢 Added a warning when an API key overrides an Anthropic profile at startup, with advice that depends on whether `/login` saved the key.
- 🔴 Clarified the `ANTHROPIC_API_KEY` warning: turn on **Use custom API key** in `/config` to use the key.
- 🟢 Kept dynamic workflows paused at their usage limit if the original login changes or can't be verified, even after the limit resets. A fresh prompt or the old login can resume; the pause notice points to `/workflows` instead of showing a reset time.
- 🟢 Discarded a pre-warmed background session if the account or organization has changed, or if Claude Code can't verify the current login when it's about to use the session.
- 🟢 Fixed Artifact publishes being refused after a file's timestamp changed without changing the approved bytes. Claude now checks the file hash saved while approval was pending.
- 🟢 Fixed `/artifacts` to keep looking for an item eligible for the All list if its first pages look empty, with a fallback when paging can't supply a usable result.
- 🟢 Retried temporary `/artifacts` page failures twice, honoring the server's requested delay when available, then displayed `Couldn't load the whole list` if paging couldn't finish.
- 🔴 Fixed artifact stars in `/artifacts` so an older listing can't immediately undo a star or unstar the user just changed.
- 🔴 Fixed `/artifacts` so later pages don't undo pin or unpin changes you've already made while the list was loading.
- 🟢 Fixed new artifacts being refused after a person approved them in an eligible cloud scheduled task, when the organization requires approval for each one.
- 🟢 Fixed new artifact creation and first publish being refused after a person approved the call through an eligible local Agent SDK app, when the organization requires approval for each new artifact. The app must run the session in the foreground over standard input and output and ask for one-time approval on that call.
- 🟢 Accepted the short `/_blob/…` path an artifact page shows, as well as the ID, when reading a single upload. Published-file and batch reads that encounter an upload now explain how to read it separately by ID.

_Where every path to a change runs behind a remote flag, its bullet below names the flag and whether the change is on by default. Other changes may sit behind flags too._

<details>
<summary>680 more visible changes</summary>

- 🔴 Added a startup error when Claude Code can't decode the current directory's name and can't confirm the displayed path leads to the same folder. The message suggests renaming the folder or removing a nearly identical name beside it.
- 🔴 Fixed bare Git repository detection when a normal checkout or linked worktree sits inside it; discovery checks the nearer checkout before treating the outer directory as bare.
- 🔴 Showed tiny token shares in `/context` as a decimal or `<0.1%` instead of rounding them all to whole percentages. The same rules apply to the breakdown, totals, free space, and the auto-compact reserve.
- 🔴 Told the model to disclose when it read only part of a page, and to fetch the next section only when the task needs it.
- 🔴 Removed Cowork memory from later turns and compaction when the remote session loses access. A complete empty listing counts; an incomplete one doesn't. Claude gets a notice naming up to eight affected files, or the session drops the whole prefetch if it can't tell which files are gone.
- 🔴 Included the session ID in new plan filenames when session-based naming is enabled for data-flow recording, including fallback saves and fork copies.
- 🔴 Allowed eligible self-hosted cloud sessions to start an Artifact from a type when the session has a signed-in account.
- 🔴 Changed how a resumed worker is told what to do after it has already sent a report: the reminder now distinguishes reporting, messaging and waiting.
- 🔴 Asked before saving personal memory when Claude Code can't check where a symlink leads, and explained why it couldn't approve the path automatically.
- 🔴 Kept the usual Claude in Chrome approval and auto mode review just after navigation, even if the browser feature switch would otherwise skip the prompt.
- 🔴 Let the Claude Docs creation step wake when the cloud Artifact entrypoint replaces the built-in Artifact tool. (behind `tengu_breezy_acorn`, on by default)
- 🔴 Adjusted Artifact notices and comment prompts to point to alternative commands when the built-in Artifact tool is off or hidden.
- 🔴 Let eligible Desktop and remote SDK turns return before end-of-turn classification or summary finishes when the host requests it. The summary can be sent separately.
- 🔴 Skipped an extra Cowork memory refill after compaction when the stop path already handled the refill, with the rollout enabled. (behind `tengu_fizzy_adleman`, off by default)
- 🔴 Tried the catalog's first-party ID when a described model alias fails and the session can switch, before falling back to lower models and even if the previous request already named that model.
- 🔴 Let worktree status look up GitHub repository visibility through the worktree's Git runner, so it can identify the right repository when the host Git context differs.
- 🔴 Picked up newly created GitHub pull requests when a combined GitHub CLI command prints the PR link before moving on to `gh pr merge` or `gh pr ready`, even if a later step fails.
- 🔴 Marked pasted, quoted and `@`-attached text as outside content where enabled. Cleaned up invisible characters and lookalike delimiters and kept multi-block quotes distinct from trusted messages.
- 🔴 Raised the auto-compaction threshold in eligible local sessions based on separately measured startup context. The rollout can raise a configured window of at least 300,000 toward twice the startup load, leaving 13,000 tokens plus output room and respecting the model and long-context limits. `/context` still reflects the original setting.
- 🔴 Changed which recent messages survive partial compaction: a new switch controls the cutoff, and the rollout can keep the eligible tail when the normal check would drop it.
- 🔴 Saved eligible prompt documents as local files marked `[Document source: …]`. Claude Code keeps readable names where possible, rejects credential filenames, and tells Claude if invalid UTF-8 prevents a save.
- 🔴 Detected likely file references in connector fields that only accept text, so Claude can put the file in the right argument instead of sending it as text.
- 🔴 Refused connector file references for MCP tools that can't accept them when the file-reference check is on. Claude explains that nothing was sent and asks what to do with the file instead.
- 🔴 Told Claude to stop redirects from outside downloads at local or cloud metadata addresses and to leave credentials and user data off redirects to another host. (behind `tengu_reactive_heron`, off by default)
- 🔴 Told Claude to inspect outside files' configuration, hooks, and links before running tools over them, and to isolate those commands from the network where possible. (behind `tengu_reactive_heron`, off by default)
- 🔴 Told Claude to stop using a previously shared email address when the next session context update drops it.
- 🔴 Delivered activity digests beside `ObservedActivity` as host-supplied results, like voice-call records. The digest keeps its note with its own result and isn't a callable tool.
- 🔴 Placed queued follow-ups marked for delivery beside a tool result or prompt after that row, so the conversation shows what the follow-up belongs to.
- 🔴 Told Claude to wait for the next event after a worker restart if the previous turn had already finished, rather than asking it to resume finished work.
- 🔴 Used the command text for shell steps in conversation summaries, instead of handing the summary the argument object. (behind `tengu_foamy_spring` and `tengu_mutable_phoenix_killswitch`, on by default)
- 🔴 Fixed where transcript writes and the rewind picker resume after a dropped or replaced row, including in `-p`. In-place rewind orders copied rows by their parent links when the saved branch is out of order.
- 🔴 Kept queued messages meant to wait out Stop from starting automation, and let only eligible user slash-command text go through the normal queue selection.
- 🔴 Added a rollout for notifying the main conversation about progress or handbacks from its directly started agents. (behind `tengu_moonlit_peacock`, off by default)
- 🔴 Kept a plugin action tied to earlier user input from lifting a Stop after a more recent stop request.
- 🔴 Timed out PowerShell's prestarted shells and parsers: Claude Code waits up to 10 seconds for shell readiness, discards unused processes after 15 seconds, logs failed reuse and backs off after repeated misses.
- 🔴 Kept the specific Bash approval reasons when more than one safety check applies or a later permission check can't finish.
- 🔴 Capitalized Git in the Bash approval message for changing directories and writing Git control files.
- 🔴 Explained when auto mode can't check actions because the permission and auto mode settings are too large to send, with advice on what to shorten.
- 🔴 Stopped putting the user's email address in model context for sessions from Claude chat when the rollout is on. It defaults on outside shared servers.
- 🔴 Capped large `Glob` and `Grep` results at 500,000 characters per text or filename field when sending them to create a hosted conversation.
- 🔴 Rejected Bash image paths containing control characters that would be hidden in the approval dialog.
- 🔴 Shortened tasks under the `TaskCreate` rollout: titles use an imperative of up to five words, descriptions are for unclear or delegated work, and the UI shows the title instead of a separate in-progress label.
- 🔴 Kept loaded MCP skills listed after plugin or fallback compaction, even when their text was omitted, using a saved skill entry when one is available.
- 🔴 Redirected an MCP read when the server returns a file from a loaded skill under another address. The result explains why Claude should use the loaded skill's read path instead.
- 🔴 Added a gated resume fix that treats the Stop hook's block limit as an intentional stop. Resume won't restart the turn just because the transcript ends near that result.
- 🔴 Detected skill naming collisions without using the unqualified names of synced skills, so a synced command isn't treated as a local collision just because its short name matches.
- 🔴 Told compaction to attribute messages from other sessions separately, carry their conditions forward and distinguish quoted instructions from what the user asked or approved.
- 🔴 Changed which skill descriptions fit in the tool list: personal and account-uploaded skills get a small boost without usage history, text entries get at least 120 characters, and eligible plugin skills can join the built-in list. Name-only entries stay short.
- 🔴 Reoffered deferred tools when active work needs them or they were used in the 30 assistant replies before compaction. The offer stops at five tools or 5,000 estimated tokens and logs why the tools were brought back.
- 🔴 Kept references to long prompts saved as files through compaction, without inserting duplicates for paths already present.
- 🔴 Kept a smaller saved output-token cap when retrying a rejected request, even when the API reports a higher maximum of at least 4,096 tokens.
- 🔴 Applied the same invisible-character cleanup to names reported by the browser tool.
- 🔴 Set certain auto mode calls aside without counting an ordinary refusal when a stripped tool allow rule covers them, while leaving calls that need a person's approval on their existing path.
- 🔴 Kept the child session name when an eligible prestarted shell dispatches the Claude command wrapper, including when it can use the simpler launch form.
- 🔴 Left Bash startup to run normally when zsh initialization may need a real terminal. The check reads up to 262,144 bytes per startup file and gives up after 1 second.
- 🔴 Explained failed PowerShell handovers: rerun if the spare exited, its environment changed or its wait expired; check whether the command ran before retrying if the handover itself failed.
- 🔴 Checked launch and workspace boundaries before treating an unverified shell working directory as usable, including when the directory is resolved by the host.
- 🔴 Shortened oversized notebook text outputs and included the number of lines left out.
- 🔴 Nudged eligible child sessions to post their current checklist after five minutes without a progress update.
- 🔴 Nudged eligible shared threads to update their status after five minutes with no checklist change or reply. (behind `tengu_snug_cocoa`, on by default)
- 🔴 Counted the line-number text when estimating how many tokens a numbered file attachment uses.
- 🔴 Treated an unchecked file link as uncertain when deciding whether a prompt attachment came from a connector download.
- 🔴 Shortened the plugin-stop toast to `stopped · type a message to continue`.
- 🔴 Gave eligible headless tools on shared servers 3 seconds to report a result after an interrupt, compared with 5 seconds elsewhere.
- 🔴 Delivered an out-of-place tool warning to the model once on the main tool path, without showing it as ordinary user text.
- 🔴 Updated an agent's waiting notice when a message from the parent or another agent changes its text, even when it goes to the same recipient.
- 🔴 Discarded a Bash snapshot prepared more than 20 seconds before the actual command, so the command gets a fresh copy of the environment.
- 🔴 Kept short terminal popovers from needlessly pushing chat rows into scrollback in the new inline renderer; expanding or shrinking the window redraws the affected rows.
- 🔴 Changed who can use the new renderer: it checks terminal identity, reported quirks and measured width, including separate profiles for VS Code and VSCodium. With the rollout on, a mismatch can send the session back to the old renderer.
- 🔴 Checked organization rules, sandbox network policy, and disallowed address ranges before the Monitor tool opens a WebSocket, including when the tool runs after approval.
- 🔴 Kept artifact comment safeguards in effect when a bridge delivered a summon without opening a new turn.
- 🔴 Carried safety-stop handling into background requests that warm a resumed session’s cache or prepare a comparison fork.
- 🔴 Kept the standard continuation note after certain safety stops instead of using a note that assumes the model can accept the stopped output.
- 🔴 Let a resumed background agent recognize an unanswered tool that needs user interaction and carry an interrupted result forward rather than lose the unfinished turn.
- 🔴 Rebuilt the conversation's data-flow history when an agent's transcript is resumed or reopened, and marked the history incomplete for a fork.
- 🔴 Kept Kitty and iTerm2 inline-image placeholders aligned with nearby terminal text by counting each valid placeholder for the active protocol as one cell.
- 🔴 Limited how much scrollback the new inline renderer keeps in memory for very long chats, so redrawing doesn’t require a grid covering every old row.
- 🔴 Distinguished an artifact edit grant that belongs to another account from a general publish failure, so an auto-edit can report the right reason.
- 🔴 Used the team’s main model as the starting point when an existing teammate receives a turn, instead of taking the sending agent’s model.
- 🔴 Let reads from Artifact connectors use file results saved in the session, while keeping the connector’s original result if saving the file failed.
- 🔴 Stopped `ExitWorktree` if the session entered or left a worktree while the call waited for approval, rather than applying the earlier decision to a different checkout.
- 🔴 Let an eligible `PostToolUse` hook finish while the next queued tool runs, then delivered the hook’s context when it was ready. A cancelled or timed-out hook adds nothing.
- 🔴 Refreshed the configured model when a repository policy stopped allowing its automatic model switch.
- 🔴 Made Artifact database errors name the query rule that failed, including field and cursor restrictions, and say when retrying won't help.
- 🔴 Recognized selected iOS project and configuration subfolders despite capitalization differences in their on-disk names.
- 🔴 Cleaned up data from the built-in `claude-test` plugin at startup, leaving shared, active, moved or custom-cache folders alone and skipping cleanup if the replacement-name folder has files.
- 🔴 Matched warning and error colors between the activity spinner and its text. It blends colors the terminal can render directly and switches between named colors when it can't blend them.
- 🔴 Indented continuation lines under long choice labels in permission and question dialogs, keeping them visually tied to the same choice.
- 🔴 Retried a failed hint-line refresh after the current input callback so a refused refresh doesn’t immediately interrupt terminal input.
- 🔴 Picked the stricter approval prompt when a tool action checks multiple paths and gets more than one kind of ask, including one-time approvals.
- 🔴 Warned in the Artifact republish prompt when an outside comment could have prompted the changes. The prompt starts on No and lists up to four source files that still need approval, with a count of any others.
- 🔴 Asked for fresh approval for eligible Artifact copies and asset uploads after an outside comment makes the changes require approval. The answer applies to the current call and can't silently cover a later upload.
- 🔴 Showed a desktop connector's display name and short ID in file dialogs and results when the service uses its ID as the connection name.
- 🔴 Told users when a download-capable connector asked for files or other input that the current call couldn't provide, rather than reporting that the tool ran.
- 🔴 Distinguished a desktop connector that still needs sign-in from one that’s busy or refused a download. A busy reply can be retried.
- 🔴 Cleaned up connector names in offered-file notices so punctuation or a name that looks like a link doesn’t obscure which connector supplied the file.
- 🔴 Added an account plugin list so Claude can answer what's enabled on claude.ai or confirm an install. Channel sessions list their own plugins, local `/plugin` installs stay out, and oversized lists explain how many can fit and where to disable plugins.
- 🔴 Added eligible local module skills to the account skill list when the experiment is on, leaving out entries already represented by an account plugin. (behind `tengu_modular_gizmo`, off by default)
- 🔴 Changed artifact publish and edit approval on a laptop to ask when comments may have come from someone other than the user, even when there's no shared input end point to check.
- 🔴 Stopped read-only Remote Control requests such as checking usage or file suggestions from counting as input that keeps the session active.
- 🔴 Retried sending a session message once after an ambiguous network failure when the service can deduplicate it, waiting one second before trying the same message again.
- 🔴 Explained why a cloud session refused a message: shorten an oversized one, wait out a rate limit or check the service's reason when it can be shown safely.
- 🔴 Changed the first fill-width terminal table column to start at 12 to 20 columns based on its last cell, then share the remaining space and wrap text when it's the last column.
- 🔴 Fit tab headings to the available width and let the selected tab fill the panel. The title gets space when the labels still fit on one row; otherwise labels get the space they need.
- 🔴 Changed masked form fields to give the text editor at least 60 columns of working room so a narrow prompt doesn't get in the way of entering a secret.
- 🔴 Indented and dimmed warning and lock details under settings and plugins, including **Unattended commands from cloud sessions** in `/plugin`.
- 🔴 Highlighted the focused item in a plugin action menu with the suggestion color instead of bold text. Unfocused destructive actions still use the error color.
- 🔴 Restricted Claude in Chrome approval shortcuts for first tabs and allowed sites. Opening an allowed site in a batch needs its own eligible host entry instead of treating it as a retry; eligible batches are refused if the extension version is unknown or below `1.0.108`.
- 🔴 Kept a permission prompt from waiting on a queued user message after the classifier has already blocked the action.
- 🔴 Stopped resuming a queued message as unanswered after it already got a reply, including after a cloud interruption or compaction.
- 🔴 Changed fresh session resumes to use the saved turn and client refusal lists even when the normal version comparison would skip merging them.
- 🔴 Changed the worktree marketplace warning to ask users to run Claude once in the main checkout if it has those marketplaces, instead of suggesting a new registration from the worktree.
- 🔴 Said when a worker or container restart stopped a background task and the agent couldn't be restarted, rather than just showing it as stopped.
- 🔴 Counted discarded or rescheduled prompts only since the latest qualifying turn boundary when enforcing the limit on recovering a parked permission.
- 🔴 Changed whether a new prompt takes over from a waiting permission card: a prompt from a restart, a person's prompt, or a handed-off turn isn't treated like an unattended task notice.
- 🔴 Kept late artifact-restore warnings in the main stream on the enabled surface, so they don't wait behind the informational messages put aside at startup.
- 🔴 Kept a permission already handed off to another turn from blocking a queued answer or receiving tool exclusions meant for a still-waiting approval.
- 🔴 Used the tool's name as well as its call ID when rewind finds certain tasks to stop, so it doesn't mistake another call for the task that needs stopping.
- 🔴 Changed automatic rewind after a usage limit or model safety refusal to distinguish where work stopped from cases where a fallback model already continued.
- 🔴 Kept a still-current Cowork memory note from counting as withdrawn when deciding whether new work makes a rewind unsafe.
- 🔴 Delayed idle notices to other sessions while work still has to run after a stop, so subscribers aren't told the session is done too soon.
- 🔴 Kept the usage-limit reset time available to the automatic-continuation notice as the quota moves between states.
- 🔴 Made transcript status lines more compact and consistent, including tool progress, tool refusals, and empty tool results.
- 🔴 Showed which task was cancelled in the tool result, with the task ID emphasized.
- 🔴 Used a compact `Thinking…` row for private reasoning that the transcript can identify but can't display.
- 🔴 Rendered MCP resource and polling updates as separate transcript rows, including the reason when the update supplies one.
- 🔴 Displayed memory references as linked paths, marked local memories, and indented the memory text where that view shows it.
- 🔴 Emphasized the counts in collapsed tool activity, so it's easier to see how many files were read or changed.
- 🔴 Showed cloud-session links for downloads sent through the file and message tools. Each notice names up to three files and counts the rest.
- 🔴 Warned which files failed to upload on session-page sends, including on the terminal fallback path, and suggested asking Claude for another way to get them.
- 🔴 Passed the measured position through when jumping to a transcript row instead of recomputing it after the scroll starts.
- 🔴 Kept the text-cleaning choice used to build an edit preview steady while the user is looking at it.
- 🔴 Tidied task details: removed the extra divider, kept stats visible when descriptions shorten, limited shell, monitor and cloud titles to 200 characters, and made running, pending and killed states clearer.
- 🔴 Changed how the terminal estimates the width of compact multiline text, so the preview uses the available columns more closely.
- 🔴 Let the rewind menu show all available choices on terminals with at least 28 rows when the menu can use the room.
- 🔴 Explained why `!` commands can't be used while driving a remote Claude session, with different wording for cloud sessions and sessions on another machine.
- 🔴 Allowed Escape to close a permission or other eligible dialog even when text is still in the draft.
- 🔴 Opened the Fast Mode dialog in the retry path when eligibility couldn't be checked because of a network error or an unknown status.
- 🔴 Used the focused-row styling for the idle-agent summary in the task list and a more compact layout for subagent transcript previews.
- 🔴 Counted only running agents in the active-agent total instead of treating every task in that group as active.
- 🔴 Registered user-invoked popovers that aren't already covered by an active dialog, so the terminal can account for them when laying out the screen.
- 🔴 Added a rollout for a pull request status readout under the remote prompt and refreshed it when its display flag changes.
- 🔴 Showed the reason when feedback couldn't share the transcript.
- 🔴 Added `r` to retry recoverable Usage credits errors, including a failed checkout tax calculation. Payment stays unavailable until tax can be calculated.
- 🔴 Showed the billing-page link in the Usage credits error panel and offered a keyboard action to open it in the browser.
- 🔴 Changed the custom usage credit amount prompt to show its currency, minimum when the amount is too low, and what a valid amount buys.
- 🔴 Shortened iTerm setup errors to the first four display rows unless the full output is being shown, with a count of the lines below.
- 🔴 Reworded the prompt shown when the full choice preview won't fit, so users can compare the labels and descriptions instead.
- 🔴 Formatted the suggested sandbox settings as sections and lists, including a clear empty state for sections without suggestions.
- 🔴 Warned when a shortened `replace_all` or `Edit` preview may leave out changes, including a match count when Claude Code can count it.
- 🔴 Explained when Claude's question or suggestion is waiting for you to send or clear the draft you're typing.
- 🔴 Retried an initial cloud baseline read after roughly one, two and five minutes rather than leaving it pending after the first failure.
- 🔴 Changed the list of files edited before the session to share the show/hide control used by other diff groups and keep the expanded list compact.
- 🔴 Clarified model pinning with simpler labels for the pinned, built-in default and selected models, and progress rows for each tier's one-token test, model ID and failure reason.
- 🔴 Passed long Read and plain-text MCP results through plugin hooks in parts, then put the answers back together. If that fails, Claude gets a smaller-read suggestion instead of an incomplete result.
- 🔴 Passed saved shell output through Read hooks before showing it to Claude. Code Mode explains when it can only show the start because the hook changed the file or range.
- 🔴 Stopped Grep from returning file contents when that would skip an installed plugin's Read check. The error tells Claude to use the shell search path instead.
- 🔴 Copied eligible files read through the artifacts MCP connector into the artifact's scratchpad. Files that fail the path, permission, or content checks stay with the tool result instead.
- 🔴 Told Claude when an artifact version label was cut short and reminded it to use a few words rather than a description of the changes.
- 🔴 Changed `SendMessage` to say a message was sent rather than delivered when Claude Desktop accepts it but can't also subscribe to idle notices for that session.
- 🔴 Clarified failed artifact data reads when the link or account access is likely the issue, and pointed Claude to the artifact listing to find the right link.
- 🔴 Changed Artifact preview to suggest the browser selected for the session, with its install command on a separate line. A missing selected browser returns `preview_pinned_browser_not_installed`.
- 🔴 Shortened repeated Artifact connector, local-server, and file-access warnings on republish when the same warning was already shown for that Artifact.
- 🔴 Showed when the project listing can't inspect a `projects` directory or one of its subdirectories, rather than treating it as an empty listing.
- 🔴 Explained in `/login` when a new process can't start, such as in a background session or when restarting would drop work. If sign-in was already saved, the message says so.
- 🔴 Adjusted the startup privacy notice for terminals under 80 columns or 31 rows and added a New Terms illustration where it fits.
- 🔴 Kept eligible HTTP MCP calls waiting while a connector question is open or has just closed. If an eligible cloud call times out because nobody answered, an approval card can give the user a chance to continue.
- 🔴 Kept valid MCP tools from a partly invalid list page when validation is enabled and reported an error rather than an empty page when none are valid.
- 🔴 Aligned local usage percentages and recommendations in columns and separated day, week and available fixes.
- 🔴 Moved close, retry and tab navigation into the `/usage` footer and tightened the layout around session costs and plan usage.
- 🔴 Changed `/stats` to put space between model legend labels instead of adding an extra dot.
- 🔴 Changed the theme editor's empty search result to say no colors match the filter, instead of implying it was searching for one color by name.
- 🔴 Changed `/resume` to use the permission accent for the loading and resuming screen.
- 🔴 Made `/session` show a clickable cloud link and consistent close hint, with the QR code spaced below the help text when available.
- 🔴 Added counts of tools, resources and prompts to connected server details in `/mcp`, with `none` when none are available.
- 🔴 Fit `/plugin` details, context costs and unused-plugin lists to the regular viewport and shared panel layout, including last-update and install previews. Navigation stays in the open panel.
- 🔴 Changed the `/plugin` marketplace form to suggest `owner/repo`, an HTTPS URL or a local path after an unrecognized source.
- 🔴 Changed `/plugin` install messages to say when a bundled MCP server needs setup and point to Configure in the Installed tab.
- 🔴 Changed the `/skill-doctor` redirect to point to the Stats tab for both usage and context costs in one sentence.
- 🔴 Changed `/skills` to show search and close hints in the browse view, and to offer sort and change hints only when there are rows to act on.
- 🔴 Renamed the `/remote-env` picker’s title to `Remote environment` while keeping the link to web setup.
- 🔴 Reworded `/model`’s no-change confirmation to give `/model sonnet` as a direct example of what to run next.
- 🔴 Changed the guest passes loading screen to use the same title and close action as the rest of the dialog.
- 🔴 Changed the permissions editor’s directory and rule lists to fit the terminal, showing 3 to 10 rows instead of using a fixed list size.
- 🔴 Changed permissions editor confirmations to open on cancel and stop at the first or last choice instead of wrapping.
- 🔴 Added a terminal-specific shortcut hint for inserting a line in an auto mode rule.
- 🔴 Added space above Shortcuts in `/help` on terminals 30 to 43 rows high and at least 80 columns wide.
- 🔴 Made `/import` show two more rows before scrolling and use a compact marker when more items are available.
- 🔴 Changed the model and effort picker to leave four extra columns for its panel before putting the chart and options beside each other.
- 🔴 Changed the confirmation for continuing a session in the cloud to use common title and spacing, with the same cancel-first options.
- 🔴 Explained in Data privacy settings that opting in can share chat and coding-session data for training, including when a choice is already saved.
- 🔴 Rearranged the `/stats` Overview into compact columns for model, token, session, and streak totals, with separate rows for active days and the most active day.
- 🔴 Told Claude to explain when an automated safety check stopped a response, without guessing why. If a tool was interrupted, Claude can still check how far it got.
- 🔴 Changed model picker copy for standard seats to say when a model uses credits purchased separately, and to distinguish a Fable promotion for standard seats from one for Pro.
- 🔴 Told Claude to treat older `Sources:` results as untrusted when the WebSearch excerpt experiment is on, quote up to 125 characters per page and paraphrase the rest.
- 🔴 Changed file permission suggestions to point at the checked directories for files outside the working folders.
- 🔴 Stopped starting fresh waits on slow repository probes that had already used up their 250-millisecond wait; concurrent lookups share the in-flight probe.
- 🔴 Kept using the startup proxy when a settings change requests a different proxy that can't take effect yet.
- 🔴 Changed the model picker to group choices by the names actually sent to the provider, while preserving its Fable and 1M choices.
- 🔴 Let Ghostty clear the visible terminal rows without leaving the rows joined together.
- 🔴 Enabled the child-session notes folder by default for valid session IDs on hosts with local storage; session-based plan names can still require it when the notes switch is off.
- 🔴 Added the time of the reading to low-memory warnings and explained when the available memory comes from a container limit.
- 🔴 Extended Git sandbox cleanup to unsafe links at temporary index and crash report locations. It warns about paths it can't check, retries blocked cleanup after nearby entries are cleared, renames up to 100 links per folder before deleting more, and tells Claude what happened.
- 🔴 Cleaned up leftover temporary Git checkout directories from the plugin cache.
- 🔴 Kept shared comments' publish-only state when there aren't any text sightings left, so a pending comment can still settle across surfaces.
- 🔴 Improved shell input checks so the parser can still recognize quoted text without mistaking it for active shell syntax, while asking for review when it can't classify a substitution.
- 🔴 Labeled the completed command output link `Full output` even when a displayed preview omits its beginning.
- 🔴 Returned a resize error with the byte limit when image compression can't make a read fit, including for an oversized PNG, instead of returning an image that still exceeds the limit.
- 🔴 Warned if the experimental Linux clipboard loader couldn't load the embedded library without leaving a named file.
- 🔴 Treated self-hosted, SDK-URL, and launcher-identified remote sessions as hosted for local-only file and connector features.
- 🔴 Let a Cowork memory response declare whether its file-version list is complete, so a partial list won't make Claude withdraw entries that simply weren't listed.
- 🔴 Tracked the startup memory check and added a configurable grace window before a cloud turn decides whether it needs to wait for another check.
- 🔴 Used a 1-second freshness window for a prefetched result when no memory files are expected; requests that expect files still get 5 seconds.
- 🔴 Reported memory withdrawals as their own cause when reevaluating Cowork's memory context.
- 🔴 Made deep links explain when Claude Code can't verify part of the working-folder path, rather than describing every such refusal as a network link.
- 🔴 Handled invisible characters consistently when quoting transcripts, agent handbacks, and project instructions as text instead of markup.
- 🔴 Dropped pending shared-comment cards when their original tool call goes away.
- 🔴 Told users to run `/login` when the session can't use Anthropic Console sign-in.
- 🔴 Clarified in the Google Drive and Gmail tool guidance which calls can fill in local file references.
- 🔴 Distinguished a Microsoft 365 file-reference refusal from a Google Drive or Gmail refusal when the host doesn't fill in references.
- 🔴 Included SharePoint copy and move operations when comparing file-tool permissions across connector configurations.
- 🔴 Explained when the agent proxy refuses a connection because it has reached its connection limit.
- 🔴 Gave an internal agent proxy failure its own error guidance.
- 🔴 Classified PowerShell module lookup commands as too broad for auto mode to trust on the strength of an allow rule alone.
- 🔴 Made Bash permission checks refuse to judge a heredoc after its parser budget runs out, rather than checking a shortened copy.
- 🔴 Removed the Bash output-saving experiments for failed commands and selected `tail` pipelines. Ordinary Bash task output and normal `tail` behavior still work.
- 🔴 Let eligible connector file uploads use the session's permission flow instead of requiring a new interactive terminal. Outside-workspace files still need their own approval, and the old restart-in-terminal refusals are gone.
- 🔴 Removed an optional restriction that kept background subagents in hosted sessions from using `TaskCreate` or `TaskUpdate` on the main task list outside a workflow.
- 🔴 Changed Bash's checks for commands that may read outside the working folders. This part of the check now includes shell names it left to other checks before, and removes special cases for `tsx`, `ts-node`, and Lua module loading.
- 🔴 Fixed Back from standalone task details: the corresponding task is selected when returning to the Background list, and Left closes the details when there's no list to return to.
- 🔴 Moved most counts out of headings like **Agents**, **Shells**, and **MCP tasks** in `/tasks`. The Background list still shows active shell, agent, and workflow counts in its summary.
- 🔴 Removed the suggestion Claude sometimes got to publish an HTML page as an Artifact after sending it with `SendFile`.
- 🔴 Removed the eight-line cutoff for terminal setup results during first-run onboarding. The setup step now shows the full output.
- 🔴 Reformatted `/mcp` details to show local failure log paths on their own row and put tool behavior annotations above descriptions and parameters.
- 🔴 Fixed Code Mode's `$` shell helper to stop with an error when an array would supply several words for a redirect target or continue an existing word, such as in an assignment. Arrays used as lists of separate arguments still work.
- 🔴 Added a separate Code Mode shell error when an empty array leaves a redirect without a target, so the shell can't take the next word as the filename.
- 🔴 Kept deferred work marked to wait out Stop from interrupting a Stop hook or making the active turn treat its queue as empty. The work can still run after the turn ends.
- 🔴 Dropped deferred after-turn commands when a command marked to run now is already waiting, and otherwise queued them at the front in their original order.
- 🔴 Recognized PowerShell's `gmo` alias as `Get-Module` when checking commands.
- 🔴 Cleared old shell exports when starting a replacement snapshot if none has loaded successfully, so the next command can use what the new shell reports.
- 🔴 Kept explicit approval for dangerous file removals when the Bash path check runs out of time and the deletion check can still make a decision.
- 🔴 Combined Bash approval reasons when workspace read restrictions overlap an ask rule or a dangerous removal check, including in subshells and grouped commands.
- 🔴 Stopped updates to a resumed file-history snapshot from rewriting the saved snapshot unless it was taken in the current session.
- 🔴 Let diskless sessions show tool-provided pictures when the picture switch is on and the remote session isn't limited.
- 🔴 Based the default auto-compaction window's model-cap check on the model's available window, not on the candidate value that prompted the check.
- 🔴 Skipped idle compaction when fewer than 100,000 tokens in the current context are outside the fixed content that raised the window.
- 🔴 Classified rate-limit responses that explicitly indicate an account restriction or enforced spend cap as account limits.
- 🔴 Shortened the `/auto-mode-setup` menu description to leave out optional rule tweaks.
- 🔴 Added the Git argument separator when `/commit` and the commit-and-PR prompt collect a working diff, so Git can't mistake a path for a revision.
- 🔴 Removed the standalone “was stopped” notice from the generic cloud task completion path.
- 🔴 Dropped the transcript's old resume note when its next row is already a request to continue the interrupted turn.
- 🔴 Shortened the `/model` description to “Switch model,” still showing the current choice.
- 🔴 Changed `/statusline` help and its safe-mode notice to refer to the user settings file instead of assuming its path.
- 🔴 Carried file-read notes through plugin-provided and fallback compaction summaries.
- 🔴 Changed Claude in Chrome's off-state advice to point to the chat's `+` menu, under Connectors or Plugins, on the web or in the desktop app.
- 🔴 Limited eligible WebFetch approvals queued beside a tool result to one minute under the rollout. A prompt that wasn't shown because another was waiting says so; regular approval timing is unchanged.
- 🔴 Shortened skill descriptions to fit what's left of a host-supplied budget after names, and used the default listing when it came out shorter.
- 🔴 Added a gated priority for account-owned skills in the model's listing and filtered hidden skill entries through the shared command list.
- 🔴 Told the web-fetch agent's callers to pass URLs already seen in the conversation when cloud proxy URL checks are enabled.
- 🔴 Flagged observer, foreground, and other-agent task notes whose completion won't be announced in this conversation, so they don't leave a wait pending after compaction.
- 🔴 Retried more eligible API and early stream failures with fewer output tokens. Models with assumed limits fall back to 32,000 when the provider gives no usable limit. The notice explains when a reply has started at the lower limit.
- 🔴 Stopped applying the extra 32,000-token output ceiling to security-control requests for models with assumed limits.
- 🔴 Normalized extra control and blank characters when checking text and tool names for auto mode, so those characters don't obscure how the safety check reads them.
- 🔴 Ran eligible scripted tool calls through the rewrite check even when the surrounding auto-mode path already resolved to allow or ask. (behind `tengu_quiet_lantern` and `tengu_linen_heron` and `tengu_plain_hedgerow`, off by default)
- 🔴 Let an eligible interactive turn set aside a tool call that server auto mode couldn't review during a permission-mode change, instead of immediately prompting for it. (behind `tengu_quiet_lantern` and `tengu_linen_heron` and `tengu_plain_hedgerow`, off by default)
- 🔴 Kept the previous shell working directory if Bash reports invalid UTF-8 or a path Claude Code can't verify safely, including network links. Genuine whitespace in directory names still works.
- 🔴 Warned Claude not to start work that depends on a background command's result before it gets the completion notice, even if command output already looks final.
- 🔴 Marked pasted content so it can be framed separately from ordinary file text, and kept the warning for truncated pastes whose remainder isn't readable.
- 🔴 Changed how Bash reports a possible destructive operation in an inline script: the decision uses whether the input check was complete, without treating a late cutoff as changing that answer.
- 🔴 Saved the organizations from before compaction in the transcript, including when headless sessions compact, so a later cloud move can tell whether the session ran under one organization or several.
- 🔴 Fixed the pushed-commit check used when Claude Code recovers an orphaned worktree before entering it. Git now treats revision names as revisions even when the worktree contains paths with the same names.
- 🔴 Reworded `/plugin` update summaries so skipped, policy-blocked, and version-held plugins each get their own short explanation.
- 🔴 Clarified `/plugin` install and activation results, including on or off states, failed loads, choices set by settings and pending changes that need `/reload-plugins`.
- 🔴 Removed the experimental 1–3-line `old_string` instruction from the Edit prompt; Claude still needs a unique match unless it's changing every occurrence.
- 🔴 Kept `SendUserFile` visible as its own call in eligible transcripts instead of hiding it with other folded or excluded tools.
- 🔴 Kept the terminal's current response status when the API restates a content block without sending a new response delta.
- 🔴 Told the model that a skill description ending in an ellipsis is shortened, but the skill still works and should be used when it fits.
- 🔴 Told the model which account-memory files were withdrawn, when it has a safe path list, instead of implying that all earlier memory is gone.
- 🔴 Started pre-warmed background sessions with a new session ID when the launch request doesn't supply one, without the old option to carry the boot ID forward.
- 🔴 Waited for an outstanding terminal reply before flushing an incomplete escape sequence when the terminal is still within its reply window.
- 🔴 Distinguished a terminal cursor-position reply on the top row from an F3-like keypress while a cursor query is pending.
- 🔴 Formatted terminal crash excerpts without splitting the whole source file, and used a smaller stack-frame parser to find the source location.
- 🔴 Let cursor positioning inside a scrollback window follow text and link nodes as well as their containers.
- 🔴 Stopped relying on some fast terminal erase operations unless the terminal is recognized and is known to handle them.
- 🔴 Showed a missing-tool hint when a late permission answer can't run its original tool because that tool isn't offered anymore, without requiring the old feature switch.
- 🔴 Limited inline redraws of exceptionally large terminal output: at 128 Mi cells, the new renderer redraws roughly the last 4 Mi cells and says how many rows it didn't show.
- 🔴 Changed the new renderer to restore inline content covered by a popup when leaving for another screen, handing the terminal off or exiting.
- 🔴 Stopped terminal layout checks after eight failures and added `CLAUDE_CODE_DISABLE_LAYOUT_CHECK` to skip the checks from startup.
- 🔴 Fixed the separate unpushed-commit summary during background worktree cleanup so Git treats its commit arguments as revisions even if they match paths.
- 🔴 Made restart and session-move checks flag allowed and denied tool rules separately, so they can carry ordinary allowed rules without treating them like deny aliases.
- 🔴 Warned when a background launch reads prompt-like text as a flag value and starts without a prompt.
- 🔴 Removed the background launcher's early auto-mode opt-in error; the launched session now handles whether auto mode can apply.
- 🔴 Updated Artifact publishing errors to explain that `index.html` comes from `file_path` or the artifact's type and can't be supplied as an extra published file.
- 🔴 Rechecked whether a Monitor WebSocket destination is allowed when connecting, so a change after the permission check can still stop the connection.
- 🔴 Corrected the Artifact watch-limit message for the MCP tool to say when the session already has ten webhook triggers.
- 🔴 Adjusted the prompt used for a supported same-model continuation after a safety stop, with the usual wording kept for other stops.
- 🔴 Kept pending inputs and newly recorded tool results when a late permission check is resolved without an answer and the conversation can continue.
- 🔴 Removed the separate transcript notice that said a replayed tool call was allowed after restart.
- 🔴 Resolved subagents' model and availability before a start or resume rather than picking again in the agent. Requests can carry an explicit model where needed, including nested agents on a non-default parent model.
- 🔴 Refused to start a subagent if its chosen model became unavailable under organization settings before it began, and asked the user to start it again.
- 🔴 Kept withdrawn-file notices that still name files when deciding where to restart an agent's conversation.
- 🔴 Added handback guidance for a restarted agent with no unread messages, pointing the caller to its report or explaining whether to message it or wait.
- 🔴 Let a subagent's report reach its background parent even when the main conversation is also notified. (behind `tengu_moonlit_peacock`, off by default)
- 🔴 Added a notice when another agent's message wakes a finished top-level agent, so the session knows a new report can follow. (behind `tengu_clever_pillow`, on by default)
- 🔴 Kept the next remote notification prompt held through the stop that interrupted an earlier one.
- 🔴 Changed how Claude asks to read outside the working folders in headless sessions: hosts without a permission responder skip the request except in bypass mode.
- 🔴 Warned when a remote machine didn't confirm a background task stop, instead of reporting that the task had stopped.
- 🔴 Let eligible helper-model web searches run at low effort when no other effort was set. (behind `tengu_plum_vx3`, off by default)
- 🔴 Made language-server lookups decline to read a file when they can't safely verify the path, and explain that failure for a direct lookup.
- 🔴 Stopped a worktree removal if the session has already left the worktree when its permission check runs.
- 🔴 Made context assembly willing to reuse the previous memory section when the session's record of file access is incomplete.
- 🔴 Made memory writes that rely on the current worktree permission check ask again when the target couldn't be fully resolved.
- 🔴 Made the too-long prompt notice distinguish messages sent by the user from messages injected by another source; for the latter, Claude is told not to act on the two remaining fragments.
- 🔴 Fixed when a shortened prompt takes effect in the request history, so a later failure doesn't rewrite an earlier request that still contained the full text.
- 🔴 Included later queued tools when deciding whether work from a streamed response can still run after the stream ends. (behind `tengu_pewter_samphire`, on by default)
- 🔴 Explained to team members that model usage credits are purchased separately from the plan when the extra-use notice applies.
- 🔴 Let a large non-user message use output recovery's shortening path even when it doesn't reach the normal per-message length threshold.
- 🔴 Recognized a pasted text attachment when applying the special context handling for tagged prompts, even if the user row only refers to the paste.
- 🔴 Rejected saved worktree paths Claude can't verify as local when resuming, and explained why that worktree couldn't be reused.
- 🔴 Applied the launch folder's relative `--add-dir` rules when restoring a saved worktree, including from the resume picker.
- 🔴 Prepared attached documents for reading when an eligible session submits a direct user prompt, so the files can be read without waiting for a separate model request.
- 🔴 Made the cloud Artifact file list distinguish published files that can't be read through the session from an artifact that has no file list.
- 🔴 Avoided using stale account flags to offer the Claude API skill after a sign-in change.
- 🔴 Looked up the current Git repository before routing `/code-review` requests that will post or comment when the supplied target isn't already an explicit URL or merge request.
- 🔴 Showed the effort and limit that `/code-review` will reuse when the user types its arguments.
- 🔴 Pointed users to `/status` from `/doctor` so they can check their setup even if Claude is unreachable.
- 🔴 Updated the history shown by the built-in release notes to add 2.1.326 and drop 2.1.323, 2.1.322 and 2.1.321.
- 🔴 Used the ordinary missing-file notice for an Artifact file that's known to be absent from the served frame, when the cloud mount flag enables that distinction.
- 🔴 Included agent IDs and isolation type in the shortened task list when the full list is too large to send. (behind `tengu_rippling_orbit`, off by default)
- 🔴 Sent a finished dynamic workflow's notice as a child completion when the workflow belongs to a subagent.
- 🔴 Refused to adopt background task paths that Claude Code can't reliably verify, with a separate error for a link whose target can't be checked.
- 🔴 Kept the launch request from setting the session title from the prompt when the session was launched by another session.
- 🔴 Skipped updating repository-provided plugins from their command source when a pre-started process can't tell which checkout the plugin belongs to.
- 🔴 Updated the key chord timeout notice to separate the cancelled chord from the reason.
- 🔴 Adjusted terminal key hints: hid list navigation where there's no list, showed more actions assigned to Escape and combined related shortcuts.
- 🔴 Kept the end-of-line shortcut from falling onto the next display row when vertical navigation has just landed short of its remembered column.
- 🔴 Repainted a scroll box after its React state changes so a queued scroll can show up on screen.
- 🔴 Simplified the task-list overflow count when every hidden task has the same status: the summary shows the count once, followed by `in progress`, `open`, or `done`.
- 🔴 Updated the background-session list layout: the full view can wrap the Claude Code heading, the introductory copy has more side padding, and the resume and new-session settings panels drop their borders in simple view.
- 🔴 Kept the background-session row's screen-reader cursor marker while renaming and stopped announcing a group header as selected while its group name is being edited.
- 🔴 Dismissed the background-session list's notice when navigating with arrows, Home, End, Page Up, Page Down, Ctrl+N, or Ctrl+P.
- 🔴 Put short hints on the same line as terminal status notices, after a middle dot, while keeping longer details on their own line.
- 🔴 Updated select-menu rows to fit long labels and descriptions, show a dim focus marker when a disabled picker asks for one, and show overflow hints when more choices are offscreen.
- 🔴 Added rebindable page navigation to multi-select lists when their choices don't all fit; question prompts keep their direct Page Up and Page Down handling.
- 🔴 Changed terminal panel spacing: titleless panels don't reserve a title row, and a panel inside another framed panel doesn't add a second frame.
- 🔴 Stopped the background-session picker and renderer-error recovery from starting a new loop or recovery path after shutdown had already begun.
- 🔴 Changed `/voice` confirmations to say when voice mode was turned on or off.
- 🔴 Updated `/autocompact`'s invalid-value help to say the window must be at least 100k tokens, dropping the old 1M upper limit from the message.
- 🔴 Changed Ultrareview's draft `REVIEW.md` refusals to tell users they can omit `--review-md` or unset `CLAUDE_CODE_ULTRAREVIEW_REVIEW_MD_FILE` to run without a draft.
- 🔴 Used Desktop to fetch connector file offers for its connections. Results distinguish a refused transfer from one that needs the user to sign in to the app again.
- 🔴 Spread out connector download retries after the relay reports it's too busy, adding up to 25% to its retry delay.
- 🔴 Normalized line endings before shortening captured PowerShell script output, so the line breaks and omitted-character count agree.
- 🔴 Added plugin-backed skills to the claude.ai enabled-skill listing when that inventory is enabled, skipping skills already returned by claude.ai.
- 🔴 Used the session's creation time as the activity timestamp when listing cloud peers with no recorded last activity. (behind `tengu_ccr_v2_session_crud_cli`, on by default)
- 🔴 Arranged scheduled-task details in labeled rows so the cron schedule, directory, prompt, and limits are easier to scan.
- 🔴 Reworked the background service screen with labeled status and unattended-command sections, clearer focus and empty state, first and last row shortcuts and current key bindings in the hint line.
- 🔴 Moved settings-change review to the standard selectable list, with a Done row and a shortcut for closing the dialog.
- 🔴 Kept empty selection lists in the regular focus and pointer handling and put the clipped-row count next to the heading of its list.
- 🔴 Updated the Ultrareview evaluation report to schema version 3 and split counts for pull requests excluded during preparation from reviews held back by `--max-cost`.
- 🔴 Rounded Ultrareview evaluation durations up to whole minutes, with a minimum of one minute.
- 🔴 Gave Ultrareview a separate result when a review process finishes without a list of findings.
- 🔴 Simplified the `/status` warning when transcript cleanup is paused because `cleanupPeriodDays` couldn't be read.
- 🔴 Clarified the unused API key row in `/status`: `claude -p` uses the key even when the interactive session doesn't.
- 🔴 Put the Exit and Reset choices directly under the file name and explanation in the invalid-settings dialog.
- 🔴 Shortened the workspace-change fallback notices in connected sessions; the view still shows per-turn changes when it can't load the workspace diff.
- 🔴 Showed the reason for a refused connector file in compact transcripts and live progress, wrapping long refusals instead of clipping them.
- 🔴 Updated the usage-limit footer when the user declines to wait for the current reset.
- 🔴 Used the standard fallback status symbol if an explicit override is empty in a terminal running with a screen reader.
- 🔴 Shortened the exit-code 127 hint for a failed `PreToolUse` hook. It still says the check may not have run and points to `/hooks`.
- 🔴 Changed the collapsed tool summary to list permission denials separately from actions the user rejected. A single denial can also name where the permission rule came from.
- 🔴 Added a session-page link below successful file sends in the terminal when the conversation has a remote session page.
- 🔴 Moved the fullscreen overflow arrows one column to the right.
- 🔴 Clarified `/skills` messages for project-level changes, empty results and skills disabled by safe mode. Turning off safe mode restores customizations.
- 🔴 Kept Anthropic-only warnings visible in demo mode, where other notices marked Anthropic-only stay hidden.
- 🔴 Fixed off-screen startup notices in the flicker-free renderer so they can update as soon as scrolling brings them back into view.
- 🔴 Changed resume notices for background agents so a passive notice can wait for the next turn without starting one.
- 🔴 Changed the resumed-session context estimate to apply the same compaction-window adjustment as the live token budget.
- 🔴 Named the source of an ask rule in permission prompts and shortened the explanation, including when the rule overrides auto mode.
- 🔴 Showed a disabled “don't ask again” choice in terminal permission prompts, including attached sessions, when that option would normally be offered but this request needs a fresh answer. No stays in the usual place.
- 🔴 Added a sign-in error for plugin publishing when the saved sign-in lacks publishing access, for both plugin uploads and organization-library submissions. The message points to signing in again in the Claude app or using a signed-in session.
- 🔴 Let interactive slash commands display a short notice when the user runs them, including logging and setup-check notices, whether the command is entered alone or stacked with another.
- 🔴 Spelled out `tokens` instead of `tok` in agent and workflow token totals, including the Dynamic workflows run list.
- 🔴 Clarified the workflow save dialog's Tab hint and showed “Saving workflow…” while the file is written.
- 🔴 Changed the backgrounding progress message to “Moving to the background…” and put the warnings about what will stop or carry over in the confirmation body.
- 🔴 Reworded the notices when accepting the fullscreen offer: one says it’s restarting, and the other says a restart would drop new work.
- 🔴 Cleared the pre-compaction file-read history on rewind, including in-place rewinds, so Edit can't rely on reads from a discarded turn.
- 🔴 Kept the “press again to exit” hint visible in the rewind dialog while a second quit keypress is pending.
- 🔴 Skipped a duplicate restart of an adopted or orphaned background agent when another restart is already in progress, instead of treating it as a failed restart.
- 🔴 Changed `/fast` to show an information message for ordinary reasons fast mode isn’t available, while keeping the error treatment for network and unknown failures.
- 🔴 Stopped guessing whether to edit or fix setup from the wording of an unknown HTTP 400 or invalid-request error. The terminal leaves off recovery advice when it can’t classify that failure.
- 🔴 Made the prompt-history picker’s empty messages more specific: it says when it’s loading prompts, when there aren’t any yet, and which query didn’t match.
- 🔴 Kept selection in view as shared pickers shrink, showed offscreen result counts, and kept shortcut hints in place when search turns up no rows.
- 🔴 Spelled out “change scope” in the prompt-history picker’s keyboard guide when there’s room.
- 🔴 Reworded `/model` instructions for setting a default, adjusting effort, and seeing which model is active for this session only.
- 🔴 Updated the prompt marker’s accent as the current team color changes in agent teams. Bash and viewed-agent marker colors work as before.
- 🔴 Made the transcript’s queued-message panel use the full width, while keeping the “Waiting to be delivered” list in the same place.
- 🔴 Stopped adding dismissal messages to the transcript when closing cloud session, shell, monitor and several other Background detail views.
- 🔴 Focused the keep option in the Ultraplan stop confirmation. It now says “No, keep plan” if stopping would discard a plan, or “No, keep running” otherwise.
- 🔴 Made “No, keep running” the first and initially selected answer when asked whether to stop an Ultrareview.
- 🔴 Distinguished a running shell or monitor with “No output yet” from one that finished with “No output.”
- 🔴 Showed the “stop all agents” shortcut on a stoppable local-agent row when more than one agent is running, rather than requiring that row to also offer a foreground action.
- 🔴 Added a hint to the empty local Dynamic workflows view telling you to ask Claude to run a workflow.
- 🔴 Showed the Back action in Dynamic workflows details when there are other runs or a list to return to.
- 🔴 Counted the loaded session’s size when showing how close you are to compaction, so the warning follows the working context limit.
- 🔴 Flattened multiline environment-hook notices into one line above the prompt.
- 🔴 Simplified the terminal’s signed-out notice to point to `/login`; it no longer suggests `/config` for `ANTHROPIC_API_KEY` on eligible installations.
- 🔴 Used the active theme’s suggestion color for feedback and survey choices instead of fixed cyan.
- 🔴 Reworked the survey question and answer review screens to put helper text directly under the question and separate the answer area, warnings and keyboard guide.
- 🔴 Removed the Cancel row from the usage credit purchase amount list; use Escape to go back.
- 🔴 Rearranged the Usage credits menu to put the balance and auto-reload status beside the payment method and say “Change auto-reload” if it’s already on.
- 🔴 Added titles to the progress screens for loading usage credits, changing the spend limit, and turning off auto-reload.
- 🔴 Restyled the optional-input label and the note about what gets sent without review in feedback forms.
- 🔴 Included the compact suggestion and dialog panels in prompt-overlay tracking, so their position is available to other parts of the terminal layout.
- 🔴 Made the cloud Dynamic workflows empty view put its caveat about runs that ended before attachment on a separate line.
- 🔴 Lowercased the opening words of the Option as Meta hint when the terminal displays it.
- 🔴 Used the common success style when showing that a transcript was shared or its bundle was saved.
- 🔴 Matched the confirmation for turning on Usage credits to the other credit panels, without changing what the confirmation agrees to.
- 🔴 Rearranged usage-credit checkout to show the payment method and charge notice together under the total.
- 🔴 Changed the typed `yes` check for purchases, high monthly limits and auto-reload to show an error beside the field after a wrong answer and clear it as soon as you edit again.
- 🔴 Changed the monthly usage-credit limit form to show the formatted amount under the entry and put Set limit, Set to unlimited and Cancel in a vertical menu.
- 🔴 Changed the usage-credit auto-reload form to put the reload estimate and validation messages beside the fields, with a vertical menu for saving, turning it off or canceling.
- 🔴 Changed terminal dialog spacing and color across usage-credit status and requests, IDE setup, Remote Control, session resume, computer-use permissions, question notes, auto-mode setup, browser setup, and file previews.
- 🔴 Changed the final AskUserQuestion screen to say `Some questions are unanswered` when applicable and go straight from the answer review to the submit choices.
- 🔴 Removed the separate Enter and Escape key guide from the auto-mode rule proposal dialog.
- 🔴 Fixed the Up arrow in unnumbered Bash, PowerShell, web-fetch and file permission menus to stay at the first option instead of jumping to the last.
- 🔴 Changed the notice after saving a plan to `plan saved`.
- 🔴 Fixed `/rewind` so the prompt can run it immediately even when the command uses the local text path.
- 🔴 Changed prompt review to wait when a submission needs an asynchronous check, and to let the check tell whether you've already changed the draft it's looking at.
- 🔴 Reworded the plugin auto-update warning to say `/plugin to review` when the in-session reload had errors.
- 🔴 Changed the idle push notification tip to put `/config` before `to turn on`. (behind `tengu_kairos_push_notifications`, off by default)
- 🔴 Changed the diff viewer to show untracked and truncated tags alongside the filename and use a shared layout for restricted, binary, oversized and unstaged files.
- 🔴 Changed the workspace diff list to separate its scrolling cues from the counts of read-denied and undisplayed files, and label the test and generated file toggle with its count.
- 🔴 Changed the terminal dock to reserve its regular width when panes are open rather than trimming that width separately for the transcript viewport.
- 🔴 Changed provider setup to use shared headings and form spacing across the Bedrock, Vertex and cloud gateway screens, including manual profile, project, region, and key entry.
- 🔴 Changed the sign-in and Teleport panels to use the shared headings and spacing for account choices, AWS credential refresh and device sign-in.
- 🔴 Changed SendMessage to say `Not confirmed` if a Claude Desktop session hasn't shown that it started on the message. The result warns that resending may deliver it twice.
- 🔴 Changed the project document refusal for text over 1.9 MB to tell Claude to preserve the existing document and offer to save the full text in smaller documents rather than shortening it without permission.
- 🔴 Rejected Artifact URLs longer than 2,000 characters before parsing them.
- 🔴 Changed Artifact publishing to show the matching Edit ask rule as the approval reason when no stronger file safety check needs to be shown.
- 🔴 Changed Artifact republish approvals triggered by outside comments to cover the sources shown and approved on that prompt, rather than every file sent with the publish. (behind `tengu_vellum_godwit`, on by default)
- 🔴 Added a shortened SHA-256 hash beside non-live files in the Artifact publish file list. (behind `tengu_vellum_godwit`, on by default)
- 🔴 Passed the in-process teammate's output file to the runner and included it in the spawn result so callers know where to find its output.
- 🔴 Changed command-line deny rule checks for new teammates to accept a rule only when it and every rule produced from it are still represented. If the check fails, the error names the rule and points to `permissions.deny`.
- 🔴 Kept Claude Code history and temporary files out of workspace diffs alongside credentials. Outside Git repositories the scanner also skips likely configuration folders and avoids following unsafe paths.
- 🔴 Changed Remote Control file sends to say when delivery isn't confirmed, using the same explanation as message sends. (behind `tengu_send_file`, off by default)
- 🔴 Made Claude in managed sessions wait briefly for files staged on the worker to be saved before asking a paired device to copy them.
- 🔴 Kept a cloud subagent's permission card after a restart only if the worker actually plans to resume that agent.
- 🔴 Told the session when a background agent that was going to resume quietly couldn't start because the worker was interrupted again.
- 🔴 Let a late approval wait until the cloud turn that's already running finishes. A newer user message, a denial, or Stop can cancel the waiting approval. (behind `tengu_settled_wren_killswitch` and `tengu_quizzical_knuth`, on by default)
- 🔴 Let new input take precedence when a denial lands on a cloud permission card that the queued message was going to leave open. Claude still gets the denial reason.
- 🔴 Let some cloud permission prompts be asked again without leaving an old withdrawal notice in the conversation when background input moves the session on. (behind `tengu_settled_wren_killswitch` and `tengu_quizzical_knuth`, on by default)
- 🔴 Kept the tool restrictions attached to a queued SDK prompt when a rewind leaves other prompts waiting behind it.
- 🔴 Allowed slash-prefixed text that isn't a command to move a running subagent to the background like other follow-up messages, when the model and session support that input.
- 🔴 Let the project text-copy check recognize the project folder in a tool's prose prompt, including a folder name at the end of a sentence. A prose mention can use an existing mount but won't create one.
- 🔴 Explained full and partial project text-copy failures to Claude: how many documents weren't copied, when it can still read files directly, and when naming the folder again will retry the copy.
- 🔴 Kept the latest Cowork memory-withdrawal notice when rewinding removes it but leaves conversation before it.
- 🔴 Preserved the current tool deny lists when writing the new transcript marker for an in-place rewind.
- 🔴 Changed the rewind recovery warning when the session can't confirm its restored rows. It now refers to a refused turn, since recovery also covers more than usage limits.
- 🔴 Updated the Claude Docs reminder for a resolved relay to mention both available reply tools when the session can use either one. It still says those replies won't reach the document.
- 🔴 Avoided sending the same background-task stop notice again after a cloud worker restart.
- 🔴 Sent artifact resume warnings to VS Code before the next live turn instead of buffering them with routine session notices.
- 🔴 Warned when a connected cloud session has been archived or deleted, and closed the client normally once the service confirms it.
- 🔴 Let an SDK client attach to a sleeping cloud session without immediately waking its worker when the client can account for all pending permission requests and has no opening changes to make.
- 🔴 Stripped the session ID suffix from the plan name when `--worktree` uses that plan to name a worktree.
- 🔴 Switched the bypass-permissions and development-channel confirmations and several folder-trust and onboarding screens to the shared dialog sections for spacing.
- 🔴 Updated the Login dialog, Chrome extension detection prompt and third-party model upgrade prompt to use the standard dialog styling.
- 🔴 Reworded the voice notices for a connection that isn't ready, an unavailable recorder, and a missing claude.ai login.
- 🔴 Made the first Claude in Chrome dialog show what Enter and Esc will do.
- 🔴 Checked for keys and pastes while preparing the training prompt. When sharing isn't already on, unintended typing or pasted input moves focus to the safer choice rather than turning sharing on.
- 🔴 Kept the training dialog open when it can't confirm a save, showing the error and checking the stored choice before restoring the toggle. A server refusal and an unknown result get different messages.
- 🔴 Changed the guidance in the older training settings dialog: read-only accounts get a close action, and an unresolved value doesn't get the green saved indicator. (behind `tengu_playful_puzzle`, on by default)
- 🔴 Added a reminder that the terms haven't been answered when the notice has set aside input from an in-progress prompt. The user can press Down to start choosing.
- 🔴 Refused to turn training on until the terms sentence can be read in the notice; a terminal that can't fit it says to make the window larger, and OFF is still available.
- 🔴 Warned when a training choice may not have saved because the account changed, and kept the terms notice open after Off if the service still reports On or can't confirm the choice.
- 🔴 Changed the first-run and standalone terms notices to reuse the same document illustration; the newer notice hides it and uses tighter spacing in a terminal smaller than 80 columns or 31 rows.
- 🔴 Reworked the Power-ups tour with the shared lesson list, progress and clearer open, close, done and back actions. Ctrl+C leaves a lesson, and completion and file-reference editor guidance are easier to find.
- 🔴 Adjusted the theme picker's diff preview width to the renderer's actual side padding and put the introductory hint under its title.
- 🔴 Told users to rerun `/terminal-setup` if the first-run terminal setup fails.
- 🔴 Reworded the voice microphone errors for remote sessions and for a microphone SoX couldn't open.
- 🔴 Tightened the resume picker help and session preview. Esc goes back from a preview, empty lists say when no sessions are available and shortcut hints follow the current view.
- 🔴 Cleaned terminal control codes out of cloud resume errors and let Esc leave the standalone error screen without requiring the keyboard-fix setting.
- 🔴 Cleaned up the session title shown while a cloud resume is in progress.
- 🔴 Restyled the cloud resume prompts for a different checkout or an unverified Git remote, using the default dialog color and tighter grouping of related text.
- 🔴 Tightened the layout of the prompts for device enrollment, home folder settings and synced MCP server consent, including prompts to approve changed servers again.
- 🔴 Grouped settings validation errors more tightly above their suggestions and documentation links.
- 🔴 Updated the `/status` footer to show tab navigation and close together, and clarified which rows come from the remote session.
- 🔴 Updated the stable channel picker to use the shared keyboard hints and the usual prompt styling.
- 🔴 Made Config's choice lists reject text pasted or typed by mistake.
- 🔴 Clarified that changing approval for a custom API key also affects open sessions.
- 🔴 Cleaned and shortened theme names in the Config tab's change summary.
- 🔴 Hid an empty server response from the plan usage error, leaving the useful error message.
- 🔴 Moved `/stats` copy status into the footer, which switches between tab navigation and date or copy actions based on focus.
- 🔴 Rearranged the `/stats` Models chart and model cards, and tightened the date selector and loading indicator on both tabs.
- 🔴 Left more space around the Settings tabs on short terminals by making their content area two rows shorter when height is limited.
- 🔴 Clarified GitHub sign-in in `/web-setup` progress and errors. Confirmation choices name whether they'll connect, replace a connection or cancel.
- 🔴 Updated the custom theme editor's footer to distinguish going back from an edit and closing the editor, with the shortcuts grouped together.
- 🔴 Grouped the cloud ultrareview explanation above its confirmation choices, keeping the terms link and “Proceed?” question when those terms apply.
- 🔴 Highlighted slider arrows on hover when there's another value to move to.
- 🔴 Reorganized the scroll speed preview so the terminal and editor values line up above the scroll demo.
- 🔴 Shortened the plugin options form's save button to “Save.”
- 🔴 Clarified `/mcp` sign-in and tool-loading messages, including whether sign-in succeeded but reconnecting still failed.
- 🔴 Labeled the MCP server disable action “Disable in this project.”
- 🔴 Showed the collapse shortcut in the footer when an MCP message has been expanded.
- 🔴 Changed the choice for leaving a plugin's install menu without installing it to “Not now.”
- 🔴 Made long `/plugin` details scroll independently in the dialog, with Page Up and Page Down, a hidden-row count and actions below the details.
- 🔴 Gave commands that install a plugin a separate indented block in that plugin's details.
- 🔴 Kept tab-switching shortcuts available in plugin detail screens that allow switching tabs, such as the plugin's Usage view.
- 🔴 Used `anthropics/claude-plugins-official` as the example in the Add marketplace field and shortened its confirmation text.
- 🔴 Clarified plugin options messages: the form says when it's reading saved options and whether it saved, kept or skipped configuration when turning the plugin on.
- 🔴 Updated the Discover list's marketplace filters and warnings, including when it's showing the last saved catalog because a refresh failed.
- 🔴 Reformatted plugin context costs and the Usage view to show per-turn and on-invocation costs, skill activity and totals in aligned rows.
- 🔴 Used the usual confirmation color for the prompt that asks before running a command from a link.
- 🔴 Renamed marketplace auto-update actions to “Turn on auto-update” and “Turn off auto-update.”
- 🔴 Explained `/plugin` load and component errors separately for unsafe or unreadable paths, invalid manifests or hook files and unavailable dependencies, with next steps where available.
- 🔴 Changed `/plugin` details to group the marketplace, installation level, version or commit, and status. The status row calls out plugins required by the organization, pending updates, and recent use.
- 🔴 Added a Dismiss choice for a plugin flagged as removed from its marketplace. Its details show the marketplace, reason, and date.
- 🔴 Added a Not now choice when removing a plugin that couldn't load. Plugins controlled by managed settings or their marketplace still don't offer removal there.
- 🔴 Changed `/plugin list` to show plugins as on, off, or unable to load, and to point to `/plugin install` when nothing's installed.
- 🔴 Shortened last-used dates in plugin usage reviews, such as `7d ago` instead of `7 days ago`.
- 🔴 Changed unused-skill notices from warnings to informational tips, including the advice to turn synced skills off rather than delete copies that sync will bring back.
- 🔴 Added progress indicators while `/plugin tag` prepares a tag and `/plugin validate` checks files.
- 🔴 Changed how `/plugin marketplace list` labels a local marketplace that hasn't been added and one hosted on claude.ai.
- 🔴 Reworded the plugin error screen's suggestion to restart when Claude Code couldn't load plugins.
- 🔴 Labeled the cancel choice `No, cancel` on the plugin error screen's marketplace removal and plugin uninstall confirmations.
- 🔴 Changed the Plugins tab accents and kept the double-press-to-close prompt visible on its error screen.
- 🔴 Switched the read-only cloud `/skills` dialog to the default dialog accent.
- 🔴 Sized the local `/skills` list for the available terminal space.
- 🔴 Hid duplicate source text on `/skills` rows when the skill's source matches the selected source.
- 🔴 Changed the usage-limit menu to show a progress indicator while the 5-hour limit resets.
- 🔴 Added a separate spend-limit error that asks you to confirm again if removal failed and the service didn't provide a reason.
- 🔴 Reworded the spend-limit confirmation to ask whether to remove or keep the monthly limit.
- 🔴 Renamed the release notes picker's full-history option `All versions` and moved its view shortcut into the key hints.
- 🔴 Changed the release notes list to stop at its first and last entries when you move with the arrow keys.
- 🔴 Changed the Remote Control dialog's key hints to show how to close it, including when reviewing a connection that's already running.
- 🔴 Moved `/remote-env` help into the body of its empty and error screens.
- 🔴 Changed `/remote-env` to show a separator only when both hosted and self-hosted environments are listed.
- 🔴 Changed `/sandbox` to show the active configuration in labeled rows and point to the Mode tab when overrides aren't available because the sandbox is off.
- 🔴 Updated `/sandbox` dependency and override guidance, avoiding repeated icon labels for screen readers and adding current key hints and a link to the sandboxing guide.
- 🔴 Changed `/sandbox` Mode and Overrides to reject stray typing and keep the selected row marked when focus moves to the tab bar.
- 🔴 Reworded `/sandbox` errors for project settings that can't be written, settings overridden by policy, unsupported platforms, and missing or unknown subcommands.
- 🔴 Changed `/mobile` to put its title, phone link, and close hint in the shared dialog layout. It still skips the QR code in screen-reader mode.
- 🔴 Changed the Guest passes dialog to show the number left under its title, wrap the pass statuses, and show a copy-link hint only when a link is available.
- 🔴 Added an example path to the add-directory prompt and made Tab and arrow-key hints appear only when path suggestions are available.
- 🔴 Kept the last highlighted row marked while focus is on the tab bar in `/permissions`, including in the tool lists, Workspace, and Recently blocked.
- 🔴 Put `Add directory` first in the `/permissions` Workspace tab and moved the starting directory into a labeled row.
- 🔴 Returned focus to the selected directory when you cancel its removal in `/permissions`.
- 🔴 Stripped terminal control codes and invisible characters from workspace paths shown in `/permissions` confirmations and results.
- 🔴 Labeled the `/permissions` removal and deletion choices with the action, such as `Yes, remove` or `Yes, delete`, and made the arrow keys stop at the ends of those confirmations.
- 🔴 Told users to press Enter to continue when typing a tool rule in `/permissions`, and clarified the rule dialogs' titles.
- 🔴 Changed `/permissions` confirmations for built-in auto mode rules to use a warning when actions make them more permissive. Esc returns to the rule details; the editable rule menu has Cancel.
- 🔴 Shortened the cut-off rule text in `/permissions` to show how many lines or characters aren't visible, with a pointer to the editor for the full built-in rule.
- 🔴 Reworded the auto mode rule editor's messages for when it couldn't open the editor, write the file, or read anything back.
- 🔴 Updated the `/permissions` Allow tab to explain why rules that don't apply in auto mode aren't listed.
- 🔴 Changed `/permissions` to show shortcut hints for the active tab, including retry or approve in Recently blocked and search where it's available.
- 🔴 Clarified the confirmation to replace the built-in auto mode environment and the error shown if `/permissions` can't save it.
- 🔴 Changed the spacing around the current plan and editor hint in `/plan`.
- 🔴 Changed `/loop` form spacing around the interval and prompt fields and compacted the active-loop list.
- 🔴 Changed `/mcp` on short terminals to reserve another line around the server list when the old view wouldn't have shown its debug footer.
- 🔴 Kept the last highlighted MCP server selected when you back out to the server list.
- 🔴 Removed the generic debug hint from the `/mcp` server list when a server fails. Its docs link stays.
- 🔴 Changed `/mcp` enable and disable results to say which servers were turned on or off.
- 🔴 Removed the separate pending-approval result from the terminal `/mcp` toggle command.
- 🔴 Returned focus to the last file you opened when you back out to the cloud `/memory` file list.
- 🔴 Changed `/memory` loading messages to distinguish loading the file list from reading a cloud file, and moved the safe-mode explanation into an info notice.
- 🔴 Reworked MCP task details around the tool name, labeled status and ID rows, and visible close, back and stop shortcuts. Esc goes back.
- 🔴 Reworded the cloud `/model` notices for an unavailable model list or a switch you've stopped waiting for.
- 🔴 Reworked `/goal` details to label the active or paused state, goal and latest check. Token counts start once the goal uses tokens, and completion shows the condition, success and how to start another goal.
- 🔴 Added a `SKILL.md` creation tip when the custom command list in `/help` is empty.
- 🔴 Changed `/help` tab accents and put the available close shortcut or double-press prompt in the dialog footer.
- 🔴 Changed `/hooks` to group its read-only and safe-mode guidance with the applicable warning or information message.
- 🔴 Shortened `/hooks` matcher dialog titles to the event name.
- 🔴 Reworded `/ide`'s auto-connect choices to say whether you're turning it on, keeping it, or connecting manually.
- 🔴 Moved `/ide` setup instructions under its empty state and made its one-session VS Code limit an informational tip.
- 🔴 Reworded `/ide` setup, cancellation, and IDE detection messages, and changed the connecting message's accent.
- 🔴 Changed the usage reset offer and confirmation to use the dialog's default accent.
- 🔴 Added a progress indicator while the usage-limit menu sends an extra-usage request to an admin.
- 🔴 Changed the default environment picker to say `Saving default environment…` while it saves your choice.
- 🔴 Changed interactive `/export` to shorten long destination paths in its messages.
- 🔴 Simplified `/feedback` notices to `Feedback cancelled`, `Couldn't send feedback` and `Describe the issue first.`
- 🔴 Changed the `/desktop` handoff error dialog to use the standard close keys instead of dismissing on ordinary typing.
- 🔴 Changed the ultracode picker to show the current organization's restriction message instead of the generic warning.
- 🔴 Changed `/chrome`'s confirmation after you switch browsers to say `Switched to browser` rather than `Now using browser`.
- 🔴 Changed `/chrome` to label the default toggle with what it'll do and show reconnecting as a loading state.
- 🔴 Changed the `/chrome` browser picker to put focus back on `Choose browser` when you go back.
- 🔴 Shortened `/chrome`'s sign-in, restart, and extension setup instructions and put the per-session flags next to the extension's site access guidance.
- 🔴 Renamed the copy picker to `Copy response` and made its labels for quoted or multi-line selections easier to scan.
- 🔴 Changed `/advisor` to label its experimental status separately from the menu title.
- 🔴 Changed `/artifacts` to keep a blank artifact visible in the current list after you unpin it or a pin attempt fails.
- 🔴 Fit `/artifacts` lists and action menus to the available terminal space and showed `Esc to close` in the gallery footer.
- 🔴 Simplified the auto-mode setup wizard's key hints, including the prompt to continue after saving your answers.
- 🔴 Fixed `/autofix-pr` to report a cancellation as cancelled if you stop it while it's cleaning up after the GitHub subscription failed. (behind `tengu_federated_flask`, off by default)
- 🔴 Changed the fork confirmation to use the permission color for its prompt and italic text for the secondary note.
- 🔴 Updated the layout of Design sign-in, `/add-dir`, cloud plugin consent, Continue in Projects, MCP import from Claude Desktop, and `/diff` to match the newer dialog style.
- 🔴 Fixed the truncated Write preview's hidden-line count so it doesn't count the final newline as another line, and added the hint for expanding the preview.
- 🔴 Updated connector message and session-inbox transcript layout, including the inbound marker and indentation under a speaker's label.
- 🔴 Refreshed the Git remote used by the cloud session picker when no repository URL was provided, so it can pick up a remote URL change without a restart.
- 🔴 Changed the Data privacy message for a switch blocked by account or organization policy to explain that it can't be turned on for this account.
- 🔴 Kept eligible messages in a separate turn when they come through the synthetic cloud host path or are put back on the local queue after waiting for permission.
- 🔴 Told Claude when a resumed subagent's new reply couldn't be delivered through its earlier report, and explained whether to send another message or wait for its background work to finish.
- 🔴 Kept PNG output when it fits the smaller image budget or when converting risks losing pixel detail.
- 🟢 Matched Config's nested pickers and footer to the current view, including Output style, Language and Notifications. Search, save, back and tab shortcuts appear where they apply; sliders keep the Left and Right hints even in narrow terminals.
- 🟢 Displayed notices from `FileChanged` and `CwdChanged` hooks in the terminal. Failed outputs with error text are grouped under the hook's event name.
- 🟢 Clarified the sign-in error in Console-only sessions: `/login` needs a Console account, and the message points to the Console or an administrator to get one.
- 🟢 Started a PowerShell parser ahead of eligible commands and checked that each parsed result belongs to the command that requested it. A failed warm start can still fall back to a new parser.
- 🟢 Refused to run Bash commands whose expanded arguments, environment values, or redirect paths are too large for the permission check, with a message explaining the limit.
- 🟢 Stopped compaction summaries with many unclosed analysis or summary tags from stalling the tag scan.
- 🟢 Kept only the worktree for some relative `--add-dir` values above the launch directory, such as `..`, rather than granting that parent path in the moved session.
- 🟢 Distinguished a Stop hook that prevented continuation, one that couldn't run, and one that ran and reported a problem. The warning identifies the affected hook even when several ran and shows `Stop hook failed` instead of `Stop hook reported a problem` when it couldn't run. The transcript keeps the reason and any extra text.
- 🟢 Sped up the Bash tool’s changed-file diff by skipping extra reads of files already seen in the same snapshot and avoiding index work during tree comparisons. (behind `tengu_snug_catmull`, on by default)
- 🟢 Kept link destinations readable when older tmux runs inside Windows Terminal, or when VTE reports version 0.50.0 as `5000`. Hyperlink detection now uses the environment passed to Claude Code.
- 🟢 Fixed Ghostty redraws where erasing previously wrapped rows left them joined in scrollback.
- 🟢 Used shorter notes to write background-agent progress briefs on Amazon Bedrock, Google Vertex AI, and Microsoft Foundry.
- 🟢 Reported when a `UserPromptSubmit` hook blocked or couldn’t check a cloud message sent during another turn. The saved report omits the hook’s private wording.
- 🟢 Ignored stray keys briefly after `fg` resumes the terminal, even in lists that would normally accept immediate input. Esc still works where available.
- 🟢 Removed a second divider and indent from nested dialogs, including plugin and sign-in steps, and reduced the padding inside fullscreen dialogs.
- 🟢 Kept eligible `later` messages from other sessions out of the current turn in Remote Control and Desktop sessions, so they wait to start a turn of their own.
- 🟢 Accepted unused `prompt`, `limit` and `label` on Artifact calls that don't need them, explaining why each was ignored: file or asset reads, file or asset listings, and asset uploads, respectively.
- 🟢 Told Claude which design-system cards to read after getting a README without those cards attached, using the paths or stored collection entries that apply to that design system.
- 🟢 Asked before `Get-Module` and equivalent PowerShell module discovery when the safety switch is on, including calls the parser can't fully read, so users can review commands that may load module manifests.
- 🟢 Refused new artifacts without prompting when the organization requires approval but nobody can answer, including in `dontAsk` mode and ineligible SDK turns. Claude explains where to make the request instead.
- 🟢 Reported unconfirmed messages to cloud or Remote Control sessions as possibly delivered instead of failed. Automatic retry is limited to the path that recognizes a repeat send of the same message.
- 🟢 Fixed the resume picker’s row count by using the height of its footer instead of a fixed guess, so it can show all the sessions that fit.
- 🟢 Restored the selected tool when returning from its details to the server's tool list in `/mcp` or `/plugin`.
- 🟢 Changed `/desktop` to a keyboard-selectable download list when the app is missing or needs an update, with `Not now` selected first.
- 🟢 Refused Artifact calls once settings disable Artifacts, even if the tool was already available. Error 25, `switched_off`, tells Claude to explain the setting instead of retrying.
- 🟢 Fixed working-folder permission checks to revisit their path decisions once the session's folders finish resolving, and to use the resulting folder names when checking restrictions.
- 🟢 Left most launch prompts from sessions started by another Claude Code session out of prompt history. In those sessions, input submitted outside the terminal composer isn't saved there.
- 🟢 Told users to continue from the terminal when a paused session needs a fresh login.
- 🟢 Fixed truncated terminal previews to count visible text without color codes and still show that at least one line was omitted.
- 🟢 Let Code Mode start alongside installed mods that hook tool calls. The organization, credential, provider and feature checks still apply, including managed settings that turn Code Mode off.
- 🟢 Fixed deck and design setup suggesting token files that don't exist. When the published file list is available, Claude gets the paths that exist and a note about missing files.
- 🟢 Skipped the terminal setup prompt on first run in Remote SSH sessions detected for VS Code, Cursor, Windsurf, and Devin.
- 🟢 Let the rewind handler choose the transcript checkpoint once the replacement rows are written, so a rewind can keep its chosen leaf even when it has an older timestamp.
- 🟢 Changed Bash's explanation when it can't rule out a deletion: Claude may restate a non-deleting command in plain shell syntax, but it must wait for approval before attempting any deletion.
- 🟢 Warned that a future release will remove the local auto-mode fallback for ineligible sessions and incompatible gateways. The fallback still works in this release.
- 🟢 Made restricted workspace checks reject an unresolved path even when the spelling looks like it's inside an allowed directory.
- 🟢 Added Git prompt guidance to avoid committing files likely to contain secrets and to warn the user if they request it.
- 🟢 Made the implicit-profile OAuth error tell Console users to sign in with a Console account when Claude Code has the wrong kind of saved sign-in.
- 🟢 Made summary cleanup recognize the first complete analysis and summary tags even when they aren't at the start of a line.
- 🟢 Ran eligible early Bash snapshots in a separate process so shell initialization doesn't hold up launch.
- 🟢 Displayed `SessionEnd` hook failures that returned error text in the terminal, not just in the shutdown logs. Cancelled hooks don't show a failure.
- 🟢 Restored terminal selection highlighting when the configured selection color can't be used on a basic 16-color terminal, by applying reverse video rather than toggling it off.
- 🟢 Told the model to ask a reachable subagent for its finished result after compaction if the output file isn't readable and the model doesn't have the result.
- 🟢 Kept refreshing terminal dimensions after a suspended session resumes even when sending the resize signal fails.
- 🟢 Made background launches resolve `--add-dir` arguments from the launching folder before handing them off, including launches that switch into a worktree.
- 🟢 Started eligible shell tools preparing their environment as soon as Claude began streaming the call instead of waiting for the complete arguments.
- 🟢 Cleared an ignored number when the user pressed Enter too early in a screen-reader picker, so the next number can still answer the prompt.
- 🟢 Told Claude to avoid committing likely secret files when using PowerShell, and to warn the user if asked to commit them.
- 🟢 Fitted scheduled-task form fields to the space in their row instead of assuming 60 columns, including when masking a saved value.
- 🟢 Directed users to Console sign-in when `forceLoginMethod` requires it, instead of suggesting a subscription after rejecting the account.
- 🟢 Kept the system, thinking, and inbound-message markers out of fullscreen text selections, including the collapsed incoming-message pointer.
- 🟢 Restored failure notices for `SubagentStop` hooks in the terminal, including both command failures and hook evaluation errors. `Stop` hook errors still have their own display. (behind `tengu_amber_flint`, on by default)
- 🟢 Added line counts above and below the visible part of a long unmasked text field, so long pastes don't disappear out of view without a hint.
- 🟢 Reworked workflow details for narrow terminals: tables get more room and line up with the surrounding content, and the footer keeps the most useful shortcuts when space runs out.
- 🟢 Kept a fullscreen terminal on its alternate screen when pressing Left to open the background session list, so the shell doesn’t flash during the switch. (behind `tengu_bg_leftarrow_inprocess`, on by default)
- 🟢 Put the exit and close shortcuts first in the fullscreen transcript’s `?` help so they don’t disappear off the bottom of short windows.
- 🟢 Stopped suggesting an immediate resend for API errors identified as account limits, even when that error would otherwise offer a retry.
- 🟢 Changed MCP form previews to wrap a long single-line question before shortening it to fit the panel, so more of the question is readable.
- 🟢 Fixed AskUserQuestion's editor hint to show the configured external-editor shortcut instead of always saying Ctrl+G.
- 🟢 Fixed the exit notice about keeping a worktree so it still prints when there aren't transcript rows to show.
- 🟢 Stopped offering an always-allow choice for Artifact publish in delegated or other non-human turns, including turns where a compaction makes the earlier approval context unclear. (behind `tengu_vellum_godwit`, on by default)
- 🟢 Kept long branch and folder labels from pushing the selected session offscreen in the resume picker by limiting each hint row to one line.
- 🟢 Showed cloud resume errors above the list in the embedded `/teleport` picker, so the user can leave it open and choose another session.
- 🟢 Removed numbering from Config's choice lists outside screen reader mode.
- 🟢 Reported the new theme when leaving Config by checking the currently selected theme rather than relying on a settings snapshot.
- 🟢 Stopped a marketplace's details from repeating a list-wide fetch warning when that marketplace still has catalog information to show.
- 🟢 Removed numbers from the `/sandbox` Mode and Overrides choices outside screen-reader mode; use the arrow keys and Enter.
- 🟢 Fixed `/chrome` so pressing Ctrl+C twice closes the dialog even from the browser list.
- 🟢 Turned off mouse tracking during the fullscreen switch to the background agents list and restored terminal input and redrawing when the switch screen goes away. Quitting during that switch no longer leaves mouse codes or a blank screen in the shell. (behind `tengu_bg_leftarrow_inprocess`, on by default)
- 🟢 Sped up some large PNG reads by testing palette compression on a small sample before spending time converting the whole image.
- 🔴 Capitalized the Bash approval warning for environment assignments the parser can't check when `permissions.blockReadsOutsideWorkingDirectories` is on.
- 🔴 Added a serial comma to the `/status` command description shown in command help.
- 🔴 Changed the model's `TaskCreate` recovery hint for a malformed multi-task call to ask for a short title. That wording only applies when `tengu_composed_pillow` is on; it defaults off.
- 🔴 Reworded the IDE extension install error shown when the install command fails without a specific error or exit status. It now says the command couldn't be run or didn't finish.
- 🔴 Kept the main session's automatic compaction-window increase from carrying over to ordinary subagents. When `tengu_nested_kite` is on (the default) and the parent qualifies for an increase, subagents start from the setting before the increase and can apply their own `autoCompactWindow`. The built-in fork still takes the full window.
- 🔴 Reworded `/plugin list` completions for `--enabled` and `--disabled` to refer to plugins that are on or off.
- 🔴 Reworded `/plugin` command completions to say `enable` turns on an installed plugin and `disable` turns one off.
- 🔴 Recognized three more line separators in quoted tool results and errors in the detailed report Claude gets when a Code Mode script stops. Code Mode's off by default; `CLAUDE_CODE_EXPERIMENTAL_CODE_MODE=1` requests it.
- 🔴 Shortened the keyboard hints in the Background task list. The list now shows keys and their actions without “to” between them, using the compact style by default.
- 🔴 Showed the double-press-to-close hint on more `/plugin` screens, including the marketplace list and the installed-plugins loading screen, after the first close keypress.

</details>

<details>
<summary>577 admin and integration changes</summary>

- 🔴 Added Desktop host fields for enabling a load-based auto-compact increase, recording the user's answer to an offer, and reporting the old and raised window sizes in live context.
- 🔴 Moved plugin hook matcher tests to the check for their actual worker, including isolated account-plugin hosts, without running the regex during load. The host logs and skips batches over 262,144 characters including test overhead.
- 🔴 Checked custom identity-provider CA bundles before giving them to TLS: dropped ambiguous text with a warning, rejected bundles without a usable certificate, and dropped chains that no longer reach a trusted root after removing distrusted certificates.
- 🔴 Added separate approval tracking for an organization's plugin listings and for settings that enable plugins, so a decision about listing plugins doesn't have to stand in for consent to enable them.
- 🔴 Included plugins already enabled by an organization when comparing what a new marketplace entry would enable, except when the entry points to a local file or directory.
- 🔴 Kept a copy of remote managed settings from before filtering so the CLI can list them before consent and explain which invalid entries, including auto mode rules, were dropped or replaced.
- 🔴 Hid Remote Control folders and refused picks if Claude Code couldn't verify that the live managed settings still called for the same policy helper used at startup.
- 🔴 Stopped prestarted workers applying session-specific choices before claim. `set_model` and an `apply_flag_settings` request containing `model` or `agent` fail until the worker knows the session; hosts can send other settings on their own.
- 🔴 Let plugins replace, append to or insert at the caret in a supporting headless client's draft, with optional styling and caret placement. Replies identify the client button press that prompted the edit.
- 🔴 Added a headless composer request for plugins to paint and clear their own layer of styling without changing the draft, caret, or undo history; styling applies only while the draft still matches.
- 🔴 Added a headless composer request to show a dim suggestion in an empty draft for Tab to accept, from a plugin or Claude Code's own next-prompt suggestion.
- 🔴 Let plugins read selected text from the first attached local client with a selection, including the transcript row when the selection fits inside one; selection requests stay on the local host connection.
- 🔴 Let supporting composers request `prompt.edit` for draft, caret and styling or `prompt.autocomplete` for choices at the caret. Only the newest waiting request of each kind runs its hooks.
- 🔴 Gave headless composer fill, style, suggestion and selection requests five seconds for a reply. Eligible remote sessions can try the next client when one doesn't answer.
- 🔴 Kept cloud sessions opened with an Anthropic Console profile tied to that sign-in's organization. If the machine switches credentials, Claude Code refuses to send to or update the session and says how to sign back in.
- 🔴 Added opt-in `--nested-model-access` on Linux self-hosted runners. Eligible noninteractive children can use the session's private model socket without getting upstream credentials; their own sign-in or provider wins. Operators must list models with `--nested-model-allow` or `SELF_HOSTED_RUNNER_NESTED_MODEL_ALLOW`, including fallback and advisor models. The 200-call default returns HTTP 402 when spent and can be changed with `--nested-model-call-budget` or `SELF_HOSTED_RUNNER_NESTED_MODEL_CALL_BUDGET`; token counts don't use it. The runner cleans up its sockets.
- 🔴 Explained why a nested run can't use its runner's model connection: a non-absolute `CLAUDE_CODE_RUNNER_MODEL_SOCKET`, `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB`, `--sdk-url`, permission bypass, a missing session token or unreadable policy or settings.
- 🔴 Added `allowNestedModelAccessUnderForceLogin` for machines with managed login requirements. The runner's sign-in still has to satisfy the required gateway, login method and `forceLoginOrgUUID`. The organization check waits ten seconds per request and retries server errors up to three times unless told to stop. Refusals distinguish missing or mismatched organizations, incompatible sign-ins and runners too old to report them.
- 🔴 Stopped retrying nested model requests when the socket is gone, refuses the connection or denies access. Errors suggest using a running session or checking socket and sandbox permissions, depending on the cause.
- 🔴 Let a managed Linux launcher request the machine administrator's `apiKeyHelper` for eligible noninteractive HTTPS gateway launches. Startup gateway and credential settings win; the helper won't run if the gateway or headers changed, TLS checks are off, the launch mode doesn't qualify, `CLAUDE_CODE_USE_GATEWAY` requires a launcher token, or another credential already takes precedence. Warnings explain why a managed helper couldn't run.
- 🔴 Checked plugin publishing access separately from session access, trying another eligible sign-in when the host lacks the publishing permission. Organization and workspace mismatches are refusals.
- 🔴 Refreshed runner model credentials from the token file within 60 seconds of expiry and forwarded nested requests with the newest model token available, falling back to the one current on arrival.
- 🔴 Explained in `/status` when the cross-session inbox is listening but peers can't reach it because publishing its key failed.
- 🔴 Treated engine-only connector markers in peer messages as text, so tool-use attribution doesn't mistake them for connector output.
- 🔴 Required marketplace sources, repository hosts and local skills folders to match every managed allowlist, including both `strictKnownMarketplaces` lists when set. A block from either local or organization settings still applies.
- 🔴 Checked the installed copy of a marketplace plugin against its organization listing before letting hooks from that copy run at the organization's trust level.
- 🔴 Reported when a plugin couldn't reload after its hook process died; eligible hooks can run again once it comes back.
- 🔴 Skipped plugin hooks on sensitive tool events when the plugin doesn't already have a matching standing rule for that tool.
- 🔴 Waited for pending mod checks when rebuilding command lists or resuming agents, so a newly listed mod can be considered once the check finishes.
- 🔴 Checked plugin hook modules with the JavaScript engine and refused them if it disagrees with the initial scan, can't validate the build or stops on deeply nested source. Errors name the file and distinguish exports named `then`, top-level `await` in dependencies, import attributes, writes to imports, missing exports and compilation or linking failures.
- 🔴 Validated plugin storage writes as JSON, naming fields that can't be encoded and reporting the size when a value exceeds 4,194,304 characters.
- 🔴 Told Claude to limit verbatim WebSearch excerpts to 125 characters per page when results include plain page text. Code is exempt, and the prompt still forbids quoting song lyrics.
- 🔴 Added independent plain-page-text budgets for main-thread and subagent WebSearch results, with a separate setting for the cloud-runner search proxy.
- 🔴 Told the WebFetch helper to use URLs already seen in the conversation when the URL permission check is on; a new address can need approval or be refused.
- 🔴 Limited headless consent prompts to eligible top-level entrypoints. Background and daemon sessions are treated as noninteractive, nested runs can't use a parent's stdio consent channel, and top-level SDK sessions can ask their host for supported mid-turn prompts.
- 🔴 Skipped the long reconnect schedule for remote MCP servers when TLS certificate verification has been disabled.
- 🔴 Parsed remote managed MCP entries with the regular HTTP and SSE server schemas, after checking that their definitions can be used without reading the user's environment.
- 🔴 Applied the first-party OAuth request path to runner-backed model requests that opt into it.
- 🔴 Parsed fewer ambiguous shell commands as simple commands when the protected parser check is on, so permission checking can send them through the more careful path.
- 🔴 Checked the credential actually used for requests before enabling subscription-only behavior, even if a Claude subscription is still signed in locally.
- 🔴 Used the checked permission view only when the corresponding feature switch is on; other tool calls keep their original permission context.
- 🔴 Used the safe display label when a directory name includes invisible or unusual spacing characters.
- 🔴 Recognized more spellings of broad shell permission rules on Windows before deciding what the rule covers.
- 🔴 Kept mod loading out of session scratchpads and directories laid out like them, including when the mod links into one.
- 🔴 Kept checking earlier spellings of a denied working directory when path resolution can't finish, so a later path change doesn't silently drop the deny rule.
- 🔴 Applied the extra permission checks for raw MCP calls to in-process SDK servers as well as Claude.ai-proxy servers, before sending the calls.
- 🔴 Advertised file support on eligible proxy and SDK MCP connections, including before the old terminal upload prompt could run. Remote tool lists and calls describe supported transfers, and tools whose proxy reads a file reference can still receive it.
- 🔴 Started looking up the cross-session sender address ahead of an eligible human turn, letting the lookup start while MCP clients are still connecting.
- 🔴 Added a ten-second wait for the settings refresh after changing the stored gateway sign-in.
- 🔴 Let eligible API clients read request and response-idle timeouts from the data for their selected model.
- 🔴 Extended protected `.claude` write checks to linked added directories on Linux and WSL under the rollout. Deny rules still apply to the known path when the link can't be checked, and cleanup removes placeholders replaced through those links.
- 🔴 Checked more special Git paths when building the sandbox rules, including entries in the repository's linked-worktree metadata. (behind `bot_protected_tengu_robust_island` and `bot_protected_tengu_squishy_nebula`, on by default)
- 🔴 Stopped sandbox write permissions from standing in for the session's usual permission decision on server-side calls or when the sandbox is enabled but isn't running.
- 🔴 Kept the Artifact publish check in play when outside comments still need a response; the check remembers the content and which sources it has already seen.
- 🔴 Loaded the embedded clipboard module through an unnamed temporary file on Linux when the new loader is selected.
- 🔴 Validated remote MCP replies before using them. Requests fail if there's no reply check, a request for more input reports an unknown outcome, and unrecognized JSON-RPC errors are normalized with a fallback message when needed.
- 🔴 Refused to start a child or teammate when the command line can't carry all the deny rules from its parent.
- 🔴 Kept the working startup proxy when settings later repeat an invalid proxy value, and told the operator why the change didn't take effect.
- 🔴 Skipped automatic HTTPS proxy repair if a separately supplied `GRPC_PROXY` or `grpc_proxy` value also failed to parse.
- 🔴 Added an `ELOOP` error for managed settings paths when Claude Code can't check the symlink target without following it.
- 🔴 Kept remote sessions from moving queued calls away from a server permission check that's still running or has returned a result the next worker can't safely reuse.
- 🔴 Required an individual approval for eligible remote auto-mode Claude Pages and Docs create, update and batch calls when computer access puts them on that path; the prompt can't save an always-allow rule.
- 🔴 Saved prior organization IDs through compaction for local session data-flow records, leaving them out of the remote internal-event copy.
- 🔴 Distinguished sessions launched by another session from ones that followed `/clear` in data-flow records, and marked the source unknown when launch ancestry is missing.
- 🔴 Distinguished sessions launched by another session from sessions that only followed `/clear` when deciding which account-transfer checks to run.
- 🔴 Recorded plugin hook module loads in session data-flow history, even if the module loaded before the current request or transcript write.
- 🔴 Recorded account and organization changes in session data-flow records, including on resume and when earlier records are restored.
- 🔴 Allowed eligible account-plugin hooks in shared `claude-code-server` sessions when their plugin is linked on the tool container and can run in a sandboxed host.
- 🔴 Let HTTPS remote-settings requests report when the connection is ready, including when a pooled connection is reused.
- 🔴 Distinguished Drive file reference uploads from other connector uploads when choosing how to handle the upload.
- 🔴 Made the headless SDK's direct compaction callback reject with a message pointing callers to turn-based compaction instead.
- 🔴 Marked permission decisions made entirely by built-in plugin hooks so later checks can distinguish them from decisions involving other plugins.
- 🔴 Used the linked tool-container copy of a plugin for trust decisions after verifying that it matches the listed copy.
- 🔴 Added a separate schema-repair rollout for streamed browser and remote-device tool arguments. A second check can detect when the streamed text and structured inputs mean different things before running the call.
- 🔴 Combined session data-flow entries when reading the transcript so session records can carry information from the other sessions they refer to.
- 🔴 Removed Python `__pycache__` folders from eligible installed marketplace plugin copies before comparing their contents, so generated bytecode doesn't make an unchanged copy look edited.
- 🔴 Skipped reading external plugin folders on shared server hosts when verifying a listed copy, and reported why it couldn't be read.
- 🔴 Held back repository settings, plugins and their hooks, binaries and language servers in prestarted workers until they know the claimed session folder. The worker explains the wait and rereads the repository after claim.
- 🔴 Recorded where text from hooks came from in session data-flow records, distinguishing official and other plugin hooks from project and user settings.
- 🔴 Added `CLAUDE_CODE_DISABLE_SHELL_HEAD_START` to skip prestarting the PowerShell parser and command shells.
- 🔴 Kept track of which Bash ask and deny rules can be restored on the next worker, and warned when a rule from a permission answer can only be applied on the current worker.
- 🔴 Sent the current shell working directory to supported clients before a main-thread tool call when a post-tool hook has asked for it.
- 🔴 Added stable data-flow IDs for known first-party MCP addresses, so the Design, Pages and developer docs connectors can be recognized when recording tool use.
- 🔴 Recorded session file reads by search, file tools, attachments and plan loading, and linked `Edit` and `Write` changes to the other session whose files they touch. Unknown Markdown paths that could hold earlier session data mark the record incomplete.
- 🔴 Added `CLAUDE_CODE_HOST_AUTH_ASK_AT_START` so a supporting SDK host can supply a missing startup provider credential. It retries once and logs failures, cancellation or a missing token.
- 🔴 Applied the same organization-policy checks to loading plugins from skill folders as to checking whether they're eligible.
- 🔴 Remembered user declines for the current chain of calls so automatic follow-up checks can distinguish them from classifier refusals.
- 🔴 Recorded Artifact reads from page views, conversation text and comment activity in session data-flow history.
- 🔴 Recorded unknown sources separately from local messages and host notices, including externally supplied chat text and quoted agent output.
- 🔴 Distinguished an account limit from other rate limits when overage is unavailable, retry is explicitly refused or the account reaches a spend limit, so the terminal can suggest waiting rather than resending the prompt.
- 🔴 Logged the selected environment and self-hosted runner pool when creating a teleport or remote session, so support can see where the session landed.
- 🔴 Recorded uncertain data origins when WebFetch reads internal Claude service URLs, without treating the public download and status sites the same way.
- 🔴 Recorded when WebFetch passes an Artifact page to a model for summarization.
- 🔴 Offered only eligible linked computer-tool definitions on shared server sessions, and kept the choice consistent for the current request context.
- 🔴 Retried model requests without full-text `claude_md` after an invalid-request HTTP 400, even if the error names another field. If the second request also gets a 400, later requests put the field back.
- 🔴 Gave HTTP 413 its own classifier diagnostic, explaining that the shortened permission context lasts until the next turn or compaction.
- 🔴 Added a hosted rollout for serializing main-turn API requests directly to bytes. It can compare 1% to 100% of requests with the regular serializer and switches back after a mismatch, error or slow-path result.
- 🔴 Reused valid server routing hints on the next eligible model-thread continuation. Claude Code logs and skips malformed hints, drops a hint after two counted failures, and can retry without it and pause hints for 30 minutes when the hint caused a refusal.
- 🔴 Allowed eligible thread continuations over the dedicated self-hosted runner model socket to carry the first-party beta, without enabling it for arbitrary Unix sockets.
- 🔴 Kept permission checks that crashed or didn't finish from being treated as complete when plugin hooks evaluate a tool call.
- 🔴 Recorded suspected nested Claude shell output as an unknown source when it can't be tied to a transcript; logs distinguish the cloud, background and job launch paths involved.
- 🔴 Marked data-flow origin as unknown when a task-tool result can come from a shared or named task list, including `CLAUDE_CODE_TASK_LIST_ID`.
- 🔴 Marked `Fix what /usage flagged.` prompts as referring to information outside the visible conversation in session data-flow records.
- 🔴 Recorded nonempty output from completed pre- and post-compaction hooks as hook text in session data-flow records.
- 🔴 Let a rollout treat more synchronous `UserPromptSubmit` hook runs as interrupted instead of timed out when their remote execution is canceled.
- 🔴 Distinguished MCP-name lists from pattern matchers when reading hook settings, so clients can find the rules that refer to MCP servers by name.
- 🔴 Treated an async callback response as successful without storing a result, and left internal callback success rows out of the transcript.
- 🔴 Refused plugin hooks' substitute answers for protected tool and account-memory calls, including diskless `SendUserFile` and `SendUserMessage`. The error names the affected call.
- 🔴 Recorded writes to local memory and project memory separately, including when a tool couldn't tell the recorder whether it was read-only.
- 🔴 Left the current connectors in place and returned a separate error when a refresh couldn't fetch a complete list.
- 🔴 Checked connector listing eligibility separately from whether Claude Code can dial the connector, so workers loading their session can report a policy or project-approval block.
- 🔴 Read the project's `.mcp.json` directly when checking whether a server already exists, so filtered or disabled server entries still count.
- 🔴 Looked up the configured MCP server by plugin name when reloading a plugin-provided integration, without reloading servers from unapproved project skill folders.
- 🔴 Capped the wait for retry delays from `Retry-After` on plugin `$.model.complete` calls at 60 seconds from the start of the call.
- 🔴 Verified installed copies of listed plugins by folder, manifest name and Git revision before trusting their hooks. Pinned revisions must match, unreadable or changed copies aren't trusted, and a saved hash is written only to the entry that was checked. Host-supplied plugins use their own path.
- 🔴 Recovered plugin hooks in order after a worker dies, keeping still-running modules where possible. Eligible calls can wait for a replacement, which rebuilds lost checks and confirms the hook still applies.
- 🔴 Used the sandboxed plugin worker when the host supplies one, taking precedence over the usual separate-worker or same-thread mode.
- 🔴 Reused running plugin hooks on refresh only if their order, source, folder, approval and settings still match.
- 🔴 Checked whether settings for the requested model had finished loading before sending model details to an SDK client, instead of checking the session's current model by default. (behind `tengu_warm_crescent` and `tengu_deep_shore`, off by default)
- 🔴 Removed stale Git lock files before reusing an idle checkout on a self-hosted runner, so a Git command cut short in an earlier session is less likely to block the next one.
- 🔴 Required Git 2.32 or newer for proxied HTTP(S) clones of Anthropic-managed repositories. The runner warns when Git is too old or unknown and can use `GIT_CONFIG_PARAMETERS` for hooks if it can't read the version.
- 🔴 Carried operator-configured plugin enablement and local-file, local-directory, and GitHub marketplace sources into self-hosted runner sessions, using only the recognized fields.
- 🔴 Left nested model access off if the runner can't verify organization policy in 3 seconds or the session can't use it under policy. Token renewal can end access for the rest of the session if policy changes. Two failed checks pause calls until a check succeeds; after a minute the error stops recommending retries.
- 🔴 Retried missing or refused credentials in the runner's nested model service, reporting when the caller needs a new session after a minute. The CLI no longer treats known runner credential and quota refusals as generic network failures.
- 🔴 Required nested model requests to come from the runner's own session and carry a distinct valid child session ID. Errors differentiate missing, duplicate, invalid and parent IDs.
- 🔴 Refused unsupported nested model requests on self-hosted runners. Unknown routes return HTTP 404; unsupported fields or formats return HTTP 400 with a reason. Requests relying on server-stored files, containers or hosted tool history explain when to send inline files or start a fresh conversation.
- 🔴 Limited a runner's nested model socket to eight active calls and 64 waiting for up to 60 seconds; extra calls get a retryable HTTP 429. Requests over 40 MiB get HTTP 413 and don't spend the call budget.
- 🔴 Filtered request headers and API beta flags before forwarding nested calls, marked them as runner-originated, and kept the session’s additional protection setting on for model requests when enabled.
- 🔴 Returned proxy or network errors to nested clients when the runner can't reach its model API. Redirects return HTTP 502 without forwarding credentials; recognized connection failures, API credential refusals, rate limits and overloads refund a call.
- 🔴 Streamed nested model replies through the runner, cancelling upstream calls on disconnect and dropping uploads or writes after 60 seconds without activity.
- 🔴 Logged whether nested model access opened on a self-hosted runner, why it was refused, and how many generation calls were charged and requests refused when the session ended.
- 🔴 Ran self-hosted session restore before startup and a final save before leaving the runner, so saved uncommitted edits can carry over. Restore failures are reported before launch and failed saves warn that recent work may be lost.
- 🔴 Ran eligible self-hosted cloud setup scripts alongside checkpoint restore, with `CLAUDE_ENV_FILE` for session settings. Startup cleanup reports whether it kept or removed `.environment-setup`.
- 🔴 Removed stale repository link metadata before reusing a kept checkout through the self-hosted runner’s agent proxy, so the next session uses its own checkout.
- 🔴 Waited for managed model policy before resolving the model and its capabilities for delegated agent launches and resumes.
- 🔴 Filtered MCP tools and clients during cross-agent resume, so static servers have to belong to the session or resumed agent before their tools can be used.
- 🔴 Kept the file-specific handling for connector results after a file approval, so an approved send’s result isn’t treated like ordinary untrusted tool text.
- 🔴 Sent the entrypoint and requested version with organization skill downloads, when known, so the service knows which skill version the caller asked for.
- 🔴 Checked `managedSettings` supplied on the first `initialize` row against the ones applied before startup and returned a distinct startup reason if requested settings never arrived.
- 🔴 Reported `initialize_fields_not_delivered` if `--initialize-fields` names an unsupported field, conflicts with `--settings` or `--disallowedTools`, or can't read a usable first `initialize` request. Startup also rejects first lines over 268,435,456 characters or settings it can't apply, such as overly deep values.
- 🔴 Let cloud SDK clients declare supported dialog types on attach and use `cloudStatusBeforeInit` to report the session ID before the worker finishes starting.
- 🔴 Kept a runner’s own tool-container MCP server when policy disables sideload flags, while continuing to apply the separate rules for safe mode, hermetic mode, and enterprise MCP.
- 🔴 Waited up to two seconds for startup settings before initially loading marketplace plugin hooks, and retried hook loading after refreshing entries whose installed copy still needs checking.
- 🔴 Added a background check for enabled managed marketplace plugins whose installed files lack a matching saved hash. Startup doesn't wait: until the files are verified against the recorded commit, the copy stays in the user trust tier. A later startup or reload can use the verified copy at the organization tier.
- 🔴 Deferred ineligible MCP connections in prestarted cloud workers and added `CLAUDE_CODE_SPARE_DEFER_ALL_MCP` to defer all of them. After claim, the worker checks current policy and plugins and explains deferred servers or plugins that need a reload.
- 🔴 Refreshed Claude.ai connectors without reconnecting unchanged servers, removed stale and failed connections, and restored plugin servers that no longer duplicate a connector. Reconnect checks also include configs waiting for a spare claim.
- 🔴 Added `--initialize-fields` for local Agent SDK starts using `--await-initialize` and piped stream JSON. `--initialize-fields=settings` and `--initialize-fields=disallowedTools` accept `initialize.settings` as a JSON object and `initialize.disallowedTools` as a string list. SDK callers opt in with `settingsDelivery: "initialize"` or `disallowedToolsDelivery: "initialize"`; the reply reports `settings_applied` or `disallowed_tools_applied`.
- 🔴 Changed the default renderer rollout to use terminal profiles and per-terminal choices when `CLAUDE_CODE_NEW_RENDERER` isn’t set. Tmux control mode needs to be enabled separately.
- 🔴 Let the old renderer report cursor mismatches when enabled for the terminal, without repainting the screen as the new renderer does.
- 🔴 Let Claude in Chrome continue an organization policy read for up to 13 seconds without holding startup the whole time. Connection reuses or checks the pending result before handing out the browser token.
- 🔴 Stopped advertising connector uploads when organization policy denies the feature. For connectors that can refresh their tool list, waited for a readable policy before advertising uploads.
- 🔴 Changed whether an eligible connector can offer file downloads when organization policy can't be read: Claude checks whether the connector also supports changing its tool list.
- 🔴 Removed the approval shortcut for MCP tools marked `anthropic/readsOwnArtifactsOnly`. Reads from those tools go through the usual same-turn checks before another Artifact action can proceed without asking.
- 🔴 Extended file-upload permission checks to eligible Desktop-hosted MCP connectors.
- 🔴 Added an `include_hearth` request option for worker metadata and requested it when reading the remote project's binding.
- 🔴 Made the Agent SDK reject options it can't send in the session initialization request before starting the subprocess, naming the option that needs fixing.
- 🔴 Warned that an inline `createSdkMcpServer()` under an agent's `mcpServers` is ignored, and pointed Agent SDK users to the top-level `mcpServers` option instead.
- 🔴 Added `rule_usually_offered` to host and Remote Control permission requests so clients can explain an always-allow choice that's normally present but is hidden for this call.
- 🔴 Added an experimental second read for slow cloud files after a configurable 300 to 5,000 milliseconds. It can try the shared analytics connection first, falls back to the regular one and cancels or drains the loser.
- 🔴 Added `CLAUDE_CODE_DISABLE_HELD_MESSAGE_RECOGNITION` so a session can turn off the recognition experiment used when restoring queued messages after compaction.
- 🔴 Reported dropped administrator route and environment setting names to supporting SDK hosts and in headless setup diagnostics and system messages, without disclosing their values.
- 🔴 Added `reload_connectors` for an SDK host to refresh the signed-in account's Claude.ai connectors after the startup list loads. Unsupported connections get their own error.
- 🔴 Added host requests and incoming stream frames for eligible attachment prefetch. Replies report whether a request was accepted and diagnostics identify the stream lane; workers requiring verified frames still enforce that check.
- 🔴 Waited up to seven seconds for pending cloud uploads before sending an eligible device transfer. Checked file calls can tell the device when the cloud copies are ready.
- 🔴 Changed when MCP tools can request eager loading: eligible calls can keep the request through the held-message path, while an experiment can turn eager loading off for non-exempt tools.
- 🔴 Changed cloud workers with a restored permission prompt to wait for SDK MCP tool enumeration on the remote path, instead of using the prior feature switch.
- 🔴 Saved which plugin function-hook markers a cloud session read, so an eligible resumed session can account for those reads.
- 🔴 Rejected streamed control requests whose input size can't be measured when a host sets a maximum request size.
- 🔴 Gave MCP auth and status requests different explanations for a session waiting to be claimed and one whose claim failed.
- 🔴 Changed claims to check the last settings answer that would actually change something. They still wait for unanswered requests and refuse settings that wouldn't take effect.
- 🔴 Let Desktop's Code Tab save whether a user accepted, kept, or undid an offered auto-compaction window for the signed-in account and model. Earlier answers can't overwrite newer ones.
- 🔴 Added a way for local host integrations to wait for the peer inbox key to finish publishing.
- 🔴 Closed agent proxy connections that don't send a complete CONNECT request before the request timer runs out. The HTTP 408 reply and reconnect guidance show the configured interval, normally 30 seconds; an incomplete connection can wait nearly 60 seconds depending on when it opened.
- 🔴 Kept an agent proxy connection counted until its in-flight dial ends, even if the client has already gone away.
- 🔴 Limited reported OpenTelemetry service names to the Claude Code, Claude Code Desktop, and Cowork names; otherwise reports the name for the active surface.
- 🔴 Retried eligible Remote Control requests after a network error or timeout while still connected. Retries can start every 3 seconds for up to 50 seconds, time out after 10 seconds each, and stop if a newer request of the same kind takes over.
- 🔴 Hid full file and patch content from affected edit, write, notebook, and shell results when exposing those results to plugins.
- 🔴 Changed design-system reads to skip the file list when listing would need its own permission prompt or is denied by a rule.
- 🔴 Added `rootId` to workspace and branch diffs so SDK clients can open a relative path from the right root. `stale_root_id` asks the client to refresh when the root changes or the CLI restarts.
- 🔴 Looked for waiting main-session and subagent approvals when attaching to a remote session, checking saved events for answers and cancellations. The lookup reads up to three pages, reports incomplete coverage and lets attach continue after five seconds.
- 🔴 Verified eligible marketplace plugins against their recorded Git commit, rather than today's marketplace contents. Some failed checks wait up to a day before retrying the same commit and version.
- 🔴 Waited for an in-flight privacy opt-out write at shutdown, including when the choice is saved through the host.
- 🔴 Flagged MCP tool input schemas nested more than 64 levels deep for the normalizer's fallback rather than accepting the deeply nested schema as-is.
- 🔴 Reported the total number of replacements for `replace_all` even when plugin hooks don't receive the full file or diff.
- 🔴 Relayed selected cloud connector calls through the assigned local tool host. It forwards cancellation, checks whether a relay session still exists after a minute idle and distinguishes a route or session change from a definite tool failure.
- 🔴 Let eligible hosted MCP tool-port connections finish setup without waiting on the initialized notification when the rollout is on. If that send fails or times out, the connection closes and Claude schedules a reconnect after active calls finish.
- 🔴 Expanded cloud-session lookup and listing data: main repository branch separately from the session branch, pin and unread state, status and timestamps, ancestry, waiting connector actions and self-hosted runner wait details. SDK session details include the list row when available.
- 🔴 Added `update_cloud_session` for SDK rename, archive, unarchive, delete, pin, unpin and mark-read actions, and for recording detach, open and completion. Rename rejects blank titles and limits trimmed names to 500 characters. Errors distinguish changes that didn't start, were refused or may have gone through.
- 🔴 Added SDK typing and pickup pings for cloud sessions, with a 10-second request timeout.
- 🔴 Waited for plugins from the claimed folder before starting their held MCP servers. Refused stale plugin servers and explained policy refusals before waiting or failed-claim status. The temporary prestart marker alone doesn't change a server's configuration.
- 🔴 Started Remote Control without sharing or reusing the earlier history when a resumed conversation points to a different sign-in or organization. (behind `tengu_deep_lemur`, on by default)
- 🔴 Changed `/web-setup` to show its missing GitHub workflow permission warning inline without the extra top gap.
- 🔴 Counted direct navigations and navigations inside a browser batch as recent page changes in Claude in Chrome. The permission check can use that signal for three seconds when its flag is on.
- 🔴 Fixed connector file uploads to fill in the destination when the original tool call leaves it blank, including an empty or null value.
- 🔴 Added support for the connected app to send a file to a connector on Claude Code's behalf, with separate outcomes when the app didn't send anything.
- 🔴 Explained connector upload failures in progress messages, including changed files, slow or lost transfers, storage errors, missing sign-in and policy or role restrictions. A storage failure can leave a partial file, so Claude warns before retrying.
- 🔴 Refused connector file uploads initiated by a mod on its own and told the caller that Claude has to make the call.
- 🔴 Added open, read and close operations for streaming connector requests through the runner tool host. It allows 64 open requests and bodies up to 8 MiB, waits 20 seconds per read, and cleans up after five idle minutes. Missing and cut-off replies have different outcomes.
- 🔴 Listed eligible installed plugin hooks, commands and servers from the remote tool host. It checks the listed definition and event before running a command, can use a loaded settings copy if the ordinary one is missing, and refreshes missing plugins before asking callers to retry the listing.
- 🔴 Remembered the plugin hooks the tool host listed to a caller and checked later plugin commands against that list. A caller that identifies the list can use a matching plugin agent or skill without listing the hooks again.
- 🔴 Added separate list and relay operations for machine-installed MCP and passed `CLAUDE_CODE_MCP_SERVE_MACHINE_PLUGINS` to serving processes to enable those plugins. Hermetic mode leaves them out.
- 🔴 Added `ownFiles` to tool-host project-context requests. It can include user `CLAUDE.md` content after verifying that a matching regular file still exists under the user's settings folder.
- 🔴 Added organization plugin settings to runner tool hosts without overriding existing local plugin choices, and removed custom marketplace headers from the settings passed in.
- 🔴 Let a remote tool host prepare the module for a synced plugin or explain why it can't. Linked modules and remote plugin replies over 15 MiB return `TOO_LARGE`.
- 🔴 Added remote relay for eligible plugin module calls to run a process, fetch a URL, read environment values and work with plugin storage. It stops when the requesting caller goes away.
- 🔴 Added on-demand account plugin and skill archives from the paired client. Transfers reject skipped parts and archives over 256 MiB, wait up to 20 seconds per offer and stop after a minute without the caller.
- 🔴 Added a tool-host write option that leaves any existing destination entry alone, including entries that aren't regular files.
- 🔴 Reused eligible runner checkouts, cleared stale Git locks and refused proxied startup if the runner can't inspect `.git`. On configured hosts a missing launch directory for a command can be recreated empty, with a notice that its files are gone.
- 🔴 Added a remote tool host cleanup operation for old prompt files, with a list of filenames to keep and counts for removed, kept and failed files.
- 🔴 Added a remote tool host check that tries to flush pending runner output files and reports whether they're stored or still waiting.
- 🔴 Refused `--initialize-fields` on startup paths that can't read an SDK initialize request, including `--bg`, `--routine` and `--worktree --tmux`. The error points hosts to `--settings` and `--disallowedTools` instead.
- 🔴 Changed terminal shell-integration installation to surface a failed edit of the settings array instead of replacing the array with a new one.
- 🔴 Added `CLAUDE_CODE_DESKTOP_RESULT_FIRST` for eligible Desktop launches so tool results can be sent ahead of the matching assistant message.
- 🔴 Removed `CLAUDE_CODE_CONSENT_POINTERLESS` and its alternate keyboard focus behavior on consent prompts.
- 🔴 Removed `CLAUDE_CODE_SPARE_KEEPS_BOOT_SESSION_ID`. A prestarted background session without a supplied session ID no longer keeps its startup ID through this switch.
- 🔴 Removed `CLAUDE_CODE_CCR_SKIP_HYDRATE_ON_CURRENT_TIP`; hosted resume no longer uses that switch to skip hydrating from the transcript.
- 🔴 Removed the hosted-session override `CLAUDE_CODE_CCR_SLOW_SKILLS_WATCH`, which could leave the remote skill watcher at its slower idle rate after an edit.
- 🔴 Removed `CLAUDE_CODE_PLAIN_DELTA_TAKE_AT_SOURCE` as an override for the plain-text streaming fast path, which now depends on the hosted experiment.
- 🔴 Reported cloud agent proxy connection counts, limits, refusals and incomplete-request timeouts in status output. Operators can set `CCR_AGENT_PROXY_CONNECTION_LIMITS_DISABLED` to turn off connection limits and the related timer.
- 🔴 Added a marketplace error for local folders reached through links Claude Code can't verify. It suggests using the folder's own path or a repository URL.
- 🔴 Stopped lower-priority settings aliases from surviving when a higher-priority parent settings source sets the main spelling.
- 🔴 Counted previously parsed settings as read from disk when they first contribute to a settings walk, so a cache hit doesn't hide their source.
- 🔴 Let an organization-enabled marketplace plugin qualify for the managed tier after its installed copy verifies, even without a hook module or an entry in `prependPlugins` or `appendPlugins`. Unlisted plugins follow the listed `prepend` plugins; warnings explain when a matching copy qualifies.
- 🔴 Blocked project-supplied Anthropic routing, thread, conversation, affinity, and state headers from the set of headers that can be passed through without user trust.
- 🔴 Tied managed hook eligibility in `plugin.register` to the installed copy. App-supplied, organization-required and auto-installed plugins qualify only when their hooks pass the applicable checks; a result for one path doesn't cover another or a changed plugin name.
- 🔴 Added `CLAUDE_CODE_RUNNER_MODEL_SOCKET` for self-hosted runner model access, and treated the socket path as a credential when filtering environment variables.
- 🔴 Removed `CLAUDE_CODE_STREAM_TIMER_SLACK_MS` and its rollout switch. Stream heartbeats, body-idle checks and retry timers no longer use the optional extra wait.
- 🔴 Stopped shell permission suggestions from treating a path as a verified directory when it can't pass the file check.
- 🔴 Kept `proxyAuthHelper` credentials off a proxy address whose scheme Claude Code had to fill in. A one-time warning explains why the proxy may return HTTP 407 and what to check.
- 🔴 Added `comparison_fork` to the headless host protocol. The SDK and cloud worker use it to compare the first request of a hypothetical rerun with the saved one. It returns `stale_turn` if the original can't be found in time.
- 🔴 Added an optional `toolUseId` to the typed Agent handback report so clients can match copies of the report to the call that sent it.
- 🔴 Added `permission_error` to the API error types cloud workers can report for their own refusals.
- 🔴 Let a host ask to restore messages removed by a rewind when API safeguards refuse the replacement turn. It leaves the rewind in place if safeguards had stopped the removed response or if the replacement switched models.
- 🔴 Added `shell_cwd` frames for Agent SDK hosts that only need the working directory after Bash or PowerShell. A host can mark a non-blocking `PostToolUse` matcher with `replacedByFrame` at initialize to get the frame instead of individual callbacks.
- 🔴 Let Agent SDK hosts mark eligible `PostToolUse` hooks as `contextOnly` at initialize, so the next matching tool can start while the callback finishes. Claude Code warns and drops answer fields other than the added context on that path.
- 🔴 Kept the usual approval-dialog row in place but disabled it when persistent approval isn't allowed, so keyboard movement still reaches the same choice.
- 🔴 Added cursor paging and `only_runs` and `only_archived` filters to the SDK cloud-session list.
- 🔴 Added `agent_id` to SDK cloud transcript reads. Invalid IDs are refused, and a page containing another agent's messages is discarded.
- 🔴 Added `token_state: not_refused` to host credential requests when the CLI doesn't have the starting credential and no provider has rejected it.
- 🔴 Stopped startup from using local AWS keys or profiles when a host is set to supply `AWS_BEARER_TOKEN_BEDROCK`.
- 🔴 Prevented gateway model discovery from falling back to the local sign-in when the launcher owns credentials and hasn't supplied a helper.
- 🔴 Let supported third-party providers match model restrictions across model spellings instead of treating a lookup as first-party merely because the host name matched.
- 🔴 Ran the launcher-owned `apiKeyHelper` from `/` with an environment picked for the helper, so it doesn't pick up the workspace's environment or shell settings.
- 🔴 Reported the launcher-owned `apiKeyHelper` as a managed credential source in the sign-in conflict guidance.
- 🔴 Added a managed policy check for Microsoft 365 connector file references, off under HIPAA and when the policy can't be confirmed.
- 🔴 Removed `cmd+c` from the reserved key list on macOS. The list still protects the system paste shortcut.
- 🔴 Read managed mod options through their supported plugin names, including an explicit off value that takes precedence over a lower-priority spelling.
- 🔴 Made permission checks on tools forwarded by the host use the host's view of permissions even when the permission-layer switch is off.
- 🔴 Stopped looking for project hooks beside the repository when the host already keeps plugins there separately.
- 🔴 Let host tools see when a session has another unanswered action or a message waiting while the current tool is still open.
- 🔴 Checked session transcript paths before offering exceptions for them to clients that restrict file reads to working folders.
- 🔴 Kept `ListAgents` available when the host's policy allows messaging only the session's own agents. It lists those subagents without looking up peers, and its prompt describes only that list.
- 🔴 Recognized Apple Messages for Business as a distinct source when matching built-in messaging connectors.
- 🔴 Added separate tracking for an organization being left and signing in for a new process.
- 🔴 Blocked repository settings from setting `CLAUDE_CODE_SPARE_DEFER_ALL_MCP` or `CLAUDE_CODE_REMOTE_TOOLS_DESKTOP_APP_HOST`.
- 🔴 Limited gateway credential settings to the launcher and eligible managed settings when a host asks Claude Code to obtain its own gateway token.
- 🔴 Kept the last eligible managed transport settings in use when a managed settings source refreshes without supplying those values.
- 🔴 Tracked when a repository tries to set the feature flag switch so Claude Code can explain why it hasn't applied.
- 🔴 Allowed eligible API request compression with desktop-provided certificate settings.
- 🔴 Let the host refuse an API request before trying its request route or the normal network path.
- 🔴 Rebuilt the API client if the launcher's API key helper lost its usable result during initialization.
- 🔴 Gave third-party API requests separate handling when choosing the body idle timeout.
- 🔴 Added session ingress authentication to eligible API requests.
- 🔴 Cleared stale Git locks before preparing a cloud runner's checkout when the configured repository source requests cleanup.
- 🔴 Let cloud runner worktree cleanup finish even if the run has already been canceled.
- 🔴 Passed a completion callback through nested and streaming plugin hook dispatches.
- 🔴 Changed plugin hook re-entry to skip only the registrations that would re-enter, leaving other matching hooks eligible.
- 🔴 Kept a plugin's completion hook from being credited for the fallback completion it triggered in its own isolated worker.
- 🔴 Included whether an MCP server is waiting to be claimed in its reported connection state.
- 🔴 Let a host disable image viewing for the session.
- 🔴 Refreshed the active sandbox rules when a switch changes how denied links are checked, and protected the launch folder under either supported link check.
- 🔴 Applied configured read-deny rules when checking eligible pane file paths on diskless hosts.
- 🔴 Asked for host approval before sending files from protected credential stores through pane uploads when the extra check is on.
- 🔴 Checked alternate names when deciding if a file belongs to Claude Code's own files and needs approval before sharing.
- 🔴 Used separate path checks for sandbox allow and deny entries when judging whether a file tool can reach a path.
- 🔴 Let plugin sync accept managed hook modules through a host relay and use that relay's listing without requiring the local login to have the plugin permission.
- 🔴 Closed failed plugin download streams and used the same failure classification for the download paths.
- 🔴 Skipped the plugin feature path until a parked spare session is ready to serve.
- 🔴 Checked streamed JSON-RPC replies, ignoring unrelated messages and refusing unexpected questions. Event IDs and retry hints are omitted when the response path doesn't issue IDs.
- 🔴 Kept native-only plugin hook dispatches on the native host, kept host-only events out of worker hooks, and sent unloads to the host or worker that runs the module.
- 🔴 Stopped including the raw account and organization IDs in the result of a refused account resolution.
- 🔴 Raised the desktop connector download limit from 500 MiB to 5 GiB, matching claude.ai.
- 🔴 Removed private MCP download information before returning nested downloadable content.
- 🔴 Reused the permission view and the MCP classifier decision across calls in the same group.
- 🔴 Marked data flow incomplete when a built-in skill reads other sessions and tracked which skill folders it opens.
- 🔴 Changed policy-limit requests to use eligible session ingress authentication and a supplied HTTPS agent, signal when the connection opens and stop retrying unrecoverable route errors. Bearer lookup respects the caller's deadline and returns a failure code without repeating generic text.
- 🔴 Left host credential endpoint settings unset when the launcher requests authentication at startup and hasn't supplied those settings.
- 🔴 Counted the prompts cut for size in session state, separately from the event for each cut.
- 🔴 Preferred `SESSION_INGRESS_URL` to `ANTHROPIC_BASE_URL` for runner-backed session startup, tool proxy requests and file references when using the runner's model socket.
- 🔴 Matched plugin hooks against the actual permission prompt before including them in the dispatch.
- 🔴 Recognized document source labels in the same attachment-text check as image source labels.
- 🔴 Included a call behind a permission card even when its host can't be identified, as long as it isn't already covered by another card.
- 🔴 Let request replay recording use the caller's session ID and supplied context snapshots.
- 🔴 Refused connector file sends when no file check was recorded for this call (`prompt_record_missing`) or the arguments changed afterward (`input_changed`), even if the call didn't need a human prompt.
- 🔴 Removed the once-per-pattern debug warning for a tool hook matcher that names an MCP server without a tool. The hook configuration diagnostics still flag those matchers.
- 🔴 Removed the shared debug-log line that counted setup problems and pointed to commands like `/mcp`, `/plugin`, and `/sandbox`.
- 🔴 Changed Code Mode to send nested tool calls through the tool permission checks even when installed mods hook tool calls, instead of refusing those calls up front and suggesting that the mods be disabled.
- 🔴 Removed startup checks and cleanup for the built-in Claude Test Playwright helper, including `CLAUDE_TEST_HEADED`, its Node.js, file and proxy checks, cached helper versions, live report pages and remembered app starts.
- 🔴 Changed MCP schema normalization to try keeping constraints from every branch of a top-level `anyOf` or `oneOf` without the separate `tengu_cozy_volcano` flag. If that form can't be sent, Claude Code still tries the simpler form.
- 🔴 Changed connector upload size errors to print readable units instead of raw byte counts. The overall file cap, for example, is now shown as `499MB`; the cap itself hasn't changed.
- 🔴 Limited when a plugin's `consent` counts as user input: the answer must come from its own element before the handler ends or times out, with no intervening Stop. Desktop `Client` elements don't count.
- 🔴 Updated `/claude-api` telemetry setup to test a destination through `ant` or the Console with an `anthropic.test` span, and explained that an empty destination list can print nothing.
- 🔴 Documented server-side OpenTelemetry spans for token counting and the Message Batches API. Individual requests inside a batch still don't emit them.
- 🔴 Added `/claude-api` guidance for labeling API spans with an opaque `user.id` from the OpenTelemetry `baggage` header. The API drops values containing an email address, and the guide warns that the AWS gateway may drop the header.
- 🔴 Explained in the OpenTelemetry guide that Anthropic-hosted sandboxes with restricted networking can get HTTP 403 managing destinations, and that model requests there may not produce API spans.
- 🔴 Updated `ant` telemetry examples to clear inherited Anthropic credentials for destination and workspace commands. The guide recommends rotating a backend key exposed in a request-body error and not repeating it in chat.
- 🔴 Added `/claude-api` OpenTelemetry recipes for Dynatrace, Elastic, Pydantic Logfire, Snowflake and Splunk Agent Observability, and set the Arize Phoenix recipe's content cap to 2,000,000 bytes.
- 🔴 Made transcript cleanup on cloud workers give up if the replacement file can't be synced to disk within one second or exit flushing starts while it waits. Cleanup leaves the original file in place in either case.
- 🔴 Stopped transcript writes directed at another session from updating the current session's file through the active-target lookup.
- 🔴 Changed cloud resume to check whether newly hydrated rows warrant transcript cleanup even when the existing transcript wasn't already up to date.
- 🔴 Used a supplied settings relay for synced plugin and skill listings and downloads, letting the relay enable sync.
- 🔴 Verified that a pinned plugin commit belongs to the branch or tag the marketplace follows before using the checkout. Host-held fetches also make that check when the host supplies the expected ref.
- 🔴 Let eligible organization plugins load hooks from checked data when a separate managed copy isn't required. Recording a managed install outside the active set requires a known version and a matching loaded copy.
- 🔴 Skipped disk checks for non-built-in plugin copies in sessions without disk access or under the restricted execution path.
- 🔴 Returned an empty array from the plugin conversation API when there are no rows after the requested anchor, instead of running the empty result through row formatting.
- 🔴 Updated the agent frontmatter schema's description of the auto-compaction window to describe a minimum rather than a fixed advertised range.
- 🔴 Kept commands, skills, agent definitions and output styles from unclaimed plugins out of their loaded lists until the session is claimed.
- 🔴 Rejected permission answers that would allow a tool while a deny or ask rule from the same answer can't be restored by the next worker. This keeps the user's restrictions in force across a worker change.
- 🔴 Added a gated browser-input repair that aligns the arguments that run with the ones used to judge the call when the two can differ.
- 🔴 Asked the host for missing provider credentials before ordinary and side-query requests and before token counting, including gateway sampling.
- 🔴 Used the configured spelling of additional instruction directories when recognizing local Git worktrees. (behind `bot_protected_tengu_frolicking_pelican`, on by default)
- 🔴 Made Bash permission checks in evaluation sessions report an incomplete check or pass through the failure instead of treating the usual recovery as a full result.
- 🔴 Removed Glob filenames and Grep filenames or matching text from saved tool results when the storage mode calls for stripping them.
- 🔴 Refreshed Git repository detection for cloud bundle preparation rather than reusing a cached repository result; simultaneous checks still share the work.
- 🔴 Added a warning when a user-invocable command loses its name to a built-in skill, unless that built-in skill already reads its folder.
- 🔴 Recorded the session's organization for data tracking when session-start hooks are prepared.
- 🔴 Changed when hosted cowork and TypeScript SDK chats get the chat-specific WebFetch URL policy, using the chat origin and client type instead of the previous switch.
- 🔴 Kept comparison forks off the parent's same-model request path, even when they would otherwise qualify.
- 🔴 Used the same required-field and name validation for local agent loading and parse diagnostics, so an invalid agent reports the same reason on either path.
- 🔴 Added a Bun request-body serializer for eligible first-party main requests when incremental encoding is off. If byte checks disagree or serialization fails, later requests use the usual serializer.
- 🔴 Updated the memory-pass request diagnostic to identify the `x-claude-code-auxiliary-type` header when server tools or MCP servers make the model unable to call a tool.
- 🔴 Changed the API retry delay calculation to use the same `Retry-After` handling for hook requests as for other requests.
- 🔴 Left off the cache-diagnosis beta header when connecting through the runner's model socket.
- 🔴 Kept model comparison forks from updating shared request and memory-use state, and stopped their internal run after capturing the comparison request.
- 🔴 Recorded the transcript rows used to assemble a request for replay, and skipped saving the replay if those rows or the session changed before it could be written. (behind `tengu_quirky_tide`, off by default)
- 🔴 Included eligible one-shot helper requests in the side-query beta handling alongside hook and named helper requests.
- 🔴 Allowed thinking highlights in eligible hosted first-party requests even when the previous display check doesn't pass. (behind `tengu_quirky_tide`, off by default)
- 🔴 Passed the approved tool arguments through when a plugin evaluation’s permission check allows a call that auto mode needs to reevaluate.
- 🔴 Refused a held Claude Docs write when the conversation is too long for auto mode to review, instead of falling back to a prompt. The refusal distinguishes lack of review from an unsafe verdict. (behind `tengu_quiet_lantern` and `tengu_linen_heron` and `tengu_plain_hedgerow`, off by default)
- 🔴 Gave more specific auto-mode guidance when permission rules and connector choices are too large for the server check, including which choices are up to the user or an admin. (behind `tengu_quiet_lantern` and `tengu_linen_heron` and `tengu_plain_hedgerow`, off by default)
- 🔴 Removed the Bash runner's extra input channel for passing messages to certain commands after they started.
- 🔴 Rejected network-backed working directories during shell launch when the path checks can't establish that they're local.
- 🔴 Skipped nested or newly discovered skills in disallowed directories and logged the reason, including when the directory resolves to a disallowed location.
- 🔴 Stopped the newer task-notice path from gathering output for running tasks during attachment cleanup, while still evicting completed tasks eligible for removal. (behind `tengu_splendid_zephyr`, on by default)
- 🔴 Rechecked linked hook paths before deciding they came from outside a download folder, and treated uncertain paths as uncertain rather than assuming they're local.
- 🔴 Started fetching model capability data for agent hooks and passed the resulting model details into their request.
- 🔴 Passed the parent conversation's loaded MCP skills into agent hooks so their resource checks can use them.
- 🔴 Included SDK-connected MCP servers in the hook path that already uses the hosted proxy client.
- 🔴 Skipped compaction and turn-end hooks in model comparison forks as well as cache-warming requests.
- 🔴 Passed the tool-call details to the classic `PreToolUse` hook dispatcher.
- 🔴 Held repo plugin hooks and install commands back while a spare is parked or after an unsuccessful or changed claim. Logs distinguish why execution was skipped.
- 🔴 Identified the source of successful hook output and added context, and treated context from built-in callbacks as Claude Code's own text.
- 🔴 Stopped built-in callback successes from appearing as separate hook-success messages.
- 🔴 Gave plugins a command-specific refusal when a built-in startup check blocks `$.command.run` during `session.start`.
- 🔴 Changed module reloads to use the host's known plugin entry for approved modules and refuse to reload a protected module from an unverified copy on disk.
- 🔴 Deferred repository MCP bundles found by an unclaimed spare until its session is attached, and marked server entries that must wait for that claim.
- 🔴 Let plugin tool registration use the host's permission view without building a tool-call context when the host provides that view.
- 🔴 Stopped scanning non-built-in plugin files for contributions in diskless sessions.
- 🔴 Applied the hook sandbox's host, container, or refusal rules to plugin requests, including streaming requests; native handlers still serve operations reserved for the host.
- 🔴 Kept the earlier plugin hooks running during a registration refresh while a replacement is checked, and only for copies that still match their source and settings.
- 🔴 Kept in-flight hook host operations from being cancelled just because a placed worker died, except for process spawning.
- 🔴 Let the hook host load and restart modules in individually placed workers when the host provides that facility.
- 🔴 Let host-served plugin MCP registrations answer loopback requests through the host instead of assuming the session can open a listening port.
- 🔴 Cloned plugins without a controlling terminal for host-held fetches.
- 🔴 Checked fetched plugin copies against the origin they were fetched from when installing from staging.
- 🔴 Turned off Git's automatic submodule recursion for the self-hosted runner's proxied HTTP(S) clones.
- 🔴 Recognized `session-end-checkpoint.sh` as a self-hosted runner `SessionEnd` hook for checkpointing when the tool server stops.
- 🔴 Reported self-hosted checkpoint restore outcomes, including whether contents differed, the restore was skipped, or the checkout was lost.
- 🔴 Refused self-hosted runner startup when nested model access is enabled alongside the agent proxy, proxy authorization options or `ANTHROPIC_UNIX_SOCKET`.
- 🔴 Passed the runner's machine plugin list into supported tool-server children instead of the previous enabled-plugin environment value.
- 🔴 Used the runner's explicit agent-proxy setting to decide whether to write repository credentials, instead of inferring the route from an HTTPS proxy variable.
- 🔴 Polled the server for current turn activity before a self-hosted runner with eligible idle-looking sessions drains, waiting up to 3 seconds within the existing shutdown budget.
- 🔴 Left `NODE_EXTRA_CA_CERTS` lookup to the launcher when the host manages the provider and runs the credential helper.
- 🔴 Stopped unrelated background sessions and child processes inheriting the launcher's Desktop tool-host, startup authentication and API-key-helper controls.
- 🔴 Passed worktree cleanup and the final exit message through the same shutdown options for clients closing prompt input.
- 🔴 Distinguished a background Claude launch from a desktop-owned launch even when there’s no parent session ID to pass along.
- 🔴 Made remote organization settings available for listing when a headless client or a client without a consent UI can't approve their use, without treating a listing as approval to run them.
- 🔴 Passed the server's requested retry delay from Artifact list requests to clients that retry failed pages.
- 🔴 Kept an Artifact read started before an account change from updating the remembered version the server is serving.
- 🔴 Reapplied settings environment variables when a repository flag changes after the startup settings pass had to skip it.
- 🔴 Updated the data-source record when accepting a plan file in plan mode, so the session can account for the local plan and the write.
- 🔴 Marked remote notification batches as an external input in the session's data-source record.
- 🔴 Stopped sending queued-prompt companion metadata when there's no result ID it can point to.
- 🔴 Included the expected working folder in tool-host session information when no open directory has been reported yet.
- 🔴 Stopped a bridge file read when the saved diff root is no longer current. A `diff_root` read reports `stale_root_id` so the client can refresh before trying again.
- 🔴 Passed the configured HTTPS agent to gateway model discovery, so the startup model list uses the same network route as the caller.
- 🔴 Reloaded remote synced skills when their list changes and removed the separate slow idle polling interval for remote skill directories.
- 🔴 Counted `/debug`, `/doctor`, `/explain-usage` and `/fewer-permission-prompts` as reading other sessions in the data-use record.
- 🔴 Checked at SDK startup whether the supplied settings or denied tool rules can be carried to a hosted session.
- 🔴 Reloaded managed plugin hooks during startup repair when their listed plugin copies need to be prepared.
- 🔴 Left out parent-session bookkeeping from the cloud launch report of local environment variables that don't carry over.
- 🔴 Probed quota status during eligible startup even when the normal startup prefetch is being skipped. (behind `tengu_dapper_acorn`, off by default)
- 🔴 Checked enabled plugin copies after plugin initialization when startup isn't deferring housekeeping, or when a stream-JSON client needs the result. (behind `tengu_dapper_acorn`, off by default)
- 🔴 Treated a local Artifact file path as unsafe to read if its link target couldn’t be checked reliably.
- 🔴 Saved session IDs alongside adopted background and exit tasks so the receiving session can identify where those tasks came from.
- 🔴 Limited the MCP file-upload form check to events identified as Drive file-reference uploads.
- 🔴 Warned when a headless send targets a Remote Control session that may treat terminal messages as coming from another session, with the same notice in JSON output.
- 🔴 Prepared Git metadata on the headless cloud path that needs repository information after startup.
- 🔴 Marked a command-line process as a waiting spare before it reads SDK initialize settings when `--await-claim` is set.
- 🔴 Returned an SDK startup error when `initialize.settings` parses as an object but couldn't be applied, such as when it's nested too deeply.
- 🔴 Made the tool-host handshake accept the optional `restore` report, with `restored` or `not_restored` as valid values.
- 🔴 Kept plugin naming information available to the compaction request and attached it to the conversation afterward.
- 🔴 Let the Artifacts `read_file` integration write files into the caller's workspace with normal file permissions. Other artifact downloads keep their private file mode.
- 🔴 Stopped plugins awaiting a claim from contributing workflow scripts.
- 🔴 Resolved the model and its restrictions before spawning local workflow agents and detached agents from skills, so the task status and the agent use the same choice.
- 🔴 Stopped a detached agent from a skill from launching if its session changed before launch completed; ended sessions and coordinator mode are checked even without a plugin hook.
- 🔴 Included creation time and whether tasks are enabled in the project list data returned by the CLI integration.
- 🔴 Reported `Grep` and `Glob` deny rules alongside `Read` in the file permission view sent to clients and tool hosts.
- 🔴 Kept links to running resumable agents' transcripts when `/clear` carries the agents into the next conversation.
- 🔴 Treated some unreadable or unsafe PowerShell commands as refusals that ordinary allow rules can't override; where the check must ask, it doesn't offer to save a rule it couldn't verify.
- 🔴 Escaped special tags in MCP and peer-message text, including tags split across adjacent text blocks, so external text isn't mistaken for Claude Code's own instructions.
- 🔴 Passed launch-session IDs and, when available, a child-notes directory to language servers, after applying the plugin's environment.
- 🔴 Described recognized Artifacts database operations more precisely in permission checks so the checker can distinguish known reads and writes from an unknown input shape.
- 🔴 Checked for pending comments from outside the conversation when changing artifacts from a laptop, and required an answer or an automatic safety check where those comments could have directed the change.
- 🔴 Required evidence that Claude recently read the relevant result before letting an outside MCP response justify a tool call, including for servers trusted for the current user only.
- 🔴 Used current turn state before announcing idle to Remote Control after history loads, and reported a new permission mode only after the incoming update and its settings write are confirmed.
- 🔴 Omitted organization hints collected before compaction when uploading the compacted transcript row to Remote Control.
- 🔴 Omitted withdrawn file locations and their old rendered text when exporting a memory attachment without content.
- 🔴 Kept cloud peer messages on the sending session's sign-in and stopped following redirects when posting them.
- 🔴 Checked connector file references for a complete hash when they accompany held text before using those references in permission and path checks.
- 🔴 Explained that Remote Control folder selection can be refused by changed or unreadable settings, and told users when to restart the service.
- 🔴 Recognized linked Git worktree metadata when the remote tools host decides whether a folder belongs to another checkout.
- 🔴 Stopped adding another prefix to MCP request and cancellation IDs inside the SDK's connected-server transport.
- 🔴 Ended Agent SDK sessions before sending a prompt if Claude Code didn't confirm settings or blocked tools delivered in the initialize request.
- 🔴 Distinguished host auth refreshes before and after an authentication refusal. The Agent SDK callback gets `tokenState` set to `"not_refused"` before a refusal, from the protocol's `token_state: "not_refused"` field.
- 🔴 Copied hybrid-mode settings into the Agent SDK's temporary resume directory, including the staging, local, and custom OAuth copies.
- 🔴 Refused to start a Claude apps gateway with nonempty `oidc.ca_cert_pem` and no usable certificate, with setup guidance for the operator.
- 🔴 Documented when gateway checksums can be absent or differ between machines and why admins should treat them as sensitive diagnostics.
- 🔴 Applied more path and symbolic-link checks to file reads served through MCP, including alternate spellings of a path.
- 🔴 Started the usual local messaging inbox without waiting for its key file to be written. Peers still need the key where authentication is required, and an explicitly requested socket still waits for it.
- 🔴 Added resumed MCP task completions to the session event log.
- 🔴 Marked the session log incomplete on resume when previously listed background work is missing without a completion notice, including agents and workflows orphaned by an earlier process exit.
- 🔴 Stopped counting a deliberately cancelled remote event send as a failed send in telemetry.
- 🔴 Used one interpreter list in Bash permission checks, including the general script-path rules for Deno instead of a separate argument check.
- 🔴 Waited up to 500 milliseconds for a pending local messaging key write when closing the inbox, and cleaned up a pending key file as well as the active one on exit.
- 🔴 Changed plugin-provided MCP startup to wait for the selected plugin path’s trust decision when the path is known, instead of using the verdict for the whole source.
- 🔴 Passed the shell working-directory update through the interactive engine's event feed for desktop integrations that handle the update themselves.
- 🔴 Sent teammates permission decisions without the caller's extra content blocks. Approval keeps the decision and its rule updates but drops feedback text; denial still sends its feedback text.
- 🔴 Changed the warning for organization-disabled MCP channels to tell users to ask their admin instead of pointing them to a managed setting they can't change.
- 🔴 Corrected `/hooks` to say that exit code 0 doesn't show stdout or stderr for `PostToolUse`, `PostToolUseFailure` or `PermissionDenied` command hooks.
- 🔴 Changed cloud agent proxy socket failures to close the affected live connection and report a handler error separately from a normal upstream dial failure.
- 🔴 Used the session ingress address for the cloud agent proxy when the runner supplies the model socket, unless an explicit proxy route overrides it.
- 🔴 Changed Code Mode to send saved Bash and PowerShell output through the Read tool when a matching read hook applies, without asking for read permission or passing along the contents of a denied file.
- 🔴 Changed Code Mode's refusal to name the protected tool when a plugin hook tries to supply its result, while keeping the account-memory-specific message for that server.
- 🔴 Refused content-mode Grep in Code Mode when a loaded mod can check Read but can't check that Grep call. The refusal points Claude to shell search under the shell's own checks. (behind `tengu_repl_mcp_error_throw`, on by default)
- 🔴 Changed connector tool listings to omit the read-only and destructive hints from the returned catalog while keeping their use in permission checks.
- 🔴 Removed the rollout gate that could stop a project tool from downloading an existing uploaded file by ID.
- 🔴 Changed Artifact database writes and publishes to honor a matching Edit ask rule on every checked path for the local source file.
- 🔴 Changed the Artifact database read error for a missing target or account to explain that an empty collection normally reads as empty and point readers to the Artifact's link and access.
- 🔴 Added the discovered `token_files` list to the design system result and its count to the guide's diagnostics.
- 🔴 Applied workspace diff exclusions to the corresponding macOS data-volume path when checking deny patterns.
- 🔴 Changed workspace diff scanning to surface file-descriptor and memory exhaustion as scan failures rather than quietly handling them as an unreadable folder.
- 🔴 Let a newly created project document arm its Artifact comment wake when the Artifact tool is available through the remote session's Artifact entry.
- 🔴 Added a debug warning explaining why a directory that might contain Claude Code data was left out of a workspace diff, such as a link, an unreadable directory or more than 256 non-file entries.
- 🔴 Recorded who approved a restored shell call when the resumed worker has to move the shell to the call's saved working directory.
- 🔴 Made the MCP bridge cancel a matching outgoing tool request if the tool run has already failed, while still honoring the request's own cancel signal.
- 🔴 Enabled the hosted worker socket when the hosting layer advertises its protocol version, including `shared_tool_request` for the shared tool stream. Stream events pass through without re-encoding; other messages have a configurable member limit and warning.
- 🔴 Let eligible cloud workers make a second attachment download request if the first takes too long to deliver headers. An optional connection pool can handle the first request.
- 🔴 Added the configured attachment retry delay and pool use to cloud attachment timing reports.
- 🔴 Loaded cloud transcript rows from the saved event position when it matches the local transcript. After a restart, the host can still get a notice for a previously accepted prompt if there won't be a turn to carry it.
- 🔴 Made cloud workers leave `shell_cwd` events off the outgoing session stream, where host-owned shell hooks can instead use them locally.
- 🔴 Added `slash_words_join_v1` to headless session capabilities when the selected model supports combining text and slash-command input.
- 🔴 Made a headless session with no initial transcript load its saved tool deny lists directly; it no longer needs an earlier transcript list to restore them.
- 🔴 Added a way for MCP reconnect reports to distinguish a server that stopped being eligible from one that ran out of attempts.
- 🔴 Made hosted MCP tools honor the worker's rules about which tools may be listed up front instead of copying the device's always-load hint as-is.
- 🔴 Let cloud sessions keep track of a prompt that ended before yielding a model turn, so the host can get a result for that prompt without issuing another model request.
- 🔴 Kept a cloud server error from yielding to another user prompt solely because the retry rollout was enabled.
- 🔴 Removed the configurable long-sleep limit tied to the last stream frame in headless background waits. The wait now uses its regular timer.
- 🔴 Reported the amount of the MCP startup wait budget the session spent when it ended the wait.
- 🔴 Kept pending permission answers through an interrupt before startup finishes without requiring the old cloud rollout flag.
- 🔴 Kept later, standalone prompts out of the headless check that interrupts current work for immediate follow-ups.
- 🔴 Removed the early headless check that refused elicitation from built-in plugins just because there was no terminal; other host and policy checks still run.
- 🔴 Kept headless turns from starting while a spare is waiting, being claimed, or has failed its claim.
- 🔴 Kept shell working-directory events from being counted as the last conversation result when an SDK client asks for the last message.
- 🔴 Applied the local SDK's auto-compaction offer answer during startup and when the host updates it.
- 🔴 Delayed a prestarted SDK worker's scheduled jobs until it has successfully claimed a session.
- 🔴 Added `settings_applied` and `disallowed_tools_applied` to the SDK initialization result, alongside the report for managed settings.
- 🔴 Kept a stored cloud permission request pending when its new host can't answer it. Fresh requests still get the usual failure answer if the host can't handle them.
- 🔴 Held later cloud sends behind an interrupt until the interrupt has been posted, without making them wait for the worker's response.
- 🔴 Sent queued cloud configuration before the first prompt, except rewinds, and let the cloud client resend a lost configuration request when it's holding up later sends.
- 🔴 Forwarded the prepared prompt to `claude -p --cloud` when the caller supplies one, while still requiring an input task.
- 🔴 Extended the shell's personal-email check to recognize the two Claude account-email variables when deciding whether to allow a request that uses identity.
- 🔴 Stopped accepting `mods` as a Code Mode restart reason.
- 🔴 Let `Elicitation` hooks answer or decline questions from built-in MCP servers and `ElicitationResult` hooks change the answer those servers receive.
- 🔴 Changed the terminal's MCP dialog to use the standard conditions for hiding nearby content, instead of always doing that for built-in plugin questions.
- 🔴 Accepted `skills/get` responses with no `skill` field, so an absent remote skill doesn't fail response validation.
- 🔴 Recognized file-capable connectors that supply a verified server identity through the connection, including where there isn't a matching static connector configuration.
- 🔴 Passed the tool call's permission phase and approver through to the connector file-input step.
- 🔴 Passed file lists along on follow-up connector upload requests when the upload flow supplies them.
- 🔴 Included the connector files just sent in the tool result alongside any newly offered downloads.
- 🔴 Kept the default connector upload checks from switching to direct local upload handling in remote sessions.
- 🔴 Skipped the cloud worker's disconnected status on exit when a notification is still waiting out Stop and the worker isn't already waiting for an answer.
- 🔴 Refreshed eligible MCP tool listings when file support changes. Servers using the modern protocol or tool-list notifications only advertise their upload or download support after the applicable exchange.
- 🔴 Applied the connection's host permission check to eligible proxied MCP requests, and let the proxy check the MCP server's negotiated capabilities. (behind `tengu_nimble_doorstop`, off by default)
- 🔴 Identified known MCP service addresses and servers provided for supported plugins as Anthropic destinations when applying data-sharing rules.
- 🔴 Added a hosted MCP file exchange for eligible connectors: send selected files separately, carry references in calls and accept file downloads. Calls using that download route don't use MCP Tasks.
- 🔴 Made raw MCP tool listings skip malformed tool entries for eligible servers instead of rejecting the whole list.
- 🔴 Stopped waiting on the MCP tool request in the transport after the caller's wait has failed, when the HTTP send tracking rollout is on.
- 🔴 Used the extension's project root when `/model` reloads agent definitions after switching coordinator mode and the two project roots match.
- 🔴 Explained in `/hooks` that changing a hook supplied through `--settings` means changing those settings and restarting Claude Code.
- 🔴 Changed the `/install-github-app` workflow step to name the repository with the existing file, explain optional Actions setup and label skipping it `Not now`. Esc still exits without changes.
- 🔴 Reworded the `/install-github-app` browser sign-in progress messages.
- 🔴 Changed `/install-github-app` results to say if a workflow was created or kept, and point to creating and merging the pull request when needed. Success and error screens use the dialog's close action.
- 🔴 Kept a typed custom secret name when switching between reusing the existing `ANTHROPIC_API_KEY` and entering another name in `/install-github-app`.
- 🔴 Kept `/install-github-app` on the API key form when you submit a blank key, with an error beside the field.
- 🔴 Added a way back from entering a new API key to subscription sign-in in `/install-github-app` when the account offers that choice.
- 🔴 Kept a typed repository name in `/install-github-app` when switching between the current repository and another one, and let pasted names go to the other-repository field.
- 🔴 Changed dynamic workflow, skill, file, notebook, shell, connector, inbox, and Artifact tool results to use the shared terminal status rows for short summaries and errors.
- 🔴 Changed the dynamic workflow approval preview to use the same script display check for its full code, its phase summary, and the decision about whether it can show the script for approval.
- 🔴 Changed `claude mcp list`'s rejected-server label to use the terminal's status glyph, including the screen reader variant.
- 🔴 Refused claims of a waiting process when it couldn't prepare the main agent named in project settings. It reports `project_settings_not_claimable` and tells the caller to start a fresh process.
- 🔴 Added paging to the newer SDK cloud-session list API, including pinned sessions beyond the first page and a check that rejects invalid cursors.
- 🔴 Added SDK cloud-session list filters for archived sessions and scheduled runs. Runs are listed 50 at a time instead of the usual 200.
- 🔴 Expanded SDK cloud-session lists and details to return more session information on the newer API, including pins, unread status, lifecycle and connection status, and the session's model and permission mode.
- 🔴 Added additional repository and current-branch information to SDK cloud-session listings when the server supplies it.
- 🔴 Added runner-wait details and pending connector actions to SDK cloud-session details when those are available.
- 🔴 Updated the SDK error for cloud-session requests made from a program another Claude Code session started. It now explains the restriction applies to all cloud-session actions, not just reads.
- 🔴 Let Artifact attachment approvals accept a blank slug when creating a new Artifact, using the destination arguments returned by the service.
- 🔴 Raised the streamed connector upload deadline from 12 to 17 minutes and staggered retries so failed clients don't all retry at once.
- 🔴 Distinguished Desktop connector upload errors that need another sign-in from those where Desktop refused the transfer and a retry won't help.
- 🔴 Stopped connector file calls before sending if Claude Code can't confirm the account from its config file or that account differs from the session's sign-in.
- 🔴 Removed the blanket requirement for an interactive terminal for connector uploads from sessions you start yourself. File checks and permission rules still apply, and sessions started on your behalf stay restricted.
- 🔴 Fixed connector file checks so a file under a permitted working folder keeps that classification when the resolved path uses a different name for the same folder.
- 🔴 Changed connector upload permission prompts to distinguish files inside and outside permitted folders. Outside files still need an explicit yes, and inside files can follow the tool's permission decision.
- 🔴 Limited Desktop-assisted connector uploads to direct file uploads; a form upload can't use that path.
- 🔴 Added file progress and stop reasons to connector upload updates and results. Hosts can see which files the service already received without exposing the storage backend's error text.
- 🔴 Turned off the image viewer for tools run inside the eval child session.
- 🔴 Moved remote tool-host hook transcripts to a user-owned directory on a verified shared-memory filesystem, separate from project files.
- 🔴 Checked that tool-host MCP project calls based on a saved hook list still name the plugin that owns the server.
- 🔴 Added machine-plugin hook diagnostics that distinguish an absent hook from a mismatch in its command, arguments, or condition.
- 🔴 Added account plugin and skill availability to the tool host's environment reply when those lists have been supplied.
- 🔴 Added tool-host requests for listing a remote directory and asking for file-name suggestions.
- 🔴 Added a tool-host request to record saved uploads.
- 🔴 Added a tool-host request to save session outputs.
- 🔴 Added tool-host reads for browser uploads and file references, with the attachment checks applied on the host.
- 🔴 Changed the tool host's remote file checks to use its standard handling for links whose paths it can't decode.
- 🟢 Reworked `/install-github-app` setup around standard pickers, a progress spinner and inline errors. It separates workflow titles and descriptions, shows the repository and app link when access is missing, supports a new secret name in the picker, and makes it clearer how to enter an API key or recover from setup failures.
- 🟢 Added `claude mcp add-json <name> -` to read a server definition from stdin instead of the command line. The CLI documents it, rejects empty input or more than 1 MiB and doesn't allow it inside a Claude Code session.
- 🟢 Fixed eligible starts without a terminal that get a bare `host:port` from `HTTPS_PROXY` or `https_proxy`: Claude Code adds `http://` before startup requests. Set `CLAUDE_CODE_DISABLE_PROXY_SCHEME_REPAIR` to turn that off.
- 🟢 Reported tools a plugin hook allowed or tried to allow in `claude plugin eval`, without changing the exit code. Terminal and HTML show both outcomes by run; `claude plugin eval --json` includes `plugin_approval`, `refused`, `pluginHookApprovals` and the affected-run count `pluginHookApprovalRuns`.
- 🟢 Kept `claude plugin eval` deny rules and failed tool permission checks in effect even if a plugin permission hook would allow the call.
- 🟢 Sped up secret masking on long text, including text with a URL, by using simpler patterns when the text doesn't need invisible-character matching.
- 🟢 Refused more symlink paths when Claude Code can't verify the destination text without following the link. Network-path checks and file permissions no longer trust those links.
- 🟢 Stopped following Linux process and file-descriptor aliases when deciding which paths count as working directories for file permissions.
- 🟢 Stopped passing the parent's `CLAUDE_CODE_OAUTH_SCOPES` to a child that loses its inherited `CLAUDE_CODE_OAUTH_TOKEN` and gets neither a replacement token nor an OAuth refresh token, so the child can use its own sign-in under an organization pin.
- 🟢 Ignored repository settings that turn off feature flags or interfere with required plugins and skills in Anthropic-hosted cloud sessions when the new cloud check is on.
- 🟢 Kept retrying eligible terminal MCP connections to remote HTTPS hostnames for about a day, including HTTP, SSE, and claude.ai connections. The background schedule can make 69 slower attempts after the fast retries, varies the wait up to a 15-minute cap, and checks the current settings and credentials for each try.
- 🟢 Let repository settings turn telemetry off, but not on, when the new cloud check is on. Kept telemetry off when ignoring a mixed set of cloud repository settings.
- 🟢 Warned below the prompt when a settings change couldn't be saved, and asked users to check that settings files are valid and writable. This covers `/config` changes to auto-updates, output style, and response language, as well as changes from `/advisor`, `/fast`, the theme picker's syntax-highlighting toggle, and the Auto-memory and Auto-dream switches in `/memory`.
- 🟢 Added and documented `x-claude-code-managed-settings-checksum` for eligible gateway requests when managed settings enable `CLAUDE_CODE_GATEWAY_HINT_HEADERS`.
- 🟢 Refused more credential-file uploads in Claude in Chrome, including when a path leads into a protected credential store.
- 🟢 Stopped `claude -p` waiting indefinitely for Anthropic's model settings before its first request. It continues after 10 seconds if the fetch stalls; the same wait applies to a subagent's first request on its selected model, with a 1.5-second limit in interactive sessions.
- 🟢 Recognized explicit deny rules inside combined tool decisions, so plugin checks can't treat them as ordinary refusals.
- 🟢 Kept nonempty directories named by `GIT_CONFIG_GLOBAL` when self-hosted runners start with `--use-anthropic-git-proxy`. The runner warns and picks a writable config file instead.
- 🟢 Applied the session’s permission limits when one agent resumes another. The agent that asked for the resume can’t make the resumed agent less restricted.
- 🟢 Added a confirmation to `/install-github-app` before it writes to a repository. It names the repository, lists any workflow writes, and says which secret will be written or replaced. Cancel is selected first.
- 🟢 Stopped expanding permission roots through symbolic links when the path can't be checked safely. Claude Code checks aliases before treating them as another spelling of the same root.
- 🟢 Added `CLAUDE_CODE_DISABLE_SHELL_HEAD_START` to stop Bash from starting its environment and reading startup files before the first eligible call is approved.
- 🟢 Sent model, side-query and token-count billing groups through Claude apps gateways that set `admin.billing_group_tag`. That signed tag survives credential refresh; `attributionProject` or the organization's default names the group.
- 🟢 Kept a subagent's earlier task prompt when a background-task notice wakes it, instead of reporting the raw notice as its new prompt.
- 🟢 Treated only 2xx responses as success when updating the extra-usage spend limit.
- 🟢 Ran local Git metadata preparation only for interactive, remote or bridge sessions rather than all session starts.
- 🟢 Reported a result to headless clients when a prompt-submit hook drops an already queued prompt, as long as the transport can still deliver it.
- 🟢 Fixed startup parsing for `--disallowedTools` rules with more complex arguments, including parentheses. Permission setup keeps rules the newer parser understands without warning that they're incomplete.
- 🟢 Refused to resume a past background session when Claude Code can't verify how the operating system resolves a directory link.
- 🟢 Refused repository-authored plugin evaluation paths when Claude Code can't verify how the operating system resolves a directory link.
- 🟢 Refused local file links in connector file checks when Claude Code couldn't verify their destination without following them.
- 🟢 Required an actual answer to the individual Artifacts tool call where the organization asks for one. An automatic allow doesn't count as approval.
- 🟢 Applied the organization's per-creation Artifact approval check only to new artifacts rather than routine edits to existing artifacts.
- 🟢 Narrowed stale cloud-peer recovery to plain HTTP 400, 404, 409, or 410 responses, so timeouts and rate limits no longer count as stale addresses.
- 🟢 Cleared inherited `CLAUDE_CODE_OAUTH_SCOPES` for Remote Control children when they don't get the parent's token, and removed inherited scopes when starting the remote tools host.
- 🟢 Kept a clean Git worktree when Remote Control stops but its session can resume, so ignored worktree files are still there when it returns.
- 🟢 Corrected the `/hooks` descriptions of the fields supplied to `PostToolUse`, `PostToolUseFailure` and `PermissionRequest` command hooks.
- 🟢 Listed hooks from `--settings` alongside hooks from other settings files in `/hooks`, where local hooks are allowed.
- 🟢 Limited cloud agent proxies to 8,192 client connections by default, including idle ones, or fewer if Linux file descriptor limits require it. When full, the proxy immediately returns HTTP 429 and accepts new connections again when a slot opens.
- 🟢 Kept eligible headless sessions retrying dropped MCP connections to servers on other hosts after eight fast reconnect attempts, with up to 69 attempts total in that retry loop while the server stays eligible.
- 🔴 Refused empty destination hostnames in the cloud agent proxy before trying to connect. Requests routed directly under `CCR_AGENT_PROXY_RELAY_MODE=selective` now get HTTP 403 if hostname cleanup leaves the destination blank.

</details>

<details>
<summary>387 internal changes</summary>

- 🔴 Built the `/autocompact` slider labels from the model's range instead of a fixed list. The slider still uses `auto` and 100,000-token steps.
- 🔴 Carried data-flow records through transcript rewrites, resume, forks, background takeovers and `/clear`. The updater merges records from related session IDs without self-links, keeps unsent and incomplete records, marks fork ancestry and the current account, refreshes before writing and removes tool use from rolled-back responses.
- 🔴 Replaced the comment-aware JSON reader and editor used for settings, package validation and keybindings. Normal JSON takes a fast path, multi-edit updates rebuild the document once, comments and line endings survive, and edits beyond an unterminated block comment fail.
- 🔴 Distinguished a missing tunnel from runner capacity or availability when reporting why a cloud MCP connection can't start.
- 🔴 Limited reads of Linux process identity and CPU accounting files to 4,096 bytes at a time and opened them without acquiring a controlling terminal.
- 🔴 Shared in-flight Git repository lookups by working directory, including for Remote Control bundle prep and Ultrareview. Callers can require a fresh check, and an outstanding lookup can be checked in ten short waits within the same 250 ms budget.
- 🔴 Included the facts behind a rejected MCP file-tool input in the error object, separate from its existing message and error code.
- 🔴 Fixed model-specific bootstrap lookup and reuse: tracked each model's fetch separately, waited for the selected entry, and started a missing fetch at terminal or headless turn start. Plan and fallback variants can share the primary entry. Startup can reuse a fresh catalog and clears cached beta choices after new model data arrives.
- 🔴 Shortened retries inside classic webhook and prompt hooks by honoring the server's wait without adding random delay and removing the first warm-up wait.
- 🔴 Escaped an internal cache-marker spelling in nested model payloads so it arrives as literal data.
- 🔴 Waited for the cross-session inbox key before advertising a new socket, with five seconds for registration and half a second for cleanup. It records timing for directory and key work, skips writes for sessions that no longer need a key, and records a publish failure instead of silently advertising an unreachable inbox.
- 🔴 Added `tengu_fgts` to the small set of flags accepted when Claude Code fetches flags while signed out in eligible environments.
- 🔴 Recorded whether a remote session names a proxy, separately from whether it names a Unix socket.
- 🔴 Reused the reverse list of a plugin's aliases while the current alias map is unchanged.
- 🔴 Hid private MCP-backed skill and server names in source attribution and invocation, use, and API success or failure logs. A generic `mcp` label identifies those skills where a label is needed.
- 🔴 Marked data-flow reports incomplete when event recording fails, the account or organization switches or carried transcript tool activity can't all be reconstructed. File IDs with no established source are left out.
- 🔴 Reused normalized permission rules and model names across repeated lookups instead of cleaning and parsing the same text each time.
- 🔴 Made the model catalog use the published catalog in eligible sessions even when ordinary served settings aren't in use.
- 🔴 Saved which mod notices have been read across updates that add or clear those marks.
- 🔴 Changed the SDK child-process memory breakdown to report local MCP and language server RSS separately, dropping separate Bash and miscellaneous rows but keeping the total for all children.
- 🔴 Reused tool-name lookups after a tool list has been queried repeatedly.
- 🔴 Marked memory extraction API requests with separate Claude Code and Cowork use cases.
- 🔴 Kept a brief comparison request in the same request category as main and subagent turns, rather than falling through to the category for other background requests. (behind `tengu_wiggly_dove`, off by default)
- 🔴 Recorded `AccountSkillLoad` when Claude or a subagent opened, preloaded or listed an account skill, including forked skills and CLI or guide command listings. Records include a path or MCP origin when known and omit the old prompt label on preplayed tool results.
- 🔴 Recorded `AccountPluginEnabled` when an enabled account plugin loads, except for catalog plugins and previews.
- 🔴 Recorded Remote Control linking, incoming activity and configuration messages, including configuration with no settings to apply. Outbound-only links and keepalives or housekeeping requests don't count as inputs.
- 🔴 Recorded more Artifact reads in session data flow: tool replies, saved copies, publish checks and conflicts, handovers and ordinary WebFetch of Artifact links. Reports separate failed fetches and can mark the ID unknown; design-system reads also count when a guide supplies their text.
- 🔴 Added data-flow attribution for reports a child session leaves behind, keeping separate child IDs when they're known.
- 🔴 Recorded more MCP server and tool origins in data flow, including registry addresses, installed plugins, hosted web agents and deferred results. More MCP-shaped names count, a conflicting host listing makes the origin unknown, and a report without usable links is marked incomplete.
- 🔴 Recorded cross-session message direction and peers, including messages sent back to the same session. SendMessage activity identifies Remote Control recipients by bridge address.
- 🔴 Downloaded plugins through the relay when one is supplied; direct downloads still work and clean up a failed response stream.
- 🔴 Included local skills in the plugin and skill listing returned to the model when their names aren't already represented.
- 🔴 Saved the inputs needed to replay a model request when the transcript allows it and the conversation still matches the request.
- 🔴 Let a caller opt an eligible request using a custom CA into automatic gzip, when the corresponding flag is on. The existing proxy and client-certificate exceptions still apply.
- 🔴 Preserved both the old and filtered Cowork memory wording when preparing later context checks, so either form can still be recognized.
- 🔴 Named oversized saved hook outputs by invocation and content rather than sequence. An earlier file can be reused when its size still fits, or rewritten if it can't be used.
- 🔴 Refreshed offsets for active tasks in the background when `tengu_splendid_zephyr` is on, rather than adding their status attachments to every turn.
- 🔴 Let a new task use a different ID for its output file while keeping its own task ID.
- 🔴 Used the memory check from the matching startup fetch when deciding how long a remote Cowork turn should wait for memory.
- 🔴 Stopped startup in builds that can't carry the list of code-running tools a Bash deny rule must also disable, with an error advising users to install another version.
- 🔴 Shared the eligibility rules for forwarding a queued agent message immediately and moving a long-running shell call aside for an agent report. (behind `tengu_mellow_minsky`, on by default)
- 🔴 Looked up Cowork memory context for data-flow recording without letting a failed lookup stop a model request.
- 🔴 Marked transcript gaps so writes can choose a checkpoint that leads to the newer conversation. Added a diagnostic when a rewind saves its resume point after another row is written first.
- 🔴 Matched tool calls, results and API errors to the model reply that produced them during transcript recovery. This keeps unfinished sibling calls together without keeping unlimited work in memory; recovery falls back to the original call row if it can't rebuild the reply.
- 🔴 Validated recorded hook counts and index values as nonnegative safe integers before using the saved hook result.
- 🔴 Distinguished launcher-supplied environment values from matching settings values when deciding whether a launch can synchronize or reuse plugins.
- 🔴 Separated missing `./` from other plugin manifest errors and recorded the first schema error for recognized fields, including nulls and invalid relative paths.
- 🔴 Disabled diagnostics in the prestarted PowerShell parser process with `POWERSHELL_DIAGNOSTICS_OPTOUT` and `DOTNET_EnableDiagnostics`.
- 🔴 Tracked auto mode calls set aside without a safety result. Up to eight refusals that don't stop the turn are remembered until their status changes; reports say whether retrying allowed, asked, refused or still lacked a result.
- 🔴 Logged when fallback compaction can't scan messages for loaded MCP skills, instead of failing the whole compaction.
- 🔴 Checked whether the prior thread response has changed before reusing it as the continuation anchor, separately from the sampled audit.
- 🔴 Sampled thread-echo audit events at 10% and reported the sampling rate alongside the existing comparison counts.
- 🔴 Restarted the stream heartbeat timer after processing an item, so the next keepalive check runs 10 seconds after the recent activity.
- 🔴 Prepared eligible Bash environment snapshots in a detached process, cleaned up on timeout or buffer overflow, and reported whether the first command got a usable snapshot, how long it waited and whether the snapshot was stale.
- 🔴 Checked pending plugin shell binaries before preparing Bash, including when preparing the shell ahead of the first tool call.
- 🔴 Changed the startup debug log to describe the shared hook worker rather than one worker per plugin.
- 🔴 Changed how hosted-worker process diagnostics get their socket reader, so the launcher can start diagnostics without supplying one. The diagnostics-file setting still controls whether they run.
- 🔴 Recorded when a browser tool argument could be interpreted two ways, and let the tool’s action judgment influence whether Claude Code leaves the model’s input as written.
- 🔴 Bound renderer instance methods before handing them to the terminal scheduler, so callbacks keep the renderer they act on.
- 🔴 Stored terminal layout measurements in typed arrays and rebuilt cached measurements that don’t match the expected layout.
- 🔴 Replaced the terminal error screen’s dependency for reading stack frames with a local parser that handles constructor, `eval`, and native frames.
- 🔴 Reported terminal cursor-check mismatches with renderer, terminal type and wide-character details, including how often checks disagree and whether a terminal version was requested.
- 🔴 Reported whether a delayed approval with no input still matches the saved tool arguments, including a comparison-only report for old input with no digest and a cloud diagnostic for changed subagent input.
- 🔴 Added an off-by-default option to label observer activity with the instructions that accompanied the digest. (behind `tengu_keen_umbrella`, off by default)
- 🔴 Recorded whether an Artifact MCP `read_file` reply supplied a file the session could copy. When the handler is missing, fails, or can't copy the file into the workspace, the caller gets the original result.
- 🔴 Recorded model-turn inputs for troubleshooting, including the main session's tool list, browser and computer enablement tools, and whether the turn began with compaction or an active teleport cache.
- 🔴 Reused successful local startup prefetches within a remotely configured time window, so restarting quickly can skip fetching the same startup data again. The wait setting defaults to zero.
- 🔴 Added `sdk_initialize_fields` reporting for SDK startup, including the reason when required fields didn’t arrive.
- 🔴 Logged why cleanup left an old `claude-test` plugin folder in place and whether the next session will try again.
- 🔴 Moved consent settings route selection to a new rollout flag. The default still uses account settings, and the flagged path still chooses between Claude.ai and the consent service.
- 🔴 Logged when the Artifact publish approval included the outside-comment notice.
- 🔴 Changed how denied tool calls quote control characters in their fallback preview, using the shared character list.
- 🔴 Expanded MCP failure diagnostics with HTTP status, credential refusal or withholding and service refusal categories, using `other` for unknown reasons. Artifact failures report creation and deployment separately.
- 🔴 Measured MCP tool host startup steps separately: listener bind, launcher tunnel setup, tunnel dial and tunnel open. `claude mcp serve` marks when it accepts the tunnel, and the timings appear in runner diagnostics.
- 🔴 Added support for turning events received through the worker socket into the same event stream the client reads from its other transport.
- 🔴 Changed cloud event-log restore to read its local and server state without the old preparatory read or fallback race, while still dropping an answer when it detects a conflict.
- 🔴 Recorded an unknown starting source for dispatch and other service-started sessions whose input doesn't identify the original data source.
- 🔴 Added a separate runner for an MCP request so work associated with it can be tracked for the length of the call.
- 🔴 Restored and recorded the worker's permission mode on restart only after a transcript has been restored.
- 🔴 Logged when a queued-message permission check throws, without logging the error text, which could contain the user's words or tool input.
- 🔴 Logged invalid attachment prefetch requests, distinguishing the live stream where relevant.
- 🔴 Changed quiet background waiting to check for activity on 100-millisecond ticks and run the idle check after a second without a change.
- 🔴 Changed cloud resume to recover a waiting permission card from the saved prompt, looking past loader-only rows. Removed the older search for a card behind a loader-inserted continuation and kept a shorter diagnostic when recovery fails.
- 🔴 Logged the choice and token count when Code Tab saves a new auto-compaction offer answer, and reported save failures separately.
- 🔴 Reported how many subagents cloud resume planned to restart and why a planned restart was skipped, including pending permission, missing task data, no attempts left and restrictions on background permission. (behind `tengu_iterative_haven` and `tengu_steady_puppy`, off by default)
- 🔴 Included server type and the original MCP tool name in diagnostics when a tool preflight can't proceed as submitted.
- 🔴 Kept exception details out of the agent proxy's shared request-handler diagnostics by recording the error type instead.
- 🔴 Marked edit previews shown by the permission dialog separately from previews shown elsewhere, so the display can make the right choice for the approval view.
- 🔴 Reported how many background MCP reconnects were tried and why retries ended, including success, sign-in required, a change in eligibility, and reaching the limit.
- 🔴 Dropped the old setup-issue counters and blank zero counts from the startup summary. Individual checks still log by severity; required sandbox and other startup messages still run.
- 🔴 Cleared extra terminal handles from the background launcher and left inert handles in their old slots so later files can't accidentally take them.
- 🔴 Ran tools that aren't safe to check concurrently in sequence when plugin judge hooks or a pending hook restart require that ordering.
- 🔴 Recorded which other sessions created the saved jobs in a `CronList` result, so those sessions can be tracked as inputs.
- 🔴 Removed temporary marketplace clones after the plugin commit checks finish.
- 🔴 Made the MCP response reader reuse the connection's stream checker for traffic relayed through the tool host.
- 🔴 Reported the result of the MCP tool-port initialized notification separately from opening the connection, including when startup no longer waits for the answer.
- 🔴 Logged when the startup consent dialog ignores an opt-in key, once per mounted dialog. (behind `tengu_playful_puzzle`, on by default)
- 🔴 Counted malformed tools left out of MCP listings alongside the transport and connector details, without counting them twice when a list is fetched again.
- 🔴 Added Bash image-viewing telemetry for how many images Claude asked for, how many it got and how long it waited.
- 🔴 Added SDK cloud-session update telemetry that distinguishes successful changes from refusals and other failures.
- 🔴 Separated MCP server display details from the HTTP and SSE sign-in lookup so callers that only need the description don't make the extra lookup.
- 🔴 Added `/artifacts` telemetry for an incomplete gallery, including pages read and the last HTTP status when available.
- 🔴 Added `plugin_hook_approvals` to plugin evaluation results that record which tools a hook allowed or tried to allow despite a deny rule.
- 🔴 Added telemetry that distinguishes successful runner launch-directory recovery from the reason it couldn't be made again.
- 🔴 Replaced the bundled line-chart library in `/stats` with a built-in renderer for model tokens per day. It skips non-finite values and keeps the existing inputs and labels.
- 🔴 Updated named timing points for connector work, SDK initialization, shell head starts and self-hosted work, including a mark before starting prefetch.
- 🔴 Used an explicit list of invisible Unicode characters for terminal text and deep-link checks instead of relying on the runtime's Unicode table.
- 🔴 Added state for settings, MCP servers, and workspace moves held by a prestarted background session until it gets claimed.
- 🔴 Added a separate launch flag for Desktop acting as the host for remote tools.
- 🔴 Split startup, resume and SDK initialize timings into their component waits: conversation loading and hooks, workspace trust and reply preparation, network and shell discovery, command loading, model catalog and choice, setup and MCP readiness.
- 🔴 Skipped a second wait on the same plugin hook review for one caller that already awaited it.
- 🔴 Skipped MCP wire-name translation when no aliases have been registered, and read the server and tool portions of the name without splitting the entire string.
- 🔴 Registered `refusal_retry_accepts_stop` and `refusal_retry_spares_bio_llm` so the model’s capabilities can govern retrying after a safety stop.
- 🔴 Added a cache for compiled model entries so repeated model lookups can reuse them.
- 🔴 Tracked whether startup waited for client model data and whether a host already asked for the model catalog.
- 🔴 Tracked folders read by bundled skills so other parts of skill loading can tell which folders they've already read.
- 🔴 Kept names that still come from other commands in the session's skill listing state.
- 🔴 Tracked plugins named by an organization separately from copies whose contents have been checked.
- 🔴 Kept an edited custom CA bundle from supplying an empty list of certificates.
- 🔴 Kept the Brahmi number joiner when cleaning up text from external sources for Claude.
- 🔴 Removed hosted Bash CPU diagnostics for repeated time limits, deadline restarts and heavy-task follow-up work. The idle worker sampler that used `CLAUDE_CODE_DIAGNOSTICS_FILE` is also gone. Existing time limits still stop the tasks.
- 🔴 Saved which plugin and developer module markers hooks read when the set changes and the host provides storage, rather than keeping that information only in memory.
- 🔴 Kept track of connector work started at launch so a later reload can wait for it to finish.
- 🔴 Kept the server declaration that went with an MCP tool listing.
- 🔴 Skipped the Artifact link cleanup helper’s URL parsing for values over 2,000 characters.
- 🔴 Separated matching the start of an instruction marker from matching its whole tag, so ordinary text doesn't need both checks.
- 🔴 Added the `web-search-results-only-2026-11-15` API beta header for first-party requests that ask for search results without a model reply.
- 🔴 Reported when a cloud turn waits through the startup memory grace period and added `boot_age_ms` for the age of the last completed startup check.
- 🔴 Moved prompt-cache record work in cloud server workers until after the request has been sent.
- 🔴 Detected iOS projects from nearby `.xcconfig` files when the Xcode project refers to those files but contains no matching build setting itself.
- 🔴 Let callers use their own Git command runner to detect repository and remote URLs.
- 🔴 Classified `EHOSTUNREACH` as a downstream connection failure rather than a timeout.
- 🔴 Added a flag-controlled shell parsing check that defaults on and only accepts a change from the built-in flag service.
- 🔴 Kept track of whether a hosted session called its computer tool and whether linked computer tools had already been offered.
- 🔴 Changed telemetry loader lookups to ask directly for `juniper_shoal`, instead of reading that key from the whole flag object.
- 🔴 Used the checked Git executable for the local settings tracking check when available. If that executable can't start, Claude Code treats the file as tracked.
- 🔴 Updated the bundled model catalog to revision 5174 with an expiry of November 24, 2026.
- 🔴 Checked a first-party catalog model's provider ID before using its runtime description, and checked provider-specific IDs only for supported third-party providers.
- 🔴 Allowed the hosted gateway to use catalog-described models in more cases and kept its Fable entry eligible when the catalog already describes it.
- 🔴 Skipped the initial retry delay in hosted sign-in contexts that ask for no backoff.
- 🔴 Recognized the account's `billing_group_tag` when decoding gateway sign-in claims.
- 🔴 Read the `native_1m` capability for eligible catalog models on third-party providers too.
- 🔴 Recognized a worker's signed-in account from its session token without also requiring a self-hosted-cloud environment flag.
- 🔴 Turned off connection keepalive for the first flag fetch in a diskless session and after it succeeds again.
- 🔴 Stopped a diskless session from using the local `ownWord` marker as evidence about who wrote a prompt.
- 🔴 Rejected a limited feature-flag reply when it contains an unrequested plugin hook module flag.
- 🔴 Changed telemetry compression to size buffers to each part, handle cloud tenant batches separately and stop compressing those batches after HTTP 400, 403, or 415 responses.
- 🔴 Included the other pending actions alongside the lead action in cloud session status.
- 🔴 Left the new session title out of the debug log when renaming a cloud session.
- 🔴 Escaped citation wrappers in external text alongside other markers used by Claude Code.
- 🔴 Used an event's logged time for telemetry if it's in the past minute, and still recorded when the event was observed.
- 🔴 Reused already checked nested arguments when one plugin hook hands a call to the next, so unchanged objects don't need to be copied and checked again.
- 🔴 Gave grouped plugin hooks a signal for when their call or stream has finished, including on cancellation.
- 🔴 Credited the plugin that changed a completed turn's answer even when its hook ran in its own group.
- 🔴 Added version 32 to the mod hook interface for transcripts supplied from memory.
- 🔴 Kept the account bucket handed over with a session when choosing which account's plugin data to use.
- 🔴 Replaced the terminal renderer’s bundled border table with border definitions built into the renderer.
- 🔴 Updated the feature flag lists to add new rollout controls and drop flags whose earlier behavior was removed or made standard.
- 🔴 Cached the normalized model allowlist so later model checks don't have to rebuild it while the list stays the same.
- 🔴 Reduced the work of recognizing reserved Windows device names by trimming spaces directly rather than matching a pattern.
- 🔴 Ignored formatting characters when hashing the PEM payload used to identify a blocked CA certificate.
- 🔴 Recorded file origins for connector reads, files named in tool inputs or outputs, `Grep` results, Markdown reads and pasted file diffs. Records identify the calling agent across processes and mark eligible unknown Markdown sources incomplete.
- 🔴 Passed the feature flag's name into the early settings lookup so features can use the cached value for that key.
- 🔴 Rearmed API stream idle timers on new chunks without the previous slack and dropped that margin from watchdog telemetry. Reply-hold reports now show the most recent and longest waits for bytes.
- 🔴 Recorded whether an API response actually came from the first-party service for code that uses the request outcome.
- 🔴 Cleared the saved answer state for shared-session comments when signing out or starting a new conversation, so it doesn't carry into a different conversation.
- 🔴 Added a sandbox debug note for cases when a command's result can't say whether its earlier permission check was what kept it sandboxed.
- 🔴 Extended data-flow tracking for child-session notes, including which session was written when another session launches it.
- 🔴 Stopped using the launcher's account and organization IDs for child and background sessions when building the data-flow identity.
- 🔴 Recorded the incoming session event when a teammate shuts down and its tasks become unassigned.
- 🔴 Started keeping the finish time of the most recent browser navigation for browser tool timing checks.
- 🔴 Passed the shell parser's telemetry options through the video and background-command checks.
- 🔴 Stopped writing filesystem error text to the debug log when reading the end of a background command's output fails.
- 🔴 Removed the old and new part counts from prompt-change reports that still say whether parts were added or dropped.
- 🔴 Kept both sides' distinct prefetched Cowork memory entries in the advisor's rendered context.
- 🔴 Removed the earlier-command input-tail fields from shell telemetry.
- 🔴 Let test runs that request repeatable timing skip the random extra backoff.
- 🔴 Kept the tool-call rewrite details in the metadata used to judge a call even when its server classifier is off.
- 🔴 Removed the short MCP server hash from the set of telemetry fields allowed through the privacy filter.
- 🔴 Treated comparison forks like other auxiliary requests in the query-source check used for prompt suggestions and agent summaries.
- 🔴 Recognized the reminder to message an agent that hasn't responded as part of the harness's own guidance.
- 🔴 Changed Windows special-path detection to use the original internal spaces in a path segment when checking device names.
- 🔴 Marked session ancestry as unknown when a `session.receive` hook consumes the incoming message before it can be queued.
- 🔴 Included input from earlier tool calls when describing the actions behind a grouped tool result. (behind `tengu_foamy_spring` and `tengu_mutable_phoenix_killswitch`, on by default)
- 🔴 Saved shell CPU measurements before stop or timeout tears down the process. A failed read leaves out the sample rather than affecting the task.
- 🔴 Compared the next optimized API request body after the fallback writer produced only bytes, and disabled the shortcut on a mismatch.
- 🔴 Removed a hosted-session experiment that could delay request diagnostics and skip them after the request's later checks.
- 🔴 Removed cloud resume's current-tip shortcut and its hydration, timing and prefetch diagnostics. Resume no longer asks the worker if it's at the last locally saved event to decide whether to hydrate.
- 🔴 Removed the last skill-list pass that dropped a bundled skill when an earlier command had the same name. The command menu has its own name handling.
- 🔴 Removed the Projects bridge's periodic status cue for Claude, including cues from the server's mount response. Launch policy and the new tool-based fallback can still nudge Claude.
- 🔴 Removed Bash telemetry about the old `tail` experiment, including whether capture was considered, how much was saved, and why it was dropped.
- 🔴 Changed the subagent transcript's checks for lookalikes of its parent-turn labels, so it can distinguish more of them from the labels Claude Code supplies.
- 🔴 Removed the old MCP `pending approval` status and special actions and messages from inline `/mcp`; reconnect, enable and disable now use the normal paths.
- 🔴 Replaced part of the terminal renderer's character-width tracking. The old lookup tables no longer grow through their shared array-copy routine.
- 🔴 Removed `tengu_shell_completion_failed` along with the old shell completion subprocess.
- 🔴 Removed the separate startup diagnostic for an enabled Claude in Chrome extension that hasn't been paired. The Chrome setup flow and browser connection stay.
- 🔴 Changed native-install startup logs to give the individual PATH notices instead of just reporting their total.
- 🔴 Stopped clearing the environment-hook notice handler when the prompt footer unmounts. The next footer can replace the handler.
- 🔴 Changed the local cloud-worker relay to pass event data through as received, replacing line breaks with spaces rather than parsing and rewriting all of the JSON.
- 🔴 Changed quiet hosted model calls to send a transport keepalive after 240 seconds without output, or sooner if the heartbeat requires it. The old `tengu_valiant_heron` setting had a 120 to 260 second range.
- 🔴 Changed long waits for background work in headless sessions to recheck after 1,000 ms. The old Bash-only wait in remote sessions no longer uses `tengu_snazzy_starlight` delays of up to 10,000 ms.
- 🔴 Removed the old Claude Test write grouping and telemetry: `claude_test_helper_files`, `claude_test_helper_start`, `claude_test_run` and `claude_test_draft`.
- 🔴 Added `viewedImagePaths` to Bash results so the transcript can identify the image files the command showed to Claude.
- 🔴 Marked WebSearch results that carry page text instead of citations with `plainPageText`. Claude is told to link to those sources in Markdown even when citations are enabled.
- 🔴 Added disk sync wait time and reasons for slow or failed syncs to cloud transcript cleanup diagnostics.
- 🔴 Added a cloud-resume signal when hydration brings in rows for an empty local transcript and there’s no resumed-tool tally to report.
- 🔴 Marked more external inputs in session data flow: host-supplied startup context, externally launched sessions, usage reports from saved files, shared task lists, plugin credential use for outgoing fetches, external tool output and pasted shell input.
- 🔴 Added browser-input telemetry for cases where the permission check and the streamed input repair can read the same arguments differently.
- 🔴 Added `tengu_bash_too_long_to_judge` telemetry for expanded Bash commands refused because the check can't read them in full.
- 🔴 Marked a default-to-No approval request as following a failed auto-mode safety check, so permission handling can distinguish it from a normal ask.
- 🔴 Refreshed session data-source tracking without awaiting a filesystem scan when preparing a session for its ownership check.
- 🔴 Added a gated path that leaves the workspace snapshot out of the initial context when the session can supply it separately.
- 🔴 Moved tool cache controls out of the reusable tool definitions so request assembly can place them on the tools it sends.
- 🔴 Let first-party API requests through the configured runner model socket qualify for the same request path as host-managed sessions.
- 🔴 Attributed observer activity and follow-up prompts to the observer, with its instructions when enabled. Synthetic observer results carry separate directions, are excluded from ordinary tool accounting and count among capabilities used while away.
- 🔴 Recorded source sessions for cloud tasks and reviews, replayed workflow results, linked inbox attachments and scheduled tasks that ran or were missed, including error results when available.
- 🔴 Added telemetry for resumes that find a Stop hook block limit, distinguishing automatic resumes. (behind `tengu_foamy_spring`, on by default)
- 🔴 Added distinct compaction reports for tool-loop summaries that succeeded, missed the summary block, left it open, or had to retry.
- 🔴 Carried naming and memory-withdrawal notices through compaction, regenerating an older reactive summary when it would miss a more recent one.
- 🔴 Reported whether WebFetch tried a fetch before asking for approval in its queue telemetry.
- 🔴 Added account-owned skill counts and descriptions affected by truncation to skill listing telemetry.
- 🔴 Named tools that were replaced and shouldn't be called again when the tool-list update requests that notice.
- 🔴 Kept optional whole-file context out of the rest of a request's retry sequence after a 400, and let a fresh request try including it again.
- 🔴 Treated model comparison forks like SDK requests when choosing their prompt-cache lifetime.
- 🔴 Removed the `toolsHashKept` field from thread decision telemetry.
- 🔴 Kept a queued message beside the tool result it accompanies when preparing the conversation for the auto-mode safety check.
- 🔴 Tracked whether a rewritten call lost an auto-mode finding that had been flagged, so an earlier flag can still influence the next decision. (behind `tengu_quiet_lantern` and `tengu_linen_heron` and `tengu_plain_hedgerow`, off by default)
- 🔴 Marked Claude Docs writes that must wait for review in the auto-mode decision reports. (behind `tengu_quiet_lantern` and `tengu_linen_heron` and `tengu_plain_hedgerow`, off by default)
- 🔴 Tracked check failures separately from deliberate refusals, so a plugin evaluation can tell when the tool's own permission check never finished.
- 🔴 Recorded references to cloud flags, job output, and known programs found in shell commands.
- 🔴 Told the model about tools it may need to load again while a plan or background task is active.
- 🔴 Tracked source references embedded in queued commands, including shell excerpts and command context from an unknown source.
- 🔴 Matched deferred-tool announcement layout to the request's model capabilities and skipped connected-app handling when the list is empty.
- 🔴 Tracked extra context and text from compaction, async wake-up and prompt or agent hooks by built-in, official or other plugin source. Rendered messages leave out the internal annotations.
- 🔴 Reported the original file mention count when a prompt exceeds the 100-mention lookup limit.
- 🔴 Stopped counting the status-line mount's trust-block event and invoking its UI callback; the status-line command still doesn't run before trust is accepted.
- 🔴 Included the circuit-breaker value in tool-permission telemetry even for breakers previously hidden by the tenant telemetry filter. (behind `tengu_snoopy_creek`, off by default)
- 🔴 Gave eligible aborted tool calls 3 seconds to finish in diskless sessions or 5 seconds in other non-interactive remote sessions before Claude Code moves on. The wait no longer needs a feature switch.
- 🔴 Added `plugin_approval` to tool-permission records when the final accept or reject matches an applied or refused plugin approval.
- 🔴 Recorded shell writes to local instruction files separately from shell writes to local memory.
- 🔴 Initialized the pending reminder list used by the file-reading sidecar when a file can't be handled in place.
- 🔴 Marked computer tools as used when a client asks for permission outside the ordinary tool execution path, including shared MCP tool names with a computer-tool call in the conversation.
- 🔴 Removed the old feature-off check from tool input classification for eligible calls, and stopped passing a separate copy of the Bash command as originally written.
- 🔴 Counted a rejection in the tool permission dialog toward the turn's denial tracking.
- 🔴 Gave tool readers a reason when plugin or hook activity prevents reading command diffs or results outside the normal tool path.
- 🔴 Checked whether a plugin permission dialog owns the call before allowing a context-only callback to finish later.
- 🔴 Normalized hidden and direction-changing characters in eligible text before matching phrases for the plugin checks.
- 🔴 Passed plugin marketplace origin into hook registration only when the earlier hooks from that same plugin directory agree with it.
- 🔴 Passed known installed plugin IDs into hook policy checks when the host has enough information to verify them.
- 🔴 Let completion notices point to an existing subagent report when there's no need to repeat its contents. (behind `tengu_moonlit_peacock`, off by default)
- 🔴 Stripped more notice and tool-use boilerplate from partial subagent reports.
- 🔴 Stopped marking one generated Bash launch path as stop-on-exit independently of the caller's option.
- 🔴 Stopped background Bash tasks from keeping their optional end-of-command helper open through completion.
- 🔴 Limited isolated web connection checks to 1,024 file descriptors per process and up to 262,144 bytes from its process socket table.
- 🔴 Kept the tool's denial record from dropping flagged input simply because the classifier switch says off.
- 🔴 Escaped instruction-like tags in pasted text marked as already scrubbed before building the model message.
- 🔴 Skipped queued input that had already been withdrawn when no human input was allowed.
- 🔴 Included email withdrawal in the session-context message so the model knows it shouldn't use the earlier address.
- 🔴 Added model-facing notices when a subagent asks another part of the session for input.
- 🔴 Passed the core call handler to the hook chain for dispatches that allow module handlers.
- 🔴 Rechecked whether sandboxing is active at the shell permission decision instead of relying only on the proposed sandbox choice.
- 🔴 Repaired a self-hosted session's Git credential-helper directory before restoring its saved Git config.
- 🔴 Added the top component frame to terminal render error reports, alongside the JavaScript error location.
- 🔴 Gave terminal nodes a place to track popovers so the renderer can account for their height and position.
- 🔴 Applied the renderer's text-width measurement to hidden or formatted characters in terminal text nodes as well as containers.
- 🔴 Wrote a fallback alert before self-hosted cloud setup runs so the model can see that setup may be missing if the script fails.
- 🔴 Counted output-limit recovery attempts that used either of the newly recognized continuation messages.
- 🔴 Logged the first damaged layout record the terminal renderer detects.
- 🔴 Renamed the debug log for unreadable piped input on a background launch; the warning shown to the user is unchanged.
- 🔴 Recorded unrecognized Artifact comment wake notices as outward-only activity and stopped doing that for recognized notices marked as not starting a model turn.
- 🔴 Measured how long agents wait for model availability data and reported whether it was the first wait, which model was involved when known, and whether fetching it still left the model unavailable. (behind `tengu_deep_shore`, on by default)
- 🔴 Changed the plugin agent check for diskless sessions so agents registered during the run without a local plugin root don't take the usual runtime registration refusal path.
- 🔴 Counted the earlier unfinished part of a resumed agent turn in completion duration and tool usage, without counting messages present in both copies.
- 🔴 Used the safer fallback for the tool exclusion switch when its value can't be read.
- 🔴 Used the same hook check for an agent wake reply without a tool caller as for replies from the interactive session. (behind `tengu_clever_pillow`, on by default)
- 🔴 Added a file count and elapsed time to the report for reusing the prior memory section. (behind `tengu_melodic_mitten`, on by default)
- 🔴 Gave the memory extractor the current remote files that aren't in the saved transcript when it prepares an extraction turn.
- 🔴 Filtered more tool permission rules out of what auto mode sends to its classifier, and reused the classifier settings within one permission-context build.
- 🔴 Skipped recording carried tool use when building the comparison request for a model query.
- 🔴 Marked an output-limit retry so it continues the earlier reply.
- 🔴 Routed bridge file reads through the shared file permission checks.
- 🔴 Removed the saved agent label and color from the session restore result, which still includes the matched agent definition.
- 🔴 Passed collected attachments through when dispatching a slash command or a prompt routed to `/ultraplan`.
- 🔴 Consolidated the bootstrap fetch failure log, with shorter text for a network error.
- 🔴 Left teleport's generic precheck error to its later error-handling path; the checkout mismatch and missing-repository messages still have their own branch.
- 🔴 Added a housekeeping step for the old bundled Claude test plugin's data folders.
- 🔴 Treated input just after a terminal resume as fresh for up to 150 milliseconds when comparing input timestamps.
- 🔴 Let a pre-started process list agents loaded from flag settings when picking the agent for its claim.
- 🔴 Stopped advance-to-next-screen callbacks once startup has committed its handoff.
- 🔴 Refreshed terminal shortcut hints when focus and registered keys change, with a retry if the refresh throws.
- 🔴 Recorded use of account-level data when `/usage` renders account usage guidance.
- 🔴 Labeled connector file download telemetry when the desktop app fetched the file.
- 🔴 Recorded when Claude used account catalog data to list, find, or recommend skills and plugins.
- 🔴 Combined initial cross-session activity and cloud worker status updates when possible, and skipped repeat updates for unchanged worker metadata.
- 🔴 Let the cloud worker cut short a stalled transcript upload under 256 KiB when the caller enables that check, so the upload can be tried again.
- 🔴 Removed the cloud worker's diagnostic field for its separate startup status nudge.
- 🔴 Reported failed file-input uploads with the sanitized transport error and elapsed time.
- 🔴 Made noninteractive `/config` callers wait for spinner and reduced-motion preferences to finish writing. (behind `tengu_chomp_inflection` and `tengu_sepia_moth` and `tengu_amber_flint`, off by default)
- 🔴 Replaced invalid Unicode in the worktree path text passed to workflow agents.
- 🔴 Removed the separate workflow-script property reader from the interface used to transfer values out of the script runner.
- 🔴 Recorded the workflow script's source path when the loader reads it successfully.
- 🔴 Switched terminal notices, tool results, and transcript rows to shared formatting for one-line results, indented detail, dim text, and status icons.
- 🔴 Delivered allowed terminal messages from hooks to the active terminal view when it supports them.
- 🔴 Made synchronous forked slash commands resolve the subagent's model settings before launch.
- 🔴 Reloaded agent definitions from the current session folder when resuming a session requires switching modes.
- 🔴 Distinguished stopping artifact comment watches from stopping queued artifact auto-reactions in notifications to Claude.
- 🔴 Switched the terminal’s recovery hints to rely on structured policy and refusal fields rather than looking for a phrase in the API error text.
- 🔴 Renamed the pull request watcher’s debug log prefix to `useBackgroundSessionPr`.
- 🔴 Changed MCP form questions to count each new payload as a new question when its schema can't be turned into JSON, so an old form isn't mistaken for an updated one.
- 🔴 Changed prompt handling to count a human takeover toward proactivity only when the submit came from the terminal composer, instead of counting other submit paths the same way.
- 🔴 Waited up to 10 seconds for plugin lists still being prepared after a verdict before starting the model turn.
- 🔴 Restored the existing plan file when an initial message clears the conversation but carries its plan forward, and let the next turn see the plan in its file history.
- 🔴 Changed Remote Control to notice the plan named by a forwarded startup message before it queues the message, so the receiving session can use that plan file.
- 🔴 Changed the cloud agent proxy to keep counting a disconnected client's place until any upstream connection attempt or tunnel cleanup is done.
- 🔴 Expanded the first-turn hold on organization telemetry to sessions without disk access as well as the previously eligible host-run cloud sessions.
- 🔴 Changed BigQuery metric export to label events with a recognized `service.name`, distinguishing Claude Code, Claude Desktop and Cowork.
- 🔴 Changed Code Mode to process long tool results in pieces so matching hooks can review them before the pieces are joined and returned. (behind `tengu_repl_mcp_error_throw`, on by default)
- 🔴 Fixed the Code Mode edit helper to report the replacement count from the tool result when it doesn't include the original file.
- 🔴 Changed Code Mode to use the same result formatting for stored and reported results from built-in file tools, including the number of replace-all edits.
- 🔴 Fixed shortened Code Mode output so the ending stays on the same line as the omitted-character marker when the cut was in the middle of an input line.
- 🔴 Gave PauseGoal and GetGoal separate tool registry IDs instead of sharing the unregistered marker.
- 🔴 Changed Artifact comment handling to mark externally sourced comment work so later upload, copy and publish decisions can ask the user again.
- 🔴 Changed Artifact permission checks that weigh multiple possible approval reasons to rank an explicit ask rule ahead of an ordinary file safety prompt.
- 🔴 Kept track of open subagent permission prompts on the worker so a matching pending ask can be found even when another card is showing.
- 🔴 Allowed the worker's saved-approval check to recognize permission requests with the classifier-block marker.
- 🔴 Fixed Cowork attachment reporting to keep the normal attachment notices even if the rollout flag selects another mode elsewhere.
- 🔴 Updated the sandbox-network warning to say that a deny or ask rule also has to reach the next worker.
- 🔴 Added methods for clearing selected pending permission cards and republishing the next one when a headless worker drops them.
- 🔴 Allowed a cloud worker to start recoverable agents without adding another model turn when there's nothing else to report after a restart.
- 🔴 Let cloud rollouts enable held messages and related resume checks together; `CLAUDE_CODE_DISABLE_HELD_MESSAGE_RECOGNITION` still disables recognition where applicable.
- 🔴 Let a headless turn reuse a queued prompt's saved preparation result instead of preparing the same input again.
- 🔴 Made the headless warm-up step check whether any MCP clients are still connecting before it waits for tools.
- 🔴 Released messages set aside for after the current turn when a headless turn finishes.
- 🔴 Made the cloud rollout for applying a saved permission answer first independent of the old keep-answers-on-Stop flag.
- 🔴 Made cloud resume count repeated continuation attempts since the latest actual user input, including user input carried in a queued message.
- 🔴 Made notifications that need to wait out Stop stay out of the pending user-prompt count and wait until the worker can deliver them.
- 🔴 Removed the rollout check for including recently superseded tool calls in cloud permission recovery. (behind `tengu_settled_wren_killswitch` and `tengu_quizzical_knuth`, on by default)
- 🔴 Removed the headless long-sleep ceiling from cloud activity reports.
- 🔴 Stopped including the VS Code review upsell rollout flag in the headless initialization reply.
- 🔴 Removed retired settings and flag-wait timing from spare worker diagnostics, including `CLAUDE_CODE_CCR_SLOW_SKILLS_WATCH`, `CLAUDE_CODE_STREAM_TIMER_SLACK_MS` and `CLAUDE_CODE_PLAIN_DELTA_TAKE_AT_SOURCE`. The wait cap didn't change.
- 🔴 Added a way for cloud workers to cancel an unanswered transcript write during shutdown.
- 🔴 Told the cloud resume path when files from a lost worker may be gone, so a restart doesn't clear their pending reminders as if they were still readable.
- 🔴 Marked headless worktree verification as a startup check so resume uses the startup rules.
- 🔴 Made observer subagents resolve their model through the current subagent model and policy checks before starting.
- 🔴 Recorded a server event for ordinary unstaged MCP calls from a headless host, in addition to the checks already used for staged calls.
- 🔴 Applied hosted input normalization to content marked as coming from a host-injected peer in the headless loop.
- 🔴 Updated the sign-in diagnostics for the Claude platform plugin so they identify the plugin rather than the old `/claude-test` command.
- 🔴 Recorded decisions and changes made by the SDK's named permission-prompt tool, rather than treating its answers as if they came directly from the normal prompt.
- 🔴 Changed training notices to distinguish a recent terminal resume from ordinary typing and to catch a pasted burst of Enter keys while the choice isn't ready.
- 🔴 Stopped an earlier training-choice save from ending the newer save's loading state.
- 🔴 Logged a ListAgents call as a peer listing when it finds a session with a non-default identity or transport.
- 🔴 Renamed the iTerm2 debug log for making a teammate pane to match the team view name.
- 🔴 Removed the late plugin-load path that could retry a typed command after its plugin loaded in a terminal session.
- 🔴 Logged a separate failure reason when a browser lookup fails during a same-browser selection check.
- 🔴 Recorded separately when Chrome batches that open allowed sites are refused for an old or unknown extension, sent after classifier approval without another prompt, or fail to send.
- 🔴 Sent the existing no-approval marker with eligible empty-tab lookups in accept-edits mode when no approval had been recorded and the older permission path was in use.
- 🔴 Counted extended waits for MCP connector questions separately when the call got an answer or ran out of time.
- 🔴 Recorded whether loading the session list for the resume screen succeeded or failed.
- 🔴 Moved the terms text shown after the grace period into one shared component, so the different notice layouts use the same wording.
- 🔴 Limited `/privacy-settings` change telemetry to an explicit choice that changed a previously stored on or off value. (behind `tengu_playful_puzzle`, on by default)
- 🔴 Changed GitHub setup error reporting to recognize expected errors by their opening wording rather than by finding a phrase anywhere in the error.
- 🔴 Added a tool-use record when an MCP server task is picked back up after a restart, so the resumed tool appears in the session's activity.
- 🔴 Added attachment upload failure data about the check that allowed the call, its permission mode, and the upload response, without logging response text that could name files.
- 🔴 Added a server-controlled option for loading the bundled Linux audio capture module from an unnamed file, with a warning if that route fails.
- 🔴 Added tracking for a successful pre-started session move, so later launch work can tell that the waiting process has already moved.
- 🟢 Fixed Linux relaunch handoff: marked the old process as shutting down at spawn, put `/dev/null` in its old terminal slots and reported a spawn failure to stderr and background jobs before exiting with status 1.
- 🟢 Cached each connector's blocked tool names, so permission checks in resumed sessions don't rescan large organization lists for every call.
- 🟢 Changed when a later tool script ends the earlier script's permission handling, so starting a script already allowed by the classifier doesn't necessarily end it.
- 🟢 Sped up Bash permission checks for deeply nested paths that don’t exist yet, by walking the missing components without rebuilding the whole path each time.
- 🟢 Changed secret masking to scan URL delimiters as needed instead of keeping an entry for every character, and to reuse the pattern variants it needs for each kind of input.
- 🟢 Added `keptInSandboxByCheck` to Monitor results so Claude can tell when an otherwise exempt shell command stayed sandboxed because its permission check relied on the sandbox.
- 🟢 Changed host-backed workflows that pause for login to count the session’s new human prompts while held and announce the pause.
- 🟢 Marked when a suspended terminal resumes so prompts can ignore keys already in flight.
- 🟢 Sent unread background child completions to the main conversation when the parent subagent finished on its own without reading the result, outside auto mode.
- 🟢 Marked background wake-up notices as notices on subagent launch and resume so clients can avoid displaying them as raw prompts.
- 🟢 Sped up Bash changed-file diffs by reusing Git index information, running private tree commits alongside eligible diff reads, skipping temporary index hashes and syncs, and checking repository safety for up to 64 files at once. A failed tree commit still withholds the diff.
- 🟢 Removed an extra startup query for installed plugins that check tool calls. (behind `tengu_dapper_acorn`, off by default)
- 🟢 Let workflow agents and detached agents started by skills wait for any pending model settings before they start, using the shared subagent model picker.
- 🟢 Stopped `claude -p` launched by a script or hook on a self-hosted runner from inheriting the parent session's rate-limit retry setting. A nested command no longer waits out the parent's usage limit.
- 🟢 Deferred a Git repository lookup until the first model turn in ordinary `-p` sessions; remote and startup-sensitive hosts still begin it earlier.
- 🟢 Logged which kind of question an MCP server asked, and whether a hook accepted, declined or canceled it, with field names instead of values.
- 🔴 Made the resume-point scan skip malformed user-message content when checking for tool results. Only object blocks marked as tool results stop the scan.
- 🔴 Made the feature-flag disk cache use stricter equality when it checks whether a fetch needs to be saved. When it skips unchanged writes, including eligible cloud sessions, a flag value of negative zero no longer compares equal to zero.
- 🔴 Named the unreadable flag in Cowork's memory-window warning when a flag lookup throws. The `tengu_fancy_crab` wait still falls back to zero seconds.
- 🔴 Assigned known IDs to Remote Control activity, mod loads, account skill and plugin events, and hook text in session data-flow reports when the tool registry is available.
- 🔴 Changed the diagnostic when a pending tool call's reply can't be rebuilt. With `tengu_eager_pillow` on (off by default), the lookup logs the saved row's ID instead of the tool call's ID and still uses that one row.
- 🔴 Masked two more invisible marks in the Artifact input summary used for auto mode permission review. The checker sees a question mark in their place; the Artifact arguments aren't rewritten.

</details>

<details>
<summary>28 changes with no effect in this build</summary>

- 🔴 Added Windows console sharing for eligible child launches and Git metadata or Bash diff commands, with a check that sharing is available, NTFS filename checks for Git and a retry in a regular subprocess within the remaining timeout. Linux doesn't use these paths.
- 🔴 Added Windows checks for reserved device names in path components, including device paths with suffixes.
- 🔴 Recognized more Git for Windows installations when choosing how to start Git, including UCRT64 and Clang ARM64 layouts. The Linux chooser doesn't use that Windows check.
- 🔴 Checked Windows `SystemRoot` and the executable roots used by the PowerShell probe and launcher, Command Prompt and Docker path lookup. Invalid paths fall back to `C:\Windows` where applicable; these paths don't run on Linux.
- 🔴 Added host or container routing and refusals for unsupported installed mod operations in Claude Code Server. This build never sets up the container route.
- 🔴 Prepared Windows Bash shells for eligible synchronous `PostToolUse` and `PostToolUseFailure` batches that don't need the foreground. A spare closes after 30 idle seconds; failures are logged and a fresh shell starts if handoff exceeds 2 seconds or a tenth of the hook timeout. Linux doesn't use this path.
- 🔴 Added a guard for carrying an earlier tool description into a request; this build supplies no description to that guard.
- 🔴 Added API request-comparison code that doesn't send a request; the shipped build doesn't register a comparison callback or start a comparison turn.
- 🔴 Added an in-process route for registered plugin tools; the only caller here still uses the normal loopback listener.
- 🔴 Recognized a differently capitalized connector file path as inside a working folder on Windows only after checking that both spellings lead to the same directory. The Windows-only check doesn’t run in this Linux build.
- 🔴 Added an unconfirmed-remote-stop warning to the task-stop tool, but the build has no caller that sets the confirmation callback.
- 🔴 Shipped a set of transcript labels that distinguish actual parent user turns from text sent by a delegating agent; this copy of the labels has no reader in this build.
- 🔴 Removed an inactive MCP restriction that would have limited a server to resources listed by its currently loaded skills, along with the message for that refusal. The restriction's check always returned false in 2.1.326.
- 🔴 Removed an unused repository-validation error branch from cloud teleport. Missing, mismatched, and unverified repositories still take their existing paths.
- 🔴 Added a gated Windows path for shell aliases that sets the invoked command name directly. The Linux build keeps the existing alias dispatch.
- 🔴 Added a pinned Chrome install to the browser preview checks; this build doesn't include the Chrome preview implementation that supplies it.
- 🔴 Added a place for cloud prompt blocks in the startup decision; the decision always passes false in this build.
- 🔴 Added a way for startup to wait before releasing prompts and for late module hooks to wait before running. No caller sets either wait in this build.
- 🔴 Added remote session methods to count and release messages waiting for a Bash command's output. This build doesn't start that wait.
- 🔴 Changed the indentation of the sandbox violation list, which isn’t displayed on Linux.
- 🔴 Added wiring for the cloud-start refusal tracker, the unsent remote shell-command count at exit, and a connection identity check for awaited remote rewinds. This build never sets the tracker or the count, and only permission-mode changes enter the wait.
- 🔴 Added a busy branch for remote shell input. This build never enters it and still rejects `!` input when attached to another machine.
- 🔴 Added a place on the sign-in waiting screen to explain why sign-in can't start a new Claude Code process. The sign-in callers in this build leave the message empty.
- 🟢 Fixed `claudeMdExcludes` on Windows to match a drive letter written in the other case, so excluded user, project, and local instruction files stay out. The positive-pattern check doesn't run on Linux.
- 🟢 Started warming eligible Windows PowerShell parsers and workers while calls stream. Compatible commands reuse the worker if its environment and working directory fit; others start normally. The Windows launcher doesn't run in this build.
- 🟢 Checked Windows PowerShell's standard install locations together when `pwsh` isn't on the path, while keeping their search order.
- 🟢 Sped up repeat PowerShell commands on Windows by starting a shell ahead and reusing it for the next compatible command. This needs `tengu_sunny_deer`, which defaults off. The Linux build doesn't prestart the shell even if the PowerShell tool is enabled.
- 🔴 Let a Desktop app that hosts its own remote tools take the same tool-handling path as a server-hosted tenant.

</details>

---

### 2.1.325 ◆◆◆◆

- 🔴 Removed the built-in `cc-plugin-themes` plugin added in 2.1.324, with `/custom-theme`, `/fireworks` and 20 animated chat backdrops for desktop and mobile clients.

<details>
<summary>1 change with no effect in this build</summary>

- 🔴 Moved the startup profiler's initialization mark and first wall-clock and process-uptime samples to a different point in module loading inside the worker that runs plugin function hooks; nothing in the worker reports them.

</details>

---

### 2.1.324 ◆◇◇◇

- 🔴 Added built-in `cc-plugin-themes`: `/custom-theme` offers 20 animated desktop/mobile backdrops; `/fireworks` runs a show and can run on "merged" in a main reply with a theme chosen, unless auto is off. Removed in 2.1.325.
- 🔴 Added terminal approval for sending a connector a file from the home or temporary folder outside the session's folders; links, other disks, credential-like names and Read ask or deny rules still stop it (remote switch, off by default).
- 🔴 Added remote rules that can move built-in subagents with no chosen model to a lower model family in chats hosted by claude.ai; a model or effort chosen in the agent, the call or the environment is kept. Inactive without a rule.
- 🔴 Added a training-consent service behind `/privacy-settings` and the startup notice, with eligibility checks and a "changed somewhere else" answer in place of overwriting a newer choice (remote switch, off by default).
- 🔴 Added per-redirect connector-block checks for plugin HTTP and SSE MCP servers when organization policy requires them.
- 🔴 Added local JPEG and PNG backdrops to mods' `$.theme.set`: a file in the mod's folder, at most 1 MiB and 4096 pixels a side, with network paths and hard-linked files refused.
- 🔴 Made Artifact supporting files and file removals available without a remote switch; `CLAUDE_CODE_ARTIFACT_MULTI_FILE` can still turn them off, and copying another artifact's files stays behind its own switch, off by default.
- 🟡 (2.1.325) Changed unset `sandbox.autoAllowBashIfSandboxed` to false on macOS and Linux/WSL, sending sandboxed Bash commands through ordinary permission checks; explicit settings still win (remote switch, on by default).
- 🟡 (2.1.325) Added a repository check to read-only Git auto-approval in Bash and PowerShell: a checkout that cannot be matched to the project or a trusted folder goes through further permission checks (remote switch, on by default).
- 🟡 (2.1.325) Routed scheduled `/skill` prompts that need approval to the Skill tool, and told Claude not to work around a refusal (remote switch, on by default).
- 🟡 (2.1.325) Stopped a running slash command's `allowed-tools` grants and connector-card approvals from carrying over to woken background agents and permission-checked host MCP calls (remote switch, on by default).
- 🟡 (2.1.325) Widened the account-email refusal to WebFetch URLs, MCP arguments, file writes and up to eight relative scripts named by shell calls, plus client identity fields beyond User-Agent (remote switch, on by default).
- 🟡 (2.1.325) Moved unpacked plugin MCP bundles to a shared content-hash cache; `CLAUDE_CODE_PLUGIN_MCPB_DIR` accepts absolute folders neither inside nor above the starting folder. Zip-cache mode uses a session folder instead.
- 🟡 (2.1.325) Stopped `/teleport` from stashing when a `MERGE_AUTOSTASH` record exists or the stash list cannot be read, and warned against `git stash pop` when the list holds an entry it did not add (remote switch, on by default).
- 🟡 (2.1.325) Added a check after worktree creation for an unexpected `commondir` file in the repository's Git folder: setup stops, and the file is deleted when that can be done safely (remote switch, on by default).
- 🟡 (2.1.325) Extended Bash approval checks to allowed bare assignments that may alter exported variables, except safe names, full-command allow rules and sandbox auto-approval (remote switch, on by default); `CLAUDE_CODE_DISABLE_EXPORTED_VARIABLE_PROMPT` turns this off.
- 🟡 (2.1.261) Added session archiving to the terminal's resume pickers: Ctrl+E archives or restores the selected session, and Ctrl+S shows archived ones (remote switch, off by default).
- 🟡 (2.1.269) Added taking a queued message back from a cloud session in the terminal, returning its text to the prompt and warning when Claude may already have read it (remote switch, off by default).

---

### 2.1.323 ◆◇◇◇

- 🔴 Moved connector downloads out of the working tree's `connector-downloads` folder into Claude Code's per-project data folder, where `cleanupPeriodDays` cleanup applies (remote switch, off by default).
- 🔴 Added `permissions.blockFileToolReadsOutsideWorkingDirectories` in launch `--settings`: file reads use host folders plus Claude-owned files and path exemptions; `/add-dir` cannot widen them, and shell commands are unchanged.
- 🔴 Stopped `mcp_tool` hooks in cloud sessions from calling the linked-computer (`remote-devices`) or Claude in Chrome servers, because a hook call skips the normal permission check.
- 🔴 Refused subagent MCP resource reads and `mcp_tool` calls from subagents or certain background hooks to session-metadata servers that would treat those calls as the main session's.
- 🔴 Removed the `<package>@npm` route for installing a plugin straight from npm (remote switch, off by default); npm sources listed in a marketplace still work.
- 🔴 Added `/motion` and `/whiteboard` prompts for published Artifact types. They need Artifact access or host-provided command support; Whiteboard also needs organization policy.
- 🔴 Added `CLAUDE_CODE_OVERLOADED_RETRY_MAX_WAIT_MS` to cap the total time the main conversation waits through overload retries, from 5 to 60 minutes; `CLAUDE_CODE_MAX_RETRIES` and the retry watchdog take precedence.
- 🔴 Added a post-compaction reminder to re-read earlier text with the claude.ai conversation tool before quoting it, and to say so when it cannot (remote switch, off by default).
- 🔴 Added a warning when a named shared-memory delete is unconfirmed and sync is not sending changes, telling Claude that other sessions may still read the content and to correct any claim that it is gone.
- 🔴 Raised experimental observers from one to four per agent, named in the comma-separated `observer` field; `CLAUDE_CODE_EXPERIMENTAL_OBSERVER_AGENTS=1` enables them.
- 🟢 Widened Code Mode's model check from Opus 5.5 and Sonnet 5.5 to Fable and Mythos 5.2 or later and other Claude families at 5.5 or later; its experimental opt-in still applies (remote switch, off by default).
- 🟢 Raised the fallback context window for unknown model IDs from 200K to 1M; model data and window overrides still win, and `CLAUDE_CODE_DISABLE_1M_CONTEXT=1` keeps the fallback at 200K.
- 🟢 Stopped writing new allow rules to settings when a secret scan flags or fails, or Git tracks the local settings file; existing saved rules are left alone.
- 🟢 Checked plugins against `engines["claude-code"]` in `plugin.json` at install, update and load; refused incompatible copies, but warned and continued for local author folders.

---

### 2.1.322 ◆◆◆◆

- 🟡 (2.1.323) Left plugins' `config.set` hooks unasked when `/fast on` or `/fast off` is typed on Vertex; the `/fast` menu still asks them.
- 🟢 Added fast mode on Vertex AI for Fable 5.2 and Mythos 5.2 in the `global`, `us` and `eu` regions; `/fast` turns it on for the session only, moves an unsupported model to Fable 5.2, and shows the price as 2x standard.
- 🟢 Retried Vertex fast-mode refusals at standard speed; a 400 about fast mode or `speed` skips fast mode for that model until `/fast on` or restart, while a 403 or 404 pauses fast mode for 30 minutes.

---

### 2.1.321 ◆◇◇◇

- 🔴 Added folder browsing to `claude remote-control`: it lists folders below the shared folder for claude.ai and can start a session in a picked one unless settings' Read rules cannot be checked (remote switch, off by default).
- 🔴 Added a system-prompt rule to treat the user's name, project, email address and other personal details as private and keep them out of requests to outside services unless asked (remote switch, off by default).
- 🔴 Added a system-prompt note for Fable 5.2 and Mythos 5.2 to check documentation more often on tasks such as accelerator kernel optimization, citing training measures that limit advances in a narrow set of AI-development skills.
- 🔴 Added user-delegated runner sessions (`CLAUDE_CODE_USER_DELEGATED_SESSION_DIRECT_API`) that refuse gateways, custom API endpoints, background sessions and `claude plugin eval`, and limit local auto-mode checks to allowed models.
- 🔴 Blocked more Git forms in worktree-isolated Bash calls: `fast-import --allow-unsafe-features`, and (remote switch, on by default) `git config --edit`, `git rebase --exec` and Git run from above the shared checkout.
- 🔴 Added mod notification requests for supporting hosts: `$.ui.notify` accepts a key, `delayMs` or `at` up to 7 days ahead, and `untilBack` to cancel a pending alert on return; `$.ui.cancelNotify` asks the host to withdraw one.
- 🟢 Added Fable 5.2 (`claude-fable-5-2`, 1M context) as `fable`'s built-in default outside gateways; eligible saved 5.1 picks move at startup, with Team/Enterprise waiting for organization access (remote switch, on by default).
- 🟢 Added `claude remote-control --settings <file>` for one JSON file shared by its child sessions; inline JSON is refused, and file changes prevent new sessions until Remote Control restarts.
- 🟢 Changed the privacy hook to deny Bash, Monitor and PowerShell inputs containing the signed-in email and a User-Agent-like setting, replacing broader email prompts (remote switch, on by default).
- 🟢 Added `claude plugin restore-set-aside <plugin>` for options and secrets saved during a managed marketplace replacement; `--discard` deletes those saved values instead, leaving plugin data folders alone.

---

### 2.1.319 ◆◇◇◇

- 🔴 Added approved downloads from claude.ai connectors into a Git-ignored `connector-downloads` folder, up to 10 files per result and 100 MiB each (remote switch, off by default).
- 🔴 Added local-file uploads for compatible claude.ai connectors through MCP's file-upload extension, with approval at Claude Code's own terminal (remote switch, off by default).
- 🔴 Skipped `CLAUDE.md` and project skills in `connector-downloads` folders and labeled reads as connector data. Project settings and `.mcp.json` are skipped when download files or a `*` `.gitignore` mark the folder.
- 🔴 Made `connector-downloads` folders in working directories or their Git roots block web tools under the existing connector-isolation policy; conversations can stay restricted after the folder is removed.
- 🔴 Blocked isolated-worktree Bash commands using Git options that can affect another checkout, including `--ignore-other-worktrees`, forced `worktree add`, `fetch --update-head-ok` and `apply --unsafe-paths`.
- 🔴 Added opt-in loading of large saved conversations from a resume index and newer transcript text, with whole-file fallback: `CLAUDE_CODE_TRANSCRIPT_TAIL_LOAD=on` (macOS and Linux).
- 🟢 Added `permissions.denyInsteadOfPrompting` to refuse calls still awaiting approval after rules, auto mode and hooks decide; allowed calls continue, and repeated refusals can end the turn.
- 🟢 Added `CLAUDE_CODE_DISABLE_FEATURE_FLAGS=1` as an alternative to `DISABLE_GROWTHBOOK=1` for stopping feature-switch fetching without turning off telemetry.

---

### 2.1.318 ◆◇◇◇

- 🔴 Added `claude --cloud <local-session-id>` to continue saved history in the cloud, filtering account-memory blocks and local attachments while leaving the original session unchanged (remote switch, off by default).
- 🔴 Added `/project` to assign usage reports to a claude.ai Project; `--save` applies it to future sessions in that folder (remote switch, off by default).
- 🔴 Added `claude ultrareview eval` for merged pull requests and JSON reports; `--max-cost` caps review starts, but the final bill can exceed it (remote switch, off by default).
- 🔴 Stopped allow rules for tools with the same Claude Code name within one MCP server; the rules work again once the server renames one of the tools (remote switches, off by default).
- 🔴 Added permission checks for private Artifacts marked as visible outside your organization, including unattended routine edits.
- 🔴 Added warnings when compaction, subagents and workflows relay text marked as a Remote Control report from another device, telling Claude to treat it as data rather than instructions.
- 🔴 Restricted more SSH proxy commands with unclear `%` expansions during worktree isolation, asking for written-out hosts and users (remote switch, on by default).
- 🟡 (2.1.323) Added opt-in experimental JavaScript code mode in the native interactive terminal for Opus 5.5 and Sonnet 5.5 through Anthropic. Auto mode reviews scripts before execution (remote switch, off by default).
- 🟢 Added stored-login expiry dates to `claude auth status` (`loginExpiresAt`) and headless JSON (`credential_expires_at`), plus warnings for headless text output. Dates can be estimates.
- 🟢 Stopped unattended launches from overwriting the global configuration when its JSON is malformed; errors explain how to restore a backup or repair the file.
- 🟢 Added input schemas or tool-search advice to some MCP tool-call errors when the schema was missing from Claude's prompt.

---

### 2.1.317 ◆◇◇◇

- 🔴 Added `/use-case` to choose an account's Life Sciences use case from the next request onward and save it for later sessions (remote switch, off by default).
- 🔴 Added Gmail attachments by file reference for sends and new drafts with named recipients, with per-call approval outside bypass mode (remote switch, off by default).
- 🔴 Made cloud routine changes and manual runs ignore whole-tool allow rules and request approval in ordinary modes; bypass mode can still allow them, and auto mode still reviews them.
- 🔴 Made Windows sandbox startup failures from extra hard links on protected files block sandboxed commands and outside-sandbox retries (remote switch, on by default).
- 🔴 Stopped `/goal` relayed from Slack, Teams or project threads from setting, clearing or showing the goal (remote switch, on by default).
- 🔴 Added warnings when a host reports personal hooks are off under `CLAUDE_CODE_RESTRICT_PERSONAL_CONFIG`.
- 🔴 Added warnings for images hidden after an attachment deletion, telling Claude to avoid new visual claims or reopening saved copies.
- 🟡 (2.1.318) Added checks that refuse selected command forms on Bash 2–3, and some on Bash 4, with advice to simplify the command or use a newer shell (remote switch, on by default).
- 🟢 Added SDK hook failure blocking with `onFailure` set to `block`; callback errors or invalid replies stop tool calls and prompts, and the TypeScript SDK refuses a CLI that cannot honor it.
- 🟢 Made detected disk-full or quota errors saving transcripts or config exit 1 in `claude -p` text and JSON modes; `stream-json` warns without changing status. Desktop and SDK URL sessions skip this check (remote switch, on by default).
- 🟢 Marked partial Grep and Glob results as incomplete when a search times out, so Claude sees a warning even when some matches or no files are returned.

---

### 2.1.316 ◆◆◆◆

- 🔴 Changed macOS background sessions to build `ClaudeCode.app` from the embedded plist, or relaunch directly if absent, for executables in the native installer's versions folder; a remote wrapper-bypass switch defaults off.

---

### 2.1.315 ◆◆◆◆

- 🔴 Allowed model configuration to turn on Bash edit diffs in Auto and bypass-permissions modes, while `CLAUDE_CODE_BASH_EDIT_DIFF` and `bashEditDiffEnabled` still take priority; the setting's description now explains the default.
- 🔴 Split long Bash edit-diff path lists into smaller Git calls using a 28,000-byte argument estimate, for both CRLF-only filtering and replacement patches.
- 🔴 Added up to three warnings per session telling Claude when the shared memory limit killed a process during an eligible foreground Bash command, even if the killed process belonged to another task (remote switch, off by default).
- 🔴 Added beginning-and-ending previews for eligible saved Bash output, with omitted-byte counts; separate remote switches control previews for successful and failed commands, and both default off.
- 🔴 Added remotely configured advice after eligible Bash commands, matching command kind, runtime, success or failure and output filtering; up to eight rules can each show a note one to five times per session, with no rules by default.
- 🔴 Added shared Git snapshots and diff reuse for concurrent Bash edits, with changed-file lists when full patches fail and wording that other commands may have contributed (remote switch, off by default). Shared-snapshot Git timeouts allow three event-loop passes before aborting.
- 🔴 Stopped asking before Claude in Chrome creates its own tab group when the session and account both use `acceptEdits`, the tool advertises this shortcut and no Chrome MCP mode override is set; the `tabs_context_mcp` call must contain only `createIfEmpty: true`.
- 🔴 Removed the experimental keepalive-triggered and log-only idle-compaction paths; idle compaction now uses only the existing one-hour prompt-cache timer, still set to 90% of its lifetime.
- 🔴 Changed old rate-limit checkpoint ref cleanup to use one sandboxed Git command when sandboxing is on (remote switch, on by default), and report pruning failures without discarding a successful checkpoint.
- 🔴 Changed shared Git subprocess settings to replace a repository's `core.alternateRefsCommand` with Claude Code's `git for-each-ref` helper (remote switch, on by default).
- 🔴 Added checks that `.git` belongs to the checkout before automatic runtime-file ignore writes, plus shared open-file checks for those writes and checkpoint ignores that skip links, hard-linked files, detected replacements and files over 1 MiB (remote switch, on by default).
- 🔴 Changed linked-worktree locking to create the `locked` marker directly instead of invoking `git worktree lock`, after checking that both checkouts share Git's common directory (remote switch, on by default). The initial write refuses an existing marker; stale-lock recovery can still clear one and retry.
- 🔴 Changed Git metadata caches on Linux with `CLAUDE_CODE_CONFIG_WATCH_EVENTS` set to check watched files on reads, at most once a second and with a 100 ms wait limit; added-repository HEAD watches use filesystem events with polling fallback, and stopped watches leave the read-check list.
- 🔴 Changed project-level configuration imports to read text and copy skills through checked file handles, enforce the 10 MiB text-read limit and create new skill, rule, command and agent files without replacing existing targets (remote switch, on by default). Skill copies drop special permission bits and leave partial copies with retry instructions after failure; user-level imports are unchanged.
- 🔴 Let running sessions add the Artifacts MCP server when tool availability or organization policy becomes ready after startup, with a matching signed-in account and the normal MCP permission checks still required.
- 🔴 Skipped a second size-check walk for ASCII-only MCP listing entries, counting their original size even when sanitization removes fields; listings near the limit can therefore lose trailing tools or prompts.
- 🔴 Rechecked current MCP access after OAuth redirect discovery and before a localhost retry, stopping sign-in when a server is now disabled or blocked; control-request sign-in also checks for an account change.
- 🔴 Stopped extra MCP reconnects while a scheduled retry is in progress, including slow background attempts after the first fast retries.
- 🔴 Added `CLAUDE_CODE_SORTED_OTTER` to switch MCP's handling of reconnecting servers on or off, including tool holds and instruction updates; it overrides a remote switch that's off by default.
- 🔴 Changed `claude mcp serve` to answer repository permission-rule requests with the last good read or other available settings after a file fails to load, warning when rules are skipped instead of refusing the tool call.
- 🔴 Fixed remote-workspace startup warnings to list only ignored `--mcp-config` and agent-frontmatter servers, and added a separate warning when `--chrome` has no effect because MCP runs remotely.
- 🔴 Restricted the memory agent's shell deletions to Markdown paths with no colon below the memory directory, blocking Windows alternate-data-stream paths that otherwise pass the `.md` check.
- 🔴 Added withdrawal of stale or unavailable Cowork memory from subagents and other spawned loops after tool steps (remote memory switch, off by default).
- 🔴 Improved unavailable-model errors to include the catalog's explanation, naming the required Claude Code version only when the model is offered on the current provider.
- 🔴 Cleaned model-validation and model-switch errors to remove control and hidden characters and neutralize Markdown links, shortening displayed model names after 128 characters and capping final model-switch refusals at 1,024 UTF-16 units.
- 🔴 Fixed the auto-memory read exception for searches whose root directory lacks a trailing separator when `permissions.blockReadsOutsideWorkingDirectories` is on.
- 🔴 Made the write-path guard check edit-deny rules against every supplied path spelling before its directory checks, including when outside-directory write blocking is off.
- 🔴 Changed environment-variable wildcard checks to treat unexpected normalization or decoding errors as a possible wildcard; decoding errors caused by malformed percent escapes still return no wildcard.
- 🔴 Restricted `/loop` from relay participants and non-human input, except scheduled triggers and two exempt channels; human Remote Control input still works (remote switch, on by default).
- 🔴 Added normal permission and Auto mode review for MCP resource reads instead of their built-in automatic allowance, hiding “Always Allow” shortcuts (remote switch, off by default). With it on, directory reads fail their permission check when listing is unavailable, and hooks or plugins can only tighten resource-read decisions in Cowork cloud Auto mode.
- 🔴 Reduced permission context for selected MCP checks in default, accept-edits, `dontAsk` and bypass modes by omitting ordinary allow/ask rules and extra directory details while keeping deny rules and the main directory; `CLAUDE_CODE_AUTO_MODE_FULL_CONTEXT=1` restores the full context. Context-size diagnostics now distinguish full and reduced reports and count configured Auto mode allow rules before the scheduled-task rule is added.
- 🔴 Stopped saved session allow rules from skipping cloud-browser safety checks in Auto, bypass-permissions and qualifying plan modes; protected calls lose “Always Allow” choices and rule suggestions, and saved browser allow rules are also left out of classifier context.
- 🔴 Changed Google Drive file-reference handling to log `dontAsk` refusals separately and return the existing refusal message in some unattended cases with an ask rule when a hook's approval was ignored (remote switch, off by default). Desktop keeps its ask-rule exclusions and skips this extra refusal in `dontAsk`.
- 🔴 Changed recognized plugin prompts to show their body and plugin label instead of the model-facing wrapper in transcripts, session previews, titles, search and default `/export` filenames; older transcripts and hook-rewritten prompts can recover the display information.
- 🔴 Removed the instruction to match surrounding code's comment density, naming and idiom from readable system prompts; the lean fallback now omits the communication section entirely.
- 🔴 Made cloud hook script-loading warnings clean up their explanation and cut it to 400 UTF-16 code units before adding the warning's wording (remote switch, on by default).
- 🔴 Changed SDK descriptions, unattended-run guidance, organization-restricted MCP notices and artifact permission explanations to call cloud routines scheduled tasks.
- 🔴 Changed move-to-cloud handling to recognize an already-cloud session from HTTP 400 with `session_not_bridge`, rather than matching words in the error message.
- 🔴 Changed sandbox cleanup to move suspicious directories aside instead of deleting their contents and explain recovery. Outside Windows it also checks saved parent directories and restores missing owner permissions; failed moves can still remove links or make owned entries unreadable.
- 🔴 Added write protection for initially missing Git `objects/info/alternates` files in Linux and WSL sandboxes when the Git directory, `objects` and `objects/info` are directories rather than symlinks (remote switch, on by default).
- 🔴 Changed Linux and WSL sandbox setup to create Git `config.worktree` placeholders through open directory handles, with no-follow checks on the Git directory and each subdirectory it opens. Linked worktrees and deferred thin-client setup use the same helper.
- 🔴 Added sandbox write protection for staged `mcp_call` files on non-Windows systems, plus checks on input-file destinations and their parents and refusal to overwrite an existing name (remote switch, on by default).
- 🔴 Stopped settings-file `env` blocks from changing the new background-socket, plugin-reload, MCP-relist, policy-redial, held-message and copied-session launch controls; project settings also can't set `CLAUDE_CODE_SORTED_OTTER` or use `CLAUDE_CODE_DISABLE_ULTRATHINK` to re-enable the keyword.
- 🔴 Changed remote-settings cache loading to require two consecutive matching disk reads, trying up to five reads and ignoring a copy that keeps changing.
- 🔴 Added regular-file checks to signed-cache, remote-settings and helper-consent saves, plus a data-sync step before signed-cache and helper-consent writes finish.
- 🔴 Made settings reads return the last successfully parsed values after temporary filesystem errors such as `EBUSY` or `EMFILE`, while still reporting that the current read failed.
- 🔴 Improved the Bash safety-check error for `for ((…)) { … }` bodies that already fail validation, recommending `do …; done` syntax (remote switch, on by default).
- 🔴 Stopped worktree-isolation checks from assuming assignments inside C-style loop bodies or command and process substitutions are integers just because the surrounding construct uses arithmetic (remote switch, on by default).
- 🔴 Restricted worktree-isolated `gcloud compute ssh`, `kubectl exec` and `oc exec` commands with ambiguous global options or local credential and configuration overrides instead of assuming their effects stay remote (remote switch, on by default).
- 🔴 Restricted worktree-isolated `sed` commands whose option order can hide the program, including bare `-i` consuming the next word as a backup suffix on macOS and BSD; computed programs and backup suffixes are refused (remote switch, on by default).
- 🔴 Restricted arithmetic in worktree-isolated Bash commands when an assigned value names a shell-managed variable and an unset arithmetic operand could inherit text leading back to it, with guidance to run the arithmetic separately (remote switch, on by default).
- 🔴 Restricted worktree-isolated shell commands when empty expansions, unmatched globs or array expansions can remove an argument and shift a later word into an option, host or script position, including in SSH, `gcloud`, interpreters and `set` (remote switch, on by default).
- 🔴 Restricted worktree-isolated `test` and single-bracket expressions using `-v`, `-R` or `-t` to plain variable names or digits even when the expression parses as binary; `[[ … ]]` keeps its separate checks (remote switch, on by default).
- 🔴 Restricted worktree-isolated `sshpass` commands with unreadable options, disappearing arguments or launchers hiding a later remote client, while following nested `sshpass` calls and checking `rsh` like SSH (remote switch, on by default).
- 🔴 Improved recognition of OAuth failures for missing accounts, missing organizations and lost membership, so matching response codes or plain-text denials can be classified as organization-access failures.
- 🔴 Made gateway sign-in show "Finishing sign-in…" for accounts with or without an email and ignore Escape while credentials are being saved or the successful sign-in is finishing.
- 🔴 Changed background-agent progress summaries to wait 30 seconds between refresh attempts instead of following a remotely configured interval sequence; callers can still override the delay, and cold-cache waits still check every 2 seconds.
- 🔴 Changed short background-agent summaries to use only an eligible default Haiku model on Anthropic's API, a fixed 2,000-token input allowance and the task's language unless a language is set (remote switch, off by default; hosts that request SDK progress summaries use full summaries). Ineligible writers use full summaries; a failed short-summary request switches later summaries to full.
- 🔴 Added policy and account checks to Anthropic's safety-event telemetry: sessions subject to policy checks withhold these events until policy and account attribution are available, and HIPAA-blocked or wrong-account events are dropped. Saved events are checked again before upload, and account-marked batches no longer retry an HTTP 401 without authentication.
- 🔴 Changed terminal text cleanup to replace standalone Escape tokens with the Cancel control byte, while leaving complete escape sequences on their existing path.
- 🔴 Blocked eight more CA public-key hashes and included them in Java proxy truststore cache filenames so caches built with the earlier filter aren't reused.
- 🔴 Changed certificate trust-data detection to require decoded PEM bytes to start with the parsed certificate before treating extra bytes as appended trust data.
- 🔴 Changed CA cleanup to check self-issued certificates' signatures, with exemptions for bundled roots, appended trust data and trusted issuer chains; `CLAUDE_CODE_DISABLE_SELF_NAMED_CERT_DROP=1` opts out, and client-certificate setups still skip main-list chain cleanup.
- 🔴 Changed HTTPS connection reuse to separate sockets when `NODE_TLS_REJECT_UNAUTHORIZED` or `BUN_TLS_REFUSE_DISTRUSTED_KEYS` changes, while keeping the main agent without client certificates across routine cache clears; `CLAUDE_CODE_DISABLE_HTTPS_AGENT_KEEPING` opts out.
- 🔴 Changed DNS-name TLS setup to leave hostname verification to the runtime; HTTPS proxy connections do the same unless a caller supplies its own check.
- 🔴 Changed `mcp__easel__chart` to refuse calls when a session doesn't provide a conversation array, explaining that charting isn't available there.
- 🔴 Changed message-ID loading for another session to use its project's recorded transcript path when available.
- 🔴 Clarified the archived cloud-session warning to say messaging can resume after unarchiving the session at claude.ai, while keeping the existing new-session suggestion and session link.
- 🔴 Fixed permission feedback menus allowing disabled choices to be selected or submitted.
- 🔴 Added a period after fallback-notice titles when joining them to warning text, unless they already end with one.
- 🔴 Stopped using locale values longer than 64 characters from `LC_ALL`, `LC_TIME` or `LANG` when formatting dates and times.
- 🔴 Restricted links in verbose memory-save notices to eligible auto-memory `.md` files on platforms other than Windows or WSL, rejecting colons below the memory directory; other paths get non-clickable rows (remote switch, on by default).
- 🔴 Restricted memory-save status verbs to 1–32 ASCII letters or hyphens, starting with a letter; invalid values now display `Saved`.
- 🔴 Sanitized service-supplied usage-limit messages and resume prompts by stripping invisible characters, normalizing Unicode and escaping angle brackets, with a 2,000-character limit (remote switch, on by default). Resume prompts also reject `/`, `@`, exit commands and `ultracode`/`ultraplan`/`ultrareview`/`ultrathink`, falling back to built-in text.
- 🔴 Rejected working-directory changes into Bun's embedded filesystem, including startup deep links, and recognized Windows embedded paths on any drive regardless of letter case.
- 🔴 Made failed session-directory updates try to restore the process's previous working directory before returning the error, including `/cd` and startup deep links.
- 🟡 (2.1.316) Applied `--restricted` read rules to both requested and resolved attachment paths, alongside `permissions.blockReadsOutsideWorkingDirectories`. Removed the base64 workaround from the `cannot_upload` error in restricted mode when the host doesn't supply the contents.
- 🟡 (2.1.316) Stopped Bash snapshots from turning on disabled options whose names contain `on`, while keeping the existing `monitor` exception.
- 🟡 (2.1.316) Fixed Bash snapshots losing functions whose names start with `-` or `+`.
- 🟡 (2.1.316) Fixed multiline Bash aliases outside Git Bash and shell snapshot failures from empty winpty filters or large alias lists; snapshots still keep only the first 1,000 alias lines.
- 🟡 (2.1.316) Fixed Claude in Chrome uploads using a Read allow rule to bypass `--restricted` working-directory limits, including when the session also uses `bypassPermissions`.
- 🟡 (2.1.317) Allowed files to be read again after their earlier Read or shell-read results leave the model's context through tool-result clearing (remote switch, off by default); later complete reads or writes remove the pending reread mark.
- 🟡 (2.1.316) Fixed duplicate saved MCP disable entries swallowing a newly disabled server, and made the last occurrence of each server count toward the saved list's 64-server limit.
- 🟡 (2.1.311) Made Opus alias migration and provider setup add `[1m]` only when the model needs it, while keeping the context suffix chosen for Opus Plan fallbacks.
- 🟡 (2.1.316) Improved Bedrock and Vertex model-access checks to recognize account, Marketplace, region and VPC Service Controls refusals, including Vertex errors with different capitalization.
- 🟡 (2.1.316) Added a local `/model` confirmation warning when Claude Code doesn't recognize the chosen model, naming the context window auto-compact assumes and pointing to ways to configure it.
- 🟡 (2.1.316) Added `CLAUDE_CODE_DISABLE_ULTRATHINK` to disable the `ultrathink` effort boost, and support for triggering it from eligible local standard-input prompts, including VS Code and Claude Desktop (host support uses a remote switch, off by default).
- 🟡 (2.1.316) Fixed buffered HTTP response errors being swallowed after the request was destroyed, which could leave the request hanging.
- 🟡 (2.1.316) Stopped host settings from restoring defaults when a nonempty managed `autoMode.allow` omits `$defaults`, and `/permissions` or `/auto-mode-setup` from restoring them when the combined allow list is nonempty and omits it. Kept the scheduled-task rule that automated prompts aren't fresh human consent even when custom rules replace defaults.
- 🟡 (2.1.316) Fixed `claude -p` text errors losing or showing another turn's Auto mode stop reason when a result waits behind background work; the explanation now travels with the result and prints before “Execution error”.
- 🟡 (2.1.316) Made Claude Code stop live and already-stopping shell command trees when it exits on macOS and Linux, except jobs marked to stay running, such as asynchronous hooks (remote switch, on by default). Cleanup gives processes up to 1.5 seconds before a forced kill.
- 🟡 (2.1.316) Fixed Grep and Glob hanging after their search timeout when ripgrep doesn't report that it exited; timeout completion no longer depends on a process-exit event.
- 🟡 (2.1.218) Made settings-approval summaries omit `CLAUDE_CODE_SUBAGENT_EFFORT`, plus `CLAUDE_CODE_DISABLE_ULTRATHINK` when its value disables the keyword.
- 🟡 (2.1.316) Added organization detection for eligible Team and Enterprise stored claude.ai logins so their organization's API-fetched managed settings apply without approval and signed responses can be checked against it; gateway, hybrid-organization and non-production OAuth configurations are excluded.
- 🟡 (2.1.309) Stopped worktree-isolation checks from trusting values in Bash and zsh variables the shell rewrites, including zsh's `MATCH`, `argv` and `reply`, and refused integer or float declarations that could make their later contents execute as arithmetic (remote switch, on by default).
- 🟡 (2.1.316) Added interactive gateway sign-in to terminal `claude auth login` when settings require a gateway and no `--console`, `--claudeai`, `--sso` or `--email` option is supplied.
- 🟡 (2.1.314) Extended saved-login checks to consider server-delivered `apiKeyHelper` settings while they're loading or refused, and matching endpoint or provider overrides left behind after those settings are removed.
- 🟡 (2.1.314) Improved saved-login refusal messages to distinguish an administrator configuration problem, organization settings that couldn't load and a sign-in needing a restart, including file uploads and self-hosted runner requests.
- 🟡 (2.1.316) Fixed nested subagents losing an inherited non-default parent model from their saved metadata, and stopped later metadata updates from dropping a saved model choice.
- 🟡 (2.1.316) Added `CLAUDE_CODE_SUBAGENT_EFFORT` for non-fork subagents and workflow agents without an explicit or inherited effort, except built-in web-fetch; `CLAUDE_CODE_EFFORT_LEVEL` still wins. Background jobs and launched teammates pass it through, and teammates also receive `CLAUDE_CODE_DISABLE_ULTRATHINK`.
- 🟡 (2.1.316) Made prompts cleaned of invisible characters send on the first Enter (remote switch, on by default); non-prompt modes, bidirectional controls, empty text, `/` or `!` prefixes and exit words still require review.
- 🟡 (2.1.316) Improved TLS error guidance to distinguish a server's missing intermediate certificate from a trusted CA whose issuer is missing, and directed Claude Desktop and `local-agent` users to their IT team's certificate settings.
- 🟡 (2.1.316) Improved missing-GitHub-repository messages for `/ultrareview`, cloud sessions and cloud agents to suggest creating a repository at https://github.com/new and pushing the project.
- 🟡 (2.1.316) Made first-run security notes, terminal-setup explanations and the power-up tour introduction shrink to fit terminals narrower than 70 columns.
- 🟡 (2.1.316) Delayed automatic `/diff` refreshes after slow Git work, by up to 30 seconds or up to 60 seconds after repeated failures lasting at least five seconds (remote switch, on by default). Manual refreshes and base changes can skip the wait.
- 🟡 (2.1.316) Reduced Git queries in `/diff` by normally requesting per-file statistics first; bases with cut-off output are remembered so changes above 500 files can use totals without repeatedly requesting details (remote switch, on by default).
- 🟡 (2.1.316) Changed organization survey invitations to wait for the configured submit key after a typed `1` or `0`, rather than a pause; blocking overlays or replacement input rows stop keyboard confirmation, and the card shows when it's opening.
- 🟡 (2.1.316) Fixed the voice-footer hint using up its three displays without being shown or on footer remounts; displays now count once per session, and no hint appears when push-to-talk has no visible key binding.

<details>
<summary>91 more for administrators and for authors of plugins, hooks, MCP servers and SDK hosts</summary>

- 🔴 Stopped adding fallback relative `@`-file references to Cowork context when the tools directory is marked as non-Git; absolute references, attached contents, oversized-file notices and PDF references still work.
- 🔴 Kept Gmail attachment download tool names across conversation clears and worker restores, recognizing both bare `download_gmail_attachment` and names prefixed with `__` or `:`.
- 🔴 Started fetching eligible images' model copies alongside their original attachments, reusing the request only while the token matches and retrying transient failures; the existing lower size threshold still needs a remote switch, off by default.
- 🔴 Changed task-file uploads blocked by stored-login checks to report `egress_blocked` instead of `upload_network`.
- 🔴 Added extracted text from uniquely named uploaded project documents to eligible project context, sharing the 20-item limit with knowledge documents and keeping accepted knowledge text if uploads fail or don't fit (remote switch, off by default).
- 🔴 Changed background sockets with authentication configured to wait for a nonempty line before takeover, or a protocol frame before terminal replay, including the wait after shell exit (remote switch, on by default). Added a 5-second inactivity timeout, adjustable from 100–5,000 ms with `CLAUDE_BG_SILENT_PEER_TIMEOUT_MS`; the daemon sets or clears `CLAUDE_BG_SOCK_ADMIT_SILENT_PEERS` to choose immediate handling or waiting.
- 🔴 Added an option to skip unused Bash file-change diffs in noninteractive remote parent sessions without `CLAUDE_CODE_ENVIRONMENT_KIND` (remote switch, off by default); explicit diff settings, first-party client prompts and plugin, hook or file-watch consumers keep them.
- 🔴 Added automatic comment watching for newly created Claude Docs in remote sessions, with instructions to reply on the document rather than a hidden relay (remote switch, on by default; requires the Docs connector and Artifact comments). Unavailable comment wakes now produce a passive warning that this session isn't subscribed.
- 🔴 Required plugin-supplied dynamic HTTP connectors in remote sessions to use an HTTPS first-party Docs endpoint before Claude Code treats them as Claude Docs connectors; a matching name or metadata alone no longer suffices.
- 🔴 Changed cloud command hooks to suppress Git log signatures, use `medium` log formatting, disable automatic Git maintenance and index refresh, and replace repository alternate-ref commands (remote switch, on by default).
- 🔴 Expanded cloud-session hook matchers to support counted repetitions, named groups and general positive or negative lookahead, with compilation limited to six times (pattern length + 1) work units; local sessions keep native regular expressions.
- 🔴 Moved the check for unreadable or startup-blocking machine policy from the shared hooks-disabled test to repository-hook listing, so selection and state comparison no longer make that check. Hook-state comparison now sends all caught errors to the common logger.
- 🔴 Added the triggering `toolUseID` and a shared `limit.startedAt` value to remote plugin-hook calls; the caller now reads that shared time when distinguishing a timeout from another cancellation.
- 🔴 Raised the remote protocol's advertised maximum from 17 to 18 for repository-hook listings; the default minimum remains 6 and negotiation still chooses the lower peer maximum.
- 🔴 Added a cancellation frame for an MCP question after its response arrives in Structured I/O, before the normal response is resolved or rejected (remote switch, off by default).
- 🔴 Added recognized MCP status text explaining that per-tool `permission_policy`, `always_ask` and `always_deny` don't apply to client-hosted MCP servers.
- 🔴 Added `cloudServiceProvided` to MCP status for recognized cloud-supplied, session-proxied HTTP/SSE servers, so hosts can distinguish them from claude.ai connectors; the status parser also accepts `error_code`.
- 🔴 Changed postponed device-MCP consent to a notice instead of a warning when no in-session consent question is available, and let the normal consent reminder appear when the postponement ends.
- 🔴 Let selected clients skip MCP tool-timeout pauses while a server question is open (remote platform list, empty by default; existing question-pause switch, off by default).
- 🔴 Raised the project and local permission-settings file limit for MCP-served tools from 256 KiB to 2 MiB; user settings already used the larger limit.
- 🔴 Added `CLAUDE_CODE_MCP_SKIP_UNCHANGED_RELIST=1` for managed cloud sessions to reuse unchanged MCP tool wrappers for up to an hour after a fresh tool listing, avoiding state replacements when the same tools are already installed; reuse requires unchanged conversion settings and inactive tool-name remapping.
- 🔴 Added MCP configuration limits of 65,536 UTF-16 code units for server URLs and 4,096 for OAuth `authServerMetadataUrl`, rejecting longer values before further validation.
- 🔴 Clarified SDK error-code documentation for model-version requirements and MCP registration failures, including `--sdk-url` and sessions started by `claude remote-control`.
- 🔴 Started remote feature-setting initialization for hosted `claude mcp serve --transport http` daemons. The feature-setting client now uses environment-supplied identity, skips saving local configuration and omits experiment-exposure logging; its local-configuration read path stays disabled.
- 🔴 Stopped the hosted HTTP MCP daemon from advertising `io.modelcontextprotocol/tasks` even when task support's remote switch is on (off by default).
- 🔴 Added a filesystem-root check for `CLAUDE_CODE_REMOTE_MEMORY_DIR` in remote-memory edit normalization and path warmup, and made the edit containment check case-insensitive on Windows. Warmup now includes the memory directory itself and reports unresolved links separately from success.
- 🔴 Fixed remote-memory file-creation edits with an empty `old_string` losing trailing whitespace; edits to existing non-Markdown content still trim it.
- 🔴 Added validation of the model catalog's optional `early_access` boolean, treating invalid values as `true`.
- 🔴 Changed tool-loading text for models using system-message formatting to follow `tool_loaded_text`, removing standalone “Tool loaded.” blocks when false. Added that capability to 22 built-in models and recognized `bash_edit_diff` as a capability name.
- 🔴 Added `ultrathink_available` in SDK init messages and `ultrathinkAvailable` in `get_settings.applied`, telling hosts when the keyword is available for the current model and effort.
- 🔴 Changed cloud-session approval notices to distinguish calls that need your answer from calls the session may approve automatically, carrying classifier eligibility through live tool-call status and updating the notice when a waiting call starts needing you.
- 🔴 Added launcher-only `permissions.blockReadsExemptPaths` for selected absolute local files or folders to pass the outside-directory read block while normal permissions still apply; updates take effect during a session, but diskless, restricted and `CLAUDE_CODE_EVAL_CONFINED` sessions ignore the list.
- 🔴 Kept auto-memory Markdown files writable under outside-directory write blocking when `CLAUDE_COWORK_MEMORY_PATH_OVERRIDE` chooses the memory directory; restricted sessions and the existing memory-path and settings checks still apply.
- 🔴 Extended tools' unanswered-permission callbacks to `dontAsk` denials, preserving eligible user-only questions and reporting both the denial mode and ignored hook approvals; the calls still stay denied.
- 🔴 Allowed plugins outside the managed-plugin list to read listed attachments through `prompt.media` during an active `prompt.submit` hook; the submission must still be in progress after the read.
- 🔴 Added `CLAUDE_CODE_CCR_ONE_PLUGIN_LOAD_PER_RELOAD=1` to reuse one cache-only plugin load during a refresh when synchronous installs are off, no external-marketplace entries apply and no load is running; a failed or replaced load falls back to a full load.
- 🔴 Added `CLAUDE_CODE_CCR_ONE_PLUGIN_LOAD_PER_RELOAD`, `CLAUDE_CODE_MCP_SKIP_UNCHANGED_RELIST`, `CLAUDE_CODE_MCP_NO_REDIAL_AFTER_POLICY_REFUSAL` and `CLAUDE_CODE_SESSION_BEGAN_AS_COPY` to the environment variables removed when starting child sessions.
- 🔴 Corrected the bundled `/claude-api` references to say Mythos 5.1 enforces thinking-history checks for accounts of any age, including after earlier per-turn reminders are removed. Removed the former exemption and related explanations, and described Bedrock access in `us-east-1` as limited availability.
- 🔴 Changed the bundled `/claude-api` migration checks to test an edited replay on the same model and platform before trusting empty reports, using the target model's own thinking block after the edit; recovery without the controls beta now strips only the error's named block and later `thinking` or `redacted_thinking` blocks.
- 🔴 Expanded plugin-authoring guidance on keeping already-sent prompts and tool descriptions unchanged to avoid losing thinking and prompt-cache reuse, recommending appended context or a new thread or compaction instead. Updated API comments to describe active-submission media access, plugin prompt attribution and scheduled tasks, and warn that re-asking `tool.describe` after resume can overwrite saved descriptions.
- 🔴 Changed the SDK error for combining `sessionStore` with `persistSession: false` to recommend a new private temporary `CLAUDE_CONFIG_DIR` directory that the caller deletes after the query.
- 🔴 Stopped remote archive-status requests when `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` has any nonempty value.
- 🔴 Added one retry for remote archive-status reads after an expired-login error when the session's login-refresh callback succeeds, using the current access token.
- 🔴 Fixed overlapping remote archive-status checks so a session reset can start a fresh request and an older request finishing won't clear the newer one.
- 🔴 Added server-supplied turn IDs to eligible cloud-browser sessions for same-session input, checking its origin and waiting up to two seconds for feature settings, then clearing the ID when the turn ends (remote switch, off by default, or an explicit session allowance).
- 🔴 Added `CLAUDE_CODE_DISABLE_HELD_MESSAGE_RECOGNITION` to disable held-message recognition and the self-hosted startup check for held messages in saved transcripts, and passed it to runner children. Gave that startup check its own remote switch (on by default), separate from ordinary recognition (remote switch, off by default).
- 🔴 Added busy-state tracking for foreground Bash calls in cloud sessions whose workers can move them into the background, with a two-second recognition delay and restoration from session history.
- 🔴 Stopped older self-hosted session-token renewal work from retrying or applying returned configuration once a newer token becomes current.
- 🔴 Restricted self-hosted runner-tool IDs to 1–256 ASCII letters, digits, `_` or `-`, and rejected hidden characters in text inputs. Local launch paths now reject control characters and line breaks, with guidance to use a readable path or ask for the file to be renamed or moved.
- 🔴 Changed SDK `register_repo_root` permission checks to use the registered directory's own path instead of expanding it to alternate resolved paths.
- 🔴 Expanded SDK API-error descriptions to cover organization-imposed API-key helpers, unconfirmed settings and who can resolve a refused stored login, plus the response fields accompanying `model_requires_usage_credits` errors.
- 🔴 Made Send now in print-mode sessions wait for automatic compaction to finish before delivering queued messages, keeping its summary (remote switches, on by default).
- 🔴 Added `CLAUDE_CODE_SESSION_BEGAN_AS_COPY=1` and hidden `--session-began-as-copy 1` so print-mode resume won't automatically continue the source session's unfinished turn or run its deferred tool call; child processes don't inherit the environment marker.
- 🔴 Extended Cowork's restrictions-only managed-settings path to diskless host sessions marked as derived from Cowork, skipping its loading wait and reporting no managed hooks when no hooks or policy errors are found; a remote Cowork launch surface alone no longer skips managed-settings checks.
- 🔴 Restricted storage-backend saves of remote settings to diskless host sessions; sessions with disk access now save to local files even when a backend is supplied.
- 🔴 Added hook-script checks for heredocs that cross command- or process-substitution parsing boundaries, including a closing parenthesis before the heredoc body arrives (remote switch, on by default).
- 🔴 Extended hook-script checks to follow relative paths stored in variables through interpreter search paths, Git configuration, shell startup files and nested shell scripts (remote switch, on by default).
- 🔴 Added cloud-session checks for local hook scripts mentioning zsh's `histchars` or `HISTCHARS`, even in comments or behind quote and escape syntax, because these variables can make apparent comments execute (remote switch, on by default).
- 🔴 Stopped developer-configured gateways from being offered when `forceLoginMethod` requires `claudeai` or `console`, and stopped applying managed `gatewayInternalNetworks` to developer-selected gateway URLs.
- 🔴 Stopped OAuth recovery from treating every `ANTHROPIC_UNIX_SOCKET` connection as an SSH tunnel; only a matching `ssh-placeholder` credential now gets the tunnel-specific handling for supplied and recovered logins.
- 🔴 Changed workload-identity policy tracking to remember the first profile or credential source even before its organization is known, so a later source change can't silently become the starting point.
- 🔴 Added remote text-upload counts, gaps over 500 ms and maximum flush-timer lateness to successful SDK results when first-text timing is active and the session isn't diskless; counts cover posts with new, nonblank main-agent text, and timer lateness is capped at 60 seconds.
- 🔴 Added separate turn-start timings for conversation append, init-frame sending, loop entry and request announcements (`CLAUDE_CODE_REMOTE` or `CLAUDE_CODE_EMIT_STARTUP_TIMING` for startup output).
- 🔴 Expanded main-session attachment diagnostics with setup, prompt-producer, ambient startup/wait and rendering times, producer and token-count totals, token-count and changed-file-read time, and file-size/hash-only counts. Slowest-producer timing now excludes work spent starting its group (`CLAUDE_CODE_REMOTE` or `CLAUDE_CODE_EMIT_STARTUP_TIMING` for startup output).
- 🔴 Changed hosted-worker MCP requests without an explicit proxy to bypass their connection pool after TLS settings change, using only CAs shared by the old and current settings and asking to reconnect if none remain (`CLAUDE_CODE_CCR_OWN_CONNECTION_POOL` or a remote switch, off by default).
- 🔴 Added structured `rate_limit_info` to 429 `model_requires_usage_credits` errors alongside their display text and error details.
- 🔴 Changed the bundled Bun runtime to another build that still reports 1.4.4, with about 25 MB more native code, new `.bun_aot`, `.bun_trim`, `.bun_features` and `.note.bun` sections, new `--aot` options and `BUN_AOT_*` variables for an ahead-of-time compile mode, and changes to the runtime's own built-in JavaScript.
- 🟡 (2.1.321) Added a hosted Linux Bash path that carries eligible commands containing `pkill`, `pgrep` or `pidwait` in `__CLAUDE_CC_COMMAND_TEXT` instead of shell arguments (remote switch, off by default). If shell startup removes the variable, the call fails with rerun advice and later calls use arguments again.
- 🟡 (2.1.316) Added `guardrail.input_tagging` to Claude apps gateway Bedrock upstreams to wrap selected message text and, optionally, the system prompt in Guardrails input tags; failover upstreams must agree, and preparation failures return 400 without sending the request.
- 🟡 (2.1.316) Added monthly shared group budgets to the Claude apps gateway through `pooled_budget`, alongside each member's own cap; requests get `429 billing_error` and a reset date once every matching pool is used up. Pool-spend recording warns when another charge arrives after a write has stalled for 30 seconds.
- 🟡 (2.1.316) Added opt-in `managed.policy_mode: layered` to combine matching group and email-domain policies in the Claude apps gateway, keeping earlier ordinary fields while merging deny/ask rules, hooks and blocked lists; helpers and path exclusions that could undo another policy's restrictions are rejected.
- 🟡 (2.1.316) Fixed the Claude apps gateway forwarding blocked nested `model` choices without checking role and operator model lists; checks now return 400 with the offending field's path and reject more than 100 nested model names, while skipping input, schema and example payloads.
- 🟡 (2.1.316) Fixed PowerShell hooks beginning with `using`, `param`, a type cast or an attribute to propagate exit code 2; declaration-style scripts matching a named-block pattern and `PermissionRequest` hooks stay exempt.
- 🟡 (2.1.316) Kept restored MCP off-switches in saved worker state while shutdown is pending, and stopped late shutdown callbacks from disabling a server that's since been re-enabled. Self-hosted runner diagnostics now distinguish a close timeout from a completed close that failed.
- 🟡 (2.1.290) Stopped scheduled startup and cached-connection retries to HTTPS MCP servers after a recognized proxy policy refusal in hosted sessions with `CLAUDE_CODE_MCP_NO_REDIAL_AFTER_POLICY_REFUSAL=1`.
- 🟡 (2.1.316) Added `pending_elicitation_requests` to SDK initialization and reinitialization replies so joining clients can recover unanswered MCP questions (remote switch, off by default); peer replies, live-only replies and ordinary awaited control results strip the list.
- 🟡 (2.1.316) Made Auto mode stop errors mark hosted sessions as failed and use the stop explanation as their status detail. Limited proxy-detail hiding to the generic no-safety-verdict warning, keeping other Auto mode stop explanations intact.
- 🟡 (2.1.316) Added `self` as an accepted read or write level in artifact capability rules, alongside `view`, `interact`, `admin` and `owner`.
- 🟡 (2.1.316) Made host-managed first-party gateways honor `CLAUDE_CODE_AUTO_MODE_SERVER=1` even when session-level experimental betas are off, provided organization policy and the existing eligibility checks allow it.
- 🟡 (2.1.319) Changed host-managed teammate and background launches to forward `CLAUDE_CODE_AUTO_MODE_SERVER` from before project environment changes when its current value enables server checks.
- 🟡 (2.1.278) Added a Desktop gateway fallback notice explaining that server checks aren't working, the built-in Auto mode check sends paid requests, and the session can continue, with a link to the Desktop gateway troubleshooting guide.
- 🟡 (2.1.316) Fixed credential scrubbing corrupting added environment values when a credential variable contains a boolean-like value such as `1` or `false`, and left values equal to the requested working directory unchanged.
- 🟡 (2.1.323) Added optional tracking and resend of missing assistant replies and tool results when eligible self-hosted cloud sessions resume (remote switch, off by default); sessions with `MessageDisplay` hooks skip recovery, and repeatedly failing old rows can step aside for new ones.
- 🟡 (2.1.316) Stopped self-hosted sessions from replaying follow-ups that saved history ties to a user-stopped turn after worker replacement; `CLAUDE_CODE_DISABLE_HELD_MESSAGE_RECOGNITION` or a remote kill switch (off by default) disables this check.
- 🟡 (2.1.316) Added inline `ReportFindings` reviews for eligible Claude Desktop and web remote sessions, excluding Cowork (remote switch, on by default). Most review templates also ask for one text line per finding with its outcome, so clients that hide the tool call can still show it.
- 🟡 (2.1.316) Fixed SDK `register_repo_root` with `reload_claude_md` leaving an outside directory's `CLAUDE.md` out of cached instructions when additional-directory instruction loading is enabled through `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD` or a host setting.
- 🟡 (2.1.316) Added hidden `claude auth login --gateway-json` for hosts such as VS Code to present gateway, certificate and account questions over JSON lines, plus `gatewaySignInOffered` in JSON auth status. The login mode requires redirected stdin and refuses calls from inside Claude Code.
- 🟡 (2.1.314) Made saved-login checks select `apiKeyHelper` defaults in platform order and honor per-platform `outputBehavior: "merge"`, with the chosen defaults overriding matching top-level settings and environment entries.
- 🟡 (2.1.314) Added destination-aware, cancellable waits for saved-login policy before self-hosted runner requests, cloud-session creation and task-file transfers, and checked the credential being sent on Files API operations instead of only the session's current login.
- 🟡 (2.1.265) Added resume code for marking unfinished `Agent` and legacy `Task` calls as interrupted by process shutdown, plus interruption rendering and suppression of the tool-batching reminder. Generating those results requires `CLAUDE_CODE_RESUME_SHOWS_CUT_OFF_CALLS` and a caller option that nothing in this build sets.
- 🟡 (2.1.316) Added `CLAUDE_CODE_DISABLE_LOOPED_CROSS_CERT_DROP` to keep looped cross-certificates; this and `CLAUDE_CODE_DISABLE_SELF_NAMED_CERT_DROP` follow protected connection-setting rules, refresh CA caches when changed, and are checked by runner proxies and HTTP hooks.

</details>

<details>
<summary>48 internal changes: telemetry, diagnostics and bookkeeping</summary>

- 🔴 Added Bash edit-diff wait telemetry with separate ordinary/shared counters and 10-, 60- and 300-second thresholds, plus a debug line showing whether commands share snapshots.
- 🔴 Reused system-prompt and tool-schema hashes between matching requests instead of recalculating them for prompt-cache diagnostics.
- 🔴 Added local text-and-image token estimates to tool-result clearing metadata and capped reported savings at a valid client estimate. Idle tracking also records how many tool results the latest post-idle reply cleared.
- 🔴 Gave hook timeout errors the distinct name `NoAnswerInTimeError` for callers and diagnostics.
- 🔴 Stopped text-file reads from resolving symlinks just to prepare a debug message when the current log level or mode would discard it.
- 🔴 Added live `pluginSteered`, `forRemoteExecution` and `spawnedByWorkflowRunId` getters to shared tool-context adapters, exposing the calling context's plugin steering, remote-execution mode and workflow-run ID.
- 🔴 Moved the startup profiler's initialization mark and first wall-clock and process-uptime samples to a different point in module loading, both with `CLAUDE_CODE_PROFILE_STARTUP` set and in the 0.5% automatic sample.
- 🔴 Delayed resolving the session directory for MCP file baselines until the call names a supported file tool and supplies a string path to a non-notebook file.
- 🔴 Skipped environment-expansion setup for empty or entirely literal MCP header maps, building the variable block lists only when a header contains `${` and reusing them afterward.
- 🔴 Added the original MCP connection failure code to wrapped not-connected errors as `mcpDialCode`, alongside the existing error-source classification; nothing in this build reads it by name.
- 🔴 Skipped MCP HTTP debug-message preparation when the log sink won't use it, including hosted sessions with `CLAUDE_CODE_SKIP_MCP_DEBUG_LOG_FILES=1` and debug-level logging off.
- 🔴 Let MCP listing callers choose size and Unicode-work limits, keeping the old defaults; tool-listing memo checks use a smaller estimated-size limit of about 1 MB.
- 🔴 Added the full MCP tool name to live and replayed background-task notifications' `toolsUsed` metadata and used-tool tracking; recording tool use follows a remote switch that's on by default.
- 🔴 Updated the bundled model catalog from version 3354 to 3444, moving its issue and expiry dates forward one day without changing its model rows.
- 🔴 Added a withdrawal indicator to permission-result callbacks for dialogs that weren't shown.
- 🔴 Added a prompt-text helper that leaves out recognized pasted-content blocks before applying the usual display cleanup.
- 🔴 Made parsed cloud errors keep an optional `message` field for later consumers.
- 🔴 Added support for starting remote internal-event catch-up after a supplied event ID, with caller-selected page parsing, compression handling, byte and event limits, and timestamp-length checks.
- 🔴 Added the submitting `clientPlatform` to latest-human-prompt tracking and a tool-state marker when that platform starts with `claude_code_`.
- 🔴 Changed remote upload trimming to keep request IDs on all assistant messages with a nonempty API message ID, rather than only API errors (replacement remote switch, off by default), and expanded the associated rejection diagnostics.
- 🔴 Removed the remote-worker policy that delayed the first idle-status report while inbound work was waiting.
- 🔴 Changed remote whole-turn upload holds to end after a matching assistant reply or tool result enters an outgoing batch, except for round-trip releases; the batch's release reason controls whether another hold is allowed.
- 🔴 Added earlier signed sender-address prefetch for eligible diskless first-party prompts (remote switch, off by default), retrying early misses. A separate remote switch, off by default, gives ordinary prefetch three event-loop passes to start.
- 🔴 Added a `refusedByProxy` marker to runner token-refresh errors when an HTTP 403 lacks both a valid Anthropic request ID and Cloudflare response markers; nothing in this build reads the marker by name.
- 🔴 Added old temporary copies of `remote-settings-consent.json` to age-based cleanup when their names end in `.tmp.` plus eight lowercase hexadecimal digits.
- 🔴 Expanded Bash telemetry with program names, how much of a test suite a command selects, and test results inferred from foreground or background output, including MCP-server background shells. Added backgrounding reasons and Datadog support for background completion events, plus execution fields for output previews, command notes, memory kills, output sizes and whether command text mentions saved-result or background-output folders.
- 🔴 Added Chrome permission diagnostics that distinguish tab-lookup timeouts, missing tab groups, non-JSON replies, tool errors and approval requests, and identify tab URLs without a plain host.
- 🔴 Added Desktop and local-agent diagnostics for whether an Auto mode server safety check was requested, returned a verdict, ended without one or wasn't requested, including the reason and server-switch setting, once per state and reason.
- 🔴 Expanded diagnostic event lists for artifact-document wakeups, Bash edits and notes, Remote Control, project uploads, plugin skills, permission checks, subagent effort defaults, runner session renewal, usage-limit text changes and idle CPU samples.
- 🔴 Changed first-party event logging to send records directly to its batch processor by default, using the OpenTelemetry SDK logger only when a log-record or global attribute-length or attribute-count limit is configured.
- 🔴 Added the number of blocking hook outcomes to hook-completion diagnostics, plus whether a stop hook was already active for `Stop` and `SubagentStop` events.
- 🔴 Added Linux hosted-worker idle CPU diagnostics that separate shells producing output, quiet shells, finished shells and leftover agent processes, with listening-socket checks and VM CPU totals (remote switch, off by default; requires `CLAUDE_CODE_DIAGNOSTICS_FILE` and excludes child and diskless sessions).
- 🔴 Added CLI CPU time to remote post-turn wait diagnostics when CPU readings are available; diskless sessions skip the measurement.
- 🔴 Changed query telemetry to use the first request built for dispatch, with a one-time fallback attempt to log request metadata if the query fails before that point.
- 🔴 Added WebFetch telemetry for long pages and continued reading in remote-agent sessions originating from claude.ai chat, when a tool-use ID is present and the result uses the regular page-reading path.
- 🔴 Added Bash repeat-work telemetry with elapsed time, intervening edits, possible writes, tests and lints, new user messages, earlier outcomes, whether earlier output is still in context and saved-output size. Clearing a conversation resets the comparisons.
- 🔴 Added `reads_own_output` telemetry to Read and Bash to classify references to the session's saved-result directory, background-output directory or both, when the session has a project and directory lookup succeeds.
- 🔴 Added `cli_trial_keys` to hosted remote startup reports, encoding enabled values of `CLAUDE_CODE_MCP_SKIP_UNCHANGED_RELIST`, `CLAUDE_CODE_CHROME_LATE_MCP_SERVER` and `CLAUDE_CODE_SKIP_MCP_DEBUG_LOG_FILES` as bits 1, 2 and 4.
- 🔴 Changed the feature-telemetry category for subagent wake failures handed to the main agent from `bad` to `sad`.
- 🔴 Added a one-in-32 sample of queue-to-init telemetry for main VS Code sessions, separating queue wait from turn-start-to-init time and skipping prompts queued before the previous turn ended.
- 🔴 Expanded Unicode normalization's combining-mark tables with additional Hebrew, extended and supplementary characters, including U+05C8–U+05C9 and U+1ADE–U+1ADF.
- 🔴 Added U+209D, U+209E and U+209F to the character look-alike tables for `w`, `y` and `z`, respectively.
- 🔴 Changed custom-CA and client-certificate HTTPS agents to request no per-host or total socket-count cap.
- 🔴 Saved and restored the tool names kept through `/clear` in worker metadata, and marked the reset list as already written for the current transcript; a different transcript or a changed tool list can produce tools-used metadata again.
- 🔴 Added a line-by-line parsing attempt for non-ASCII JSONL buffers, and sent non-ASCII transcript batches through the per-row path instead of the native batch fast path.
- 🔴 Stopped conversation hashing from treating null-content user or assistant transcript rows as message content, resetting its running hash state as it already did for missing content.
- 🔴 Restricted compact number formatting to JavaScript numbers, replacing other input types with `NaN` before formatting.
- 🟡 (2.1.271) Added account and credential checks for policy answers and safety-event telemetry, using salted hashes for unnamed credentials and credential versions for SSH tunnels; non-tunnel policy fetches now keep the identity captured for their request locally.

</details>

<details>
<summary>15 changes with no effect in this build: inactive code added, changed or removed</summary>

- 🔴 Added inactive scaffolding for sharing through the Remote Control dialog: empty dialog helpers that nothing calls, a `live-preview` notice whose condition is always false, port-row and ended-share state that is never filled, and a disabled list of sharing outcomes such as `already_shared`, `nothing_listens` and `session_refuses`.
- 🔴 Added a sign-in recheck beside the held live-preview session checks that nothing calls; it would answer `sign_in_changed`, `sign_in_unreadable`, `no_sign_in_check` or `sign_in_in_doubt`.
- 🔴 Added named failure reasons to the hook-matcher pattern reader (`legacy_grammar`, `look_behind`, `flags_group`, `back_reference`); its one caller discards them, so matchers it can't read are handled as before.
- 🔴 Added placeholders that have no effect: `renewSessionTokens` and `isThawing` callbacks passed to an empty self-hosted runner helper, an MCP resource check that always passes with a refusal branch behind it, fields nothing reads (`teamNameAskedAs`, `rowsNow`), and an error renderer for a tool slot that is always empty.
- 🔴 Added more code that can't take effect: an early exit for diskless launches in the cloud-browser session check that reads a value nothing sets, one more always-empty slot in the command lists, a third cleanup call on a hook the self-hosted runner never sets, and a disabled, blank form of the skill prompt's "Yes, and don't ask again" option with a check refusing it; the condition that would disable the option also removes it.
- 🔴 Replaced unused inline-text hooks on the tool-result store: the `keepsTextInline`, `takeInlineTextChars` and `noteInlineTextCut` lookups are gone, and an optional `takesTextInline` check with a scan for runs of 5,000 UTF-16 code units without ASCII whitespace takes their place. No store in either build has these methods, so oversized results are saved or truncated as before.
- 🔴 Changed the held live-preview session checks, which nothing calls: they gained a test for a replaced fixed sign-in credential, whose callback is never supplied, and the sign-in failure debug message now says "owner check failed" instead of "owner check for a start failed".
- 🔴 Added two preview-tunnel options that no caller sets: `refusesOtherSessions`, which would make a tunnel reject other-session requests and refreshed tokens and force TLS certificate checks, and `onUnlistedPortProbed`, which would tell a preview host when its own session asks for the status of an unlisted port from 1024 through 65535. The only trace is an `own_refusal` field, always false, in the tunnel's permanent-failure telemetry.
- 🔴 Added a `dialogKey` lookup so a local JSX command could use a built-in dialog loader under a name other than its own; no command sets it, so loaders are still chosen by command name.
- 🔴 Added memory-extraction hooks that nothing uses: an `onPassSkipped` callback that is never set, `smaller_pass` settings in `CLAUDE_CODE_POST_TURN_MEMORY_CONFIG` that are parsed and never read, comparison-trial hooks on a slot that is never filled, and an option to skip permission-denial telemetry that only those trials would use.
- 🔴 Added an emergency-stop check where laptop-memory context is built and withdrawn; the memory backend it asks is never set in this build, so it never fires.
- 🔴 Added an exported `loadDefaultMcpClientModule` helper for loading the default MCP v2 client; nothing calls it.
- 🔴 Changed the wait for lazily loaded plugin agent definitions so cancelled agent launches, resumes and forked skills would stop waiting, except on a turn interrupt; no agent definition in either build is loaded lazily, so nothing waits.
- 🔴 Added Remote Control upload counters that nothing reads: whole-turn upload holds, releases and held rows, the first undelivered count at close for each upload path, and events handed to an upload or telemetry queue after it closes. The two methods that would report them, `uploadHoldInAll` and `uploadLaneCounts`, have no caller.
- 🔴 Added bookkeeping that nothing reads: the first dispatch time in the turn timer's saved snapshot, which no code takes, and first-wait, unsigned-call and completed-ask counters for signed sender-address lookups.

</details>

---

### 2.1.314 ◆◇◇◇

- 🔴 Added `/send-to-project` for local claude.ai sessions with organization permission, live Remote Control, typed input or headless stdin and confirmation (remote switch, off by default); `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` disables it.
- 🔴 Added a server-only auto mode option for Anthropic API sessions, some settings requiring a direct connection (remote switch, off by default); actions without a verdict cannot run, and some settings skip local subagent-report review.
- 🔴 Added a cloud-session stop for sustained use of at least 90% of all sandbox CPUs, ending shell commands and child processes while keeping files; inactive unless remote configuration sets a duration.
- 🔴 Added Design Sync checks for local-file uploads that reject protected files, hard links, files requiring separate read approval, and files replaced after validation.
- 🔴 Added opt-in `stream-json` merging for consecutive text updates with `CLAUDE_CODE_STREAM_EVENT_HOLD_MS` (1–2,000 milliseconds) and line-size limits via `CLAUDE_CODE_STREAM_EVENT_MAX_LINE_BYTES`.
- 🔴 Added an instruction not to recreate existing games, defaulting to Opus 5.5 outside Cowork and local-agent sessions; `CLAUDE_CODE_MODEL_CAPABILITIES` and capability settings can override it.
- 🟡 (2.1.316) Added an Opus 4.8 fast-mode deprecation warning with a remotely configurable removal date, defaulting to 2026-11-30; fast mode itself is unchanged.
- 🟢 Made `permissions.blockWritesOutsideWorkingDirectories` block Claude Code settings edits by `Write`, `Edit` and `NotebookEdit` inside working directories; some session files and shell commands are exempt, and no allow rule overrides it.
- 🟢 Restricted Claude apps gateway upstreams with `groups` to developers with matching identity-provider claims; developers without group claims can use only upstreams without `groups`.
- 🟢 Added Bedrock `guardrail_trace` logging of triggered policies and filters, excluding matched text but potentially including request IDs, subjects and emails; the raw trace is removed from responses.
- 🟢 Added lower-effort retries for organization effort caps without changing user settings, excluding auto mode safety checks and requests naming a flagged-response fallback model (remote kill switch, off by default).
- 🟢 Added signed-out retries for HTTP and SSE MCP servers when stored sign-in is rejected and no refresh token is saved (remote switch, on by default).
- 🟢 Changed `/upgrade` for App Store and Google Play plans to direct users to the mobile app, then `/login` to activate the updated plan, instead of opening browser sign-in.
- 🟢 Renamed the `/permissions` review tab to Recently blocked, clarified that approval adds no rule, and blocked approvals for missing, blank, unreadable or truncated inputs and calls too tall to display.
- 🟢 Fixed terminal hyperlinks staying enabled when `FORCE_HYPERLINK` is set to `false`, `no` or `off`.
- 🟢 Made sandboxed headless `--resume` and `--continue` rebuild protections when entering a worktree, refusing to resume if rebuilding fails (remote switch, on by default).

---

### 2.1.312 ◆◇◇◇

- 🔴 Added server-provided Cyber Verification Program status to cyber-safeguard refusals, only on Anthropic's API, Claude Platform on AWS or Google Cloud, and Claude apps gateways.
- 🔴 Blocked `CLAUDE_CODE_OAUTH_TOKEN`, `CLAUDE_CODE_OAUTH_REFRESH_TOKEN`, `CLAUDE_CODE_OAUTH_SCOPES` and `CLAUDE_CODE_OAUTH_CLIENT_ID` in project settings, preserving shell and user settings.
- 🔴 Stopped several first-party requests from following cross-origin redirects, and OAuth token, refresh, revoke and account requests from following any redirects.
- 🔴 Changed the model-training notice to request opt-in only when unset; `grove_locked_by_claude_science` limits it to OFF and locks the setting in `/privacy-settings`.
- 🔴 Added session activity, approval status and a default 60-minute idle timeout to `claude remote-control` folder hosting, with two presses of `x` within 5 seconds to disconnect (remote switches, off by default).
- 🔴 Stopped Bash read-only checks and non-exact allow rules approving words with `*`, `?` or `[` or nonliteral assignments after `builtin`, `command` or `noglob` (remote switch, on by default); extra-check errors now need approval.
- 🟢 Made `Write` require a full read in one call before replacing nested `CLAUDE.md`, path-matched rules or `@`-included files truncated in context (remote switch, on by default).
- 🟢 Added Claude apps gateway `oidc.user_id_claim` for custom user-ID claims (default `sub`), including nested paths; `sub` remains required and invalid claims fail with `USER_ID_CLAIM_MISSING`.
- 🟢 Made fullscreen queued-message send-now hints clickable with mouse support, including with all keys rebound but not unbound; `CLAUDE_CODE_DISABLE_MOUSE` and `CLAUDE_CODE_DISABLE_MOUSE_CLICKS` disable clicks.
- 🟢 Changed unknown model IDs, including application inference profile ARNs unless mapped by `modelOverrides`, to use shorter system prompts on Bedrock, Vertex, Foundry and Mantle; `CLAUDE_CODE_SIMPLE_SYSTEM_PROMPT=0` forces full prompts.
- 🟢 Added refusal fallback for unknown model IDs, using configured Opus when available on cloud providers; disable it with `CLAUDE_CODE_DISABLE_REFUSAL_FALLBACK` or per-model `no_refusal_fallback` via `CLAUDE_CODE_MODEL_CAPABILITIES`.
- 🟢 Changed CLI-started cloud sessions to request titles and branch names from an Anthropic endpoint, except under HIPAA or zero-data-retention restrictions or without an organization-policy verdict.
- 🟢 Limited prompt and agent hooks to 24 concurrent per session, plus 12 for hooks past event timeouts, keeping 12 per event, with a one-second `timeout` minimum; `PermissionRequest` agent hooks are exempt from concurrency limits.
- 🟢 Changed subagents to honor `autoCompactWindow` when it is smaller than `CLAUDE_CODE_AUTO_COMPACT_WINDOW`; larger values still do not override the environment.
- 🟢 Changed OpenTelemetry `mcp_server.name` to report real names for fixed-URL MCP servers from verified official Anthropic marketplace plugins, excluding variable URLs and MCPB servers.
- 🟢 Changed `claude_code.safety_event` and its matching first-party analytics event to off by default; their remote switch can still enable them.

---

### 2.1.311 ◆◇◇◇

- 🔴 Added shell-only `CLAUDE_CODE_EVEN_MORE_DANGEROUSLY_SKIP_PERMISSIONS` to skip bypass-mode `rm` prompts until Bash, Edit or Read deny or ask rules, outside-directory read blocks or approval hooks or tools apply; Claude-run CLIs are excluded.
- 🔴 Added managed-settings-only `extraKnownMarketplaces[…].pluginDigests` to pin organization-tier hooks of plugins enabled by managed settings in `prependPlugins` or `appendPlugins` to matching plugin-folder SHA-256 digests.
- 🔴 Restricted repository and other plugins' environment variables for managed authentication and OpenTelemetry helpers and organization-plugin MCP and LSP servers; trusted-repository identifiers still reach `apiKeyHelper` and cloud helpers.
- 🔴 Added working-directory rechecks before tool calls, including file tools, rejecting calls if the session's project directory is deleted or resolves to a different path.
- 🔴 Allowed self-hosted runners behind an agent proxy to use Anthropic-managed git with `--exec-path` or command hooks, with Claude Code 2.1.311 or newer.
- 🔴 Made opt-in MCP cross-app sign-in (`CLAUDE_CODE_ENABLE_XAA=1`) stop sign-in and silent refresh when the issuing authorization server is no longer advertised; re-add the server with its client secret for expected moves.
- 🔴 Updated `/claude-api` docs for Go `/v2`, Java SDK 3.2.0, PHP SDK 1.0.0 or later for Managed Agents, automatic `pause_turn` resumption in newer Python and TypeScript runners, and API-key precedence over OAuth.
- 🔴 Added `Shift+Up` and `Shift+Down` to change effort at the prompt, stopping below `max` (off by default); `/effort max` is still needed for `max`.
- 🟢 Added `--hold-idle-file` (`SELF_HOSTED_RUNNER_HOLD_IDLE_FILE`) to hold idle `claude self-hosted-runner` sessions using a fresh regular file, excluding detected network paths and uncheckable links; `--max-hold-min` defaults to 24 hours.
- 🟢 Added compaction and retry when standard-context requests over 200K tokens are rejected for requiring usage credits, if auto-compaction is on and succeeds (switch on by default; cloud switch off by default).
- 🟢 Improved `Write` and `Edit` result diffs for replacements with no shared lines or unchanged outer lines; harder cases still have a five-second limit.
- 🟢 Added support in the `@` file picker for Git-ignored files re-included by `.ignore` or `.rgignore`.
- 🟢 Stopped deleting a subagent's clean isolated worktree while another task still uses it as its working directory.
- 🟢 Improved `/mcp` sign-in status: clearing authentication now reconnects remote servers immediately, and connected tools alone no longer count as signed in.
- 🟢 Improved `/rewind` warnings: conversation restore removes the selected and later messages, while code restore overwrites whole files but leaves Bash changes to other files untouched.
- 🟢 Added a follow-up for missing `<summary>` blocks in `/compact` and overlong-request compaction (switch on by default), excluding some automatic compactions, agent-team teammates and rewind summaries.
- 🟢 Improved dropped-connection errors for Anthropic's API to name the proxy or `ANTHROPIC_BASE_URL` gateway, or mention VPNs, firewalls and security software; cloud sessions are unchanged.
- 🟢 Changed `--output-format json` and `stream-json` to escape DEL, C1 and Unicode format characters, plus U+2028 and U+2029 in `json` (switch on by default), without changing parsed text.

---

### 2.1.310 ◆◇◇◇

- 🔴 Changed `/teleport` to allow confirmed cloud continuation with local changes or unpushed commits, using the branch as last pushed or the default if absent remotely; repositories rooted at the home folder or without `origin` are refused.
- 🔴 Stopped enforcing sandbox deny paths on other computers, with `/status` and `claude doctor` warnings; file-tool denies still apply, and write and edit denies from managed policy, user settings or `--settings` are generally kept.
- 🔴 Updated `/claude-api` model retirements and Anthropic-managed Google Cloud guidance; added a retiring-model scan behind a remote switch and experiment (off by default).
- 🔴 Fixed damaged blocked entries discarding all of `NODE_EXTRA_CA_CERTS`, and allowed two roots outside Anthropic, Claude, cloud-provider API and telemetry domains, except with Anthropic credentials or certain proxies.
- 🔴 Added support for Anthropic-managed git sessions with supplied git setup through self-hosted agent proxies (`--agent-proxy-url` or relay mode), except on Windows or runners using `--exec-path` or command hooks.
- 🔴 Stopped hooks attributed to the organization from inheriting repository settings' `env`, and added debug logs naming withheld variables.
- 🔴 Restricted shell auto-approval for path-like or command-substituted environment values and six more zsh builtins, and blocked `Read` on `/dev/pts/N`, `/dev/ptmx` and `/dev/tty*`.
- 🔴 Restricted more `git init` options, `-c` settings, `GIT_TEMPLATE_DIR` and `ARGV0` in worktree-isolated sessions and subagents, behind a remote switch (on by default).
- 🟡 (2.1.311) Added opt-in `CLAUDE_CODE_MODEL_CATALOG` support for Bedrock, Vertex, Foundry and Bedrock Mantle; remote catalogs stay disabled with nonessential traffic off, and project settings can only disable it.
- 🟢 Made `Monitor`, plugin monitors, `Monitor` scheduling guidance and the block on long foreground sleeps independent of remote flags; `Monitor` still requires a non-Windows system or Git Bash.
- 🟢 Added retries without `effort-2025-11-24` and `output_config.effort` on 400 responses rejecting the beta, and expanded fallback for rejected mid-conversation system messages.
- 🟢 Fixed background cleanup considering recent agent worktrees for removal when a very large `cleanupPeriodDays` value creates an invalid cutoff date.
- 🟢 Added symlink destinations to file read permission prompts outside remote workspaces, with a warning when a target cannot be resolved.
- 🟢 Fixed quoted-only `PreToolUse` and `PermissionRequest` command hooks allowing calls on Windows without Git Bash, behind a remote switch (on by default), except hooks with `shell`, `args`, `async` or `"onFailure": "continue"`.
- 🟢 Changed auto mode's first outside-directory read prompt to clarify that shell commands it cannot check require approval when the sandbox does not restrict those reads.
- 🟢 Added mod `Image` support in tmux with kitty 0.28+, Ghostty or iTerm2 3.5+, requiring `allow-passthrough` and `focus-events`; iTerm2 also needs passthrough `on`, fullscreen rendering and a window no larger than the terminal.
- 🟢 Improved Linux and WSL sandbox errors for AppArmor blocking bubblewrap, and made `sandbox.failIfUnavailable` refuse startup when that restriction prevents sandboxing.
- 🟢 Improved MCP timeout errors in print-mode and SDK sessions when a server question goes unanswered, telling Claude not to retry that turn; a repeated-question cap is off by default.

---

### 2.1.308 ◆◆◆◆

- 🔴 Changed only the bundled Bun runtime, to another build that still reports version 1.4.4, with about 240 KB more machine code and changes to Bun's own built-in JavaScript; Claude Code's JavaScript and its other embedded files are identical to 2.1.307 apart from the version number, build time and commit hash.

---

### 2.1.306 ◆◆◆◆

- 🔴 Made the retry that drops the thinking display-updates beta header after an HTTP 400 or 422 go ahead, logging the error, when checking for a direct Anthropic API connection fails, where it used to give up.
- 🟢 Made Claude Code read a cloud session's handed-off login files (`.oauth_token`, `.api_key`, `.session_ingress_token`) even without `CLAUDE_CODE_REMOTE`, so a `claude -p` launched in the session without its environment variables is no longer "Not logged in".
- 🟢 Stopped an unrelated HTTP 400 or 422 from Anthropic's API making Claude Code resend the request with every tool loaded, which with many MCP tools ended in "prompt is too long" and a needless compaction; on direct connections, except sessions signed in through an Anthropic profile or workload identity federation with no API key, only an error that mentions `defer_loading` triggers that resend now.

---

### 2.1.303 ◆◆◆◆

- 🔴 Stopped trusting eMudhra's four emSign root certificates (emSign Root CA - G1 and - C1, emSign ECC Root CA - G3 and - C3) for Claude Code's own HTTPS connections: they, and any certificate with the same public key, are now dropped from the runtime's bundled roots, the system store and the `NODE_EXTRA_CA_CERTS` file, so a server or proxy whose chain ends at one of them fails certificate checks. When the `NODE_EXTRA_CA_CERTS` file holds one in a form that can't be parsed, Claude Code ignores that block, or the whole file when the block can't be cut out, and says so once on stderr (in the debug log instead when the interactive screen is already up).
- 🔴 Made Claude Code trust its bundled root certificates (without the four above) when only the system certificate store is selected and every certificate it returns has expired; such a setup used to leave the runtime's default store in charge, or with `NODE_EXTRA_CA_CERTS` set, only that file's certificates.

<details>
<summary>2 changes with no effect in this build</summary>

- 🔴 Changed what happens when only the system store is selected (`CLAUDE_CODE_CERT_STORE=system` or `--use-system-ca`) and the runtime has no system-certificate API: the bundled roots, minus the denied ones, are now used instead of the runtime's own default. This build's Bun runtime always provides that API, so the branch never runs.
- 🔴 Added the error "No root certificates are left to trust", for when every root built into the runtime is on the deny list; this runtime bundles many roots that aren't on the four-entry list, so it can't appear.

</details>

---

### 2.1.296 ◆◇◇◇

- 🔴 Added SDK `set_model` support for replacing only the custom system prompt via `only_if_model_is_current: true`, requiring a nonempty prompt and an explicit matching model, excluding hybrid mode and Remote Control.
- 🔴 Fixed strict-host permission replies allowing tool calls when their requested deny or ask restrictions could not be kept.
- 🔴 Added experimental `Agent` effort values `lower` and `higher` for relative adjustments (off by default), excluding `max`, forks, remote agents and teammates, subject to existing model and effort overrides.
- 🔴 Added experimental MCP file-argument uploads for supported claude.ai connectors and matching host-held HTTP servers (off by default), requiring authorization for local files.
- 🔴 Added an opt-in host policy restricting `SendMessage` to plain-text messages between agents in the same session.
- 🟢 Improved secret redaction for plain and escaped assignments when an empty value precedes a later value, including JSON in shell strings.
- 🟢 Added `allow_large` to `Read` for larger text files in local sessions with spare context, without increasing remote-call or other-machine limits.
- 🟢 Added an `Edit` encoding check that refuses UTF-8 rewrites of files containing invalid UTF-8 bytes, rather than replacing undecodable characters.
- 🟢 Added subagent `autoCompactWindow` in frontmatter and `--agents` to lower local windows to integer token counts of 100,000–1,000,000; `CLAUDE_CODE_AUTO_COMPACT_WINDOW` takes precedence and built-in forks keep the parent's window.
- 🟢 Added `CLAUDE_CODE_WORKFLOW_SUBAGENT_MODEL` to override models for Workflow agents only; empty values and `inherit` leave selection unchanged, and forced-model policy still applies.
- 🟢 Added `CLAUDE_CODE_OVERLOADED_RETRY_MAX_DELAY_MS` to set eligible overload retries' backoff cap to 32,000-64,000 ms; jitter and `Retry-After` can extend delays.
- 🟢 Changed Sonnet 5.5 cache-read cost estimates from $0.20 to $0.10 per million tokens; account billing is unaffected.
- 🟢 Improved plugin diagnostics to identify `hooks.json` hooks or hook modules skipped for duplicate plugin names, and explain which enabled copy takes precedence.
- 🟢 Added a `/code-review` foreground-fork formatter for numbered findings with file and line locations or a no-findings message; raw text and JSON modes bypass it.

---

### 2.1.294 ◆◆◆◆

- 🔴 Told the model that judges `prompt` and `agent` hooks to ignore instructions planted in what it judges: the event's JSON and anything it reads while judging are now only things to check, and any rule, exception or instruction inside them is to be disregarded, even one that claims to come from the user.
- 🟢 Fixed `prompt` and `agent` hooks written as rules (such as "block any `rm -rf`") letting through what they should block: the judging model is now told that `ok` true lets the action go ahead and false blocks it, and to apply a rule and answer with its outcome. Prompt hooks on events other than Stop and SubagentStop are now framed as deciding whether the action may go ahead instead of whether a condition is met, and agent hooks are asked for a reason every time.
- 🟢 Added the same guidance to the stop-condition prompt of Stop and SubagentStop `prompt` hooks, so a hook written as an instruction ("keep going until the tests pass") is judged as a rule, and Claude is less often let stop while the hook says it should keep working.

---

### 2.1.291 ◆◆◆◆

- 🔴 Took back 2.1.290's change that kept timers, input and drawing running while a large session loads: a transcript over 5 MB is parsed in one go again, so the interface waits until it's loaded, and a session's subagent transcripts load all at once instead of four at a time.
- 🔴 Stopped sessions that run as remote workers (cloud sessions, self-hosted runner sessions and sessions a Remote Control host starts) from reporting the running cost and token totals to Anthropic's session server during a turn, which 2.1.290 did as often as every 15 seconds whenever the cost had changed; mid-turn reports now need a remote setting that's off by default and sets their interval (15 seconds at least), while the reports after each turn and when the session goes idle are unchanged.
- 🟢 Stopped transcript rewrites and loads from waiting for each other, as they had since 2.1.288: removing a message, the compaction rewrite, moving the transcript after a directory change, and writing a session's or subagent's transcript from the server no longer wait for loads of the same file to finish or stall for 5 seconds, and loads no longer wait for rewrites. 2.1.290's restart of a load that a rewrite overtook (up to three passes, then reporting the transcript unreadable) went with it.

---

### 2.1.289 ◆◆◆◆

- 🔴 Made Bash permission checks treat more `find` commands as unsafe: `-rm` (which the built-in `bfs` that runs as `find` reads as `-delete`), the option spellings `-sf`, `-xf` and `-df` that different versions of `find` read differently, and an action hidden behind `-D` or `-f` (whose following word used to go unchecked) now make the command ask instead of passing as read-only, under sandbox auto-allow or through a `Bash(find:*)` allow rule. The read-only check also splits a `find` command's words only at spaces and tabs now; `-rm` in the main checks sits behind a remote switch that's on by default.
- 🔴 Stopped sandbox auto-allow from running `test`, `find`, `jobs` and `set` commands unprompted when an argument holds `<` or `>` or ends in a backslash, `test` commands with `$` in certain places, and `find` commands with a quoted option that takes a value (such as `'-name'`) or an unquoted glob in an option's value; they go through the usual permission check instead.
- 🔴 Made a deny rule on the whole `Read` tool in settings also stop the files Claude Code reads by itself (@-mentioned files and folders, IDE selections and diffs, changed-file updates and other attachments), which only rules with a path stopped before; rules given on the command line don't count for this.
- 🔴 Made the values a plugin's hooks return or answer with, and the arguments they pass to `next()` or to `$` methods, get copied as plain data when they cross out of the plugin's environment, so later changes inside the plugin can't reach the engine; a value that can't be copied is refused (for example "the answer is not plain data"). A remote switch that's on by default controls this.
- 🔴 Added a `hook` field to the core permission decision a plugin's `tool.check` hook sees, naming the hook that decided it (`PreToolUse` when a hook's ask set the floor).
- 🔴 Made `$.ui.selection()` answer in sessions the desktop app hosts: the session asks the attached clients that answer selection reads for what's selected in the app's page and takes the first selection that isn't empty.
- 🔴 Added a "0 Dismiss" choice to the feedback cards of the built-in `cc-plugin-you-should-know` plugin (the ones offering "That was helpful"), which its explanation card already had; the plugin is off unless you enable it.
- 🔴 Gave plugin toasts their own error handling: a toast that throws while drawn now hides the toast stack until the toasts change and tells the plugin's author ("ui.toast threw while drawn"), where the throw used to reach the interface's outer error handling.
- 🟢 Made a deny rule that matches a nested part of a compound shell command hold against a user-installed plugin's approval where the built-in security default plugin runs (machines with managed settings, Team and Enterprise organizations): that plugin only holds deny decisions that name their rule, and the core decision plugins' `tool.check` hooks see now names a matching deny or ask rule at any depth of the command.
- 🟢 Gave syntax highlighting a work budget tied to the text's length and a limit of 32 nesting levels, after which a block is drawn as plain text, so a short code block packed with unclosed `<script>` tags or nested `${` substitutions no longer hangs the terminal.
- 🟢 Put the same highlighting budget into the highlight.js bundle that published artifact pages carry, so such code blocks no longer hang or crash a reader's browser tab.
- 🟢 Made `Read` deny rules catch files and folders reached through symlinks when Claude Code reads them on its own (@-mentions, changed files, IDE selections and diffs): every path the links lead through is checked, and a mention that can't be checked in time isn't attached, with Claude told it couldn't be examined and the transcript showing "Referenced … (not read: could not be examined)".
- 🟢 Made Bash deny and ask rules match the command itself when it follows an environment-variable assignment whose value expands, such as `TZ="$HOME" rm -rf build`, which they used to miss, with sandbox auto-allow and without it.
- 🟢 Made sandbox auto-allow check deny and ask rules against the command itself also when the line holds one command preceded by a bare variable assignment.
- 🟢 Counted the sign-in tools of an MCP server your organization manages as the organization's, so a plugin you installed can no longer rewrite their descriptions.
- 🟢 Stopped `claude auth status` from waiting to send its usage event and flush analytics before it exits, taking back the 2.1.288 change that may have made sign-outs more frequent in VS Code.
- 🟢 Let plugins' `agent.spawn` hooks see and change teammate spawns too (deny them, or rewrite their prompt, description, model or type, though not into a spawn a permission rule wouldn't allow or a type that can't be a teammate), gave each agent a single id across hook events plus a `teammateId` for teammates, made `$.agent.list()` report `idle` and `waiting`, and reworded the deny reason a hook gets when its `subagentType` names no agent the call can use (its ending "a fork names none" became "none is available").
- 🟢 Made installed plugins' hooks modules load in the first session after an upgrade: when an earlier session saved the switch that turns them on as off, loading now waits briefly for a fresh answer, as it already did with no saved answer, and loads them late if the switch answers on after loading went ahead.
- 🟢 Made `claude plugin list`, `plugin eval` and `plugin update` read a plugin installed from a local folder marketplace from that folder, where sessions already load it, instead of a stale copy (`claude plugin list` shows where each plugin is read from, and `/reload-plugins` says what it re-read from a folder), and made hot reload follow a symlinked `--plugin-dir`.
- 🟢 Made `claude plugin validate`, given a folder that holds both a marketplace manifest and a `plugin.json`, also check the `plugin.json` and the folder's skills, agents and commands, where it checked only the marketplace; the `plugin.json` is held to the rules for an Anthropic marketplace's own plugin when such a marketplace lists the folder itself, and it's listed in the results only when it has something to report.
- 🟢 Neutralized stray escape and C1 control characters in drawn text even when the text has no tab, which stops such text, and short tabbed text with Windows line endings, from writing over the rows beneath it.
- 🟢 Drew a plugin's `Box` with a border style Claude Code doesn't know without a border, telling the plugin's author, where it used to freeze Claude Code or force it to quit at launch.
- 🟢 Let terminal plugin links use destinations such as `file:` paths, uppercase hosts and an `@` in the path, where one such link used to leave the whole pane blank; on other surfaces, a destination that fails their address checks is drawn as plain text instead of hiding the pane.
- 🟢 Made Claude Code draw its own row when a value a plugin's `ui.render` hook wrote makes the row throw while drawn, where the session used to end with "unrecoverable interface error".
- 🟢 Kept a failing plugin `Client` from taking down the rest of what its plugin drew: it fails on its own and the plugin hears about it through a new `ui.fault` event, which SDK hosts such as the desktop app can raise too with a new `ui_client_fault` request.
- 🟢 Let a plugin `Client` region recover after the terminal throws while drawing it, instead of staying failed for the rest of the session.
- 🟢 Stopped rejected promises in plugin `Client` code from ending supervised and background sessions: a rejected promise a handler returns fails that Client, and one it doesn't return is reported without failing the Client.
- 🟢 Stopped a plugin region with no height that keeps growing from ending the session with an interface error: a `Client` region that changes size at each of 8 measurings in one frame, or keeps the screen measuring for 20 passes, is drawn as last measured, and the plugin's author is told to give it a height and a width.
- 🟢 Kept the close mark and `[-]` one column in from the terminal's edge and reserved their columns, so right-aligned content in a plugin's pane or band no longer draws underneath them.
- 🟢 Stopped a plugin band whose drawing fails from briefly making the cards below it move out of its way.
- 🟢 Stopped plugin rows above the prompt from showing an out-of-date row in fullscreen while the Background tasks dialog is open.
- 🟢 Stopped a failed plugin component from showing `Error` or an empty reason when its failure carried no message.
- 🟢 Made the notice a plugin's author gets when its band or pane fails to draw name the plugin and say what became of it: nothing was drawn for a band, and the pane was closed.
- 🟢 Made large files open faster in a plugin's code pane: the highlighted view takes the pane's width from the start instead of being laid out again once the width is known.

<details>
<summary>6 internal changes</summary>

- 🔴 Kept the artifact-page syntax highlighter's generated helper variables inside a function instead of the page's global variables; `hljs` stays available to the page.
- 🔴 Added telemetry for plugin rows that throw while drawn, plugin toast stacks that fail, selection reads across clients, @-mentions refused by a symlinked `Read` deny, and errors caught by the plugin interface's error boundaries.
- 🔴 Added a log line for a plugin hook whose rewrite was only partly applied ("the rest of its rewrite went on"), written as "hook" where skipped hooks say "hook skipped"; such a report doesn't count as a failed hook.
- 🔴 Updated the plugin-authoring reference and the plugin API type declarations that ship with Claude Code to describe this release's plugin changes (teammates in `agent.spawn`, agent states, `ui.fault`, folder-read plugins, selection reads in the desktop app).
- 🔴 Changed how highlighted code wraps colour resets in tokens that already hold one, without doubling the inner reset codes as before; it looks the same on screen.
- 🔴 Made a plugin `Client` in the terminal ignore props in a late reply to its `surface.post()` once the Client has been mounted again.

</details>

<details>
<summary>1 change with no effect in this build</summary>

- 🔴 Changed the names that two empty checks in remote session setup refer to; they sit behind a constant false condition, so nothing runs them.

</details>

---

### 2.1.278 ◆◆◆◆

- 🔴 Made auto mode let the local classifier decide a tool call the server-side classifier has no usable verdict for (the server reports its classifier unavailable for a reason other than being turned off, leaves the call out of its results, or reports the call unavailable for a reason other than a cut-off response), wherever that classifier runs with local fallback: Bedrock, Vertex, Foundry, Mantle, Claude Platform on AWS or Google Cloud, and Anthropic API connections through a proxy. Bedrock, Vertex, Foundry and Mantle sessions that had turned the server-side classifier on blocked such calls as classifier-unavailable before.
- 🔴 Extended the silent retry after a refusal to the default Sonnet model, for refusals the API puts in its `cyber` or `frontier_llm` category, when the account's client data turns the retry on for that model and no refusal fallback has happened yet in the session; before, only the default Opus model was retried this way.
- 🔴 Passed `CLAUDE_CODE_AUTO_MODE_SERVER` and `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` on to background sessions and to agent team teammates started in their own panes, so they use the launching session's auto mode classifier setting and, when it has experimental betas turned off, leave them off too.
- 🔴 Changed what Claude is told when auto mode on a direct API connection denies an action because the session no longer holds the server-side classifier's result for the response that asked for it: it's now told to issue the action again once, where it was told a retry wouldn't change anything.
- 🟢 Changed auto mode on Bedrock, Vertex, Foundry and Mantle to use the provider's server-side classifier by default, falling back to the local classifier, where it needed `CLAUDE_CODE_AUTO_MODE_SERVER=1` before. Setting that variable to a non-empty value other than `1`, `true`, `yes` or `on` (such as `0`) turns it off, and while it's unset, so does `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` unless a host application manages the provider (`CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST`).
- 🟢 Moved Claude Platform sessions on AWS and on Google Cloud, and Anthropic API sessions whose `ANTHROPIC_BASE_URL` isn't `api.anthropic.com` or that connect through `ANTHROPIC_UNIX_SOCKET` (outside `ssh-remote` sessions), onto that same server-side classifier with local fallback, which `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` always turns off for them. Claude Platform sessions used only the local classifier before, and those API sessions got the server-side classifier only behind a remote switch that's off by default, with no fallback (a proxy that refused its beta header left auto mode denying every tool call it had to check until `/clear` or `/compact`).
- 🟢 Made auto mode on a direct API connection that uses the server-side classifier (behind a remote switch that's off by default) switch to the local classifier for the rest of the session when a response comes back without that classifier's result or the API says it doesn't run that classifier for the session (behind a second remote switch that's on by default); each such tool call was blocked before, with a note to Claude that retrying wouldn't help.
- 🟢 Added a notice when auto mode falls back to billed classifier requests: it says the session isn't eligible for the change that stops charging for classifier requests, names the gateway in the way when it can (a base URL host that isn't Anthropic's or a cloud provider's, or a LiteLLM, Helicone, Portkey, Cloudflare AI Gateway, Kong, Braintrust, Bifrost or Databricks gateway it has detected) and links to the classifier billing page. Enter keeps auto mode going and Escape denies the pending tool call and stops the turn; it skips Pro, Max and Team subscribers, stays away for 24 hours once acknowledged with a gateway named, sits behind a remote switch that's on by default, and `-p` runs print it once instead (to stderr, or as a warning-level system message with `--output-format stream-json --verbose`). While it's open, the session raises a needs-input notification; background sessions, agent team teammates, Remote Control and SDK hosts with their own dialogs get a log line instead, and after Enter the session isn't asked again.
- 🟢 Added an `Auto mode server` row to `/status` that reads Enabled when this session's permission mode uses the server-side classifier and its beta header hasn't been refused, and Disabled otherwise.

<details>
<summary>1 internal change</summary>

- 🔴 Added telemetry for the classifier billing notice (`tengu_auto_mode_server_fallback_notice`, with whether a gateway was found and the provider, and `tengu_auto_mode_server_fallback_notice_closed`, with the choice) and for switching to the local classifier (`tengu_server_classifier_no_result_latched`), plus debug-log lines when a call or a session falls back.

</details>

---

### 2.1.276 ◆◆◆◆

- 🟢 Stopped first-party sessions whose `ANTHROPIC_BASE_URL` points away from `api.anthropic.com` from choosing a standby advisor model when the advisor tool is on (through `CLAUDE_CODE_ENABLE_EXPERIMENTAL_ADVISOR_TOOL` or a remote switch that's off by default); since 2.1.275 that made every request through such a proxy or gateway fail with an HTTP 400 about the `advisor_20260301` tool.

---

### 2.1.272 ◆◆◆◆

- 🔴 Added the `AppifactRepl` tool to Cowork cloud sessions, behind a remote switch that's off by default: Claude can fill or revise an Artifact made from a Design or Slides type with one JavaScript program, which Node runs outside the command sandbox against the appifact SDK (a built-in copy or a loaded appifact skill's), and each store, file or asset call it makes runs as an Artifact or ArtifactData tool call with that tool's hooks and permission checks. It asks first in plan mode and wherever a rule asks before Bash as a whole runs, is removed where a rule denies Bash as a whole, isn't offered to subagents, and auto mode sets aside allow rules for it.
- 🔴 Added a read-ahead for Cowork cloud sessions opened beside a new, empty Artifact the user owns that was made from a type (same remote switch): before the first turn, Claude Code reads the type's instructions file and saves it, the type's reference pages and, when one design system was chosen for the session, that system's files to the session's scratchpad, then tells Claude in the opening context that they're on disk and to print them with Bash instead of reading them through the Artifact tool. When the files can't be saved, it puts the instructions themselves into the opening context. Where `AppifactRepl` is available, it also adds a two-step note for designs or slides to the type note.
- 🔴 Changed the Artifact tool's quickstart for slides and designs where `AppifactRepl` is available: once a type matches, it saves the type's instruction files (first-party types only) and the picked design system's files to the scratchpad, in place of reading that system's README into its answer, and replies with a two-turn plan: create the Artifact and print those files with Bash, then build it with a series of `AppifactRepl` calls. The "Created a new Artifact" result then says those files are already on disk, so they aren't attached again and the step that lists design systems and reads the chosen one's README is skipped.
- 🔴 Changed the Artifact tool's instructions in Cowork cloud sessions where the remote switch behind `AppifactRepl` is on: a Design or Slides type's instructions now end with a note on building and revising that kind of Artifact efficiently (in one `AppifactRepl` program when the tool is there), the tool's descriptions say to leave out `auto_open` when filling through `AppifactRepl`, the quickstart's hint stops suggesting `auto_open: "after_first_write"`, and a create asking for `auto_open: "after_first_write"` is refused while the tool is available, because those writes never open the page.

<details>
<summary>4 internal changes</summary>

- 🔴 Added a `start_kit` field (`both`, `type_only`, `system_only` or `none`) to `artifact_quickstart`, and success and failure events for the read-ahead (`artifact_opening_prefetch`) and for each `AppifactRepl` artifact call (`artifact_repl_call`), behind the same remote switch.
- 🔴 Added "how to build and iterate on" to the bracketed labels that outside text can't pass off as Claude Code's own, in every session: a bracket followed by those words, in any case or with look-alike letters, now gets a backslash in artifact text Claude reads and in subagent output handed back to Claude, where it's also flagged as an instruction-shaped match and reported in `tengu_subagent_output_flagged`; in Claude's own replies it's counted in `tengu_assistant_output_harness_content`.
- 🔴 Made every session that loads the main app load the `AppifactRepl` tool's module at startup, which reads and unpacks its 25 KB built-in SDK script from the executable even where the tool is off.
- 🔴 Counted an `AppifactRepl` allow rule with no pattern as an overly broad Bash rule in the startup report `tengu_ant_overly_broad_bash_detected`, which is sent outside cloud and Cowork sessions; nothing is stripped.

</details>

<details>
<summary>4 changes with no effect in this build</summary>

- 🔴 Added a way for `AppifactRepl` to run its program inside the command sandbox (a wrapped shell command, cleanup after it, sandbox failures marked in its error output) that nothing can choose: the step that plans the run refuses whenever the sandbox would take the command.
- 🔴 Moved the startup profiler's initialization mark and first wall-clock and process-uptime samples inside the worker that runs plugin function hooks, so they're now taken after 24 more small definitions (a hooks error class, matcher helpers and constants) have loaded; nothing in the worker reports them.
- 🔴 Exported the lookup for a rule that covers Bash as a whole under the name `wholeToolBashRule` from the deny-rule filter module; nothing in the build imports it by that name.
- 🔴 Changed the spelling of a name read inside a branch of the end-of-turn handling that's compiled as `if (false)`; neither build declares either name, and the branch never runs.

</details>

---

### 2.1.270 ◆◆◆◆

- 🔴 Changed the silent retry after a refused request (behind a server-sent switch) to also cover a biology-category refusal of the default Opus model when a fallback model is armed but no fallback route covers that category (Claude Opus 5's built-in routes have none for it), as long as nothing has fallen back yet in the session. Such a refusal used to end the turn.
- 🔴 Fixed a silent retry that continues a refused answer being left open when that retry was itself refused and handed to a fallback model: the first continuation is now closed at the switch, before the fallback model's request, and its salvaged partial answer is no longer carried into that request.

---

### 2.1.267 ◆◇◇◇

- 🔴 Added `session.compact` hooks to veto or rewrite non-isolated compaction, plus `$.session.compact()` for plugins to request compaction between turns in bound, non-headless sessions.
- 🔴 Added `if_version` checks to Artifact `set`, `delete` and batch entries behind the version-pin gate, with no non-atomic fallback for pinned batches.
- 🔴 Made HIPAA restrictions survive organization switches until restart, preserving Remote Control denial and adding HIPAA checks for `/insights` and transcript-scanning statistics.
- 🔴 Made `sec-default` precede user plugins where hooks modules are enabled on machines with managed settings or in Team/Enterprise organizations; only managed `secDefault: false` can opt out.
- 🔴 Improved shared-memory sync to handle deleted-content conflicts, removing stale local copies when timestamp checks pass and otherwise preserving them.
- 🟢 Added global and per-model `maxEffortLevel` caps across providers; `output_config.effort` in `CLAUDE_CODE_EXTRA_BODY` bypasses them.
- 🟢 Fixed malformed managed `allowedHttpHookUrls`, `httpHookAllowedEnvVars` and `allowedChannelPlugins` dropping restrictions; empty allowlists now apply unless valid entries remain.
- 🟢 Fixed a directory containment bypass by rejecting relative marketplace paths that contain backslashes.
- 🟢 Made command, skill and subagent effort override pinned defaults for Opus 4.7, Opus 4.8 and Fable 5, while still respecting environment effort, model support and caps.
- 🟢 Improved auto-mode safety review for Workflow `agent()` output schemas above 4,096 code units, up to 4,000,000, subject to a node budget, while still blocking schemas it cannot review.
- 🟢 Added one automatic retry on a fresh connection for eligible transport failures during Artifact deploy uploads, excluding user aborts, started responses and ambiguous or near-complete uploads.
- 🟢 Made usage-limit warnings reuse valid, unretired readings from the past 30 minutes when an allowed response omits them (remotely controlled, on by default).

---

### 2.1.266 ◆◆◆◆

- 🟢 Stopped `CLAUDE_CODE_USE_GATEWAY` alone from routing requests to the Cloud gateway, which since 2.1.265 had made every request fail with "Not signed in to the Cloud gateway" in proxy and LLM-gateway setups that set it; the sign-in error also stops suggesting the variable.

---

### 2.1.263 ◆◆◆◆

- 🔴 Extended the effort-reduction prompt, which offers to lower your default effort and covered only Opus 5 at `high`, to Fable 5.1 at `high`, `xhigh` or `max`; it still appears only when Anthropic serves its text remotely, and that text now picks which of those levels it applies to (only `high` unless it lists more), the lower level to offer and an optional warning line, and can name the model and both levels in its title and buttons. Each model's prompt is answered separately, ultracode sessions aren't offered it, and accepting changes nothing if the model or effort level changed after it appeared.
- 🔴 Stopped leaving sessions with Remote Control on out of the effort-reduction prompt, so Opus 5's existing prompt and the new Fable 5.1 one can now be shown and accepted there.
- 🔴 Stopped offering or accepting the effort-reduction prompt on custom model names that count as Opus 5 only through a `behavesAs` mapping (a `modelPicker` row in settings or the model list Anthropic serves) or through the discovered backing model of a Bedrock application inference profile; names mapped with `modelOverrides` still qualify. The Bedrock case only reaches sessions with `CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST` set, since other Bedrock sessions never receive the prompt's text.
- 🔴 Added thinking-binding controls to requests sent to Anthropic's API with thinking on, behind a remote switch that's off by default, which `CLAUDE_CODE_POLISHED_DEWDROP` overrides: `drop` asks the API to drop thinking blocks that don't match the conversation, `block` asks it to return an error instead, and `off` turns the controls off. Sessions that authenticate through workload identity federation or a profile in the Anthropic config directory instead of an API key don't send them, and neither do sessions with `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` set.
- 🔴 Added a retry for when Anthropic's API rejects the thinking-binding beta or its `block_binding` field with HTTP 400: Claude Code stops sending both for the rest of the conversation and sends the request again.

<details>
<summary>2 internal changes</summary>

- 🔴 Added the thinking mismatch behaviour each request carried (`drop_block` or `error`) to API request and success telemetry.
- 🔴 Added the model, offered effort levels and whether a warning was shown to effort-reduction prompt telemetry.

</details>

---

### 2.1.259 ◆◇◇◇

- 🔴 Made invalid `allowUnattendedServing` settings block unattended cloud commands until corrected, requiring approval to run commands on this computer.
- 🔴 Changed Artifact pinning to affect only the user's private sidebar, without changing visibility; pinning someone else's Artifact requires live consent, and unpinning is reversible.
- 🟢 Restricted organization-managed MCP servers to HTTP/SSE over valid HTTPS URLs and prevented local removal.
- 🟢 Added `--permission-prompts none` for headless `--print` hosts to automatically deny actions that need a prompt; cloud sessions still send prompts to the host over stdio.
- 🟢 Fixed config writes using defaults after a rejected locked reread by rebasing on the enqueue-time snapshot when safe or refusing the write.
- 🟢 Stopped agents from launching agents, skills, workflows or monitors, or resuming or messaging agents, while their stop is still completing.

---

### 2.1.258 ◆◆◆◆

- 🟢 Stopped user messages with empty content arrays from reaching the API, dropping them before request processing.
- 🟢 Stopped extra model calls when a re-sent permission approval cannot be applied and no prompt or deferred work remains.

---

### 2.1.257 ◆◇◇◇

- 🔴 Added release-manifest signature checks to native updates and blocked installation of staged binaries that no longer match verified checksums, with legacy signature exceptions.
- 🔴 Changed cloud-sync consent to warn that files Claude reads and command output can be seen by session viewers and Team/Enterprise admins, and cloud changes are not copied back locally.
- 🟡 (2.1.267) Made resumed conversations reuse the recorded system prompt despite later prompt flags, when system-prompt recording is enabled.
- 🟢 Added support for fencing `Read`, `Grep`, `Glob` and `LSP` to working directories in every permission mode; sandbox fencing excludes the relaxed filesystem policy.
- 🟢 Stopped repositories from granting `bypassPermissions` through project or local settings; only policy settings, user settings or flags may enable it.

---

### 2.1.252 ◆◆◆◆

- 🔴 Made `/add-dir` drop its cached CLAUDE.md content, so with `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD` set the added directory's CLAUDE.md files and rules reach Claude from the next message instead of after a later reload such as the next compaction.
- 🔴 Added recovery steps to the "task output swap refused (tasks dir moved or linked)" error: restart with `CLAUDE_CODE_TMPDIR` set to a fresh directory, or remove the stray directory or symbolic link at the project's temporary folder (not what it points to) and restart.
- 🔴 Changed the static system-prompt mode (`CLAUDE_CODE_CARVED_SLATE` or a remote switch, off by default) to send CLAUDE.md and other instruction files as a message in the conversation instead of in the context block added to every request, and after a re-read to send only the files that changed, with a line for each file that's gone.
- 🔴 Made the static system-prompt mode record the main conversation's system prompt and tool descriptions in the transcript on its first request and send those recorded versions on later requests and after a resume, until the conversation is compacted, unless a custom or appended system prompt is set or `CLAUDE_CODE_SIMPLE` is on; side requests that fork the conversation send the recorded versions too, and `/context` counts them.
- 🔴 Added, in the static system-prompt mode, the reason to notices that session context or instruction files were re-read (after compaction, a managed-settings change, an added working directory, a settings sync, an account change, or the start of a resumed session), and a notice that the earlier account, project and git values no longer apply when a re-read finds none.
- 🔴 Moved the scratchpad directory, in the static system-prompt mode, from its own system-prompt section into the environment notice, which now also announces a new scratchpad directory and tells Claude to stop using one that's no longer available; agent teams teammates keep the old section as well.
- 🔴 Made side requests that fork the conversation (compaction summaries, prompt suggestions, `/btw` questions and others) in the static system-prompt mode add the session context and date when their messages don't already carry them, which they used to go without. They also get instruction files the way the main conversation now does, as a message added when theirs has none.
- 🟢 Fixed saving settings, such as an "always allow" rule, failing with "Invalid JSON syntax in settings file" when the settings file existed but was empty or held only whitespace.
- 🟢 Stopped Remote Control sessions from waiting for session-persistence writes after tools finish, so degraded connections no longer hold turns open.
- 🟢 Cut background-task notifications longer than about 100,000 characters, and git's error text in worktree-creation errors longer than about 10,000, down to their start and end with a count of what was removed, so very large failure output such as git errors on a full disk no longer pushes a request over the API's size limit.

<details>
<summary>3 internal changes</summary>

- 🔴 Added a warning-level log line, "task output: pin of … refused", written just before the task-directory error.
- 🔴 Stopped the step that builds the CLAUDE.md block from reading the `tengu_paper_halyard` switch, which drops project and local CLAUDE.md files, unless a project or local file is among them; the nested CLAUDE.md lookup that runs when Claude reads a file still reads it, so a session without such files now logs an exposure for the switch only when that lookup runs.
- 🟢 Added, for macOS only, acceptance of a write folder that the real-path lookup spells differently but that is the same folder by device and inode; on Linux this only adds a debug-log warning when such a write is refused on a system where `/proc` can't be read.

</details>

<details>
<summary>1 change with no effect in this build</summary>

- 🔴 Added instruction-file and recorded-prompt messages to the API test-fixture filter, but fixture recording is disabled in this build.

</details>

---

### 2.1.251 ◆◇◇◇

- 🔴 Added support for Artifact live room page events from anyone in your organization with the page open, now or after sharing, to start turns after room approval, even without you present.
- 🔴 Added guidance to use `--fork-session` when branching a copied conversation.

---

### 2.1.250 ◆◆◆◆

- 🔴 Changed Artifact watch guidance to restart watches only on user request or when the task needs republishes, noting they don't reconnect automatically.
- 🔴 Improved Artifact re-watch notices to explain that there's nothing to re-watch while live updates are off.
- 🔴 Parsed existing WebSocket upgrade-failure labels so Artifact watches can use their HTTP status and Cloudflare-mitigation data in retry decisions and telemetry.
- 🔴 Treated unavailable startup responses and slow unanswered startup requests as stalled Artifact watches, using stall retries rather than counting each as an ordinary reconnect failure, and added their HTTP status to stall reports.
- 🔴 Changed stalled Artifact watches to use randomized longer retries and give-up deadlines, and clear the old stall deadline when rearmed.
- 🔴 Kept Artifact watch leases after WebSocket upgrade failures classified as unavailable or Cloudflare-mitigated, so eligible reconnects can try the existing lease again.
- 🔴 Added passive delivery for watch-ended notices while keeping their advice attached.

---

### 2.1.248 ◆◇◇◇

- 🔴 Added `/limit-reset` so eligible users can reset their session limit once a week; usage still counts toward the weekly limit.
- 🔴 Added automatic edits to watched artifacts from writer comments, subject to server rechecks of the writer, thread edit grants and single-file, size and rate limits.
- 🔴 Added live presence data on artifact pages, shared among viewers and produced by the page's code rather than typed as messages.
- 🔴 Added a server-configured `◆ <text>` status pill to the prompt footer.
- 🔴 Added `sdkMcpServers` settings, sent separately so older CLIs ignore them.
- 🔴 Changed directory sync to distinguish uploads that are incomplete, interrupted, stalled, abandoned or arriving mid-session.
- 🔴 Renamed `Coordinator check-in` to `Worker check-in`.
- 🟡 (2.1.212) Changed Windows PowerShell selection to prefer PowerShell 7 when installed, including Microsoft Store installs, with Windows PowerShell 5.1 as fallback.
- 🟢 Added `--restricted` / `CLAUDE_CODE_RESTRICTED` to remove `Bash`, `PowerShell`, `REPL` and `WebFetch` unless in `--tools`, limit file tools to the working directory, reject `bypassPermissions` and ignore user, project and local settings.
- 🟢 Added `claude self-hosted-runner --client-label` to override the runner's registration label.
- 🟢 Added a `workflow-authoring` skill with the `Workflow` tool's script-writing reference.
- 🟢 Added a warning when your GitHub CLI token lacks the `workflow` scope needed to push changes to Actions workflow files.
- 🟢 Added `desktopSessionCleanupPeriodDays` to limit how long transcripts written by desktop-host surfaces, including Claude Desktop and Cowork, survive cleanup.
- 🟢 Changed handling of invalid `crossSessionInbound` values to warn and hold messages, or reject messages under managed settings.
- 🟢 Removed the runtime gate for `/loop`'s self-paced dynamic mode.
- 🟢 Restricted cross-session trust in Linux user namespaces to processes with mapped user IDs.
- 🟢 Changed OAuth refresh to explain when another Claude Code process holds the refresh rather than sending you to the login screen.

---

### 2.1.247 ◆◇◇◇

- 🔴 Added Artifact `preview` to return screenshots of a local `.html` file in light and dark themes at desktop and phone widths
- 🔴 Added Claude Design authorization handling that distinguishes missing claude.ai credentials from valid credentials without Claude Design access
- 🔴 Added a remote-tool protocol for `claude --cloud` sessions, with tool announcements from attached machines and per-call approval
- 🔴 Added a `/cloud-plugins` consent flow for sending a machine's plugins to cloud sessions
- 🔴 Added a system prompt for writing on the user's behalf and updated memory instructions and the `whiteboard`, `design`, and `agent-design-patterns` skills
- 🔴 Renamed `/design-login` to `/design login`
- 🟢 Changed `spinnerTipsOverride` from strings to `{id, text, cooldownSessions, priority}` entries plus `tipsFile` and `label`, removing `excludeDefault`
- 🟢 Changed MCP connection errors to distinguish servers that do not answer from unreachable servers
- 🟢 Changed oversized hook and background output to be truncated with an explicit marker rather than overflowing the conversation
- 🟢 Changed terminal output to filter control and invisible characters in marketplace names and rendered hyperlinks

---

### 2.1.246 ◆◇◇◇

- 🔴 Added a shared whiteboard artifact for wireframe diagrams with shapes, sticky notes, arrows, freehand drawing, and text
- 🔴 Added artifact asset stores with `upload_asset` and `list_assets` for images, video, PDFs, fonts, and text files
- 🔴 Added artifact watching to notify sessions when pages are republished and wake cloud sessions when comments arrive
- 🔴 Added unattended replies to artifact comments after explicit consent, limited to artifacts the user can edit and whose link they supplied
- 🔴 Added scheduled recurring cloud agents, including cron-style routines
- 🔴 Added memory stores with addressable documents, a description, a writable or read-only flag, and an index document
- 🔴 Added cloud-session sync for the working tree and the user's `CLAUDE.md`, permission rules, and preferences
- 🔴 Stopped running hooks for cloud-originated calls unless their commands can be pinned to a single script outside the synced project
- 🔴 Reorganized embedded modules and assets across all five platforms, adding 189 records per client
- 🟡 (2.1.275) Added plugin sync from claude.ai, with a setting to turn it off
- 🟡 (2.1.243) Added organization-managed `modelPricing` overrides, recording whether requests use list rates or managed pricing

---

### 2.1.245 ◆◆◆◆

- 🟢 Updated the bundled Bun runtime to another build of version 1.4.0, which stopped exporting its own `malloc` family under glibc's symbol versions and no longer crashes at startup on systems with glibc 2.44. Its machine code grew by about 150 KB; Bun lost its `bun prune` and `bun dedupe` commands and `bun audit fix`, gained a `bun feedback` command with a `BUN_FEEDBACK_URL` variable, and changed its built-in JavaScript for streams, `bun:sqlite` and sockets, and its shell completions.

<details>
<summary>1 change with no effect in this build</summary>

- 🔴 Added a plain-text handler for `/autofix-pr` that hands its arguments to the autofix runner and replies "Autofix PR cancelled" when stopped, in a module nothing in the build loads; the command still opens its interactive screen.

</details>

---

### 2.1.243 ◆◆◆◆

- 🔴 Changed the LSP tool gate to default on instead of requiring an environment-variable opt-in

---

### 2.1.241 ◆◆◆◆

- 🔴 Added the effective Sable Thrush feature gate to `experiment_gates` notifications sent to the VS Code extension; the session reads the gate when the extension connects and keeps that value until it ends
- 🔴 Changed non-interactive sessions, including SDK and `-p`, to ask the API for thinking narration and connector text under the same rules as interactive sessions, where before they never did; those rules still need the Sable Thrush remote switch, which is off by default, and thinking display updates (`CLAUDE_CODE_THINKING_DISPLAY_UPDATES`, a model capability or a remote switch)
- 🔴 Changed narration signature decoding to use `atob` and `Uint8Array` instead of `Buffer`, so a signature that isn't valid base64 is now rejected as not narration instead of being decoded loosely
- 🔴 Changed SDK documentation to clarify that `null` restores Claude Code's own thinking-display handling

<details>
<summary>1 change with no effect in this build</summary>

- 🔴 Removed the internal narration exports `NARRATION_SUMMARY_LABEL` and `getSignatureBlockKind`; nothing in the build read them, and the label and the decoder are still used directly

</details>

---

### 2.1.240 ◆◆◆◆

- 🔴 Added a thinking display mode behind a remote switch that's off by default: thinking blocks the API tags as narration appear in the conversation, marked "(summarized)", where other thinking shows only in the transcript view or verbose mode. Requests then set `display: "updates"` on the thinking setting with the `thinking-display-updates-2026-08-18` beta (dropping the redact-thinking beta) when `CLAUDE_CODE_THINKING_DISPLAY_UPDATES` or a second remote switch also allows it, in interactive sessions on Anthropic's API that aren't signed in with a claude.ai subscription, on models with interleaved thinking and with no thinking display chosen.
- 🔴 Added a one-time dialog offering Claude Opus 5 users at high effort to switch their default effort to medium ("Switch your default effort to medium?"); accepting saves medium as the default. It needs two remote settings that are off by default, one of them carrying the dialog's text, and doesn't appear when `CLAUDE_CODE_EFFORT_LEVEL` is set or in background, remote or teammate sessions.
- 🔴 Started sending `narrationBlockCount`, the number of narration-tagged thinking blocks in the response, with every successful API request's telemetry; the field existed but was always left out.
- 🟡 (2.1.251) Changed saving an effort level (`/effort`, the `/model` picker) to write one place only: the top-level `effortLevel` by default, where it also used to write a per-model `modelSettings` entry, or only the per-model entry behind a remote switch that's off by default, so each model keeps its own level.

<details>
<summary>2 changes with no effect in this build</summary>

- 🔴 Added a test-only helper, `clearNarrationFlagLatchForTests`, that clears the cached narration-switch result; nothing in the build calls it.
- 🔴 Changed an unused text-row renderer's formatter reference from a git working-tree cleanliness check to a git-root lookup. Nothing in the build calls that renderer.

</details>

---

### 2.1.237 ◆◆◆◆

- 🔴 Added harness event delivery for remote sessions (`CLAUDE_CODE_REMOTE` set, `CLAUDE_CODE_ENVIRONMENT_KIND` not) that also set `CLAUDE_CODE_POLL_EVENTS`, which a settings file's `env` block can't set: the SDK `poll_event` request, always refused before, now queues events while the session is in auto mode, and a new main-thread `Poll` tool lets Claude wait for queued events and read them. An event can name its sender's authority (`human-principal`, `human-other`, `peer-agent` or `world-event`), and auto mode's classifier treats only `human-principal` events as the user's own words. Turns a poll event starts run without the always-allow rules that slash commands, skills and agents granted, and with a second remote switch (off by default) notices from other sessions arrive as `peer-agent` events.
- 🔴 Made Claude in Slack and Claude in Teams sessions treat a message that arrives through Remote Control labelled as typed by a person in the bound thread (inbound origin `slack_human` or `teams_human`) as the user's own turn, where it used to be demoted to a message from an unknown peer session. In Slack sessions, auto mode's classifier now sees each relayed turn verified as typed by a person marked "[Verified human message relayed from the bound Slack thread]:", and its rules now say that posts by bots or other agents in the thread never establish the user's intent or consent. The classifier also indents the text of relayed turns that aren't verified, so they can't open with that marker, and on the plain SDK input stream such person-typed messages are now flagged verified too.
- 🔴 Changed what auto mode's classifier is shown for user turns of unknown origin: they now carry the label "[MESSAGE FROM NON-USER SOURCE - NOT USER INPUT]" instead of appearing as plain user text. That covers synthetic messages from an SDK host and, newly, the earlier turns of a background subagent or agent-teams teammate resumed from its saved transcript; resuming also drops any Slack "verified human" flag and classifier markers stored in that transcript and counts its `human-principal` events as `human-other`.
- 🔴 Changed automatic replies to artifact comments (opt-in with `CLAUDE_CODE_ARTIFACT_COMMENTS_AUTOREACT` or a remote switch that's off by default): when Claude starts acting on a thread it was summoned into in the last 10 minutes, it now checks those summons against a turn already visible in the session and stands down if that turn took them over, and with `CLAUDE_CODE_ARTIFACT_COMMENT_FAST_ACK` or a second remote switch (also off by default) on, it answers them as fresh requests.
- 🟢 Fixed prompt caching behind LLM gateways and custom base URLs: Claude Code now puts a cache breakpoint on a system message placed among the conversation's messages only when it talks to Anthropic's API or Claude Platform on AWS at their default addresses, Bedrock, Vertex AI or Mantle without a custom base URL, or Foundry at its default address or an Azure `.services.ai.azure.com` host; anywhere else the breakpoint goes on the last user or assistant message.
- 🟢 Added Concise, a new built-in output style offered in `/config`'s output style setting: Claude opens with the answer or outcome, drops preambles, step-by-step narration and closing recaps, keeps simple answers to a few sentences, and still gives full detail when asked and for errors, failing tests, security warnings and destructive-action confirmations.

<details>
<summary>1 internal change</summary>

- 🔴 Started reporting `slack_bot_observation` as the platform class of the bridge's demotion telemetry event in Slack sessions; the value existed before but sat behind a check that was always false.

</details>

<details>
<summary>1 change with no effect in this build</summary>

- 🔴 Changed an unused text-row renderer's formatter reference from a billing-consent check to the command-history store getter. Nothing in the build calls that renderer.

</details>

---

### 2.1.231 ◆◆◆◆

- 🟢 Changed the redirect address MCP OAuth sign-in sends from `http://127.0.0.1:<port>/callback` to `http://localhost:<port>/callback`, fixing redirect mismatches with servers whose pre-registered OAuth client expects `localhost`.

---

### 2.1.226 ◆◆◆◆

- 🔴 Changed only file and product version resources on Windows, with no substantive JavaScript changes and identical bundled prompts across all five clients

---

### 2.1.220 ◆◆◆◆

- 🔴 Stopped the automatic switch after a refused request from moving to Claude Opus 5 when Claude Code can't read which models the account may use (on the Anthropic API with a claude.ai token lacking the `user:profile` scope, such as `CLAUDE_CODE_OAUTH_TOKEN` from `claude setup-token`, no API key and no cached model list): it moves to Claude Opus 4.8 instead, and skips Opus 5 when stepping down the Opus lineup.
- 🔴 Added the `auto-mode-classifier-2026-07-16` beta header to auto mode's classifier requests on the Anthropic API, unless experimental betas are turned off (`CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` or HIPAA mode) or the base URL isn't Anthropic's.
- 🔴 Added an `entitlement_blind` field to the refusal-fallback telemetry events, saying whether the account's model entitlements were unreadable.

<details>
<summary>1 change with no effect in this build</summary>

- 🔴 Reworded the warning logged when auto mode's classifier drops a beta that the API rejected; the retry that logs it only runs for a placeholder beta that's empty in this build.

</details>

---

### 2.1.215 ◆◆◆◆

- 🔴 Added `CLAUDE_CODE_THISTLE_GREBE` (or a remote switch) to tone down Claude's encouragement to use subagents: set to `no_nudges` or `counter_steer`, it drops the lines in the Glob, Grep, Agent, EnterPlanMode and ExitPlanMode texts, the list of agent types and the system prompt that push Claude toward the Agent tool or teammates, and plan mode has Claude explore and design the plan itself instead of launching Explore and Plan agents; `counter_steer` also adds a "Delegating to subagents" section telling Claude to work inline and keep spawns few. The value `default`, which applies unless something sets another, leaves every text as it was, and either of the other two values is reported in telemetry.
- 🔴 Added a switch, `CLAUDE_CODE_GAULT_KESTREL` or a remote switch that's off by default, that removes the shorter system prompt's instruction to speak up instead of proceeding when something Claude is about to delete or overwrite doesn't match how it was described or wasn't created by Claude; Claude is still told to look at the target first.
- 🟡 (2.1.224) Added a line to the Bash tool's description for models on the shorter system prompt, telling Claude that command output is shown to it but not reliably to you, behind `CLAUDE_CODE_MARL_CORMORANT` or a remote switch that's off by default.
- 🟢 Stopped Claude from running the bundled `/verify` and `/code-review` skills on its own; you can still start them by typing the command.

<details>
<summary>1 change with no effect in this build</summary>

- 🔴 Changed an unused text-row renderer's formatter reference: it now points to an undeclared name instead of a bundled React component. Nothing in the build calls that renderer.

</details>

---

### 2.1.214 ◆◆◆◆

- 🔴 Added `MIMALLOC_SCAVENGER=0` to the environment of shell commands run in the sandbox, including the copy of Claude Code that the Linux sandbox runs to apply its seccomp filter.

---

### 2.1.209 ◆◆◆◆

- 🔴 Started marking every later request to Anthropic's API with `x-is-refusal-fallback: true` once a refusal in the cyber category has moved a main-conversation request to a fallback model, also after resuming such a session and until a rewind goes back past it (before, only a silent refusal fallback did that, and only with an account-level remote switch on), and added `x-cc-fallback-latched-by`, the refused request's id, to the marking. The request re-sent on a fallback model after a refusal now also carries `x-cc-fallback-from-model`, `x-cc-fallback-category`, `x-cc-fallback-trigger` and `x-cc-original-request-id`.
- 🔴 Changed `/upgrade` back to starting a new login when the browser can't be opened, without showing the upgrade address; 2.1.208 had made it print the address and stop.
- 🔴 Added runtime capabilities for published Artifacts behind a remote switch that's off by default: the Artifact tool takes a `capabilities` input (today `mcp`, so the page can call the user's claude.ai connectors) and a `contract` input that pins, upgrades (`latest`) or rolls back the page's runtime version, its description and the `artifact-*` commands explain it, and a new `/artifact-capabilities` skill, which Claude is told to load before declaring any, fetches the current contract's type definitions and guidance.
- 🟢 Let `claude agents` background sessions open the `/model` picker (saving the pick as the default for new sessions again), the Fable 5 usage-credits consent prompt, `/login`, `/upgrade`'s sign-in and Remote Control's device-enrollment sign-in, and run `/pro-trial-expired` at startup when the Pro trial has ended, undoing those parts of the background-session guard 2.1.208 added; its blocks on `/install-github-app` and the `/mcp` menu stay.

<details>
<summary>1 change with no effect in this build</summary>

- 🔴 Changed an unused text-row renderer's formatter reference from an undeclared name to the React module object. Nothing in the build calls that renderer.

</details>

---

### 2.1.204 ◆◆◆◆

- 🟢 Made headless sessions using `--output-format stream-json` with `--verbose` write events produced during startup, such as those from SessionStart hooks, as they happen instead of holding them until the session begins, so a remote worker no longer looks idle and gets shut down partway through a hook.

---

### 2.1.201 ◆◆◆◆

- 🟢 Stopped Claude Sonnet 5 sessions from sending Claude Code's reminders as system messages in the middle of the conversation; they stay inside user turns and the `mid-conversation-system-2026-04-07` beta isn't requested, unless `CLAUDE_CODE_FORCE_MID_CONVERSATION_SYSTEM` is set or, on Bedrock, Vertex, Foundry or Mantle, the model's `…_SUPPORTED_CAPABILITIES` variable lists `mid_conversation_system`.

---

### 2.1.197 ◆◆◆◆

- 🔴 Changed auto-compact for Sonnet 5 to treat its window as 967,000 tokens (500,000 in sessions started with the `local-agent` or `remote_cowork` entrypoint) instead of the full 1M, unless a server-sent setting replaces that default.
- 🔴 Stopped `/model` from asking "Switch model?" mid-conversation when the switch only adds or drops the `[1m]` suffix, as from `sonnet` to `sonnet[1m]`.
- 🔴 Changed `/model` to open on the alias row a configured full model id resolves to, such as "Sonnet" for `claude-sonnet-5`, instead of adding a separate row for the id; the SDK's model list gained a `resolvedModel` field so hosts can match the same way.
- 🔴 Changed the system prompt's line about current models to name the Claude 5 family, Opus 4.8 and Haiku 4.5 (it named Fable 5 and the Claude 4.X family), with Sonnet 5 in its list of model ids.
- 🔴 Changed the built-in guide for creating routines (the `/schedule` skill, offered to claude.ai subscribers behind a remote switch that's off by default) to default new routines to `claude-sonnet-5` instead of `claude-sonnet-4-6`.
- 🔴 Updated the bundled `claude-api` skill for Sonnet 5 (its model tables with the $2/$10 intro price through 2026-08-31, a new guide for migrating from Sonnet 4.6, "sonnet" now meaning Sonnet 5) and added Managed Agents material on per-session agent overrides, live text previews, pagination, fetching one deployment run, webhook events for agents and deployments, and where a credential is injected. The guides also correct their existing Sonnet 4.6 guidance: its output limit is 128K, not 64K, and its effort options include `max`, which they used to call Opus-only.
- 🔴 Made Claude Code treat `CLAUDE_CODE_ENTRYPOINT=local_agent` as `local-agent`.
- 🔴 Changed the attribution line Claude Code adds to pull requests ("🤖 Generated with Claude Code …") to name `claude-sonnet-5` in Sonnet 5 sessions, where it said "claude".
- 🟢 Added Claude Sonnet 5 (`claude-sonnet-5`) with a native 1M-token context window on Anthropic's API and also on Bedrock, Vertex, Foundry and gateways (the first model with native 1M there), effort up to `max` including `xhigh`, and 64K output tokens by default with a 128K cap. It can use web search on Vertex and, where the advisor tool is turned on (Anthropic's API only, off by default), the advisor; it reads its Vertex region from `VERTEX_REGION_CLAUDE_5_SONNET`, is costed at $3/$15 per Mtok, appears in the gateway's `/v1/models` when that list includes built-in models, and on a third-party provider that lacks it the error suggests Sonnet 4.6. Its catalog entry turns on adaptive thinking, context management and mid-conversation system messages, allows images up to 2000×2000 and streams tool input eagerly on Bedrock and Vertex, and the effort picker now lists Sonnet 5 under `xhigh` and Sonnet 4.6 or later under `max`.
- 🟢 Made the `sonnet` alias resolve to Sonnet 5 on Anthropic's API unless `ANTHROPIC_DEFAULT_SONNET_MODEL` names another model; Bedrock, Vertex, Foundry and Mantle stay on Sonnet 4.5, and Claude Platform on AWS and gateways on Sonnet 4.6.
- 🟢 Changed `/model` to offer Sonnet 5 as its "Sonnet" row; on Anthropic's API, Claude Platform on AWS and gateways (not on claude.ai plans) it shows the $2/$10 per Mtok promo price, with the $3/$15 list price struck through, while a server-sent end date lies ahead. Sonnet 4.6 moved to a "Sonnet 4.6 · Previous Sonnet version" row, offered on third-party providers and, outside claude.ai plans, when Sonnet 5 isn't the Sonnet default (as on Claude Platform on AWS and gateways), and the separate 1M row, now "Sonnet 4.6 (1M context)" pinned to Sonnet 4.6 or "Sonnet 5 (1M context)", is hidden when the Sonnet default already has native 1M context.
- 🟢 Stopped refusing `sonnet[1m]` as unavailable for the account, and stopped the tip that suggests `/model sonnet[1m]`, when the Sonnet model it names already has native 1M context, as Sonnet 5 does on Anthropic's API; the refusal, which now also covers `sonnet-5[1m]` where Sonnet 5 lacks native 1M, says "Sonnet with 1M context".

<details>
<summary>3 internal changes</summary>

- 🔴 Changed error reports to tag Sonnet 5 sessions with the model's name instead of "other".
- 🔴 Changed the settings schema's descriptions of the prompt-hook and agent-hook `model` fields to use `claude-sonnet-5` as the example.
- 🔴 Added an internal `promoListPrice` field next to `resolvedModel` in the SDK's model list.

</details>

<details>
<summary>1 change with no effect in this build</summary>

- 🔴 Added an error to the Bedrock default-model setup for a model tier with no Bedrock id; the four tiers it reads (Sonnet 4.5, Opus 4.6, Haiku 4.5, Fable 5) all have one, so it can't fire.

</details>

---

### 2.1.190 ◆◆◆◆

- 🔴 Added support for a weekly Fable 5 limit (`seven_day_overage_included`, read from rate-limit headers including the new `anthropic-ratelimit-unified-7d_oi-*` ones, and added to the SDK's rate-limit types): warnings and refusals call it the "Fable 5 limit". When a plan's included Fable 5 usage runs out mid-session, the usage-credits prompt reads "You've reached your Fable 5 limit" and explains that continuing on Fable 5 uses usage credits.
- 🔴 Treated Enterprise seats, and any plan type named in the remote list `tengu_saffron_credits_only_tiers` (Enterprise by default), as plans where Fable 5 always uses usage credits: they can pick Fable 5 in `/model` and keep it as their default without the usage-credits consent prompt, and the confirmation after switching to it adds " · Draws from usage credits".
- 🔴 Started saving Fable 5 usage-credits consent under a new key (`fableOverageConsentV2`), so consent given in an earlier version no longer counts and Claude Code asks again.
- 🔴 Changed the Fable option descriptions in the `/model` picker to say " · Requires usage credits" when the account needs usage credits for Fable 5 and nothing otherwise, dropping " · Draws from usage credits", " · Included with your plan until <date>" and " · Uses your limits ~2× faster than Opus".
- 🔴 Stopped greying out Fable in `/model` (labelled "Fable (disabled)", with "contact your admin to turn on usage credits" added to its description) for Team members without billing access when Fable 5 needed usage credits their organization had turned off. The greyed-out row now sits behind a remote switch that's off by default; when on, it isn't limited to Team plans and says the model requires usage credits instead of pointing to an admin.
- 🔴 Changed Fable 5 usage-credit messages for members who can't manage billing: instead of pointing them to an admin, to `/usage-credits` or to buying more, they now say only to switch models ("/model to switch models.", "Switch models to keep working."). Refusals that opened with "You've reached your Fable 5 limit." now say that only for the weekly limit and otherwise open with "Fable 5 requires usage credits.", and when the organization's usage credits are off they tell people who can manage billing to run `/usage-credits` instead of asking an admin.
- 🔴 Changed how a Fable 5 request refused for usage credits is handled: Claude Code recognizes it by the API's `credits_required` error code (or the weekly Fable 5 limit) instead of by the error text, and no longer opens the rate-limit options menu on its own, shows the suggestion line under the notice (such as "/upgrade to increase your usage limit." or a `/usage-credits` hint), or puts the consent prompt in front of the next Fable 5 request.
- 🔴 Made turning on or buying usage credits in the usage-credits dialog count as consent to use them for Fable 5 when the session is on Fable 5 and the account needs credits for it, so the consent prompt doesn't follow.
- 🔴 Changed startup announcements: one can now be limited to people who can use a given model (`requiresModel`), the text renders as Markdown instead of picking out slash commands in colour, and the top announcement is sent to the VS Code extension (`startup_announcement`).
- 🔴 Added `model_scoped` to `rate_limits` in the SDK `get_usage` reply: per-model weekly limits (name, utilization, reset time) taken from the server's limits for the models on a remote allowlist.
- 🔴 Changed the usage-credit balance lookup to treat a reply without an amount as an unknown balance, so the Fable 5 usage-credits prompt no longer shows such accounts a zero balance when their usage credits are on, and the usage-credits screen shows their balance as "—" instead of "$NaN".
- 🔴 Added an `enabled` field to the remote Fable usage-credits config (`tengu_saffron_lattice`); when it's `false`, the config no longer marks Fable 5 as needing usage credits.

---

### 2.1.187 ◆◆◆◆

- 🔴 Changed refusal notices to read "<model>'s safeguards flagged this message" instead of "<model> has safety measures that flagged something in this session" (or, for cybersecurity and biology topics, "… that flag messages on most cybersecurity or biology topics"); the notices that switch, or offer to switch, to another model after a cybersecurity or biology refusal now add that the safeguards are intentionally broad right now and may flag safe and routine coding, cybersecurity, or biology work.
- 🔴 Reworded the messages shown when a Fable 5 request needs usage credits ("You're out of usage credits …", "You've hit your monthly spend limit …", "You've reached your Fable 5 limit …") and dropped the "API Error:" prefix they carried.
- 🔴 Made the "Now using usage credits" notice name the model and when its limit resets ("Now using usage credits for Fable 5 · Your Fable 5 limit resets …") when the server lists that model as drawing from credits; the list is empty by default.
- 🔴 Stopped offering AskUserQuestion, EnterPlanMode and ExitPlanMode in non-interactive runs (`-p` and SDK sessions) unless a permission prompt tool is set (`--permission-prompt-tool`, or the permission channel of `--sdk-url`).
- 🔴 Gave subagents that run in plan mode the ExitPlanMode tool when their tool list didn't include it.
- 🔴 Made PostToolBatch hooks run when a tool result ends the turn (an MCP server's end-turn signal or a StructuredOutput call); their messages show, and a block they return is logged and dropped because the model isn't called again.
- 🔴 Removed the "Meet Fable 5" launch notice, which showed only when a remote switch turned it on, and added a startup notice for announcements the server sends (`tengu_startup_announcements`, empty by default), each shown at most three times by default with `/command` names highlighted, and now always tells the VS Code extension not to show its own Fable 5 launch notice (`fable5_launch_show` is false).
- 🔴 Changed the Artifact tool to tell Claude it must load the `artifact-design` skill first to judge how much design a request warrants, and rewrote that skill around plain versus editorial treatments ("Design guidance and fundamentals for Artifacts"); Artifacts sit behind a remote switch that's off by default and are offered only to claude.ai logins on Team or Enterprise plans (or whose plan isn't known).
- 🔴 Changed Artifacts further: published ones are recorded in the session and show as "⧉" links on the job's card in `claude agents`, subagents keep the tool's result, the Explore and Plan agents can't use the tool, and a missing file now says "Create the file first (Write tool, or via shell if Write is unavailable), then retry with the same path."; same gate as above.
- 🔴 Added a one-time warning when the advisor is on but the main model isn't in the advisor capability table ("Advisor disabled — base model '…' isn't in the advisor capability table …").
- 🔴 Made the `claude agents` view follow the `tui` setting: with `tui: "default"` it no longer switches to the full-screen alternate screen.
- 🔴 Made the `claude agents` view offer agents from plugins as templates, not only your own and project agents.
- 🔴 Stopped counting memory-consolidation ("dreaming") tasks as background work still running, for example in the "Detached — N tasks still running" message.
- 🔴 Added a note to auto mode's "could not evaluate this action" block when an upstream safety filter refused the classifier's own request: the block is "not a verdict on this action".
- 🔴 Stopped telling Claude after an auto-mode denial that the user can add a Bash permission rule when managed settings allow only managed permission rules (`allowManagedPermissionRulesOnly`).
- 🔴 Added a Claude in Chrome classifier floor (`CLAUDE_CHROME_CLASSIFIER_FLOOR`, or a remote switch that's off by default): Chrome tool calls that an allow rule, an allowed site or bypass-permissions mode would let through are then checked by the auto-mode classifier when it can run, and go to the normal permission handling when it can't.
- 🔴 Let SDK hosts pin an MCP server's tools to `auto` through `set_mcp_permission_mode_override` when auto mode is available, and store an override for a server name that isn't configured or connected yet, with a warning, instead of rejecting it.
- 🔴 Added an `add_directory` control request for SDK hosts of cloud sessions: it downloads the file at a mounted path and adds the folder holding it to the working directories (needs `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD`); staging a file now fails on an HTTP error reply instead of carrying on.
- 🔴 Made skills that an SDK host's MCP servers publish as resources show up as commands in the session, and go away when the server is removed; MCP skills sit behind a remote switch that's off by default.
- 🔴 Made an MCP tool result that fails schema validation report 'MCP server "<name>" returned a malformed result that failed schema validation: …' instead of the raw validation error.
- 🔴 Let `--tools` take `preset:default` next to other names, meaning every default tool plus the ones listed.
- 🔴 Stopped `CLAUDE_CODE_INCLUDE_PARTIAL_MESSAGES` from making runs other than `-p --output-format stream-json` exit with the "--include-partial-messages requires …" error; the variable is now ignored there, and the error stays for the flag.
- 🔴 Made the Read tool open files without an extension as images when their first bytes are an image signature.
- 🔴 Tightened GIF detection to require a full `GIF87a` or `GIF89a` header instead of any data starting with "GIF".
- 🔴 Made the in-session surveys answered with a number key (such as "How is Claude doing this session?") accept the AZERTY digit row (`&`, `é`, `"`, `'`, `(`, `-` or `§`, `è`, `_`, `ç`, `à` for 1 to 0).
- 🔴 Recognized the keypad's arrows, Home, End, Page Up, Page Down, Insert, Delete and separator keys from terminals that use the kitty keyboard protocol.
- 🔴 Changed Bash permission checks of piped commands to strip output redirects to quoted or variable targets (`> "out.txt"`) from each segment before matching rules, as they already did for bare file names.
- 🔴 Stopped turning en and em dashes into hyphens in PowerShell redirect targets when checking where a command writes (PowerShell tool, opt-in on Linux).
- 🔴 Made Bash commands that assign `HISTCHARS` or `histchars` count as setting a dangerous variable, like `PATH` or `BASH_ENV`.
- 🔴 Extended the sandbox's write protection of `.git/config` to its `config.lock` file (and to `.git/config.worktree.lock` in GitHub Actions).
- 🔴 Made the Linux sandbox hide the real file behind a symlink listed in a read-deny rule, instead of masking the link's own path.
- 🔴 Stopped turning backslashes into slashes in project paths on Linux, so a folder whose name contains a backslash keeps its own trust and project settings.
- 🔴 Stopped FileChanged hooks from watching paths that start with two slashes or backslashes (network-share style paths other than WSL's).
- 🔴 Made worktrees created by a WorktreeCreate hook fall back to `git worktree remove` on exit when no WorktreeRemove hook is configured, instead of being left in place. When a configured removal hook leaves the folder in place, ExitWorktree and the exit dialog now report that it was kept instead of reporting a successful removal.
- 🔴 Changed automatic naming of background jobs to wait for the job's pending state writes before saving the name, and to use text the job sent through a `CLAUDE_CODE_TERMINAL_MCP_TOOLS` tool as its reply when its reply has no text; a job started from the default `claude` agent that has no name or prompt words yet now shows "new session" while working, as `bg` jobs already did.
- 🔴 Removed the built-in list of Slack bot MCP tools (`mcp__slackbot__reply` and others) that marked a turn as finished; only tools named in `CLAUDE_CODE_TERMINAL_MCP_TOOLS` do now, and a turn ending with one no longer gets the "no visible output" nudge.
- 🔴 Kept the `claude agents` supervisor running when its roster file can't be written because the disk or the file-descriptor limit is full.
- 🔴 Clarified the subagent instruction against writing report files: files written as input to another tool are fine.
- 🔴 Gave DesignSync's authorization errors their own wording for headless sessions such as claude.ai/code and told the `/design-sync` skill to pass the tool's message on word for word; the hint that `/login` with a Claude subscription also grants design access is gone.
- 🔴 Added a usage line to skill details in `/plugin`: how many times the skill ran and when it was last used, or "never invoked".
- 🔴 Made `-p` and SDK sessions start a turn right away for a prompt that's already queued when the session begins reading input, or when a duplicate incoming message is skipped, instead of waiting for the next message to arrive.
- 🔴 Made background sessions started with memory switched off (`CLAUDE_BG_MEMORY_TOGGLED_OFF=1`) say memory is "paused" when a memory read or write is blocked, and show a "Memory · Paused for this session" row in the status panel and "memory paused" in the footer.
- 🟡 (2.1.214) Made a `claude agents` supervisor that another one has replaced step aside without deleting the new one's socket.
- 🟡 (2.1.325) Fixed brief mode's reply check counting a tool result that lacks display metadata as a new user request, which made it ask for another reply after one had already been sent.
- 🟢 Added the `sandbox.credentials` setting: files listed with `mode: "deny"` can't be read inside the sandbox, listed environment variables are unset for sandboxed commands, and an invalid block is dropped with a warning that no credential protection applies until it's fixed.
- 🟢 Added org model restrictions from the account's model entitlements (first-party and gateway sessions): a restricted model is refused with "Model '…' is restricted by your organization's settings. Run /model to choose a different model.", skipped for plan-mode upgrades and in the model picker, and the default model steps down to an allowed one.
- 🟢 Added mouse clicks to select menus: clicking an option picks it and hovering shows a pointer (clicks need mouse tracking, as in fullscreen mode).
- 🟢 Fixed `--resume` finding no conversation after a `-p` run that produced no model turns: the session file is now created for system entries too.
- 🟢 Fixed structured output: a successful StructuredOutput call now ends the turn instead of letting the model call it again, and one reminder replaces the stop-hook nudges when the call is missing, in `--json-schema` runs, workflow `agent({schema})`, hook agents and background jobs (which no longer stay "working"). When reply text is empty, SDK results now also put the structured output's JSON in `result`.
- 🟢 Added an idle timeout for tool calls to remote MCP servers (HTTP, SSE, WebSocket and claude.ai connectors): no response or progress for 5 minutes aborts the call with an error; `CLAUDE_CODE_MCP_TOOL_IDLE_TIMEOUT` (milliseconds, 0 turns it off) changes it, and open elicitations pause it.
- 🟢 Fixed Claude Code Remote sessions waiting for the agent proxy's system certificate install before starting; it now runs in the background.
- 🟢 Fixed non-ASCII text, such as pasted Korean or CJK, turning into mojibake in terminals that send each UTF-8 byte as a separate kitty-protocol or modifyOtherKeys key event.
- 🟢 Fixed a session restarted to reattach Remote Control (as `/update` does) stopping at onboarding or trust dialogs; it now skips them, as background sessions do.
- 🟢 Fixed channel connections dropping when Claude Code restarts itself (`/bg`, `/tui`, `/update`, the agents view): `--channels` is now passed to the restarted process.
- 🟢 Fixed agent stop notifications to say who stopped the agent ("was stopped by Claude", "was stopped by user") and reworded them ("finished", "failed:" instead of "came to rest").
- 🟢 Fixed subagent depth tracking so resumed subagents keep their depth and forked ones count toward the five-level nesting limit, and an agent past that limit that still calls the Agent tool is refused with "Subagent nesting limit reached …".
- 🟢 Fixed leaked agent worktree registrations: cleanup now removes locked agent worktrees whose folder is gone and whose owning process has ended.
- 🟢 Listed `--bg`/`--background` in `claude --help`; the flag already worked.
- 🟢 Fixed Esc, Ctrl-C and Ctrl-D not working while `/share` is uploading.
- 🟢 Made GitHub Actions setup optional in `/install-github-app`: after the app installs you can skip the workflow and secret steps and run the command again later.
- 🟢 Added ←/→ to `/btw` to step through earlier answers.
- 🟢 Turned on unused-plugin hints for everyone (they were behind a remote switch that's off by default): a tip names plugins you haven't used for 14 days and 10 sessions, `/plugin` groups them as "not used in N days", and plugin details show "Last used".
- 🟢 Made loading a large session's messages pause regularly for other work instead of blocking until it's done.

<details>
<summary>18 internal changes</summary>

- 🔴 Stopped publishing an empty post-turn summary when a turn has no assistant text.
- 🔴 Made the Artifact tool's reads of an existing artifact tell claude.ai they come from the model (`?via=model_read`).
- 🔴 Made the end-turn step rerun the session summary, instead of reusing the stored one, when the turn's reply went through a `CLAUDE_CODE_TERMINAL_MCP_TOOLS` tool.
- 🔴 Made the `claude agents` supervisor delete the folder of a worker started from the spare pool when it exits without writing its state.
- 🔴 Added telemetry fields: subagent type and built-in flag on tool events, the OAuth error description, sandbox state on Bash events, adopted shell, agent, workflow and cron counts, the fabrication detector's tail marker with request and message ids, the skill name on API events and a carry-over count; two Bash events also go to a second first-party analytics sink, and whether an auto-mode decision concerns a Chrome MCP tool (`chromeAutomode`).
- 🔴 Made the policy-limits eligibility check say why a session isn't eligible (third-party provider, custom base URL, no login, no inference scope, consumer plan), for telemetry.
- 🔴 Kept the account's server-sent client data per entrypoint, model, version and organization (up to 12 sets, refreshed after a day) and refetched it when the model changes or no set exists yet for the current combination.
- 🔴 Let the server-sent auto-compact windows differ by entrypoint and subscription plan.
- 🔴 Made the git and global-config file watchers catch and log errors instead of letting them escape.
- 🔴 Removed abort listeners from parent signals once the child signal is garbage-collected.
- 🔴 Sped up the file index behind `@` suggestions: git path normalization is cached while git's output is unchanged and pauses for other work, and results computed across a cache reset are dropped.
- 🔴 Changed team memory sync to skip files inside dot-folders and ignore its own `.memory-sync` files; a manifest that keeps two stores from mixing sits behind a remote switch that's off by default.
- 🔴 Made background agents restarted after an interruption drop the rest of an assistant reply whose tool calls never completed.
- 🔴 Made resuming from a transcript file path load it as a full session (title, tag and other saved details), and restored the last prompt and conversation leaf from the session's saved metadata.
- 🔴 Made the main screen reset the current session's transcript in the shared store each time it mounts, instead of keeping an existing one.
- 🔴 Made a session function hook registered again with the same id replace the earlier one instead of stacking.
- 🔴 Changed workflow logs and labels to show objects as `[object]` and functions as `[function]`, and added progress, total tokens and tool calls to a finished workflow's result record.
- 🔴 Treated the memory-extraction side query like the other background side queries (no "no visible output" nudge, no reactive compaction, no prompt-too-long event), and changed some debug logs (path redaction in error text, an `mcp add-json` invalid-JSON line).

</details>

<details>
<summary>15 changes with no effect in this build</summary>

- 🔴 Added a helper to flush and replace team-memory stores and remove old mount folders, plus a test helper for inspecting the current stores. Nothing in the build calls them.
- 🔴 Added a helper that copies the current session's metadata into an object. Nothing in the build calls it.
- 🔴 Renamed the disabled `/toggle-memory` command to `/pause-memory` (with `memory-pause` and `toggle-memory` as aliases) and reworded its own replies to say memory is "paused" and "resumed"; the command's enable check always returns false, so the command can't be used.
- 🔴 Added `allow_context_tips` to the policies HIPAA organizations have off; nothing in this build asks for that policy.
- 🔴 Removed the Claude Code Desktop upsell dialog ("Open in Claude Code Desktop", "Not now", "Don't ask again"); its enable check always returned false.
- 🔴 Removed the "verify plan" reminder attachment; its generator always returned nothing.
- 🔴 Removed an automatic permission to write "wiki files"; the check behind it always returned false.
- 🔴 Removed the `verification_agent` query source from internal lists; nothing set it.
- 🔴 Removed a startup warning about allow rules "not available for Ants" and added an internal deny-rule list; the warning's list was always empty, and so is the new one.
- 🔴 Added an embedded layout for the main session screen (fixed height, cancel hook from outside); nothing renders it that way.
- 🔴 Let cloud-session creation pass an appended system prompt and an output schema; no caller sets them.
- 🔴 Added `setMcpPermissionModeOverride` to the bundled SDK client; nothing calls it.
- 🔴 Removed the pending plan-verification state set after a plan was accepted; it was always off.
- 🔴 Added a plugin "Usage" view listing each skill's approximate tokens per turn; nothing opens it.
- 🟢 Fixed Cmd+click on URLs in fullscreen mode in Ghostty on macOS; the check requires macOS, so it never applies in this build.

</details>

---

### 2.1.185 ◆◆◆◆

- 🔴 Turned on the `thinking-token-count-2026-05-13` beta for first-party requests to models with interleaved thinking, where a remote switch that's off by default used to decide, and stopped sending it through Claude Platform on AWS or Foundry; with it, Claude Code takes thinking-token counts from the API instead of estimating them from the thinking text.
- 🟢 Changed the stalled-stream hint to appear after 20 seconds without data instead of 10, reading "Waiting for API response · will retry in …" instead of "No response from API · Retrying in …".

---

### 2.1.183 ◆◆◆◆

- 🔴 Removed the `/migrate` command added in 2.1.182, and with it `claude migrate`: it imported MCP servers, a default permission mode, subagents, skills, slash commands and `AGENTS.md` or `GEMINI.md` instructions from OpenAI Codex and Gemini CLI configs. `/init` no longer tells Claude to offer that import when it finds those configs.

---

### 2.1.177 ◆◆◆◆

- 🔴 Added a remote per-model block: when Anthropic's server-side config gives a model a block message, most requests to it, including the conversation's and subagents', stop before reaching the API and show that message as a rate-limit error, and `/model` lists the model as disabled with the config's hint. The config is empty by default.
- 🔴 Limited the "Meet Fable 5" startup announcement ("Fable 5 is here!" when you're already on it) to the Anthropic API, so users of other providers who have Fable set up no longer see it, and stopped showing it when its remote switch has no value, which used to let it show.

---

### 2.1.174 ◆◆◆◆

- 🔴 Added a Projects tool for sessions started inside a claude.ai Project: Claude gets the project's name, instructions and document list in its context and can read, search, write and delete the project's documents (new bare file names go under `claude/`, and a write that would push the project past its chat-injection budget is refused unless forced). It's off unless `CLAUDE_PROJECT_TOOL` is set and the session has `CLAUDE_PROJECT_UUID`; it needs Anthropic as the provider and either a cloud session's access token or a claude.ai login (to which it adds the `user:projects:read` and `user:projects:write` scopes), and it's unavailable to HIPAA organizations and when nonessential traffic is off.
- 🔴 Turned `/design-sync` and its DesignSync tool on for every session using the Anthropic API as its provider (rather than Bedrock, Vertex, Foundry or Claude Platform on AWS), where it used to need `CLAUDE_CODE_ENABLE_DESIGN_SYNC` (which now does nothing) or a remote switch. An organization policy that disallows design sync still keeps it off, and turning off nonessential traffic now keeps it off too.
- 🔴 Added a capability order to the advisor tool (Haiku 4.5, then Sonnet 4.6, Opus 4.6, Opus 4.7 and 4.8, then Mythos 5 and Fable 5): an advisor less capable than the main model is no longer used for the main model (subagents may still use it), `/advisor` and `--advisor` warn about such a pairing, and the advisor's startup notice says it won't activate. A request whose own model outranks the advisor is sent without it.
- 🔴 Made `/advisor <model>` refuse models that can't be an advisor and list the valid ones, and made `CLAUDE_CODE_ENABLE_EXPERIMENTAL_ADVISOR_TOOL`, which already turned the advisor tool on, also accept any main model and any advisor model that `availableModels` allows, skipping the capability check for the main model. `--advisor` is now always accepted, and ignored where the advisor tool isn't available (it used to be an unknown option there); in background sessions an unusable `--advisor` model is logged instead of stopping the session.
- 🔴 Stopped applying the interactive model picker's Fable 5 and Mythos 5 availability checks when choosing an advisor in non-interactive sessions (print mode, the SDK), with or without the experimental variable; `availableModels` still applies.
- 🔴 Added a clear error for the API refusing an advisor pairing ("cannot be used as an advisor when the request model is …"): it says the configured advisor isn't compatible with the request's model and points to `/advisor`, or to the `advisorModel` setting and `--advisor` in non-interactive sessions. Such a refusal no longer switches the turn to the fallback model, and isn't retried as if the server-side fallback's credit header had caused it.
- 🔴 Added a token refresh and one retry when a claude.ai organization request (extra usage billing setup, admin requests, the Projects tool) gets HTTP 401; if the refresh token itself has stopped working, the request fails with "OAuth refresh token is no longer valid; run /login to re-authenticate".
- 🔴 Changed model fallback notices to a single "Switched to …" message for every reason, shown in bold only when the original model is unavailable; after a non-retryable error it now ends with the first 300 characters of that error, and SDK `model_fallback` messages gained the `server_error` and `last_resort` triggers.
- 🔴 Added the model a subagent actually ran on (`resolvedModel`) to Agent tool results and progress; an agent row that names a model now shows that one instead of the requested model, when it differs from the main model or from the request.
- 🔴 Changed agent teams to send commands to tmux panes and windows as literal text followed by a separate Enter, to refuse commands that contain control characters in tmux and iTerm2, and to refuse agent and team names that contain control characters for every kind of teammate.
- 🔴 Added recaps for cloud sessions: after a turn, Claude Code writes a recap of under 40 words to the session's metadata and clears it when the next turn starts. It's off unless `CLAUDE_CODE_ENABLE_REMOTE_RECAP` is set or a remote switch that's off by default turns it on.
- 🔴 Restricted the Agent SDK's `update_environment_variables` message to `CLAUDE_CODE_SESSION_ACCESS_TOKEN` and `CLAUDE_CODE_OAUTH_TOKEN`; it used to set any variable, and other names are now refused and logged.
- 🔴 Applied per-tool permission settings (`toolPermissions`: `allow`, `ask` or `blocked`) to HTTP and SSE MCP servers as well as claude.ai connectors, and turned an organization's per-tool ceiling (`org_max_permission`) on remote servers sent through the SDK's `mcp_set_servers` into those settings, so an `ask` or `blocked` ceiling becomes that tool's setting.
- 🔴 Softened two rules in auto mode's classifier prompt: a high-severity action counts as user-intended when the request describes it unambiguously, even if not every word is spelled out (it used to have to describe that operation directly), and the user must have clearly given its target and key parameters rather than explicitly giving all of them.
- 🔴 Stopped user messages from posing as turn boundaries in the transcript auto mode's classifier reads: carriage returns, Unicode line and paragraph separators and form feeds are turned into newlines before quoted lines are indented, and the prompt now says indented `User:` or `Assistant:` lines are quoted content, never consent.
- 🔴 Changed the settings schema to accept any string in `$schema`; only the schemastore URL for Claude Code settings used to validate.
- 🔴 Fixed Bash permission checks assuming a variable kept a known literal value after an `if` or `while` condition whose `&&` may have changed or skipped its assignment; such commands now ask for approval.
- 🔴 Fixed `/**/` in wildcard permission patterns (`Bash` rules with `*` and tools' path patterns) requiring at least one directory, so `src/**/a.ts` now also matches `src/a.ts`.
- 🔴 Tightened approvals for edits inside `.claude` folders: a `.claude` folder nested inside an exempt one in the project (for example under `.claude/skills/` or `.claude/worktrees/`) no longer gets its own `skills`, `agents`, `commands` or `worktrees` exemption, and session-wide approvals for `.claude` edits and the "always allow" suggestion no longer cover paths with a nested `.claude` folder or suspicious paths such as network locations outside the trusted network directories.
- 🔴 Made sensitive-file checks strip invisible Unicode marks, colon suffixes and trailing dots or spaces from folder and file names before matching them, for every protected name (`.git`, `.vscode`, `.cargo`, shell startup files, `.npmrc` and others) as well as `.claude`.
- 🔴 Made background job state files readable only by their owner (mode 0600), and made atomic file writes that fall back to copying keep the file mode they were asked for.
- 🔴 Made the `claude agents` and background-session screens read keys from `/dev/tty` when standard input is piped or redirected, as the main interactive screen does.
- 🔴 Added a notice when fast mode is turned on with Opus 4.6 as the model, from the `/fast` picker, `/fast on` or Remote Control: "Opus 4.6 fast mode is deprecated and will be removed on" a date that a remote setting controls (June 29, 2026 unless changed); it stops after that date.
- 🔴 Turned on tool search for Claude Haiku 4.5: by default only Claude 3.5 Haiku and Claude 3 Haiku are now excluded, where every Haiku model was, though a remote list can still override it.
- 🔴 Removed `CLAUDE_CODE_VELVET_FALCON` and its server-sent model setting, which could give other models Fable 5's model-specific handling, such as its system prompt sections; now only Fable 5 and Mythos 5 get it.
- 🔴 Added `CLAUDE_CODE_HIDE_SETTINGS_HINT` to drop the suggestion to add a Bash permission rule to settings from the message Claude gets when auto mode's classifier denies an action; the suggestion is also left out under the `claude_in_slack`, `remote_trigger`, `remote_cowork` and `remote_baku` entrypoints.
- 🔴 Made skills turned off with `skillOverrides` refuse to run when typed as a command: `/<skill>` now says the skill is disabled via `skillOverrides` and how to turn it back on (in `/skills` or settings), and disabled skills are left out of "did you mean" suggestions.
- 🔴 Fixed `/usage` treating an error returned with a success status as empty usage data on claude.ai subscriptions: it now shows the limits taken from recent response headers when it has them, and otherwise says the data is unavailable, mentioning when the error was a rate limit.
- 🔴 Fixed an error thrown while opening the voice connection going unhandled: voice mode now shows "Voice connection failed. Check your network and try again." and returns to idle.
- 🔴 Added a `get_usage` request to Remote Control, so a connected client can ask a session for the usage data `/usage` shows.
- 🔴 Changed Remote Control to prepare and queue incoming prompts in the order they arrive, so a prompt that takes longer to prepare, such as one with attachments, can no longer be overtaken by a later one; commands that run immediately still don't wait for queued prompts.
- 🔴 Changed the egress gateway that cloud sessions use for outbound connections to reuse up to four idle connections (for at most 10 seconds idle and 45 minutes in all), to fall back to a fresh connection when a reused one fails before answering, and to negotiate a newer tunnel protocol that marks the end of each request, which reuse depends on; more failures before a response now answer HTTP 502.
- 🔴 Made cloud sessions confirm that Claude was told about background tasks lost in a worker restart: those tasks are now marked stopped ("Stopped by a worker restart: …"), and if delivery of the notice isn't confirmed within 20 seconds it's kept and sent again after the next restart.
- 🔴 Added `CLAUDE_STAGE_FILE_ROOT` to choose where the SDK's `stage_file` request puts files (an absolute path; `/mnt/user-data/uploads` by default).
- 🔴 Made `--input-format=stream-json` stop with "Error: --input-format=stream-json requires --print." when the session would otherwise start interactively (no `--print` and output going to a terminal).
- 🔴 Added a marketplace refresh to background plugin installation: when enabled plugins are missing from the local catalog of a marketplace that auto-updates (by default only Anthropic's official ones), Claude Code refreshes that marketplace after a random delay of up to 5 seconds and reloads plugins if they turn up.
- 🔴 Changed inline JSON given to `--settings` to be parsed and written out again, with C1 control characters escaped, before it's saved as a temporary settings file.
- 🔴 Added a cleaner retry for malformed tool calls behind a remote switch that's off by default: the broken response is removed from the conversation and Claude is asked to retry the tool call, instead of the response staying in place with a note that it couldn't be parsed.
- 🔴 Changed the Fable 5 announcement to say "Switch anytime with /model" instead of "Try anytime with /model".
- 🔴 Changed the `xhigh` effort description, and the error for models without `xhigh`, to name "Fable 5, Opus 4.7+" instead of "Fable 5, Opus 4.8/4.7 only".
- 🔴 Turned on by default the skipping of the MCP initialize handshake for stateless claude.ai connectors: Claude Code uses the connector's cached initialize response and sends its own capabilities in request headers instead; a remote switch can still turn it off.
- 🔴 Made a network failure while uploading an Artifact end like any failed upload, with an "upload failed: …" error instead of a generic request error.
- 🔴 Changed the bundled `/design-sync` skill: a new step before upload writes a conventions file (`.design-sync/conventions.md`, attached with the new `readmeHeader` config key) telling the design agent that reads the generated README how to wrap and style the components. It also gained `--skip-dts` for faster fix-loop builds (uploading such a build fails with `[DTS_STUBBED]`), a re-sync that skips the render check when there's nothing to upload and samples it when only files that don't affect rendering changed, `hypothesis:` lines under build errors, support for Vite's `import.meta.env`, and sampling on first syncs that lets a warning-free component's matching primary story stand for its other stories, while the solo set and components with portals, theme or provider sensitivity, authored previews or warnings still have every story graded. Its workflow changed too: design systems with more than 20 storied components are captured in batches of 5 to 8 with at most four browser runs at once, workers stop at one config-level failure or the same root cause in two components so the coordinator fixes it before the next wave (and removes workarounds that would hide the fix), headless (`-p`) sessions are told to run long commands in the foreground because completion notices don't resume them, first syncs also end with a final driver run, known config fixes from pending sheets are combined into one rebuild, a compare run that crashes or runs out of memory has its finished sheets graded and is then rerun without `--force` (which would clear those grades), and the initial four-component trial must include a component whose stories load remote images, so missing assets that blank both comparison panels can't pass as a match.
- 🔴 Changed what makes Design Sync grade previews again or upload: its grading hashes follow the authored previews in `.design-sync/previews/` (edits to generated cache previews no longer count), ignore `tsconfig` changes and count any added, edited or deleted `.mjs` converter fork, even an unused one; a targeted preview rebuild now updates the rebuilt component's saved source hash from its authored preview and saved config instead of clearing it (story-file edits still need a full build); and a newer grading-hash format requests an upload even when the design files match, so the project's `_ds_sync.json` is brought up to date.
- 🔴 Made Design Sync's checks stricter: validation fails when Chromium or the render check throws, instead of logging that the check was skipped and passing; `--no-render-check` now skips the check even when Playwright is installed and removes results left by an earlier run; the driver fails while `.design-sync/learnings/` holds Markdown files not yet folded into the project notes, listing them in `learningsUnmerged` and leaving the changed-Storybook check for the next run; and generated type declarations leave out large generated styling-property lists, including ones defined inside the design system itself, while keeping the file that defines the component's own Props.
- 🔴 Changed the bundled `claude-api` skill further: adaptive thinking examples opt in to `display: "summarized"`, refusal categories gain `frontier_llm`, Fable 5 is described as using Opus 4.8's tokenizer rather than about 30% more tokens, and its fallback-credit guidance now covers Claude Platform on AWS and Microsoft Foundry, sending Foundry users to client-side fallback middleware instead of server-side fallback. Its server-side fallback example now reports success only when a fallback ran and didn't also end in a refusal, its notes say `allowed_fallback_models` comes with the `server-side-fallback-2026-06-01` beta header, and they describe `recommended_model` as a suggestion that can be empty when a rate-limited or overloaded fallback couldn't be tried.
- 🔴 Changed the design sign-in errors to tell you to pick "Claude account with subscription" in `/login`, since an Anthropic Console account doesn't provide claude.ai credentials.
- 🟡 (2.1.154) Changed the bundled `claude-api` skill's C#, Go and Java examples to use Claude Opus 4.8.
- 🟡 (2.1.280) Added a remote switch, off by default, that lets background plugin auto-updates use git's credential helper.
- 🟡 (2.1.274) Added version tracking to Artifact publishing behind a remote switch that's off by default: a republish names the version it started from, and if another session has published a newer one, Claude is told to re-read the artifact, reconcile its edits and publish again.
- 🟢 Added the `wheelScrollAccelerationEnabled` setting (on by default) to turn off mouse-wheel scroll acceleration in fullscreen mode.
- 🟢 Fixed `/model` hiding the model family that Default resolves to: an Opus or Sonnet row is now added when it's missing, and a Sonnet pinned with `ANTHROPIC_DEFAULT_SONNET_MODEL` is named in that row instead of a fixed Sonnet version. For claude.ai subscribers, the 1M-context Opus row is now labelled "Opus", and "Opus (1M context)" only when plain Opus is listed beside it.
- 🟢 Stopped showing the "Fable 5 is now consuming usage credits" warning to accounts billed by usage, and stopped reporting `overageInUse` in their rate-limit events to SDK hosts.
- 🟢 Fixed Bedrock GovCloud regions (`us-gov-*`) getting the `global` inference profile prefix; they now get `us-gov`.
- 🟢 Fixed background sessions (`claude --bg`, `claude agents`) taking model, provider, endpoint and header variables from the shell that started the background daemon: the model and provider variables of the terminal that launches a session are now saved with it, and the daemon's own are dropped; with host-managed authentication the daemon's API keys and tokens aren't passed on either.
- 🟢 Fixed exiting soon after a shell command was interrupted or killed pausing for over a second: stopping a command now finishes as soon as its process group has exited (checked every 100 ms, with SIGKILL after 1.5 seconds).
- 🟢 Fixed the `Co-Authored-By` commit trailer's model name: a model Claude Code can't identify now gives "Claude" instead of "Claude (<model id>)", and a custom `ANTHROPIC_DEFAULT_FABLE_MODEL` gives "Claude Fable 5".
- 🟢 Fixed the `/advisor` dialog pre-selecting a saved advisor model that can't be used, such as one outside `availableModels`.
- 🟢 Fixed skill hot-reload sending Claude every skill's listing again when one skill changed: only skills whose description or "when to use" text changed are listed again.
- 🟢 Fixed Workflow `agent()` subagents running without their own agent identity (agent ID, parent agent, depth, subagent name), which per-agent attribution relies on.
- 🟢 Fixed pre-warmed background workers failing with "Could not resolve authentication method" when claimed after sitting idle: a claimed worker now clears its cached sign-in state, including tokens passed through file descriptors.

<details>
<summary>9 internal changes</summary>

- 🔴 Changed session forks to copy the plan file with a filesystem copy instead of reading and rewriting its bytes, so the copy keeps the source file's permissions rather than the process's default.
- 🔴 Added failure codes to voice connection telemetry (a rejected upgrade, a Cloudflare block, the WebSocket close or error code) and, when a failure has no code, a quick reachability check of Anthropic's API recorded with it.
- 🔴 Added whether a malformed tool call's text contains a leaked `<antml:invoke` tag, and whether the cleaner retry is on, to `tengu_malformed_tool_use_response`, and added the `tengu_malformed_tool_use_retry_outcome` event.
- 🔴 Added an `http_<status>_no_retry` reason to auto mode classifier error telemetry when the API says not to retry.
- 🔴 Made `claude mcp` subcommands wait for their analytics events to be sent before exiting.
- 🔴 Changed the copies of SDK messages written to stdout when `CLAUDE_CODE_TEE_SDK_STDOUT` is set: results now also leave out permission denials, structured output, deferred tool calls and errors (they already left out their text), task start, update and notification events are now copied with only their IDs, type and status, and an assistant message that schedules a wakeup keeps its delay.
- 🔴 Changed failures of the background session control server and manager to bind their socket to be logged as warnings instead of reported as errors.
- 🔴 Kept a queued message's `later` priority (which `/loop` prompts use) when the message goes back into the queue because another turn started at the same moment.
- 🔴 Stopped treating `--pool`, which isn't an option in either build, as a cloud-session flag in the `--bg` and `--cloud` argument checks, so `claude --bg --pool …` no longer gets advice to use `--pool` without `--bg`.

</details>

<details>
<summary>14 changes with no effect in this build</summary>

- 🔴 Removed the registration of the `CLAUDE_CODE_ARTIFACT_MCP` environment variable, which nothing in 2.1.173 read.
- 🔴 Made the pill-style progress bar treat a negative or infinite filled or empty length as zero. Its only caller draws it 8 to 40 columns wide with the ratio held between 0 and 1, so those lengths can't occur.
- 🔴 Added a condition that would leave out the initial message when `claude --cloud` starts a new cloud session; the flag it needs is never set in this build.
- 🔴 Made the interactive prompt queue stop treating a message from another session as a slash command when it's marked to skip them; nothing in this build creates such messages.
- 🔴 Added an optional file mode to the storage layer's exclusive-write method; none of its three callers passes one.
- 🔴 Added queue handling for human turns relayed from Claude in Slack (queued with `later` priority and marked verified, with the queue tracking which batch is running), behind a check that always returns false in this build.
- 🔴 Added an ingest path (`inbound_origin`) to the origin of messages from other sessions, so that auto mode's classifier would leave out messages relayed from a Slack bot and queued messages that arrived different ways wouldn't be batched together; nothing in this build sets it on a message's origin.
- 🔴 Added terminal turn marks (OSC 133 prompt-start and command-done sequences) around each turn, behind a check that always returns false in this build.
- 🔴 Changed AskUserQuestion's screen-reader layout to offer "Chat about this" as a list option, and dropped a space from the "selected," label of agent list rows; screen-reader mode can't be turned on in this build.
- 🔴 Added a Git Bash note to the Bash tool description and shortened the PowerShell shell descriptions in the system prompt, all for Windows; this Linux build never reports Windows.
- 🔴 Added a hidden, always-disabled placeholder command, and a command category entry for a `simulate-usage` command that doesn't exist.
- 🔴 Added methods for reading part of a file or its tail to the internal file storage; nothing calls them.
- 🔴 Added a helper that removes the server-side fallback's credit code from error text; nothing calls it.
- 🔴 Added a check for the Claude in Slack entrypoint; nothing calls it.

</details>

---

### 2.1.173 ◆◆◆◆

- 🔴 Changed the notices shown when a model's safety measures stop a response (the "Switched to …" notice, the prompt offering another model, and the "can't respond to this request" message) to mention cybersecurity or biology topics only when the API reports one of those categories. Other refusals, and the notice shown when the server itself switches models, now say the model's measures flagged something in the session, which sometimes happens with safe, normal conversations.
- 🔴 Changed refusal analytics events to report a refusal's category only as `cyber`, `bio` or `other`; they used to send the API's category name as it came, except that a refusal whose offered model switch wasn't taken reported only `cyber` or `bio`.
- 🟢 Fixed Fable model names with a `[1m]` tag keeping it on the Anthropic API, where Fable 5 already has a 1M context window: `fable[1m]`, a full Fable model name with `[1m]`, and an `ANTHROPIC_DEFAULT_FABLE_MODEL` ending in `[1m]` now resolve without the tag, so the 1M-context beta header isn't sent. Other providers and a custom `ANTHROPIC_BASE_URL` keep the tag.

<details>
<summary>1 change with no effect in this build</summary>

- 🟢 Stopped treating Windows as a platform the Bash sandbox supports, which ends the sandbox dependency warning there; this Linux build's platform check can only report Linux, WSL or unknown, so the new Windows test is never true here.

</details>

---

### 2.1.170 ◆◆◆◆

- 🔴 Turned on automatic model switching after a flagged message by default (it needed a remote switch that's off by default) and extended it from internal early-access models to Fable: when Fable 5's safety measures stop a response, Claude Code retries it on Opus (the default Opus model on the Anthropic API, Claude Platform on AWS and gateways; `ANTHROPIC_DEFAULT_OPUS_MODEL` or an Opus 4.8 model on other providers). Mythos models get no switch target, `CLAUDE_CODE_DISABLE_REFUSAL_FALLBACK` still turns it off, the `/config` toggle "Switch models when a message is flagged" now shows by default (with it off, Claude Code asks first), and on Bedrock, Vertex, Foundry and Mantle the prompt explains which two variables enable the switch.
- 🔴 Reworded the notices shown when a model's safety measures flag a message: they now always name cybersecurity or biology topics (before, "sensitive topics" unless the API reported a cyber or bio category) and add that the measures may flag safe, normal content and let Anthropic offer Mythos-level capability sooner. The "can't respond to this request" error no longer quotes the API's explanation, and the banner about a server-side switch now also shows when the switch is only reported as a response ends in a refusal.
- 🔴 Made tool calls from a response that the API takes back in a server-side model switch stop at whichever step they've reached (input check, permission prompt, before running, or a running call that fails); an open permission prompt no longer counts as a denial, and the calls' in-progress markers are cleared.
- 🔴 Made `/model <name>` on the Anthropic API (with the default base URL) check the account's server-sent model list: a model it marks disabled is refused with "Model '…' is not currently available for your account" and the server's reason, and Fable 5, when neither the list nor `ANTHROPIC_DEFAULT_FABLE_MODEL` makes it available, is first tried against the API and refused with "Fable 5 isn't available for your account yet. Run /model to pick another model." if the API doesn't know it. On Bedrock, Vertex, Foundry and Mantle, a Fable 5 model that isn't found now suggests `ANTHROPIC_DEFAULT_OPUS_MODEL` or Opus 4.8 instead.
- 🔴 Changed the `best` model alias to pick Fable 5 when it's available to the account (or `ANTHROPIC_DEFAULT_FABLE_MODEL` is set) and any `availableModels` allowlist allows it; otherwise it still means the default Opus model. On the Anthropic API with its default base URL, a Fable 5 row the server marks as disabled rules Fable out even when the variable is set.
- 🔴 Changed how 429s about usage credits are handled: a subscriber on a Fable model whose request needs usage credits now sees "API Error: Fable 5 requires usage credits. Update Claude Code to the latest version to learn more", and 429s saying usage credits or extra usage are required are no longer retried unless the API gives `fetch_error` or `org_level_disabled_until` as the reason extra usage is off.
- 🔴 Added a warning for subscribers on a Fable model when the API reports usage credits in use (header `anthropic-ratelimit-unified-overage-in-use`): "Fable 5 is now consuming usage credits instead of your plan limits…". The SDK's rate-limit info gains an `overageInUse` field.
- 🔴 Extended the remote overload switch that stops Opus requests from users without a Claude subscription to Fable 5 and Mythos 5: Fable 5 requests now stop with "We are experiencing high demand for Fable 5." and a hint to switch to the default Sonnet with `/model` (Mythos 5 gets the Opus message), and repeated overload errors on either model stop early or move to `--fallback-model`, as they do for Opus.
- 🔴 Replaced the Opus 4.8 launch notice (Anthropic API only, shown at most eight times) with a Fable 5 one, "Fable 5 is here! Our newest model for complex, long-running work" on Fable 5 and "Meet Fable 5 … Try anytime with /model." otherwise, shown while Fable 5 is available unless a remote setting turns it off, with no count of showings. On the Anthropic API it adds "Included in your plan limits until <date>" (or "for a limited time") and "then switch to usage credits to continue" until that date.
- 🔴 Started Fable 5 (and the `ANTHROPIC_DEFAULT_FABLE_MODEL` model) at `high` effort, ignoring a saved effort level until you pick one in `/effort` or a picker; until then, sessions Claude Code relaunches or sends to the background aren't passed `--effort`, even if you had picked an effort for Opus 4.7 or 4.8.
- 🔴 Added a warning to the `max` effort level, in its description and under it in the `/effort` and `/model` pickers, that it may use excessive tokens, respond slowly or overthink and is best kept for the hardest tasks; `/effort`'s help and the `xhigh` description and error now list Fable 5 among the models that support `xhigh` and `max`.
- 🔴 Gave Fable 5 and Mythos 5 sessions the system-prompt sections written for autonomous work: "Communicating with the user" (lead with the outcome, readable full sentences over terse fragments), a note that the user isn't watching and reversible steps shouldn't wait for permission (behind a remote switch that's on by default), and ending the turn when the only tool call reschedules a running dynamic `/loop`, a rule that no model could reach before. They also get the shorter system prompt on every provider.
- 🔴 Changed the system prompt's line about current models to name Fable 5 alongside the Claude 4.X family, with `claude-fable-5` first in its list of model IDs, and gave Fable 5 and Mythos 5 a January 2026 knowledge cutoff.
- 🔴 Moved auto mode's permission classifier in Fable 5 and Mythos 5 sessions to Opus 4.8 (or `ANTHROPIC_DEFAULT_OPUS_MODEL`, with `[1m]` when the session has a 1M context window) unless Anthropic's remote config names a classifier model; the background agent namer and agent-state classifier do the same when their remote config stops them using the small model.
- 🔴 Limited the `/model` rows on Bedrock, Vertex, Foundry and Mantle to models the provider has an ID for or `modelOverrides` maps, which removes the Sonnet 4.6, Opus 4.1 and Opus 4.6 rows on Mantle.
- 🔴 Changed how `/model` lists the extra models Anthropic's server sends: only on the Anthropic API or a gateway (other providers got them too), disabled ones only with the default base URL or on a gateway, placed last with " (disabled)" after the name, duplicates dropped, and each recognized, enabled one described with its display name, its price per Mtok outside subscriptions (unless the server's description already gives a price) and, for Fable 5 on a subscription unless a remote setting hides rate-limit notes, "Uses your limits ~2× faster than Opus"; unknown models and disabled rows keep the server's description. It also ignores a cached model list that isn't an array and skips rows with an invalid model value, label or description, so a damaged cache no longer breaks `/model`. A current model missing from the list now sits before the disabled rows, the picker highlights "Fable 5" and "Mythos 5" in descriptions where it highlighted "Opus 4.8", and gateway model discovery (`CLAUDE_CODE_ENABLE_GATEWAY_MODEL_DISCOVERY`) keeps a gateway's Fable 5.
- 🔴 Let `ANTHROPIC_DEFAULT_OPUS_MODEL`, `ANTHROPIC_DEFAULT_SONNET_MODEL` and `ANTHROPIC_DEFAULT_HAIKU_MODEL` (with their `_NAME` and `_DESCRIPTION`) replace the built-in `/model` rows on Claude Platform on AWS too, as they already did on Bedrock, Vertex, Foundry, Mantle and with a custom base URL.
- 🔴 Reworded the model descriptions: Opus is now "Best for everyday, complex tasks" (was "Most capable for complex work") and Sonnet "Efficient for routine tasks" (was "Best for everyday tasks"), the agent creation wizard uses these and "Fastest for quick answers" for Haiku, and the `/model` tip now reads "Fable for the hardest problems, Opus for complex work, Sonnet for most tasks, Haiku for quick questions".
- 🔴 Changed the default Opus model on Mantle to Opus 4.7; it resolved to Opus 4.6, which has no Mantle ID.
- 🔴 Changed the name in the `Co-Authored-By:` commit trailer for a model Claude Code can't name from "Claude Opus 4.8" to "Claude Fable 5"; commit and PR attribution also name Fable 5 and Mythos 5.
- 🔴 Changed the fast-mode price `/fast` shows on an Opus model fast mode doesn't support (such as Opus 4.5) to Opus 4.8's, the model it switches to.
- 🔴 Sized the conversation sent to prompt-type Stop hooks for a 1M context window instead of 200,000 tokens (by default half of it, so up to 500,000 tokens instead of 100,000) when the hook's model has that window without a `[1m]` tag: Fable 5, Mythos 5, Opus 4.7 or Opus 4.8 on the Anthropic API with its default base URL, Claude Platform on AWS or Mantle.
- 🔴 Fixed plugins enabled only in an `--add-dir` directory's `.claude/settings.json` being left out at startup: the plugin load that starts before the session ran before those directories were recorded, and the session reused its result.
- 🔴 Made logging out clear the cached server-sent models, model prices and account client data, and logging in fetch them again right away.
- 🔴 Changed the `fallback-credit` beta header sent with refusal fallbacks from `fallback-credit-2026-06-09` to `fallback-credit-2026-06-01`, and sent it on every provider; it was limited to the Anthropic API with its default base URL.
- 🔴 Changed the weekly-limit hint for Pro subscribers on a Fable model to "try /model opus · more runway".
- 🔴 Added `DISABLE_PROMPT_CACHING_FABLE` (Fable 5 and the `ANTHROPIC_DEFAULT_FABLE_MODEL` model) and `DISABLE_PROMPT_CACHING_MYTHOS` (Mythos 5); the startup warning about disabled prompt caching also names the Fable one.
- 🔴 Updated the bundled Claude API skill for Fable 5: catalog and pricing rows ($10/$50 per Mtok, 1M context, 128K output), "fable" and "most powerful" mapped to Fable 5, its thinking, effort, prefill, prompt-caching and error notes (an explicit `thinking: {type: "disabled"}` returns 400 on Fable 5), the Go constant `anthropic.ModelClaudeFable5`, and "Fable" among the skill's trigger words. It also corrected the skill's effort guidance, which offered `max` only on Opus 4.6 and later, to include Sonnet 4.6.
- 🔴 Let agents run on Fable: the Agent tool's `model` parameter accepts `fable`, the agent creation wizard offers a "Fable" row when Fable is available and always on Claude Platform on AWS, Bedrock, Vertex, Foundry and Mantle, an agent set to `fable` inside a Fable session keeps the session's model, and the `--model` help, the `update-config` skill and the agent and skill frontmatter descriptions mention `fable`.
- 🔴 Added a Fable row to the Bedrock and Vertex AI setup wizards' model step: it checks that the account can reach Fable 5 and saves a pinned pick as `ANTHROPIC_DEFAULT_FABLE_MODEL`.
- 🔴 Added a `fable()` function to the REPL tool's sandbox (`CLAUDE_CODE_REPL`); like `haiku()`, `opus()` and `sonnet()`, it calls the small fast model.
- 🔴 Grouped Fable models as `fable` in the cost breakdown by model family that `/cost` can show subscribers (behind a remote switch that's off by default), and gave them twice Opus's weight (they had the default weight, below Opus's) when Claude Code estimates from local transcripts how your usage splits across behaviours.
- 🟢 Added Claude Fable 5 (`claude-fable-5`) on every provider: the `fable` and `fable[1m]` aliases, a "Fable" row in `/model` ("Most capable for your hardest and longest-running tasks"), $10/$50 per Mtok pricing, a 1M context window by default on the Anthropic API with its default base URL, Claude Platform on AWS and Mantle, up to 128K output tokens and effort up to `max`, with `ANTHROPIC_DEFAULT_FABLE_MODEL` (plus `_NAME`, `_DESCRIPTION` and `_SUPPORTED_CAPABILITIES`) and `VERTEX_REGION_CLAUDE_FABLE_5` to set its ID and region. On the Anthropic API the row appears when the account's server-sent model list offers it; capability checks treat it like Opus 4.8 (adaptive thinking, large images, WebSearch on Vertex, the Advisor tool), thinking is never sent to it as disabled, and `claude-mythos-5` gained the same context-window, output, thinking, effort, image-size and Advisor checks, though not WebSearch on Vertex, with no alias or built-in row, and the same $10/$50 per Mtok prices for cost reporting.
- 🟢 Fixed sessions started from a shell that inherited Claude Code's variables (`CLAUDECODE` and `CLAUDE_CODE_SESSION_ID`, as in VS Code's integrated terminal) not saving their transcript or prompt history and not recording themselves as running sessions, which kept them out of `--resume`; the check and its `CLAUDE_CODE_FORCE_SESSION_PERSISTENCE` override are gone; the background service also no longer strips those two variables when it starts, so it inherits them from the session that starts it.

<details>
<summary>5 internal changes</summary>

- 🔴 Added fallback-credit analytics events, `tengu_fallback_credit_minted`, `tengu_fallback_credit_outcome` (completed, errored, aborted or dropped, per attempt) and `tengu_fallback_credit_forfeited` (a declined or cancelled refusal prompt), with request IDs, models and token counts, and gave `tengu_rotunda_pennant_credit_echoed` those fields.
- 🔴 Added the step (`phase`) and the abort kind, now including `server_fallback_tombstone`, to `tengu_tool_use_cancelled`, and a count of cleared tool calls to `tengu_rotunda_pennant_tools`.
- 🔴 Sent the VS Code extension whether refusal fallback is on, whether its `/config` toggle shows and the Fable 5 launch notice's state, in place of the remote refusal-fallback switch it got before; the disabled model rows it receives now come only with the default base URL.
- 🔴 Added the `model_refusal_fallback` system message, which Claude Code already emitted, to the SDK's message schema, and dropped internal-only wording from two schema descriptions (the list of replaced messages and `supportedDialogKinds`).
- 🔴 Replaced the launch notice's analytics event `tengu_opus48_launch_shown` with `tengu_fable5_launch_shown`, and added the `/model` failure codes `disabled_by_org`, `fable_unavailable` and `fable_probe_failed`.

</details>

<details>
<summary>3 changes with no effect in this build</summary>

- 🔴 Added a check to the Workflow tool's input validation that answers "Tool dispatch was retracted by a server fallback; the input may be truncated." when a server-side switch took back its response; the shared tool runner checks the same thing right after validation and returns its own cancelled result, so the text is never used.
- 🔴 Added a "Fable" label to the tier names of the Bedrock and Vertex model-upgrade and fallback notices; both checks only have Sonnet, Opus and Haiku tiers, so the label never shows.
- 🔴 Filled in the text of a "task continuity" system-prompt section (an agreed task's approval covers it end to end; don't announce a step without running it); the check that would include it always returns false, so no prompt gets it.

</details>

---

### 2.1.168 ◆◆◆◆

- 🔴 Stopped turning thinking off for models whose names contain `fruitcake`, `macaroon` or `mythos`: the main conversation no longer sends `thinking` as disabled to them, auto mode's classifier leaves thinking at their default and gives each of its requests 2,048 more output tokens, and the agent namer and agent-state classifier no longer turn their thinking off, which a remote switch does for other models by default.

---

### 2.1.167 ◆◆◆◆

- 🔴 Stopped the managed-settings login pins (`forceLoginOrgUUID`, `forceLoginMethod`) from refusing sessions run with `CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST`: when a host such as Claude Desktop manages the provider, the check now passes at startup and after login, and only records that a pin was skipped.
- 🔴 Stopped settings files' `env` blocks, managed settings included, from setting `HTTP_PROXY`, `HTTPS_PROXY` or `NO_PROXY` (in any capitalization) when `CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST` is on, so the host's own proxy configuration stays in force.

---

### 2.1.165 ◆◆◆◆

- 🔴 Added `cc_is_subagent=true` to the billing attribution line (`x-anthropic-billing-header: …`) that starts the system prompt of API requests made by subagents; the main session, after it's moved to the background, isn't counted as one.
- 🟡 (2.1.198) Made subagents, and compaction's own summary request (used when the cache-sharing summary is off or fails), keep the session's extended thinking setting instead of turning thinking off, but only for models on a list Anthropic's servers send with the account's client data; without such a list, only forked subagents keep it, as before.

---

### 2.1.159 ◆◆◆◆

- 🔴 Added an experimental mode behind three remote switches, off by default and optionally limited by model name: one adds the narration-summaries beta (`summarize-connector-text-2026-03-13`) to first-party, Claude Platform on AWS and Foundry requests when experimental betas are allowed, though conversation turns with thinking off or in fast mode leave it out, even when the user supplies it through `ANTHROPIC_BETAS` or the SDK; one offers `SendUserMessage` outside brief mode, with a new prompt for content the user must see word for word between tool calls, without hiding Claude's other text as brief mode does; and one turns on brief mode. `CLAUDE_CODE_PEWTER_OWL=1` turns all three on and `CLAUDE_CODE_PEWTER_OWL=0` turns them off; without it they stay off in non-interactive sessions.
- 🔴 Added the ● marker in front of messages Claude sends with `SendUserMessage` in the normal view, where they used to start after a blank indent; transcript mode already showed it.
- 🔴 Fixed a turn's ordinary text disappearing from the normal view when Claude's `SendUserMessage` call in that turn failed: only a successful call hides it now, and when the tool isn't enabled the error tells Claude to write its message as ordinary text instead.
- 🔴 Fixed `WaitForMcpServers` keeping its previous model's availability after a model switch while MCP servers are still connecting; it now follows whether the current model can use tool search.

---

### 2.1.158 ◆◆◆◆

- 🔴 Extended the automatic retry after a safety-filter refusal, which a server-side setting turns on for the default Opus model, to sessions using that model with a `[1m]` or `[2m]` context tag; the message is resent on the same model, tag kept, marked with the `x-is-refusal-fallback` header.
- 🔴 Fixed the warning that auto mode can't be used ("auto mode disabled by settings", "auto mode is unavailable for your plan" or "auto mode unavailable for this model") waiting unseen in the notification queue until something else made the notification area check it, when it came from the startup check or the re-check after a model or fast mode change; it now shows as soon as the notification area is free, and a second copy isn't queued.
- 🔴 Stopped sending the first word of each command in the Bash tool's analytics events for finished and failed commands; they now report one of 22 common command names (such as `npm`, `python`, `docker` or `curl`) or "other", as the other Bash events already did.
- 🟢 Made auto mode available on Bedrock, Mantle, Vertex, Foundry and gateway providers when `CLAUDE_CODE_ENABLE_AUTO_MODE` is set, for Opus 4.7, Opus 4.8 and model names Claude Code doesn't recognise (Opus 4.6, Sonnet and Haiku stay unsupported there). The variable counts as safe in a settings file's `env` block, so it applies before workspace trust and needs no managed-settings approval.

---

### 2.1.156 ◆◆◆◆

- 🟢 Stopped moving tool calls behind a later thinking block when a reply interleaves thinking with tool use, which made the API reject the conversation for modified thinking blocks; Claude Code now leaves such replies in order (and records the skip in telemetry), and its strip-and-retry recovery also recognises those errors when they mention `thinking` or `redacted_thinking`.

---

### 2.1.153 ◆◆◆◆

- 🔴 Changed auto-compaction outside cloud sessions to wait until a request no longer fits: compact-and-retry after the API rejects a request as too long is now on by default (it needed a remote switch and an interactive session), and the usual threshold applies only when you set `CLAUDE_CODE_AUTO_COMPACT_WINDOW` or `autoCompactWindow`, compacting through the same path. Before, only models with a 1M-token window worked this way, behind a remote switch; `/compact`, `/context` and the context warning follow the new rule, and a remote experiment can bring the threshold back in interactive sessions.
- 🔴 Changed `/compact` to apply a summary precomputed in the background when one is ready. Background precomputing itself, behind a remote switch that's off by default, now stays off in cloud sessions and no longer depends on compact-and-retry being on.
- 🔴 Changed the word that asks for a dynamic workflow from "ultrawork" to "workflow" or "workflows", and added alt+w (`chat:workflowKeywordToggle`) to ignore it for the current prompt; the "Dynamic workflow requested for this turn" notice names the key and stays 30 seconds instead of 5.
- 🔴 Added a usage warning to the dynamic workflow approval prompt ("Dynamic workflows can use a lot of tokens quickly…"), and made auto mode ask before running a dynamic workflow until you've approved one, which saves `skipWorkflowUsageWarning` to your user settings; background sessions, agent-team teammates and non-interactive runs aren't asked.
- 🔴 Reworked the dynamic workflow approval prompt: retitled "Run a dynamic workflow?", it lists the script's description and numbered phases with sample agent prompts in place of the phase diagram, renames "View workflow diagram" to "View workflow summary", passes feedback you type with Yes or No to Claude, and hides "don't ask again" when always-allow isn't permitted.
- 🔴 Added a `· from the "<name>" workflow` label to permission prompts raised by a dynamic workflow's agents, and a "Yes, and switch to auto mode" option on their Bash and tool prompts in default or accept-edits mode when auto mode is available.
- 🔴 Reworked the `/workflows` detail view: agents show queued, stopped and skipped states, and each agent card has Prompt (expand with ⏎), Activity and Outcome sections, j/k scrolling, retry attempts and how long it has been waiting or idle.
- 🔴 Changed the Workflow tool's model-selection guidance to prefer inheriting the session's model and to pick another tier only with high confidence, dropping its Haiku example.
- 🔴 Added an exhaustive-review recipe to the Workflow tool's instructions: drop findings seen in any earlier round, judge fresh findings through three different lenses with two votes needed to keep one, and stop after two rounds find nothing new; it also recommends different verifier perspectives over identical refutation prompts.
- 🔴 Removed the bundled `bughunt`, `bughunt-lite`, `plan-hunter` and `review-branch` workflows; `deep-research` is the only one left outside cloud sessions.
- 🔴 Changed the fallback after a refusal, behind a remote switch that's off by default, to move the whole session to the fallback model at the first refusal with "<model>'s safety filter flagged this message. Switched to <model> for this session."; before, it retried that turn and switched back unless the next turn was refused too. Subagents' fallbacks no longer post a notice.
- 🔴 Removed the Claude Opus 4.7 thinking reminder, a system-prompt section and per-message reminder asking the model to answer simple messages without a thinking block, which a remote switch (off by default) turned on.
- 🔴 Added `thinking_tokens` system messages with a running estimate of thinking tokens to SDK output and Remote Control, and sent the thinking-token-count beta, behind a remote switch that's off by default, from non-interactive sessions too.
- 🔴 Added a "feature of the week" usage-credit offer for Pro and Max subscribers, configured remotely and off unless Anthropic sets it: running the command it features (`/loop`, `/review` or `/security-review`) in an interactive session claims credits for your organization once and shows "<amount> in usage credits added to your account", with "Used before any purchased credits · expires in 90 days" below it.
- 🔴 Made auto mode ignore catch-all Bash and PowerShell allow rules, and Agent allow rules, every time it checks allow rules, including when it checks what accept-edits mode would allow, instead of only removing them when you switch to it; a rule whose content is only spaces or `*` now counts as catch-all.
- 🔴 Made Claude Desktop sessions ignore provider and credential variables in the `env` of project, local and managed settings, and sessions with `CLAUDE_CODE_HOST_AUTH_ENV_VAR` ignore them in every settings source and the global config, as `CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST` sessions already did.
- 🔴 Changed how organization policy-limit requests sign in: they now work with workload identity federation logins and use a cached `apiKeyHelper` key when that helper is active, and no longer fall back to a saved claude.ai login that isn't the active login method.
- 🔴 Made custom agents in `~/.claude/agents` and `.claude/agents` reload during a session when their files change, as skills and commands already did; agents passed with `--agents` stay.
- 🔴 Added plugin suggestions to the startup notices ("plugin suggestion: <plugin> · /plugin"), shown at most twice per plugin and not when `spinnerTipsEnabled` is off or `spinnerTipsOverride` replaces the default tips.
- 🔴 Changed plugin relevance hints in marketplaces: `filesRead` glob patterns, matched against files read during the session, replace the `filePath` regular expression, and a new `cwd` pattern matches the working directory, so a suggestion can show before the first turn.
- 🔴 Made `claude update` on npm installs follow the server-side version policy: it caps the update at the allowed maximum and downgrades when the policy requires it ("Downgrading to <version> … required by a server-side version policy").
- 🔴 Added a warning at the start of `/login` when `CLAUDE_CODE_OAUTH_TOKEN` is set: it will override the new login, so unset it afterwards.
- 🔴 Kept the Remote Control trusted-device enrollment when you `/login` again to the same account and organization, instead of clearing and re-enrolling it.
- 🔴 Added a message for Bash commands whose output was lost because the temp filesystem was full or out of inodes ("Command output was lost: the temp filesystem at … is full …"), suggesting `CLAUDE_CODE_TMPDIR`.
- 🔴 Added the project's `.claude/hooks` folder to the paths sandboxed commands can't write, next to `.claude/skills` and the settings files.
- 🔴 Stopped treating Bash commands that assign `TEMP` or `TMP` as read-only, so they ask for permission like assignments to other variables the shell already sets.
- 🔴 Made Bash permission checks treat a redirect target starting with `!` or `=` as shell expansion instead of a file name.
- 🔴 Made Claude Code retry with the other thinking mode when the API answers "adaptive thinking is not supported", as it already did for the longer form of that error.
- 🔴 Dropped MCP servers named `claude-in-chrome` or `computer-use` when an SDK host sets servers during a session, and stopped cloud sessions from carrying such servers over.
- 🔴 Stripped terminal escape sequences from status text, model and tool names and fallback notices received from a remote session before showing them.
- 🔴 Made the GitHub PR status follow redirects to the same host, such as a renamed repository, and stop checking GitHub directly for the rest of the session after three failed checks in a row.
- 🔴 Made project and local settings that set `subagentStatusLine` count as running commands, so the workspace-trust warning names those files as it does for `statusLine`; a new or changed `subagentStatusLine` in server-managed settings also brings up the managed-settings approval prompt in interactive sessions.
- 🔴 Changed the monthly spend-limit adjuster so the first step up from your current limit raises it by at least 10 in your currency instead of 5.
- 🔴 Moved the debug-mode, tmux-session, model-source and emergency-tip lines from the welcome banner into the startup notices below it.
- 🔴 Kept a background job's status in `claude agents` when a turn ends without any text instead of reclassifying it, and stopped mid-turn updates from overwriting a blocked status.
- 🔴 Made resuming a cloud session restore its permission mode through the normal mode switch, so a restored auto mode is set up as if you'd switched to it.
- 🟡 (2.1.274) Stopped print mode from waiting for MCP servers to connect before the first request when tool search will defer their tools, except servers set to `alwaysLoad: true`.
- 🟡 (2.1.154) Changed when dynamic workflows are available: besides `CLAUDE_CODE_WORKFLOWS`, a remote switch that's off by default can now turn them on, by default for every plan except Pro, with a new `enableWorkflows` setting and a "Dynamic workflows" switch in `/config`; organizations can block them with the `allow_workflows` policy, and `CLAUDE_CODE_WORKFLOWS` set to false turns them off.
- 🟡 (2.1.154) Renamed workflows to "dynamic workflows" throughout the interface, including `/workflows`, task labels, completion notices and save prompts.
- 🟢 Added `skipLfs` to `github` and `git` plugin marketplace sources, which sets `GIT_LFS_SKIP_SMUDGE=1` during clone, pull and fetch.
- 🟢 Added a one-time "Claude Code can't auto-update · run `/doctor`" notice after an npm global update fails for lack of write permission, and changed `/doctor`'s text for that case to "Can't auto-update: npm global folder isn't writable" with three fixes.
- 🟢 Added the last update attempt to `/doctor` ("Last update attempt: success → <version> (<date>)" or "failed (<status>)"), recorded by the npm and native updaters, and dropped its "Update permissions" line.
- 🟢 Gave status line, hook and file-suggestion commands `COLUMNS` and `LINES` with the terminal's size.
- 🟢 Changed autocomplete in `claude agents`' dispatch input to suggest built-in slash commands and bundled skills, leaving out hidden ones and those whose invocation is turned off.
- 🟢 Changed `claude agents`' PR column to show a linked `PR #N` for one PR or `N PRs` for several, with PR titles in the job's details.
- 🟢 Combined the startup "needs auth" notices for MCP servers and claude.ai connectors into one count, held back while servers are still connecting.
- 🟢 Fixed stateful MCP servers that don't offer the optional GET event stream reconnecting in a loop: a 404 from that stream no longer counts as an expired session.
- 🟢 Fixed Claude Desktop sessions with a third-party provider, and sessions with `CLAUDE_CODE_HOST_AUTH_ENV_VAR`, using your claude.ai login when `ANTHROPIC_AUTH_TOKEN` or an `ANTHROPIC_API_KEY` or `apiKeyHelper` key is set, so a gateway could get your login instead of its own token; the `forceLoginOrgUUID` error now says third-party desktop sessions should remove that setting.
- 🟢 Fixed subagent frontmatter MCP servers ignoring `--strict-mcp-config`, `--bare`, remote mode, enterprise MCP config and managed MCP allow and deny lists.
- 🟢 Stopped `--strict-mcp-config` from removing inline MCP servers of agents passed with `--agents` or the SDK, and added a notice naming subagent MCP servers that were blocked and why.
- 🟢 Fixed `claude update` on npm installs installing the newest version instead of your release channel's version; when you're already newer than the channel's version it now skips the update and says how to switch back.
- 🟢 Fixed resuming by an absolute `.jsonl` transcript path first searching session titles, which used a lot of memory with many stored sessions.
- 🟢 Fixed `claude agents` and `claude --bg` staying on a background daemon started by an older version from before binary takeover; a newer client now replaces it.
- 🟢 Fixed print mode with stream-json input not exiting when stdin closes without an end-of-file.
- 🟢 Made Claude Code exit cleanly when stdin fails (`EPIPE`, `EIO`, `ENXIO` or `EBADF`) and quietly close a failing stderr or `/dev/tty`; stdout's clean exit, which covered only `EPIPE` and `EIO`, now covers `ENXIO` and `EBADF` too.
- 🟢 Fixed `file:` links in responses with relative or `localhost` paths not opening, by resolving them against the working directory.
- 🟢 Changed `claude --help` to wrap descriptions to the terminal width, with the option column at most 36 characters wide.
- 🟢 Added MCP tool progress (its message and percentage) to collapsed tool groups.
- 🟢 Stopped the built-in `claude` subagent type from running in a temporary git worktree.
- 🟢 Changed `/background` during a response to continue that response in the background session instead of dropping it.
- 🟢 Fixed keyboard focus leaving fullscreen mode's bottom panel, where commands such as `/btw` appear, while a task is running, which made their shortcuts stop responding.
- 🟢 Stopped a background session's own job-folder files from asking for permission to read or write.
- 🟢 Fixed a background job whose working directory was deleted ending with a stack trace instead of "working directory no longer exists".
- 🟢 Made `EnterWorktree` available right away in background sessions instead of through tool search.
- 🟢 Fixed cmd+k not repainting background sessions attached from iTerm2 or Terminal.app: the check now uses the attached terminal instead of the session's own.
- 🟢 Kept the terminal cursor at the input caret after full alternate-screen repaints, which Claude Code turns on for Windows (`CLAUDE_CODE_ALT_SCREEN_FULL_REPAINT`), so IME windows appear next to the caret.
- 🟢 Fixed background colours bleeding in terminals limited to 256 colours by converting truecolor codes, and redrew cached characters when the colour level changes.
- 🟢 Fixed copying to the clipboard from an attached background session inside tmux, screen or SSH by using the attached terminal's details.
- 🟢 Stopped pre-started spare background processes from registering Remote Control sessions before they're used, which left stale entries in the Code tab.
- 🟢 Fixed a background session's name not updating right after `/rename`.
- 🟢 Changed `/model` to save your pick as the default for new sessions; `s` in the picker (keybinding `modelPicker:thisSessionOnly`, replacing `modelPicker:setAsDefault` on `d`) switches for this session only, and `/model <name>` also saves in interactive sessions.

<details>
<summary>12 internal changes</summary>

- 🔴 Added the selected job's ID and the ID used for the connection to the attach telemetry (`tengu_bg_agent_action`) that `claude agents` sends.
- 🔴 Limited error names, error codes and stack frames in telemetry and logs to identifier-shaped values.
- 🔴 Added each startup phase's start time, and the process start time, to the startup timing that cloud sessions report.
- 🔴 Renamed the agent field in cloud session events from `agent_id` to `session_agent_id`.
- 🔴 Reported the session's active goal (condition, iterations, last reason, whether it's met) in remote session metadata.
- 🔴 Added debug log lines for shutdown signals (uptime, parent process and stdin state), the end of print mode's message loop, failed reattaches and failed native updates.
- 🔴 Added the request source to permission telemetry and numbered chosen options by their position in the prompt.
- 🔴 Added an earlier prompt-cache breakpoint when a pinned fork point is also the last cacheable message in a request that skips cache writes, keeping the existing breakpoint on that message; behind a remote switch that's off by default.
- 🔴 Changed forked agents' cache fork point to the first part of the last assistant reply instead of its last part.
- 🔴 Skipped the retired `thinking_reminder` and `ultrawork_request` attachments when loading older transcripts.
- 🔴 Stopped `claude mcp add`'s interactive screen from capturing console output.
- 🔴 Renamed the workflow keyword attachment to `workflow_keyword_request` and its telemetry event to `tengu_workflow_keyword`.

</details>

<details>
<summary>9 changes with no effect in this build</summary>

- 🔴 Added `TEMP` and `TMP` exports to shell commands on Windows; the Linux build never takes that branch.
- 🔴 Added a holder for an MCP configuration warning, printed at startup and shown as a 20-second notice; nothing ever sets it.
- 🔴 Added filters that hide promotional startup notices for third-party providers and keep only one announcement; no notice is marked as either.
- 🔴 Mentioned the workload identity variables in an "API key required" error inside a branch that never runs.
- 🔴 Added a run-once function, called when `claude agents` and the main command start, that only records that it ran.
- 🔴 Added an environment check for a contained session without internet access that isn't running as root outside a deliberate sandbox; nothing calls it.
- 🔴 Added API error fields to assistant messages read from recorded test fixtures, a path that's switched off in shipped builds.
- 🟢 Changed recovery after a failed npm update on Windows to restore the original executable by copying when renaming it back fails, and, if both fail, to say where the executable was kept and how to put it back; the Linux build never takes that branch.
- 🟢 Added a `ClaudeCode.app` bundle with a microphone usage description that background sessions launch through on macOS; the Linux build never runs it.

</details>

---

### 2.1.150 ◆◆◆◆

- 🔴 Added a system-prompt section whose text comes from Anthropic's servers, taken from the `tengu_heron_brook` value in the server-supplied client data or from a remote setting of the same name; it adds nothing unless the server sends text.

<details>
<summary>2 changes with no effect in this build</summary>

- 🔴 Removed the `ant_model_override` system-prompt section, which never produced any text in this build.
- 🔴 Removed an unused function that always returned false.

</details>

---

### 2.1.148 ◆◆◆◆

- 🟢 Fixed Bash tool commands failing with exit code 127 for some users since 2.1.147: the shell snapshot again leaves out shell functions whose names start with a single underscore (two underscores are still kept), which also undoes 2.1.147's fix for aliases that call such functions.

---

### 2.1.145 ◆◆◆◆

- 🔴 Added the bundled `/run` skill, which launches and drives the project's app to show a change working; it first looks for a project skill that already covers launching, then falls back to built-in patterns for CLIs, servers, desktop and browser apps, libraries and terminal UIs.
- 🔴 Added the bundled `/run-skill-generator` skill, started only by the user, which writes or improves a project's `run-<unit>` skill under `.claude/skills/` from a template and checks the steps from a clean environment.
- 🔴 Made the bundled `/verify` skill available, as a command and for Claude to use on its own: it builds and runs the app and observes the change instead of running tests. Its files already shipped but weren't registered.
- 🔴 Added a spend-limit dialog for organizations that reach their monthly usage-credit limit with credits left: raise the monthly limit in $5 steps with ←/→ or remove it, wait for the reset, or upgrade to Max (Max 20x for Max subscribers), and the limit notice reads "You've hit your monthly spend limit" (remote switch, off by default; not for Team or Enterprise).
- 🔴 Added server-sent notes (`model_notices` in the account's client data) after the description of matching `/model` choices.
- 🔴 Changed the usage-limit message on plans with session or weekly limits: when usage credits aren't available, it names the limit reached (session, weekly, Opus or Sonnet) instead of the reason credits are unavailable.
- 🔴 Made `claude remote-control` reuse its environment when restarted in the same folder within about four hours of stopping it, so claude.ai/code sessions from the previous run reconnect; stopping it leaves the environment registered ("Environment preserved…"), and an expired one prints a warning and starts fresh.
- 🔴 Made `/color default` and Remote Control sessions fall back to the color of the session's agent definition, and the prompt border shows the agent's name when a session color is set; agent-view sessions started from an agent definition record its color.
- 🔴 Changed auto mode's reminders to Claude: one short note when auto mode turns on ("Work without stopping for clarifying questions…") replaces the full instructions and the shorter repeats sent every five turns.
- 🔴 Removed plan mode's interview phase (`CLAUDE_CODE_PLAN_MODE_INTERVIEW_PHASE` or a remote switch, off by default), with its pair-planning instructions and AskUserQuestion's "Skip interview and plan immediately" choice.
- 🔴 Changed compaction to take the summary from the summarizer reply that contains `<summary>` when there's more than one, and to send images, PDFs and notebooks in attachments as placeholders such as `[image]` in the summary request.
- 🔴 Stopped requiring saved OAuth `scopes` to be a list when deciding whether API requests can use the claude.ai login: a saved string containing `user:inference` now counts, while a number, true/false value or object makes creating the API client throw an error.
- 🔴 Fixed interrupting a ripgrep search (Grep, Glob) being reported as "Ripgrep search timed out after 20 seconds"; searches stopped by their own time limit now count as timeouts even when the process reports no signal.
- 🔴 Stopped auto-approving `lsof -i@host` and `ss` filters that name a host rather than a numeric address.
- 🔴 Stopped the PowerShell tool from auto-approving read-only commands that aren't `Verb-Noun` cmdlets or their aliases, such as `ipconfig`, `netstat`, `findstr`, `tree` and `where.exe`, when an argument is anything but a single string, number or parameter (a comma-separated list, for example), as it already did for `git`, `gh`, `docker` and `dotnet` (the tool is opt-in on Linux with `CLAUDE_CODE_USE_POWERSHELL_TOOL`).
- 🔴 Stopped auto-approving Bash commands in which the permission parser skipped text inside a redirection or a joined word, or whose redirect target is built from parts containing `$` or backticks; the parser also reads `$` before a backtick as its own token and a backslash at the end of a line as a line continuation, as the shell does.
- 🔴 Stopped a remote holdback switch from turning off the away summary (session recap) and hiding its `/config` toggle.
- 🔴 Reworded AskUserQuestion's result to Claude to "Your questions have been answered: …" with "notes:", instead of saying the user answered.
- 🔴 Stopped the session survey from suggesting `/feedback` when `DISABLE_FEEDBACK_COMMAND` or `DISABLE_BUG_COMMAND` is set.
- 🔴 Changed the native installer's error for a missing platform to "Native binaries for … are not available on this release channel", listing the platforms that version ships.
- 🔴 Changed footer keyboard navigation: moving past the last background task goes on to the next footer item, and moving back into the task list selects the last task.
- 🔴 Reworded the `/tui` switch hint to " · if you want to go back, use /tui default".
- 🔴 Made the spinner's thinking wording ("still thinking", "almost done thinking") and shading count only thinking time, leaving out time tools ran, and added a spinner mode that shows "running tool for …" or "ran tool for …" in place of the thinking text (remote switch, off by default).
- 🔴 Changed the team-artifacts tip shown while Claude works (remote switch, off by default) to appear only while there are artifacts you haven't seen, tracked in the global config, and to skip a tip with no content.
- 🟢 Added `claude agents --json`, which prints live interactive and background sessions (process ID, folder, kind, start time, session ID, name, idle, waiting or busy) and exits; `--cwd` limits it to sessions under one folder, and the no-terminal error now points to it.
- 🟢 Made the `claude agents` terminal title show how many sessions await input ("2 awaiting input · claude agents"), unless `CLAUDE_CODE_DISABLE_TERMINAL_TITLE` is set.
- 🟢 Made the prompt's command and file suggestions highlight under the mouse and accept a click in fullscreen mode, as do the agent view's skill and agent suggestions.
- 🟢 Kept the spinner and elapsed time updating after a terminal resize or refocus, rather than waiting for a keypress, by driving timers and animations from the shared clock.
- 🟢 Made push-to-talk work in the agent view: holding Space on an empty reply or prompt starts dictation (Space keeps its old action after 500 ms without recording), with a "hold space to speak" hint, and voice input returns to idle when switched off mid-recording.
- 🟢 Kept tasks created together in their creation order in the task list, by running `TaskCreate` calls one at a time.
- 🟢 Stopped "Failed to install Anthropic marketplace" from lingering once the official marketplace is installed: a successful install clears the saved failure, and an installed marketplace is reported as such.
- 🟢 Fixed the footer PR badge waiting for its next check after Claude runs `gh pr create`, `glab mr create`, `git push` or another tracked git operation; these now trigger a refresh.
- 🟢 Stopped API calls failing for Agent Teams teammates whose names have non-ASCII characters, by percent-encoding the agent and parent-agent ID request headers.
- 🟢 Made `/review` work on repositories with Classic Projects by fetching pull request details with `gh pr view --json` and a list of fields.
- 🟢 Ended a loop where a `context: fork` skill kept re-invoking itself: the Skill tool now refuses that from inside the skill's own fork and tells Claude to follow the skill's instructions.
- 🟢 Changed a whole-file Read over the token limit to return a first page (or the first characters, for very long lines) with a "PARTIAL view" notice on how to page on, instead of an error; SDK output marks it `truncatedByTokenCap`.
- 🟢 Stopped auto-approving, as read-only or by sandbox auto-allow, Bash commands with a bare assignment such as `NAME=value;` to a variable that's set in the environment commands run with (or when that can't be checked) and isn't on a safe list (`NODE_ENV`, `LANG`, `RUST_LOG`, …).
- 🟢 Added the repository (host, owner and name from the origin remote) and the current branch's open pull request (number, URL, review state) to the status line's JSON input; the status line reruns when the PR changes, and the status line setup agent describes the new fields.
- 🟢 Listed what a plugin will install on the `/plugin` Discover and Browse screens, including skills and LSP servers, from the plugin catalog when it has the list and otherwise from the marketplace entry.

<details>
<summary>14 more for administrators and for authors of plugins, hooks, MCP servers and SDK hosts</summary>

- 🔴 Changed `claude plugin validate` to warn about unknown fields in `plugin.json` and `marketplace.json` instead of failing: warnings suggest a likely spelling, point out cross-tool spellings such as `user_config`, and name manifests a field usually comes from; the new `--strict` turns warnings into failures (exit 1). Marketplace entries whose `relevance` or `experimental` value isn't an object now get a type warning: `relevance` used to fail validation, and `experimental` was ignored quietly.
- 🔴 Made a cached plugin that lacks paths its marketplace entry declares for agents, skills or commands count as stale and get copied again, unless the source lacks them too or another running session uses that cache.
- 🔴 Retried once after 250 ms, for WSL drive mounts, a plugin folder passed by path under `/mnt/` whose first check fails with an error other than "not found", and a skills folder under `/mnt/` that first reads as empty.
- 🔴 Added `CLAUDECODE=1` and `CLAUDE_CODE_SESSION_ID` to the environment of hook commands and of status line, subagent status line and file-suggestion commands, and stripped `<claude-code-hint …/>` lines from hook output.
- 🔴 Made hook input's `effort` field, and with it `$CLAUDE_EFFORT`, present whenever the model supports effort, using the model's default level when none was chosen.
- 🔴 Made the MCP OAuth callback listener answer requests for other paths with a 404 page naming the `redirect_uri` the client must register.
- 🔴 Included `resource_link` items ("[Resource link: name] uri") in the error text of failed MCP tool calls.
- 🔴 Passed server instructions from SDK-hosted MCP servers to Claude, cut at 2,048 characters with a warning.
- 🔴 Added sign-in token refresh for Claude Desktop hosts (`CLAUDE_CODE_ENTRYPOINT` of `claude-desktop`, `claude-desktop-3p` or `local-agent`): with the SDK's `getHostAuthToken` callback, a 401 makes Claude Code ask the host for a new token, put it in the variable `CLAUDE_CODE_HOST_AUTH_ENV_VAR` names (default `ANTHROPIC_AUTH_TOKEN`) and retry; `CLAUDE_CODE_HOST_AUTH_REFRESH_TIMEOUT_MS` sets the wait (30 seconds by default). The refresh applies only with a third-party provider or a custom API host and no Claude OAuth access token in use; ordinary first-party requests don't use it.
- 🔴 Updated the bundled Claude API skill's Managed Agents material: a new self-hosted sandboxes guide (`config: {type: "self_hosted"}`, `ant beta:worker`, work-queue endpoints), the `limited` network policy with `allow_package_managers` and `allow_mcp_servers`, `sessions.update()` for per-session tool and MCP changes, cache pre-warming with `max_tokens: 0`, and MCP tools guidance that now places tool execution in Anthropic's orchestration layer instead of the session container, describes offloading tool outputs over 100K tokens to files, and says invalid vault credentials don't block creating a session (it reports a `session.error` event and retries sign-in when the session next goes from idle to running).
- 🟢 Tagged `claude_code.tool` spans in OpenTelemetry traces with `agent_id` and `parent_agent_id`, and made background subagents' spans hang under the Agent tool span that launched them.
- 🟢 Added `background_tasks` and `session_crons` to `Stop` and `SubagentStop` hook input, so a hook can tell a finished session from one waiting on background work or a scheduled wake-up.
- 🟢 Made MCP prompt commands report missing required arguments by name, with a usage line, before calling the server, and split their arguments on any run of whitespace.
- 🟢 Reported a `skills` entry that names a file rather than a folder, in `claude plugin validate` and when a plugin loads, with a hint to point at the folder holding `SKILL.md`.

</details>

<details>
<summary>11 internal changes</summary>

- 🔴 Made a forked skill put the caller's previous active skill back when it finishes, so later requests' skill attribution and OpenTelemetry labels, and `spawnedBySkill` on later Agent launches, name the right skill.
- 🔴 Changed the project file-count metric (`project_file_count_rounded`) to use the count received so far when ripgrep closes without an exit code, instead of leaving the count out.
- 🔴 Sent fewer expected failures to error reporting: failed process launches, OAuth refresh network and 401/403/429 failures, a broken OAuth refresh lock, `console.error` output while the interface runs, expected tool errors (blocked domains, ripgrep timeouts, files too large, stopped tasks and others), API timeouts, rate limits, missing models and invalid keys, reactive compaction failures, Remote Control "transport closed" failures, model replies whose JSON can't be parsed, and failed writes of the `.last-cleanup` marker that carry an error code.
- 🔴 Added telemetry: extra fields on `tengu_feature_sad`, MCP `tools/list` failure codes and server details, plugin load failure component and error code, compaction reply counts and fallback details, the PR-status HTTP status, startup timings (`node_boot_ms`, `settings_load_ms`, `hooks_init_ms`, `load_initial_messages_ms`, `system_prompt_ms`, and `spawn_to_exec_ms` from `CCR_SPAWN_TIMESTAMP_MS`) and a cloud warm-spare flag.
- 🔴 Measured API request, tracing and SDK result durations (`duration_ms`, `ttft_ms`) with the monotonic clock, rounded to whole milliseconds and never negative.
- 🔴 Added debug log lines for queued commands dropped when the queue is cleared, skipped agent notifications, unrecognized keys in hook JSON output, tools missing at render time and error codes of missing plugin paths.
- 🔴 Let precomputed background compaction start from the token counts of the latest API response (remote switch, off by default).
- 🔴 Recorded which messages a compaction kept in cloud-session transcript uploads, and batched their "processing" delivery acknowledgements.
- 🔴 Passed the active `/goal` and the ultraplan session link to subagents' tool context, where `/goal` and the ultraplan checks now read them.
- 🔴 Gave the error for reading a directory the standard `EISDIR` code fields.
- 🟡 (2.1.154) Rounded the spinner text's shimmer color blend to eighths, as other spinner shading already was.

</details>

<details>
<summary>8 changes with no effect in this build</summary>

- 🔴 Added `ConnectGitHub` to the tool lists for subagent exclusion, auto-mode approval and notification labels; no tool of that name exists in this build.
- 🔴 Added a keybinding dispatcher that sends matched shortcuts as action events through focus scopes; nothing in this build declares a scope, so every key still goes to the existing handler.
- 🔴 Added exceptions to the forced-sandbox rule for sessions with `IS_SANDBOX` set or already inside bubblewrap; the setting that forces the sandbox always reads as unset in this build.
- 🔴 Added Android paths to the native installer and to the bundled SDK's binary lookup (`linux-<arch>-android`); this build never detects Android.
- 🔴 Added a function that always returns false beside the commit and PR attribution code; nothing calls it.
- 🔴 Added helpers for listing a process's child process IDs and for clearing the process-start cache in tests; nothing in this build calls either.
- 🟡 (2.1.154) Added a shell mode to the agent view's prompt (a `!` prefix) behind a check that always returns false, and put the existing, already unreachable `--exec` handling behind the same check.
- 🟢 Changed the cross-project resume hint to use `;` on Windows; this Linux build never detects Windows.

</details>

---

### 2.1.143 ◆◆◆◆

- 🔴 Let prompt-based Stop and SubagentStop hooks end the turn when their evaluator judges the condition impossible in this session; for `/goal` this also clears the goal and shows an error mark, "Goal could not be achieved" and the evaluator's reason.
- 🔴 Stopped notifying you when a plan waits for approval: plan approval now uses the same dialog as other permission prompts, which sends neither the "Claude Code needs your approval for the plan" notification nor a `permission_prompt` Notification hook event. The waiting reason each session writes to its file in `~/.claude/sessions` now reads "permission prompt" whenever any dialog is open (before: "approve ExitPlanMode" for a plan and usually "input needed" otherwise); nothing in this build displays that field.
- 🔴 Retried a streaming response up to twice, after 100 and 200 ms and with a retry notice, when its connection drops (ECONNRESET, EPIPE or closed by the server) before the first event arrives.
- 🔴 Made the Bash permission check ask before running a command with an argument that starts with `-` or `--` and contains text only known at run time (from `$(…)` or a variable), or a `cat` heredoc substitution that would make an argument start with option syntax; read-only commands also stop being approved without asking when any of their arguments comes from a command substitution or a variable.
- 🔴 Kept the remote session when a connection error switches Remote Control off, so turning Remote Control back on in the same conversation reconnects to that session (not after `/clear`); while that session is kept for reconnecting, an exit cleanup is registered to archive it.
- 🔴 Added team memory sync across several mounted memory stores, configured with `CLAUDE_MEMORY_STORES` (a JSON list of stores; read-only mounts are supported), behind the team memory remote switch, which is off by default.
- 🔴 Added `CLAUDE_ENABLE_BYTE_WATCHDOG_BEDROCK`, which applies the stalled-stream watchdog (it ends a response that stops sending bytes) to Bedrock responses; off by default.
- 🔴 Added an optional `displayName` to plugin manifests, shown in place of the plugin name in `/plugin` screens, install and enable messages, plugin command descriptions and `claude plugin details`, and matched by plugin search; channel configuration headings now fall back to the server name for a blank display name, and MCP bundle configuration headings use the bundle's display name when it has one.
- 🔴 Added spinner tips suggesting a plugin from the official marketplace when that plugin's new `relevance` signals (commands you ran, file paths, dependencies in package manifests) match your work, for anyone who has that marketplace added.
- 🔴 Added a notice before `/autofix-pr` starts, shown until you first choose "Continue", saying auto-fix can post comments on your behalf with your GitHub identity ("Not now" cancels).
- 🔴 Extended guest passes (`/passes`, its spinner tip and its upsell) to Claude Pro subscribers; the server still decides who's eligible.
- 🔴 Made background requests that ask the model for JSON, such as session titles and names, memory selection, cloud task titles, prompt hooks and `/goal` checks, accept an answer wrapped in a Markdown code fence instead of failing to read it.
- 🔴 Changed which skill the Skill tool uses when two share a name: it now prefers one Claude may invoke over one marked `disable-model-invocation`, also when no MCP prompts are loaded. In that case a skill stub marked with the internal `fallback: true` now also gives way to a plugin or bundled skill with the same name.
- 🔴 Added an optional question, "what made you switch back?", when `/tui default` leaves fullscreen in a session that `/tui fullscreen` restarted or where a remote rollout turned fullscreen on; a typed answer is sent to Anthropic as feedback.
- 🔴 Made `/tui` say "Couldn't switch renderers — … The setting was saved; restart Claude Code to apply it." when Claude Code can't restart itself.
- 🔴 Made `/insights` save each report as `report-YYYY-MM-DD-HHMMSS.html` beside `report.html` and point to the dated copy.
- 🔴 Made the startup cleanup delete `/insights` data under `~/.claude/usage-data` (reports, facets and session metadata) older than `cleanupPeriodDays`, 30 days by default.
- 🔴 Reworded the last line of the `/feedback` consent screen to "We may use these to debug related issues and improve Claude Code."
- 🔴 Stopped `--fallback-model` failing when it names the same model as `--model`; the fallback is dropped instead.
- 🔴 Stopped a stashed prompt from replacing text you typed in the meantime when it's put back after a submission or an immediate slash command; it now returns only to an empty input box.
- 🔴 Hid the "← for agents" hint in the prompt footer while a footer item is selected.
- 🔴 Moved plan approval into its own scrolling dialog with the choices below the plan: in fullscreen mode it no longer hides the sticky prompt header or the "new messages" pill, and its question always reads "Claude has written up a plan and is ready to execute. Would you like to proceed?", where the fullscreen sticky footer used to ask only "Would you like to proceed?".
- 🔴 Made Escape keybindings respect Alt/Meta: an Escape event carrying that modifier now matches an Alt/Meta+Escape binding instead of plain Escape.
- 🔴 Made `claude agents` refuse extra positional arguments instead of ignoring them.
- 🟢 Ended a turn with a warning when a Stop or SubagentStop hook blocks it more than 8 times in a row; `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP` changes the limit (0 turns it off).
- 🟢 Skipped `/goal` evaluation when a turn ends while background shells or delegated subagents are still running.
- 🟢 Added plugin dependency checks: disabling a plugin that another enabled plugin needs is refused with a ready-to-paste `claude plugin disable … && …` command, and enabling a plugin also enables its dependencies, refusing when one is missing, blocked by your organization's policy or disabled at a higher-precedence scope.
- 🟢 Added a "Context cost" section to plugin details when browsing `/plugin`, estimating the tokens a plugin adds every turn and when invoked, marked "(estimated)" when worked out from component sizes and highlighted from 2,000 every-turn tokens.
- 🟢 Added `worktree.bgIsolation: "none"` (also read from `CLAUDE_BG_ISOLATION`), which lets background sessions edit the working copy directly instead of requiring `EnterWorktree` first.
- 🟢 Made the PowerShell tool start PowerShell with `-ExecutionPolicy Bypass` unless `CLAUDE_CODE_POWERSHELL_RESPECT_EXECUTION_POLICY` is set; on Linux the tool is used only when `CLAUDE_CODE_USE_POWERSHELL_TOOL` turns it on.
- 🟢 Stopped deleting a worktree folder with `rm -rf` when `git worktree remove` fails: the folder is kept, and exiting a worktree or deleting a background job now says where it was kept.
- 🟢 Made a `.credentials.json` whose `scopes` isn't a list count as having no scopes; before, a text value was searched as text and an object or number threw an error that could hang startup or abort OAuth token refresh.
- 🟢 Changed the hint after 429, 529 and 5xx errors to name what to check: status.claude.com on Anthropic's own endpoint and Claude Platform on AWS, "your inference gateway (<host>)" with a custom `ANTHROPIC_BASE_URL`, and the provider's service status on Bedrock, Vertex AI, Foundry, Mantle and gateways (before, custom endpoints and gateways were sent to status.claude.com and the cloud providers got no hint).
- 🟢 Made `NO_COLOR` and `FORCE_COLOR` from the settings `env` apply only to commands Claude Code runs, no longer to its own interface colours.
- 🟢 Made Esc and Ctrl+C cancel a pending `/loop` wakeup while Claude is idle between iterations.
- 🟢 Made right-click in fullscreen mode on WSL copy the selection, or paste the clipboard when nothing is selected.
- 🟢 Put auto mode in the Shift+Tab cycle of sessions already running in auto mode, such as attached agent sessions, even where auto mode isn't otherwise offered.
- 🟢 Kept the model and effort level you set when a background session is retired and later woken.
- 🟢 Made `/bg` and ← detach pass `--settings`, `--plugin-dir`, `--add-dir`, `--mcp-config`, `--strict-mcp-config`, `--fallback-model` and `--allow-dangerously-skip-permissions` on to the background session.
- 🟢 Stopped `/bg` without a prompt sending "continue" to the background session, which now waits for input; the command shows a `[prompt]` hint, and its description now reads "Send this session to the background and free the terminal" (it was "Continue this session in the background and free the terminal").
- 🟢 Added `--add-dir`, `--plugin-dir`, `--settings`, `--mcp-config` and `--strict-mcp-config` to `claude agents` for the sessions it dispatches; `--plugin-dir` and `--settings` also apply to the agent view itself.
- 🟢 Changed `claude agents --allow-dangerously-skip-permissions` to make bypass-permissions mode available to dispatched sessions instead of starting them in it; the view's defaults line shows "bypass available".
- 🟢 Stopped background sessions launched from `claude agents` starting in auto mode, so they follow `permissions.defaultMode` from settings.
- 🟢 Made `--agent <name>` find a plugin's agent without its `plugin:` prefix.
- 🟢 Made deleting a session from the agent view also delete its transcript file.
- 🟢 Stopped background sessions reporting false worker stalls after the computer sleeps.
- 🟢 Stopped background sessions connecting to your IDE on their own, which could put IDE file references into the next prompt dispatched from `claude agents`.
- 🟢 Made the background service fall back to the running Claude Code binary when the `claude` launcher it starts is missing or not executable.

<details>
<summary>19 internal changes</summary>

- 🔴 Made WebSearch's API telemetry read the current permission context instead of the one saved when the search began, and made auto-mode classification read the current deny rules after an awaited permission check instead of an earlier copy. The mode sent to `UserPromptSubmit` hooks is also read after input processing, so a change made during image conversion, attachment loading or slash-command expansion reaches the hook's `permission_mode` field.
- 🔴 Cached successful plugin-catalog loads for the session, sharing one lookup between install counts and context-cost details; a failed download can be tried again.
- 🔴 Counted connections closed by the server (`ConnectionClosed`) as stale connections like ECONNRESET and EPIPE, so the API retry after one opens a fresh client (and the remote keep-alive fallback, off by default, applies), and shortened the matching log line.
- 🔴 Cached the asynchronous read of saved claude.ai credentials, clearing it together with the existing token cache.
- 🔴 Kept a token's real OAuth scopes when background sessions restore credentials from their snapshot or the background service refreshes them, and when cloud sessions refresh from their token file, instead of assuming `user:inference`.
- 🔴 Made exit cleanup steps run only once even when shutdown and a restart both trigger cleanup.
- 🔴 Registered removal of the session plugin cache (where ZIP, URL and inline plugins are unpacked) in every session that creates it, not only after headless plugin installs.
- 🔴 Added a remote switch (`tengu_ccr_v2_send_events_cli`, off by default) that sends Remote Control permission responses to the newer `/v1/code/sessions/…/events` endpoint.
- 🔴 Made remote SDK sessions (started with an SDK URL) print `SDKStartup:` startup phases and connection diagnostics to standard error, and widened what `CLAUDE_CODE_TEE_SDK_STDOUT` copies (the result message without its text, and the init message).
- 🔴 Made cloud workers ignore a leftover "archived" end-session request from an earlier run of the same session.
- 🔴 Stopped the team memory watcher pushing after pushes were suspended for the session, or when nothing changed.
- 🔴 Passed a new remote switch, `tengu_vellum_siding` (off by default), to the VS Code extension.
- 🔴 Let SDK feedback requests give `ide` as where they came from.
- 🔴 Added system error codes to the failure reasons and telemetry of background-session spawns.
- 🔴 Counted background workers respawned after an upgrade separately from dead ones in the background service's startup log and telemetry.
- 🔴 Switched spinner and animation timing to a monotonic clock started when the clock is created.
- 🔴 Drew the plan-approval dialog in the experimental scroll-region renderer (`CLAUDE_CODE_DECSTBM` or a remote switch, off by default) as the existing padded overlay, without the permission-coloured rule above it.
- 🔴 Recorded the git commit of a plugin's source folder when updating a plugin from a local marketplace.
- 🟢 Cached each process's start time for up to 60 seconds when background sessions check that their processes are alive (on Linux, fewer reads of `/proc/<pid>/stat`).

</details>

<details>
<summary>12 changes with no effect in this build</summary>

- 🔴 Added an asynchronous reader for saved plugin options and secrets, with a cache that's cleared when options change; nothing calls it.
- 🔴 Added a "share" mode to the `/feedback` dialog for posting the conversation through a shareable link; the dialog is never opened in that mode.
- 🔴 Added a "remote workflow" kind of cloud task, with its own completion text and "N remote workflows" label; nothing creates one.
- 🔴 Added pausing for workflow tasks (a pause action in the tasks dialog, a paused icon and a `paused` task status in the SDK schema); the workflow module is absent, so nothing can pause a task.
- 🔴 Added an internal `role` field (`"comms"`) to MCP server configs and the SDK server schema, copied onto MCP tools; nothing reads it.
- 🔴 Added asynchronous versions of 24 account helpers (subscription type and name, plan checks, the API key and its source, account information); nothing calls them.
- 🔴 Rewrote the bundled `verify` skill's instructions (handles branches without an upstream and uncommitted changes, asks for the scope outside a repository, adds a rule against driving destructive paths live); the skill isn't registered in this build.
- 🔴 Changed the macOS Option-key character lookups to match only their own entries; that code runs only on macOS.
- 🟡 (2.1.157) Added output-style and channel templates (an MCP server with `server.ts` and `package.json`) to the plugin scaffold, and a local example server to its `.mcp.json`; no command reaches the scaffold in this build.
- 🟢 Made background sessions on Windows start with `CLAUDE_CODE_NO_FLICKER=0` unless that variable is already set (before, only over SSH); Windows only.
- 🟢 Woke standard input on Windows when the agent view takes over again after an attached session, so the list keeps responding; Windows only.
- 🟢 Made background jobs on macOS relaunch themselves as their own responsible process, so reading files under `~/Documents`, `~/Desktop` or `~/Downloads` stops failing with "Operation not permitted"; macOS only.

</details>

---

### 2.1.138 ◆◆◆◆

- 🟡 (2.1.212) Made `CLAUDE_CODE_MID_CONVERSATION_SYSTEM`, set to part of a model's name, work behind a custom `ANTHROPIC_BASE_URL`: when it's set, sending Claude Code's reminders as mid-conversation system messages no longer requires requests to go to `api.anthropic.com`.

---

### 2.1.137 ◆◆◆◆

- 🔴 Changed nothing but the version number, build time and commit hash recorded in the build; the JavaScript, the other embedded files and the Bun runtime are identical to 2.1.136.

---

### 2.1.133 ◆◆◆◆

- 🔴 Changed SDK managed settings (`managedSettings` / `--managed-settings`) to apply only their restrictive keys even when no admin policy exists: permission rules, the `allowManaged*Only` and `strictPluginOnlyCustomization` locks, sandbox restrictions, MCP allow and deny lists and `forceLoginOrgUUID` stay, while everything else, such as `env`, hooks and `model`, is dropped.
- 🔴 Made `--managed-settings` work whatever launched Claude Code; it used to be ignored, with a note only in the debug log, unless an SDK, Claude Desktop, the local agent or the VS Code extension started it.
- 🔴 Made the startup settings-error screen and the SDK's settings-error reply include validation errors from lower-priority managed settings sources even when a higher-priority policy wins; 2.1.132 stopped collecting those sources' errors once one policy had won.
- 🔴 Stopped sending `temperature` to models Claude Code doesn't know to accept it: only Claude 3 models, Opus 4 to 4.6, Sonnet 4 to 4.6 and Haiku 4.5 get it, except that on providers other than the Anthropic API and Claude Platform on AWS a model's `ANTHROPIC_DEFAULT_*_MODEL_SUPPORTED_CAPABILITIES` or `ANTHROPIC_CUSTOM_MODEL_OPTION_SUPPORTED_CAPABILITIES` list decides; custom and unrecognized model names used to get it too.
- 🔴 Turned on fine-grained tool streaming (`eager_input_streaming`) on Google Vertex AI for Haiku 3.5, Sonnet 4 to 4.6 and Opus 4.5 to 4.7, and on Amazon Bedrock for Sonnet 4.6 and Opus 4.7, when no custom Vertex or Bedrock base URL is set. `CLAUDE_CODE_ENABLE_FINE_GRAINED_TOOL_STREAMING=1` now turns it on for any provider and endpoint, where it used to work only against the Anthropic API's own address.
- 🔴 Made Amazon Bedrock (Mantle) requests that use `AWS_BEARER_TOKEN_BEDROCK` also send the token as `Authorization: Bearer`, alongside the `X-Api-Key` header they already carried.
- 🔴 Changed subagents' newly started background shell commands: a background subagent now stays open while one runs instead of killing it when the subagent finishes, and each is stopped after an hour (`CLAUDE_SUBAGENT_BG_SHELL_MAX_MS` changes the limit). A command already tracked as a foreground task and then moved to the background, for example with Ctrl+B, gets neither change.
- 🔴 Stopped the background-agent stall watchdog (10 minutes without progress, or `CLAUDE_ASYNC_AGENT_STALL_TIMEOUT_MS`) from ending an agent while one of its tool calls is still running.
- 🔴 Added `MCP_CONNECT_TIMEOUT_MS` to set how long print and SDK sessions wait for MCP servers at startup before going on without them (5 seconds by default, as before).
- 🔴 Treated an MCP server's 404 response that mentions `mcp_session_terminated` as an expired session, the same as the standard session-expired error.
- 🔴 Made the Bash permission check treat any variable that a `for` or `while` loop body reassigns (by assignment, a nested `for`, `read`, `mapfile` or `readarray`) as unknown throughout the body, so a command early in the loop can't be approved against a value a later line changes.
- 🔴 Made Bash commands that assign `HOME` count as too complex to approve automatically, like ones that assign `PATH` or `BASH_ENV`.
- 🔴 Stopped treating `sed` commands with more than one in-place flag (`-i`, `-Ei`, `-ri`), or with a carriage return in the script, as safe to run without asking.
- 🔴 Stopped the PowerShell tool (on Linux with `CLAUDE_CODE_USE_POWERSHELL_TOOL`) from treating `git`, `gh`, `docker` and `dotnet` commands as read-only when an argument is a variable or an expression rather than plain text.
- 🔴 Widened the PowerShell tool's check for link creation with `New-Item -ItemType` to catch abbreviated, commented, curly-quoted and wildcard type names.
- 🔴 Made the PowerShell tool's guard against `Remove-Item -Recurse` on the working directory or a parent of it expand `~` and accept backslashes, so targets written like `~/project` or `..\` that point there are caught too.
- 🔴 Treated paths that start with `/\` or `\/` as network paths, so reading or editing one asks for permission, and stopped following symlinks whose target is a network-style path.
- 🔴 Stopped showing a separate "hook returned blocking error" message when a PreToolUse hook blocks a tool call; the denied tool result still carries the hook's message.
- 🔴 Added a "Plugin settings overridden" section to `/doctor` listing plugins you disabled in `~/.claude/settings.json` that project, local, `--settings` or managed settings turn back on, with advice for each; the `enabledPlugins` setting's description now explains the precedence.
- 🔴 Added an "Open agents view by default" option to `/config` (`defaultToAgentsView`) that makes a plain `claude` in a terminal open the agents view; like the agents view itself, it works only behind a remote switch that's off by default.
- 🔴 Added session colours to the agents view: a colour set with `/color` or from Remote Control is saved with the session's agents-view record, and the list shows each session's name in it.
- 🔴 Made a background session that stops at a startup dialog show as blocked ("stuck on a startup dialog", "open this session to continue setup") as soon as the dialog appears, instead of after the 45-second startup watchdog.
- 🔴 Made `claude daemon stop` mark the background sessions it ends as stopped in the agents view.
- 🔴 Made background-session commands retry up to 10 times when the daemon gives no reply (`ENOREPLY`), as they already did while it was starting.
- 🔴 Added `alt+1`-`alt+N` (up to 9) to the agents view's `?` help, for the existing shortcut that opens the sessions started from this terminal.
- 🔴 Changed recaps of an idle session to judge whether the prompt cache is still warm from the cache lifetime the last request actually used, skipping the recap when that isn't known, and to skip recaps when you're at or near a rate limit.
- 🔴 Made the cleanup of stale agent worktrees also remove ones still at the commit they were created from, even when that commit was never pushed.
- 🔴 Made print and SDK sessions handle a permission answer that arrives after its prompt is gone before any other queued message.
- 🔴 Made `--plugin-url` accept several URLs separated by spaces in one value.
- 🔴 Fixed vim mode's step back on Escape checking the wrong character when the input contains decomposed accented characters.
- 🔴 Fixed the stale-marketplace error telling you to run `claude marketplace remove`; it now says `claude plugin marketplace remove`.
- 🔴 Let the account's client data (`simple_system_prompt`) switch any listed model to the shorter system prompt, on by default for those models unless `CLAUDE_CODE_SIMPLE_SYSTEM_PROMPT` is set to false; before, only Opus 4.7 could use it, with that variable or a remote switch that's off by default.
- 🔴 Added "Each sentence of text output should change what the reader knows or does next." to the shorter system prompt's code-style guidance for Opus 4.7.
- 🔴 Changed the shorter system prompt's Bash tool description to tell Claude to avoid running `cat`, `head`, `tail`, `sed`, `awk` or `echo` (in some setups also `find` and `grep`) unless told to or no dedicated tool can do the job, replacing the note that re-reading an unchanged file returns a stub.
- 🔴 Added "Commit or push only when the user asks. If on the default branch, branch first." to the Git section of the shorter system prompt's Bash tool description, which `CLAUDE_CODE_DISABLE_GIT_INSTRUCTIONS` or `includeGitInstructions: false` still turn off.
- 🔴 Changed the Opus 4.7 reminder to answer simple messages without a thinking block: it's now an attachment, its system-prompt section is reworded, and a new remote switch (off by default) that the account's client data can override controls it; the old switch no longer turns it on.
- 🔴 Added a startup notice that Anthropic can set remotely, shown as a high-priority warning for 30 seconds (nothing is set by default, and remote workspaces skip it).
- 🔴 Added a remote switch (off by default) under which background memory extraction and auto-dream don't write to the prompt cache, and requests get a second cache breakpoint: where a forked agent branched off the conversation, or otherwise on the message before the last.
- 🔴 Changed `/radio` to open a different Claude FM stream (the command is behind a remote switch that's off by default).
- 🔴 Changed the bundled Bun runtime to another build that still reports 1.3.14, with about 750 KB more machine code and 8.6 MB less read-only data, new `--experimental-http2-fetch`, `--experimental-http3-fetch` and `--no-orphans` options with matching `BUN_FEATURE_FLAG_*` variables, and changes to Bun's built-in JavaScript in its module loader.
- 🔴 Made `claude agents` throw away keys typed while it starts, which used to reach the agents view once it opened; the agents view itself sits behind a remote switch that's off by default.
- 🟢 Added the `worktree.baseRef` setting (`fresh`, the default, or `head`) and a "Worktree base ref" row in `/config` to choose whether `--worktree`, `EnterWorktree` and agent worktrees branch from `origin/<default branch>` or local `HEAD`; `EnterWorktree` no longer always uses `HEAD`, and its description says which base applies.
- 🟢 Added the `sandbox.bwrapPath` and `sandbox.socatPath` managed settings (Linux and WSL, absolute paths, honoured only from admin-controlled settings) to point the sandbox at specific bubblewrap and socat binaries; the subprocess-isolation error names a `bwrapPath` that isn't executable.
- 🟢 Added the admin-tier `parentSettingsBehavior` key: `first-wins` (the default) ignores SDK managed settings when an admin policy exists, and `merge` layers their restrictive settings under it.
- 🟢 Added the effort level to hooks, as an `effort.level` field in their JSON input and a `CLAUDE_EFFORT` environment variable, and to Bash tool commands as `CLAUDE_EFFORT`, on models that support effort.
- 🟢 Changed focus mode's instructions for sessions on the shorter system prompt, telling Claude not to narrate between tool calls and to put what it investigated, found, changed, decided and plans next into its final message.
- 🟢 Made background sessions release memory when free memory drops below a remotely set threshold (1,024 MB by default): settled workers are retired after a minute instead of an hour and before new spawns, and the warm spare is disposed of and not refilled.
- 🟢 Stopped erasing the saved refresh token from the shared credentials when a refresh fails with `invalid_grant`; the session that hit the failure marks that token as dead for itself instead.
- 🟢 Fixed permission rules for the filesystem root (`/`) never matching, so `Edit` and `Write` rules for it always prompted.
- 🟢 Fixed a compromised lock on the prompt-history file (for example after clock skew or a slow disk) raising an unhandled error; it's now logged.
- 🟢 Fixed pressing Esc during compaction showing a spurious compaction error; cancelling now stops the summary request cleanly.
- 🟢 Fixed MCP OAuth sign-in ignoring `HTTPS_PROXY`, `NO_PROXY`, custom CA certificates and client certificates for server discovery, client registration, token exchange, token refresh and cross-app (XAA) identity-provider sign-in.
- 🟢 Fixed an interrupt from Remote Control only aborting the current request: it now cancels the session the way Esc does, including pending permission and input prompts, and lets queued messages go ahead.
- 🟢 Fixed `/effort` in one session changing the effort of other open sessions (a settings-file change no longer resets a running session's effort), and made effort changes from an IDE or SDK host apply even when they match the previous settings value.
- 🟢 Fixed subagents not getting the list of project, user and plugin skills they can call through the Skill tool.
- 🟢 Listed `--remote-control` in `claude --help`.

<details>
<summary>12 internal changes</summary>

- 🔴 Kept the first failure of the startup config read and rethrew it on later attempts, which used to return as if the config had loaded.
- 🔴 Added the repository's current commit (`commitSha`) to feedback reports.
- 🔴 Added counts of auto-mode allow, soft-deny and environment rules, and their total word count, to startup telemetry.
- 🔴 Added the first 60 characters of the error (`errorPrefix`) to compaction-failure telemetry.
- 🔴 Added `prompt_length` to a prompt telemetry event.
- 🔴 Changed API-response telemetry to report the thinking length whenever a response has a thinking or redacted-thinking block, even when it's zero.
- 🔴 Made the daemon's self-respawn after an upgrade log and report a failed spawn (`tengu_bg_daemon_spawn_failed`), where it used to start the new process without handling spawn errors.
- 🔴 Stopped passing tombstone and tool-use-summary messages from forked helper agents to their callers.
- 🔴 Moved the `action_after_plugins_init` mark in the startup timing log to when plugin loading starts instead of when it finishes.
- 🔴 Made background-session terminals remember a requested size even when resizing the pseudo-terminal fails.
- 🔴 Added a background-manager log line when a worker settles.
- 🔴 Added a note to the SDK agent definition's `tools` description that passing `Skill` there is deprecated in favour of the `skills` field.

</details>

<details>
<summary>7 changes with no effect in this build</summary>

- 🔴 Added a Windows-only way to start the background daemon through WMI so it survives closing SSH or the terminal, falling back to a direct start; this build's platform check never reports Windows.
- 🔴 Added a Windows-only delay of background-session terminal resizes until the worker is ready.
- 🔴 Added Windows-only stripping of trailing dots and spaces from path segments in PowerShell permission checks.
- 🔴 Added two SDK control-request schemas, for requests the agent loop sends and requests a client sends; nothing reads them.
- 🔴 Added a `dispose()` method to the SDK query engine that clears its messages and caches and unregisters its memory reporting; nothing calls it.
- 🔴 Added a test-only reset for the in-memory list of dead refresh tokens; nothing calls it.
- 🟢 Added trust for Windows mapped network drives passed with `--add-dir` or SDK `additionalDirectories`; only drive-letter paths qualify, so Linux paths never do.

</details>

---

### 2.1.126 ◆◆◆◆

- 🔴 Fixed `/effort` applying low, medium, high and xhigh only to the current session, whether typed or picked in its slider, although unlike `max` their reply didn't say "(this session only)"; they now save `effortLevel` to your user settings, `/effort auto` removes the saved level, and a failed write shows "Failed to set effort level: …".
- 🟢 Raised the stream watchdog's idle limit from 90 seconds to 5 minutes without data before it aborts a response; background sessions turn this watchdog on (`CLAUDE_ENABLE_STREAM_WATCHDOG`), and `CLAUDE_STREAM_IDLE_TIMEOUT_MS` can still raise the limit but no longer set it below 5 minutes.
- 🟢 Removed the reminder to consider whether each file is malware, and to refuse to improve it, that the Read tool appended to text files when the model was Claude 3.x, Sonnet 4 or 4.5, Opus 4, 4.1 or 4.5, or Haiku 4.5.

---

### 2.1.124 ◆◆◆◆

- 🔴 Changed the default model on Mantle (`CLAUDE_CODE_USE_MANTLE`) to Opus 4.7, which `ANTHROPIC_DEFAULT_SONNET_MODEL` no longer overrides; before, the default was that variable's model or else Haiku 4.5, because Claude Code lists no Sonnet 4.5 model for Mantle and the Sonnet default fell back to the first model it lists there.
- 🔴 Stopped sending `ANTHROPIC_AUTH_TOKEN` as an extra `Authorization: Bearer` header on Claude Platform on AWS and Mantle requests that authenticate with an API key.
- 🔴 Ignored a `user_oauth` profile in the Anthropic config directory (named by `ANTHROPIC_PROFILE` or the active one) when its credentials file is missing, so Claude Code falls back to its other ways of signing in.
- 🔴 Added a 10-second timeout to each token request made for workload identity federation and for `user_oauth` profiles in the Anthropic config directory.
- 🔴 Stopped sending a cloud session's access token to every WebSocket MCP server and to HTTP ones without their own sign-in; it now goes only to MCP endpoints on the session's own ingress host.
- 🔴 Made Bash permission checks stricter: backticks or `$(` that the parser leaves as plain text inside double-quoted strings, values produced at run time used as variable names by `read`, `unset`, `printf -v` and `wait -p`, and more forms of line continuation now need approval, and `printf` counts as read-only only with no options, no `$`, `\u` escapes or escapes inside a `%` conversion in its format, and plain numbers for numeric conversions.
- 🔴 Made the inline Ctrl+R history search ignore letter case.
- 🔴 Changed brief mode's reminder after a turn that ends without `SendUserMessage`: Claude is now told to send its reply without mentioning the reminder, and that it may end the turn without one if it has nothing useful to say; the reminder's wording can be replaced remotely.
- 🔴 Changed brief mode's message view to show Claude's plain text for turns in which it didn't call `SendUserMessage`, instead of hiding it.
- 🔴 Made the REPL tool (turned on with `CLAUDE_CODE_REPL`) throw an error when a script uses an unawaited shorthand result as text, and told Claude in its instructions that only `o.*` values are awaited automatically.
- 🔴 Turned off prompt caching for the WebSearch tool's internal request and for tool-use summaries.
- 🔴 Limited each auto mode classifier call to 2 retries, ignoring `CLAUDE_CODE_MAX_RETRIES`, and to 30 seconds in total, retries included.
- 🔴 Left hidden system-generated user messages out of the conversation auto mode's classifier reviews.
- 🔴 Stopped auto mode's reminders in the shorter system prompt mode (Opus 4.7 with `CLAUDE_CODE_SIMPLE_SYSTEM_PROMPT`, or its remote switch), and added a remote switch, off by default, that sends one "work without stopping for clarifying questions" reminder instead of the periodic ones.
- 🔴 Added `CLAUDE_CODE_COLD_COMPACT` to force compaction's leaner mode on or off (it skips the cached conversation prefix and strips non-essential content; otherwise used only after 90 idle minutes behind a remote switch), and made `/compact` use it too.
- 🔴 Made `/ultrareview` available only where Remote Control is: signed in with a claude.ai account, with Remote Control's remote switch on, and outside cloud sessions.
- 🔴 Stopped `/effort` from saving the level to your user settings and `/effort auto` from clearing it; the level now lasts for the current session, and only the `/model` picker's effort control saves one.
- 🔴 Made `/remote-control` and `claude remote-control` refuse to start on a device that isn't enrolled when the organization requires Trusted Devices, telling you to run `/login` to enroll it (behind a remote switch that's off by default).
- 🔴 Saved the side a session is locked to under the organization's web-search-versus-connectors isolation policy in the transcript, so `--resume`, `--continue` and `/resume` keep it even when the tool call that locked it is no longer in the resumed conversation, such as after compaction (behind a remote switch that's off by default).
- 🔴 Passed `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC`, `CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST`, `DISABLE_ERROR_REPORTING`, `DISABLE_GROWTHBOOK`, `DISABLE_TELEMETRY` and `DO_NOT_TRACK` on to agent-team teammates launched in terminal panes.
- 🔴 Shortened the shorter system prompt's opening: it no longer says to use the instructions and tools to assist the user, or tells Claude never to generate or guess URLs.
- 🔴 Removed the "background" label from the prompt footer of background sessions.
- 🔴 Added "Fix with Claude" to the invalid-settings dialog at startup; it starts the session with a prompt asking Claude to fix the listed settings errors.
- 🔴 Changed how `claude agents` loads pull request status: batches of 20 instead of 40, up to 6 batch queries at once, a 15-second timeout for those queries, last-known statuses kept when a batch fails instead of retrying each pull request, and statuses kept between openings of the view. Pull request URLs that can't be batched are now looked up in parallel instead of one at a time.
- 🔴 Made renaming a session in `claude agents` report "Couldn't rename — the job may have been removed or its state file is unwritable." and undo the new name when saving fails.
- 🔴 Changed rows in `claude agents`: a blocked job shows what it needs, a recently updated job's age ticks every second, counts are printed before their symbols, long group headers are cut to fit, and reactivating a job no longer replaces its detail line with your latest message.
- 🔴 Changed the memory survey's question from "Did this help? (optional)" to "Did this memory help? (optional)".
- 🔴 Changed how `/` command suggestions break near-ties: match scores are compared in steps of a tenth before the other ordering rules apply.
- 🔴 Made pressing Enter on a highlighted directory suggestion for a command's argument submit the command as typed, where it used to only close the suggestions.
- 🔴 Hid the Enter-to-view hint for MCP tasks in the background tasks dialog.
- 🔴 Reset text colours and styles when an attached background session ends or you detach from it, so its styling can't carry over into your terminal.
- 🔴 Added a `file_suggestions` control request for SDK hosts that returns matching file paths for a query.
- 🔴 Added `origin` to SDK result messages, copied from the submitted message's `origin`.
- 🟡 (2.1.319) Added `DISABLE_GROWTHBOOK` to turn off GrowthBook, the remote feature-flag service, without turning off analytics.
- 🟡 (2.1.153) Made `claude agents` mark a job as crashed, saying its working directory no longer exists and it can't be respawned, when that directory has been deleted.
- 🟡 (2.1.299) Made reloading plugins also re-read the list of installed plugins.
- 🟡 (2.1.121) Changed MCP duplicate detection and endpoint-conflict checks to use the `mcp_url` target of cloud WebSocket proxy URLs, so a server behind `/v2/session_ingress/mcp/ws/` can match the same server configured directly.
- 🟡 (2.1.200) Stripped ANSI escape sequences from job details, summaries and requests for input in `claude agents` before showing them.
- 🟡 (2.1.126) Added gateway models to `/model` when `ANTHROPIC_BASE_URL` points at a non-Anthropic host on the first-party provider: at startup Claude Code fetches `/v1/models?limit=1000` (3-second timeout, needs `ANTHROPIC_AUTH_TOKEN` or an API key), keeps `claude*` and `anthropic*` ids, caches them in `~/.claude/cache/gateway-models.json` and lists them as "From gateway".
- 🟡 (2.1.126) Added `claude project purge [path]` to delete a project's transcripts and memory folder, task lists, debug logs, file edit history, matching prompt history lines and `~/.claude.json` entry, with `--dry-run`, `-y`/`--yes`, `-i`/`--interactive` and `--all`.
- 🟡 (2.1.126) Made `--dangerously-skip-permissions` (and plan mode when bypass is available) skip the safety-check prompts for writes to protected paths and for sandbox overrides; only `rm` and `rmdir` on critical system directories still ask.
- 🟡 (2.1.126) Added code pasting to `claude auth login`: it prints "Paste code here if prompted > " and accepts the `code#state` value from the browser when the local callback can't be reached.
- 🟡 (2.1.126) Made the OpenTelemetry `skill_activated` event fire for skills you run by typing their slash command, with a new `invocation_trigger` attribute (`user-slash`, `claude-proactive` or `nested-skill`).
- 🟡 (2.1.126) Stopped marking a tool as running before its permission decision; it shows as in progress once it's actually dispatched.
- 🟡 (2.1.126) Stopped Bedrock, Vertex and Foundry from turning off analytics and GrowthBook when `CLAUDE_CODE_PROVIDER_MANAGED_BY_HOST` is set.
- 🟡 (2.1.126) Fixed `allowManagedDomainsOnly` and `allowManagedReadPathsOnly` being read only from the highest-priority managed settings source: either now applies when any managed tier sets it (remote, MDM, `managed-settings.json` and its drop-ins, or settings from a parent process), and allowed domains and read paths are collected from all of them.
- 🟡 (2.1.126) Stopped passing oversized GIF, JPEG and WebP images through to the API when image processing fails; as with PNG, Claude Code now asks you to resize them. When the API rejects a many-image request because an image exceeds 2000 px, that image is replaced by a note and the request retried.
- 🟡 (2.1.126) Changed the error for organizations that don't allow OAuth to "Your organization has disabled Claude subscription access for Claude Code · Use an Anthropic API key instead, or ask your admin to enable access", with its own code `oauth_org_not_allowed` for the SDK, hooks and background jobs, where it used to tell you to run `/login`.
- 🟡 (2.1.126) Raised the OAuth code exchange and token refresh timeouts from 15 to 30 seconds and made the login callback server listen on 127.0.0.1 instead of `localhost`.
- 🟡 (2.1.126) Fixed a refresh rejected with `invalid_grant` clearing a valid refresh token that another process had just saved; only the token that failed is cleared now.
- 🟡 (2.1.126) Fixed the API retry countdown, which now counts down in whole seconds to the retry's scheduled time instead of running a timer from when the message appeared.
- 🟡 (2.1.126) Fixed responses failing with "Stream idle timeout" right after the computer wakes from sleep: the stream and byte watchdogs now restart their wait when they fire after a suspend.
- 🟡 (2.1.126) Slowed mouse-wheel scrolling in Cursor and in VS Code 1.92 to 1.104 integrated terminals: a wheel event now starts at 1 line instead of 3, with the milder acceleration other terminals get.
- 🟡 (2.1.126) Stopped manually configured HTTP or SSE MCP servers that still need sign-in, or whose token expired with no refresh token, from hiding the matching claude.ai connector.
- 🟡 (2.1.126) Changed `Ctrl+L` to redraw the screen without clearing the prompt input.
- 🟡 (2.1.126) Gave subagents the list of deferred tools on their first turn when tool search is on, so tools such as WebSearch and WebFetch can be found from the start.
- 🟡 (2.1.126) Fixed `ExitPlanMode`, `EnterPlanMode` and `AskUserQuestion` being turned off in interactive sessions with channels on; they're now off only in non-interactive ones.
- 🟡 (2.1.126) Fixed `/plugin` answering an uninstall or update with "✓ Enabled …" (or, for a plugin with options, its options screen); it now reports the plugin as uninstalled or updated.
- 🟡 (2.1.126) Stopped including diffs in "file was modified" reminders once 16,384 characters of them have been added in a turn; later files get a note to use Read instead.
- 🟡 (2.1.126) Changed Remote Control failure notices to always show the error, report each retry's result, and announce "disabled after repeated failures · restart to retry" the same way; the footer's Remote Control indicator hides while there's an error.
- 🟡 (2.1.126) Stopped the PowerShell tool's permission parser from treating a bare `--` (as in `git diff -- file`) as the `--%` stop-parsing token.
- 🟡 (2.1.126) Fixed a hang when a finished tool result in a parallel batch had to wait behind a tool still running ahead of it (such as the error for a malformed tool name); the executor now waits for progress instead of looping.
- 🟡 (2.1.129) Added a Ctrl+R "Search prompts" picker in fullscreen mode (`CLAUDE_CODE_NO_FLICKER` or the `tui` setting) that starts with the current project's prompts; Ctrl+S cycles session, project and everywhere, and remote sessions get a notice that it isn't available yet.
- 🟡 (2.1.126) Stopped sending assistant turns that came back empty, which Claude Code records as `(no content)`, back to the model in later requests and resumed sessions; they're now dropped like blank turns.

<details>
<summary>7 internal changes</summary>

- 🔴 Ran the startup notices (npm deprecation, marketplace, install messages, Chrome extension, model migration, subscription switch) from one list, moved the subscription notice's three-showings limit to a `seenNotifications` record copied from `subscriptionNoticeCount`, and raised the config migration version so migrations run once more.
- 🔴 Renamed cloud sessions' upstream proxy to the egress gateway: `CCR_EGRESS_GATEWAY_ENABLED` turns it on as well as `CCR_UPSTREAM_PROXY_ENABLED`, its certificate and relay come from `/v1/code/egress/gateway` before the old `/v1/code/upstreamproxy` path, and its log lines say "[egress-gateway]".
- 🔴 Limited post-turn status summaries for cloud sessions to remote workspaces or `CLAUDE_CODE_REMOTE` sessions started from the remote, remote desktop or remote mobile entry points, excluding bug-hunting fleets.
- 🔴 Changed background session plumbing: a failed claim of a spare process reports the job-removal error, the daemon gives workers its sign-in snapshot only when it runs as a service, and adopting an already-running worker or attaching to it now counts as recent input, which keeps it from being retired as idle.
- 🔴 Removed an experiment's alternative wording for the Read tool's `offset` and `limit` parameters.
- 🔴 Moved the new `/init` flow from the remote switch `tengu_new_init` to `tengu_slate_harbor_experiment`; its longer description now shows only when `CLAUDE_CODE_NEW_INIT` is set.
- 🔴 Stopped background sessions and agent-team teammates from saving their first prompt as the last session hint in `~/.claude.json`.

</details>

<details>
<summary>17 changes with no effect in this build</summary>

- 🔴 Added a callback that the spare worker's claim socket can run once it starts listening; its only caller doesn't supply one.
- 🔴 Added an override that would make Remote Control always available; it's hard-coded off.
- 🔴 Added a session flag set after `gh pr merge` (without `--auto`) or `gh pr close`, and an internal `doneMeansMerged` setting; nothing reads either.
- 🔴 Added an `isolatePeerMachines` setting ("Require explicit approval before SendMessage can reach a peer session on another machine via Remote Control"); nothing reads it.
- 🔴 Added a `from-name` attribute for messages sent to another session; no caller supplies a name.
- 🔴 Added `ListPeers` as an old name for a `ListAgents` tool that this build doesn't contain.
- 🔴 Added an option to draw the agent banner and prompt border as a multi-colour gradient, and a slot for a "pride" command; nothing sets the gradient and the command's module is `null`.
- 🔴 Added an internal-builds-only `ccshare_url` field to the SDK's `submit_feedback` reply; it's never set.
- 🔴 Added the `token-efficient-tools-2026-03-28` beta to the table of beta headers; nothing sends it.
- 🔴 Made accepting a prompt suggestion discard a speculative run older than 30 seconds; speculation is hard-coded off.
- 🔴 Recorded each MCP server's self-reported name on its tools; nothing reads it.
- 🔴 Recorded whether Remote Control was connected when `/teleport` opens; nothing reads it.
- 🔴 Added a helper that lists directory names from the file index, reachable only through the module's export object; nothing calls it.
- 🔴 Added three empty placeholder functions, two of them called during start-up, and an unused constant `65001`; none of them does anything.
- 🟡 (2.1.126) Added PowerShell 7 locations under Program Files, the Microsoft Store apps folder and .NET global tools to the PowerShell search, behind a Windows check that's never true in this Linux build.
- 🟡 (2.1.126) Made the environment section describe PowerShell as the shell when the PowerShell tool is on, behind a Windows check that's never true in this Linux build.
- 🟡 (2.1.126) Changed Windows clipboard writes to pass the text to PowerShell on standard input with no size limit, in a branch this Linux build never takes.

</details>

---

### 2.1.123 ◆◆◆◆

- 🟢 Stopped `CLAUDE_CODE_DISABLE_EXPERIMENTAL_BETAS` from stripping the `oauth-2025-04-20` header on first-party, Claude Platform on AWS and Foundry connections, which made claude.ai logins fail with repeated 401s; with the variable set, those connections no longer trim the beta headers they send to the variable's short list, so anything named in `ANTHROPIC_BETAS` goes out too.

---

### 2.1.114 ◆◆◆◆

- 🟢 Fixed tool permission dialogs crashing when the request came from outside the local session: from an agent teams teammate, or from a remote, direct-connect or SSH session. The crash came with 2.1.111.

---

### 2.1.112 ◆◆◆◆

- 🔴 Stopped sending `temperature` to Claude Opus 4.7 on requests made with thinking off, since that model rejects the parameter: the main conversation with thinking turned off, subagents (which run without thinking unless forked) and compaction's fallback summary; other models still get the configured override or the default of 1.
- 🟢 Fixed auto mode's permission classifier failing on Claude Opus 4.7: its `temperature` of 0 is no longer sent to that model.

---

### 2.1.111 ◆◆◆◆

- 🔴 Changed the system prompt for every model and account to the shorter variant that 2.1.110 sent only to Opus 4.6 accounts in a server-side experiment: it adds a "Text output" section (say in one sentence what you're about to do before the first tool call, give one-sentence updates, end with a one- or two-sentence summary), limits of 25 words between tool calls and 100 words for final answers unless the task needs more, no code comments by default, and two-or-three-sentence answers to exploratory questions without implementing. It also condenses the tool and session guidance and drops the instructions to read code before proposing changes, avoid time estimates, diagnose a failure before switching approach, ask why a tool call was denied, and write GitHub references as `owner/repo#123`.
- 🔴 Added the `proxyAuthHelper` setting (early access, only with `CLAUDE_CODE_ENABLE_PROXY_AUTH_HELPER=1`): a shell command whose output Claude Code sends as the `Proxy-Authorization` header on requests through your proxy, cached for 5 minutes (`CLAUDE_CODE_PROXY_AUTH_HELPER_TTL_MS` changes that) and run again with the proxy's challenge in `CLAUDE_CODE_PROXY_AUTHENTICATE` after a 407. The command also gets `CLAUDE_CODE_PROXY_URL` and `CLAUDE_CODE_PROXY_HOST`; in interactive sessions a command from project or local settings waits until the workspace is trusted, while `claude -p` and SDK sessions run it without that check.
- 🔴 Added an SDK initialize option, `appendSubagentSystemPrompt`, that appends text to the system prompt of every subagent, nested ones included, when `CLAUDE_CODE_ENABLE_APPEND_SUBAGENT_PROMPT` is set.
- 🔴 Added per-tool permission policies to the SDK's `mcp_set_servers` request for HTTP and SSE servers: each tool can carry `always_allow`, `always_ask` or `always_deny`, and the allow and deny ones become session permission rules.
- 🔴 Let cloud sessions (`CLAUDE_CODE_REMOTE`) name a remote setting in `CLAUDE_CODE_SYSTEM_PROMPT_GB_FEATURE` whose text replaces the SDK session's system prompt.
- 🔴 Required manual approval for PowerShell tool commands with paths that start with `~user` (another user's home folder), since Claude Code can't check where they point.
- 🔴 Made slash commands that can't run in `claude -p` and SDK sessions say so: commands that open a panel now reply "/name opens an interactive panel and isn't available in this environment. Run it from the Claude Code terminal instead." instead of doing nothing, built-in commands the session doesn't offer reply "/name isn't available in this environment.", and the command list stays limited to commands that work there after a directory change or plugin reload.
- 🔴 Widened "did you mean" suggestions for mistyped slash commands and Skill tool names to two edits for every name length (short names allowed only one), with two swapped letters counting as one edit.
- 🔴 Raised the image size limit for Opus 4.7 to 2576×2576 pixels; other models keep 2000×2000.
- 🔴 Changed the "model isn't available" errors on Bedrock, Vertex, Foundry and Mantle to suggest Opus 4.1 for Opus 4.7 and Opus 4.5 too (Opus 4.5 had no suggestion before).
- 🔴 Stopped showing auto mode's classifier the read-only tool calls in the conversation it reviews (Read, Grep, Glob, LSP, ToolSearch and the MCP resource list and read tools).
- 🔴 Made effort support follow `modelOverrides` rather than the provider's model name, so custom-named Opus 4.6 and Sonnet 4.6 models get the effort controls and request parameter their configured model supports.
- 🔴 Removed the "We recommend medium effort for Opus" dialog that offered Pro and Max users on Opus 4.6 a one-time effort choice.
- 🔴 Removed the remote switch `tengu_garnet_loom` (off by default) that ran subagents inheriting an Opus main model on Sonnet instead.
- 🔴 Moved the "respond without a thinking block" reminder from Opus 4.6 (switched on by the account's `loud_sugary_rock` client data) to Opus 4.7, behind the remote switch `tengu_loud_sugary_rock` (off by default).
- 🔴 Kept proactive auto-compaction on when you've set the auto-compact window through an environment variable or settings, while the remote mode `tengu_cobalt_raccoon` (off by default, it turns proactive auto-compaction off) is on.
- 🔴 Blocked `/feedback` and feedback surveys for organization accounts (Team or Enterprise sign-in, or a first-party API key) whose policy hasn't loaded yet, as was already done in essential-traffic mode; one more survey now checks that policy too.
- 🔴 Made `/feedback` reports redact secrets in the report's own fields as well as the transcript, checking each JSON value in the transcript on its own.
- 🔴 Added `routines` as another name for the bundled `/schedule` skill.
- 🔴 Added pointers to `/feedback`: "Still having issues? Run /feedback to report details." at the end of `/doctor`, and "Something else? Use /feedback to report bugs or request features." in `/help` when the terminal has at least 44 rows.
- 🔴 Showed the jump-to-bottom shortcut (`ctrl+end` by default) in the "new messages" pill.
- 🔴 Kept the cursor on a plugin in `/plugin`'s installed list after you toggle it, even when the list re-sorts, and made the list's section and scope headings separate rows that the cursor skips; the list also shows more rows (at least 8, or the terminal height minus 10, where it was at least 6 or the height minus 13).
- 🔴 Sorted the merged list of available agents by agent type.
- 🔴 Reworded `/clear`'s description to "Start a new session with empty context; previous session stays on disk (resumable with /resume)".
- 🔴 Changed how cloud sessions bundle your repository: when `git stash create` fails in a repository with commits, it now stops with "Could not capture uncommitted changes … Run `git add .` or commit, then retry." instead of going ahead without your uncommitted work, and the created session can carry tags.
- 🔴 Dropped the "When issuing multiple commands" guidance from the Bash tool's description while the remote switch `tengu_relay_chain_v1` (off by default) is on.
- 🔴 Made permission checks for tool calls run from REPL code (`CLAUDE_CODE_REPL`) see the calls made earlier in the same REPL block and the full tool list.
- 🔴 Treated `VERTEX_REGION_CLAUDE_4_5_OPUS`, `VERTEX_REGION_CLAUDE_4_7_OPUS` and `OTEL_LOG_RAW_API_BODIES` in a settings file's `env` as safe: they apply before the workspace trust prompt and don't count toward its warning.
- 🔴 Updated the bundled `/claude-api` skill for Opus 4.7 (`claude-opus-4-7` as its Opus model, notes that it takes adaptive thinking only, answers `temperature`, `top_p`, `top_k` and `budget_tokens` with a 400, supports `xhigh`, omits thinking text by default and has 1M context, its caching minimum, and help with migrating between model versions), and made it write its reference files into a local skill folder on first use so Claude can read any of them. It also teaches Opus 4.7 task budgets through `output_config.task_budget` (a 20,000-token minimum, the `task-budgets-2026-03-13` beta header), and has Claude ask which files to change before migrating models unless the request already names a file, folder or list of files.
- 🔴 Told the memory fact-extraction step (behind the remote switch `tengu_billiard_aviary`, off by default) to lift facts only from memory files and never answer the query itself.
- 🔴 Renamed model labels: "(with 1M context)" became "(1M context)", Opus Plan Mode now reads "Use Opus in plan mode, Sonnet otherwise", and the 1M-unavailable message says "Opus with 1M context is not available for your account".
- 🔴 Listed Opus 4.6 and Opus 4.6 (1M context) as their own `/model` entries on Bedrock, Vertex, Foundry and Mantle when no custom Opus model is set, and kept an Opus 4.6 row for first-party and Claude Platform on AWS sessions already using it.
- 🔴 Stopped rejecting `--session-id` as "already in use" when it names the session given to `--resume` together with `--fork-session`.
- 🔴 Drew `/context`'s lists of memory files, skills, agents and MCP tools as indented trees instead of rows starting with "└".
- 🔴 Updated the Bun runtime from build `1.3.13+edb020d5d` to `1.3.13+743d2a40e`: about 295 KB more native code (the executable grew by about 970 KB) with no new or removed sections, two new variables (`BUN_DEBUG_FORCE_NIX_HOST`, `BUN_INTERNAL_TEST_CHANGED_TRIGGER_FILE`), four new git option strings (`--name-only`, `--show-toplevel`, `--exclude-standard`, `--full-name`), and built-in JavaScript that now refreshes a network socket's idle timer whenever data arrives.
- 🟡 (2.1.114) Made the tool permission dialog crash for requests that arrive without a tool context (agent-team teammates' requests and requests relayed from remote sessions): it now reads the permission mode for its analytics event without a guard, and that stayed broken through 2.1.113.
- 🟡 (2.1.113) Made `/ultrareview` run its repository and billing checks in parallel, show a "Scope:" line with the branch's diff stat in its launch dialog, and animate "Launching".
- 🟢 Fixed blank lines between the rows of `/context`'s usage grid: the layout engine now takes an element's own margins off the room its content is measured in, and each grid row is shifted one column left, so its last square used to wrap onto a second line.
- 🟢 Added Claude Opus 4.7: the `opus` alias and the picker's Opus entries now mean it, it's the default model for Max subscribers and Team seats on the Max 5x tier, agent-team teammates default to it, and the system prompt names it as the newest Opus (Bedrock `us.anthropic.claude-opus-4-7`, Mantle `anthropic.claude-opus-4-7`, `claude-opus-4-7` elsewhere, region override `VERTEX_REGION_CLAUDE_4_7_OPUS`); it's priced like Opus 4.6, with 64,000 output tokens by default up to 128,000, 1M context, adaptive thinking that `CLAUDE_CODE_DISABLE_ADAPTIVE_THINKING` doesn't turn off (the variable now applies only to Opus 4.6 and Sonnet 4.6), the advisor tool and a January 2026 knowledge cutoff, while `/fast` keeps using Opus 4.6. The legacy-model notice now says "Model updated to Opus 4.7", commits made with a model Claude Code doesn't recognize get "Co-Authored-By: Claude Opus 4.7", and this release still sent `temperature` to Opus 4.7, which rejects it, so its requests with thinking off and auto mode's classifier failed until 2.1.112.
- 🟢 Added an "Opus 4.7 is here" welcome notice, "Welcome to Opus 4.7 xhigh! · /effort to tune speed vs. intelligence", shown ahead of the other startup notices on up to 12 launches of first-party sessions.
- 🟢 Added the `xhigh` effort level between `high` and `max` (Opus 4.7 only, other models fall back to `high`) in `/effort`, `--effort`, the `effortLevel` setting, the model picker ("xHigh", taking `max`'s old ◉ icon while `max` gets ◈), agent definitions and the SDK's model list; Opus 4.7 defaults to `xhigh` and keeps it over a saved effort until you change effort once with `/effort`, the model picker, `--effort` or the settings file.
- 🟢 Made `/effort` with no argument open a slider from Speed to Intelligence (←/→ to move, Enter to confirm) instead of printing the current level; the notice after an effort change now replaces any earlier one and stays 10 seconds instead of 12.
- 🟢 Changed which models qualify for auto mode: for Max subscribers only Opus 4.7 (no longer Opus 4.6 or Sonnet 4.6), and for other first-party and Claude Platform on AWS sessions Opus 4.7 as well as Opus 4.6 and Sonnet 4.6.
- 🟢 Added "Auto (match terminal)" to the `/theme` picker and `auto` to the theme choices in the settings Claude Code lists for `/config`; in that mode Claude Code asks the terminal for its background colour and switches between light and dark when the terminal reports a theme change.
- 🟢 Added the bundled `/less-permission-prompts` skill, which has Claude scan your recent transcripts for read-only Bash and MCP tool calls and add a prioritized allowlist to the project's `.claude/settings.json`.
- 🟢 Changed `/ultrareview` (already present behind a remote switch): its description and confirm dialog show an estimated duration and cost from the remote config (defaults "~10–20 min" and "$10-$20"), the overage prompt always says "This review bills as Extra Usage" with that cost, the remote config can pick the review model, a repository too large to bundle is refused up front with a hint to review a PR instead, the merge base is looked up against `origin/<default branch>` first, an empty change set gets "It doesn't look like you have any new commits or changes to review…", and SDK hosts can start it with an `ultrareview_launch` request.
- 🟢 Turned on the PowerShell tool on Linux when `CLAUDE_CODE_USE_POWERSHELL_TOOL` is set to a true value (outside Windows it was always off); a `--tools` list that doesn't name it leaves it off.
- 🟢 Stopped prompting for read-only commands with unquoted globs such as `ls *.ts`, for a fixed list of commands (`ls`, `cat`, `head`, `tail`, `grep`, `wc`, `diff`, `du`, `stat`, `cd` and similar); unquoted variables still prompt.
- 🟢 Stopped prompting for `cd <dir> && …` chains when the `cd` goes to an absolute, `./` or `../` path inside your working directories and the line uses only `&&`: the rest is checked as if it ran in that folder, though chains that run `git` still prompt.
- 🟢 Added a "Did you mean `claude <command>`?" suggestion when `claude <word>` is a near-miss of a subcommand; it exits with code 1 instead of starting a session with that word as the prompt.
- 🟢 Named plan files after your prompt (its first few words plus random words) instead of random words alone.
- 🟢 Improved `/setup-bedrock` and `/setup-vertex`: they show the real user settings path, start from models already pinned in `ANTHROPIC_DEFAULT_*_MODEL`, mark "(currently pinned)", and offer "Pin the working models with 1M context".
- 🟢 Added sorting by estimated token count to `/skills` (`t` toggles it).
- 🟢 Made Ctrl+U clear the whole input instead of deleting to the start of the line, both in the prompt and in search boxes (in the prompt, Ctrl+Y brings it back).
- 🟢 Made Ctrl+L also force a full redraw of the screen.
- 🟢 Added `[` (print output) and `v` (open in your `VISUAL` or `EDITOR` program) to the transcript view's footer hints.
- 🟢 Replaced the "… +N lines …" marker inside very long sent prompts with a full-width "(N lines hidden)" rule.
- 🟢 Added `plugin_errors` to the `stream-json` init message when plugins were demoted for unsatisfied dependencies.
- 🟢 Added `OTEL_LOG_RAW_API_BODIES`, which sends full API request bodies (thinking redacted) and response bodies as OpenTelemetry log events, cut off after 61,440 UTF-16 code units of serialized JSON.
- 🟢 Quieted API retries: until the last attempt they now show only a dim "Retrying in … · attempt n/m" line rather than the error, except for rate-limit, SSL and connection errors (before, the first three attempts showed nothing and later ones showed the error).
- 🟢 Asked MCP servers for uncompressed responses (`Accept-Encoding: identity`) on HTTP, SSE and IDE connections.
- 🟢 Removed the non-streaming fallback request's cap of two retries and its 60-second timeout after a stalled stream, so it retries and waits like a normal request again.
- 🟢 Stripped control characters from terminal notifications (iTerm2, Kitty, Ghostty) and sent iTerm2's as "title: message" without blank lines, which fixes display tearing in iTerm2 with tmux.
- 🟢 Fixed `@` file suggestions in folders with no tracked files: untracked files are indexed even when Git lists no tracked ones, and the file list isn't rescanned on every turn outside Git.
- 🟢 Fixed stale LSP diagnostics: documents now carry increasing version numbers, diagnostics for an older version are dropped, and Write and Edit clear pending diagnostics for the file they change.
- 🟢 Fixed Tab-completing `/resume` resuming a session straight away instead of opening the picker when no argument was typed.
- 🟢 Fixed `/clear` dropping the session name set with `/rename`.
- 🟢 Improved plugin dependency errors to say whether version requirements conflict, are invalid or are too complex to intersect (control characters removed, lists cut at 200 characters), cleared the stale resolved version after `plugin update`, and made `plugin install` reinstall a dependency that's enabled but not installed at the same scope.
- 🟢 Rewrote the Skill tool's description to drop its examples (`commit`, `review-pr`, `pdf`) and tell Claude to call only skills from the available list or ones the user typed, never invented names.
- 🟢 Stopped pointing Bedrock, Vertex, Foundry and Mantle users at the Anthropic status page in 429 rate-limit errors.
- 🟢 Stopped a feedback survey from appearing while another survey is open.
- 🟢 Made bare URLs in tool output clickable links by default, so they keep working when the terminal wraps them (for outputs up to 100,000 characters).

<details>
<summary>19 internal changes</summary>

- 🔴 Moved saving the update channel chosen by `claude install latest` or `stable` ahead of the launcher checks and the cleanup of npm installs and shell aliases.
- 🔴 Started sending `high` explicitly when a supported model has no chosen or account-specific effort, instead of leaving `output_config.effort` out (`CLAUDE_CODE_EFFORT_LEVEL=auto` or `unset` still leaves it out for older models); Opus 4.6 on Max also gets `medium` even when both older effort experiments are off.
- 🔴 Delayed Claude Code's self-restarts by one event-loop turn before the replacement process starts, including the restarts after login, provider setup and a Bedrock model upgrade.
- 🔴 Added the `tengu_unknown_command_suggestion` event, sent each time the `claude <word>` typo suggestion is printed.
- 🔴 Logged failed analytics event uploads only to the debug log instead of recording them as errors.
- 🔴 Changed the Datadog event allowlist: dropped `tengu_sdk_transport_error`, added `tengu_vscode_sdk_stream_ended_no_result`.
- 🔴 Stopped sending `tengu_sdk_transport_error` and waiting up to 500 ms for telemetry when a `stream-json` input line can't be parsed; the process still prints the error and exits with code 1.
- 🔴 Added `session_state`, `last_message_type` and `pending_control_requests` to the SDK stall event, and `api_error_status` and `cause_name` to the SDK session crash event.
- 🔴 Added `is1hCacheTTL`, `queryDepth`, `querySource`, `model` and `globalCacheStrategy` to prompt-cache break diagnostics.
- 🔴 Dropped `error_class` and `fallback_cause` from the non-streaming fallback's error event.
- 🔴 Added the `tengu_ptl_surfaced_to_user` event when a prompt-too-long or image error is shown.
- 🔴 Added a `bashMissKind` label to Bash permission decisions (`sed-dangerous`, `cd-compound-write`, `multi-cd`, `no-rule-match` and others); nothing in this build reads it by name.
- 🔴 Made the list of models without tool search a remote setting (`tengu_tool_search_unsupported_models`, still Haiku by default) and the memory survey's chance a remote number (`tengu_velvet_moth`, still 0.2).
- 🔴 Moved the "send a message if the user would act on this" hint inside the `<task-notification>` block of Monitor events.
- 🔴 Made startup wait up to 300 ms for remote feature flags before loading tools when none are cached yet.
- 🔴 Kept a whitespace-only piece of an assistant message when another piece of the same message has content, instead of filtering it out.
- 🔴 Wrote transcripts hydrated from a cloud session to disk line by line instead of as one large string.
- 🔴 Added `tengu_opus47_launch_shown` and the `opus47LaunchSeenCount` and `unpinOpus47LaunchEffort` config keys behind the Opus 4.7 notice and effort pin.
- 🔴 Reworded the invalid update-channel error to "Use 'latest' or 'stable'" and made the channel name `rc` fail with the old wording.

</details>

<details>
<summary>15 changes with no effect in this build</summary>

- 🔴 Removed an unused helper that read session memory and logged its length; nothing in 2.1.110 called it.
- 🔴 Added an unused channel-versus-version check to the native installer and passed its skipped-update flag through another helper; nothing reads either.
- 🔴 Added a macOS sleep inhibitor that runs `caffeinate` while Claude works and restarts it every 4 minutes; on Linux it does nothing, as the old empty stub did.
- 🔴 Added a `pre-suspend` event before Ctrl+Z suspends the app; nothing listens to it, so suspend works as before.
- 🔴 Added an always-disabled hidden command named `stub` to a command list that nothing in the build reads.
- 🔴 Rewrote the Skillify skill's template to have Claude review the conversation above instead of receiving session memory and user messages pasted in; nothing registers that skill in this build.
- 🔴 Added a model migration guide to the `/claude-api` skill and pointed its quick reference and error-codes page at `shared/model-migration.md`, but the guide is never added to the skill's files, so that file isn't available.
- 🔴 Added a `reportMetadata` pass-through to the Remote Control bridge handle; nothing calls it.
- 🔴 Passed the permission mode to SSH sessions when they connect and when it changes; nothing in this build can start an SSH session.
- 🔴 Added a startup check for a direct-connect host that switches the permission mode to default; the host value is always unset in this build.
- 🔴 Counted a running background shell task as "busy" in the session's status; the only thing that reads it is an empty effect.
- 🔴 Added an internal `mirror_error` system message to the SDK's message schema, for transcript-mirror writes that fail; nothing in this build sends it.
- 🟢 Removed the "Session environment not yet supported on Windows" early return, so `CLAUDE_ENV_FILE` and SessionStart environment files apply on Windows; this Linux build never took that branch.
- 🟢 Fixed Windows permission rules with drive-letter paths to be root-anchored and compared paths without regard to drive-letter case; both changes sit in Windows-only branches this Linux build never takes.
- 🟢 Made the PowerShell tool on Windows follow `CLAUDE_CODE_USE_POWERSHELL_TOOL`, or else the remote switch `tengu_cobalt_ridge` (off by default), and deny it whenever Bash is denied unless it was turned on explicitly; those branches run only on Windows.

</details>
