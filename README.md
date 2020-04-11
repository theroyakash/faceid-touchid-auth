# Local Authentication with FaceID or TouchID
Local Authentication with FaceID or TouchID on Enabled Device. Before you import any code, you need to add a new key to your Info.plist file, explaining to the user why you want access to Face ID. We pass the Touch ID request reason in code, and the Face ID request reason in Info.plist.

#### Key
Privacy - Face ID Usage Description. Add this key to your info.plist file.

#### Code
First import the following, and create an ```@State private var``` named ```isUnlocked``` and set the BOOL flag to false by default.
```swift
import LocalAuthentication
@State private var isUnlocked = false
```

Swift developers use the Error protocol for representing errors that occur at runtime, but Objective-C uses a special class called NSError. Because this is an Objective-C API we need to use NSError to handle problems, and pass it using & like a regular inout parameter. We’re going to write an authenticate() method that isolates all the biometric functionality in a single place. To make that happen requires four steps:


- Create instance of LAContext, which allows us to query biometric status and perform the authentication check.

- Ask that context whether it’s capable of performing biometric authentication – this is important because iPod touch has neither Touch ID nor Face ID.

- If biometrics are possible, then we start the actual request for authentication, passing in a closure to run when authentication completes.

- When the user has either been authenticated or not, our completion closure will be called and tell us whether it worked or not, and if not what the error was. This closure will get called away from the main thread, so we need to push any UI-related work back to the main thread.
