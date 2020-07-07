# AndroidNoteKeeper Application

[Pluralsight Courses on Android Development] (https://app.pluralsight.com/library/courses/android-application-basics-understanding/table-of-contents)

### Activity
A single, focused thing that the user can do
They serve the place to present the UI
- provide a window
- UI is build with View-derived classes

Have a lifecycle
- More that just a screen
- Lifecycle calls a series of methods
- Use the onCreate method to initialise the activity

###### Activity UI
View
- Basic building block of UI
- Drawing and event handling
- Many specialised classes available

ViewGroup
- Special View that holds other views

Layout 
- Special invisible ViewGroup
- Handle View positioning behaviour
- Many specialised classes available

###### Layout Classes
Activity UIs need to be responsive
- Device display characteristics vary
- UI must adapt
- Absolute positioning would be limiting

Layout classes provide positioning flexibility
- Arrange child Views - Children can include other layout classes
- Specific positioning behaviour depends on the layout class
    - FrameLayout - Provides a blocked-out are, generally has only one direct child
    - ScrollView - provides a scrollable area
    - LinearLayout - Horizontal or Vertical arrangement, supports weighted distribution
    - RelativeLayout - Relative positioning, relative to one another or parent

###### ConstraintLayout Class
- Extremely flexible layout class
    - Often the only layout class needed
- First-class design-time experience 
    - Closely integrated with designer
    - Rarely need to resort to XML
- Children leverages constraints
    - Relative size/position
    - Ratio-base size/position
    - Group size/position distribution, ie. chains
    - Weighted relationships
    - Guideline-based size/position
- Should set horizontal and one vertical constraints:
    - Positions at 0,0 without constraints
    - Can set more that one of each
- Setting constraints with the designer
    - Drag circle at mid-line to relationship
- Setting fixed size with the designer
    - Drag corner squares

###### Creating the Activity UI
- Programmatically: 	Use Java code to create class instances, Relationships and properties set in code
- Layout files: XML files describe Views hierarchy, Usually created using UI Designer.

###### Activity/Layout Relationship
There’s no implicit relationship, 
- Activity must load layout - Use **setContentView**
- Activity must request View reference - use **findViewById**

> Activities present the UI
>
> Views are the basic building blocks
Layouts
- Handle positioning behaviour
- Important for creating responsive UI
ConstraintLayout class
- Powerful and flexible
- Closely integrated with Android Studio Designer
Activity/layout relationship
- No implicit relationship exists
- Must load layout
    * Use **setContentView**
- Must request layout View references
    * Use **findViewById**
R class provides important constants
- Layout resources
    * R.layout
- View Id’s
    * R.id

### Activity interaction 
Android is a component-oriented platform
- A number of different types, activities are the most familiar
Activities are distinct from one another
- One can’t directly create another activity, relies on intents to interact

###### Starting an Activity within your app
1. Create an intent
    * Identifies the desired Activity
    * Often can just be Activity class info
2. Call startActivity
    * Pass the intent
    * Launches Activity matching the intent

###### Describing Operations with Intents
Intents describe a desired operation	
* Often need more than just a target
* May need to provide additional info

Intent extras provide additional info
* Name value pairs
* Names & Values are operation-defined
* Added to Intent with putExtra overloads.

Accessing intent info
* Activity can access intent that started it
* Use getIntent
* Use **Intent.getXXEtra** to retrieve extras
    * Method names include return type
Intents must be cross-process friendly
* Limits allowable extras

Supported extra types
* Primitive types and String
* Arrays of supported types
* Some ArrayLists
* A few other special types

Most reference types not directly supported
* Require special handling.

Reference Types as Intent Extras
Reference types need to be ’flattened’ - converted to a bunch of bytes [wire friendly]
One option is Java Serialisation
* Supported but not preferred
* Serialisation is very runtime expensive

Use Parcelable API
* Much more efficient than serialisation
* Must be explicitly implemented

Implement Parcelable interface
* DescribeContents
    - Indicate special behaviours
    - Generally can return 0
* WriteToParcel
    - Receives a Parcel instance
    - Use Parcel.writeXX to store content

Provide public static final *CREATOR* field
* Is a **Parcelabel.Creator** implementation

Parcelabel.Creator interface 
* **createFromParcel**
    - Responsible to create new type instance
    - Receives a Parcel instance
    - User Parcel.readXXX to access content
* **newArray**
    - Receives a size
    - Responsible to create array of type

### Application Activity Relationship
Android is a component-oriented platform
* Components run within a process

###### Process lifetime
* Driven by component lifetime
* Launched for first component accessed
* Terminate after last component exits

###### Application process
* Each app has its own process
* App components run in same process
    - When simultaneously active

### Late-Binding Components
Intents describe a desired operation - identify operation target
Explicit intents
* Target is explicitly identified
* Specify the Activity class to use

Implicit intent 
* Target is implied
* Specify the Activity characteristics

Implicit intents provide late-binding
- Match is determined at runtime

System finds best match
- Often comes from another app - e.g. Gmail to send email activity from a contact activity
- Specific match may vary depending on the apps installed on the user device
- Prompts the user if tie

Decouples sender and receiver
- Sender may not know the receiver
- Receiver may not know the sender

###### Implicit Intent Characteristics
Action
- Action string identifies some kind of action
- Many standard constants available
- Example: **Intent.ACTION_VIEW** identifies the View action
- Commonly set in Intent constructor
- Only required characteristic

Category
- Provides extended qualification
- Not normally used by sender - more important to receivers of implicit intent

Data
- URI of the data to be worked on - e.g. https://pluralsight.com
- Set with **Intent.setData**

Mime type
- Common mime type or app-specific mime type - e.g. text/html
- Set with **Intent.setType**

Setting data and mime type
- Use **Intent.setDataAndType**
- setType and setData cancel each other


### Activities with Results
Some activity classes return results
Camera Activity
* Presents Camera functionality
* Returns image thumbnail
Contact Activity
* Presents contact UI
* Return Selected contact info
Started differently than other activities
* Use **StartActivityForResult**
Parameters passed to **startActivityForResult**
* Intent
* App defined integer identifier - Differentiates results within your app
Receiving results
* Override your activity’s **onActivityResult**
Parameters received by onActivityResult
* App defined integer identifier
* Result code - RESULT_OK indicates success
* Intent - contains activity results 


### Application Experience
Generally composed of multiple Activities
* Most probably come from your app
* Others may come from other apps
* Android needs to manage this flow
* Flow is managed as a task

### TASKS
A task is a collection of activities that users interact with when performing a certain job.
* Managed as a stack - known as the back stack
* Activities added going forward
* Back button removes Activities - Causes Activity to be destroyed

###### Managing persistent state
* Use edit-in-place model
* Changes saved with no special action
Saving changes
* Write to backing store when leaving
* Handle in onPause
Handling new entries
* New entires registered right away
* Handle in onCreate


### Activity Lifecycle
Common causes of Activity destruction 	
* Leaving with the back button
* Calling finish method 
* System initiated
    - Generally to reclaim resources
    - Prolonged period in the background
    - System experiencing resource pressure

###### Activity Lifecycle Methods
Lifetimes within activity lifecycle
* Total lifetime
* Visible lifetime
* Foreground lifetime

Activity lifecycle methods
* Methods for start/end of each lifecycle
* A few additional methods for transitions
￼

Activities provide state management
* Opportunities to save before destroy
* Saved state provided on restore

Saving State
* **onSaveInstanceState**
* Write Activity state to passed bundle
Restoring State
* **onCreate**
* Receives saved Bundle on restore
* Bundle is null on initial create
* Intent remains available on restore

###### Maintaining Activity State
- Writing to a persistent store is expensive
- Need a better solution for maintaining state across configuration changes

###### ViewModel
* Stores activity state in-process
* State stored separate from the activity
* Extend ViewModel class to customise
* Add properties and methods specific to your activity’s state requirements

###### ViewModelProvider
* Manages ViewModel instances
* Creates new instances when needed
* Retrieves existing when available

> ViewModel maintains state when activity is destroyed due to configuration changes like device rotation.
> In other instances both activity and ViewModel are destroyed, e.g.  moving to another activity.

In those scenarios we’ve to use the activity’s **onSaveInstanceState()** and write the info to a bundle the the system passes into our activity.
- When an activity is initially created, savedInstanceState is null
- When an activity is destroyed and recreated, savedInstanceState contains the saved state
- We only need to restore ViewModel state when it gets destroyed along with the activity  
