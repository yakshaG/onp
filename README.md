# Open notification protocol

💪 **Let's free email and communication channels from notifications.**

---
<br />
<br />
<br />

# Why a notification protocol?

😠 **Abuse of email and other communication channels for notifications.**
Email is meant for communication, not to deliver notifications. Email overload is worsened by notifications piggybacking email.

😞 **Lack of interoperable notification delivery mechanism.**
There is no foolproof way for services to deliver notifications to a user, when the user is off their app/website/platform. Communication channels like email/SMS are abused as fallback.

🤩 **Make notifications useful again.**
Potential to synchronise `on platform` and `off platform` notifications, reducing notification overload. Both users and notification publishers benefit from using a better channel optimised for notifications.

#### Examples of notifications overloading work email

| Service Category                       | Services                           |
| -------------------------------------- | ---------------------------------- |
| Workplace Collaboration Tools          | Microsoft Teams, Slack, Zoom       |
| Project Management Tools               | Asana, Trello, Jira                |
| Customer Relationship Management (CRM) | Salesforce, HubSpot                |
| Email Marketing Platforms              | Mailchimp, Constant Contact        |
| Issue Tracking and Helpdesk Systems    | Zendesk, Freshdesk                 |
| Human Resources Software               | BambooHR, Workday                  |
| Calendars and Scheduling Tools         | Google Calendar, Microsoft Outlook |

#### Examples of notifications overloading personal email

| Service Category               | Services                             |
| ------------------------------ | ------------------------------------ |
| Social Media Networks          | Facebook, Twitter, LinkedIn, YouTube |
| E-commerce and Shopping        | Amazon, eBay, Etsy                   |
| Online Banking                 | Banks, Financial Institutions        |
| Travel and Booking Services    | Airlines, Hotels, Travel Agencies    |
| News and Content Subscriptions | News Websites, Blogs                 |
| Fitness and Health Apps        | Fitbit, MyFitnessPal                 |
| Gaming Platforms               | Steam, Xbox Live                     |

---
<br />
<br />
<br />

# Notification v/s communication

Notification is also a form of communication. So how should one decide when to treat something as either communication or a notification?

> **If something is treated as notification on your app/website/platform, treat it as a notification off your platform as well. Don't offload it on to a communication channel.**

---
<br />
<br />
<br />

# A real life notification delivery example

---
<br />
<br />
<br />

# A real life user on boarding up example

1. Anjali signs up on Medium
2. During on boarding/sign up, Medium asks Anjali to choose a notification delivery channel. The options could be
	1. Piggyback my email
	2. Google notifications(assuming Anjali's email provider is Google and Google has implemented ONP)
		1. This option cannot be shown if Google has not implemented ONP
	3. Enter `notification address`

#### 1.1 Anjali selects Google notifications
1. Since the email provider, Google, has implemented ONP, the `notification address` is set equal to the email address.
2. A test notification(could be packaged as a welcome notification) is sent to the notification address, with an OTP by Medium. If the notification address works well, then Anjali should see this on her Google notifications app/web app etc.
	1. Why this test notification and OTP?
		1. To educate Anjali about the new concept of dedicated notification channel
		2. To ensure that the email provider has indeed implemented ONP and notifications can be positively delivered
3. If the notification was successfully received, then Anjali sees the OTP from the notification and enters it on the Medium on boarding flow.
4. Medium on boarding flow continues. Medium sends `off platform` notifications to Anjali via Google notifications, instead of email. These `off platform` notifications will work in tandem with Medium `on platform` notifications(in app, website) to prevent duplicate notifications for Anjali.

#### 1.2 Anjali selects `Enter notification address`
1. Anjali has to input her `notification address`.
2. If she has already registered with a notification service provider (notification receiving part of ONP), then she should have her `notiifcation address` set up. She can input that.
3. A test notification and OTP flow like above has to be completed.
4. If not, then she has to create a `notification address`,  by registering with a notification service provider of her choice and complete the process.

---
<br />
<br />
<br />

# Architecture [WIP]

- User identification with `noiification address`
- If email provider has implemented ONP, set `notification address` = `email address` for best user experience.

## Notification publishers

To create, update or delete notifications, notification publishers call the receiver's domain on port `23456`

| Field             | Description                                                                                                                                                                                                                         | Default value assumed   | Cached? |
| ----------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- | ------- |
| Notification UUID | Unique identifier for the notification                                                                                                                                                                                              | -                       | No      |
| Sender            | notification address of the sender                                                                                                                                                                                                  | -                       | -       |
| Recipient         | notification address of the receiver                                                                                                                                                                                                | -                       |         |
| Read              | To signify the read/unread status of the notification                                                                                                                                                                               | unread                  | -       |
| Update type       | `create` when the notification is created for the first time, <br /> `update` when the notification has to be updated                                                                                                               | -                       | -       |
| Title             | Notification title                                                                                                                                                                                                                  | -                       | -       |
| Message           | Notification message                                                                                                                                                                                                                | -                       | -       |
| Small icon URI    | URI or filename of image for small icon (status bar), monochrome                                                                                                                                                                    | cahed value, if present | Yes     |
| Large icon URI    | URI or filename of image for large icon, may not show if a picture is used, default is none                                                                                                                                         | cahed value, if present | Yes     |
| Accent color      | Accent color for the notification, system may adjust, default is system default                                                                                                                                                     | system default          | Yes     |
| Picture URI       | URI or filename of image for "big picture style" notification, may hide large icon, default is none                                                                                                                                 | none                    | Yes     |
| Person URI        | Contact, mailto:, tel: or name: URI of person relevant to this notification.                                                                                                                                                        | -                       | No      |
| Confidential      | Whether to only show the notification on secure screens                                                                                                                                                                             | no                      | -       |
| Category          | Notification category                                                                                                                                                                                                               | -                       | -       |
| Created at        | Unix timestamp when the notification was created                                                                                                                                                                                    | -                       | -       |
| Updated at        | Unix timestamp when the notification was last updated                                                                                                                                                                               | -                       | -       |
| Importance        | To determine the priority with respect to other notifications user receives                                                                                                                                                         | Low                     | -       |
| Action buttons    | Array of action buttons. Each button is `primary`, `secondary` or `tertiary`. Buttons must have default URI and optionally platform specific URIs. A click on any action button should trigger a `read` feedback to the subscriber. | Low                     | -       |


## Notification receiver
Notification receivers can provide feedback to the publisher about a notification, based on user actions, by calling the publisher's domain on port  `23457`
| Field             | Description                                           |
| ----------------- | ----------------------------------------------------- |
| Notification UUID | Unique identifier for the notification                |
| Read              | To signify the read/unread status of the notification |
---

