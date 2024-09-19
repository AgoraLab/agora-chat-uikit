If `userId` `and` token are set during initialization, UIKit will automatically log in when the Provider is loaded and automatically log out when the Provider is unloaded.

```javascript
import { UIKitProvider } from "easemob-chat-uikit";

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

To log in and out manually, you can obtain the Chat SDK connection instance and then call the SDK API to log in and out.

```javascript
import { useClient } from "easemob-chat-uikit";

const ChatApp = () => {
  const client = useClient();
  const login = () => {
    client.open({
      user: "userId",
      token: "chat token",
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