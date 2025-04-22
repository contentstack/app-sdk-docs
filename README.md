# Contentstack App SDK API Reference

In this document, we will discuss the API requests that a custom location can use to communicate with Contentstack.

# How to Include App SDK into Your Project

To include the **app-sdk** library in your React app or project, you need to open command prompt and run the following command:

```sh
npm install @contentstack/app-sdk
```

Alternatively, you can add it inside script tag:

```html
<script src="https://unpkg.com/@contentstack/app-sdk/dist/index.js"></script>
```

The above command will install the **app-sdk** library in your project which contains the necessary toolkit you need to manage installed apps on specific locations.

# Top level Objects

Below we have listed some of the top-level objects used in the App SDK.

-   **[Location](#supported-locations)**: It's an object that represents all locations from the Contentstack UI.
-   **[Store](#Store)**: It refers to a class that is used by a location to store your data in the [local storage](#external_localStorage).
-   **[Stack](#Stack)**: It's a class representing the current stack in the Contentstack UI.
-   **[Window Frame](#Frame)**: It refers to a class that represents an iframe window from the Contentstack UI.
    > **Note**: This class is not available for Custom Widgets.
-   **[Entry](#Entry)**: It's a class that represents an entry from the Contentstack UI.
    > **Note**: It's not available for the Dashboard Widget extension.
-   **[Modal](#Modal)**: It's a class that represents a modal dialog opened from the app within the Contentstack UI.

# Top level Methods

The App SDK provides several top-level methods that retrieve app-related information.

-   **getConfig**: Retrieves the configuration set for the app. This method allows easy access to the app's configuration data set on the app configuration page.
-   **getCurrentLocation**: Returns the current UI location type of the app.
-   **getCurrentRegion**: : Retrieves the Contentstack Region on which the app is running.
-   **getAppVersion**: Returns the version of the app currently installed. 
    > **Note**: The getAppVersion method is not available for JSON RTE Plugins.


| Method Name       | Description                                                                   | Return Type                                                                                                                                                                               |
|-------------------|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| getConfig         | Retrieves the configuration set for the app.                                  | Promise\<Object>                                                                                                                                                                          |
| getCurrentLocation| Gets the type of currently running UI location of the app.                     |"RTE" \| "FIELD" \| "DASHBOARD" \| "WIDGET" \| "APP_CONFIG_WIDGET" \| "ASSET_SIDEBAR_WIDGET" \| "FULL_PAGE_LOCATION" \| "ENTRY_FIELD_LOCATION" \| "FIELD_MODIFIER_LOCATION" |
| getCurrentRegion  | Gets the Contentstack Region on which the app is running.                      | "UNKNOWN" \| "NA" \| "EU" \| "AZURE_NA" \| "AZURE_EU"                                                                                                                        |
| getAppVersion     | Gets the version of the app currently installed.                                 | Promise\<Number \| null>                                                                                                                                                                         ||
    
**Example**

```js
// javascript
ContentstackAppSDK.init().then(async function (appSdk) {
    // Retrieve the app configuration
    const appConfig = await appSdk.getConfig();

    // Get the current UI location of the app
    const currentLocation = appSdk.getCurrentLocation();

    // Get the Contentstack Region of the app
    const currentRegion = appSdk.getCurrentRegion();

    // Get the version of the app
    const appVersion = await appSdk.getAppVersion();
});
```

# Supported Locations

Locations refers to the position or the placement of the app (sidebar widget, custom field, etc). As of now, the following locations are supported:

-   **[CustomField](#CustomField)**: It's an object representing the current Custom field reference in the Contentstack UI.
-   **[DashboardWidget](#DashboardWidget)**: It's an object representing the Dashboard widget reference in the Contentstack UI.
-   **[SidebarWidget](#SidebarWidget)**: It's an object representing the current Sidebar widget reference in the Contentstack UI.
-   **[AppConfigWidget](#AppConfigWidget)**: It's an object representing the current App configuration for the current App in the Contentstack UI.
-   **[FieldModifierLocation](#FieldModifierLocation)**: It's an object representing the Field Modifier reference over the field in the Contentstack UI.
-   **[ContentTypeSidebarWidget](#ContentTypeSidebarWidget)**: It's an object representing the Content Type Sidebar Widget in the Contentstack UI.
-   **[GlobalFullPageWidget](#GlobalFullPage)**: It's an object representing the Global Full Page Widget in the Contentstack UI.

# External

-   **[Promise](#external_Promise)**: The Promise object represents the eventual completion (or failure) of an asynchronous operation and its resulting value.
-   **[localStorage](#external_localStorage)**: The read-only localStorage property allows you to access a Storage object for the Document's origin; the stored data is saved across browser sessions.

# Using the Contentstack App SDK

**Kind**: global class

[ContentstackAppSDK](#ContentstackAppSDK)

-   [.SDK_VERSION](#ContentstackAppSDK.SDK_VERSION) : string
-   [.init()](#ContentstackAppSDK.init) ⇒ [Promise](#external_Promise)

### ContentstackAppSDK.SDK_VERSION : string

The above defines the version of the Contentstack UI App SDK.

**Kind**: static property of [ContentstackAppSDK](#ContentstackAppSDK)

### ContentstackAppSDK.init() ⇒ [Promise](#external_Promise)

To start using the APP SDK, you first need to include the Contentstack UI App SDK and Contentstack Venus UI Stylesheet in your HTML file and then call **ContentstackAppSDK.init** on component mount.

**Kind**: The static method of [ContentstackAppSDK](#ContentstackAppSDK)

**Returns**: [Promise](#external_Promise) - A promise object which will be resolved with an instance of the [Location](#Location) class which is instantiated using the data received from the Contentstack UI.

**Example** _(App Config Widget)_

```js
// javascript
ContentstackAppSDK.init().then(function (appSdk) {
    // Get AppConfigWidget object
    // this is only initialized on the App configuration page.
    // on other locations this will return undefined.
    var appConfigWidget = await appSdk.location.AppConfigWidget;

    // fetch all Installation configuration related to
    // 1. App Configuration
    // 2. Server Configuration
    // 3. Webhooks channels
    // 4. UI Locations configured in the app
    var installationData = await appConfigWidget.getInstallationData();

    // Update all Installation configuration related to
    // 1. App Configuration
    // 2. Server Configuration
    // 3. Webhooks channels
    // 4. UI Locations configured in the app
    var getInstallationData = await appConfigWidget.setInstallationData(
        installationData
    );
});
```

**Example** _(Custom Field)_

```js
// javascript
ContentstackAppSDK.init().then(function (appSdk) {
    // Get CustomField object
    // this is only initialized on the Entry create/edit page.
    // on other locations this will return undefined.
    var customField = await appSdk.location.CustomField;

    // fetch app configuration
    var appConfig = await appSdk.getConfig();

    // fetch entry information
    var fieldData = await customField.entry.getData();
});
```

**Example** _(Sidebar Widget)_

```js
// javascript
ContentstackAppSDK.init().then(function (location) {
    // Get SidebarWidget object
    // this is only initialized on the Entry edit page.
    // on other locations this will return undefined.
    var sidebarWidget = await appSdk.location.SidebarWidget;

    // fetch app configuration
    var appConfig = await appSdk.getConfig();

    // fetch entry information
    var fieldData = await sidebarWidget.entry.getData();
});
```

**Example** _(Dashboard Widget)_

```js
// javascript
ContentstackAppSDK.init().then(function (location) {
    // Get SidebarWidget object
    // this is only initialized on the Entry edit page.
    // on other locations this will return undefined.
    var dashboardWidget = await appSdk.location.DashboardWidget;

    // fetch app configuration
    var appConfig = await appSdk.getConfig();

    // fetch stack information
    var stackData = await appSdk.stack.getData();
});
```

**Example** _(Field Modifier location)_

```js
// javascript
ContentstackAppSDK.init().then(function (appSdk) {
    // Get FieldModifierLocation object
    // this is only initialized on the Entry create/edit page.
    // on other locations this will return undefined.
    var fieldModifierLocation = await appSdk.location.FieldModifierLocation;

    // fetch app configuration
    var appConfig = await appSdk.getConfig();

    // fetch entry information
    var fieldData = await fieldModifierLocation.entry.getData();
});
```

## CustomField

It is an object representing the current Custom field reference in the Contentstack UI.

**Kind**: The instance property of [AppConfigWidget](#supported-locations)

[CustomField](#CustomField)

-   [.fieldConfig](#Location+fieldConfig) : Object
-   [.field](#Location+field) : [Field](#Field)
-   [.entry](#Location+entry) : [Entry](#Entry)
-   [.frame](#frame) : [Frame](#frame)
-   [.stack](#stack) : [Stack](#stack)

### customfield.fieldConfig : Object

The above method gives you the instance configuration parameters set from the content type builder page in the field settings. This is only available for the Custom Field location.

**Kind**: instance property of [CustomField](#CustomField)

### customfield.field : [Field](#Field)

This method gives you the custom field object which allows you to interact with the field. Please note that it is available only for the Custom Field location.

**Kind**: instance property of [CustomField](#CustomField)

### customfield.entry : [Entry](#Entry)

This method gives you the entry object which allows you to interact with the current entry. Please note that it is not available for the Dashboard Widget location.

**Kind**: instance property of [CustomField](#CustomField)

### customfield.frame : [Frame](#Frame)

The frame object provides users with methods that allow them to adjust the size of the iframe containing the location. Note that it is not available for the custom widgets location.

**Kind**: instance property of [CustomField](#CustomField)

### customfield.stack : [Stack](#Stack)

This method returns the stack object which allows users to read and manipulate a range of objects in a stack.

**Kind**: instance property of [CustomField](#CustomField)

## DashboardWidget

DashboardWidget

**Kind**: instance property of DashboardWidget

[DashboardWidget](#DashboardWidget)

-   [.frame](#Location+frame) : [Frame](#Frame)
-   [.stack](#Location+stack) : [Stack](#Stack)

### dashboardWidget.frame : [Frame](#Frame)

This frame object provides users with methods that allow them to adjust the size of the iframe that contains the extension. Not available in case of custom widgets.

**Kind**: instance property of [Location](#Location)

### dashboardWidget.stack : [Stack](#Stack)

This method returns the stack object which allows users to read and manipulate a range of objects in a stack.

**Kind**: instance property of [Location](#Location)

## SidebarWidget

It is an object representing the current Sidebar widget reference in the Contentstack UI.

**Kind**: instance property of SidebarWidget

SidebarWidget

-   [.entry](#Extension+entry) : [Entry](#Entry)
-   [.stack](#Extension+stack) : [Stack](#Stack)

### extension.entry : [Entry](#Entry)

This gives you the entry object to interact with the current entry. Please note that it is not available for the Dashboard Widget extension.

**Kind**: instance property of [Extension](#Extension)

### extension.stack : [Stack](#Stack)

This method returns the stack object to read and manipulate a range of objects in a stack.

**Kind**: instance property of [Extension](#Extension)

## AssetSidebarWidget

It is an object representing the current Asset Sidebar Widget reference in the Contentstack UI.

-   `getData()`
-   `setData(asset: Partial)`
-   `syncAsset()`
-   `updateWidth(width: number)`
-   `replaceAsset(file: File)`
-   `onSave(callback: anyFunction)`
-   `onChange(callback: anyFunction)`
-   `onPublish(callback: anyFunction)`
-   `onUnPublish(callback: anyFunction)`
-   `AssetData`

### getData()

This method returns the object representing the current asset.

### setData(asset: Partial&lt;AssetData>)

This method modifies the properties of the current asset.

### syncAsset()

If your asset has been modified externally, you can use this method to load the new asset and sync its settings with the current asset.

### updateWidth(width: number)

This method is used to modify the width of the asset sidebar panel. Using this method, you can resize the panel depending on the resolution of your content.

### replaceAsset(file: File)

This method is used to replace the current asset with a new file. Unlike `setData()`, where you can only modify the properties, you can use this method to replace the actual file.

### onSave(callback: anyFunction)

This is a callback function that runs after you save the asset settings.

### onChange(callback: anyFunction)

This is a callback function that runs every time the user modifies the asset data.

### onPublish(callback: anyFunction)

This is a callback function that is executed after a user publishes an asset.

### onUnPublish(callback: anyFunction)

This is a callback function that is executed after you unpublish an asset.

### AssetData

It is the property that you can modify using the `setData()` method.

## AppConfigWidget

It's an object representing the current App configuration for the current App in the Contentstack UI.

**Kind**: instance property of [AppConfigWidget](#supported-locations)

[AppConfigWidget](#AppConfigWidget)

-   [.getInstallationData](#AppConfigWidget+getInstallationData) : [InstallationData](#InstallationData)
-   [.setInstallationData](#AppConfigWidget+setInstallationData) : [InstallationData](#InstallationData)
-   [.setValidity(isValid: boolean, options: { message: string })](#appconfig-setValidity) : void

### appconfig.getInstallationData : InstallationData

This method gives you the complete installation data.

**Kind**: instance property of [AppConfigWidget](#AppConfigWidget)

### appconfig.setInstallationData : InstallationData

This method updates installation data for the app.

**Kind**: instance property of [AppConfigWidget](#AppConfigWidget)

### <span id="appconfig-setValidity">appconfig.setValidity(isValid: boolean, options: { message: string }) : Promise<void></span>

The `appconfig.setValidity` method is used to set the validation state of the app in the Contentstack App Config location. By invoking this method with the `isValid` parameter as `false`, the user will be prevented from saving the configuration. Additionally, an optional `message` parameter can be provided to display a custom error message.

### Parameters

- `isValid` (boolean): Specifies whether the app configuration is considered valid (`true`) or invalid (`false`).
- `options` (object): An optional object containing the following property:
  - `message` (string): A custom error message to be displayed when the configuration is invalid.

**Kind**: instance property of [AppConfigWidget](#AppConfigWidget)

```javascript
// JavaScript

// Initialize ContentstackAppSDK
ContentstackAppSDK.init().then(async function (appSdk) {
    // Get the AppConfigWidget object
    const appConfigWidget = await appSdk.location.AppConfigWidget;

    // Set validity to false, notifying the user that incorrect inputs have been entered.
    appConfigWidget.installation.setValidity(false, { message: 'Please enter valid inputs' });
});
```

## FieldModifierLocation

It is an object representing the field modifier UI location in the Contentstack UI.

**Kind**: The instance property of [FieldModifierLocation](#supported-locations)

[FieldModifierLocation](#FieldModifierLocation)

-   [.field](#Location+field) : [Field](#Field)
-   [.entry](#Location+entry) : [Entry](#Entry)
-   [.frame](#frame) : [Frame](#frame)
-   [.stack](#stack) : [Stack](#stack)


### FieldModifierLocation.field : [Field](#Field)

This method gives you the current field object which allows you to interact with the field.

**Kind**: instance property of [FieldModifierLocation](#FieldModifierLocation)

-   [Field](#Field)
    -   [.uid](#Field+uid) : <code>string</code>
    -   [.data_type](#Field+data_type) : <code>string</code>
    -   [.schema](#Field+schema) : <code>Object</code>
    -   [.setData(data)](#Field+setData) ⇒ [<code>Promise</code>](#external_Promise)
    -   [.getData(options)](#Field+getData) ⇒ <code>Object</code> \| <code>string</code> \| <code>number</code>

<a name="Field+uid"></a>

#### field.uid : <code>string</code>

The UID of the current field is defined in the content type of the entry.

**Kind**: instance property of [<code>Field</code>](#Field)
<a name="Field+data_type"></a>

#### field.data_type : <code>string</code>

The data type of the current field is set using this method.

**Kind**: instance property of [<code>Field</code>](#Field)
<a name="Field+schema"></a>

#### field.schema : <code>Object</code>

The schema of the current field (schema of fields such as ‘Single Line Textbox’, ‘Number’,
and so on) is set using this method.

**Kind**: instance property of [<code>Field</code>](#Field)
<a name="Field+setData"></a>

#### field.setData(data) ⇒ [<code>Promise</code>](#external_Promise)

Sets the data for the current field.

**Kind**: instance method of [<code>Field</code>](#Field)
**Returns**: [<code>Promise</code>](#external_Promise) - A promise object which is resolved when data is set for a field. Note: The data set by this function will only be saved when the user saves the entry.

| Param | Type                                                              | Description                 |
| ----- | ----------------------------------------------------------------- | --------------------------- |
| data  | <code>Object</code> \| <code>string</code> \| <code>number</code> | Data to be set on the field |

<a name="Field+getData"></a>

#### field.getData(options) ⇒ <code>Object</code> \| <code>string</code> \| <code>number</code>

Gets the data of the current field.

**Kind**: instance method of [<code>Field</code>](#Field)
**Returns**: <code>Object</code> \| <code>string</code> \| <code>number</code> - Returns the field data.

| Param            | Type                 | Description                                                                                                                                                                                |
| ---------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| options          | <code>Object</code>  | Options object for getData() method.                                                                                                                                                        |
| options.resolved | <code>boolean</code> | If the resolved parameter is set to true for the File field, then the method will return a resolved asset object along with all the field metadata, e.g. 'field.getData({resolved:true})'. |


<a name="Field+onChange"></a>

#### field.onChange(callback)

The field.onChange() function is called when another extension programmatically changes the data of the current extension field using the field.setData() function. This function is only available for extension fields that support the following data types: text, number, boolean, or date.

**Kind**: instance method of [<code>Field</code>](#Field)

| Param    | Type                  | Description                                           |
| -------- | --------------------- | ----------------------------------------------------- |
| callback | <code>function</code> | The function to be called when an entry is published. |

### FieldModifierLocation.entry :

This method gives you the entry, object which allows you to interact with the current entry.

**Kind**: instance property of [FieldModifierLocation](#FieldModifierLocation)

-   [Entry](#Entry)
    -   [.content_type](#entry) : <code>Object</code>
    -   [.locale](#entrylocale--string) : <code>string</code>
    -   [.getData()](#entrygetdata--object) ⇒ <code>Object</code>
    -   [.getField(uid, options?)](#entrygetfielduid-options--object) ⇒ <code>Object</code>
    -   [.onSave(callback)](#entryonsavecallback)
    -   [.onChange(callback)](#Entry+onChange)
    -   [.onPublish(callback)](#Entry+onPublish)
    -   [.onUnPublish(callback)](#Entry+onUnPublish)
    -   [.getTags()](#entrygettagsoptions--array)
    -   [.setTags(tags)](#entrysettagstags--array)

<a name="FieldModifierLocation+Entry+content_type"></a>

#### entry.content_type : <code>Object</code>

Gets the content type of the current entry.

**Kind**: instance property of [<code>Entry</code>](#Entry)

<a name="FieldModifierLocation+Entry+locale"></a>

#### entry.locale : <code>string</code>

Gets the locale of the current entry.

**Kind**: instance property of [<code>Entry</code>](#Entry)

<a name="FieldModifierLocation+Entry+getData"></a>

#### entry.getData() ⇒ <code>Object</code>

Gets data of the current entry.

**Kind**: instance method of [<code>Entry</code>](#Entry)
**Returns**: <code>Object</code> - Returns entry data.

<a name="FieldModifierLocation+Entry+getField"></a>

#### entry.getField(uid, options?) ⇒ <code>Object</code>

It gets the field object, allowing you to interact with it.
This object will support all the methods and properties that work with the extension.field.
Note: For fields initialized using the getFields function, the setData() function currently works only for the following fields: single_line, multi_line, RTE, markdown, select, number, boolean, date, link, and extension of data type text, number, boolean, and date.

**Kind**: instance method of [<code>Entry</code>](#Entry)
**Returns**: <code>Object</code> - Field object

| Param                    | Type                 | Description                              |
| ------------------------ | -------------------- | ---------------------------------------- |
| uid                      | <code>string</code>  | Unique ID of the field                   |
| options.useUnsavedSchema | <code>boolean</code> | Gets the unsaved draft data of the field |

**Example**

```js
var field = entry.getField("field_uid");
var fieldSchema = field.schema;
var fieldUid = field.uid;
var fieldData = field.getData();
```

<a name="FieldModifierLocation+Entry+onSave"></a>

#### entry.onSave(callback)

This onSave() function executes the callback function every time an entry is saved.

**Kind**: instance method of [<code>Entry</code>](#Entry)

| Param    | Type                  | Description                                       |
| -------- | --------------------- | ------------------------------------------------- |
| callback | <code>function</code> | The function to be called when an entry is saved. |

<a name="FieldModifierLocation+Entry+onChange"></a>

#### entry.onChange(callback)

The onChange() function executes the callback function every time an entry has been updated.

**Kind**: instance method of [<code>Entry</code>](#Entry)

| Param    | Type                  | Description                                         |
| -------- | --------------------- | --------------------------------------------------- |
| callback | <code>function</code> | The function to be called when an entry is updated. |

<a name="FieldModifierLocation+Entry+onPublish"></a>

#### entry.onPublish(callback)

The onPublish() function executes the callback function every time an entry has been published with the respective payload.

**Kind**: instance method of [<code>Entry</code>](#Entry)

| Param    | Type                  | Description                                           |
| -------- | --------------------- | ----------------------------------------------------- |
| callback | <code>function</code> | The function to be called when an entry is published. |

<a name="FieldModifierLocation+Entry+onUnPublish"></a>

#### entry.onUnPublish(callback)

The onPublish() function executes the callback function every time an entry has been unpublished with the respective payload.

**Kind**: instance method of [<code>Entry</code>](#Entry)

| Param    | Type                  | Description                                             |
| -------- | --------------------- | ------------------------------------------------------- |
| callback | <code>function</code> | The function to be called when an entry is unpublished. |

<a name="FieldModifierLocation+Entry+getTags"></a>

#### entry.getTags(options?) ⇒ <code>Array</code>

Gets the tags of the current entry. By default, this method gets the saved tags, but you can get the draft tags by setting the options.useUnsavedSchema parameter.

**Kind**: instance method of [<code>Entry</code>](#Entry)
**Returns**: <code>Array</code> - List of tags

| Param                    | Type                 | Description                              |
| ------------------------ | -------------------- | ---------------------------------------- |
| options.useUnsavedSchema | <code>boolean</code> | Gets the unsaved draft data of the field |

<a name="FieldModifierLocation+Entry+setTags"></a>

#### entry.setTags(tags) ⇒ <code>Array</code>

Sets the tags in the entry.

**Kind**: instance method of [<code>Entry</code>](#Entry)
**Returns**: <code>Array</code> - List of tags

| Param | Type               | Description          |
| ----- | ------------------ | -------------------- |
| tags  | <code>Array</code> | The tags to be saved |


### FieldModifierLocation.stack : [Stack](#Stack)

This method returns the stack object which allows users to read and manipulate a range of objects in a stack.

**Kind**: instance property of [FieldModifierLocation](#FieldModifierLocation)

### FieldModifierLocation.frame: [Frame](#Frame)

The frame object provides users with methods that allow them to adjust the size of the iframe containing the UI location.

**Kind**: instance property of [FieldModifierLocation](#FieldModifierLocation)

-   [.updateDimension({height?, width?})](#frameupdatedimensionheight-width--promise) ⇒ [Promise](#external_Promise)
-   [.enableAutoResizing()](#frame+enableAutoResizing) ⇒ [frame](#frame)
-   [.disableAutoResizing()](#frame+disableAutoResizing) ⇒ [frame](#frame)
-   [.preventFrameClose(state: boolean)](#frame+preventFrameClose) ⇒ void


#### frame.updateDimension({height?, width?}) ⇒ [Promise](#external_Promise)

This method updates the extension dimensions on the Contentstack UI. If the dimensions are not provided, it will update the dimensions of the extension to the dimensions of the content.

**Kind**: instance method of [frame](#frame)

**Returns**: [Promise](#external_Promise) - A promise object which will be resolved when Contentstack UI sends an acknowledgement stating that the height has been updated.

| **Parameter** | **Type** | **Description**                     |
| :------------ | :------- | :---------------------------------- |
| config.height | number   | Desired height of the iframe window |
| config.width  | number   | Desired width of the iframe window  |


#### frame.enableAutoResizing() ⇒ [frame](#frame)

This method enables the auto resizing of the extension height.

**Kind**: instance method of [frame](#frame)

**Returns**: [frame](#frame)

#### frame.disableAutoResizing() ⇒ [frame](#frame)

This method disables the auto resizing of the extension height.

**Kind**: instance method of [frame](#frame)

**Returns**: [frame](#frame).

#### frame.preventFrameClose(state: boolean) ⇒ [Promise\<void>](#external_Promise)

The `frame.preventFrameClose` method allows you to manage the default closing behavior of the app when the user clicks outside its frame. By default, the app is closed in such scenarios. This method enables you to control and customize the closing behavior based on your requirements.

**Kind**: instance method of [frame](#frame)

| **Parameter** | **Type** | **Description**                     |
| --------- | -------- | --------------------------------------- |
| state     | boolean  | State to control the default closing behavior. Set to `true` to prevent the frame from closing on click outside, and `false` to revert to the default behavior.

```js
// JavaScript
ContentstackAppSDK.init().then(async function (appSdk) {
    // Get the FieldModifierLocation object
    const fieldModifierLocation = await appSdk.location.FieldModifierLocation;

    // To disable the automatic closure of the app frame when the user clicks outside
    fieldModifierLocation.frame.preventFrameClose(true);
});
```

## ContentTypeSidebarWidget

It is an object representing the Content Type Sidebar Widget in the Contentstack UI.

**Kind**: The instance property of [ContentTypeSidebarWidget](#supported-locations)

### ContentTypeSidebarWidget.getData() ⇒ [ContentType](#ContentType)

This method retrieves the current content type data of the sidebar widget.

**Kind**: instance method of [ContentTypeSidebarWidget](#ContentTypeSidebarWidget)

**Returns**: [ContentType](#ContentType) - The current content type data.

### ContentTypeSidebarWidget.onSave(callback)

Executes the specified callback function each time the content type is saved. The callback function receives the updated content type data as an argument.

**Kind**: instance method of [ContentTypeSidebarWidget](#ContentTypeSidebarWidget)

| Param    | Type                  | Description                                                      |
| -------- | --------------------- | ---------------------------------------------------------------- |
| callback | <code>function</code> | Function called with the updated content type data on save event |

**Throws**: Will throw an error if the provided `callback` is not a function.

**Example**

```js
const sidebarWidget = new ContentTypeSidebarWidget(
  initData,
  connection,
  emitter
);

// Register onSave callback to execute on each save event.
sidebarWidget.onSave((updatedContentType) => {
  console.log("Content type saved:", updatedContentType);
});
```

## GlobalFullPageWidget

It is an object representing the Global Full Page Widget in the Contentstack UI.

**Kind**: The instance property of [GlobalFullPageWidget](#supported-locations)

### GlobalFullPage.currentOrganization ⇒ [ContentType](#ContentType)

This method retrieves the current content type data of the sidebar widget.

**Kind**: instance method of [ContentTypeSidebarWidget](#ContentTypeSidebarWidget)

**Returns**: [ContentType](#ContentType) - The current content type data.

**Example**

```js
const sidebarWidget = new ContentTypeSidebarWidget(
  initData,
  connection,
  emitter
);

// Register onSave callback to execute on each save event.
sidebarWidget.onSave((updatedContentType) => {
  console.log("Content type saved:", updatedContentType);
});
```

## frame

It is a class representing an iframe window from the Contentstack UI. Please note that it is not available for Custom Widgets.

**Kind**: global class

[frame](#frame)

-   [.enableResizing()](#frame+enableResizing) ⇒ [Promise](#external_Promise)
-   [.onDashboardResize(callback)](#frame+onDashboardResize) ⇒ boolean
-   [.updateHeight(height)](#frame+updateHeight) ⇒ [Promise](#external_Promise)
-   [.enableAutoResizing()](#frame+enableAutoResizing) ⇒ [frame](#frame)
-   [.disableAutoResizing()](#frame+disableAutoResizing) ⇒ [frame](#frame)

### frame.enableResizing() ⇒ [Promise](#external_Promise)

This method activates the resize button that gives you the provision to resize your Dashboard Widget.

**Kind**: instance method of [frame](#frame)

**Returns**: [Promise](#external_Promise) - A promise object which will resolve when a resize button is visible on the Dashboard Widget.

### frame.onDashboardResize(callback) ⇒ boolean

This function executes the callback function whenever a Dashboard Widget extension is maximized or minimized. Please note that it is only applicable for Dashboard Widget extensions.

**Kind**: instance method of [frame](#frame)

**Returns**: boolean - Will return true

| **Parameter** | **Type** | **Description**                                                                   |
| :------------ | :------- | :-------------------------------------------------------------------------------- |
| callback      | function | Function to be called when a Dashboard Widget extension is maximized or minimized |

### frame.updateHeight(height) ⇒ [Promise](#external_Promise)

This method updates the extension height on the Contentstack UI. If the ‘height’ argument is not passed, it will calculate the scroll height and set the height of the location window accordingly.

**Kind**: instance method of [frame](#frame)

**Returns**: [Promise](#external_Promise) - A promise object which will be resolved when Contentstack UI sends an acknowledgement stating that the height has been updated.

| **Parameter** | **Type**         | **Description**                     |
| :------------ | :--------------- | :---------------------------------- |
| height        | string \| number | Desired height of the iframe window |

### frame.enableAutoResizing() ⇒ [frame](#frame)

This method enables the auto resizing of the extension height.

**Kind**: instance method of [frame](#frame)

**Returns**: [frame](#frame)

### frame.disableAutoResizing() ⇒ [frame](#frame)

This method disables the auto resizing of the extension height.

**Kind**: instance method of [frame](#frame)

**Returns**: [frame](#frame).

## Stack

It is a class representing the current stack in Contentstack UI.

**Kind**: global class

[Stack](#Stack)

-   [.ContentType](#Stack+ContentType)
    -   [new this.ContentType(uid)](#new_Stack+ContentType_new)
    -   [.Entry](#Stack+ContentType+Entry)
        -   [new Entry()](#new_Stack+ContentType+Entry_new)
        -   _instance_
            -   [.only([key], values)](#Stack+ContentType+Entry+only) ⇒ [Entry](#Stack+ContentType+Entry)
            -   [.except([key], values)](#Stack+ContentType+Entry+except) ⇒ [Entry](#Stack+ContentType+Entry)
            -   [.addParam(key, value)](#Stack+ContentType+Entry+addParam) ⇒ [Entry](#Stack+ContentType+Entry)
            -   [.getReferences()](#Stack+ContentType+Entry+getReferences) ⇒ [Promise](#external_Promise)
            -   [.delete()](#Stack+ContentType+Entry+delete) ⇒ [Promise](#external_Promise)
            -   [.fetch()](#Stack+ContentType+Entry+fetch) ⇒ [Promise](#external_Promise)
            -   [.includeReference()](#Stack+ContentType+Entry+includeReference) ⇒ [Entry](#Stack+ContentType+Entry)
            -   [.language(languageCode)](#Stack+ContentType+Entry+language) ⇒ [Entry](#Stack+ContentType+Entry)
            -   [.environment(environment_uid)](#Stack+ContentType+Entry+environment) ⇒ [Entry](#Stack+ContentType+Entry)
            -   [.addQuery(key, value)](#Stack+ContentType+Entry+addQuery) ⇒ [Entry](#Stack+ContentType+Entry)
            -   [.includeSchema()](#Stack+ContentType+Entry+includeSchema) ⇒ [Entry](#Stack+ContentType+Entry)
            -   [.includeContentType()](#Stack+ContentType+Entry+includeContentType) ⇒ [Entry](#Stack+ContentType+Entry)
            -   [.includeOwner()](#Stack+ContentType+Entry+includeOwner) ⇒ [Entry](#Stack+ContentType+Entry)
            -   [.getLanguages()](#Stack+ContentType+Entry+getLanguages) ⇒ [Promise](#external_Promise)
            -   [.unlocalize(locale)](#Stack+ContentType+Entry+unlocalize) ⇒ [Promise](#external_Promise)
            -   [.publish(payload, api_version="")](#Stack+ContentType+Entry+publish) ⇒ [Promise](#external_Promise)
            -   [.unpublish(payload)](#Stack+ContentType+Entry+unpublish) ⇒ [Promise](#external_Promise)
            -   [.setWorkflowStage(payload)](#Stack+ContentType+Entry+setWorkflowStage) ⇒ [Promise](#external_Promise)
            -   [.update(payload, [locale])](#Stack+ContentType+Entry+update) ⇒ [Promise](#external_Promise)
        -   _static_
            -   [.Query()](#Stack+ContentType+Entry.Query) ⇒ [Query](#Query)
            -   [.create(payload)](#Stack+ContentType+Entry.create) ⇒ [Promise](#external_Promise)
-   [.Asset](#Stack+Asset)
    -   [new this.Asset(uid)](#new_Stack+Asset_new)
    -   _instance_
        -   [.only([key], values)](#Stack+Asset+only) ⇒ [Asset](#Stack+Asset)
        -   [.except([key], values)](#Stack+Asset+except) ⇒ [Asset](#Stack+Asset)
        -   [.environment(environment_uid)](#Stack+Asset+environment) ⇒ [Asset](#Stack+Asset)
        -   [.addParam(key, value)](#Stack+Asset+addParam) ⇒ [Asset](#Stack+Asset)
        -   [.addQuery(key, value)](#Stack+Asset+addQuery) ⇒ [Asset](#Stack+Asset)
        -   [.getReferences()](#Stack+Asset+getReferences) ⇒ [Promise](#external_Promise)
        -   [.delete()](#Stack+Asset+delete) ⇒ [Promise](#external_Promise)
        -   [.publish(payload, api_version="")](#Stack+Asset+publish) ⇒ [Promise](#external_Promise)
        -   [.unpublish(payload)](#Stack+Asset+unpublish) ⇒ [Promise](#external_Promise)
    -   _static_
        -   [.Query()](#Stack+Asset.Query) ⇒ [Query](#Query)
        -   [.getRteAssets()](#Stack+Asset.getRteAssets) ⇒ [Promise](#external_Promise)
        -   [.getAssetsOfSpecificTypes(assetType)](#Stack+Asset.getAssetsOfSpecificTypes) ⇒ [Promise](#external_Promise)
-   [.getData()](#Stack+getData) ⇒ Object
-   [.getContentType(uid, params)](#Stack+getContentType) ⇒ Object
-   [.getContentTypes(query, params)](#Stack+getContentTypes) ⇒ Object
-   [.getEnvironment(name, params)](#Stack+getEnvironment) ⇒ Object
-   [.getEnvironments(query, params)](#Stack+getEnvironments) ⇒ Object
-   [.getLocale(code, params)](#Stack+getLocale) ⇒ Object
-   [.getLocales(query, params)](#Stack+getLocales) ⇒ Object
-   [.getReleases(query, params)](#stackgetreleasesquery-params--object) ⇒ Object
-   [.getPublishes(query, params)](#stackgetpublishesquery-params--object) ⇒ Object
-   [.search(queries, apiKey)](#stacksearchqueries-stacksearchquery-apikey-string--null--promiseobject) => [Promise](#promise)
-   [.getAllStacks(options)](#stackgetallstacksoptions-orguid---params---promiseobject): [Promise](#promise)
-   [.getCurrentBranch()](#stackgetcurrentbranch--branchdetail--null) ⇒ Object | null
-   [.getAllBranch()](#stackgetallbranches--branchdetail--null) ⇒ Object[]
-   [.getGlobalField(uid, params)](#Stack+getGlobalField) ⇒ Object
-   [.getGlobalFields(query, params)](#Stack+getGlobalFields) ⇒ Object
-   [.getVariantById(variant_uid)](#Stack+getVariantById) ⇒ Object

### stack.ContentType

**Kind**: instance class of [Stack](#Stack)

**See**: [ContentType](https://www.contentstack.com/docs/apis/content-management-api/#content-types)

[.ContentType](#Stack+ContentType)

-   [new this.ContentType(uid)](#new_Stack+ContentType_new)
-   [.Entry](#Stack+ContentType+Entry)
    -   [new Entry()](#new_Stack+ContentType+Entry_new)
    -   _instance_
        -   [.only([key], values)](#Stack+ContentType+Entry+only) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.except([key], values)](#Stack+ContentType+Entry+except) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.addParam(key, value)](#Stack+ContentType+Entry+addParam) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.getReferences()](#Stack+ContentType+Entry+getReferences) ⇒ [Promise](#external_Promise)
        -   [.delete()](#Stack+ContentType+Entry+delete) ⇒ [Promise](#external_Promise)
        -   [.fetch()](#Stack+ContentType+Entry+fetch) ⇒ [Promise](#external_Promise)
        -   [.includeReference()](#Stack+ContentType+Entry+includeReference) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.language(languageCode)](#Stack+ContentType+Entry+language) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.environment(environment_uid)](#Stack+ContentType+Entry+environment) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.addQuery(key, value)](#Stack+ContentType+Entry+addQuery) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.includeSchema()](#Stack+ContentType+Entry+includeSchema) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.includeContentType()](#Stack+ContentType+Entry+includeContentType) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.includeOwner()](#Stack+ContentType+Entry+includeOwner) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.getLanguages()](#Stack+ContentType+Entry+getLanguages) ⇒ [Promise](#external_Promise)
        -   [.unlocalize(locale)](#Stack+ContentType+Entry+unlocalize) ⇒ [Promise](#external_Promise)
        -   [.publish(payload, api_version="")](#Stack+ContentType+Entry+publish) ⇒ [Promise](#external_Promise)
        -   [.unpublish(payload)](#Stack+ContentType+Entry+unpublish) ⇒ [Promise](#external_Promise)
        -   [.setWorkflowStage(payload)](#Stack+ContentType+Entry+setWorkflowStage) ⇒ [Promise](#external_Promise)
        -   [.update(payload, [locale])](#Stack+ContentType+Entry+update) ⇒ [Promise](#external_Promise)
    -   _static_
        -   [.Query()](#Stack+ContentType+Entry.Query) ⇒ [Query](#Query)
        -   [.create(payload)](#Stack+ContentType+Entry.create) ⇒ [Promise](#external_Promise)

#### new this.ContentType(uid)

A content type defines the structure or the schema of a page or a section of your web or mobile property.

| **Parameter** | **Type** | **Description**     |
| :------------ | :------- | :------------------ |
| uid           | string   | UID of content type |

**Example**

extension.stack.ContentType('content_type_uid')

#### contentType.Entry

**Kind**: instance class of [ContentType](#Stack+ContentType)

**See**: [Entries](https://www.contentstack.com/docs/apis/content-management-api/#entries)

-   [.Entry](#Stack+ContentType+Entry)
    -   [new Entry()](#new_Stack+ContentType+Entry_new)
    -   _instance_
        -   [.only([key], values)](#Stack+ContentType+Entry+only) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.except([key], values)](#Stack+ContentType+Entry+except) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.addParam(key, value)](#Stack+ContentType+Entry+addParam) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.getReferences()](#Stack+ContentType+Entry+getReferences) ⇒ [Promise](#external_Promise)
        -   [.delete()](#Stack+ContentType+Entry+delete) ⇒ [Promise](#external_Promise)
        -   [.fetch()](#Stack+ContentType+Entry+fetch) ⇒ [Promise](#external_Promise)
        -   [.includeReference()](#Stack+ContentType+Entry+includeReference) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.language(languageCode)](#Stack+ContentType+Entry+language) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.environment(environment_uid)](#Stack+ContentType+Entry+environment) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.addQuery(key, value)](#Stack+ContentType+Entry+addQuery) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.includeSchema()](#Stack+ContentType+Entry+includeSchema) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.includeContentType()](#Stack+ContentType+Entry+includeContentType) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.includeOwner()](#Stack+ContentType+Entry+includeOwner) ⇒ [Entry](#Stack+ContentType+Entry)
        -   [.getLanguages()](#Stack+ContentType+Entry+getLanguages) ⇒ [Promise](#external_Promise)
        -   [.unlocalize(locale)](#Stack+ContentType+Entry+unlocalize) ⇒ [Promise](#external_Promise)
        -   [.publish(payload, api_version="")](#Stack+ContentType+Entry+publish) ⇒ [Promise](#external_Promise)
        -   [.unpublish(payload)](#Stack+ContentType+Entry+unpublish) ⇒ [Promise](#external_Promise)
        -   [.setWorkflowStage(payload)](#Stack+ContentType+Entry+setWorkflowStage) ⇒ [Promise](#external_Promise)
        -   [.update(payload, [locale])](#Stack+ContentType+Entry+update) ⇒ [Promise](#external_Promise)
    -   _static_
        -   [.Query()](#Stack+ContentType+Entry.Query) ⇒ [Query](#Query)
        -   [.create(payload)](#Stack+ContentType+Entry.create) ⇒ [Promise](#external_Promise)

##### new Entry()

An entry is the actual piece of content created using one of the defined content types.

##### entry.only([key], values) ⇒ [Entry](#Stack+ContentType+Entry)

This method is used to show the selected fields of an entry in the result set.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

| **Parameter** | **Type** | **Default** | **Description**                               |
| :------------ | :------- | :---------- | :-------------------------------------------- |
| [key]         | String   | BASE        | Single field of an entry                      |
| values        | Array    |             | Array of fields to be shown in the result set |

**Example** _( Only with field UID )_

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry("bltsomething123")
    .only("title")
    .fetch();
```

**Example** _( Only with field UID )_

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry("bltsomething123")
    .only("BASE", "title")
    .fetch();
```

**Example** _( Only with field UIDs(array) )_

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry("bltsomething123")
    .only(["title", "description"])
    .fetch();
```

##### entry.except([key], values) ⇒ [Entry](#Stack+ContentType+Entry)

This method is used to hide the selected fields of an entry in the result set.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

| **Parameter** | **Type** | **Default** | **Description**                                |
| :------------ | :------- | :---------- | :--------------------------------------------- |
| [key]         | String   | BASE        | Single field of an entry                       |
| values        | Array    |             | Array of fields to be hidden in the result set |

**Example** _( Except with field uid )_

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry("bltsomething123")
    .except("title")
    .fetch();
```

**Example** _( Except with field uid )_

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry("bltsomething123")
    .except("BASE", "title")
    .fetch();
```

**Example** _( Except with field uids(array) )_

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry("bltsomething123")
    .except(["title", "description"])
    .fetch();
```

##### entry.addParam(key, value) ⇒ [Entry](#Stack+ContentType+Entry)

This method includes a query parameter in a query.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

**Returns**: [Entry](#Stack+ContentType+Entry) - Returns

| **Parameter** | **Type** | **Description**        |
| :------------ | :------- | :--------------------- |
| key           | string   | Key of the parameter   |
| value         | string   | Value of the parameter |

**Example**

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry("uid")
    .addParam("include_count", "true")
    .fetch()
    .then()
    .catch();
```

##### entry.getReferences() ⇒ [Promise](#external_Promise)

This method fetches all the entries in which the current entry is referenced.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

**Returns**: [Promise](#external_Promise) - Required data if resolved successfully

**Additional Resource**:Learn more about [Entry references](https://www.contentstack.com/docs/apis/content-management-api/#get-all-references-of-an-entry%7C%20Entry%20References)

**Example**

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry("uid")
    .getReferences()
    .then()
    .catch();
```

##### entry.delete() ⇒ [Promise](#external_Promise)

This method deletes an existing entry.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

**Returns**: [Promise](#external_Promise) - Required data if resolved successfully

**Additional Resource**: Learn more about [Deleting an Entry](https://www.contentstack.com/docs/apis/content-management-api/#delete-an-entry| Delete Entry).

**Example**

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry("uid")
    .delete()
    .then()
    .catch();
```

##### entry.fetch() ⇒ [Promise](#external_Promise)

This method fetches information of a specific entry.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

**Returns**: [Promise](#external_Promise) - Required data if resolved successfully.

**Additional Resource**: Learn more about [Fetching a Single Entry](https://www.contentstack.com/docs/apis/content-management-api/#get-a-single-an-entry).

**Example**

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry("uid")
    .fetch()
    .then()
    .catch();
```

##### entry.includeReference() ⇒ [Entry](#Stack+ContentType+Entry)

This method is used to include referenced entries from other content types.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

**Example** _( .includeReference with reference_field_uids as array )_

```js
stack
    .ContentType("contenttype_uid")
    .Entry("bltsomething123")
    .includeReference(["category", "author"])
    .fetch();
```

**Example** _( .includeReference with reference_field_uids )_

```js
stack
    .ContentType("contenttype_uid")
    .Entry("bltsomething123")
    .includeReference("category", "author")
    .fetch();
```

##### entry.language(languageCode) ⇒ [Entry](#Stack+ContentType+Entry)

This method is used to set the language code of which you want to retrieve the data.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

| **Parameter** | **Type** | **Description**                                      |
| :------------ | :------- | :--------------------------------------------------- |
| languageCode  | String   | Language code, for e.g. ‘en-us’, ‘ja-jp’, and so on. |

**Example**

```js
extension.stack
    .ContentType("contenttype_uid")
    .Entry("bltsomething123")
    .language("en-us")
    .fetch();
```

##### entry.environment(environment_uid) ⇒ [Entry](#Stack+ContentType+Entry)

This method is used to set the environment name of which you want to retrieve the data.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

| **Parameter**   | **Type** | **Description**          |
| :-------------- | :------- | :----------------------- |
| environment_uid | String   | UID/Name of environment. |

**Example**

```js
extension.stack
    .ContentType("contenttype_uid")
    .Entry("bltsomething123")
    .environment("development")
    .fetch();
```

##### entry.addQuery(key, value) ⇒ [Entry](#Stack+ContentType+Entry)

This method is used to add a query to an entry object.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

| **Parameter** | **Type** | **Description**    |
| :------------ | :------- | :----------------- |
| key           | String   | Key of the query   |
| value         | String   | Value of the query |

**Example**

```js
extension.stack
    .ContentType("contenttype_uid")
    .Entry("bltsomething123")
    .addQuery("include_schema", true)
    .fetch();
```

##### entry.includeSchema() ⇒ [Entry](#Stack+ContentType+Entry)

This method is used to include the schema of the current content type in the result set along with the entry/entries.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

**Example**

```js
extension.stack
    .ContentType("contenttype_uid")
    .Entry("bltsomething123")
    .includeSchema()
    .fetch();
```

##### entry.includeContentType() ⇒ [Entry](#Stack+ContentType+Entry)

This method is used to include the current content type in the result set along with the entry(ies).

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

**Example**

```js
extension.stack
    .ContentType("contenttype_uid")
    .Entry("bltsomething123")
    .includeContentType()
    .fetch();
```

##### entry.includeOwner() ⇒ [Entry](#Stack+ContentType+Entry)

This method is used to include the owner of the entry(ies) in the result set.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

**Example**

```js
extension.stack
    .ContentType("contenttype_uid")
    .Entry("bltsomething123")
    .includeOwner()
    .fetch();
```

##### entry.getLanguages() ⇒ [Promise](#external_Promise)

This method returns the details of all languages in which the entry is localized.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

**Example**

```js
extension.stack
    .ContentType("contenttype_uid")
    .Entry("bltsomething123")
    .getLanguages();
```

##### entry.unlocalize(locale) ⇒ [Promise](#external_Promise)

This method is used to unlocalize an entry.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

| **Parameter** | **Type** | **Description**                                 |
| :------------ | :------- | :---------------------------------------------- |
| locale        | string   | Locale in which the entry is to be unlocalized. |

**Example**

```js
extension.stack.ContentType('contenttype_uid').Entry('bltsomething123').unlocalize('fr-fr').then(...).catch(...);
```

##### entry.publish(payload, api_version="") ⇒ [Promise](#external_Promise)

This method lets you publish an entry either immediately or schedule it to be published automatically at a later date/time.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

| **Parameter** | **Type** | **Description**          |
| :------------ | :------- | :----------------------- |
| payload       | object   | Payload for the request. |
| api_version   | string   | Send api version in headers.(optional argument) |

**Example**

```js
extension.stack.ContentType('contenttype_uid').Entry('bltsomething123').publish({
 "entry": {
 "environments": ["development"],
 "locales": ["en-us"]
 },
 "locale": "en-us",
 "version": 1,
 "scheduled_at": "2019-02-14T18:30:00.000Z"
 }).then(...).catch(...);
```

##### entry.unpublish(payload) ⇒ [Promise](#external_Promise)

This method lets you unpublish an entry either immediately or schedule it to be unpublished automatically at a later date/time.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

| **Parameter** | **Type** | **Description**          |
| :------------ | :------- | :----------------------- |
| payload       | object   | Payload for the request. |

**Example**

```js
extension.stack.ContentType('contenttype_uid').Entry('bltsomething123').unpublish({
      "entry": {
          "environments": ["development"],
          "locales": ["en-us"]
      },
      "locale": "en-us",
      "version": 1,
      "scheduled_at": "2019-02-14T18:30:00.000Z"
  }).then(...).catch(...);
```

##### entry.setWorkflowStage(payload) ⇒ **[Promise](#external_Promise)**

This method allows you to either set a particular workflow stage or update the workflow stage details of an entry.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)
| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :----------------------- |
| payload | object | Payload for the request. |

**Example**

```js
extension.stack.ContentType('contenttype_uid').Entry('bltsomething123').setWorkflowStage({
     "workflow": {
         "workflow_stage": {
             "comment": "Test Comment",
             "due_date": "Thu Dec 01 2018",
             "notify": false,
             "uid": "blt9f52a2cd5e0014fb",
             "assigned_to": [{
                 "uid": "blt296a22e28cc0c63c",
                 "name": "John Doe",
                 "email": "john.doe@contentstack.com"
             }],
             "assigned_by_roles": [{
                 "uid": "blt5b74c24c7ae25d94",
                 "name": "Content Manager"
             }]
         }
     }
  }).then(...).catch(...);
```

##### entry.update(payload, [locale]) ⇒ [Promise](#external_Promise)

This call allows you to update the entry content.

**Kind**: instance method of [Entry](#Stack+ContentType+Entry)

**See**: [Update Entry](https://www.contentstack.com/docs/apis/content-management-api/#update-an-entry)

| **Parameter** | **Type** | **Description**                                                                 |
| :------------ | :------- | :------------------------------------------------------------------------------ |
| payload       | object   | Payload for the request.                                                        |
| [locale]      | string   | Passing the ‘locale’ parameter will localize the entry in the specified locale. |

**Example**

```js
extension.stack.ContentType('contenttype_uid').Entry('bltsomething123').update(
      {
      "entry": {
          "title": "example",
          "url": "/example"
      }
  }).then(...).catch(...);
```

##### Entry.Query() ⇒ [Query](#Query)

This static method instantiates the query module for entries. To see the list of methods that can be used for querying entries, refer to the [Query](#Query) module.

**Kind**: static method of [Entry](#Stack+ContentType+Entry)

**Example**

```js
let entryQuery = extension.stack.ContentType('content_type_uid').Entry.Query();
entryQuery.where("field_uid": "10").limit(10).skip(10).find().then(...).catch(...);
```

##### Entry.create(payload) ⇒ [Promise](#external_Promise)

This method creates a new entry.

**Kind**: static method of [Entry](#Stack+ContentType+Entry)

**Returns**: [Promise](#external_Promise) - Required data if resolved successfully

**Additional Resource:** Learn more about [Creating an Entry](https://www.contentstack.com/docs/apis/content-management-api/#create-a-an-entry| Create Entry).

| **Parameter** | **Type** | **Description**          |
| :------------ | :------- | :----------------------- |
| payload       | Object   | Data to create an entry. |

**Example**

```js
extension.stack.ContentType('content_type_uid').Entry.create({
    "entry": {
      "title": "example",
      "url": "/example"
    }
  }).then(...).catch(...);
```

### stack.Asset

**Kind**: instance class of [Stack](#Stack)

**See**: [Asset](https://www.contentstack.com/docs/apis/content-management-api/#assets)

[.Asset](#Stack+Asset)

-   [new this.Asset(uid)](#new_Stack+Asset_new)
-   _instance_
    -   [.only([key], values)](#Stack+Asset+only) ⇒ [Asset](#Stack+Asset)
    -   [.except([key], values)](#Stack+Asset+except) ⇒ [Asset](#Stack+Asset)
    -   [.environment(environment_uid)](#Stack+Asset+environment) ⇒ [Asset](#Stack+Asset)
    -   [.addParam(key, value)](#Stack+Asset+addParam) ⇒ [Asset](#Stack+Asset)
    -   [.addQuery(key, value)](#Stack+Asset+addQuery) ⇒ [Asset](#Stack+Asset)
    -   [.getReferences()](#Stack+Asset+getReferences) ⇒ [Promise](#external_Promise)
    -   [.delete()](#Stack+Asset+delete) ⇒ [Promise](#external_Promise)
    -   [.publish(payload, api_version="")](#Stack+Asset+publish) ⇒ [Promise](#external_Promise)
    -   [.unpublish(payload)](#Stack+Asset+unpublish) ⇒ [Promise](#external_Promise)
-   _static_
    -   [.Query()](#Stack+Asset.Query) ⇒ [Query](#Query)
    -   [.getRteAssets()](#Stack+Asset.getRteAssets) ⇒ [Promise](#external_Promise)
    -   [.getAssetsOfSpecificTypes(assetType)](#Stack+Asset.getAssetsOfSpecificTypes) ⇒ [Promise](#external_Promise)

#### new this.Asset(uid)

An initializer is responsible for creating an Asset object.

| **Parameter** | **Type** | **Description**   |
| :------------ | :------- | :---------------- |
| uid           | string   | UID of the asset. |

**Example**

```js
extension.stack.Asset("asset_uid");
```

#### asset.only([key], values) ⇒ [Asset](#Stack+Asset)

This method is used to show the selected fields of the assets in the result set.

**Kind**: instance method of [Asset](#Stack+Asset)

| **Parameter** | **Type** | **Default** | **Description**                               |
| :------------ | :------- | :---------- | :-------------------------------------------- |
| [key]         | String   | BASE        | Single field of an asset                      |
| values        | Array    |             | Array of fields to be shown in the result set |

**Example** _( Only with the field UID )_

```js
extension.stack.Asset("bltsomething123").only("title").fetch();
```

**Example** _( Only with the field UID )_

```js
extension.stack.Asset("bltsomething123").only("BASE", "title").fetch();
```

**Example** _( Only with the field UIDs(array) )_

```js
extension.stack.Asset("bltsomething123").only(["title", "description"]).fetch();
```

#### asset.except([key], values) ⇒ [Asset](#Stack+Asset)

This method is used to hide the selected fields of the assets in the result set.

**Kind**: instance method of [Asset](#Stack+Asset)

| **Parameter** | **Type** | **Default** | **Description**                                |
| :------------ | :------- | :---------- | :--------------------------------------------- |
| [key]         | String   | BASE        | Single field of an asset                       |
| values        | Array    |             | Array of fields to be hidden in the result set |

**Example** _( .Except with the field UID )_

```js
extension.stack.Asset("bltsomething123").except("title").fetch();
```

**Example** _( .Except with the field UID )_

```js
extension.stack.Asset("bltsomething123").except("BASE", "title").fetch();
```

**Example** _( .Except with the field UIDs(array) )_

```js
extension.stack
    .Asset("bltsomething123")
    .except(["title", "description"])
    .fetch();
```

#### asset.environment(environment_uid) ⇒ [Asset](#Stack+Asset)

This method is used to set the environment name of which you want to retrieve the data.

**Kind**: instance method of [Asset](#Stack+Asset)

| **Parameter**   | **Type** | **Description**         |
| :-------------- | :------- | :---------------------- |
| environment_uid | String   | UID/Name of environment |

**Example**

```js
extension.stack.Asset("bltsomething123").environment("development").fetch();
```

#### asset.addParam(key, value) ⇒ [Asset](#Stack+Asset)

This method includes a query parameter in your query.

**Kind**: instance method of [Asset](#Stack+Asset)

| **Parameter**   | **Type** | **Description**         |
| :-------------- | :------- | :---------------------- |
| environment_uid | String   | UID/Name of environment |

**Example**

```js
extension.stack.Asset("bltsomething123").environment("development").fetch();
```

#### asset.addQuery(key, value) ⇒ [Asset](#Stack+Asset)

This method includes a query parameter in your query.

**Kind**: instance method of [Asset](#Stack+Asset)

| **Parameter** | **Type** | **Description**        |
| :------------ | :------- | :--------------------- |
| key           | string   | Key of the parameter   |
| value         | string   | Value of the parameter |

**Example**

```js
extension.stack.Asset("uid").addQuery("key", "value").fetch().then().catch();
```

#### asset.getReferences() ⇒ [Promise](#external_Promise)

This method fetches the details of entries and the assets in which the specified asset is referenced.

**Kind**: instance method of [Asset](#Stack+Asset)

**Additional Resource**: Learn more about [Asset References](https://www.contentstack.com/docs/apis/content-management-api/#get-all-references-of-asset| Asset References).

**Example**

```js
extension.stack.Asset("uid").getReferences().then().catch();
```

#### asset.delete() ⇒ [Promise](#external_Promise)

This method deletes an existing asset.

**Kind**: instance method of [Asset](#Stack+Asset)

Additional Resource: Learn more about [Deleting an Asset](https://www.contentstack.com/docs/apis/content-management-api/#delete-an-asset| Delete Asset).

**Example**

```js
extension.stack.Asset("uid").delete().then().catch();
```

#### asset.publish(payload, api_version="") ⇒ [Promise](#external_Promise)

This method allows you to publish the asset either immediately or schedule the publish for a later date/time.

**Kind**: instance method of [Asset](#Stack+Asset)

| **Parameter** | **Type** | **Description**          |
| :------------ | :------- | :----------------------- |
| payload       | object   | Payload for the request. |
| api_version   | string   | Send api version in headers.(optional argument) |

**Example**

```js
extension.stack.Asset("bltsomething123").publish({
    asset: {
        locales: ["en-us"],
        environments: ["development"],
    },
    version: 1,
    scheduled_at: "2019-02-08T18:30:00.000Z",
});
```

#### asset.unpublish(payload) ⇒ [Promise](#external_Promise)

This method instantly unpublishes the asset and also gives you the provision to automatically unpublish the asset at a later date/time.

**Kind**: instance method of [Asset](#Stack+Asset)

| **Parameter** | **Type** | **Description**          |
| :------------ | :------- | :----------------------- |
| payload       | object   | Payload for the request. |

**Example**

```js
extension.stack.Asset("bltsomething123").unpublish({
    asset: {
        locales: ["en-us"],
        environments: ["development"],
    },
    version: 1,
    scheduled_at: "2019-02-08T18:30:00.000Z",
});
```

#### Asset.Query() ⇒ [Query](#Query)

This static method instantiates the query module for assets. To see the list of methods that can be used for querying assets, refer to the [Query](#Query) module.

**Kind**: static method of [Asset](#Stack+Asset)

**Example**

```js
let assetQuery = extension.stack.Asset.Query();
assetQuery.where("title": "main.js").limit(10).skip(10).find().then(...).catch(...);
```

#### Asset.getRteAssets() ⇒ [Promise](#external_Promise)

This static method retrieves comprehensive information on all assets uploaded through the Rich Text Editor field.

**Kind**: static method of [Asset](#Stack+Asset)

#### Asset.getAssetsOfSpecificTypes(assetType) ⇒ [Promise](#external_Promise)

This static method retrieves assets that are either image or video files, based on the request query.

**Kind**: static method of [Asset](#Stack+Asset)

| **Parameter** | **Type** | **Description**                                    |
| :------------ | :------- | :------------------------------------------------- |
| assetType     | String   | Type of asset to be received for e.g., ‘image/png’ |

### stack.getData() ⇒ Object

This method returns the data of the current stack.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - Returns stack data.

### stack.getContentType(uid, params) ⇒ Object

This API allows you to retrieve data of a content type of a stack using the [Content Type API](https://www.contentstack.com/docs/apis/content-management-api/#get-a-single-content-type) requests. This method returns a Promise object.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - A promise object which will be resolved with content type details.

| **Parameter** | **Type** | **Description**                      |
| :------------ | :------- | :----------------------------------- |
| uid           | string   | UID of the desired content type      |
| params        | Object   | Optional parameters for the GET call |

### stack.getContentTypes(query, params) ⇒ Object

This API allows you to retrieve data of a content type of a stack using the [Content Types API](https://www.contentstack.com/docs/apis/content-management-api/#get-all-content-types) requests. This method returns a Promise object.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - A promise object which will be resolved with details of the content type.

| **Parameter** | **Type** | **Description**                      |
| :------------ | :------- | :----------------------------------- |
| query         | Object   | Query for the GET call               |
| params        | Object   | Optional parameters for the GET call |

### stack.getEnvironment(name, params) ⇒ Object

This API allows you to retrieve environment details of a stack using the [Environment API](https://www.contentstack.com/docs/apis/content-management-api/#get-a-single-environment) requests. This method returns a Promise object.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - A promise object which will be resolved with environment details.

| **Parameter** | **Type** | **Description**                      |
| :------------ | :------- | :----------------------------------- |
| name          | string   | Name of the desired environment      |
| params        | Object   | Optional parameters for the GET call |

### stack.getEnvironments(query, params) ⇒ Object

This API allows you to retrieve details of environments of a stack using the [Environments API](https://www.contentstack.com/docs/apis/content-management-api/#get-all-environments) requests. This method returns a Promise object.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - A Promise object which will be resolved with details of the environments.

| **Parameter** | **Type** | **Description**                      |
| :------------ | :------- | :----------------------------------- |
| query         | Object   | Query for the GET call               |
| params        | Object   | Optional parameters for the GET call |

### stack.getLocale(code, params) ⇒ Object

This API allows you to retrieve a locale of a stack using the [Language API](https://www.contentstack.com/docs/apis/content-management-api/#get-a-language) requests. Method returns a Promise object.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - A promise object which will be resolved with locale details.

| **Parameter** | **Type** | **Description**                      |
| :------------ | :------- | :----------------------------------- |
| code          | string   | Code of the desired locale           |
| params        | Object   | Optional parameters for the GET call |

### stack.getLocales(query, params) ⇒ Object

This API allows you to retrieve the locales of a stack using the [Languages API](https://www.contentstack.com/docs/apis/content-management-api/#get-all-content-types) requests. Method returns a Promise object.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - A Promise object which will be resolved with details of the locales.

| **Parameter** | **Type** | **Description**                      |
| :------------ | :------- | :----------------------------------- |
| query         | Object   | Query for the GET call               |
| params        | Object   | Optional parameters for the GET call |

### stack.getReleases(query, params) ⇒ Object

This API allows you to retrieve details of releases of a stack using the [Releases API](https://www.contentstack.com/docs/developers/apis/content-management-api/#get-all-releases) requests. Method returns a Promise object.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - A Promise object which will be resolved with details of the releases.

| **Parameter** | **Type** | **Description**                      |
| :------------ | :------- | :----------------------------------- |
| query         | Object   | Query for the GET call               |
| params        | Object   | Optional parameters for the GET call |

### stack.getPublishes(query, params) ⇒ Object

This API allows you to retrieve details of publish queue of a stack using the [Publish Queue API](https://www.contentstack.com/docs/developers/apis/content-management-api/#get-publish-queue) requests. Method returns a Promise object.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - A Promise object which will be resolved with details of the publish queue.

| **Parameter** | **Type** | **Description**                      |
| :------------ | :------- | :----------------------------------- |
| query         | Object   | Query for the GET call               |
| params        | Object   | Optional parameters for the GET call |

### stack.search(queries: StackSearchQuery, apiKey?: string | null) => [Promise](#promise)\<Object\>

This API allows you to search for content in a stack. It uses the API used by the Contentstack Search Bar. By default, the current stack is searched. Method returns a Promise object.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - A Promise object which will be resolved with details of the stacks in the organization.

| **Parameter** | **Type** | **Description**                                         |
| :------------ | :------- | :------------------------------------------------------ |
| queries       | Object   | Queries for the GET call                                |
| apiKey        | string   | Optional parameters to search stack of specific API key |

#### StackSearchQuery

This is the type of the `queries` parameter of the `search` method.

```typescript
StackSearchQuery {
    type: "entries" | "assets";
    skip?: number;
    limit?: number;
    include_publish_details?: boolean;
    include_unpublished?: boolean;
    include_workflow?: boolean;
    include_fields?: boolean;
    include_rules?: boolean;
    include_title_field_uid?: boolean;
    query?: AnyObject;
    search?: string;
    save_recent_search?: boolean;
    desc?: string;
}
```

### stack.getAllStacks(options: {orgUid = "", params = {}}): [Promise](#promise)<Object[]>

This API allows you to retrieve details of all the stacks of an organization using the [Get all Stacks API](https://www.contentstack.com/docs/developers/apis/content-management-api/#get-all-stacks) requests. Method returns a Promise object.

> Note: In order to fetch the stacks, you must have access to the stacks.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - A Promise object which will be resolved with details of the stacks in the organization.

| **Parameter**  | **Type** | **Description**                               |
| :------------- | :------- | :-------------------------------------------- |
| options.orgUid | string   | Optional parameter to change the organization |
| options.params | Object   | Optional parameters for the GET call          |

### stack.getCurrentBranch() ⇒ [BranchDetail](#branchdetail) | null

This API allows you to retrieve details of current branch of the stack, if available. If the Branches plan is not available, it will return `null`.

> Note: Branch feature is plan based.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - An object which will be resolved with details of the current branch.

### stack.getAllBranches() ⇒ [BranchDetail](#branchdetail)[] | null

This API allows you to retrieve details of all the available branches in the stack. If the Branches plan is not available, it will return an empty array.

> Note: Branch feature is plan based.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Array - An Array of object which will be resolved with details of all the branches.

#### BranchDetail

This is the interface of the branch returned by the `stack.getCurrentBranch()` and `stack.getAllBranches()` methods.

```typescript
BranchDetail {
    api_key: string;
    uid: string;
    source: string;
    alias: {
        uid: string;
    }[];
}
```
### stack.getVariantById(variant_uid) ⇒ Object

This API allows you to retrieve data of a single entry variant of a stack using the [Variants API](#) requests. This method returns a Promise object.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - A promise object which will be resolved with variant entry details.

| **Parameter** | **Type** | **Description**                      |
| :------------ | :------- | :----------------------------------- |
| variant_uid   | string   | variant UID of desired entry |

### stack.getGlobalField(uid, params) ⇒ Object

This API allows you to retrieve data of a single global field of a stack using the [Global Field API](https://www.contentstack.com/docs/developers/apis/content-management-api#get-single-global-field) requests. This method returns a Promise object.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - A promise object which will be resolved with global field details.

| **Parameter** | **Type** | **Description**                      |
| :------------ | :------- | :----------------------------------- |
| uid           | string   | UID of the desired global field      |
| params        | Object   | Optional parameters for the GET call |

### stack.getGlobalFields(query, params) ⇒ Object

This API allows you to retrieve data of all global fields of a stack using the [Global Fields API](https://www.contentstack.com/docs/developers/apis/content-management-api#get-all-global-fields) requests. This method returns a Promise object.

**Kind**: instance method of [Stack](#Stack)

**Returns**: Object - A promise object which will be resolved with global field details.

| **Parameter** | **Type** | **Description**                      |
| :------------ | :------- | :----------------------------------- |
| query         | Object   | Query for the GET call               |
| params        | Object   | Optional parameters for the GET call |

## The Store Class used by an extension to store your data in [localStorage](#external_localStorage).

**Kind**: global class

-   [Store](#Store)
    -   [.get(key)](#Store+get) ⇒ [Promise](#external_Promise)
    -   [.getAll()](#Store+getAll) ⇒ [Promise](#external_Promise)
    -   [.set(key, value)](#Store+set) ⇒ [Promise](#external_Promise)
    -   [.remove(key)](#Store+remove) ⇒ [Promise](#external_Promise)
    -   [.clear()](#Store+clear) ⇒ [Promise](#external_Promise)

### store.get(key) ⇒ [Promise](#external_Promise)

It fetches the value of a key.

**Kind**: instance method of [Store](#Store)

| **Parameter** | **Type** | **Description**        |
| :------------ | :------- | :--------------------- |
| key           | string   | Key of the stored data |

**Example**

```js
extension.store.get("key").then((value) => console.log(value)); // will log value for the given key
```

### store.getAll() ⇒ [Promise](#external_Promise)

It fetches an object with all the stored key-value pairs.

**Kind**: instance method of [Store](#Store)

**Example**

```js
extension.store.getAll().then((obj) => obj);
```

### store.set(key, value) ⇒ [Promise](#external_Promise)

It sets the value of a key

**Kind**: instance method of [Store](#Store)

| **Parameter** | **Type** | **Description**         |
| :------------ | :------- | :---------------------- |
| key           | string   | Key of the stored data. |
| value         | \*       | Data to be stored.      |

**Example**

```js
extension.store.set("key", "value").then((success) => console.log(success)); // will log 'true' when value is set
```

### store.remove(key) ⇒ [Promise](#external_Promise)

It removes the value of a key.

**Kind**: instance method of [Store](#Store)

| **Parameter** | **Type** | **Description**                              |
| :------------ | :------- | :------------------------------------------- |
| key           | string   | Key of the data to be removed from the store |

**Example**

```js
extension.store.remove("key").then((success) => console.log(success)); // will log 'true' when value is removed
```

### store.clear() ⇒ [Promise](#external_Promise)

It clears all the stored data of an extension.

**Kind**: instance method of [Store](#Store)

**Example**

**Example**

```js
extension.store.clear().then((success) => console.log(success)); // will log 'true' when values are cleared
```

## Query

Creates an instance of the query

**Kind**: global class \
**Version**: 2.2.0

-   [Query](#Query)
    -   [.lessThan](#Query+lessThan) ⇒ [Query](#Query)
    -   [.lessThanOrEqualTo](#Query+lessThanOrEqualTo) ⇒ [Query](#Query)
    -   [.only([key], values)](#Query+only) ⇒ [Query](#Query)
    -   [.except([key], values)](#Query+except) ⇒ [Query](#Query)
    -   [.addQuery(key, value)](#Query+addQuery) ⇒ [Query](#Query)
    -   [.greaterThan(key, value)](#Query+greaterThan) ⇒ [Query](#Query)
    -   [.greaterThanOrEqualTo(key, value)](#Query+greaterThanOrEqualTo) ⇒ [Query](#Query)
    -   [.notEqualTo(key, value)](#Query+notEqualTo) ⇒ [Query](#Query)
    -   [.containedIn(key, value)](#Query+containedIn) ⇒ [Query](#Query)
    -   [.notContainedIn(key, value)](#Query+notContainedIn) ⇒ [Query](#Query)
    -   [.exists(key)](#Query+exists) ⇒ [Query](#Query)
    -   [.notExists(key)](#Query+notExists) ⇒ [Query](#Query)
    -   [.ascending(key)](#Query+ascending) ⇒ [Query](#Query)
    -   [.descending(key)](#Query+descending) ⇒ [Query](#Query)
    -   [.skip(skip)](#Query+skip) ⇒ [Query](#Query)
    -   [.limit(limit)](#Query+limit) ⇒ [Query](#Query)
    -   [.or(Array)](#Query+or) ⇒ [Query](#Query)
    -   [.and(Array)](#Query+and) ⇒ [Query](#Query)
    -   [.addParam(key, value)](#Query+addParam) ⇒ [Query](#Query)
    -   [.includeReference()](#Query+includeReference) ⇒ [Query](#Query)
    -   [.includeSchema()](#Query+includeSchema) ⇒ [Query](#Query)
    -   [.language(languageCode)](#Query+language) ⇒ [Query](#Query)
    -   [.includeContentType()](#Query+includeContentType) ⇒ [Query](#Query)
    -   [.includeOwner()](#Query+includeOwner) ⇒ [Query](#Query)
    -   [.environment(environment_uid)](#Query+environment) ⇒ [Query](#Query)
    -   [.equalTo(key, value)](#Query+equalTo) ⇒ [Query](#Query)
    -   [.count()](#Query+count) ⇒ [Query](#Query)
    -   [.query(query)](#Query+query) ⇒ [Query](#Query)
    -   [.tags(values)](#Query+tags) ⇒ [Query](#Query)
    -   [.includeCount()](#Query+includeCount) ⇒ [Query](#Query)
    -   [.getQuery()](#Query+getQuery) ⇒ [Query](#Query)
    -   [.regex(key, value, [options])](#Query+regex) ⇒ [Query](#Query)
    -   [.search(value)](#Query+search) ⇒ [Query](#Query)
    -   [.find()](#Query+find)
    -   [.findOne()](#Query+findOne)

### query.lessThan ⇒ [Query](#Query)

This method provides only the entries with values less than the specified value for a field.

**Kind**: instance property of [Query](#Query)

| **Parameter** | **Type** | **Description**                     |
| :------------ | :------- | :---------------------------------- |
| key           | String   | UID of the field                    |
| value         | \*       | The value used to match or compare. |

**Example**

```js
extension.stack.ContentType("blog").lessThan("created_at", "2015-06-22");
```

### query.lessThanOrEqualTo ⇒ [Query](#Query)

This method provides the entries with values less than or equal to the specified value for a field.

**Kind**: instance property of [Query](#Query)

| **Parameter** | **Type** | **Description**                    |
| :------------ | :------- | :--------------------------------- |
| key           | String   | UID of the field                   |
| value         | \*       | The value used to match or compare |

**Example**

```js
extension.stack
    .ContentType("blog")
    .lessThanOrEqualTo("created_at", "2015-03-12");
```

### query.only([key], values) ⇒ [Query](#Query)

This method is used to show the selected fields of an entry in the result set.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Default** | **Description**                               |
| :------------ | :------- | :---------- | :-------------------------------------------- |
| [key]         | String   | BASE        | Single field of an entry                      |
| values        | Array    |             | Array of fields to be shown in the result set |

**Example** _( Only with field UID )_

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry.Query()
    .only("title")
    .find();
```

**Example** _( Only with field UID )_

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry.Query()
    .only("BASE", "title")
    .find();
```

**Example** _( Only with field UIDs(array) )_

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry.Query()
    .only(["title", "description"])
    .find();
```

### query.except([key], values) ⇒ [Query](#Query)

This method is used to hide the selected fields of an entry in the result set.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Default** | **Description**                                |
| :------------ | :------- | :---------- | :--------------------------------------------- |
| [key]         | String   | BASE        | Single field of an entry                       |
| values        | Array    |             | Array of fields to be hidden in the result set |

**Example** _( Except with field uid )_

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry.Query()
    .except("title")
    .find();
```

**Example** _( Except with field uid )_

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry.Query()
    .except("BASE", "title")
    .find();
```

**Example** _( Except with field uids(array) )_

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry.Query()
    .except(["title", "description"])
    .find();
```

### query.addQuery(key, value) ⇒ [Query](#Query)

This method includes a query parameter in a query.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**        |
| :------------ | :------- | :--------------------- |
| key           | string   | Key of the parameter   |
| value         | string   | Value of the parameter |

**Example**

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry.Query()
    .addQuery("key", "value")
    .find()
    .then()
    .catch();
```

### query.greaterThan(key, value) ⇒ [Query](#Query)

This method provides the entries with values greater than the specified value for a field.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                    |
| :------------ | :------- | :--------------------------------- |
| key           | String   | UID of the field                   |
| value         | \*       | The value used to match or compare |

**Example**

```js
extension.stack.ContentType("blog").greaterThan("created_at", "2015-03-12");
```

### query.greaterThanOrEqualTo(key, value) ⇒ [Query](#Query)

This method provides the entries with values greater than or equal to the specified value for a field.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                    |
| :------------ | :------- | :--------------------------------- |
| key           | String   | UID of the field                   |
| value         | \*       | The value used to match or compare |

**Example**

```js
extension.stack
    .ContentType("blog")
    .greaterThanOrEqualTo("created_at", "2015-06-22");
```

### query.notEqualTo(key, value) ⇒ [Query](#Query)

This method provides the entries with values not equal to the specified value for a field.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                    |
| :------------ | :------- | :--------------------------------- |
| key           | String   | UID of the field                   |
| value         | \*       | The value used to match or compare |

**Example**

```js
extension.stack.ContentType("blog").notEqualTo("title", "Demo");
```

### query.containedIn(key, value) ⇒ [Query](#Query)

This method provides the entries with values matching the specified values for a field.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                                            |
| :------------ | :------- | :--------------------------------------------------------- |
| key           | String   | UID of the field                                           |
| value         | \*       | An array of values that are to be used to match or compare |

**Example**

```js
extension.stack.ContentType("blog").containedIn("title", ["Demo", "Welcome"]);
```

### query.notContainedIn(key, value) ⇒ [Query](#Query)

This method provides the entries that do not contain values matching the specified values for a field.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                                            |
| :------------ | :------- | :--------------------------------------------------------- |
| key           | String   | UID of the field                                           |
| value         | Array    | An array of values that are to be used to match or compare |

**Example**

```js
extension.stack
    .ContentType("blog")
    .notContainedIn("title", ["Demo", "Welcome"]);
```

### query.exists(key) ⇒ [Query](#Query)

This method provides the entries that contain the field matching the specified field UID.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**      |
| :------------ | :------- | :------------------- |
| key           | String   | The UID of the field |

**Example**

```js
extension.stack.ContentType("blog").exists("featured");
```

### query.notExists(key) ⇒ [Query](#Query)

This method provides the entries that do not contain the field matching the specified field UID.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**      |
| :------------ | :------- | :------------------- |
| key           | String   | The UID of the field |

**Example**

```js
extension.stack.ContentType("blog").notExists("featured");
```

### query.ascending(key) ⇒ [Query](#Query)

This parameter sorts the entries in the ascending order on the basis of the value of the specified field.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                   |
| :------------ | :------- | :-------------------------------- |
| key           | String   | Field UID to be used for sorting. |

**Example**

```js
extension.stack.ContentType("blog").ascending("created_at");
```

### **query.descending(key) ⇒ [Query](#Query)**

This method sorts the entries in the descending order on the basis of the specified field.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                  |
| :------------ | :------- | :------------------------------- |
| key           | String   | Field UID to be used for sorting |

**Example**

```js
extension.stack.ContentType("blog").descending("created_at");
```

### **query.skip(skip) ⇒ [Query](#Query)**

This method skips the specified number of entries.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                 |
| :------------ | :------- | :------------------------------ |
| skip          | Number   | Number of entries to be skipped |

**Example**

```js
extension.stack.ContentType("blog").skip(5);
```

### **query.limit(limit) ⇒ [Query](#Query)**

This method limits the response by providing only the specified number of entries.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                                         |
| :------------ | :------- | :------------------------------------------------------ |
| limit         | Number   | Maximum number of entries to be returned in the result. |

**Example**

```js
extension.stack.ContentType("blog").limit(10);
```

### **query.or(Array) ⇒ [Query](#Query)**

This method performs the OR operation on the specified query objects and provides only the matching entries.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                                             |
| :------------ | :------- | :---------------------------------------------------------- |
| Array         | object   | of query objects/raw queries to be taken into consideration |

**Example** _( OR with query instances)_

```js
let Query1 = extension.stack
    .ContentType("blog")
    .Entry.Query()
    .where("title", "Demo");
let Query2 = extension.stack
    .ContentType("blog")
    .Entry.Query()
    .lessThan("comments", 10);
let blogQuery = extension.stack.ContentType("blog").or(Query1, Query2);
```

**Example** _( OR with query instances)_

```js
let Query1 = extension.stack
    .ContentType("blog")
    .Entry.Query()
    .where("title", "Demo")
    .getQuery();
let Query2 = extension.stack
    .ContentType("blog")
    .Entry.Query()
    .lessThan("comments", 10)
    .getQuery();
let blogQuery = extension.stack.ContentType("blog").or(Query1, Query2);
```

### **query.and(Array) ⇒ [Query](#Query)**

This method performs the AND operation on the specified query objects and provides only the matching entries.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                                             |
| :------------ | :------- | :---------------------------------------------------------- |
| Array         | object   | of query objects/raw queries to be taken into consideration |

**Example** _( AND with raw queries)_

```js
let Query1 = extension.stack
    .ContentType("blog")
    .Entry.Query()
    .where("title", "Demo");
let Query2 = extension.stack
    .ContentType("blog")
    .Entry.Query()
    .lessThan("comments", 10);
let blogQuery = extension.stack.ContentType("blog").and(Query1, Query2);
```

**Example** _( .and with raw queries)_

```js
let Query1 = extension.stack
    .ContentType("blog")
    .Entry.Query()
    .where("title", "Demo")
    .getQuery();
let Query2 = extension.stack
    .ContentType("blog")
    .Entry.Query()
    .lessThan("comments", 10)
    .getQuery();
let blogQuery = extension.stack.ContentType("blog").and(Query1, Query2);
```

### **query.addParam(key, value) ⇒ [Query](#Query)**

This method includes a query parameter in a query.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**        |
| :------------ | :------- | :--------------------- |
| key           | string   | Key of the parameter   |
| value         | string   | Value of the parameter |

**Example**

```js
extension.stack
    .ContentType("content_type_uid")
    .Entry.Query()
    .addParam("key", "value")
    .find()
    .then()
    .catch();
```

### **query.includeReference() ⇒ [Query](#Query)**

This method is used to include the referenced entries from other content types.

**Note**: This method is only valid for querying [Entry](#Stack+ContentType+Entry).

**Kind**: instance method of [Query](#Query)

**Example** _( .includeReference with reference_field_uids as array )_

```js
stack
    .ContentType("contenttype_uid")
    .Entry.Query()
    .includeReference(["category", "author"])
    .find();
```

**Example** _( .includeReference with reference_field_uids )_

```js
stack
    .ContentType("contenttype_uid")
    .Entry.Query()
    .includeReference("category", "author")
    .find();
```

### **query.includeSchema() ⇒ [Query](#Query)**

This method is used to include the schema of the current content type in the result set along with the entry/entries.

**Note**: This method is only valid for querying [Entry](#Stack+ContentType+Entry).

**Kind**: instance method of [Query](#Query)

**Example**

```js
extension.stack
    .ContentType("contenttype_uid")
    .Entry.Query()
    .includeSchema()
    .find();
```

### **query.language(languageCode) ⇒ [Query](#Query)**

This method is used to set the language code of which you want to retrieve the data.

**Note**: This method is only valid for querying [Entry](#Stack+ContentType+Entry).

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                                     |
| :------------ | :------- | :-------------------------------------------------- |
| languageCode  | String   | Language code, for e.g. ‘en-us’, ‘ja-jp’, and so on |

**Example**

```js
extension.stack
    .ContentType("contenttype_uid")
    .Entry.Query()
    .language("en-us")
    .find();
```

### **query.includeContentType() ⇒ [Query](#Query)**

This method is used to include the current content type in the result set along with the entry(ies).

**Note**: This method is only valid for querying [Entry](#Stack+ContentType+Entry).

**Kind**: instance method of [Query](#Query)

**Example**

```js
extension.stack
    .ContentType("contenttype_uid")
    .Entry.Query()
    .includeContentType()
    .find();
```

### **query.includeOwner() ⇒ [Query](#Query)**

This method is used to include the owner of the entry(ies) in the result set.

**Note**: This method is only valid for querying [Entry](#Stack+ContentType+Entry).

**Kind**: instance method of [Query](#Query)

**Example**

```js
extension.stack
    .ContentType("contenttype_uid")
    .Entry.Query()
    .includeOwner()
    .find();
```

### **query.environment(environment_uid) ⇒ [Query](#Query)**

This method is used to set the environment name of which you want to retrieve the data.

**Kind**: instance method of [Query](#Query)

| Param           | Type   | Description              |
| :-------------- | :----- | :----------------------- |
| environment_uid | String | UID/Name of environment. |

**Example**

```js
extension.stack
    .ContentType("contenttype_uid")
    .Entry.Query()
    .environment("development")
    .find();
```

### **query.equalTo(key, value) ⇒ [Query](#Query)**

This method provides only the entries containing field values that match the specified condition.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                    |
| :------------ | :------- | :--------------------------------- |
| key           | String   | UID of the field                   |
| value         | \*       | The value used to match or compare |

**Example**

```js
extension.stack.ContentType("blog").where("title", "Demo");
```

### **query.count() ⇒ [Query](#Query)**

This method provides only the number of entries that match the specified filters.

**Kind**: instance method of [Query](#Query)

**Example**

```js
extension.stack.ContentType("blog").count();
```

### **query.query(query) ⇒ [Query](#Query)**

This method is used to set raw queries on the Query instance.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                                             |
| :------------ | :------- | :---------------------------------------------------------- |
| query         | object   | Raw {json} queries to filter the entries in the result set. |

### **query.tags(values) ⇒ [Query](#Query)**

The 'tags' parameter allows you to specify an array of tags to search for objects.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description** |
| :------------ | :------- | :-------------- |
| values        | Array    | Tags            |

**Example**

```js
extension.stack.ContentType("blog").tags(["technology", "business"]);
```

### **query.includeCount() ⇒ [Query](#Query)**

This method also includes the total number of entries returned in the response.

**Kind**: instance method of [Query](#Query)

**Example**

```js
extension.stack.ContentType("blog").includeCount();
```

### **query.getQuery() ⇒ [Query](#Query)**

This method provides raw{json} queries based on the filters applied on the Query object.

**Kind**: instance method of [Query](#Query)

**Summary**: returns Returns the raw query which can be used for further calls (.and/.or).

**Example**

```js
extension.stack.ContentType("blog").where("title", "Demo").getQuery();
```

### **query.regex(key, value, [options]) ⇒ [Query](#Query)**

This method provides only the entries that match the regular expression for the specified field.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                       |
| :------------ | :------- | :------------------------------------ |
| key           | String   | UID of the field                      |
| value         | \*       | The value used to match or compare    |
| [options]     | String   | Match or compare a value in the entry |

**Example** _( .regex without options)_

```js
let blogQuery = extension.stack.ContentType('blog').regex('title','^Demo') |
```

**Example** _( .regex with options)_

```js
let blogQuery = extension.stack.ContentType('blog').regex('title','^Demo', 'i') |
```

### **query.search(value) ⇒ [Query](#Query)**

This method is used to search data in entries.

**Kind**: instance method of [Query](#Query)

| **Parameter** | **Type** | **Description**                 |
| :------------ | :------- | :------------------------------ |
| value         | string   | Value to search in the entries. |

**Example**

```js
extension.stack.ContentType("blog").search("Welcome to demo");
```

### **query.find()**

This method provides all the entries which satisfy the specified query.

**Kind**: instance method of [Query](#Query)

**Example**

```js
let blogQuery = extension.stack.ContentType('blog').find() |
```

### **query.findOne()**

This method provides a single entry from the resultant set.

**Kind**: instance method of [Query](#Query)

**Example**:

```js
let blogQuery = extension.stack.ContentType('blog').findOne() |
```

## **Promise**

The **Promise** object represents the eventual completion (or failure) of an asynchronous operation and its resulting value.

**Kind**: global external

**See**: [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

## **localStorage**

**localStorage** is the read-only property that allows you to access a Storage object for the Document’s origin. The data that gets stored is saved across browser sessions.

**Kind**: global external

**See**: [localStorage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)

## JSON RTE Plugins

In this document, we will discuss the API requests that a JSON RTE Plugin can use to communicate with Contentstack.

**Prerequisites**

-   Basic understanding of [JSON Rich Text Editor](https://www.contentstack.com/docs/developers/json-rich-text-editor/about-json-rich-text-editor)
-   [JSON structure](https://www.contentstack.com/docs/developers/json-rich-text-editor/schema-of-json-rich-text-editor) and terminology associated with it

### Structure of JSON Rich Text Editor

```json
{
    "type": "doc",
    "children": [
        {
            "type": "p",
            "children": [
                {
                    "text": "Paragraph"
                }
            ]
        },
        {
            "type": "h1",
            "children": [
                {
                    "text": "Heading One"
                }
            ]
        }
    ]
}
```

In the JSON Rich Text Editor, the JSON structure is represented as a **Node** which consists of two types:

1. Block Node
2. Leaf Node

The editor content that is inside a Node of type doc acts as a root for the content. Where a Block node is a JSON structure with a “children” key in it. Whereas a Leaf node will just have “text” which may include formatting properties (mark) such as “bold”, “italic”, etc.

**Mark:** You'll see a mark used in below example which is nothing but a leaf property on how to render content.

For example:

```json
{
    "text": "I am Bold",
    "bold": true
}
```

Here, bold is the mark or the formatting to be applied to the "I am Bold" text.

![Block Leaf Image](./images/BlockLeaf.png "Block Leaf")

#### Render Type

A Block node can be rendered in three different ways as follow:

1. Block
2. Inline
3. Void

![Block Types](./images/BlockTypes.png "Block Types")

#### Path

Path arrays are a list of indexes that describe a node's exact position.

![Path](./images/Path.png "Path")

In the JSON Rich Text Editor, a Path has the following structure:

```ts
Number[]
```

For example, the path for doc is [0], paragraph is [0,0] from the above given example.

#### Point

Point objects refer to a specific location of text in the leaf node. Its path refers to the location of the node in the tree, and its offset refers to distance into the node's string of text.

![Point](./images/Point.png "Point")

In the JSON Rich Text Editor, a Point has the following structure:

```ts
{
   path: Path,
   offset: Number
}
```

#### Range

A Range is a set of `start (anchor)` and `end (focus)` points that specify a range in a JSON document.

png

The structure of a Range is as follows:

```ts
{
   anchor: Point,
   focus: Point
}
```

#### Location

Location is one of the ways to specify the location in a JSON document. It could be a Path, Point, or Range.

### Inclusion in your project

For JSON RTE Plugins, you will need to install `@contentstack/app-sdk` in your React project. You will also need to clone the [boilerplate](https://github.com/contentstack/rte-plugin-boilerplate) GitHub repository that contains the template needed to create a JSON RTE plugin.

### Classes

#### RTEPlugin(plugin_id, callback)

This method allows you to create a JSON RTE plugin instance.

**Kind:** Instance property of the JSON RTE plugin

**Returns:** Object - Plugin Object

| **Parameter**    | **Type**                    | **Description**                                                                                                                                                                |
| ---------------- | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `plugin_id`      | string                      | Unique ID of the plugin                                                                                                                                                        |
| `configCallback` | (rte: IRteParam) => IConfig | This function receives an [RTE instance](#rte) as argument and it expects you to return a [config object](#config-object) that includes details like title, icon, render, etc. |

<span id='config-object'></span>

##### configCallback: (rte: IRteParam) => IConfig

**IConfig:** This user-defined object will have all the essential metadata for the plugin.

The following table contains the possible properties of IConfig:

| **Key**       | **Type**                           | **Description**                                                                           |
| ------------- | ---------------------------------- | ----------------------------------------------------------------------------------------- |
| `title`       | string                             | Identifier for the toolbar button                                                         |
| `icon`        | ReactNode                          | Icon which will be used for buttons                                                       |
| `display`     | (‘toolbar’ \| ‘hoveringToolbar’)[] | Location of the plugin                                                                    |
| `elementType` | (‘inline’ \| ‘void’ \| ‘block’)[]  | Render type                                                                               |
| `render`      | ReactNode                          | Component to be rendered within the editor when corresponding plugin_uid appears in json. |

**rte:** An instance which has all the essential functions required to interact with the JSON RTE.

Following are a list of helpful properties and methods of the JSON RTE instance.

###### Properties:

**rte.ref:** Returns the HTML reference of the JSON RTE.

**rte.fieldConfig:** Contains metadata about the JSON RTE field which can be specified in the [content type builder page](https://www.contentstack.com/docs/developers/json-rich-text-editor-plugins/use-json-rte-plugins-in-content-types).

| **Key**          | **Description**                                                               | **Type**                         |
| ---------------- | ----------------------------------------------------------------------------- | -------------------------------- |
| `rich_text_type` | Type of JSON RTE                                                              | 'basic' \| 'advance' \| 'custom' |
| `reference_to`   | List of content type uid used in JSON RTE references.                         | string[]                         |
| `options`        | Array of selected toolbar buttons ( available if rich_text_type is ‘custom’ ) | string[]                         |
| `title`          | Title of the field                                                            | string                           |
| `uid`            | Unique ID for the field                                                       | string                           |

###### rte.getConfig: () => Object

Provides configuration which is defined while creating the plugin or while selecting a plugin in the content type builder page.

For example, if your plugin requires API Key or any other config parameters, then you can specify these configurations while creating a new plugin or you can specify field specific configurations from the content type builder page while selecting the plugin. These configurations can be accessed through the `getConfig()` method.

###### RTE Methods

These methods are part of the RTE instance and can be accessed as `rte.methodName()`.

| **Method**         | **Description**                                                                                                                                                                                                 | **Type**                                                                                                                         |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| `getPath`          | Retrieves the path of the node                                                                                                                                                                                  | (node:Node) => Path                                                                                                              |
| `setAttrs`         | Sets attributes for the node. <br/> For Eg: These attributes can be <br/> href for anchor plugin <br/>width, src for image plugin.                                                                              | (attrs:Object, options:Option) => void </br> Option: [NodeOptions](#node-options)                                                |
| `isNodeOfType`     | Retrieves a boolean value, whether the node at the current selection is of input type                                                                                                                           | (type:string) => boolean                                                                                                         |
| `getNode`          | Retrieves node at given location                                                                                                                                                                                | (location:Location) => Node                                                                                                      |
| `getNodes`         | Retrieves a [generator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Generator) of nodes which includes the location specified in options `at` default at current selection | (options:Option) => Node[] </br> Option: [NodeOptions](#node-options)                                                            |
| `string`           | String value of JSON in given path                                                                                                                                                                              | (location:Location) => string                                                                                                    |
| `addMark`          | Adds mark to the text                                                                                                                                                                                           | (key:string, val:any) => void                                                                                                    |
| `removeMark`       | Removes mark from the text                                                                                                                                                                                      | (key:string) => void                                                                                                             |
| `hasMark`          | Checks if the selected text has a mark                                                                                                                                                                          | (key:string) => boolean                                                                                                          |
| `insertText`       | Inserts text at a given location                                                                                                                                                                                | (text:string, location: Location) => void                                                                                        |
| `getText`          | Gets text from a given location                                                                                                                                                                                 | () => string                                                                                                                     |
| `deleteText`       | Deletes text from selected range                                                                                                                                                                                | () => void                                                                                                                       |
| `updateNode`       | Updates nodes based on provided options                                                                                                                                                                         | (type:string,attrs:Object, options: Option) => void </br> Option: [NodeOptions](#node-options)                                   |
| `unsetNode`        | Converts a node to a normal paragraph based on provided options                                                                                                                                                 | (options: Option) => void </br> Option: [NodeOptions](#node-options)                                                             |
| `insertNode`       | Inserts a node at a given location. Having `select` option true will select the node after insertion                                                                                                            | (node:Node, options?: Option) => void </br> Option: [NodeOptions](#node-options)` & { select?: boolean }`                        |
| `deleteNode`       | Deletes a node at a given location                                                                                                                                                                              | (options: Option) => void </br> Option: `{at?: Location, distance?: number, unit?: 'character' \| 'word' \| 'line' \| 'block'} ` |
| `wrapNode`         | Wraps node based on provided options with given node                                                                                                                                                            | (node:Node, options: Option) => void </br> Option: [NodeOptions](#node-options)                                                  |
| `unWrapNode`       | Unwraps node based on provided options from the parent node                                                                                                                                                     | (options: Option) => void </br> Option: [NodeOptions](#node-options)                                                             |
| `mergeNodes`       | Merges nodes based on provided options                                                                                                                                                                          | (options: Option) => void </br> Option: [NodeOptions](#node-options)                                                             |
| `getEmbeddedItems` | Gets details of embedded items JSON RTE.                                                                                                                                                                        | () => Object                                                                                                                     |
| `getVariable`      | Gets local variable                                                                                                                                                                                             | (name: string) => any                                                                                                            |
| `setVariable`      | Sets a local variable                                                                                                                                                                                           | (name: string, val:any) => void                                                                                                  |

**rte.selection:** Contains a set of methods to perform selection related tasks.

| **Function**   | **Description**                                                            | **Type**                                                                                                                                             |
| -------------- | -------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| `get`          | Retrieves current selection                                                | () => Range                                                                                                                                          |
| `set`          | Sets the selection to a given location                                     | (location: Location) => void                                                                                                                         |
| `isSelected`   | It is a React hook which returns `true` when the current node is selected. | () => boolean                                                                                                                                        |
| `isFocused`    | It is React hook which returns `true` when the current node is focused     | () => boolean                                                                                                                                        |
| `getEnd`       | Retrieves the end location of the editor                                   | () => Path                                                                                                                                           |
| `before`       | Retrieves the prior location before current selection                      | (location: Location, options: Option) => Location </br> Option: `{distance?: number, unit?: 'offset' \| 'character' \| 'word' \| 'line' \| 'block'}` |
| `after`        | Retrieves the subsequent location after current selection                  | (location: Location, options: Option) => Location </br> Option: `{distance?: number, unit?: 'offset' \| 'character' \| 'word' \| 'line' \| 'block'}` |
| `isPointEqual` | Checks if two points are equal                                             | (point1: Point, point2: Point) => boolean                                                                                                            |

<span id='node-options' />

###### Node Options:

Functions that involve transformation or change have an options parameter which includes options specific to transform and general `NodeOptions` to specify which nodes in the document you can apply the transform function.

```ts
interface NodeOptions {
    at?: Location;
    match?: (node: Node, path: Location) => boolean;
}
```

-   The `at `option selects a `Location `in the editor. It defaults to the user's current selection.
-   The `match `option filters the set of nodes with a custom function.

###### Events function:

These are accessible methods of the RTE instance. Can be invoked like `rte.{{event_name}}()`
| **Function** | **Description** | **Arguments** |
| ------------ | ------------------------------ | ------------- |
| `isFocused` | Check if the editor is focused | () => boolean |
| `focus` | Focuses the editor | () => boolean |
| `blur` | Blurs the editor | () => boolean |

#### Plugin:

##### Editor Events

###### `Plugin.on: (event_type, callback) => void`

| **event_type**   | **Description**                                                                | **Callback Arguments**                                                                                    |
| ---------------- | ------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- |
| `keydown`        | When keydown is performed                                                      | ({event: KeyboardEvent, rte: RTE}) => void                                                                |
| `exec`           | When a button is clicked or triggered                                          | (rte: RTE) => void                                                                                        |
| `deleteBackward` | When backward deletion occurs                                                  | ({rte: RTE, preventDefault: Function, ...args:[unit:"character" \| "word" \| "line" \| "block"]}) => void |
| `deleteForward`  | When forward deletion occurs                                                   | ({rte: RTE, preventDefault: Function, ...args:[unit:"character" \| "word" \| "line" \| "block"]}) => void |
| `normalize`      | It is used to normalize any dirty ( unwanted structure ) objects in the editor | ({rte: RTE, preventDefault: Function, ...args:[[node:Node, path:Path]]}) => void                          |
| `insertText`     | Inserts text in the current selection                                          | ({rte: RTE, preventDefault: Function, ...args:[string]}) => void                                          |
| `change`         | When there is a change in the editor                                           | ({rte: RTE, , preventDefault: Function}) => void                                                          |
| `insertBreak`    | When the enter key is pressed                                                  | ({rte: RTE, preventDefault: Function}) => void                                                            |

##### Dropdown plugin

###### Plugin.addPlugins: (...Plugin) => void

The addPlugins method can help you group the plugins under a dropdown that shares the same theme. Also, the `addPlugins `method takes a list of plugins as input.

For example, the code for `addPlugins `is as follows:

```ts
const ChooseAsset = RTE("choose-asset", () => {
    /** Choose Asset Code   */
});
const UploadAsset = RTE("upload-asset", () => {
    /** Upload Asset Code   */
});
const Asset = RTE("asset-picker", () => {
    /** Asset Picker Code */
});
Asset.addPlugins(ChooseAsset, UploadAsset);
```

The output for a dropdown plugin in UI is as follows:

<img src="./images/Dropdown.jpg" width='350' style="text-align:center" />

## Metadata

Metadata is a piece of information that lets you describe or classify an asset/entry. You can manage your digital entities effectively and enable improved accessibility with additional metadata. This object manages all the CRUD operations you can perform with metadata, e.g., adding, updating, or deleting additional metadata.

> Note: The Metadata feature allows users to update their asset metadata or entry metadata without incrementing the asset or entry version.

### createMetadata(metadataConfig: IMetadataCreate)

```ts
IMetadataCreate {
    entity_uid: string;
    type?: "asset" | "entry"; // default: "asset"
    _content_type_uid?: string;
    locale?: string;
    [key: string]: any; // other fields you want to add
}
```

This method adds new metadata for an asset or entry. It accepts metadata configuration as required arguments. This config contains basic details that you need to identify the metadata object and other data you need for your app.

### retrieveMetadata(metadataConfig: IMetadataRetrieve)

```ts
IMetadateRetrieve {
    uid: string;
}
```

This method retrieves metadata for an asset or entry. It accepts metadata configuration as required arguments. This config contains basic details that you need to identify the metadata object you want to retrieve.

### retrieveAllMetaData(metadataConfig: AnyObject)

You can use Get all Metadata to fetch the metadata information of all the entries/assets present in your stack.

### updateMetadata(metadataConfig: IMetadataUpdate)

```ts
IMetadataUpdate {
    uid: string;
    [key: string]: any; // other fields you want to update
}
```

This method updates existing metadata for an asset or entry. It accepts metadata configuration as required arguments. This config contains basic details that you need to identify the metadata object and other data you want to update.

### deleteMetadata(metadataConfig: IMetadataDelete)

```ts
IMetadateDelete {
    uid: string;
}
```

This method deletes existing metadata for an asset or entry. It accepts metadata configuration as required arguments. This config contains basic details that you need to identify the metadata object you want to delete.

## Modal

The `Modal` class represents a modal dialog opened from the app within the Contentstack UI. This feature of the App SDK enables apps to open modal dialogues, providing an enhanced user experience.

> **Note**: Starting from v1.6.0 of the App SDK, modals now open to take the full screen by default, without any additional user action.

### `setBackgroundElement(element: HTMLElement)`

This method allows developers to specify a custom HTML element to be displayed in the background, in place of the app iframe when the modal is opened. By default, the App SDK automatically selects an element to be shown in the background. However, this method provides the flexibility to choose a different element if required.

**Example**

```javascript
// JavaScript
ContentstackAppSDK.init().then(async function (appSdk) {
    // Set the background element to be shown
    appSdk.modal.setBackgroundElement(element);
});
```
