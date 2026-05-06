# VHSify release and monetization checklist

Last updated: 2026-05-06

## Current app state

- App name: `VHSify`
- iOS bundle ID: `com.ochamecompany.vhsify`
- Version in `pubspec.yaml`: `1.0.6+7`
- Current monetization implementation: iOS AdMob banner ads and a rewarded ad before conversion.
- Current paid purchase implementation: none. There is no in-app purchase, subscription, StoreKit purchase flow, or RevenueCat code in this repo.

## GitHub Pages / app-ads.txt

- Publish the contents of `docs/` with GitHub Pages.
- The file that AdMob needs is `app-ads.txt`.
- Current derived AdMob seller line:
  `google.com, pub-4599068365771807, DIRECT, f08c47fec0942fa0`
- Verify the exact seller line in AdMob before final release: AdMob can show a personalized snippet, and that should be treated as the source of truth.
- Critical AdMob detail: AdMob crawls the developer website host and expects `https://<developer-site-host>/app-ads.txt`. A project Pages URL such as `https://tetokasane.github.io/VhsLook-main/` may display `app-ads.txt` under the repo path, but crawlers can strip the path and look at `https://tetokasane.github.io/app-ads.txt`.
- Safest setup: use a custom domain for this Pages site, or publish from the `tetokasane.github.io` user site repo so `app-ads.txt` is available at the hostname root.

## App-side release checks

- iOS currently has release AdMob IDs in code and `Info.plist`; verify they match the iOS AdMob app before upload.
- iOS release ad unit IDs can be overridden with `--dart-define`:
  - `ADMOB_IOS_TOP_BANNER_UNIT_ID`
  - `ADMOB_IOS_BOTTOM_BANNER_UNIT_ID`
  - `ADMOB_IOS_CONVERSION_REWARDED_UNIT_ID`
- UMP consent is collected before `MobileAds.initialize()` and ads are requested only after `canRequestAds()` is true.
- ATT is wired through `AppTrackingTransparency.framework` with `NSUserTrackingUsageDescription`.
- In AdMob Privacy & messaging, publish the GDPR/US state messages you need and an iOS IDFA explainer message if you want UMP to explain ATT before the system prompt.
- The app opens `Privacy Policy` and `Terms of Use` inside an in-app WebView. Override the base URL for release with:
  `--dart-define=VHSIFY_LEGAL_BASE_URL=https://your-developer-site.example`

## Store submission fields

- Privacy Policy URL: `https://reakinoko.github.io/privacy.html`
- Terms URL, if requested: `https://reakinoko.github.io/terms.html`
- App Store Marketing URL: `https://reakinoko.github.io/`
- App Store Support URL: use a working support page or the store account's support contact.
- Do not declare in-app purchases or subscriptions unless billing is implemented and product IDs are configured in the stores.
- If a paid feature is added later, use Apple In-App Purchase for iOS digital features.
- App Privacy declarations should include media selection for app functionality, Google AdMob data collection for advertising, and tracking/IDFA if personalized ads or ad measurement use tracking.
