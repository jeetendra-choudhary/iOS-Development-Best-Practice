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
 
 - The second convenient method that we’ll add in the AppDelegate.swift file is going to display an alert controller with a message that we’ll provide as a parameter each time we use it. The implementation isn’t difficult, however there’s something special here; an alert controller must be presented by a view controller, and the app delegate is not a view controller. To counterattack this, it’s necessary first to find the top most view controller that is currently presented in the app window, and then present the alert controller in that view controller. Here we go:

````
func showMessage(message: String) {
    let alertController = UIAlertController(title: "Birthdays", message: message, preferredStyle: UIAlertControllerStyle.Alert)
 
    let dismissAction = UIAlertAction(title: "OK", style: UIAlertActionStyle.Default) { (action) -> Void in
    }
 
    alertController.addAction(dismissAction)
 
    let pushedViewControllers = (self.window?.rootViewController as! UINavigationController).viewControllers
    let presentedViewController = pushedViewControllers[pushedViewControllers.count - 1]
 
    presentedViewController.presentViewController(alertController, animated: true, completion: nil)
}
````
