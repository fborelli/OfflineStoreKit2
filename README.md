# Offline StoreKit 2

![SwiftPM](https://img.shields.io/badge/SwiftPM-compatible-orange.svg)
[![](https://img.shields.io/endpoint?url=https%3A%2F%2Fswiftpackageindex.com%2Fapi%2Fpackages%2Ffborelli%2FOfflineStoreKit2%2Fbadge%3Ftype%3Dswift-versions)](https://swiftpackageindex.com/fborelli/OfflineStoreKit2)
[![](https://img.shields.io/endpoint?url=https%3A%2F%2Fswiftpackageindex.com%2Fapi%2Fpackages%2Ffborelli%2FOfflineStoreKit2%2Fbadge%3Ftype%3Dplatforms)](https://swiftpackageindex.com/fborelli/OfflineStoreKit2)

Offline StoreKit 2 is a StoreKit wrapper to recognize in-app purchases and subscriptions without internet (offline) or airplane mode. To do this, valid transactions are stored in UserDefaults using AES.GCM encryption.

## Motivation
[DataAppz](https://www.data-appz.com/)'s main product on the iOS AppStore is the [AeroChart](https://apps.apple.com/us/app/id1287592414?l=pt&ls=1&mt=8) app. This is an app for pilots and aviation enthusiasts. Focused on private and commercial aviation, it helps with navigation by allowing the visualization of aeronautical charts in an organized manner. In recent years, we have been using receipt validation on the device and on the server. StoreKit 2 has made it much easier to validate purchase receipts. However, if you are offline, purchases are only cached for a few minutes (5 to 15 minutes). Since some of our users need to use [AeroChart](https://apps.apple.com/us/app/id1287592414?l=pt&ls=1&mt=8) in airplane mode, our app ended up leaving many users disappointed. We thought of this solution as a way to allow receipt validation offline without compromising data reliability.

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

## Requirements
- iOS 15.6+
## Swift Package Manager
- https://github.com/fborelli/OfflineStoreKit2.git

## License
- OfflineStoreKit2 is a commercial product and uses a license key to verify your copy against the bundle ID you provide.
- Please [purchase your license here](https://fborelli.gumroad.com/l/rqnmja) to make the product work properly and support an independent software developer.
> [!IMPORTANT]
> The bundle ID must be entered at the time of license purchase, and cannot be changed later.

> [!NOTE]
> Please note that these IDs are case-sensitive, so make sure you register the correct case variant.

> [!TIP]
> To find the app bundle ID in Xcode navigate to your project/target settings. Under the “General” tab, locate the “Bundle Identifier” field, which contains your app’s bundle ID.

## Sequence diagram
```mermaid
sequenceDiagram
    participant YourApp
    participant AppStore
    participant OfflineStoreKit2
    YourApp->>AppStore: requestProducts()
    AppStore-->>YourApp: [StoreKit.Product]
    YourApp->>OfflineStoreKit2: storeOfflineProducts
    YourApp->>AppStore: Transaction.currentEntitlements
    AppStore-->>YourApp: [StoreKit.Transaction]
    YourApp->>OfflineStoreKit2: storeOfflineTransactions
    Note over YourApp,OfflineStoreKit2: Offline (or Airplane Mode)
    OfflineStoreKit2->>YourApp: didNetworkStatusChanged()
    YourApp->>OfflineStoreKit2: updateCustomerProductStatusOffline()
    OfflineStoreKit2-->>YourApp: [offlinePurchasedNonConsumables]
    OfflineStoreKit2-->>YourApp: [offlinePurchasedSubscriptions]
    OfflineStoreKit2-->>YourApp: [offlinePurchasedNonRenewableSubscriptions]
```


## Getting Started
1. For the next steps, for educational purposes, we will illustrate using Apple's StoreKit 2 Sample Code. Download the example code at: [Implementing a store in your app using the StoreKit API](https://developer.apple.com/documentation/storekit/implementing-a-store-in-your-app-using-the-storekit-api). Please use the analogy to replicate the code modifications to your project.
2. Use the Swift Package Manager to add the OfflineStoreKit2 framework to your Xcode project. Open your project in Xcode and select project and then the Package Dependencies tab. Copy the URL below into the search field. Set the Dependency Rule to Up to next major and click "Add Package". Apple has a great [article](https://developer.apple.com/documentation/xcode/adding-package-dependencies-to-your-app) on how to add SwiftPM to your project.
```
  https://github.com/fborelli/OfflineStoreKit2.git
```
3. Remove ${SAMPLE_CODE_DISAMBIGUATOR} from the Bundle ID. The Bundle ID should now look like this: "com.example.apple-samplecode.implementing-a-store-in-your-app-using-the-storekit-api". This is required to validate the license.
4. All subsequent modifications will be in the Store.swift class. First include the import:
```Swift
    import OfflineStoreKit2Framework
```
5. Include the following properties. The first three are used to receive purchased products that were previously stored. The last one is the call to the OfflineTransactionsManager class passing the respective license to the BundleID: "com.example.apple-samplecode.implementing-a-store-in-your-app-using-the-storekit-api". Please [purchase your license here](https://fborelli.gumroad.com/l/rqnmja).
```Swift
    @Published private(set) var offlinePurchasedNonConsumables: [OfflineProduct] = []
    @Published private(set) var offlinePurchasedNonRenewableSubscriptions: [OfflineProduct] = []
    @Published private(set) var offlinePurchasedSubscriptions: [OfflineProduct] = []
    @Published private(set) var offlineTransactionsManager = OfflineTransactionsManager(license: "eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzM4NCJ9.eyJzdWIiOiJjb20uZXhhbXBsZS5hcHBsZS1zYW1wbGVjb2RlLmltcGxlbWVudGluZy1hLXN0b3JlLWluLXlvdXItYXBwLXVzaW5nLXRoZS1zdG9yZWtpdC1hcGkifQ.XVWVAT04h4AP5VnmVRx1L-aq6tvJj8ewq_BWTCI4UPoI3DDPLP3jl72KIOl_PS0d6HfkHqybZmzl0f45J0IhJJtuOru4JFWxxCkYdJlsWDCNYZMfTlDMOVGC8A148MxJ")
```
6. Make the class implement the protocol OfflineTransactionsManagerDelegate:
```Swift
    class Store: ObservableObject, OfflineTransactionsManagerDelegate {
```
7. Implement the delegate method
```Swift
    func didNetworkStatusChanged() async {
        Task {
            print("Network status changed - isConnected: \(offlineTransactionsManager.isOnline)")
            await requestProducts()
            await updateCustomerProductStatus()
        }
    }
```

8. Make modifications bellow on updateCustomerProductStatus func:
```Swift
    @MainActor
    func updateCustomerProductStatus() async {
        if(offlineTransactionsManager.isOnline) {
            ...
            /// Keep here the previous code of updateCustomerProductStatus
            ...
            /// Store offline transactions
            await offlineTransactionsManager.storeOfflineTransactions(currentEntitlements: Transaction.currentEntitlements)
        } else {
            await offlineTransactionsManager.updateCustomerProductStatusOffline(purchasedNonConsumables: &offlinePurchasedNonConsumables, purchasedSubscriptions: &offlinePurchasedSubscriptions, purchasedNonRenewableSubscriptions: &offlinePurchasedNonRenewableSubscriptions)
        }
    }
```
9. Make modifications on requestProducts add the line bellow after products request:
```Swift
    let storeProducts = try await Product.products(for: productIdToEmoji.keys)
    await offlineTransactionsManager.storeOfflineProducts(storeProducts: storeProducts)
```

