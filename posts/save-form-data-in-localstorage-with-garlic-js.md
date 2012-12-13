---
title: Saving unsubmitted form data with localStorage and Garlic.js
date: '2012-12-11'
description:
categories:
---

I was thinking about improving user forms in one of my projects lately, and the first thing that came to mind was using some sort of autosave functionality. But I can't really see a clean way to implement it with traditional Railsy style controllers. We would have to create an empty model instance, persist it, skip validations while user fills in all the fields. And we have to clean up if the user never really save the model. So I really dislike this approach.

We can easily do any kind of autosave on our update actions of the existing model. But the real problem is only with new data, which has not been validated and persisted yet. So how can we persist data from a form before it is submitted? And here comes localStore and fancy little library [garlic.js](http://garlicjs.org). The only thing you have to do after adding the library is to add ```data-persist="garlic"``` to your form. That's it, your input will be stored if form is not submitted.

I also have some image uploading in my form, so to retrieve the images on form reload I put image ids in hidden inputs.

```html
<input class="garlic-auto-save" type="hidden" name="image_ids[0]" value=''>
```

You will have to trigger 'change' event manually for garlic to save new value

```coffeescript
$('input[value=""]').eq(0).val('1').trigger('change')
```

And now on the form reload we can recreate the same state as before. Pretty cool!