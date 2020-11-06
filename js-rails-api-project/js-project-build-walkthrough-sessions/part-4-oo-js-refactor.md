# Part 4: Object-Oriented JavaScript Refactor

## Error Handling

* Use validations in the models and conditional statements in the controllers to catch errors in the backend
* Use `.catch` on the fetch to catch errors in the frontend

## OOJS

### Create a New Branch

**First step:** Always create a new branch

### Create a JavaScript Class

* Make sure your new JavaScript files are linked via the `index.html` script tag
  * Be careful of the order the script tags are entered into the HTML file! \(i.e. If `syllabus.js` is required in `index.js`, `syllabus.js` should be loaded in the `index.html` file before `index.js`.\)

### Create a Constructor

* Use `this` syntax in the constructor function
* Make sure you account for the `fast_jsonapi` nesting structure when adding attributes to this method
  * Do this by passing the constructor function two parameters: `syllabus` and `syllabusAttributes` and two arguments from the fetch: `syllabus` and `syllabus.attributes`
* Make sure to push each new instance of `Syllabus` into an array to act as a `Syllabus.all` object

### Refactor Render Function

* Don't need the syllabus object as a parameter in a class method
* Use the `this` syntax
  * Change syllabus to `this`
* Change the interpolated parameters to match the constructor attributes
* Don't need the `function` keyword in a class method

### Create a Static Method

See notes file

## Sources

### Notes

{% embed url="https://github.com/AyanaZaire/seeda\_syllabus\_frontend/blob/master/project-build-part4-notes.md" caption="Project Build Part 3 Review & Project Build Part 4 Notes" %}

### Video

{% embed url="https://www.youtube.com/watch?v=oUiLxmgOvJ8&feature=youtu.be" caption="Project Build Part 4 Video" %}

