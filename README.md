# SDK 2.0 for developers

## Agenda
* [Getting started](#getting-started)
* [Widget settings](#widget-settings)
* [Style settings](#style-settings)
* [Public API](#public-api)

## Getting started

[Default SDK instruction](https://docs.google.com/document/d/11WVvjKUNsnZx3BnDH0Ttav-ac8mesgU3U7QZovGaKSA/edit)

To get started with the Carcode widget SDK, first go to your dealer settings in the Carcode dashboard to grab your "Default Widget Embed Code" (or create a new configured widget to get a new embed code). The embed code will have the format 

``` html
<script src='https://www.carcodesms.com/widgets/s/[id].js' type='text/javascript' async defer></script>
```

Once you have the embed code above, you can use it to embed the Carcode widget JavaScript toolkit into your website by simply inserting it either right before the closing `</head>` tag or in the HTML body right before the closing `</body>` tag in your site layout's HTML.

#### Custom SDK elements
Carcode widget it finds all elements containing the class `sms-button` and attaches Carcode widget click handlers to them.

Example of custom element with default behavior:

``` html
<a class="sms-button" href="sms://+15551234567" data-make="Volkswagen" data-model="Golf R"
  data-stock_no="12345" data-vin="ABC123" data-year="2015" data-status="New">Text Us</a>
```

You can specify which widget should be opened by clicking on SDK button using  `data-widget` attribute. Possible options one of:
* 'livechat'
* 'sms'
* 'facebook'

Example of custom element to open SMS form:

``` html
<a class="sms-button" href="sms://+15551234567" data-make="Volkswagen"  
  data-model="Golf R" data-stock_no="12345" data-vin="ABC123"  
  data-year="2015" data-status="New" data-widget="sms">Text Us</a>
```

## Widget settings
With the Carcode JavaScript widget toolkit embedded into the page, you’ll be able to customize the behavior of your SMS texting button(s) using the global configuration variable `__carcode` introduced into the page. 

`__carcode` should be defined before loading embed code: 

``` html
<script type='text/javascript'>
  window.__carcode = {
    skipButton: true
  };
</script>

<script src='https://www.carcodesms.com/widgets/s/[id].js' type='text/javascript' async defer></script>
```

#### Options

| Setting | Type | Default value | Description |
| -------- | -------- | -------- | -------- |
| skipButton | Boolean| false | Indicates if carcode widget should skip the default fixed button or not |
| sdkWidget | String| 'default' | Indicates which widget should be opened by clicking on SDK button. Possible options one of ['livechat', 'sms', 'facebook'] |
| sdkButtonClass | String| 'sms-button' | css class that will be used to look up SDK buttons on the page |
| appendWidgetTo | String | document.body | Id of the container where carcode widget will be rendered |
| manualControl | Boolean | false | Indicates if carcode widget should automatically init and render to the container |
| skipHeader | Boolean | false | Indicates if carcode widget should not append all it's headers |
| skipPiiForm | Boolean | false | Indicates if carcode widget should not append pii form |
| skipChatBackButton | Boolean | false | Indicates if carcode widget should not append back button in chat view |
| skipSdkButtons | Boolean | false | Indicates if carcode widget should not try to look for sdk buttons on the page |
| skipMobileApp | Boolean | false | Indicates if carcode widget should not try to open sms messenger on mobile phones |
| tracking | Object | {} | Object with params to override or extend gtm or edw events. E.g. { edw: { param1: 'test' }, gtm: { param1: 'test' } } |
| disableGoogleAnalytics | Boolean | false | Indicates if carcode widget should disable all GA tracking |
| disableEdwAnalytics | Boolean | false | Indicates if carcode widget should disable all EDW tracking |
| disableGtmAnalytics | Boolean | false | Indicates if carcode widget should disable all GTM tracking |
| cannedMessagesType | String | 'default' | Indicates the type of canned messages view. Right now we support 'default' or 'bubble' type |
| alwaysOpenForm | Boolean | The value configured in widget settings using carcode admin app | Indicates if carcode widget should always open form even for mobile experience |
| optOutText | String | Don't worry - you can opt-out by texting STOP when you don't want to receive any more texts from us. | Configure the opt out text below the phone input for sms form |
| phoneNumberPolicy | String | 'default' | Specify phone number policy. <br><br>For `'edmunds-only'` policy carcode widget will only try to use edmunds.com phone number. If no edmunds.com number, then widget will not work; <br><br>For `'edmunds-first'` policy carcode widget will firstly try to use edmunds.com number, if it exists then no additional actions. If edmunds.com number doesn't exist, then carcode widget will use sales phone number for chat and sales form, and service for service form;<br><br>For `'default'` policy carcode widget will use sales number for chat and sales form, service number for service form. |
| skipDraggable | Boolean | false | Indicates if carcode widget should not be draggable |
| themeConfiguration | Object | {} | Object with theme configuration for the widget. These settings will override all widget default settings. Please see [next section](#Theme) for more details.|

#### Example of the SDK configuration
``` javascript
window.__carcode = {
  skipButton: true, /*doesn't attach default floating button to page*/
  appendWidgetTo: 'edmundsCarcodeWidgetContainer',
  manualControl: true,
  skipHeader: true,
  skipSdkButtons: true,
  disableGoogleAnalytics: true,
  disableEdwAnalytics: true,
  disableGtmAnalytics: true,
  tracking: {
    gtm: {
      synpartner: 'some_test_synpartner',
    },
  },
  themeConfiguration: {
    formBackgroundColor: '#f3f3f3',
    formLightBackgroundColor: '#ffffff',
  },
};
```

## Style settings
Carcode widget is installed on thousands of dealerships pages and we should have some protect mechanism for our styles. We decided to not use iframe, that's why we add !important with webpack loader to all our styles. But we provide you mechanisms to customize styles: global CSS classes and `__carcode.themeConfiguration` variable.

#### SDK global classes
You may use these classes to add custom css styles.
You should use `!important` for all styles. 

| Setting | Description |
| -------- | -------- |
| .CarcodeWidgetGlobalSmsContainer | Appended to container of sms forms |
| .CarcodeWidgetGlobalChatContainer | Appended to container of chat views |
| .CarcodeWidgetGlobalButtonContainer | Appended to container of carcode widget button |
| .CarcodeWidgetGlobalCallOutContainer | Appended to container of callOut views|

In this example we add shadow to widget button:
``` css
.CarcodeWidgetGlobalButtonContainer button {
    box-shadow: 0 8px 6px -6px #1c1c1c;
}
```

#### Theme

Widget theme is configured in widget settings using carcode admin app. You may use `__carcode.themeConfiguration` to override it.

Values for color options should be valid [css color value](https://css-tricks.com/almanac/properties/c/color/).

##### Options
| Setting | Description |
| -------- | -------- |
| formBackgroundColor | Background color of widget menu, chat, sms form (see also `dotColor`) |
| formLightBackgroundColor | Background color of light elements in widget menu, chat, sms form |
| formLightColor | Font color of inside light elements |
| formTextColor | Font color of sms form  |
| separatorColor | Color of horizontal separators in widget and sms menu |
| backgroundButtonColor | Primary widget color (in particular used as background color of widget button) |
| buttonColor | Font color of elements with primary color background (in particular widget button) |
| fontSize | Font size of widget button, value should be valid [css font-size](https://css-tricks.com/almanac/properties/f/font-size/), default value: '16px' |
| fontFamily | Font family of all text elements |
| formButtonBackgroundColor | Background color of submit button in sms form |
| dotColor| Dots' color in bacground of sms-form |
| formBoxShadow | Widget shadow, value should be valid [css box-shadow](https://css-tricks.com/snippets/css/css-box-shadow/) |
| chatReplyMessageBackground | Background color of customer messages in chat view |
| chatMessageBackground | Background color of dealer messages in chat view |
| scrollBarColor | Color of scrollbar thumb |
| scrollBarBackground | Color of scrollbar track |
| inventoryPrimaryLightColor | Font color of light text elements in inventory search view |
| inventoryPrimaryDarkColor | Font color of dark text elements in inventory search view |

#### Example of the SDK theme configuration:
``` javascript
window.__carcode = {
  themeConfiguration: {
    formBackgroundColor: '#f3f3f3',
    formLightBackgroundColor: '#ffffff',
    formLightColor: '#555',
    formTextColor: '#999999',
    separatorColor: '#e8ede8',
    backgroundButtonColor: '#007EE5',
    buttonColor: '#ffffff',
    fontSize: '16px',
    fontFamily: '\'Open Sans\', \'Arial\', \'Helvetica\', \'sans-serif\'',
    formButtonBackgroundColor: '#007EE5',
    dotColor: '#f3f3f3',
    formBoxShadow: 'rgba(85,85,85,0.5)',
    cannedMessagesColor: '#4A4A4A',
    cannedMessagesBorder: '#d8d8d8',
    chatReplyMessageBackground: '#fff',
    chatMessageBackground: '#e3eaec',
    scrollBarColor: '#c1c1c1',
    scrollBarBackground: '#e9e9e9',
    inventoryButtonBackground: '#777',
    inventoryPrimaryLightColor: '#999',
    inventoryPrimaryDarkColor: '#555',
    inventoryButtonBorderColor: '#eee',
  },
};
```

## Public API

### Constructor
<i>CarcodeWidget()</i><br>
creates a new instance of the widget.
Example of usage:
``` javascript
const widget = new CarcodeWidget();
```

### Methods

[init](#-i-init-callback-i-)  
[attachSmsWidget](#-i-attachsmswidget-i-)  
[detachSmsWidget](#-i-detachsmswidget-i-)  
[openSmsSalesForm](#-i-opensmssalesform-i-)  
[openSmsServiceForm](#-i-opensmsserviceform-i-)  
[openSmsHubForm](#-i-opensmshubform-i-)  
[openChat](#-i-openchat-i-)  
[closeChat](#-i-closechat-i-)  
[openFacebook](#-i-openfacebook-i-)  
[handleChatBack](#-i-handlechatback-i-)  

#### <i>init(callback)</i>
initialize carcode widget. Callback argument will be executed once widget initialized. **This method should be always executed before all actions for manual mode.**  
Example of usage:
``` javascript
const widget = new CarcodeWidget();
widget.init(() => console.log('carcode widget has initialized'));
```
___

#### <i>attachSmsWidget()</i>
render sms form to the widget container  
Example of usage:
``` javascript
const widget = new CarcodeWidget();
widget.init(() => {
    widget.attachSmsWidget(); // sms form will be attached to the widget container
});
```
___

#### <i>detachSmsWidget()</i>
remove sms form from the widget container  
Example of usage:
``` javascript
const widget = new CarcodeWidget();
widget.init(() => {
    widget.attachSmsWidget(); // sms form will be attached to the widget container
    widget.detachSmsWidget(); // sms form will be removed from the widget container
});
```
___

#### <i>openSmsSalesForm()</i>
open sales form inside the sms widget  
Example of usage:
``` javascript
const widget = new CarcodeWidget();
widget.init(() => {
    widget.openSmsSalesForm(); // open sales form inside sms widget
    widget.attachSmsWidget(); // sms form will be attached to the widget container and sales form will be shown
});
```
___

#### <i>openSmsServiceForm()</i>
open service form inside the sms widget  
Example of usage:
``` javascript
const widget = new CarcodeWidget();
widget.init(() => {
    widget.openSmsServiceForm(); // open service form inside sms widget
    widget.attachSmsWidget(); // sms form will be attached to the widget container and service form will be shown
});
```
___

#### <i>openSmsHubForm()</i>
open hub form to select between sales and service departments  
Example of usage:
``` javascript
const widget = new CarcodeWidget();
widget.init(() => {
    widget.openSmsHubForm(); // open hub selector
    widget.attachSmsWidget(); // sms form will be attached to the widget container and hub selector will be shown
});
```
___

#### <i>openChat()</i>
render chat to the container  
Example of usage:
``` javascript
const widget = new CarcodeWidget();
widget.init(() => {
    widget.openChat();
});
```
___

#### <i>closeChat()</i>
remove chat from the container  
Example of usage:
``` javascript
const widget = new CarcodeWidget();
widget.init(() => {
    widget.openChat(); // chat renderred
    widget.closeChat(); // chat element removed
});
```
___

#### <i>openFacebook()</i>
open Facebook meesenger in new tab  
Example of usage:
``` javascript
const widget = new CarcodeWidget();
widget.init(() => {
    widget.openFacebook(); // fb messenger will be opened in new tab
});
```
___

#### <i>handleChatBack()</i>
handle one back button inside the chat view. Can be usefull when client has configured skipHeader to true, but still wants to use 'Inventory Search' feature.  
Example of usage:
``` javascript
const widget = new CarcodeWidget();
widget.init(() => {
    widget.openChat(); // chat opened
    // let's assume that customer clicks on inventory search button
    // if client configured skipHeader = true, then client doesn't have the option to go back
    widget.handleChatBack(); // will handle one back click, as a result inventory search will be closed
});
```
