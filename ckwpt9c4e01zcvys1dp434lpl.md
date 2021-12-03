## The new class_names in Rails 6.1

If you've worked with React.js I am sure you already used a precious package named `classnames`, if you haven't you [definitely need to take a look at it](https://www.npmjs.com/package/classnames).

A package like this helps you in many ways and allows you to build components that are easier to understand, even if here we're just talking about populating the `class` attribute of any HTML element.

Basically, all it lets you do is to **simplify the way you conditionally write classes** and from Rails 6.1 we have something really similar 🎉

When you work with a component system there are many reasons why you want (or not) add a class, and there are numerous approaches that you can take to reach the end result.

Today I don't want to look at the past, I just want to show you how you can write your classes from now on:

```ruby
<% 
	classes = class_names(
		:card,
		{
			"card--style-#{style}": style,
			"card--position-#{position}": position,
			"card--spacing-#{spacing}": spacing
		}
	)
%>

<div class="<%= classes %>">
	<!-- Your card -->
</div>
```
Here you can see a preference of mine, I usually like to separate the generation of classes with their actual usage because I find it more readable to separate the logic from the actual usage. `style`, `position` and `spacing` are just local variables that hold a truthy value, something like:
```ruby
style = card.dig(:options, :style)
```
But nothing stops you to use `class_name` right withing your component:
```html
<div class="<%= class_names(:card, {"card--style-#{style}": style, "card--position-#{position}": position, "card--spacing-#{spacing}": spacing}) %>">
	<!-- Your card -->
</div>
```
Personally, I find it a bit annoying but you know, there are infinite ways of do things and as long as you (and your team) find it readable you're good to go.

## Even better with ActionView::Helpers
If you like to use `tag` or `content_tag` to generate the HTML of your views I have good news for you: you can exclude the call to `class_names` and just pass the array of your classes!

The example I've shared above becames:
```ruby
<%= tag.div class: [:card, {"card--style-#{style}": style, "card--position-#{position}": position, "card--spacing-#{spacing}": spacing}] do %>
	Content
<% end %>
```

## Interesting bit
`class_names` it's just an alias for [`token_list`](https://api.rubyonrails.org/classes/ActionView/Helpers/TagHelper.html#method-i-token_list)