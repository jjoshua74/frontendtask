
Using a PNG image is not great for performance/size reason. Especially when the image doesn't need to be transparent. Image pixel size is very large, then shrunk using CSS. This is wasteful in terms of bandwidth.
- Created a jpg with the same colour background and reduced its size dramatically. 

Styling does not work well on mobile.
- should be fixed with bootstrap column stuff. 

Code has some empty lines, and some very long lines. Maybe a auto code formatter should be used?
- formatted. 

Locations are called 'regions' in the code
- refactored 

Not all areas from the API are selectable, eg. Etc, CET, WET
- Intentional. I reverted it

unncessary API call
- removed it 

Code to display the locations in the selected area will error if areas without locations are included
- fixed

Props should be define with types.
- fixed

TimeDisplay.vue using css class to hide/show time can be done with `v-show`. disableRegions: true should be removed.
- implemented `v-show`
- removed `disableRegions` variable. Used `locations.length <= 1` instead.

Unusual layout
- Completely redesigned. 
- Time is now displayed below the select boxes. 
- More visually appealing colour scheme.
- The time message now disappears when the user selects a new area until they select the corresponding location

CSS `#heading` has `Background-color`, which should be `background-color`.
- oops.
- UI redesigned no longer uses this code. 

`v-on` can be written as `@`
- refactored

`v-bind` can be written as `:`
- refactored 

De-duplicating the list of areas can be done much clearer and more efficiently.
- attempted to make this more efficient by populating an array with duplicates, converting that array into a set and then back into an array.

No API error handling (shown to user).
- now shown using alerts. 
- added package `sweetalert2` for this

Shouldn't log errors to console.
- removed these lines. Replaces them with alerts. 

Would be better to use async/await with axios calls.
- implemented asynchronous methods instead of .then() syntax

No loading state
- implemented a loading state
- a loading wheel appears whilst the programming is making API calls. 
