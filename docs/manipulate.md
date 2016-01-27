## Add and remove skills

Now that we have learned about the DOM and used the console inside of our browser to get the hang of event handlers, let's create a new JavaScript file and start building our app. 

You might remember that our build tool, Gulp, will compile any `.js` files that we put in the `app` directory and make sure they are available. Let's begin by creating a new file `app.js` inside of the app directory.

With that in place, we can now begin to add more functionality to our form. We have more than just a single skill to boast about, so we want to be able to add multiple skills. We may also change our mind while we're filling out the form so we will have to be able to remove a skill, also.

### Adding a skill

You probably noticed the nice `+` button next to the `skills` input. Right now it doesn't do anything. We need to add a `click` handler in order to bring it to life.

In order to do that, we need to first select the `.add-skill` button which we will attach the handler to and then we need to also have a "blueprint" of the original div containing the skill elements so that we can copy it when we add another one.

```js
var addSkillButton = document.querySelector('.add-skill')
var skillTemplate = document.querySelector('.skills')

var addSkillHandler = function(evt) {
  alert('adding skill')
}
addSkillButton.addEventListener('click', addSkillHandler)
```

If you give this a try in your browser now, you'll see the alert pop up when you click on the plus.

What does "add skill" really mean? We want to duplicate the skill blueprint we have, `skillTemplate` and create another Node just like it. Then, we want to append it at the end of the list of skills.

We can clone any DOM Node with the `cloneNode` method. The `cloneNode` method takes an optional boolean argument to determine whether it should be a deep or shallow clone. Since we want the entire `input-group` element and all of it's children Nodes, we are going to pass a `true` in.

Unfortunately, there is no `appendAfter` method we can use like in jQuery, but there is an [`insertBefore`][insert-before] method. We can use `insertBefore` to append the new cloned Node just before the submit button.

The `insertBefore` method uses a handle on the parent node to attach the new Node in the correct spot within the tree. We can use the `parentNode` accesor on any DOM Node to get it's parent, or, since we know the parent is the form we can just directly select it again.

```js
var addSkillButton = document.querySelector('.add-skill')
var skillTemplate = document.querySelector('.skills').cloneNode(true)

var addSkillHandler = function(evt) {
  var submitNode = document.querySelector('.submit')
  var parentNode = submitNode.parentNode
  var newSkill = skillTemplate.cloneNode(true)
  parentNode.insertBefore(newSkill, submitNode)
}
addSkillButton.addEventListener('click', addSkillHandler)
```

We just grew a branch on our DOM tree 🌳! Does it look right, though?

![added skill](https://s3.amazonaws.com/f.cl.ly/items/113x3C3q1H133y3b3C1e/Screen%20Shot%202016-01-24%20at%2010.14.00%20AM.png?v=ad58e105)

After we clone a new node, we need to change the plus sign to a minus sign on the previous one. If you look at the `public/index.html` you'll see that both the plus and minus icons already exist in the DOM, except that the minus is currently `hidden` with the class `hidden`.

In jQuery, you can use the `.last()` method after selecting a group of elements to get the last one. Let's write a helper method that does just that for a given selector.

```js
var last = function(selector) {
  var all = document.querySelectorAll(selector)
  var length = all.length
  return all[length - 1]
}
```

Now let's update our `addSkillHandler` method to get a handle on the previous skill, update it to show the minus instead of the plus, and _then_ add the new skill to the end of the list.

When we have a handle on the DOM Node as we will with the previous skill, we can get a list of the Node's classes by using the [`classList` accessor][class-list]. We can then use `.add()` and `.remove()` to add and remove classes from this Node.


```js
var addSkillButton = document.querySelector('.add-skill')
var skillTemplate = document.querySelector('.skills')

var addSkillHandler = function(evt) {
  var prevSkill = last('.skill')
  var newSkill = skillTemplate.cloneNode(true)
  var submitNode = document.querySelector('.submit')
  var parentNode = submitNode.parentNode

  prevSkill.querySelector('.add-skill').classList.add('hidden')
  prevSkill.querySelector('.remove-skill').classList.remove('hidden')
  parentNode.insertBefore(newSkill, submitNode)
}
addSkillButton.addEventListener('click', addSkillHandler)
```

If you head over to the browser, it should work! Sweet DOM manipulation.

Did you try to click the second plus button like a smarty pants? Then you realize that it didn't work, right?

Fix it by attaching the handler to the new element as well.


```js
var addSkillButton = document.querySelector('.add-skill')
var skillTemplate = document.querySelector('.skills')

var addSkillHandler = function(evt) {
  var prevSkill = last('.skill')
  var newSkill = skillTemplate.cloneNode(true)
  var submitNode = document.querySelector('.submit')
  var parentNode = submitNode.parentNode

  prevSkill.querySelector('.add-skill').classList.add('hidden')
  prevSkill.querySelector('.remove-skill').classList.remove('hidden')
  newAddSkill.querySelector('.add-skill').addEventListener('click', addSkillHandler)

  parentNode.insertBefore(newSkill, submitNode)
}
addSkillButton.addEventListener('click', addSkillHandler)
```

Great, now you're ready to remove some skills!

### Removing a skill

Much like we created an event handler for adding a skill, we will now just create one to remove a skill.

```js
var removeSkillHandler = function(evt) {}
```

When the minus button is clicked, you'll remember from the event section that the minus button element becomes the `currentTarget` on the event object passed in as the first argument to our handler.

If we have a handle on the minus button, then we can select the `parentNode` to get the group of elements that makes up a single skill. And if we can do that, then we can just call [`remove()`][remove-node] do remove the Node from the DOM.

```js
var removeSkillHandler = function(evt) {
  var skill = evt.currentTarget.parentNode
  skill.remove()
}
```

[class-list]: https://developer.mozilla.org/en-US/docs/Web/API/Element/classList
[cookies]: https://devdocs.io/dom/document/cookie
[css-selector]: https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Getting_Started/Selectors
[devdocs]: http://devdocs.io
[dom]: https://devdocs.io/dom/
[event-listener]: https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener
[events]: https://devdocs.io/dom_events
[git-scm]: http://git-scm.com/downloads
[gulp]: http://gulpjs.com/
[heroku-node]: https://devcenter.heroku.com/articles/getting-started-with-nodejs#introduction
[heroku-toolbelt]: https://devcenter.heroku.com/articles/getting-started-with-nodejs#set-up
[html-input]: https://developer.mozilla.org/en-US/docs/Web/API/HTMLInputElement
[insert-before]: https://developer.mozilla.org/en-US/docs/Web/API/Node/insertBefore
[jquery]: https://jquery.com/
[localhost]: http://localhost:3000
[mdn]: https://developer.mozilla.org/en-US/
[mdn-appendchild]: https://developer.mozilla.org/en-US/docs/Web/API/Node/appendChild
[mdn-createelement]: https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement
[mdn-createtextnode]: https://developer.mozilla.org/en-US/docs/Web/API/Document/createTextNode
[mdn-events]: https://developer.mozilla.org/en-US/docs/Web/Events
[mdn-foreach]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach
[mdn-validations]: https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/Constraint_validation
[mocha]: https://devdocs.io/mocha/
[mouse-event]: https://developer.mozilla.org/en-US/docs/Web/API/MouseEvent
[node-install]: https://nodejs.org/download/
[node-list]: https://developer.mozilla.org/en-US/docs/Web/API/NodeList
[npm-g-without-sudo]: https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md
[onreadystatechange]: https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/onreadystatechange
[rails-api]: https://github.com/rails-api/rails-api
[remove-node]: https://developer.mozilla.org/en-US/docs/Web/API/ChildNode/remove
[san diego js]: http://sandiegojs.org/
[sdjs-app]: //sandiegojs-vanilla-workshop.herokuapp.com
[xhr]: https://devdocs.io/dom/xmlhttprequest
[xhr-mdn]: https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest
[zepto]: http://zeptojs.com/