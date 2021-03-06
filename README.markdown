Updated by Leon on 2013-12-27
-----------------------------

* update UI to IOS7 style
* add range select 

![ScrrenShot](https://raw.github.com/phaibin/Kal/master/ScreenShot.png)

-----------------------------------------
Kal - a calendar component for the iPhone
-----------------------------------------
![](http://farm9.staticflickr.com/8308/7898361456_debb9e2695.jpg)

This project aims to provide an open-source implementation of the month view in Apple's mobile calendar app (MobileCal). When the user taps a day on the calendar, any associated data for that day will be displayed in a table view directly below the calendar. As a client of the Kal component, you have 2 responsibilities:

1. Tell Kal which days need to be marked with a dot because they have associated data.
2. Provide UITableViewCells which display the details (if any) for the currently selected day.

In order to use Kal in your application, you will need to provide an implementation of the KalDataSource protocol to satisfy these responsibilities. Please see KalDataSource.h and the included demo app for more details.

Release Notes
-------------

**June 21, 2012**

Today I added VoiceOver/Accessibility support. Special thanks to Matt Gemmell's [excellent article](http://mattgemmell.com/2010/12/19/accessibility-for-iphone-and-ipad-apps/) on adding accessibility support to your iPhone app. I wish I would have done this a long time ago.

If your app is localized, then you will also want to localize the 4 new accessibility strings that I added in this release: "Previous month", "Next month", "Marked" and "Today".

**July 9, 2010**

This is the iOS 4.0 / iPhone4 release. New features include:

1) A refactored project file. Kal is now built as a static library in a separate Xcode project. Regardless of whether you are a new or existing user of Kal, please read the section entitled "Integrating Kal into Your Project" below.

2) The project now specifies iOS 4.0 as the Base SDK. So if you want to upgrade to this release of Kal, you must upgrade your SDK.

3) Added hi-res graphics for Retina Display support. Extra special thanks to Paul Calnan for sending me the hi-res graphics.

4) Added a new example app, "NativeCal," which demonstrates how to integrate Kal with the EventKit framework that Apple made available in iOS 4.

**NOTE** I'm not crazy about the KalDataSource asynchronous/synchronous API. I will probably be changing it in the future and updating the example apps to use GCD and blocks.

**March 11, 2010**

A lot of people have emailed me asking for support for selecting and displaying an arbitrary date on the calendar. So today I pushed some commits that make this easy to do. You can specify which date should be initially selected and shown when the calendar is first created by using -[KalViewController initWithSelectedDate:]. If you would like to programmatically switch the calendar to display the month for an arbitrary date and select that date, use -[KalViewController showAndSelectDate:].

**January 1, 2010**

I have made significant changes to the KalDataSource API so that the client can respond to the data request asynchronously. The Kal demo app, "Holidays," now includes 2 example datasources:

1. HolidayJSONDataSource - retrieves data asynchronously from http://keith.lazuka.org/holidays.json
2. HolidaySqliteDataSource - queries an Sqlite database inside the application bundle and responds synchronously (because the query is fast enough that it doesn't affect UI responsiveness too badly).

**December 19, 2009**

Initial public release on GitHub.

Example Usage
-------------

Note: All of the following example code assumes that it is being called from
within another UIViewController which is in a UINavigationController hierarchy.

How to display a very basic calendar (without any events):

    KalViewController *calendar = [[[KalViewController alloc] init] autorelease];
    [self.navigationController pushViewController:calendar animated:YES];

In most cases you will have some custom data that you want to attach
to the dates on the calendar. The first thing you must do is provide
an implementation of the KalDataSource protocol. Then all you need to do
to display your annotated calendar is instantiate the KalViewController
and tell it to use your KalDataSource implementation (in this case, "MyKalDataSource"):

    id<KalDataSource> source = [[MyKalDataSource alloc] init];
    KalViewController *calendar = [[[KalViewController alloc] initWithDataSource:source] autorelease];
    [self.navigationController pushViewController:calendar animated:YES];

NOTE: KalViewController does not retain its datasource. You probably will want to store a reference to the dataSource in an instance variable so that you can release it after the calendar has been destroyed.

Integrating Kal into Your Project
---------------------------------

* Just drag "Kal/src/" and drop it into your project.
* Add "Localizable.strings" file and add 
```ruby
"CalendarTitle"     = "LLLL yyyy";
```
* #import "Kal.h"

Additional Notes
----------------

The Xcode project includes two demo apps:
1) "Holiday" demonstrates how to use Kal to display several 2009 and 2010 world holidays using both JSON and Sqlite datasources.
2) "NativeCal" demonstrates how to use Kal with the EventKit framework.

Kal is fully localized. The month name and days of the week will automatically
use the appropriate language and style for the iPhone's current regional settings.


