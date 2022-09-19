# Process
## Using Vue.js
- Because I could potentially be a member of the Ground Signal team, I thought it would be a good idea to use Vue.js. So far my experience with it has been great! This is my first time working with this front end framework and I really enjoyed learning and getting familiar with it.

## Creating an API key
- First I noticed the given code had an API key and thought that would be suffice. After testing to see if the key was still active, I decided to create my own API key through google cloud 
- I Restricted the key using the Maps Javascript API

## Add API key
- Added it to index.html

## Create a Map Component and Route
- Under the src folder, I created a new folder called pages
- Then I created a new component called Map.vue
- In the router folder, I created a route to the Map component

## Search bar and Autocomplete
- In the Map component, I created an input field
- When the user interacts with the input field, suggested locations populate a list for the user to see.
- When business is clicked on, result is set to that specific business

## Map
- Using google maps platform, the initial page the user is greeted with is a default set location.
- Once user selects a location from the Autocomplete list, the location is centered in the middle of the map with a marker on its exact location
- Clicking on the marker brings up the details modal

## Details Modal
- In the components folder, I created a new component for the Details Modal.
- This was done so this component can be reused when needed.
- When the user clicks the marker for the location selected a details modal appears.
- This modal will contain the data/details for a selected location if it is available

# Notes
## Map click handler
- Initial when I created the input field and managed to implement an auto complete feature, I had an outside click handler that would hide the autocomplete list.
- After implementing the Map, I noticed that this handler was no longer functional. I realized that when clicking outside of the input field, I was clicking the Map. Therefore, I added a map click handler that would hide the autocomplete list if it was open.

## Conditional Rendering Sample data
- I noticed that some of the objects in the sample-data.js file did not contain the same elements. (eg. Some objects did not contain the a website link). I then added some conditional rendering logic that would only display the elements in the modal if the existed. Otherwise, they would be left out.

## Implementing icon-pin.svg
- Initially, I replaced the list-style from discs to the icon-pin.svg (for the autocomplete list). Because of a sizing issue and for the sake of having something that works for now, I set the list-style to `none`. Then I inserted the svg as an img on the left hand side of the autocomplete options in the list.

## Being Unique
- One of the first questions I thought of was do I need to use the code given exactly how it is. When I read the directions, I saw at the end that I could be unique. Therefore, I used some of the files as references to implement my own instances.
- I changed the map zoom and the size of the icon-pin svg to be more visible on the map.

## Future improvements
- [] Create a separate component for the search bar
- [] Outside click handler