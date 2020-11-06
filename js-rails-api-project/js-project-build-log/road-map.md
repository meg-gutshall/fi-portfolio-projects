# Road Map

## April 28, 2020 – May 10, 2020

I took **FOREVER** to plan. I did a little bit of project planning throughout April, but I began in earnest on April 28, 2020. To help plan out my project, I used the [Planning, Tips, and Support documentation](https://github.com/learn-co-students/js-spa-project-instructions-online-web-sp-000/blob/master/project-planning-tips.md) found in the [JavaScript Project Instructions repository](https://github.com/learn-co-students/js-spa-project-instructions-online-web-sp-000) on GitHub.

When I started writing code snippets into my plan, I realized how ridiculous it was getting and made myself actually _start_ the project. And I'll let you in on a little secret... Even with all that pre-planning, I **still** ended up changing a ton of stuff as I wrote out the code. **Lesson learned:** Don't let too much preparation prevent you from jumping in and starting, but you should still go in with a plan. Finding that happy medium comes with experience and the best experience usually comes from messing up a few times!

I also must add that I was **so lucky** to be part of a group of students who were able to attend a live project build series run by Flatiron School instructor Ayana Cotton-Zaire. She's an incredible teacher! She's highly dedicated and you can see it in the detailed notes she created for each part of the build.

## May 11–21, 2020

I finally began coding by implementing Learn.co's [JWT Auth Rails guide](https://learn.co/lessons/jwt-auth-rails) in my app. It was really easy to follow along step-by-step. I like how they encouraged the use of Postman as well. It's a handy tool and pretty easy to learn!

### JWT

This goes beyond the scope of the project requirements by including JSON Web Tokens. Admittedly, this wasn't my intention at first. I just wanted to build out a `User` model and figured since I was recommended this guide by a fellow codepanion \(s/o Sushi!\) and have plans to scale this project in the future to include authorization, why not!?

I've come to realize that a large number of my fellow classmates implement JWT in their project because it's an important security feature of many apps. I look forward to learning more about it down the line!

## May 27–28, 2020

I wrote a rough draft of my blog post because that's my least favorite part about projects. I did the post on the JWT Auth Rails implementation. I figured it would be easier to do a rough right after I had implemented it, then clean it up at the end of my build. We'll have to wait and see if that pans out!

Then I created my model map diagram. I had already planned out my models \(of course\), but I didn't make a diagram of them and really wanted that visual to be able to refer back to.

Next, I set up my front-end and connected it to my back-end! Once I had all that working and was ready to start writing the front-end JavaScript code, I realized that I didn't even have a visual for how I wanted my app to look. I had a picture in my head, but I didn't have it drawn out on paper anywhere and to me, the concept of rendering something on a page without first knowing what it's supposed to look like is strange. So over the next few days I spent some time drawing out wireframes and figuring out what I wanted my different views to look like.

## June 14, 2020

I implemented my `Request` model on the back-end and seeded the database.

## June 17–18, 2020

I finally got some code written on my front-end! I created a `fetch` request for all `Request` objects.

I also decided to try a few different serializers. Flatiron recommends the [Fast JSON API](https://github.com/Netflix/fast_jsonapi) gem, but it's a very in-depth serializer for \(what is for now\) a simple project. I saw that Fast JSON API was forked into a gem called [jsonapi-serializer](https://github.com/jsonapi-serializer/jsonapi-serializer). Since it's being actively maintained and improved upon by a group of open-source programmers, I tried it out but the functionality isn't much different than its parent gem. Also, I found a gem called [Blueprinter](https://github.com/procore/blueprinter) that looked interesting, but there wasn't clear documentation on how to install it and I kept getting errors that I couldn't resolve so I ultimately settled on Fast JSON API.

## June 24, 2020

Originally, I was thinking of making `Modules` their own JavaScript class. The plan was to scrape them directly from the Learn.co website using the Nokogiri gem, then create a new instance of each module for the dropdown menu as well as the new `Request` form.

After playing around with the code for a bit, I decided to hard code them into an array instead. It's not part of the MVP and I can always go back and refactor later if I want to!

## June 25–27, 2020

I wanted to use Bootstrap to style my project since it makes inserting a responsive navbar \(among other things\) ridiculously easy. I wasn't having any luck when trying to install it using yarn or npm, so I decided to go for broke and use webpack! Webpack has a comprehensive ["Getting Started" guide](https://webpack.js.org/guides/getting-started/) that was incredibly helpful in getting me familiar with the basics. I highly recommend reading through to the end of the "Development" section where you'll learn how to implement live reload. It auto updates the browser any time there are edits made to the code so you don't have to worry about refreshing the page to see changes.

## June 28, 2020

I created a custom HTML template to act as the skeleton for all of my web pages. Since I'm using the Bootstrap framework, I'll have containers and rows alongs with a navbar and footer on every page. Those parent elements stay constant, therefore, I render them in my HTML template.

I built out my `Request` class in JavaScript by add a constructor method along with a few static methods.

I also created a `DOMElements` class as a JavaScript service class. I use it to extract the process of rendering HTML elements from my actual JavaScript code. I think this just makes it cleaner and easier to reuse certain elements.

## June 30, 2020

I found a big 'uh-oh' in my project! I had defined the `Request` class on the front-end to match my `Request` model on the back-end. It turns out that JavaScript has a native `Request` class so my naming convention was messing all kinds of things up! To solve this, I went through my whole app \(the full stack\) and renamed `Request` to `TopicRequest`. As soon as I did that, all of my problems with using the ES6 module import/export functioning cleared up and random little bugs just disappeared. Poof!

## July 1, 2020

After taking yet _another_ look at my model diagram and my MVP, I decided to rip out my `Upvote` model and add it as an attribute on the `TopicRequest` model instead. Well, I didn't _exactly_ rip it out. It's still lurking in my project in a separate branch for whenever I come back to it later. For now, I'm just ready to finish this thing!!

## October 1, 2020

Yeah... about being just ready to finish this thing... Well plans changed. Again. This time it wasn't my build plans at least!

Don't get me wrong, I'm still ready to finish this project—more than ready—but I got side tracked by a couple of side projects that I had the opportunity to lead.

### Side Projects

In early July I was asked to create a website for the local NAACP branch and jumped at the chance. It is a one pager that accepts donations for a fundraiser called "Black Is Beautiful." \(_Link to fundraiser, website, whatever else, etc.\)_ It took me roughly three weeks to plan and build out the site. It's built with HTML, CSS, and Vanilla JS and uses Webpack.



{% hint style="warning" %}
_Cover the following:_

* [ ] _Side projects_
* [ ] _Change in my attitude toward the topic I chose for the project_
* [ ] _Progress I've made in my project \(i.e. commits on the repos\)_
* [ ] _What I have left to do going forward_
{% endhint %}

