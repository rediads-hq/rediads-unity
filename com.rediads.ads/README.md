# Rediads Ads for Unity

Banners, interstitials and rewarded ads for Unity games on iOS and Android, drawn by the native Rediads
SDKs (iOS 0.1.3, Android 0.1.0-beta1). Version 0.1.0-beta1.

Requires Unity 2022.3 or later, iOS 15 or later, Android 7.0 (API 24) or later, and Unity's External
Dependency Manager (added with this package), which adds the native SDKs to your Xcode and Gradle builds.

```csharp
using Rediads;

RediadsAds.Start("YOUR_ACCOUNT_ID", error => Debug.Log($"Rediads: {error ?? "started"}"));

var banner = new RediadsBanner("home_banner", RediadsBannerSize.Banner320x50, RediadsBannerPosition.Bottom);
banner.Load();                     // Destroy() when the scene ends

var interstitial = new RediadsInterstitial("level_end");
interstitial.Load(error => { if (error == null) interstitial.Show(); });

var rewarded = new RediadsRewarded("extra_life", "lives", 1);
rewarded.OnEvent += e => { if (e.Type == RediadsAdEventType.Rewarded) GrantLife(e.RewardAmount); };
rewarded.Load();                   // later: if (rewarded.IsReady) rewarded.Show();
```

Events arrive on Unity's main thread. In the Editor, Start succeeds and every load fails, so the game's
no-ad path runs. Installation and the full guide: https://github.com/rediads-hq/rediads-unity
