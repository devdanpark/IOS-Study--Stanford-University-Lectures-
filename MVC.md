# MVC
- All about managing communication between campss
## Model
- What your application is(but not how it is displayed)
- Model is (should be) UI independent
- The Model and View should never speak to each other
## Controller
- How your Model is presented to the user(UI logic)
- Controllers can always talk directly to their Model
- Controllers can also talk directly to their View
- Controllers interpret/format Model information for the View.
- Controllers(or other Model)"tune in" to interesting stuff
- 
## View
- Your Controller's minions
- Sometimes the View needs to synchronize with the Controller.
- View might "tune in," but probably not to a Model's "station"
- Controller can drop a 'target' on itself
- Then hand out an action to the view
- The View sends the action when things happen in the UI.
- The Controller sets itself as the View's delegate
- The Delegate is set via a protocol(i.e. it's "blind" to class)
- Views do not own the data they display
- Controllers are almost alwways that data souce(not Model)

