# Integrate UIKit 

Before using UIKit for chat rooms, you need to integrate it into your application.

## Prerequisites

Before you start, make sure your development environment meets the following requirements:

- Android Studio Arctic Fox (2020.3.1) or above;
- Android API level 21 or above;
- Developed using Kotlin language, version 1.5.21 or above;
- JDK 1.8 or above;
- Gradle 7.0.0 or above.

## Add the ChatroomUIKit local module dependency

Find the downloaded **ChatroomUIKit** module and add it as a local dependency. Then import the `ChatroomService` module into the project.

```kotlin
// settings.gradle
include ':ChatroomUIKit'
include ':ChatroomService'
project(':ChatroomUIKit').projectDir = new File('../ChatroomUIKit/ChatroomUIKit')
project(':ChatroomService').projectDir = new File('../ChatroomUIKit/ChatroomService')

// app/build.gradle
dependencies {
  implementation(project(mapOf("path" to ":ChatroomUIKit")))
}
```

