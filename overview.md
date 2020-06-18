Overview of the Swiss Proximity Tracing System (PT-S)
=====================================================

The Swiss Proximity Tracing System provide users with a SwissCovid Android or iOS smartphone app to enable exposure notification. The app notifies user when they have been exposed to a COVID-19 positive user who also uses the SwissCovid app.

The SwissCovid apps use the [Google/Apple Exposure Notification (GAEN) System](https://www.apple.com/covid19/contacttracing/) to enable privacy-friendly exposure measurement. When activated, the GAEN system transmits ephemeral Bluetooth identifiers which are received and recorded by nearby other SwissCovid apps.

To enable exposure computation, the GAEN API requires a list of diagnosis keys corresponding to COVID-19 positive users. The Swiss Proximity Tracing System operates backend infrastructure to ensure that the diagnosis keys of COVID-19 positive users (and only of those users) are distributed to all SwissCovid apps.

The backend infrastructure consists of two major backends. The SwissCovid app backend receives authenticated diagnosis keys from SwissCovid apps, and publishes diagnosis keys for download by other apps. The Health Authority (HA) Auth-Code Generation Service authenticates uploads of diagnosis keys by COVID-19 positive users upon authorization from a health official. Finally, the infrastructure also provides a Config Service to enable pushing new configurations to SwissCovid apps.

SwissCovid mobile applications
------------------------------

SwissCovid provides two mobile applications:

-   [Android SwissCovid App](https://github.com/DP-3T/dp3t-app-android-ch) Internally, this app uses the [DP3T SDK for Android](https://github.com/DP-3T/dp3t-sdk-android) to handle the low-level interactions with the GAEN API.
-   [iOS SwissCovid App](https://github.com/DP-3T/dp3t-app-ios-ch) Internally, this app uses the [DP3T SDK for iOS](https://github.com/DP-3T/dp3t-sdk-ios) to handle low-level interactions with the GAEN API.

We refer to the [UI descriptions](https://github.com/DP-3T/dp3t-ux-screenflows-ch) for an overview of the main screens and user-interactions.

Apps download new batches of diagnosis keys from the SwissCovid app backend service twice a day (currently around 6am and 6pm). The app then uses the GAEN API to compute the user\'s exposure and informs the user if the exposure is too high.

If a user is diagnosed with COVID-19, they are asked to contact the Cantonal Doctor. After confirming the diagnosis, the doctor will login to the HA Auth-Code Generation Service and specify the test date. The service computes the onset of symptoms date (currently 2 days before the test date), and generates a 12-digit COVID Code. The doctor relays this COVID code to the user. Users enter this code in their app to enable the upload of their keys. The app validates the code at the HA Auth-Code Generation Service to receive an authentication token; queries the GAEN API to receive the daily diagnosis keys for the days during which the user was contagious; and uploads the diagnosis keys to the SwissCovid app backend service.

Apps make fake requests to both backends to hide from network observers whether the user is COVID-19 positive. SwissCovid apps follows the approach outlined in [The OpSec Document (SOON TO BE RELEASED](https://ontheinternet.com) and generate fake actions (that include validating a Covidcode and uploading keys) according to a Poisson distribution with a rate of one fake action per 5 days.

Every 6 hours (and at app start), apps retrieve a new configuration from the Config server.

SwissCovid backend infrastructure
---------------------------------

The SwissCovid backend infrastructure operates three services: the HA Auth-Code Generation Service, the SwissCovid app backend service, and the Config service.

### Health Authority Side (black)

The [HA Auth-Code Generation Service](https://github.com/admin-ch/CovidCode-Service) is responsible for generating and validating 12-digit Covidcodes. Doctors interact with this backend service using a [website/UI](https://github.com/admin-ch/CovidCode-UI/) to request Covidcodes for their patients. Doctors use government IAM (eIAM) service to authenticate themselves to the HA Auth-Code generation Service.

The HA Auth-Code Generation Service temporarily stores generated Covidcodes and the corresponding onset dates in a database. After the user entered their Covidcode into the SwissCovid app, the app makes a POST request to the HA Auth-Code Generation Service to validate the code. If the code is valid, the backend returns a signed JWT token to the SwissCovid app. The JWT contains the onset date.

To enable the use of fake actions, the HA Auth-Code Generation Service accept POST requests with fake COVID Codes. It handles them exactly the same as real requests, but returns a signed JWT token that is also marked as fake.
