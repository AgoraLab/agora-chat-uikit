- [Introduction to ChatroomUIKit](#introduction-to-chatroomuikit)
    - [Development environment requirements](#development-environment-requirements)
    - [ChatroomUIKit installation](#chatroomuikit-installation)
    - [Sample project demonstration](#sample-project-demonstration)
        - [Project initialization](#project-initialization)
    - [Project structure](#project-structure)
    - [Quick start](#quick-start)
    - [Customization](#customization)
    - [Contributing](#contributing)
    - [License](#license)

# Introduction to ChatroomUIKit

ChatroomUIKit is designed to address most users' chat room requirements specific to pan-entertainment scenarios. It delivers good user experience in the use of APIs (for user-side developers) by streamlining the SDK integration, facilitating customization, and offering comprehensive documentation.

## Development environment requirements

- MacOS 12 or higher
- React-Native 0.66 or higher
- NodeJs 16.18 or higher

For iOS app:

- Xcode 13 or higher and its related dependency tool.

For the Android app:

- Android Studio 2021 or higher and its related dependency tool.

## ChatroomUIKit installation

```sh
npm install react-native-chat-room
# or
yarn add react-native-chat-room
# or
npx expo install react-native-chat-room
```

## Sample project demonstration

The sample project is located in the `example` folder. You can download the source code, compile and run the demo.

- Download the source code repository:

```sh
git clone https://github.com/agora/rncr/react-native-chat-room
```

- Download the source code archive:

```sh
curl -L -o file.zip  https://github.com/AsteriskZuo/react-native-chat-room/archive/refs/heads/main.zip
```

### Project initialization

1. Navigate to the root directory and run the `yarn & yarn env` command to complete project initialization.

2. Modify the necessary configurable items in the generated `example/src/env.ts` file.

- For the iOS app, run the `pod install` command to complete project initialization.
- For the Android app, run the `gradle sync` command to complete project initialization.

## Project structure

```sh
.
├── biz // UI components with business.
├── config // Global configuration service.
├── container // UIKit entry component.
├── dispatch // The event dispatch service which allows the closely related components to send and receive notifications to/from each other.
├── error // Error object that provides error codes.
├── hook // Function components, with Class components unavailable.
├── i18n // Internationalization service.
├── room // Chat room service that provides unified error processing for ease of use.
├── theme // Theme service that provides the light and dark themes. You can customize basic colors.
├── ui // Basic UI components that lay the foundation for other advanced components.
└── utils // Toolkit.
```

![svg](./docs/chatroom_architecture.svg)

## Quick start

[quick start portal](./docs/en/quick-start.md)

## Customization

[custom configure portal](./docs/en/custom-configure.md)

## Contributing

See the [contributing guide](CONTRIBUTING.md) to learn how to contribute to the repository and the development workflow.

## License

MIT