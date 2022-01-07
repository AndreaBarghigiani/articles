## How to know if CSS animation has ended

CSS animations are great but sometimes you wish to do change your DOM once it has ended, who knows probably you want to remove the element that you just hidden.

Let's say for example that you have two lists, one for recipes and the other for your favorite recipes.

Each recipe has a *Like* button, once this get's pressed your application will add it to your favorite list. At the same time, each favorite recipe has a *'Delete'* button that allows you to remove it from the list.

Don't care about the backend tasks that will handle this behavior, as a Frontender you only care about the animation that runs when you add (*fadeIn*) or remove (*fadeOut*) the item from the favorites.

The add is simple, you add the element to the list with a specific `class` and CSS does the rest but when you remove it something happens. The item gets removed right away from the list, because you want to get rid of it from the DOM, and no animation is played.

This probably happens because in response to the `click` event you have something like:
```js
element.remove()
```

But you can tell to JavaScript that it needs to wait for the animation to end before remove the element from the DOM, all you have to do is to listen for the `animationend` event:
```js
element.addEventListener("animationend", () => {
	element.remove()
})
```
I've skipped all the middle operations because this is part of one of my TIL.