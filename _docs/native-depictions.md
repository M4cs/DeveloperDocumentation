---
layout: doc
title: Native Depictions
permalink: /native-depictions
---
# Native Depictions
**NOTE:** This page is under heavy development and is subject to change.

Sileo uses a revolutionary new format for depictions, which can give your users amazing new experiences whilst browsing your repository. Learn how simple it is to support this new format, with examples.

---

## Getting started

Sileo native depictions are written using JSON. To add support for a Sileo depiction to a package, simply add a `SileoDepiction` key to your repo's Packages file. Here's a (shortend) example package entry for ShortLook.

```
Package: co.dynastic.ios.tweak.shortlook
Name: ShortLook
...
Depiction: https://repo.dynastic.co/depiction/68292503631036416/
SileoDepiction: https://repo.dynastic.co/sileo-depiction/68292503631036416/
```

**Note:** The `SileoDepiction` key will always take prescedent over the `Depiction` key when using Sileo.

<br>    

## Structure

Sileo depictions are composed of tabs, which contain an array of views. Views can be formed from any of the classes listed in the below section.

Each of the below objects *must* have a class.

### Root object

<!-- TODO: Get AB to style tables properly. -->

Class: `DepictionTabView`

| Key           | Type                                  | Description                                                                             |
|---------------|---------------------------------------|-----------------------------------------------------------------------------------------
| `minVersion`  | String                                | Minimum app version required for Sileo to display the depiction. Should be 0.1 for now.
| `headerImage` | String (URL)                          | A URL to the image that should be displayed in the header of the package page.
| `tintColor`   | String (Color)                        | A CSS-compatible color code. Most views also support this property.
| `tabs`        | [Array of Tab objects](#tab-object) | An array of tabs that the depiction should display.                                     |

<br>

### Tab object

Class: `DepictionStackView`

Tabs are separate screens that are made up of views. They can be used to separate between more important data (such as a package's name) and other supplemental information (such as a changelog).

| Key       | Type                                  | Description                    | Required?  
|-----------|---------------------------------------|--------------------------------|------------|
| `tabname` | String                                | The name of the tab.           | Yes        
| `views`   | [Array of View objects](#view-object) | The views (layout) of the tab. | Yes        
| `orientation` | String (`landscape`/`portrait`)   | Whether the view is portrait or landscape. | No
| `xPadding` | Double                               | Change the horizontal padding. | No 

<br>

### View object

A tab is made up of multiple views, allowing repos to customize how their information is displayed on screen. There are multiple different kinds of views, each dictated by a different class.

Each view will define the `class` key, with the remaining keys required being determined by what class is used.

#### Headers

Class: `DepictionHeaderView`

Creates a large title intended for separating major sections of a given tab.

| Key       | Type                                  | Description                    | Required?
|-----------|---------------------------------------|--------------------------------|----------------|
| `title` | String | The title of the header.           | Yes
| `useMargins` | Boolean | Allow margins above/below the header. | No
| `useBottomMargin` | Boolean | Adds a margin below the header (if margins are enabled). | No
| `useBoldText` | Boolean | Make the text bold. | No
| `alignment`   | AlignEnum | Change the alignment to the left (`0`), center (`1`), or the right (`2`). | No

#### Subheaders

Class: `DepictionSubheaderView`

Subheaders are smaller headers.

| Key       | Type                                  | Description                    | Required?
|-----------|---------------------------------------|--------------------------------|----------------|
| `title` | String | The title of the header. | Yes
| `useMargins` | Boolean | Allow margins above/below the header. | No
| `useBottomMargin` | Boolean | Adds a margin below the header (if margins are enabled). | No
| `useBoldText` | Boolean | Make the text bold. | No

#### Markdown Text

Class: `DepictionMarkdownView`

Allows for basic Markdown or HTML to be displayed, ideal for large blocks of text.

| Key       | Type                                  | Description                    | Required?
|-----------|---------------------------------------|--------------------------------|----------------|
| `markdown` | String (Markdown) | The text to be rendered as Markdown (or HTML)           | Yes
| `useSpacing` | Boolean | Enable or disable the use of vertical spacing.           | No
| `useMargins` | Boolean | Enable or disable the use of horizontal margins.           | No
| `useRawFormat` | Boolean | If true, `markdown` will accept basic HTML instead of Markdown. | No

#### Videos

Class: `DepictionVideoView`

Renders a video.

| Key       | Type                                  | Description                    | Required?
|-----------|---------------------------------------|--------------------------------|----------------|
| `URL`     | String (URL)                          | The URL to the video to show.  | Yes
| `width`   | Double                                | The width of the image.        | Yes
| `height`  | Double                                | The height of the image.       | Yes
| `cornerRadius` | Double                           | The roundness of the view's corners. | Yes
| `alignment` | AlignEnum                           | Change the alignment to the left (`0`), center (`1`), or the right (`2`). |  No
| `autoplay` | Boolean (whitelisted)                | Enables auto-play for the video. | No
| `showPlaybackControls` | Boolean (whitelisted)    | Hides the controls if auto-play is enabled. | No
| `loop`    | Boolean (whitelisted)                 | Allows videos to loop if auto-play is enabledd. | No

*Note: Only whitelisted repositories can use auto-play functionality. Please contact the Sileo team for inquiries.*

#### Images

Class: `DepictionImageView`

Renders an image.

| Key       | Type                                  | Description                    | Required?
|-----------|---------------------------------------|--------------------------------|----------------|
| `URL`     | String (URL)                          | The URL to the image to show.  | Yes
| `width`   | Double                                | The width of the image.        | Yes
| `height`  | Double                                | The height of the image.       | Yes
| `cornerRadius` | Double                           | The roundness of the view's corners. | Yes
| `alignment` | AlignEnum                           | Change the alignment to the left (`0`), center (`1`), or the right (`2`). | No
| `horizontalPadding` | Double                      | Padding to put above and below the button. | No

#### Screenshots

Class: `DepictionScreenshotsView`

Displays a screenshot carousel with provided images.

| Key       | Type                                  | Description                    | Required?
|-----------|---------------------------------------|--------------------------------|----------------|
| `screenshots` | [Array of Screenshot objects](#screenshot-object) | The screenshots to be used. | Yes
| `itemCornerRadius` | Double | The roundness of the view's corners.                 | Yes
| `itemSize` | Dimensions (`{x,y}`) | Change the size of the view. | Yes
| `iphone ` | `DepictionScreenshotsView` | Override class with this property if on an iPhone. | No
| `ipad `   | `DepictionScreenshotsView` | Override class with this property if on an iPad.   | No


##### Screenshot Object

Screenshot objects just store a URL to an image and accessibility text to suppliment it.

| Key       | Type                                  | Description                    | Required?
|-----------|---------------------------------------|--------------------------------|----------------|
| `url` | String (URL) | A URL to the screenshot. | Yes
| `accessibilityText` | String | Text to be interpreted by accessibility features like VoiceOver. | Yes
| `video` | Boolean | Sets whether the URL is a video rather than an image | No

#### Table Text

Class: `DepictionTableTextView`

Adds a table cell with a given value that is ideal for displaying the current version or release date of a tweak.

| Key       | Type                                     | Description    | Required?
|-----------|---------------------------------------|-------------------|----------------|
| `title` | String | The title of the row. | Yes
| `text` | String | The text to be displayed next to the title. | Yes

#### Table Button

Class: `DepictionTableButtonView`

Adds a table cell that opens a given URL or performs another action when tapped.

| Key       | Type                                     | Description    | Required?
|-----------|---------------------------------------|-------------------|----------------|
| `title` | String | The button's label. | Yes
| `action` | String (URL) | The URL to open when the button is pressed. | Yes
| `backupAction` | String (URL) | An alternate action to try if the action is not supported. | No
| `openExternal` | Double | Set whether to open the URL in an external app. | No
| `yPadding` | Double | Padding to put above and below the button. | No

#### Button

Class: `DepictionButtonView`

Adds a button that opens a given URL or performs another action when tapped.

| Key       | Type                                     | Description    | Required?
|-----------|---------------------------------------|-------------------|----------------|
| `text` | String | The button's label. | Yes
| `action` | String (URL) | The URL to open when the button is pressed. | Yes
| `backupAction` | String (URL) | An alternate action to try if the action is not supported. | No
| `openExternal` | Double | Set whether to open the URL in an external app. | No
| `yPadding` | Double | Padding to put above and below the button. | No

#### Separator

Class: `DepictionSeparatorView`

Displays a separator.

#### Spacer

Class: `DepictionSpacerView`

Adds a space of variable height.

| Key       | Type                                     | Description    | Required?
|-----------|---------------------------------------|-------------------|----------------|
| `spacing` | Double | How high the spacer should be. | Yes

#### AdMob Integration

Class: `DepictionAdmobView`

Adds an [AdMob](https://admob.google.com/home/) banner to the depiction if you wish to display an advertisement.

| Key       | Type                | Description     | Required?
|-----------|---------------------------------------|-------------------|----------------|
| `adUnitID`| String              | Your Ad Unit ID provided by Google. | Yes 
| `adSize`  | String (`Banner`/`LargeBanner`/`SmartBanner`) | The size of the ad to be displayed. | No

#### Ratings

Class: `DepictionRatingView`

Graphically displays a rating on a five-star scale.

| Key         | Type                | Description                          | Required?
|-------------|---------------------|--------------------------------------|----------------|
| `rating`    | Double (0-5)        | How many stars should be shaded in.  | No
| `alignment` | AlignEnum           | Change the alignment to the left (`0`), center (`1`), or the right (`2`). | No


#### Reviews

Class: `DepictionReviewView`

Displays a full review entry, including author, the rating they left, and a Markdown-compatible reply.

| Key         | Type                | Description                          | Required?
|-------------|---------------------|--------------------------------------|----------------|
| `title`     | String              | The title of the review.             | Yes
| `author`    | String              | The person who wrote the review.     | Yes
| `markdown`  | String (Markdown)   | The body of the review.              | Yes
| `rating`    | Double (0-5)        | How many stars should be shaded in.  | No

