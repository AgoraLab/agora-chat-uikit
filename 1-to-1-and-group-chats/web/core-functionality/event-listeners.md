 UIKit provides `eventHandler` to register and monitor events. It can monitor all API calls inside UIKit. The event name is the name of the corresponding API provided by the SDK.
 
## Usage examples

```javascript
import React, { useEffect } from "react";
import { eventHandler } from "ease-chat-uikit";

const ChatApp = () => {
  useEffect(() => {
    eventHandler.addEventHandler("handlerId", {
      onError: (err) => {
        // The error events of all events will be called back in onError
        console.error(err);
      },
      recallMessage: {
        success: () => {
          toast.success("Withdrawal successful");
        },
        error: (error) => {
          toast.error("Undo failed");
        },
      },
      reportMessage: {
        success: () => {
          toast.success("Report successful");
        },
        error: (error) => {
          toast.error("Report failed");
        },
      },
      sendMessage: {
        error: (error) => {
          if (error.type == 507) {
            toast.error("You have been banned and cannot send messages");
          } else if (
            error.type == 602 &&
            error.message == "not in group or chatroom"
          ) {
            toast.error("Message sending failed, you are no longer in the current group");
          }
        },
      },
    });
  }, []);
  return <div> </div>;
};
```