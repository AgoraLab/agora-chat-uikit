# Integrate UIKit 

Before using UIKit for chat rooms, you need to integrate it into your application.

## Prerequisites

Before you start, make sure your development environment meets the following requirements:

- Agora Chat SDK (included);
- Flutter 3.3.0 or above;
- For iOS: 
  - Xcode 13 or above; 
  - iOS 11 or above. 
- For Android: 
  - minSDKVersion 21;
  - When releasing, set the anti-obfuscation rules in `xxx/android/app/proguard-rules.pro`:

    ```java
    -keep class com.hyphenate.** {*;}
    -dontwarn  com.hyphenate.**
    ```

## Install UIKit into your project

Enter the project and execute the following command:

```
flutter pub get add chatroom_uikit
```

