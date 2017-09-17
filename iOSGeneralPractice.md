# iOS General Practices


## Convenient Methods

### App Delegate Methods

 - Making access to application delegate (AppDelegate) easy from any other class by writting new class method as below 

````
class func getAppDelegate() -> AppDelegate {
	return UIApplication.shared.delegate as! AppDelegate
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

 - Use below convenient method to dismiss keyboard when tapped anywhere in a view apart from text field. Put the below extension code any where you want in a project. Recommended in a utility file.

````
// Put this piece of code anywhere you like
extension UIViewController {
    func hideKeyboardWhenTappedAround() {
        let tap: UITapGestureRecognizer = UITapGestureRecognizer(target: self, action: #selector(UIViewController.dismissKeyboard))
        view.addGestureRecognizer(tap)
    }

    func dismissKeyboard() {
        view.endEditing(true)
    }
}
````

Now in every UIViewController, All you have to do is call this function -

````
override func viewDidLoad() {
    super.viewDidLoad()
    self.hideKeyboardWhenTappedAround() 
}
````

### Placeholder for UITextView
 - UITextView doesn't come with a placeholder property, which is required in many cases. To address that there is a custom solution as per this Stack Overflow Ansewer [Placeholder in UITextView](https://stackoverflow.com/questions/1328638/placeholder-in-uitextview) which I find really helpful.

 - create these two UITextView delegate method in the view controller.

````
     func textViewDidBeginEditing(_ textView: UITextView) 
    {
        if (textView.text == "placeholder text here...")
        {
            textView.text = ""
            textView.textColor = .black
        }
        textView.becomeFirstResponder() //Optional
    }

    func textViewDidEndEditing(_ textView: UITextView)
    {
        if (textView.text == "")
        {
            textView.text = "placeholder text here..."
            textView.textColor = .lightGray
        }
        textView.resignFirstResponder()
    }
````

 - Remember to set UITextView with exact text on creation (may be in viewDidLoad method) e.g
````
 let myUITextView = UITextView.init()
 myUITextView.delegate = self
 myUITextView.text = "placeholder text here..."
 myUITextView.textColor = .lightGray
````

 - Then make the parent class a UITextViewDelegate e.g
````
class MyClass: UITextViewDelegate
{

}
````



