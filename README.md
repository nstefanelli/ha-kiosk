Home Assistant for Apple Platforms (Kiosk Mode Fork)
=================

> **This is a fork of [home-assistant/iOS](https://github.com/home-assistant/iOS) with Kiosk Mode functionality for wall-mounted tablets and dedicated displays.**

[![License Apache 2.0](https://img.shields.io/badge/license-Apache%202.0-green.svg?style=flat)](https://github.com/home-assistant/iOS/blob/master/LICENSE)
[![Feature Request](https://img.shields.io/badge/HA-Feature%20Request-41BDF5)](https://github.com/orgs/home-assistant/discussions/2403)

## Kiosk Mode Features

This fork adds a comprehensive **Kiosk Mode** designed for wall-mounted tablets and dedicated Home Assistant displays:

### Display Control
- **Screensaver** - Clock, photos, or custom URL with configurable timeout
- **Prevent Auto-Lock** - Keep screen on without Guided Access
- **Brightness control** via notifications

### Security & Lockdown
- **Navigation lockdown** - Disable swipe gestures and browser navigation
- **Edge protection** - Block accidental touches near screen edges
- **PIN/FaceID protection** - Secure exit from kiosk mode
- **Secret exit gesture** - Configurable multi-tap in corner to access settings

### Remote Control (via HA Notifications)
- `command_screen_on` / `command_screen_off` - Wake or sleep display
- `command_navigate` - Navigate to any dashboard
- `command_refresh` - Reload current page
- `command_show_camera` - Show camera popup (great for doorbells)
- `command_tts` - Text-to-speech announcements
- [Full command reference](Sources/App/Kiosk/README-KIOSK.md)

### Automation Triggers
- Wake on motion/presence detection from cameras
- Wake on entity state changes
- Dashboard switching based on triggers

---

*Interested in this feature in the official app? Vote on the [Feature Request Discussion](https://github.com/orgs/home-assistant/discussions/2403).*

---

## Getting Started

Home Assistant uses Bundler, Homebrew and Cocoapods to manage build dependencies. You'll need Xcode 26.2 (or later) which you can download from the [App Store](https://developer.apple.com/download/). You can get the app running using the following commands:

```bash
git clone https://github.com/home-assistant/iOS.git
cd iOS

# you must do one of the following, but you do not need to do all of them:

## install cocoapods via homebrew, use that
brew install cocoapods
$(brew --prefix)/opt/ruby/bin/gem install cocoapods-acknowledgements
pod install --repo-update

## install ruby via homebrew, use that
brew install ruby@3.1
$(brew --prefix)/opt/ruby@3.1/bin/bundle install
$(brew --prefix)/opt/ruby@3.1/bin/bundle exec pod install --repo-update

## install ruby via rbenv, use that
brew install rbenv ruby-build
rbenv install
bundle install
bundle exec pod install --repo-update
```

Once this completes, you can launch  `HomeAssistant.xcworkspace` and run the `App-Debug` scheme onto your simulator or iOS device.

## Testing just the frontend

To just test the [frontend](https://github.com/home-assistant/frontend), you can use a simulator version built by our GitHub actions.

1. Install Xcode from the [App Store](https://developer.apple.com/download/) making sure it's at least the version noted above. You do not need to install or run anything else.
2. Launch the simulator at `/Applications/Xcode.app/Contents/Developer/Applications/Simulator.app` or in Xcode under the Xcode menu > Open Developer Tool.
3. Open a simulator under File > Open Simulator. You can install older versions of iOS in Xcode's Components preferences.
4. Download a simulator build from the [the GitHub action](https://github.com/home-assistant/iOS/actions/workflows/ci.yml?query=branch%3Amaster) under "Artifacts."
5. Drag the result `.app` on drop it on top of the simulator.
6. Locate the app on the home screen and click it to launch.

The simulator behaves different than you might expect:

| Action | Effect |
| -- | -- |
| Click | Tap |
| Click & drag | Scroll |
| Hold ⌥ | Add a second touch point |
| Hold ⇧⌥ | Move both touch points |
| ⌘←, ⌘→ | Rotate |
| ⌘S | Take screenshot |
| ⌘R | Record video |
| ⌘K | Toggle software keyboard |

You can now debug the WebView in this simulator build using Safari's Web Inspector:

1. Make sure "Show Develop menu in menu bar" is enabled in Safari's Advanced preferences.
2. Under the Develop menu, expand the "Simulator" menu for the simulator you've opened.
3. Choose the WebView you want to inspect. A new window will open.

## Code Signing

Although the app is set up to use Automatic provisioning for Debug builds, you'll need to customize a few of the options. This is because the app makes heavy use of entitlements that require code signing, even for simulator builds.

Edit the file `Configuration/HomeAssistant.overrides.xcconfig` (which will not exist by default and is ignored by git) and add the following:

```bash
DEVELOPMENT_TEAM = YourTeamID
BUNDLE_ID_PREFIX = some.bundle.prefix
```

Xcode should generate provisioning profiles in your Team ID and our configuration will disable features your team doesn't have like Critical Alerts. You can find your Team ID on Apple's [developer portal](https://developer.apple.com/account); it looks something like `ABCDEFG123`.

## Code style

Linters run as part of Pull Request checks. Additionally, some linting requirements can be autocorrected.

```bash
# checks for linting problems, doesn't fix
bundle exec fastlane lint
# checks for linting problems and fixes them
bundle exec fastlane autocorrect
```

In the Xcode project, the autocorrectable linters will not modify your source code but will provide warnings. This project uses several linters:

- [SwiftFormat](https://github.com/nicklockwood/SwiftFormat)
- [SwiftLint](https://github.com/realm/swiftlint) (for things SwiftFormat doesn't automate)
- [Rubocop](https://rubocop.org) (largely for Fastlane)
- [YamlLint](https://yamllint.readthedocs.io/en/stable/index.html) (largely for GitHub Actions)

## Continuous Integration

We use [Github Actions](https://github.com/home-assistant/iOS/actions) alongside [Fastlane](https://fastlane.tools/) to perform continuous integration both by unit testing and deploying to [App Store Connect](https://appstoreconnect.apple.com). Mac Developer ID builds are available as an artifact on every build of master.

### Environment variables

Fastlane scripts read from the environment or `.env` file for configuration like team IDs. See [`.env.sample`](https://github.com/home-assistant/iOS/blob/master/.env.sample) for available values.

### Deployment

Although all the deployment is done through Github Actions, you can do it manually through [Fastlane](https://github.com/home-assistant/iOS/blob/master/fastlane/README.md):

### Deployment to App Store Connect

```bash
# creates the builds and uploads to the app store
# each save their artifacts to build/
bundle exec fastlane mac build
bundle exec fastlane ios build
```

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md)

## LICENSE

Apache-2.0

## Credits

The format and some content of this README.md comes from the [SwipeIt](https://github.com/ivanbruel/SwipeIt) project.

[![Home Assistant - A project from the Open Home Foundation](https://www.openhomefoundation.org/badges/home-assistant.png)](https://www.openhomefoundation.org/)
