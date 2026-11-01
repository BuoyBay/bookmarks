---
title: "File bookmarks"
summary: "Persist macOS file references and explicitly balance security-scoped access."
category: "Overview"
order: 0
keywords: ["bookmarks", "file"]
related: ["getting-started.md", "api.md", "typed-api.md"]
---

# File bookmarks

The bookmarks package creates native NSURL bookmark bytes and resolves them back into files, including stale-reference detection. It is useful for reopening user-selected files after an application restart or a file move.

## Platform support

Native operations require macOS 11.2 or newer. BookmarksSupported returns false on Windows and Linux, where creation and resolution throw. A bookmark is not an entitlement or a permission grant.

## Start reading

- [Getting started](getting-started.md) walks through installation and a complete example.
- [API reference](api.md) describes public symbols, state, errors, and ownership.

## Scope

Persist CreateBookmark bytes yourself. Resolve with the same securityScoped policy used during creation. For a scoped reference call StartAccess explicitly, keep the wrapper and scope alive until all consumers finish, and Close in Finally. StopAccess balances only a successful start; repeated starts and Close are idempotent. Do not share one wrapper across concurrent access/close calls.
