# IOS-CodeSnippets
Here u can find some useful ios snipptes to save u some time :)
check [here](https://www.youtube.com/watch?v=ixlLgEgPnRk) to see how to add a code snippet to your xcode

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
## Core Data 
after setting up ur core data these snipptes will help in managing it

#### vars
```
//array to hold all created objects of the CoreData Entity
var itemsList = [NameOfCoreDataEntity]()

//needed objects
let context = (UIApplication.shared.delegate as! AppDelegate).persistentContainer.viewContext
let save = (UIApplication.shared.delegate as! AppDelegate).saveContext

```
#### func
#### 1) fetching data
```
func fetchingData() {
        let result: NSFetchRequest<NameOfCoreDataEntity> = NameOfCoreDataEntity.fetchRequest()
        do {
            itemsList = try context.fetch(result)
        }catch {
            print(error)
        }
        // you can reload the data of ur table/collectionView in here
    }

```
#### 2) Add new object to the CoreData
*Note: you don't need to use the alert dialog, but it would be an easier choice for data entry *
```
        let alert = UIAlertController(title: "Add New object", message: "please fill in the fields", preferredStyle: .alert)
        alert.addTextField { textField in
            textField.placeholder = "Enter your Name"
        }
        alert.addAction(UIAlertAction(title: "Save", style: .default, handler: { _ in
            let name = alert.textFields![0].text!
            if name.isEmpty {
                return
            }
            let newObject = NameOfCoreDataEntity(context: self.context)
            newObject.name = name
            self.save()
            self.fetchingData()
        }))
        alert.addAction(UIAlertAction(title: "Cancel", style: .cancel, handler: .none))
        present(alert, animated: true)
    }

```
#### 3) Delete an object from the CoreData
```
        context.delete(itemsList[the index of the desired item])
        save()
        fetchingData()

```
## Image Picker
#### vars
```
// variable
var imageView = UIImageView()

//Picker calling variable
let picker = UIImagePickerController()
        picker.delegate = self
        picker.sourceType = .photoLibrary
        present(picker, animated: true)

```
#### extension
```
extension yourViewControllerName : UIImagePickerControllerDelegate, UINavigationControllerDelegate {
    func imagePickerController(_ picker: UIImagePickerController, didFinishPickingMediaWithInfo info: [UIImagePickerController.InfoKey : Any]) {
    
        guard let userPickedImage = info[.originalImage] as? UIImage else { return }
        imageView.image = userPickedImage
        picker.dismiss(animated: true, completion: nil)
        
    }
}

```
## Delete from API
```
static func deleteTaskWithObjective(id: String, completionHandler: @escaping(_ data: Data?, _ response: URLResponse?, _ error: Error?) -> Void) {

        // Create the url to request
        if let urlToReq = URL(string: "http://127.0.0.1:8080/tasks/\(id)") {
        
            // Create an NSMutableURLRequest using the url. This Mutable Request will allow us to modify the headers.
            var request = URLRequest(url: urlToReq)
            request.httpMethod = "DELETE"// Set the method to DELETE
           
            let session = URLSession.shared // Create the session
            let task = session.dataTask(with: request as URLRequest, completionHandler: completionHandler)
            task.resume()
        }
    }

```
## Date Format
```
let calendar = Calendar.current
let today = calendar.startOfDay(for: Date())
//pass the string format 
print(today.toString(format: "EEEE, MMM d, yyyy"))

extension Date {

    func toString(format: String) -> String {
        let formatter = DateFormatter()
        formatter.dateStyle = .short
        formatter.dateFormat = format
        return formatter.string(from: self)
    }
    
}
```


# Credit 
Thanks to the Marvlous [SAFA Falaqi](https://github.com/safafalaqi) for this brilliant idea :)
And huge thanks to [Mohammed Jaha](https://github.com/MiroJaha) for the amazing code snipptes !
