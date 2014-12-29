<properties linkid="develop-mobile-tutorials-get-started-with-push-js" urlDisplayName="Get Started with Push (JS)" pageTitle="Get started with push notifications (JavaScript)" metaKeywords="" description="Learn how to use push notifications in your Windows Store app with Azure Mobile Services." metaCanonical="http://www.windowsazure.com/en-us/develop/mobile/tutorials/get-started-with-push-dotnet/" disqusComments="1" umbracoNaviHide="1" title="Get started with push notifications in Mobile Services using Visual Studio 2012" documentationCenter="Mobile" authors="glenga" />

<tags ms.service="mobile-services" ms.workload="mobile" ms.tgt_pltfrm="mobile-windows-store" ms.devlang="javascript" ms.topic="article" ms.date="01/01/1900" ms.author="glenga"></tags>

# Get started with push notifications in Mobile Services using Visual Studio 2012

<div class="dev-center-tutorial-selector sublanding"> 
    <a href="/en-us/develop/mobile/tutorials/get-started-with-push-dotnet-vs2012" title="Windows Store C#">Windows Store C#</a><a href="/en-us/develop/mobile/tutorials/get-started-with-push-js-vs2012" title="Windows Store JavaScript" class="current">Windows Store JavaScript</a><a href="/en-us/develop/mobile/tutorials/get-started-with-push-wp8" title="Windows Phone">Windows Phone</a><a href="/en-us/develop/mobile/tutorials/get-started-with-push-ios" title="iOS">iOS</a><a href="/en-us/develop/mobile/tutorials/get-started-with-push-android" title="Android">Android</a><a href="/en-us/develop/mobile/tutorials/get-started-with-push-xamarin-ios" title="Xamarin.iOS">Xamarin.iOS</a><a href="/en-us/develop/mobile/tutorials/get-started-with-push-xamarin-android" title="Xamarin.Android">Xamarin.Android</a> 
</div>

This topic shows you how to use Azure Mobile Services to send push notifications to a Windows Store app.
In this tutorial you add push notifications using the Windows Push Notification service (WNS) to the quickstart project. When complete, your mobile service will send a push notification each time a record is inserted.

<div class="dev-callout"><b>Note</b>
    <p>This tutorial adds push notifications to a Windows Store app created in Visual Studio 2012. Visual Studio 2013 includes new features that make it easy to setup push notifications in your Windows Store app using Mobile Services. For the Visual Studio 2013 version, see <a href="/en-us/develop/mobile/tutorials/get-started-with-push-js">Get started with push notifications</a>.</p>
</div>

This tutorial walks you through these basic steps to enable push notifications:

1.  [Register your app for push notifications and configure Mobile Services][]
2.  [Create the Registrations table][]
3.  [Add push notifications to the app][]
4.  [Update scripts to send push notifications][]
5.  [Insert data to receive notifications][]

This tutorial requires the following:

-   Microsoft Visual Studio 2012 Express for Windows 8
-   Active Windows Store account

This tutorial is based on the [Get started with data][] tutorial. Before you start this tutorial, you must first complete [this tutorial][Get started with data].

## <a name="register"></a><span class="short-header">Register your app</span>Register your app for the Windows Store

To be able to send push notifications to Windows Store apps from Mobile Services, you must submit your app to the Windows Store. You must then configure your mobile service to integrate with WNS.

[WACOM.INCLUDE [mobile-services-register-windows-store-app][]]

Both your mobile service and your app are now configured to work with WNS. Next, you will create a new table to store registrations.

## <a name="create-table"></a>Create a new table

[WACOM.INCLUDE [mobile-services-create-new-push-table][]]

## <a name="add-push"></a><span class="short-header">Add push notifications</span>Add push notifications to your app

1.  Open the file default.js and insert the following code fragment into the **app.OnActivated** method overload, just after the **args.setPromise** method:

        // Get the channel for the application.
        var channel;
        var channelOperation = Windows.Networking.PushNotifications
            .PushNotificationChannelManager
            .createPushNotificationChannelForApplicationAsync()
            .then(function (newChannel) {
                channel = newChannel;
            });

    This code acquires and stores a push notification channel each time the application is launched.

2.  Insert the following code after the code that creates the **MobileServiceClient** instance:

        // Insert the new channel URI into the Registrations table.
        var registrationsTable = client.getTable('Registrations');
        registrationsTable.insert({ handle: channel.uri });         

    This code inserts the current channel into the Registrations table.

3.  Open the Package.appxmanifest file and make sure that in the **Application UI** tab, **Toast capable** is set to **Yes**.

    ![][]

    This makes sure that your app can raise toast notifications.

## <a name="update-scripts"></a><span class="short-header">Update the insert script</span>Update the registered insert script in the Management Portal

[WACOM.INCLUDE [mobile-services-update-registrations-script][]]

1.  Click **TodoItem**, click **Script** and select **Insert**.

    ![][1]

2.  Replace the insert function with the following code, and then click **Save**:

        function insert(item, user, request) {
            request.execute({
                success: function() {
                    request.respond();
                    sendNotifications();
                }
            });

            function sendNotifications() {
                var registrationsTable = tables.getTable('Registrations');
                registrationsTable.read({
                    success: function(registrations) {
                        registrations.forEach(function(registration) {
                            push.wns.sendToastText04(registration.handle, {
                                text1: item.text
                            }, {
                                success: function(pushResponse) {
                                    console.log("Sent push:", pushResponse);
                                }
                            });
                        });
                    }
                });
            }
        }

    This insert script sends a push notification (with the text of the inserted item) to all channels stored in the **Registrations** table.

## <a name="test"></a><span class="short-header">Test the app</span>Test push notifications in your app

1.  In Visual Studio, press the F5 key to run the app.

2.  In the app, type text in **Insert a TodoItem**, and then click **Save**.

    ![][2]

    Note that after the insert completes, the app receives a push notification from WNS.

    ![][3]

## <a name="next-steps"> </a>Next steps

This tutorial demonstrates the basic push notification functionality provided by Mobile Services. If your app requires more advanced functionalities, such as sending cross-platform notifications, subscription-based routing, or very large volumes, consider using Azure Notification Hubs with your mobile service. For more information, see one of the following Notification Hubs topics:

-   [Get started with Notification Hubs][]
    Learn how to leverage Notification Hubs in your Windows Store app.

-   [Send notifications to subscribers][]
    Learn how users can register and receive push notifications for categories they're interested in.

-   [Send notifications to users][]
    Learn how to send push notifications from a Mobile Service to specific users on any device.

-   [Send cross-platform notifications to users][]
    Learn how to use templates to send push notifications from a Mobile Service, without having to craft platform-specific payloads in your back-end.

Consider finding out more about the following Mobile Services topics:

-   [Get started with data][]
    Learn more about storing and querying data using Mobile Services.

-   [Get started with authentication][]
    Learn how to authenticate users of your app with Windows Account.

-   [Mobile Services server script reference][]
    Learn more about registering and using server scripts.

-   [Mobile Services HTML/JavaScript How-to Conceptual Reference][]
    Learn more about how to use Mobile Services with HTML and JavaScript.

<!-- Anchors. --> <!-- Images. --> <!-- URLs. -->

  [Windows Store C\#]: /en-us/develop/mobile/tutorials/get-started-with-push-dotnet-vs2012 "Windows Store C#"
  [Windows Store JavaScript]: /en-us/develop/mobile/tutorials/get-started-with-push-js-vs2012 "Windows Store JavaScript"
  [Windows Phone]: /en-us/develop/mobile/tutorials/get-started-with-push-wp8 "Windows Phone"
  [iOS]: /en-us/develop/mobile/tutorials/get-started-with-push-ios "iOS"
  [Android]: /en-us/develop/mobile/tutorials/get-started-with-push-android "Android"
  [Xamarin.iOS]: /en-us/develop/mobile/tutorials/get-started-with-push-xamarin-ios "Xamarin.iOS"
  [Xamarin.Android]: /en-us/develop/mobile/tutorials/get-started-with-push-xamarin-android "Xamarin.Android"
  [Get started with push notifications]: /en-us/develop/mobile/tutorials/get-started-with-push-js
  [Register your app for push notifications and configure Mobile Services]: #register
  [Create the Registrations table]: #create-table
  [Add push notifications to the app]: #add-push
  [Update scripts to send push notifications]: #update-scripts
  [Insert data to receive notifications]: #test
  [Get started with data]: /en-us/develop/mobile/tutorials/get-started-with-data-js
  [mobile-services-register-windows-store-app]: ../includes/mobile-services-register-windows-store-app.md
  [mobile-services-create-new-push-table]: ../includes/mobile-services-create-new-push-table.md
  []: ./media/mobile-services-windows-store-javascript-get-started-push-vs2012/mobile-app-enable-toast-win8.png
  [mobile-services-update-registrations-script]: ../includes/mobile-services-update-registrations-script.md
  [1]: ./media/mobile-services-windows-store-javascript-get-started-push-vs2012/mobile-insert-script-push2.png
  [2]: ./media/mobile-services-windows-store-javascript-get-started-push-vs2012/mobile-quickstart-push1.png
  [3]: ./media/mobile-services-windows-store-javascript-get-started-push-vs2012/mobile-quickstart-push2.png
  [Get started with Notification Hubs]: /en-us/manage/services/notification-hubs/getting-started-windows-dotnet/
  [Send notifications to subscribers]: /en-us/manage/services/notification-hubs/breaking-news-dotnet/
  [Send notifications to users]: /en-us/manage/services/notification-hubs/notify-users/
  [Send cross-platform notifications to users]: /en-us/manage/services/notification-hubs/notify-users-xplat-mobile-services/
  [Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-js
  [Mobile Services server script reference]: http://go.microsoft.com/fwlink/?LinkId=262293
  [Mobile Services HTML/JavaScript How-to Conceptual Reference]: /en-us/develop/mobile/how-to-guides/work-with-html-js-client/
