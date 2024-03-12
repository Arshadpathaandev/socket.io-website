---
title: "Tutorial step #6 - Connection state recovery"
sidebar_label: "Step #6: Connection state recovery"
slug: step-6
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Connection state recovery
adasudhasudqhwuihwqd  dasdahsjdasjdhsjadhjasd asdas dasdhuewqdeuwdeuw dasdjsadjsadsa dsajoidjewqpdjewd
First, let's handle disconnections by pretending that there was no disconnection: this feature is called "Connection state recovery". 

This feature will **temporarily** store all the events that are sent by the server and will try to restore the state of a client when it reconnects:

- restore its rooms
- send any missed events

It must be enabled on the server side:

```js title="index.js"
const io = new Server(server, {
  // highlight-start
  connectionStateRecovery: {}
  // highlight-end
});
```

Let's see it in action:

<video controls width="100%"><source src="/videos/tutorial/connection-state-recovery.mp4" /></video>

As you can see in the video above, the "realtime" message is eventually delivered when the connection is reestablished.

:::note

The "Disconnect" button was added for demonstration purposes.

<details className="changelog">
    <summary>Code</summary>

<Tabs groupId="syntax">
  <TabItem value="es6" label="ES6" default>

```html
<form id="form" action="">
  <input id="input" autocomplete="off" /><button>Send</button>
  // highlight-start
  <button id="toggle-btn">Disconnect</button>
  // highlight-end
</form>

<script>
  // highlight-start
  const toggleButton = document.getElementById('toggle-btn');

  toggleButton.addEventListener('click', (e) => {
    e.preventDefault();
    if (socket.connected) {
      toggleButton.innerText = 'Connect';
      socket.disconnect();
    } else {
      toggleButton.innerText = 'Disconnect';
      socket.connect();
    }
  });
  // highlight-end
</script>
```

  </TabItem>
  <TabItem value="es5" label="ES5">

```html
<form id="form" action="">
  <input id="input" autocomplete="off" /><button>Send</button>
  // highlight-start
  <button id="toggle-btn">Disconnect</button>
  // highlight-end
</form>

<script>
  // highlight-start
  var toggleButton = document.getElementById('toggle-btn');

  toggleButton.addEventListener('click', function(e) {
    e.preventDefault();
    if (socket.connected) {
      toggleButton.innerText = 'Connect';
      socket.disconnect();
    } else {
      toggleButton.innerText = 'Disconnect';
      socket.connect();
    }
  });
  // highlight-end
</script>
```

  </TabItem>
</Tabs>
</details>

:::

Great! Now, you may ask:

> But this is an awesome feature, why isn't this enabled by default?

There are several reasons for this:

- it doesn't always work, for example if the server abruptly crashes or gets restarted, then the client state might not be saved 
- it is not always possible to enable this feature when scaling up

:::tip

That being said, it is indeed a great feature since you don't have to synchronize the state of the client after a temporary disconnection (for example, when the user switches from WiFi to 4G).

:::

We will explore a more general solution in the next step.

:::info

<Tabs groupId="lang">
  <TabItem value="cjs" label="CommonJS" default attributes={{ className: 'display-none' }}>

You can run this example directly in your browser on:

- [CodeSandbox](https://codesandbox.io/p/sandbox/github/socketio/chat-example/tree/cjs/step6?file=index.js)
- [StackBlitz](https://stackblitz.com/github/socketio/chat-example/tree/cjs/step6?file=index.js)


  </TabItem>
  <TabItem value="mjs" label="ES modules" attributes={{ className: 'display-none' }}>

You can run this example directly in your browser on:

- [CodeSandbox](https://codesandbox.io/p/sandbox/github/socketio/chat-example/tree/esm/step6?file=index.js)
- [StackBlitz](https://stackblitz.com/github/socketio/chat-example/tree/esm/step6?file=index.js)


  </TabItem>
</Tabs>

:::
