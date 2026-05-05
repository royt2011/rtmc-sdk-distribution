# rtmc-sdk-distribution
RTMC SDK
RTMC SDK is an Android library used for event tracking, analytics, and backend integration across multiple applications.

The SDK is distributed via GitHub Packages (Maven repository) and is designed to be used as a dependency without exposing source code.

Overview
RTMC SDK provides:

Event tracking API
Firebase Analytics integration
Crash reporting support
Lightweight and scalable architecture
Easy integration via Gradle dependency
Versioned releases via GitHub Packages
Installation
1. Add GitHub Packages repository
In your root settings.gradle.kts or project build.gradle.kts:

maven {
    url = uri("https://maven.pkg.github.com/royt2011/rtmc-sdk-distribution")

    credentials {
        username = project.findProperty("gpr.user") as String?
        password = project.findProperty("gpr.key") as String?
    }
}
## 2. Add credentials (LOCAL ONLY!!!)
Add to gradle.properties:
gpr.user=YOUR_GITHUB_USERNAME

gpr.key=YOUR_GITHUB_PERSONAL_ACCESS_TOKEN
Required token permissions:

* read:packages
* write:packages (for publishing)
* repo ()

⚠️ NEVER commit this file to Git(with credentials)
---

# Usage in another application

After adding the repository and credentials, you can use RTMC SDK in any Android application as a dependency.

## 1. Add dependency

In your app-level `build.gradle.kts`:

```kotlin

dependencies {

implementation("com.royt2011.rtmc.distribution:sdk:1.0.1")
}
and in  settings.gradle
   maven {

            url = uri("https://maven.pkg.github.com/royt2011/rtmc-sdk-distribution)

            credentials {

                username = providers.gradleProperty("gpr.user").orNull

                password = providers.gradleProperty("gpr.key").orNull

            }

        }
        }
----------------------------------------------------------------------------------------------------------------------------------------
\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \
                                                                      Implementation Of Revenue Cat
\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \
----------------------------------------------------------------------------------------------------------------------------------------


📦 Paywall Module (RevenueCat Integration)

This module provides a simple wrapper around the RevenueCat Paywall UI for Android.
It allows any app to:

Initialize RevenueCat with a single function

Launch the RevenueCat Paywall using an ActivityResultLauncher

Receive purchase, restore, and cancel events via a callback

Set user attributes such as name and email

This makes RevenueCat integration reusable, clean, and easy to maintain across multiple apps.

🚀 Features

✔ One-line initialization of RevenueCat SDK

✔ Simple PaywallManager class for launching paywalls

✔ PaymentCallback interface for getting results

✔ Ability to save attributes (email, displayName, name)

✔ Works inside Activities & Fragments

✔ Designed for modular Android projects

📁 Module Structure
PaywallManager

Handles:

Launching the paywall

Setting RevenueCat subscriber attributes

Sending paywall results back through PaymentCallback

class PaywallManager(
caller: ActivityResultCaller,
private val callback: PaymentCallback,
) : PaywallResultHandler {

    private val launcher = PaywallActivityLauncher(caller, this)

    fun openPaywall(entitlementId: String) {
        launcher.launch()
    }

    fun saveAttributes(name: String, displayName: String, email: String) {
        Purchases.sharedInstance.setAttributes(
            mapOf(
                "\$displayName" to displayName,
                "\$email" to email
            )
        )
        Purchases.sharedInstance.setEmail(email)
        Purchases.sharedInstance.setDisplayName(displayName)
    }

    override fun onActivityResult(result: PaywallResult) {
        when (result) {
            is PaywallResult.Purchased -> callback.onPaymentResult(true, "Purchase completed")
            is PaywallResult.Restored -> callback.onPaymentResult(true, "Purchase restored")
            is PaywallResult.Cancelled -> callback.onPaymentResult(false, "User cancelled")
            else -> callback.onPaymentResult(false, "Unknown result")
        }
    }

}

Payment Callback Interface
interface PaymentCallback {
fun onPaymentResult(success: Boolean, message: String)
}

⚙️ RevenueCat Initialization

The module provides a simple entry point:

MyLibrary.initialize()
object MyLibrary {
fun initialize(context: Context, revenueCatApiKey: String) {
RevenueCatInitialized.init(context, revenueCatApiKey)
}
}

RevenueCat setup:
object RevenueCatInitialized {
fun init(context: Context, api: String) {
Purchases.logLevel = LogLevel.DEBUG
Purchases.configure(
PurchasesConfiguration.Builder(context, api).build()
)
}
}

 How to Implement in Your App

Follow these steps to use the module inside any Android app.

Step 1 — Initialize in Application class
class MyApp : Application() {
override fun onCreate() {
super.onCreate()

        MyLibrary.initialize(
            this,
            revenueCatApiKey = "your_public_sdk_key"
        )
    }

}

Add to manifest:

<application
android:name=".MyApp">

Step 2 — Use PaywallManager in your Activity
@AndroidEntryPoint
class MainActivity : AppCompatActivity(), PaymentCallback {

    private lateinit var paywallManager: PaywallManager

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        paywallManager = PaywallManager(this, this)
        showPaywall()
    }

    private fun showPaywall() {
        paywallManager.saveAttributes("name", "displayName", "email@example.com")
        paywallManager.openPaywall("")
    }

    override fun onPaymentResult(success: Boolean, message: String) {
        if (success) {
            // User purchased or restored
        } else {
            // User cancelled or error occurred
        }
    }

}

✅ Summary

This library makes it extremely easy to:

Integrate RevenueCat

Display paywalls

Receive purchase events

Store subscriber attributes

Keep billing code modular and reusable

----------------------------------------------------------------------------------------------------------------------------------------
\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ 
                                                                         Implementation Of Ads Mobs
\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \
----------------------------------------------------------------------------------------------------------------------------------------

AdMob Integration with Firebase Remote Config
Overview

This repository provides a comprehensive solution for integrating AdMob Ads (Native, Interstitial, and Rewarded) into an Android project with dynamic configuration using Firebase Remote Config. The app fetches configuration values (like Ad IDs) from Firebase, allowing you to manage your ads remotely without needing to update the app each time.

Features:

AdMob Integration: Easily manage Native Ads, Interstitial Ads, and Rewarded Ads.

Remote Config: Fetch ad-related values (Ad IDs, types, etc.) from Firebase in real-time.

Customizable: Change AdMob IDs, ad types, and intervals directly from the Firebase Console.

Setup and Configuration
1. Setup Firebase Project

Go to the Firebase Console
.

Create a new project or use an existing one.

Add your Android app to the Firebase project and download the google-services.json file.

Place the google-services.json file in the app/ directory of your Android project.

2. Integrate Firebase and AdMob SDKs

Project-level build.gradle:

buildscript {
repositories {
google()
}
dependencies {
classpath 'com.google.gms:google-services:4.3.15' // Add Google services classpath
}
}


App-level build.gradle:

apply plugin: 'com.google.gms.google-services'  // Add this line at the top of the file

dependencies {
implementation 'com.google.firebase:firebase-config:21.1.1'  // Firebase Remote Config SDK
implementation 'com.google.android.gms:play-services-ads:22.0.0'  // AdMob SDK
}

3. Set Up Firebase Remote Config

In your Firebase Console, go to Remote Config and set up the following parameters (or customize them as needed):

ads_enabled: Boolean value (true or false) to enable or disable ads.

ads_type_home: Ad type for the home screen (e.g., "native").

ads_type_details: Ad type for the details screen (e.g., "banner").

interstitial_interval: Interval for displaying interstitial ads in seconds.

AdMob Ad Unit IDs (replace these with your actual ad unit IDs):

admob_banner_id

admob_native_id

admob_interstitial_id

admob_rewarded_id

admob_app_open_id (optional)

Example of Remote Config setup:

{
"ads_enabled": true,
"ads_type_home": "native",
"ads_type_details": "banner",
"interstitial_interval": 3,
"admob_banner_id": "ca-app-pub-3940256099942544/6300978111",
"admob_native_id": "ca-app-pub-3940256099942544/2247696110",
"admob_interstitial_id": "ca-app-pub-3940256099942544/1033173712",
"admob_rewarded_id": "ca-app-pub-3940256099942544/5224354917",
"admob_app_open_id": "ca-app-pub-3940256099942544/3419835294"
}

4. Initialize Firebase and Remote Config

In your Application class (or any other appropriate location), initialize Firebase and Remote Config:

class MyApplication : Application() {
override fun onCreate() {
super.onCreate()
FirebaseApp.initializeApp(this)

        // Fetch and activate Remote Config values
        lifecycleScope.launch {
            initRemoteConfig() // Fetch ad settings from Firebase Remote Config
        }
    }
}


In the initRemoteConfig() function (as provided in your code), Firebase Remote Config is initialized, and default ad values are set.

suspend fun initRemoteConfig() {
val remoteConfig = Firebase.remoteConfig

    val settings = remoteConfigSettings {
        minimumFetchIntervalInSeconds = 3600 // Fetch config every hour
    }

    remoteConfig.setConfigSettingsAsync(settings).await()

    // Set default values for the ads
    remoteConfig.setDefaultsAsync(mapOf(
        "ads_enabled" to true,
        "ads_type_home" to "native",
        "ads_type_details" to "banner",
        "interstitial_interval" to 3,
        "admob_banner_id" to "ca-app-pub-3940256099942544/6300978111",
        "admob_native_id" to "ca-app-pub-3940256099942544/2247696110",
        "admob_interstitial_id" to "ca-app-pub-3940256099942544/1033173712",
        "admob_rewarded_id" to "ca-app-pub-3940256099942544/5224354917",
        "admob_app_open_id" to "ca-app-pub-3940256099942544/3419835294"
    )).await()

    try {
        val updated = remoteConfig.fetchAndActivate().await()
        Log.d("RemoteConfig", "Remote config updated: $updated")
    } catch (e: Exception) {
        Log.e("RemoteConfig", "Error fetching remote config", e)
    }
}

5. Create AdsViewModel and Repository

You can use the AdsViewModel and AdsRepository classes provided in the code to manage and show ads in your activity.

AdsViewModel: Manages the logic for loading and showing ads (Native, Interstitial, and Rewarded).

AdsRepository: Handles the actual ad loading logic and interactions with the AdMob API.

Example AdsViewModel setup:

class AdsViewModel(private val repo: AdsRepository) : ViewModel() {

    private val _nativeAd = MutableLiveData<NativeAd?>()
    val nativeAd: LiveData<NativeAd?> get() = _nativeAd

    private val _interstitialReady = MutableLiveData<Boolean>()
    val interstitialReady get() = _interstitialReady

    private val _rewardedAd = MutableLiveData<RewardedAd?>()
    val rewardedAd get() = _rewardedAd

    lateinit var adRequest: AdRequest

    fun getMobAdds(view: AdView) {
        adRequest = AdRequest.Builder().build()
        view.loadAd(adRequest)
    }

    fun loadInterstitial() {
        repo.loadInterstitial(
            onLoaded = { _interstitialReady.postValue(true) },
            onFailed = { _interstitialReady.postValue(false) }
        )
    }

    fun showInterstitial(activity: Activity, afterDismiss: () -> Unit) {
        repo.showInterstitial(activity) {
            afterDismiss()
            loadInterstitial() // Reload the interstitial ad after it is shown
        }
    }

    fun loadNativeAd() {
        repo.loadNativeAd(
            onLoaded = { _nativeAd.postValue(it) },
            onFailed = { _nativeAd.postValue(null) }
        )
    }

    fun loadRewardedAd() {
        repo.loadRewardedAd(
            onLoaded = { _rewardedAd.postValue(it) },
            onFailed = { _rewardedAd.postValue(null) }
        )
    }
}


In your MainActivity, observe the AdsViewModel to update the UI with ads:

class MainActivity : AppCompatActivity() {
private lateinit var viewModel: AdsViewModel
lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = DataBindingUtil.setContentView(this, R.layout.activity_main)

        viewModel = ViewModelProvider(this, AdViewModelFactory(this)).get(AdsViewModel::class.java)

        // Observe ads
        observeAds()

        // Load ads
        viewModel.getMobAdds(binding.adViewContainer.adView)
        viewModel.loadInterstitial()
        viewModel.loadNativeAd()
        viewModel.loadRewardedAd()

        // Fetch Remote Config values
        getFirebase()
    }

    private fun observeAds() {
        // Native Ad
        viewModel.nativeAd.observe(this) { ad ->
            if (ad != null) {
                AdsManager.showNativeAd(ad, binding.nativeAdContainer)
            }
        }

        // Rewarded Ad
        viewModel.rewardedAd.observe(this) { ad ->
            // Handle rewarded ad showing
        }

        // Interstitial Ad
        viewModel.interstitialReady.observe(this) { isReady ->
            if (isReady) {
                // Show interstitial ad when ready
                viewModel.showInterstitial(this) {
                    Log.d("Ads", "Interstitial dismissed")
                }
            }
        }
    }

    private fun getFirebase() {
        lifecycleScope.launch {
            Log.d("RC", "Ads Enabled = ${RemoteConfigManager.adsEnabled}")
            Log.d("RC", "Banner ID = ${RemoteConfigManager.bannerId}")
            Log.d("RC", "Native ID = ${RemoteConfigManager.nativeId}")
            Log.d("RC", "Interstitial ID = ${RemoteConfigManager.interstitialId}")
            Log.d("RC", "Rewarded ID = ${RemoteConfigManager.rewardedId}")
        }
    }
}

Conclusion

This setup allows you to integrate AdMob Ads into your Android application while using Firebase Remote Config for dynamic configuration management. This solution enables you to change AdMob ad unit IDs, ad types, and other ad-related settings directly from the Firebase Console without requiring an app update.

----------------------------------------------------------------------------------------------------------------------------------------
\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \
                                                                        Implementation Of AI Module
\ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \ \
----------------------------------------------------------------------------------------------------------------------------------------

