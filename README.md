# OfflineStoreKit2

![SwiftPM](https://img.shields.io/badge/SwiftPM-compatible-orange.svg)
[![](https://img.shields.io/endpoint?url=https%3A%2F%2Fswiftpackageindex.com%2Fapi%2Fpackages%2Ffborelli%2FOfflineStoreKit2%2Fbadge%3Ftype%3Dswift-versions)](https://swiftpackageindex.com/fborelli/OfflineStoreKit2)
[![](https://img.shields.io/endpoint?url=https%3A%2F%2Fswiftpackageindex.com%2Fapi%2Fpackages%2Ffborelli%2FOfflineStoreKit2%2Fbadge%3Ftype%3Dplatforms)](https://swiftpackageindex.com/fborelli/OfflineStoreKit2)

OfflineStoreKit2 is a StoreKit wrapper to recognize in-app purchases and subscriptions without internet (offline) or airplane mode. To do this, valid transactions are stored in UserDefaults using AES.GCM encryption.

## Motivation
[DataAppz](https://www.data-appz.com/)'s main product on the iOS AppStore is the [AeroChart](https://apps.apple.com/us/app/id1287592414?l=pt&ls=1&mt=8) app. This is an app for pilots and aviation enthusiasts. Focused on private and commercial aviation, it helps with navigation by allowing the visualization of aeronautical charts in an organized manner. In recent years, we have been using receipt validation on the device and on the server. StoreKit 2 has made it much easier to validate purchase receipts. However, if you are offline, purchases are only cached for a few minutes (5 to 15 minutes). Since some of our users need to use [AeroChart](https://apps.apple.com/us/app/id1287592414?l=pt&ls=1&mt=8) in airplane mode, our app ended up leaving many users disappointed. We thought of this solution as a way to allow receipt validation without internet without compromising data reliability.

## Features

### Definition
- Persists in-app purchases and subscriptions for offline use (or airplane mode)
- Stores non-consumable, auto-renewable, and non-renewable subscriptions
- Covers StoreKit Transactions and Products

### User Experience
- Allows paying users to have offline access.
- Avoids customer friction

### Security
- Stored in UserDefaults with AES.GCM encryption
- Only works with verified transactions
- Communicates with the App Store when back online
- Prevents cross-device tampering attacks

### Ease
- Lightweight, easy-to-implement solution
- Lifetime purchase
- No subscriptions

## Getting Started
- 

## Requirements
- iOS 15.6+

## License
- This is a commercial product. Please purchase your license for the product to work properly.
