---
title: "bookmarks typed API"
summary: "Source-derived public signatures, properties, parameters, defaults, and return values."
category: "Reference"
order: 21
types: ["ResolvedBookmark"]
dependencies: []
symbols: ["ResolvedBookmark"]
related: ["api.md", "getting-started.md", "index.md"]
---

# bookmarks typed API

Generated from the package source declarations. Platform guards are shown where present; inherited members are documented on the base type. See the [API contracts](api.md) for behavior and limitations.

### BookmarksSupported

`Public Function BookmarksSupported() As Boolean`

No parameters.

#### Returns

| Type |
| --- |
| `Boolean` |

### CreateBookmark

`Public Function CreateBookmark(file As FileSystemItem, securityScoped As Boolean = False, readOnlyAccess As Boolean = True) As Bytes`

Bookmark bytes are not authorization by themselves. Preserve the user's original grant.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `file` | `FileSystemItem` | `ByVal` | Required |
| `securityScoped` | `Boolean` | `ByVal` | `False` |
| `readOnlyAccess` | `Boolean` | `ByVal` | `True` |

#### Returns

| Type |
| --- |
| `Bytes` |

## ResolvedBookmark

`Public Class ResolvedBookmark`

Owns one resolved NSURL and at most one explicit security-scope grant.

### Properties

| Name | Type | Read-Only | Shared | Default | Description |
| --- | --- | --- | --- | --- | --- |
| `IsStale` | `Boolean` | Yes | No | — | — |
| `File` | `FileSystemItem` | Yes | No | — | — |
| `IsAccessing` | `Boolean` | Yes | No | — | — |

### Constructor

`Public Sub Constructor(data As Bytes, securityScoped As Boolean = False)`

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `data` | `Bytes` | `ByVal` | Required |
| `securityScoped` | `Boolean` | `ByVal` | `False` |

Returns: no value.

### StartAccess

`Public Function StartAccess() As Boolean`

Idempotent per wrapper. False is not proof of access; ordinary file I/O can still succeed.

No parameters.

#### Returns

| Type |
| --- |
| `Boolean` |

### StopAccess

`Public Sub StopAccess()`

No parameters.

Returns: no value.

### Refresh

`Public Function Refresh(readOnlyAccess As Boolean) As Bytes`

Recreate stale bytes from the resolved URL, preserving the original scoped/read-only policy explicitly.

#### Parameters

| Name | Type | Passing | Default |
| --- | --- | --- | --- |
| `readOnlyAccess` | `Boolean` | `ByVal` | Required |

#### Returns

| Type |
| --- |
| `Bytes` |

### Close

`Public Sub Close()`

No parameters.

Returns: no value.

### Destructor

`Public Sub Destructor()`

No parameters.

Returns: no value.
