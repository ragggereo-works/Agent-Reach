# Agent Reach access repair: 17 October 2026

## Changes

Merged creator main at 94f06c1, preserving this fork's TikTok, Instagram and
LinkedIn OpenCLI integration. The MCP launch configured in Codex uses this
checkout and its .venv, so no package replacement is necessary.

OpenCLI's default adapter session rejected Bilibili navigation while a public
browser session could open the same site. The same search succeeded using
`--site-session persistent`. The MCP now retries that exact navigation error
once with the documented option. It does not repeatedly retry HTTP rate limits.
This establishes a session-path failure; the underlying extension rejection
mechanism has not been proven.

Added browser_read and browser_close MCP tools. Browser reads return an isolated
session identifier, support continued scrolling and extraction offsets, and
release only their own sessions. They use public OpenCLI commands. They expose
no arbitrary JavaScript, login automation or posting actions. Syntax supports
OpenCLI 1.x and 2.x; live verification used installed 1.8.5 / extension 1.0.24.
The 2.x syntax is based on the official release notes:
https://github.com/jackwener/opencli/releases/tag/v2.0.0
OpenCLI itself was not upgraded during this repair.

Facebook search is now included in the MCP schema. Empty browser extraction is
reported as unsuccessful. Setup diagnostics are explicitly described as setup
evidence rather than a live platform verdict. Shared skills direct agents to
try visible browser access before declaring a platform inaccessible.

## Live verification

A fresh MCP stdio client initialized and exposed all five tools. Bilibili and
XiaoHongShu searches returned results. Instagram browser_read returned NASA post
content; a continuation scrolled twice and OpenCLI state showed page_scroll
1.3 up / 3 down. The session was released afterwards. An additional shell check
through the same router returned search results for Twitter, Reddit, TikTok,
LinkedIn jobs, YouTube, GitHub and Exa.

Instagram's structured profile/search adapters still returned HTTP 429/404.
Browser content works; this is not a repaired structured Instagram API.
Facebook search returned an empty list. Public browser navigation initially
showed cookie consent; optional cookies were declined, after which the page
still presented a login prompt and extraction was empty. Facebook content
access is not verified.

Earlier in this chat, web page reading, RSS links/dates, and public V2EX/Xueqiu
homepages worked via Jina. Their native API/feed capabilities were not fully
validated. Xiaoyuzhou transcription remains unconfigured. Newly merged Boss
support has not been live-tested. No claim is made that every site or feature
will work, or that scrolling always loads more posts.

## Activation

Shared instructions were installed into ~/.agents/skills, ~/.claude/skills and
~/.config/opencode/skills. Existing Agent and Claude skill folders were backed
up under /tmp/agent-reach-skill-backup-20261017-211656 before replacement.

Existing MCP processes retain loaded Python modules and tool lists. Reconnect
Agent Reach MCP or restart the client, then confirm browser_read/browser_close
appear. New chats using the shared skill can also use the documented OpenCLI
shell fallback. Browser login sessions and platform restrictions still apply.

## Validation

` .venv/bin/python -m pytest tests/ -v `: 657 passed, 16 subtests passed.
Existing unrelated working tree files were preserved. Changes are on
codex/agent-reach-access-repair; main was not pushed or replaced.
