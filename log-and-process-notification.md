# Log and Process Notification

* They help users understand what's happening during long-running operations instead of wondering if something has broken
* Users get real-time feedback showing exactly what's happening behind the scenes.&#x20;
* They can see progress bars, status messages, and detailed logs as the operation runs.
* On the server -&#x20;
  * `context.report_progress()` - Update progress with current and total values
* On the client side, you need to set up callback functions to handle these notifications.&#x20;
* The server emits these messages, but it's up to your client application to decide how to present them to users
* Notifications depends on your application type:
  * **CLI applications** - Simply print messages and progress to the terminal
  * **Web applications** - Use WebSockets, server-sent events, or polling to push updates to the browser
  * **Desktop applications** - Update progress bars and status displays in your UI
