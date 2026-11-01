---
title: "File bookmarks API"
summary: "Persist macOS file references and explicitly balance security-scoped access."
category: "Reference"
order: 20
keywords: ["bookmarks", "file", "API", "lifecycle"]
symbols: ["BookmarksSupported", "CreateBookmark", "ResolvedBookmark", "ResolvedBookmark.File", "ResolvedBookmark.IsStale", "ResolvedBookmark.IsAccessing", "ResolvedBookmark.StartAccess", "ResolvedBookmark.StopAccess", "ResolvedBookmark.Refresh", "ResolvedBookmark.Close"]
related: ["index.md", "getting-started.md", "typed-api.md"]
---

# File bookmarks API

Import the `bookmarks` package. Constructors below use the class name; invoke them with `New`. Listed members are public client APIs; native declarations and trampoline helpers are implementation details.

## Public surface

| Symbol | Contract |
| --- | --- |
| `BookmarksSupported() As Boolean` | Reports the macOS target; check before native operations. |
| `CreateBookmark(file As FileSystemItem, securityScoped As Boolean = False, readOnlyAccess As Boolean = True) As Bytes` | Creates ordinary or explicitly app-scoped bytes. Ordinary bookmarks exclude implicit scope. |
| `ResolvedBookmark(data As Bytes, securityScoped As Boolean = False)` | Owns the resolved native URL. Resolution shows no UI and does not mount volumes or implicitly start a scope. |
| `ResolvedBookmark.File As FileSystemItem` | Resolved file, read-only. |
| `ResolvedBookmark.IsStale As Boolean` | Read-only result of resolution. The app decides when to replace persisted bytes. |
| `ResolvedBookmark.IsAccessing As Boolean` | True only while this wrapper owns an explicitly started scope. |
| `ResolvedBookmark.StartAccess() As Boolean` | Starts at most one explicit scope per scoped wrapper. |
| `ResolvedBookmark.StopAccess()` | Balances a successful start without releasing the resolved URL. |
| `ResolvedBookmark.Refresh(readOnlyAccess As Boolean) As Bytes` | Recreates bytes from the resolved URL. Pass the original read-only policy explicitly. |
| `ResolvedBookmark.Close()` | Stops an owned scope and releases the native URL. Repeated calls are safe. |

## Ownership and timing

Persist CreateBookmark bytes yourself. Resolve with the same securityScoped policy used during creation. For a scoped reference call StartAccess explicitly, keep the wrapper and scope alive until all consumers finish, and Close in Finally. StopAccess balances only a successful start; repeated starts and Close are idempotent. Do not share one wrapper across concurrent access/close calls.

## Errors and result interpretation

Nil or empty bookmark data, Nil files, nonexistent files, and NUL-containing paths are rejected. Corrupt or unresolvable native bookmarks throw with an NSError description. StartAccess on an ordinary bookmark or a closed wrapper is invalid. A false start result does not prove ordinary file I/O will fail; check the operation itself.

## Platform behavior

Native operations require macOS 11.2 or newer. BookmarksSupported returns false on Windows and Linux, where creation and resolution throw. A bookmark is not an entitlement or a permission grant.

See the [complete usage example](getting-started.md) and the [package overview](index.md).


[Full typed API: properties, parameters, defaults, and returns](typed-api.md)
