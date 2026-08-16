# loader-ios (kmljkjj fork)

Full fork of [unbound-app/loader-ios](https://github.com/unbound-app/loader-ios) with a **hardened UnboundNative JSI bridge** and a **Build IPA** GitHub Action.

## Fix: `UnboundNative module not found`

See [FIXES.md](./FIXES.md) and [patches/unbound-native-fix.patch](./patches/unbound-native-fix.patch).

Apply the patch on a clean tree:

```bash
git apply patches/unbound-native-fix.patch
```

(Or merge the changes into `sources/Unbound.xm` and `sources/UnboundNative.xm`.)

## Build IPA (GitHub Actions)

1. Go to **Actions** → **Build IPA** → **Run workflow**
2. Fill **ipa_url** with a **direct download link** to a decrypted Discord IPA
3. Optional:
   - `release` = true → creates a GitHub Release with IPA + deb
   - `is_testflight` = true if the IPA is TestFlight
4. Download the artifact **discord-ios-loader-ipa** (or the Release)
5. Sideload with **TrollStore / AltStore / SideStore**

### What the workflow does

1. Builds the Theos tweak (`.deb`) with [theos-action](https://github.com/Randomblock1/theos-action)
2. Downloads your Discord IPA
3. Runs [unbound-app/patcher-ios](https://github.com/unbound-app/patcher-ios)
4. Injects the tweak with **cyan** (pyzule-rw)
5. Uploads the IPA + optional Release

You **must** provide a decrypted IPA URL (Discord does not publish those publicly).

## Jailbreak install

```bash
make package
# install the .deb from packages/
```

Depends on **ellekit**.

## Requirements

- ElleKit-compatible injection (sideload IPA from this workflow uses cyan)
- Discord iOS with **New Architecture** enabled (same as upstream)
- For local builds: macOS + Xcode + Theos

## Credits

- Upstream: [unbound-app/loader-ios](https://github.com/unbound-app/loader-ios) (GPL-3.0)
- UnboundNative hardening + Build IPA workflow: this fork

## License

GPL-3.0
