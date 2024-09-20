UIKit integrates the Agora Video SDK, which enables audio and video calls in single chats or group conversations. 

## Usage examples

You need to configure the following parameters. Refer to the [Callkit integration document](https://www.npmjs.com/package/chat-callkit).

```javascript
import { Chat } from "easemob-chat-uikit";

const ChatApp = () => {
  return (
    <Chat
      rtcConfig={{
        onInvite: handleInviteToCall,
        agoraUid: agoraUid,
        onAddPerson: handleAddPerson,
        getIdMap: handleGetIdMap,
        onStateChange: handleRtcStateChange,
        appId: appId,
        getRTCToken: getRtcToken,
      }}
    />
  );
};
```