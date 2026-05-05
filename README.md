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
