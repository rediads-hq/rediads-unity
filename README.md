# Rediads for Unity

Banners, interstitials and rewarded ads for Unity games on iOS and Android, drawn by the native
Rediads SDKs. Optional packages add Rediads bids to AdMob mediation and Google Ad Manager.

| Package | What | Needs |
|---|---|---|
| `com.rediads.ads` | Rediads banners, interstitials and rewarded ads | |
| `com.rediads.ads.admob` | Rediads bids in AdMob mediation | `com.rediads.ads`, Google Mobile Ads Unity plugin 11.x |
| `com.rediads.ads.gam` | Rediads in Google Ad Manager (bidding-only or SDK-rendered) | `com.rediads.ads`, Google Mobile Ads Unity plugin 11.x |

This version: **0.1.0-beta1**, with the iOS SDK 0.1.3 and the Android SDK 0.1.0-beta1.

Requires Unity 2022.3 or later, iOS 15 or later and Android 7.0 (API 24) or later.

## 1. Before you start

Ask Rediads for an **account ID**, and have your game's bundle ID (iOS) and package name (Android)
registered for it. The ad server rejects apps that aren't registered.

## 2. Add the package

Pick **one** way.

**Package Manager (Git URL).** In Unity: **Window > Package Manager > + > Add package from git URL**,
and enter:

```
https://github.com/rediads-hq/rediads-unity.git?path=com.rediads.ads#0.1.0-beta1
```

For AdMob or Google Ad Manager, add that package's URL too, after the first one:

```
https://github.com/rediads-hq/rediads-unity.git?path=com.rediads.ads.admob#0.1.0-beta1
https://github.com/rediads-hq/rediads-unity.git?path=com.rediads.ads.gam#0.1.0-beta1
```

Or in `Packages/manifest.json`:

```json
"com.rediads.ads": "https://github.com/rediads-hq/rediads-unity.git?path=com.rediads.ads#0.1.0-beta1",
```

**.unitypackage.** Download `Rediads-0.1.0-beta1.unitypackage` (and `RediadsAdMob-…` or `RediadsGAM-…`
if you need them) from the [release](https://github.com/rediads-hq/rediads-unity/releases/tag/0.1.0-beta1),
then **Assets > Import Package > Custom Package**. The files go to `Assets/Rediads/`. This way needs
Unity's External Dependency Manager in the project; Google's Mobile Ads plugin brings it, otherwise
add it from https://github.com/googlesamples/unity-jar-resolver.

Google's Mobile Ads plugin, for the AdMob and Google Ad Manager packages, is added your usual way
(Google's .unitypackage or OpenUPM).

## 3. Native SDKs

The External Dependency Manager adds the native SDKs when you build:

- **Android**: `com.rediads:rediads-display` (and `rediads-admob` / `rediads-gam`) from Maven Central.
  If your build shrinks code (minify), the packages carry their own keep rules.
- **iOS**: the Rediads pods, from Rediads' pod list (`source 'https://github.com/rediads-hq/rediads-ios.git'`
  in the generated Podfile), and CocoaPods' own source for Google's pods.

iOS notes:
- Build for devices. The plugin's iOS libraries are for iPhone and iPad (arm64), not the Simulator.
- Google's pods declare iOS 12, which current Xcode rejects ("deployment target ... is set to 12.0").
  Raise them to iOS 15 in an Editor script after the Podfile is written, or in the Podfile's
  `post_install`.

## 4. Show ads

```csharp
using Rediads;

RediadsAds.Start("YOUR_ACCOUNT_ID", error => Debug.Log($"Rediads: {error ?? "started"}"));

var banner = new RediadsBanner("home_banner", RediadsBannerSize.Banner320x50, RediadsBannerPosition.Bottom);
banner.OnEvent += e => Debug.Log(e);
banner.Load();                     // Destroy() when the scene ends

var interstitial = new RediadsInterstitial("level_end");
interstitial.Load(error => { if (error == null) interstitial.Show(); });

var rewarded = new RediadsRewarded("extra_life", "lives", 1);
rewarded.OnEvent += e => { if (e.Type == RediadsAdEventType.Rewarded) GrantLife(e.RewardAmount); };
rewarded.Load();                   // later: if (rewarded.IsReady) rewarded.Show();
```

Events arrive on Unity's main thread. On Android, Unity pauses while a full-screen ad covers it, so
that ad's events (the reward included) arrive when the game resumes. In the Editor, Start succeeds
and every load fails, so the game's no-ad path runs.

**AdMob mediation** (`com.rediads.ads.admob`): add a Rediads bid to each AdMob request; AdMob's
mediation groups decide through the Rediads custom events.

```csharp
using Rediads.AdMob;

var request = new AdRequest();
RediadsAdMob.AddBid(request, RediadsAdMobAdType.Rewarded, "extra_life", error =>
    RewardedAd.Load(adUnitId, request, (ad, loadError) => { /* as usual */ }),
    RediadsAdFormat.Video);
```

**Google Ad Manager** (`com.rediads.ads.gam`): bidding-only, where GAM draws a Rediads win
(`RediadsGAM.AddBid` on your `AdManagerAdRequest`), or SDK-rendered, where the Rediads SDK draws it
(`RediadsGAMBanner`, `RediadsGAMInterstitial`, `RediadsGAMRewarded`). Ask Rediads for the GAM and
AdMob setup guides.

## License

Apache License, Version 2.0: [LICENSE.md](LICENSE.md).
