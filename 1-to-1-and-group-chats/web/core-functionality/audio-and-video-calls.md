# Audio and video calls

UIKit integrates the Agora Video SDK, which enables audio and video calls in one-to-one and group chats. 

Configure the following parameters to use it:

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

Refer to the [Callkit integration document](https://www.npmjs.com/package/chat-callkit).