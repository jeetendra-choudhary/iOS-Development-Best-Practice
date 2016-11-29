# iOS General Practices


## Convenient Methods

### App Delegate Methods

 - Making access to application delegate (AppDelegate) easy from any other class by writting new class method as below 

````
class func getAppDelegate() -> AppDelegate {
    return UIApplication.sharedApplication().delegate as! AppDelegate
}
````

With it, we can access any property and method of the app delegate in a much simpler way. For example, we can get the contacts store property from any class in the project as shown next:

````
AppDelegate.getAppDelegate().contactStore
````
