# File bookmarks · 0.1.0

Persist native NSURL bookmark bytes instead of assuming a saved path stays valid.
`Import bookmarks`. macOS 11.2+ is required by the resolution options used here.

Start with the [installation and complete usage example](../resources/buoy-docs/getting-started.md), including explicit close and sandbox requirements. The [API reference](../resources/buoy-docs/api.md) describes errors and access ownership.

- `BookmarksSupported()` reports whether the target is macOS.
- `CreateBookmark(file, securityScoped = False, readOnlyAccess = True) As Bytes` creates ordinary bookmark bytes, or explicitly requests an app-scoped bookmark. Ordinary bookmarks exclude implicit ephemeral security scope.
- `New ResolvedBookmark(data As Bytes, securityScoped = False)` resolves with no UI, no volume mounting and no implicit ephemeral-scope start. `File` and `IsStale` are read-only results.
- `Refresh(readOnlyAccess As Boolean) As Bytes` recreates bytes using the **resolved native URL**. Pass the original read-only policy explicitly; it cannot be inferred from opaque bookmark bytes. Persist the returned bytes yourself when `IsStale` is true; the package never silently writes preferences or files.
- For scoped bookmarks only, `StartAccess() As Boolean` starts at most one scope per wrapper. Repeated successful starts are idempotent. `IsAccessing` reflects that explicit grant.
- `StopAccess()` balances only a successful start. `Close()` also stops and releases the NSURL, and is idempotent; the destructor closes as a last resort. Use `Try`/`Finally` and explicit `Close` when the lifetime is known.

A bookmark is not a permission grant by itself. Sandboxed apps need the appropriate
user-selected-file and app-scoped-bookmark entitlements and a legitimate original
user grant (typically an open panel). The package does not add entitlements,
prompt for permissions or manufacture access. `StartAccess = False` is not proof
that file I/O will fail—already accessible/non-scoped resources can behave that
way. Always check the actual operation's result. Keep an active scope alive until
all consumers, including asynchronous previews, finish. Do not concurrently call
StartAccess/StopAccess/Close on the same wrapper from multiple threads. The read-only creation
option requests reduced access; callers cannot use it to expand an existing grant.

Errors include native NSError descriptions; missing/moved-unresolvable resources
and corrupt bytes throw. Windows/Linux report unsupported and operations throw.

Apple API: [NSURL bookmark data](https://developer.apple.com/documentation/foundation/nsurl/bookmarkdata(options:includingresourcevaluesforkeys:relativeto:)).
