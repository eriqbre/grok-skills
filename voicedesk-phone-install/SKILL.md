---
name: VoiceDesk phone install
description: Use when a VoiceDesk walk-ready tip needs to be built and installed on Eriq’s iPhone, or when he asks if a new build is on the phone.
---

# VoiceDesk phone install

Install a walk-ready VoiceDesk tip on Eriq’s paired iPhone from his Mac. Do not clone. Do not send him commands to copy-paste unless ExternalShell is blocked.

## Paths and device

- Checkout `/Users/eriqbreland/Desktop/projects/voicedesk-v1` (not `~/voicedesk-v1`)
- Device: Qs iPhone 15 Pro Max, id `00008130-000219D01413803A`
- Bundle: `app.voicedesk.ios`
- Scheme / project: VoiceDesk / `VoiceDesk.xcodeproj`
- Secrets: `VoiceDesk/Secrets.plist`
- Empty `DEVELOPMENT_TEAM` is fine

## Steps

```bash
cd /Users/eriqbreland/Desktop/projects/voicedesk-v1
git status
git stash   # skip if clean
git fetch origin
git checkout <branch>
git pull --ff-only origin <branch>
git log -1 --oneline   # expect <SHA>
# Wipe VoiceDesk-* DerivedData and SPM caches. NEVER delete Package.resolved.
rm -rf ~/Library/Developer/Xcode/DerivedData/VoiceDesk-*
rm -rf ~/Library/Caches/org.swift.swiftpm
# do not delete VoiceDesk.xcodeproj/project.xcworkspace/xcshareddata/swiftpm/Package.resolved
xcodebuild -resolvePackageDependencies -project VoiceDesk.xcodeproj -scheme VoiceDesk
# if Promises/FBLPromises fails, retry the resolve once
xcodebuild -project VoiceDesk.xcodeproj -scheme VoiceDesk -configuration Debug \
  -destination 'id=00008130-000219D01413803A' \
  -derivedDataPath VoiceDesk-phone \
  build
xcrun devicectl device install app --device 00008130-000219D01413803A \
  VoiceDesk-phone/Build/Products/Debug-iphoneos/VoiceDesk.app
xcrun devicectl device process launch --device 00008130-000219D01413803A \
  app.voicedesk.ios
```

Then tell Eriq the SHA, the branch, and that he is waiting on the phone.
