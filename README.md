# ImageDownloadManager

<p align="center">
<a href="https://actions-badge.atrox.dev/gustavo-garcia-leite/ImageDownloadManager/goto" target="_blank"><img src="https://github.com/gustavo-garcia-leite/ImageDownloadManager/workflows/ImageDownloadManager/badge.svg?branch=main" alt="Build Status" /></a>
<img src="https://img.shields.io/badge/platforms-iOS%20%7C%20macOS%20%7C%20tvOS%20%7C%20watchOS%20%7C%20Linux-333333.svg" alt="Supported Platforms: iOS, macOS, tvOS, watchOS & Linux" />
<br />
<a href="https://cocoapods.org/pods/ImageDownloadManager" alt="ImageDownloadManager on CocoaPods" title="ImageDownloadManager on CocoaPods"`><img src="https://img.shields.io/cocoapods/v/ImageDownloadManager.svg" /></a>
<a href="https://github.com/Carthage/Carthage" alt="ImageDownloadManager on Carthage" title="ImageDownloadManager on Carthage"><img src="https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat" /></a>
<a href="https://github.com/apple/swift-package-manager" alt="ImageDownloadManager on Swift Package Manager" title="ImageDownloadManager on Swift Package Manager"><img src="https://img.shields.io/badge/Swift%20Package%20Manager-compatible-brightgreen.svg" /></a>
</p>

`ImageDownloadManager` allows you to download multiple images and store them using `NSCache`. And it uses the Observer Pattern to notify classes when the download of each image is complete, whether successful or not.

## Installation

`ImageDownloadManager` doesn't contain any external dependencies.

These are currently the supported installation options:

### Swift Package Manager

You can easily install the SDK using Swift Package Manager. Just add the following to your `Package.swift`:

```swift
dependencies: [
    .package(url: "https://github.com/gustavo-garcia-leite/ImageDownloadManager.git", from: "1.0")
]
```

### CocoaPods

You can also use CocoaPods to install the SDK by adding the following to your `Podfile`:

```ruby
pod 'ImageDownloadManager', '~> 1.0'
```

Then run:
```ruby
$ pod install
```
### Carthage

To use Carthage, add the following to your `Cartfile`:

```ruby
github "gustavo-garcia-leite/ImageDownloadManager" ~> 1.0
```
Then run:
```
$ carthage update
```

## Usage

### Downloading images

To download an image just write the code bellow.

```swift
import ImageDownloadManager

ImageDownloadManager.shared.downloadImage(imageURL: "https://www.test.com.br/images/image.png")
```
### Getting a image from cache

```swift
import ImageDownloadManager

let result = ImageDownloadManager.shared.getDownloadedImage(imageURL: "https://www.test.com.br/images/image.png")

if let image = result.image {
    // Image with download completed
    imageView.image = image
} else if !result.containError {
    // Image with download in progress, so let's wait until the manager returns a response.
    ImageDownloadManager.shared.addObserver(observer: self)
} else {
    // the image had error during download
}
```

If you would like to receive notifications when an image finishes downloading, you will need to extend your class in this way. 
```swift
extension MyView: ImageDownloadObserver {

    func didCompleteDownload(of imageURL: String, image: UIImage?) {
        
        guard imageURL == myImageURL else {
            return
        }
        
        if let image {
            DispatchQueue.main.async { [weak self] in
                self?.imageView.image = image
            }
        }
    }
    
}
```

## Example
If you would like to see a complete example of the usage, check the `Example.swift` file.

## License
ImageDownloadManager is released under the MIT license. See LICENSE for details.
