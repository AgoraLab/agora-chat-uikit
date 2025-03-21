# Integrate UIKit

Before using UIKit, you need to integrate it into your app. This page explains the necessary steps. 

## Prerequisites

Before you start, make sure your development environment meets the following conditions:

- Xcode 15.0 or above;
- iOS 13.0 or above;
- A project signed with a developer signature;
- A valid Agora project with users and tokens generated. See [Enable and configure Chat](https://docs.agora.io/en/agora-chat/get-started/enable) and [Secure authentication with tokens](https://docs.agora.io/en/agora-chat/develop/authentication) for details. 

## Integrate UIKit

You can use CocoaPods to install UIKit as a dependency in your Xcode project.

1. Add the following dependencies in `Podfile` :

   ```
   source 'https://github.com/CocoaPods/Specs.git'
   platform :ios, '13.0'
   
   target 'YourTarget' do
     use_frameworks!
     
     pod 'chat-uikit'
   end
   
   post_install do |installer|
     installer.pods_project.targets.each do |target|
       target.build_configurations.each do |config|
         config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '13.0'
         config.build_settings["EXCLUDED_ARCHS[sdk=iphonesimulator*]"] = "arm64"
       end
     end
   end
   ```

1. Run the following command to navigate to the folder where `Podfile` is located:

    ```
    pod install --repo-update
    ```
   
If you see the `Sandbox: rsync.samba(47334) deny(1) file-write-create...` error when compiling with Xcode 15, search for `ENABLE_USER_SCRIPT_SANDBOXING` in **Build Settings** and change the **User Script Sandboxing** setting to **NO**.