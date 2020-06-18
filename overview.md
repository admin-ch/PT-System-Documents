Overview of the Swiss Proximity Tracing System (PT-S)
=====================================================

The Swiss Proximity Tracing System provide users with a SwissCovid Android or iOS smartphone app to enable exposure notification. The app notifies user when they have been exposed to a COVID-19 positive user who also uses the SwissCovid app.

The SwissCovid apps use the [Google/Apple Exposure Notification (GAEN) System](https://www.apple.com/covid19/contacttracing/) to enable privacy-friendly exposure measurement. When activated, the GAEN system transmits ephemeral Bluetooth identifiers which are received and recorded by nearby other SwissCovid apps.

To enable exposure computation, the GAEN API requires a list of diagnosis keys corresponding to COVID-19 positive users. The Swiss Proximity Tracing System operates backend infrastructure to ensure that the diagnosis keys of COVID-19 positive users (and only of those users) are distributed to all SwissCovid apps.

The backend infrastructure consists of two major backends. The SwissCovid app backend receives authenticated diagnosis keys from SwissCovid apps, and publishes diagnosis keys for download by other apps. The Health Authority (HA) Auth-Code Generation Service authenticates uploads of diagnosis keys by COVID-19 positive users upon authorization from a health official. Finally, the infrastructure also provides a Config Service to enable pushing new configurations to SwissCovid apps.

SwissCovid mobile applications
------------------------------

SwissCovid provides two mobile applications:

-   [The Android SwissCovid App](https://github.com/DP-3T/dp3t-app-android-ch) Internally, this app uses the [DP3T SDK for Android](https://github.com/DP-3T/dp3t-sdk-android) to handle the low-level interactions with the GAEN API.
-   [iOS SwissCovid App](https://github.com/DP-3T/dp3t-app-ios-ch) Internally, this app uses the [DP3T SDK for iOS](https://github.com/DP-3T/dp3t-sdk-ios) to handle low-level interactions with the GAEN API.

We refer to the [UI descriptions](https://github.com/DP-3T/dp3t-ux-screenflows-ch) for an overview of the main screens and user-interactions.
