# Brief "Accessibility in Mobile" Guide
This brief guide is targeting on Android and iOS since together they make up over 80 % of the market share on smartphone sells (2017). Further, it's merely a pull-together of what's in the web somewhere but it should give you a general idea of what to look for when developing accessible mobile apps.

## General considerations

You should make your app accessible by default because (from Apple Developer Guideline):
- It increases your user base. You've worked hard to create a great application; don't miss the opportunity to make it available to even more users.

- It allows people to use your application without seeing the screen. Users with visual impairments can use your application with the help of a screen reader.

- It helps you address accessibility guidelines. Various governing bodies create guidelines for accessibility and making your smartphone application accessible to screen reader users can help you meet them.

- It's the right thing to do.
---
> ### @icon-info-circle A definition of accessibility
> An application is accessible when all user interface elements with which users can interact are accessible. A user interface element is accessible when it properly reports itself as an accessibility element. 
---
### High level steps to take as a developer 
- Label (interactive) UI elements
- **Group** content (container should not be in the accessibility tree)
- Make **navigation** easy to follow (by keyboard)
- Make **touch targets large**
- Provide adequate color **contrast**
- Use **visual cues** other than color
- Present accessible media content (i.e. CC)
- Check with **inverse color settings** turned on (white on black)
- Add **additional** accessibility **settings** to your app (large/different font, invert  colors, etc.)

## Platform specific implementations
The most important OS support you can adapt to on all platforms is the screen reader API. Both platforms use some sort of (markup) extensions for their accessibility APIs. 
> Note: If you use the default controls on Android or iOS you save yourself a lot of hassle. Native controls on both platforms are by default accessible. 


### Control extensions to be set:  
Controls both on Android and iOS have baked in properties which can be set to support screen readers. 
![Control overview in the example of iOS](iphoneax_axinfo_2x.png)

**Label**.  
A short, localized word or phrase that succinctly describes the control or view, but does not identify the element's type. Examples are "Add" or "Play."
A label should:   
- Briefly describe the element
- Begin with a capitalized word
- Not end with a period
- Be localized  

> @icon-exclamation-triangle Don't include the type of control or view. The screen reader will include it and will otherwise read "Add button button", which is annoying. 

**Traits** (only iOS). 
A combination of one or more individual traits, each of which describes a single aspect of an element's state, behavior, or usage. For example, an element that behaves like a keyboard key and that is currently selected can be characterized by the combination of the Keyboard Key and Selected traits.   

**Hint**.
A brief, localized phrase that describes the results of an action on an element. Examples are "Adds a title" or "Opens the shopping list."   
> Note: Screen reader users can choose whether to hear available hints by selecting an option in the screen readers settings on their device. Spoken hints are turned on by default.

**Frame**.
The frame of the element in screen coordinates, which is given by the CGRect structure that specifies an element's screen location and size.

**Value**. 
The current value of an element, when the value is not represented by the label. For example, the label for a slider might be "Speed," but its current value might be "50%."

---

### [iOS](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)

Intentionally left out since we don't have any native iOS developers

#### Testing on iOS
see [Apples Accessibility Inspector](https://developer.apple.com/library/content/technotes/TestingAccessibilityOfiOSApps/TestingtheAccessibilityofiOSApps/TestingtheAccessibilityofiOSApps.html)

---
### [Android](https://developer.android.com/guide/topics/ui/accessibility/apps)

#### Label (interactive) UI elements

For _static_ content add `attributes` to labels in the XML (i.e. `android:contentDescription`). For _dynamic_ content set the content within the logic of loading the dynamic content (using the `set..()` methods of the element, i.e. `setContentDescription()`). 

|UI element type | Attribute | Setter
|---- | ------------ | 
|ImageView, ImageButton    | `android:contentDescription` | `setContentDescription()`
|Decorative elements | `android:importantForAccessibility`*  | `isImportantForAccessibility()` 
|EditText   | `android:hint` | `setHint()`       
|View   | `android:labelFor`** | `setLabelFor()`    

\* Android 4.1+: set `android:contentDescription` to `@null`  
** Available only on Android 4.2+

#### **Group** content
Often it is easier to follow a screen reader when UI items are grouped. Otherwise the user has an endless long list of the visual representation. Therefor the `focusable` property of a container can be set to `true`, activating a screen reader to read out the `contentDescription`. 

```xml
<RelativeLayout
    android:id="@+id/song_data_container"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:focusable="true">         <------------ set focus to true

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/song_title"
        android:text="@string/song_title" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/singer"
        android:layout_toRightOf="@id/song_title"
        android:text="@string/singer" />
    ...
</RelativeLayout>
```

#### Testing on Android
see [Android Accessibility Scanner](https://developer.android.com/training/accessibility/testing)

---
### [Xamarin](https://docs.microsoft.com/en-us/xamarin/cross-platform/app-fundamentals/accessibility)
### [Xamarin.Android](https://docs.microsoft.com/en-us/xamarin/android/app-fundamentals/accessibility) 

Android provides a `ContentDescription` property that is used by screen reading APIs to provide an accessible description of the control's purpose.

The content description can be set in either C# or in the AXML layout file.
Code behind: 

```csharp
saveButton.ContentDescription = "Save data";
```
AXML:   
```xml
<ImageButton
    android:id=@+id/saveButton"
    android:src="@drawable/save_image"
    android:contentDescription="Save data" />
```
For the diffent UI elements use the following properties:  

|UI element type | Attribute | Setter
|---- | ------------ | 
|ImageView, ImageButton    | `android:contentDescription` | `setContentDescription()`
|EditText, TextView   | `android:hint`* | `setHint()`       
|View, input fields   | `android:labelFor`** | `setLabelFor()`  

\* `Hint` will only be used in case the value of `TextView` or `EditText` is not set. 

Setting the `LabelFor`:  
```csharp
EditText edit = FindViewById<EditText> (Resource.Id.editFirstName);
TextView tv = FindViewById<TextView> (Resource.Id.labelFirstName);
tv.LabelFor = Resource.Id.editFirstName;
```
AXML:  
```xml
<TextView
    android:id="@+id/labelFirstName"
    android:hint="Enter some text"
    android:labelFor="@+id/editFirstName" />
<EditText
    android:id="@+id/editFirstName"
    android:hint="Enter some text" />
```

> **Focus settings**  
> Accessible navigation relies on controls having focus to aid the user in understanding what operations are available. 

Android provides a `Focusable` property (`android:focusable`, `<android:focusable="false" />`) which can tag controls as specifically able to receive focus during navigation.

You can also control focus order with the `nextFocusDown`, `nextFocusLeft`, `nextFocusRight`, `nextFocusUp` attributes, typically set in the layout AXML. Use these attributes to ensure the user can navigate easily through the controls on the screen.

### [Xamarin.iOS](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/accessibility)

iOS provides the `AccessibilityLabel` and `AccessibilityHint` properties.  

|UI element type | Property
|---- | ------------ 
|Control    | `userControl.AccessibilityLabel = "Hello";`
| Control (important)|`usernameInput.Hint = "Press Enter to ...";`
|Disable accessibility   | `someUIElem.IsAccessibilityElement = "false";`

#### PostNotification
A speciality of the iOS Accessibility API are the `Announcement` and `LayoutChanged` of controls that can be subscribed to in order to support accessibility even further. An announcement could me made when the UI has changed after a callback...

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

... or the layout of the UI changed and the focus moved:
```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```

### [Xamarin Forms](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/accessibility/index)

> @icon-exclamation-triangle `AutomationProperties.IsInAccessibleTree` has to be set to `true` so the control is visible to screen readers. 

_Before:_  
```xml
<StackLayout Spacing="20" Padding="15">
    <Label  Text="Text" 
            FontSize="Medium" />
    <Entry  Text="{Binding Item.Text}" 
            FontSize="Small" />
    <Label  Text="Description" 
            FontSize="Medium" />
    <Editor Text="{Binding Item.Description}" 
            FontSize="Small" 
            Margin="0" />
</StackLayout>
```

_After:_  

```xml
<StackLayout Spacing="20" Padding="15">
    <Label Text="Enter item name" 
           FontSize="Medium" 
           x:Name="ItemName" />
    <Entry Text="{Binding Item.Text}" 
           FontSize="Small" 
           AutomationProperties.IsInAccessibleTree="True"
           AutomationProperties.LabeledBy="{Reference ItemName}" />
    <Label Text="Description" 
           FontSize="Medium"
           x:Name="DescriptionLabel"/>
    <Editor Text="{Binding Item.Description}" 
            AutomationProperties.IsInAccessibleTree="True"
            AutomationProperties.LabeledBy="{Reference DescriptionLabel}" 
            FontSize="Small" 
            Margin="0" />
</StackLayout>
```

--- 

## What next?
Get certified for your hard work and constant considerations: http://www.interactiveaccessibility.com/services/accessibility-certification