# Getting Started

In this tutorial, you are going to make an iOS app that will help users track their daily habits. You will not be using any third-party libraries to create this project, everything will be built in and provided by Apple already. One of the biggest differences you will see with this app is the way that the views are structured.

In many tutorials, you will be shown how to make all of your views inside of a storyboard file. You will not be using a storyboard in this project. Before you get freaked out by that, let me assure you that you will still be building your views as you did in a storyboard, you are just going to build the views in individual files rather than in one massive storyboard file. Learning how to structure apps this way will make you a better developer as it is common industry practice to move away from storyboards.

# First Steps

The first step is to create a new Xcode Project.

> [action]
> Open Xcode and create a new **Single View App** project.

![New Project](./assets/new_project.png)

> [action]
> Name your project `Habitual` and make sure that the *Language* is set to `Swift`.
> You will not be using *Core Data*, *Unit Tests*, or *UI Tests* so make sure all of those options
> are unchecked.

![Name Project](./assets/name_project.png)

# Removing Storyboards

Now that you have a new project set up, you need to make a few tweaks to your settings since you will not be using storyboards. Apple has a lot of great defaults set up when you create a new project. Without them setting many of these defaults, it would be very complicated to set up a new project. It is okay to deviate from these settings though, and that is exactly what you are going to do now.

> [action]
> In the File Navigator on the left-hand side, right-click on `Main.storyboard` and delete it, moving
> it to the trash. 
>
> Next, select the project (top of the file) and select the Build Settings Tab. Find "UIKit Main Storyboard File Base Name". Select this line and hit Delete. This should remove it. 

![Remove Main](./assets/remove_main.png)

> The last thing to do is open the Info.plist file and press Command + F to look for the word "main" Delete the Storyboard Name row by clicking on the minus sign.

![Remove Main](./assets/info-plist.png)

With those first steps out of the way, you're well on your way to getting this app up and running with no storyboards! Before anything will show up, you need to change the main view that will be loaded when your app launches. You will set up this change in the `SceneDelegate.swift` file.

> [action]
> Open `SceneDelegate.swift` and locate the `scene(_: willConnectTo)` method. Replace the code inside that function with the following:

```Swift 
// Create and set the window to be the same size as the screen
guard let windowScene = (scene as? UIWindowScene) else { return }
window = UIWindow(frame: UIScreen.main.bounds)
// Create an instance of the main view controller
let mainController = UIViewController()
mainController.view.backgroundColor = .green

// Tell the window to load the main controller as its root view
window?.rootViewController = mainController
window?.makeKeyAndVisible()
window?.windowScene = windowScene
```

The first thing you need to do is create an instance of the `UIWindow` object and make it the size of the `main` screen's bounds. The next step is to create an instance of the main view controller, or the first controller, that you want to load. In this case, you are just creating a temporary controller to see if you have hooked up the settings correctly. You set the background to green so that it is different from the launch screen. The last step is to set the `rootViewController` of the window to the controller that you initialized and then set the window to be visible using `makeKeyAndVisible()`.

Go ahead and run your project now and make sure that when it loads up you see the green screen. If you are seeing nothing but a black screen, make sure that you check your code and the settings one more time to make sure that they match. If you are seeing the green screen you are ready to move on to the next section!

# Summary

In this section, you learned how to create a new Xcode Project and remove the storyboard file! You also learned how to set up the settings in your project to use the view controllers that you created. Throughout the rest of the tutorial, you will be creating view controllers programmatically and loading the associated view files. You will need to get comfortable with this as you move forward. Head on over to the next section to learn how to set up your first view and load it up!
