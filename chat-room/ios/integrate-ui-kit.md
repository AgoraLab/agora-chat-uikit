# Integrate UIKit 

Before using UIKit for chat rooms, you need to integrate it into your application.

## Prerequisites

Before you start, make sure your development environment meets the following requirements:

- Xcode 14.0 or above;
- iOS 13.0 or above;
- A project with a valid developer signature;

## Integrate ChatroomUIKit using CocoaPods

You can add `ChatroomUIKit` as a dependency to your Xcode project using CocoaPods.

1. Add the following dependencies to `Podfile`:

    ```
    source 'https://github.com/CocoaPods/Specs.git'
    platform :ios, '13.0'
    
    target 'YourTarget' do
      use_frameworks!
      
      pod 'ChatroomUIKit'
    end
    
    post_install do |installer|
      installer.pods_project.targets.each do |target|
        target.build_configurations.each do |config|
          config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '13.0'
        end
      end
    end
    ```

1. Execute the following command in the directory of `Podfile`:

   ```
   pod install --repo-update
   ```

    If the following error appears when compiling with Xcode 15: `Sandbox: rsync.samba(47334) deny(1) file-write-create...` , you can search for `ENABLE_USER_SCRIPT_SANDBOXING` in **Build Settings** and change the **User Script Sandboxing** setting to **NO**.