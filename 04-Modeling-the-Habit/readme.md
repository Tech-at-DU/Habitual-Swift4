# Creating the Habit Model

You've populated a table view with a list of names. To change this to a list of habits you will need to store more information for each row than just a single string. 

To do this you will use a struct. A struct is a collection type. 

Primitive data types are strings and numbers. These represent a single value such as 42 or "William". 

Collection types are structs and arrays. A struct stores a "collection" of values. For example, the names array has a list of many strings. A struct has values assigned to keys like: `{ name: "William", shoesize: 42 }`

# Habits struct

> [action]
> Create a new **Swift** file, choose File > New > File... and name it **Habit**. 

> Add the following code to `Habit.swift`:

```swift
import Foundation

struct Habit {
    var title: String
}
```

Note! It is a convention to store a single class or struct in a file with the same as the class or struct defined in that file. 

Note! It is a convention to name classes and structs beginning with an uppercase letter. 

Here you created a `struct` to hold our structured habit and added a property for the title.

Use the `Habit` struct in your tableview. First, create a new array full of habits. 

> [action]
> _Replace_ the **names** array with the following:

```swift
class HabitsTableViewController: UITableViewController {
    var habits: [Habit] = [
    Habit(title: "Go to bed before 10pm"),
    Habit(title: "Drink 8 Glasses of Water"),
    Habit(title: "Commit Today"),
    Habit(title: "Stand up every Hour")
    ]
    ...
}
```

Update the two table view methods to populate the table view using the array of habits.

> [solution]
> Replace `name` with `habits`. Notice that the title displayed in the cell gets its value from `habit.title` this is the "key" where the `Habit` struct stores a string. 

```swift
class HabitsTableViewController: UITableViewController {
        override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return habits.count
    }

    override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        var cell: UITableViewCell
        if let dequeueCell = tableView.dequeueReusableCell(withIdentifier: "cell") {
            cell = dequeueCell
        } else {
            cell = UITableViewCell(style: .default, reuseIdentifier: "cell")
        }
        let habit = habits[indexPath.row]
        cell.textLabel?.text = habit.title
        return cell
    }
}
```

> Find the `pressAddHabit(sender:)` method. This still refers to the array `names` you need to update this to `habits`. It also adds a string but the `habits` array needs a `Habit`. Add a new `Habit` to the `Habits` array:

```Swift
@objc func pressAddHabit(_ sender: UIBarButtonItem) {
    habits.insert(Habit(title: "New Habit"), at: 0) // add a new habit
    let topIndexPath = IndexPath(row: 0, section: 0)
    tableView.insertRows(at: [topIndexPath], with: .automatic)
}
```

Test your work. You should see the list of habits and read the title of each habit. Tapping the button in the upper right should add a "New Habit" to the list!

# Adding more Properties to Habit

To make the app use it would well store more data with each habit. Since you used a struct, which is a collection type, you can easily expand the number of properties. 

Consider the following properties:

- `dateCreated`
- `selectedImage`
- `currentStreak`
- `bestStreak`
- `lastCompletionDate`
- `numberOfCompletions`

What type should each of these be? 

- `dateCreated` type `Date` Foundation provides a special Date type used to store a date and all of the features of a date (its a collection)
- `selectedImage` type `UIImage` UIKit provides a special image type
- `currentStreak` type `Int` is good here for counting the number of times you've used this habit
- `bestStreak` type `Int` highest number out of the streaks
- `lastCompletionDate` type `Date`
- `numberOfCompletions` type `Int` count the number of times a habit was completed. 

Update the `Habit` struct with the new properties. Notice that these are variables that are stored in the struct. Each time you make a new instance of this struct that instance stores all of these variables with their values. 

> [solution] 
> Update the `Habit` struct to add the new properties. 

```swift
import UIKit

struct Habit {
    var title: String
    let dateCreated: Date
    var selectedImage: UIImage
    var currentStreak: Int
    var bestStreak: Int
    var lastCompletionDate: Date?
    var numberOfCompletions: Int
}
```

Note! Some of these "properties" are declared with `var` and can be updated, while others are declared with `let` and are constants that can not be changed. 

`dateCreated` can't be changed. Makes sense because if you create a habit on Monday, you can't say you created it in the future!

`numberOfCompletions` on the other hand will be updated each time you complete a habit. 

Note! `lastCompletionDate` is type `Date?` this is an optional. This means it might be a `Date` or it might be `nil`. Think about it like this. When you decide to brush your teeth as a habit, you haven't brushed them yet, so the `lastCompletionDate` doesn't exist. Later that day when you brush your teeth, you update your habit tracker and the `lastCompletionDate` is created. 

What if you decided that climbing mount Everest was a good habit? It might go uncompleted for a long time, you might never complete it! 

We use optionals in Swift for situations where a variable might have a value but might also not have a value. 

## Dates

The `Date` class is built in and has a lot of methods and features. A date keeps track of a moment in time, that includes the year, the month, the date, the day of the week, and the time of day including the hour, minutes, seconds, and more! 

In this step, you'll add an extension to the Date class with some helper methods to make it easier to get formatted dates that will be used in this app. 


> [Action]
> Create a new Swift file: File > New > File... Name the new file: **DateExtensions.swift**

> Add the following code in **DateExtensions.swift**:

```swift
extension Date {
    var stringValue: String {
        return DateFormatter.localizedString(from: self, dateStyle: .medium, timeStyle: .none)
    }

    var isToday: Bool {
        let calendar = Calendar.current
        return calendar.isDateInToday(self)
    }

    var isYesterday: Bool {
        let calendar = Calendar.current
        return calendar.isDateInYesterday(self)
    }
}
```

There are three methods here and they are written differently from other methods you have written. These are called computed properties. A computed property acts like a function internally but looks like a property/value externally. Read more about them here: 

https://docs.swift.org/swift-book/documentation/the-swift-programming-language/properties/#Computed-Properties

The methods above are: 

- `stringvalue` - This returns a human-readable date string from the `Date` instance
- `isToday` - Returns `true` if the `Date` instance is today otherwise `false`
- `isYesterday` - Returns `true` if the `Date` instance is yesterday

Add a computed property to your `Habit` struct. 

> [action]
> In the **Habit.swift**, add the `completedToday` property:

```swift
import UIKit

struct Habit {
    ...
    var completedToday: Bool {
        return lastCompletionDate?.isToday ?? false
    }
}
```

This "computed" property returns `true` if the `lastCompletionDate` date is today. 

# Loading Images from the Project

The icons we'll have for each habit will be in-app icons.
Meaning, we'll provide a fixed number of icons.

The icons come in several resolutions, x1, x2, and x3, for different screen sizes. You'll have to tell Xcode which icon is which resolution. 

> [action]
> Download [these icons](./assets/Icons.zip) and unzip them if they are not unzipped. 

> Add the x1 icons to **Assets.xcassets**. Then add the x2 version of the icon and the x3 version. Like this: 

![adding icons](./assets/adding-icons.gif)

This gif shows how to add the first three icons, you should add them all! Notice that x1 icons first, then added the x2 for each, and x3 for each after. 

Next, we need some code to make it easy to track these icons. Swift gives different data structures for different purposes. An array is a list of things that can hold as many or as few things as needed. Use it for things like a shopping cart. It might be empty at the start and have items added and removed as you go. 

An enum is a data type that represents a fixed number of items that are always the same. These are things like days of the week, which are always 7 and always have the same names.

Since we have a fixed number of icons an enum makes a good choice. 

> In **Habit.swift**, add the following enum:

```swift
struct Habit {
    enum Images: Int, CaseIterable {
        case book
        case bulb
        case clock
        case code
        case drop
        case food
        case grow
        case gym
        case heart
        case lotus
        case other
        case pet
        case pill
        case search
        case sleep
        case tooth

        var image: UIImage {
            guard let image = UIImage(named: String(describing: self)) else {
                fatalError("image \(self) not found")
            }

            return image
        }
    }
    ...
}
```

This is an `enum` that represents each image you added to **Assets.xcassets**. Notice that the names in the list match the names of the images in Xcassets. 

`image` is a computed property that creates a `UIImage` from the name. If it can't find the name it throws a fatal error. 

`CaseIterable` helps us by giving us a class property that returns all the cases in the enum. `Habits.Images.allCases` will give us an array of all cases.

The last step is to add a custom initializer and assign some default values to our `Habit` struct.

Add the following properties to the Habit struct:

1. Give default values to the following properties:
 - dateCreated
 - currentStreak
 - bestStreak
 - numberOfCompletions
2. Write an initializer that takes in a `String` for the **title** and an `Habit.Images` for the **selectedImage**
3. Update the **selectedImage: UIImage** to use the `Habits.Images` enum.


> [solution]
> Edit `Habit.swift`. Look at the code below find a similar code and make these changes. If the code is not shown leave it as is. 

```swift
struct Habit {
    ...
    var title: String
    let dateCreated: Date = Date()
    var selectedImage: Habit.Images

    var currentStreak: Int = 0
    var bestStreak: Int = 0
    var lastCompletionDate: Date?
    var numberOfCompletions: Int = 0

    var completedToday: Bool {
        return lastCompletionDate?.isToday ?? false
    }

    // Add this initializer
    init(title: String, image: Habit.Images) {
        self.title = title
        self.selectedImage = image
    }
}
```

With these changes, it will change the way that the `Habit` struct is created! To create a Habit you will include the title and the image for that habit. The image will be an icon/category to represent that habit. 

Before you run the project, update the `HabitsTableViewController`'s habits array to say:

```swift
class HabitsTableViewController: UITableViewController {
    var habits: [Habit] = [
        Habit(title: "Go to bed before 10pm", image: .book),
        Habit(title: "Drink 8 Glasses of Water", image: .drop),
        Habit(title: "Commit Today", image: .code),
        Habit(title: "Stand up every Hour", image: .gym)
    ]
    ...
}
```

Note! `.book` or `.drop` is a shortcut for `Habit.Images.book` or `Habit.Images.drop`. When you type the dot Xcode will code hint you to one of the images in the enum. 

For now, nothing has changed on our table view. But, in the next section, you will build a custom cell to display more information about the habit.

- [Populating the TableView](./03-Populating-the-Table-View/)
- [Building a Custom Cell](./05-Building-a-Custom-Cell/)
