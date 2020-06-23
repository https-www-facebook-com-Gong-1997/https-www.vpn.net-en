# https-xoso.com.vn-xo-so-mien-nam
Code XSMN
 "Force FGS notifications to show for a minimum time
 It's possible for a service to do a start/stop foreground and cause a couple of things to happen: NotificationManagerService will enqueue a EnqueueNotificationRunnable,post a PostNotificationRunnable (for the startForeground), and then also enqueue a CancelNotificationRunnable. There is some racy behavior here in that the cancel runnable can get triggered in between enqueue and post runnables. If the cancel happens first, then NotificationListenerServices will never get the message.
 This behavior is technically allowed, however for foreground services we want to ensure that there is a minmum amount of time that notification listeners are aware of the foreground service so that (for instance) the FGS notification can be shown.
 This CL does two things to mitigate this problem
 Introduce checking in the CancelNotificationRunnable such that it will not cancel until after PostNotificationRunnable has finished executing.
 Introduce a NotificationLifetimeExtender method that will allow a lifetime extender to manage the lifetime of a notification that has been enqueued but not inflated yet.
 Bug: 20 | 05 | 89 
 Test: atest NotificationManagerServiceTest
 Test: atest ForegroundServiceNotificationListenerTest 
 Change-Id: I0680034ed9315aa2c05282524d48faaed066ebd0 
 Merged-In: I0680034ed9315aa2c05282524d48faaed066ebd0
