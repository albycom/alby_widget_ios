[![Languages](https://img.shields.io/badge/languages-OjbC%20%7C%20%20Swift-orange.svg?maxAge=2592000)](https://github.com/albycom/alby_widget_ios)
[![Apache License](http://img.shields.io/badge/license-APACHE2-blue.svg?style=flat)](https://www.apache.org/licenses/LICENSE-2.0.html)
![GitHub Tag](https://img.shields.io/github/v/tag/albycom/alby_widget_ios)


## Installation

AlbyWidget for iOS supports iOS 15+. 
Xcode 15 is required to build Alby iOS SDK.

### Swift Package Manager
Add `https://github.com/albycom/alby_widget_ios` as a Swift Package Repository in Xcode and follow the instructions to add `AlbyWidget` as a Swift Package.


## Setup and Configuration
This SDK only works with SwiftUI.

## Prerequisites  
1. Brand ID - This is an organization identifier that represents your brand
2. Widget ID - This is a unique identifier for the alby widget that you can get in the widgets embed page inside the alby UI.

## Initialization
The SDK must be initialized with the unique identifier for your alby account (Brand ID).

```swift
// AppDelegate

import AlbyWidget

class AppDelegate: UIResponder, UIApplicationDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        AlbySDK().initialize(brandId: "your-brand-id")
        return true
    }
}
```

The SDK should be initialized before any other alby SDK methods are called.


## Components

### addAlbyWidget
The `addAlbyWidget` function displays the Alby widget inside a sheet (modal). This is ideal for cases where you want the widget to appear in an overlay or pop-up format, giving users the option to engage with the widget without leaving the current screen.

Go to the SwiftUI View where you want to place the widget and after everything just add
```swift
.addAlbyWidget(productId: "your product id", brandId: "your-brand-id", widgetId: "your-widget-id")
```

The default placement will be in the bottom of the screen. If you have a bottom bar or something similar, make sure you add a bottom
offset. In the example below we are moving the alby bottom sheet 50 points upwards.

```swift
.addAlbyWidget(productId: "123", brandId: "456", widgetId: "789", bottomOffset: 50)
```

#### Possible issues
Depending on how your view is structured the keyboard inside the bottom sheet might not work as expected.
Make sure that you place the widget inside a ScrollView so the keyboard can scroll and the content be displayed.

#### Example Usage
```swift
struct HomeView: View {    
    var body: some View {
        ScrollView {
            .padding()
        }
        .addAlbyWidget(productId: "your-product-id", brandId: "your-brand-id", widgetId: "your-widget-id", bottomOffset: 1)
            .background(Color(UIColor.white))        
    }
}
```

You can also pass in A/B test parameters to the widget by passing in the `testId`, `testVersion` and `testDescription` parameters:
```swift
.addAlbyWidget(productId: "your-product-id", brandId: "your-brand-id", widgetId: "your-widget-id", bottomOffset: 1, testId: "your-test-id", testVersion: "your-test-version", testDescription: "your-test-description")
```


### AlbyInlineWidgetView
The `AlbyInlineWidgetView` is a component that allows embedding the Alby widget directly into your app's UI. It’s perfect for inline use on any page, like product details or brand-specific screens, where the widget integrates seamlessly within the existing view hierarchy.

In the SwiftUI View where you want to place the widget, add the AlbyInlineWidgetView component and pass in the required brandId, productId and widgetId parameters:
```
AlbyInlineWidgetView(
    brandId: "your-brand-id",
    productId: "your-product-id",
    widgetId: "your-widget-id"
)
```

#### Example Usage
```swift
AlbyInlineWidgetView(
    brandId: "your-brand-id",
    productId: "your-product-id",
    widgetId: "your-widget-id"
)
```

Optional: You can pass in A/B test parameters to the widget by passing in the `testId`, `testVersion` and `testDescription` parameters:
```swift
AlbyInlineWidgetView(
    brandId: "your-brand-id",
    productId: "your-product-id",
    widgetId: "your-widget-id",
    testId: "your-test-id",
    testVersion: "your-test-version",
    testDescription: "your-test-description"
)
```

## Event Tracking
The SDK also provides an API to sending purchase data and other events via HTTP requests.

### Usage
1. Use the sendPurchasePixel method to send a purchase pixel request:
```swift
AlbySDK().sendPurchasePixel(
    orderId: 12345, // Order ID (String or Number)
    orderTotal: 99.99, // Order total (Float or Number)
    productIds: ["A123", 456], // List of product IDs (String or Number)
    currency: "USD" // Currency code
)
```
2. Use the sendAddToCartEvent method to send an add to cart event:
```swift
AlbySDK().sendAddToCartEvent(
    price: 99.99, // Price of the item
    variantId: "A123", // Variant ID of the item
    currency: "USD", // Currency of the item
    quantity: "1" // Quantity of the item
)
```