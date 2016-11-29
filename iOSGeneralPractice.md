# iOS General Practices


## Convenient Methods

### App Delegate Methods

 - Making access to application delegate (AppDelegate) easy from any other class by writting new class method as below 

````
class func getAppDelegate() -> AppDelegate {
    return UIApplication.sharedApplication().delegate as! AppDelegate
}
````
