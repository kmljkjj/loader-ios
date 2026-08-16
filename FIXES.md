# Fixes applied for `UnboundNative module not found`

## Error

```
Unbound failed to initialize: Error:
UnboundNative module not found at anonymous (/tmp/hermesc/….js:…)
```

## Cause

The Unbound JS client does:

```ts
export const UnboundNative = globalThis.UnboundNative;
if (!UnboundNative) { /* fail */ }
```

The native tweak must install a JSI global named `UnboundNative` **before** the client Hermes bundle runs.

## Fixes in this fork

| Change | File |
|--------|------|
| Register native interop as soon as `RCTHost` initializes the runtime | `sources/Unbound.xm` |
| Re-install + verify before every client bundle eval | `sources/Unbound.xm` |
| Abort client eval with a clear alert if still missing | `sources/Unbound.xm` |
| Mirror binding to `globalThis` / `window` / `self` after install | `sources/UnboundNative.xm` |

## Build IPA (GitHub Actions)

1. Open **Actions** → **Build IPA** → **Run workflow**
2. Paste a **direct URL** to a decrypted Discord IPA (`ipa_url`)
3. Wait for the job — download the IPA artifact (or GitHub Release)
4. Sideload with TrollStore / AltStore / SideStore

Requires **ElleKit**-compatible injection (cyan injects the tweak).

## Apply source patches

If `sources/Unbound.xm` is still upstream:

```bash
git apply patches/unbound-native-fix.patch
```
