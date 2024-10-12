# Log in to UIKit

If `userId` and `token` are set during initialization, UIKit will automatically log in when the Provider is loaded and automatically log out when the Provider is unloaded.

The sample code is as follows:

```javascript
import { UIKitProvider } from "agora-chat-uikit";

const App = () => {
  return (
    <UIKitProvider
      initConfig={{
        appKey: "",
        userId: "",
        token: "",
      }}
    ></UIKitProvider>
  );
};
```

To log in and out manually, you can obtain the Chat SDK connection instance and then call the SDK API:

```javascript
import { useClient } from "agora-chat-uikit";

const ChatApp = () => {
  const client = useClient();
  const login = () => {
    client.open({
      user: "userId",
      accessToken: "chat token",
    });
  };
  const logout = () => {
    client.close();
  };
  return (
    <div>
      <button onClick={login}>Login</button>
      <button onClick={logout}>Logout</button>
    </div>
  );
};
```