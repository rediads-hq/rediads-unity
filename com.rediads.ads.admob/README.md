# Rediads for AdMob (Unity)

Rediads bids in AdMob mediation, for games that show ads with Google's Mobile Ads Unity plugin. A Rediads
bid goes into each AdMob request, and AdMob's mediation groups decide through the Rediads custom events.
Version 0.1.0-beta1.

Needs **Rediads Ads** (`com.rediads.ads`) 0.1.0-beta1 and Google's Mobile Ads Unity plugin 11.x, added
your usual way (.unitypackage or OpenUPM).

```csharp
using Rediads.AdMob;

var request = new AdRequest();
RediadsAdMob.AddBid(request, RediadsAdMobAdType.Rewarded, "extra_life", error =>
    RewardedAd.Load(adUnitId, request, (ad, loadError) => { /* as usual */ }),
    RediadsAdFormat.Video);
```

Installation and the full guide: https://github.com/rediads-hq/rediads-unity
