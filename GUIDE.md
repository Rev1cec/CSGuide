# CS Guide — Android Studio project

CS Guide is an offline learning app for Counter-Strike 2. It provides a map catalogue, basic and instant lineup cards, T/CT round calls, economy recommendations, binds, movement advice, and a feedback form.

## Run the project

1. Open `CSGuideAndroid` in Android Studio.
2. Set the Gradle JDK to 17 or newer (the project targets JVM 17).
3. Install Android SDK 36.
4. Run the `app` configuration on an emulator or Android device.

To build a debug APK in Windows:

```powershell
.\gradlew.bat assembleDebug
```

The APK is written to `app/build/outputs/apk/debug/app-debug.apk`.

## Architecture and local data

The project uses a deliberately simple, offline Compose structure without MVVM or a remote backend.

- `MainActivity.kt` starts the Compose application.
- `ui/CSGuideApp.kt` contains the `NavHost`, the bottom navigation, and persistent favorites state.
- `ui/navigation/Routes.kt` defines app destinations and map arguments.
- `data/GameData.kt` is the local catalogue; `filteredMaps()` applies search, category filtering, favorites filtering, and sorting.
- `data/GuidePreferences.kt` stores favorite map keys in DataStore.
- `data/FeedbackValidator.kt` validates the offline feedback form.
- `data/maps/` holds a separate content object for each map.
- `ui/screens/` contains individual Compose screens.

All map content remains local. The app does not require network access, a database, or an account.

## Resources and image attribution

- `res/values/strings.xml` contains Russian interface strings.
- `res/values-en/strings.xml` contains English interface strings.
- `res/xml/locales_config.xml` declares supported locales.
- `res/drawable/` contains legacy project previews.
- `res/drawable-nodpi/` contains official local CS2 map and gameplay reference images used by the application.
- `res/font/` contains the application font family.
- `ui/theme/` defines the Material 3 color palette, typography, and shapes.

Use **Settings → App language** to switch Russian and English. The language choice is saved by AndroidX AppCompat and the current navigation route is retained after the activity is recreated. Favorites remain stored locally in DataStore.

Each lineup can show two distinct official in-game reference views:

1. **Throw-position view** — the relevant starting area before the throw.
2. **Coverage-area view** — a different game view of the area affected by the utility.

The reference images help identify the game areas; follow the detailed text guide for the exact position, aim point, and throw timing. Where a distinct pair is unavailable, the app keeps the text guide and clearly indicates that the second reference image has not been added.

## User guide

1. On the home screen choose **Maps**, **Economy**, or **Tips**.
2. In **Maps**, search by name, open filters, select a category, show only favorites, or sort alphabetically.
3. Open a map to switch between **Basic** and **Instant** lineup cards. Tap a lineup to see its detailed five-step guide: preparation, starting position, aim reference, throw method, and result check. When licensed photos are available, the same screen also shows the throw position and landing result. Use the star to save a map to favorites.
4. Use **Round calls** to switch between T and CT plans for the selected map.
5. In **Economy**, switch sides to read appropriate pistol, eco, force-buy, and full-buy recommendations.
6. In **Tips**, select a category to focus on binds, movement, teamwork, or practice.
7. In **Settings**, select the interface language. The app recreates the current screen in the selected locale without sending the user back to Home.

## Testing

Run the local checks from the project root:

```powershell
.\gradlew.bat lintDebug
```

```powershell
.\gradlew.bat testDebugUnitTest
```

```powershell
.\gradlew.bat assembleDebug
```

With an emulator or physical device connected, run UI tests with:

```powershell
.\gradlew.bat connectedDebugAndroidTest
```

The included unit tests verify map search/filtering/sorting and feedback validation. Manually verify Russian and English UI, system Back navigation, favorites persistence, map detail dialogs, empty search results, feedback errors, and both T/CT selections before submitting the project.

## Content note

The lineup cards are educational prompts. Verify coordinates, aim references, and lineups in the current CS2 version before using them in competitive matches.
