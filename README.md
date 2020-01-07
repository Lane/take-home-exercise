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

# Getting Started

This is a placeholder for instructions on how to run your implementation.

# Follow Up Questions

  1. Describe which task you found most difficult in the implementation, and why.
  1. What lead you to choose these libraries or frameworks you used in your implementation?
  1. If there were no time restrictions for this exercise, what improvements would you make in the following areas:
      - Implementation
      - User Experience
      - Accessibility
  1. If you were asked to refactor this exercise so it could be repurposed for any type of species, not just "Amphibians", what changes would you make?
  1. Assume you are provided with a unique identifier for each student that completes the exercise.  You are tasked with storing user answers in persistent storage so their answers are loaded when they return to the exercise.  Give a high level overview of how you would implement this feature.

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