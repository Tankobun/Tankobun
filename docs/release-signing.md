# Official APK signing

Starting with 4.2.2 (versionCode 50), official APKs use a new Tankobun signing key with a proof-of-rotation lineage authorized by the previous key. The minimum Android version remains Android 10 (API 29).

- Previous certificate SHA-256: `52aee0b1f34ffa3c8277f2099c5e447ef18df36d7ed1d4e33ba7acc8b91a77c8`
- Current certificate SHA-256: `e70be3de98b77f0951f523e881b5482a0f0184a16804f37444436f0e6dcfa720`

## Maintainer release procedure

1. Keep the application id `com.tankobun.app` and increase `versionCode` for each release.
2. Build an unsigned release APK. Leave all `tankobunRelease*` properties and corresponding `TANKOBUN_RELEASE_*` environment variables unset. Keep the AniList configuration and use `https://tankobun.github.io/Tankobun/updates.json` for updates.
3. Use `assinar-apk.py` from the private signing backup with Android Build Tools 36 and Java 21. It aligns the APK, signs it with the stored key rotation lineage, and verifies signatures for API 29–32 and 33–36. Keep `--rotation-min-sdk-version 28` so the rotated signer is supported throughout the app's Android range.
4. Test installation over the previous official release without uninstalling, checking that user data remains available.
5. Publish only the final signed APK. Update both update manifests with its exact size and SHA-256, and update the website and release notes.

The private backup includes both keystores, passwords, the lineage, and the reusable signing script. Keep it outside the repository. The Gradle single-keystore signing configuration is suitable for independent builds; it does not apply the official rotation lineage. Do not publish a future official APK signed only with the old key or with the new key without its lineage.

## Installing 4.2.2

Users on 4.2.1 or earlier need to download 4.2.2 once and install it over their existing app. Do not uninstall. Subsequent update checks use the new address, and Android asks the user to confirm installation.
