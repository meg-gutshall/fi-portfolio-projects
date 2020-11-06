# Part 1: Planning and Building a Rails API

## Before You Code

* [x] Check project requirements in JavaScript READMEs
* [x] Ideate! What do you want to build?
* [x] Wireframing and user stories
  * [x] Write out your models, attributes, and their associations
* [x] Design your MVP
  * [x] Your "minimum viable product" vs. your "stretch goals" \(bonus features you want but don't need to meet the project requirements\)

## Project Setup

* [x] Generate a new [Rails API](https://edgeguides.rubyonrails.org/api_app.html) app using the following command:

```bash
rails new fi_study_group_central_backend --database=postgresql --api
```

* [x] Create a GitHub repo
* [x] Create a new branch when building out each migration and model:

```bash
git co -b user
```

{% hint style="info" %}
**NOTE:** It is best practice to always create a new branch when working on a new feature or editing your code. Your `master` branch should only have working code. Debug issues in a new branch, not in `master`. This way you can always go back to the `master` branch for a fresh and clean copy of your app.
{% endhint %}

## Build Your Models \(One at a Time\)

{% hint style="info" %}
**NOTE:** Remember to _**VERTICALLY**_ build your MVP! This means building out one model/feature at a time. DO NOT build out _all_ the models and controllers at once. This is the easiest way to get lost in your project very early on. Refer back to the [Planning, Tips, and Support document](https://github.com/learn-co-students/js-spa-project-instructions-online-web-sp-000/blob/master/project-planning-tips.md#build-vertically-not-horizontally) for more info.
{% endhint %}

* [ ] Generate a new model:

{% tabs %}
{% tab title="User" %}
```bash
rails g model User first_name last_name email_address password_digest
```
{% endtab %}

{% tab title="Upvote" %}
```bash
rails g model Upvote comment:text
```
{% endtab %}

{% tab title="Request" %}
```bash
rails g model Request topic module description:text
```
{% endtab %}
{% endtabs %}

* [ ] Add the appropriate associations to the model file that was generated
* [ ] Run your migrations:

```bash
rails db:create
rails db:migrate
```

* [ ] Create some seed data in `app/db/seeds.rb`, then load it into the database and test out your models, associations, and migrations:

```bash
rails db:seed

rails console
```

{% hint style="warning" %}
**EXTRA:** Add a preset list of modules to the modules dropdown on the request form. Create a `modules.js` file and export it to the `request.js` file using Parcel. Then iterate over the modules to populate the dropdown selector. See below for an example:

```javascript
const mockFetch = new Promise(resolve => {
  const data = ["Honda", "Audi", "Ford", "Toyota"];
  setTimeout(() => {
    resolve(data)l  
  }, 2000);
});

function populateDropdownSelection() {
  mockFetch.then(data => {
    let carSelection = document.getElementById("cars");
    data.forEach(car => {
      let option = document.createElement("option");
      option.setAttribute("value", car);
      option.innerHTML = car;
      carSelection.appendChild(option);
    });
  });
}

populateDropdownSelection();
```

[_https://codesandbox.io/s/eager-hofstadter-66z4n?file=/index.html_](https://codesandbox.io/s/eager-hofstadter-66z4n?file=/index.html)\_\_
{% endhint %}

* [ ] If everything works, _**then**_ merge the branch into `master`

## Sources

### Notes

{% embed url="https://github.com/AyanaZaire/seeda\_syllabus\_backend/blob/master/project-build-part1-notes.md" caption="Project Build Part 1 Notes" %}

### Video

{% embed url="https://www.youtube.com/watch?v=Q5R7HSqdGFk" caption="Project Build Part 1 Video" %}

