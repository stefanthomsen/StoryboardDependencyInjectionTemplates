# StoryboardDependencyInjectionTemplates

This repository provides templates for [Sourcery](https://github.com/krzysztofzablocki/Sourcery). They allow you to code generate initializers for storyboard-based view controllers.

# StoryboardDependencyInjection.ejs

1. Add the following protocol to your project:
    ```swift
    protocol StoryboardInitializable where Self: UIViewController {
        static func instantiateFromStoryboard() -> Self
    }
    ```
2. Conform view controllers to it.
3. Use this template with [Sourcery](https://github.com/krzysztofzablocki/Sourcery).

### Example

For the following view controller:

```swift
final class ProfileViewController: UIViewController, StoryboardInitializable {
    var viewModel: ProfileViewModel!

    static func instantiateFromStoryboard() -> ProfileViewController {
        let storyboard = UIStoryboard(name: "Profile", bundle: Bundle.main)
        return storyboard.instantiateViewController(withIdentifier: "profileViewController") as! ProfileViewController
    }
}
```

Sourcery will generate `StoryboardDependencyInjection.generated.swift` with:

```swift
// MARK: - ProfileViewController - StoryboardDependencyInjection
extension ProfileViewController {

    static func makeFromStoryboard(
        viewModel: ProfileViewModel
    ) -> ProfileViewController {
        let viewController = StoryboardScene.Profile.instantiateProfileViewController()
        viewController.setDependencies(
            viewModel: viewModel
        )
        return viewController
    }

    func setDependencies(
        viewModel: ProfileViewModel
    ) {
        self.viewModel = viewModel
    }
}
```

Now, use `makeFromStoryboard(viewModel:)` to create instances of `ProfileViewController`.
When you add a new implicitly unwrapped optional property to that view controller, the definition of this method will automatically update, and the Swift compiler will let you know about all call sites needing update.

# StoryboardDependencyInjectionWithSwiftGen.ejs

1. Add the following empty protocol to your project:
    ```swift
    protocol StoryboardInitializable where Self: UIViewController { }
    ```
2. Conform view controllers to it.
3. Set up and run [SwiftGen](https://github.com/SwiftGen/SwiftGen).
4. Use this template with [Sourcery](https://github.com/krzysztofzablocki/Sourcery).

### Example

For the following view controller:

```swift
final class ProfileViewController: UIViewController, StoryboardInitializable {
    var viewModel: ProfileViewModel!
}
```

Sourcery will analyze your code and code generated by SwiftGen (`enum StoryboardScene`) and generate `StoryboardDependencyInjectionWithSwiftGen.generated.swift` with:

```swift
// MARK: - ProfileViewController - StoryboardDependencyInjection
extension ProfileViewController {

    static func makeFromStoryboard(
        viewModel: ProfileViewModel
    ) -> ProfileViewController {
        let viewController = StoryboardScene.Profile.instantiateProfileViewController()
        viewController.setDependencies(
            viewModel: viewModel
        )
        return viewController
    }

    func setDependencies(
        viewModel: ProfileViewModel
    ) {
        self.viewModel = viewModel
    }
}
```

Now, use `makeFromStoryboard(viewModel:)` to create instances of `ProfileViewController`.
When you add a new implicitly unwrapped optional property to that view controller, the definition of this method will automatically update, and the Swift compiler will let you know about all call sites needing update.
