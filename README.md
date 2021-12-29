# IOS-CodeSnippets
Here u can find some useful ios snipptes to save u some time :)
check [here](https://stackoverflow.com/questions/52417561/how-to-add-remove-custom-code-snippets-in-xcode-11-and-above) to see how to add a code snippet to your xcode

## Alert with Text Field
```
 let alert = UIAlertController(title: "Alert Title", message: "Alert Message",preferredStyle: .alert)
 
        let saveAction = UIAlertAction(title: "Save", style: .default) 
        {
            _ in let textField = alert.textFields![0]
            print("te user entered: \( textField.text!)")
        }
        
        let cancelAction = UIAlertAction(title: "Cancel", style: .default) {UIAlertAction -> Void in }
        alert.addTextField { UITextField -> Void in }
        alert.addAction(saveAction)
        alert.addAction(cancelAction)
        self.present(alert, animated: true, completion: nil) 
```

## Left-Right Gestures
#### vars
```
        let swipeRight = UISwipeGestureRecognizer(target: self, action: #selector(respondToSwipeGesture))
        swipeRight.direction = .right
        self.view.addGestureRecognizer(swipeRight)
        
        let swipeLeft = UISwipeGestureRecognizer(target: self, action: #selector(respondToSwipeGesture))
        swipeRight.direction = .left
        self.view.addGestureRecognizer(swipeLeft)
```
#### func
```
  @objc func respondToSwipeGesture(gesture: UIGestureRecognizer) {
        
        if let swipeGesture = gesture as? UISwipeGestureRecognizer {
            
            switch swipeGesture.direction {
            case .right:
                print("Swiped right")
                performSegue(withIdentifier: "swapLeftSegue", sender: self)
            case .left:
                print("Swiped left")
                performSegue(withIdentifier: "chartSegue", sender: self)
                
            default:
                break
            }
        }
    }
```

## Fitting Image To Container
#### 1) Set the Image size
```
    newImage =  newImage!.resizedImage(Size: CGSize(width: 100, height: 100))
```
#### 2) Add extension
```
extension UIImage
        {
            func resizedImage(Size sizeImage: CGSize) -> UIImage?
            {
                let frame = CGRect(origin: CGPoint.zero, size: CGSize(width: sizeImage.width, height: sizeImage.height))
                UIGraphicsBeginImageContextWithOptions(frame.size, false, 0)
                self.draw(in: frame)
                let resizedImage: UIImage? = UIGraphicsGetImageFromCurrentImageContext()
                UIGraphicsEndImageContext()
                self.withRenderingMode(.alwaysOriginal)
                return resizedImage
            }
        }

```
## MVC Structure
#### The Model
```
class Model{
    static func getAll(myURL : String ,completionHandler:@escaping (_ data: Data?, _ response: URLResponse?, _ error: Error?) -> Void) {
        // Specify the url that we will be sending the GET Request to
        let url = URL(string: myURL)
        // Create a URLSession to handle the request tasks
        let session = URLSession.shared
        // Create a "data task" which will request some data from a URL and then run the completion handler that we are passing into the getAllPeople function itself
        let task = session.dataTask(with: url!, completionHandler: completionHandler)
        // Actually "execute" the task. This is the line that actually makes the request that we set up above
        task.resume()
    }
}
```
#### Reading API Data
```
 func readURL(myURL : String){
        Model.getAll(myURL: myURL, completionHandler: {
            data, response, error in
            guard let myData = data else {return}
            do {
                let jsonDecoder = JSONDecoder()
                let apiResponse = try jsonDecoder.decode(YourApiStruct.self, from: myData)
                // do whatever you need with the API data in here
            } catch {
                print(error)
            }
        })
    }
```

# Credit 
thanks to the Marvlous [SAFA Falaqi](https://github.com/safafalaqi) for this amazing idea :)
