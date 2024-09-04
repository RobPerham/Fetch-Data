<h1>Fetch Data</h1>
When building software, we often start with default behaviors and then modify them to improve performance.

We’ve learned that the default behavior of the Effect Hook is to call the effect function after every single render.

Next, we learned that we can pass an empty array as the second argument for useEffect() if we only want our effect to be called after the component’s first render.

In this exercise, we’ll learn to use the dependency array to further configure exactly when we want our effect to be called!

When our effect is responsible for fetching data from a server, we pay extra close attention to when our effect is called. Unnecessary round trips back and forth between our React components and the server can be costly in terms of:

Processing
Performance
Data usage for mobile users
API service fees
When the data that our components need to render doesn’t change, we can pass an empty dependency array so that the data is fetched after the first render. When the response is received from the server, we can use a state setter from the State Hook to store the data from the server’s response in our local component state for future renders. Using the State Hook and the Effect Hook together in this way is a powerful pattern that saves our components from unnecessarily fetching new data after every render!

An empty dependency array signals to the Effect Hook that our effect never needs to be re-run, that it doesn’t depend on anything. Specifying zero dependencies means that the result of running that effect won’t change and calling our effect once is enough.

A dependency array that is not empty signals to the Effect Hook that it can skip calling our effect after re-renders unless the value of one of the variables in our dependency array has changed. If the value of a dependency has changed, then the Effect Hook will call our effect again!

Here’s a nice example from the official React docs:

useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // Only re-run the effect if the value stored by count changes


1.
We’ve started building a weather planner app that will fetch data about the weather and allow our users to write some notes next to the forecast. A lot of good code has already been written, but there currently isn’t anything rendering to the screen.
Let’s read through the code and start to wrap our heads around what is going on here. What part of our code do we think is keeping the component from rendering?
In our JSX, we are trying to map over an array stored by the data state variable, but our effect that fetches this data doesn’t get called until after the first render. So during the first render, data is undefined and attempting to call map() on undefined is causing our error!
Let’s prevent this error by checking to see if the data has loaded yet. If it hasn’t, then we want our function component to just return a paragraph tag with the text “Loading…”. If the data is no longer undefined, then the data has been loaded, and we can go ahead and render the full JSX!

2.
Our data fetching is being done in our effect. Notice how we are currently just using alert() messages to keep track of requesting and receiving data from our server. Instead of just stringifying the response data and showing it in an alert message, let’s store that data in our state.
When the data has been fetched, use our state setter function to store that data in our component’s state!
Remember that we want to store an array in our data state variable, not the whole response object.

3.
Type in each of the notes’ input fields in our table. What do you notice? Why do you think this is happening?
Each time that we type in an input field, the component re-renders to show the new value of that field. Even though we don’t need any new data from the backend, our component is fetching new data after every render!
Use an empty dependency array to ensure that data is only fetched after our component’s first render.

4.
Wow, that small code change made a huge difference in the performance of our weather planner app!
Let’s make one more improvement. Did you notice the buttons at the top of our app? We want our users to be able to choose between planning around daily weather forecasts and weekly weather forecasts. Clicking on these buttons currently doesn’t change anything. Let’s fix that!
The server has two different endpoints called: /daily and /hourly. Let’s use the value of the forecastType state variable to determine which endpoint our effect should request data from.
After making this change, our effect behaves differently based on the value of forecastType. You could say that how we use our effect depends on it! Add this variable to our dependency array so that the effect is called again, updating data appropriately, after re-renders where the user has selected a different forecast type.
