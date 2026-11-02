---
title: "File bookmarks getting started"
summary: "Install bookmarks and use its core API with explicit application ownership."
category: "Guides"
order: 10
keywords: ["bookmarks", "file", "installation", "example"]
related: ["index.md", "api.md", "typed-api.md"]
---

# File bookmarks getting started

## Install and import

Copy the sealed `bookmarks-0.1.0.buipkg` archive into your project `packages/` directory, or install a source/registry dependency named `bookmarks` using BPM and sync it. Then use `Import bookmarks`. The archive includes this documentation as package resources.

Use a consuming source filename such as `main.bui`; a local module named exactly like the package shadows its import.

## Complete usage example

The example is compiler-checked as a complete Buoy module. Functions and Subs are intended to be called from the consuming app at the appropriate user action or lifecycle event.

```basic
Import bookmarks

Sub InspectChosenFile(file As FileSystemItem)
    If Not BookmarksSupported() Then Return
    Dim data As Bytes = CreateBookmark(file)
    Dim resolved As New ResolvedBookmark(data)
    Try
        Print resolved.File.FullPath()
        If resolved.IsStale Then data = resolved.Refresh(True)
    Finally
        resolved.Close()
    End Try
End Sub
```

## Integrate with the application

Persist CreateBookmark bytes yourself. Resolve with the same securityScoped policy used during creation. For a scoped reference call StartAccess explicitly, keep the wrapper and scope alive until all consumers finish, and Close in Finally. StopAccess balances only a successful start; repeated starts and Close are idempotent. Do not share one wrapper across concurrent access/close calls.

## Sandboxed file access

For security-scoped bookmarks, the consuming app needs the appropriate user-selected-file and app-scoped-bookmark entitlements plus a legitimate original user grant, usually from an open panel. Preserve whether the bookmark was scoped and read-only alongside its opaque bytes; those policies cannot be recovered reliably from the bytes. The package neither adds entitlements nor displays a permission panel. Keep a successful scope alive until asynchronous readers and previews finish.

## Handle failures and unsupported targets

Nil or empty bookmark data, Nil files, nonexistent files, and NUL-containing paths are rejected. Corrupt or unresolvable native bookmarks throw with an NSError description. StartAccess on an ordinary bookmark or a closed wrapper is invalid. A false start result does not prove ordinary file I/O will fail; check the operation itself.

Native operations require macOS 11.2 or newer. BookmarksSupported returns false on Windows and Linux, where creation and resolution throw. A bookmark is not an entitlement or a permission grant.

Return to the [overview](index.md) or consult the [API reference](api.md).
