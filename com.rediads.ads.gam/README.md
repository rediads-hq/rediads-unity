# Rediads for Google Ad Manager (Unity)

Rediads in Google Ad Manager (GAM), for games that show GAM ads with Google's Mobile Ads Unity plugin.
Two ways, chosen per slot, and GAM decides the winner in both: bidding-only (`RediadsGAM.AddBid`, GAM
draws a Rediads win) and SDK-rendered (`RediadsGAMBanner`, `RediadsGAMInterstitial`,
`RediadsGAMRewarded`, the Rediads SDK draws a Rediads win). Version 0.1.0-beta1.

Needs **Rediads Ads** (`com.rediads.ads`) 0.1.0-beta1 and Google's Mobile Ads Unity plugin 11.x, added
your usual way (.unitypackage or OpenUPM).

```csharp
using GoogleMobileAds.Api.AdManager;
using Rediads.GAM;

var request = new AdManagerAdRequest();
RediadsGAM.AddBid(request, RediadsGAMAdType.Interstitial, "level_end", error =>
    AdManagerInterstitialAd.Load("/NETWORK_CODE/level_end", request, (ad, loadError) => { /* as usual */ }));
```

Installation and the full guide: https://github.com/rediads-hq/rediads-unity
