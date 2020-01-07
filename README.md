# Overview

In this assignment you will build an interactive learning exercise where a student will read a paragraph about a species, then proceed to identify all species of that type from a list. The list of identified species is sent to a server-side function to evaluate if the student's answer is correct.
    
  - [View Demo Video](https://onlea-takehome.surge.sh/prototype.mp4)


# Instructions

1. Fork this repository and create a new branch for your development work
1. Create your implementation following the [Specification](#Specification) below
1. Add instructions on how to run your implementation to the [Getting Started](#Getting-Started) section.
1. In the [follow up questions](#Follow-Up-Questions) section below, respond inline to each of the questions.
1. Commit your implementation and answers to your branch, then [create a pull request](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/creating-a-pull-request-from-a-fork) for review on the original repository.


**Guidelines:**
- Do not spend longer than one hour on your implementation, a perfect implementation is not required or expected. Please move on to the [follow up questions](#Follow-Up-Questions) after one hour.
- You may use any libraries or frameworks of your choosing (e.g. React, Vue, Angular, Web Components, etc.)
- The visual design of the components does not need to be exactly as seen in the prototype video. Feel free to use an existing component design framework (e.g. Material Design, Bootstrap, etc.) or design your own.


# Specification

A sample HTML template (`./src/index.html`) and some base styles (`./src/css/main.css`) have been provided.

Using these files as a starting point, you will build the following functionality:
  - [ ] Load the species available for selection via GET request from the provided endpoint (See [Exercise Data](#Exercise-Data))
  - [ ] Display all of the loaded species in a 3x3 grid
  - [ ] When a user clicks on one of the species, toggle a visual "selected" state to indicate the species has been selected
  - [ ] Enable the "Check Answer" button when one or more of the species have been selected.
  - [ ] When the "Check Answer" button has been pressed, send a POST request with the selected items to the answer checking service (See [Answer Checking](#Answer-Checking))
  - [ ] If the answer is incorrect, display a message indicating how many selections were incorrect and how many selections are missing.
  - [ ] If the answer is correct, hide the "Check Answer" button, display a "Correct!" message, and do not allow the selected items to be modified.


## Exercise Data

The species selections to show should be loaded using an HTTP get request to the following URL: 

- `https://6gw3kytg07.execute-api.us-east-1.amazonaws.com/dev/items`

the JSON data contains an array of "Animal" objects, structured as follows:

```js
[
  {
    "id": "", // identifier for animal
    "label": "", // text label for animal
    "thumbnail": "" // url to thumbnail image of animal
  },
  ...
]
```


## Answer Checking

A server-side answer checking service is available at the following endpoint:
  - `https://6gw3kytg07.execute-api.us-east-1.amazonaws.com/dev/amphibians`

To evaluate a students answer, create a POST request where the body contains a JSON object with the key `answer`.  The value for the `answer` key should be an array of all of the species IDs that have been selected.  For example, to check an answer with the Crocodile, Komodo Dragon, and Fire Salamander selected, the following JSON object will be sent to the endpoint:

**Request Body**
```json
{
  "answer": [ "crocodile", "komodo-dragon", "fire-salamander" ]
}
```

**Response**
```js
{
  // correct answer IDs
  "correct": [ "crocodile", "komodo-dragon" ], 
  // incorrect answer IDs
  "incorrect": [ "fire-salamander" ], 
  // true if the student completed the exercise
  "success": false, 
  // number of correct answers missing
  "missing": 3 
}
```


# Getting started

To run this project you need Node / npm installed. Open your terminal and navigate to the directory, then run:

```
npm install
```

After the install completes, run:

```
npm run start
```

this will start a local version of the project at http://localhost:5000/


# Follow Up Questions

  **1. Describe which task you found most difficult in the implementation, and why.**
  
  I found the answer checking to be the most difficult.  Initially when I was sending the POST request to the answer service endpoint I was getting CORS errors.  I had to adjust the jQuery POST request to include `crossDomain: true` in order to get it to work.

**2. What lead you to choose these libraries or frameworks you used in your implementation?**

This is a very simple app with limited time for implementation, so I decided jQuery would be suitable.  I decided this because it doesn't require any complex tooling or setup like some other front end libraries allowing me to quickly get started.  Also, only having the single dependency keeps the app lightweight and well optimized.

**3. If there were no time restrictions for this exercise, what improvements would you make in the following areas:**
  - Implementation
    
    If I had more time I would change the implementation to utilize React using `create-react-app`.  This would add in the necessary tooling to use features like ES6, production builds, unit testing, hot reload, and component based architecture.  I also would have added error handling to handle if the data was unable to load or the answer service was unavailable.

  - User Experience

    Many improvements could be made for the user experience of this exercise. A few improvements include:
      
      - loading placeholder when the species and images are being loaded to indicate to the user the exercise is not yet ready, but will be soon.
      - adding helpful hints on incorrect answers to help users understand why they're answer is wrong.  For example, "Incorrect, the Komodo Dragon is not an amphibian because it has scales.  Amphibians do not have scales."
      - providing the answer message in a dialog window instead of below the selection area.  This way the status of their answer is presented front and center and does not risk being lost if it is not fully scrolled into view.

  - Accessibility

    To improve accessibility I would:
    
    - add a visual indicator, like a check mark, to items that are selected instead of relying on the colored border alone.  Color should not be the only way to signify important information.
    - refactor to use `<input type="checkbox">` and `<label>` for each of the selectable options so a screen reader will accurately identify the controls.  This will also allow keyboard operation of the app because users would be able to tab through the options and activate / deactivate with spacebar.
    - Use semantic HTML elements in the HTML (e.g. using proper `<h1>`, `<h2>` tags for headings, using `<main>` for main content area).  This would allow users with screen readers or other assistive technologies to easily jump to the areas they are interested in.
      
**4. If you were asked to refactor this exercise so it could be repurposed for any type of species, not just "Amphibians", what changes would you make?**

To refactor this to be a reusable exercise for other species I would switch to a component based library like React.  I would create components for `SelectItem`, `SelectionArea`, and `IdentifyExercise`.  The `IdentifyExercise` component would accept attributes for title, instructions, data endpoint, and answer checking endpoint.  This would allow an exercise creator to specify all of the required options and answering criteria to build their own exercise.

**5. Assume you are provided with a unique identifier for each student that completes the exercise.  You are tasked with storing user answers in persistent storage so their answers are loaded when they return to the exercise.  Give a high level overview of how you would implement this feature.**

In order to implement this feature I would:
  - Create a hosted database instance, my preference would be PostgreSQL with AWS RDS. The database would only need a single table with two columns (id, answer).
  - Create some server-side node functions for loading answers and saving serialized answers to the database, then deploy that to an AWS endpoint using Serverless.
  - On the front end when the exercise loads I would send a GET request to the endpoint to retrieve the user's answer (if any).  If there is an answer, I would restore the state in the user interface.
  - When the user submits an answer, I would send a POST request to the save answer endpoint and have it save a serialized version of the answer JSON along with the user identifier.


# Evaluation Criteria

You will be evaluated out of a total of 40 points based on the following criteria.

  - Learning Exercise (20 points total)
    - **Functionality (10 points)**: is the requested functionality implemented as described without bugs?
    - **Code Quality (10 points)**: is the code well structured and easily read? is the code optimized for performance?
    - **Bonus (3 maximum)**: bonus points are awarded for anything that goes above and beyond the items in the specification.  For example, a responsive implementation, or improvements for accessibility.
  - Follow Up Questions (20 points total)
    - Question 1 (2 points)
    - Question 2 (3 points)
    - Question 3 (5 points)
    - Question 4 (5 points)
    - Question 5 (5 points)