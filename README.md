# Cubicle-Bot
A Python library for simulating workers using a web application.

Cubicle Bots are YAML scripts that define workers and tasks. A worker is a set of tasks that a typical user of the application might do in a given time period.

Consider the following hypothetical scenario. You are a mutual insurance company that is using the [BriteCore SAAS Application](http://britecore.com) for managing your quoting and processing.

You might have different types of office workers who do different things for your organization.

For instance: a claims adjuster might spend their day processing claims. This worker could be defined to enter new claims, decline, or approve claims.

A payment processor might spend their day entering manual payments (made with cash or check), adding automatic payment methods, and running payments received reports.

The point is, you can use Cubicle Bot to simulate all of those workers.

The magic behind Cubicle Bot is that it uses extremely simple methods for setting up workers and tasks.

Workers are defined using YAML. A typical worker might look something like this:

`claims-adjustor.yml`
```
name: Claims Adjustor
work-day-starts: "09:00:00 AM"
work-day-ends: "05:00:00 AM"
tasks:
    new-claim:
        frequency: 300
        frequency-type: seconds
        starting-url: britecore/claims/list
```

Then in your HTML you will simply use custom attributes to reference the task being performed and tell the worker how to do the job you wish to see done.

```
<a href="#" id="newClaim_btn" new-claim="first|click|create-claim|wait-for-clickable: done-button|timeout: 60"></a>
```

The worker will go to the starting URL and, when it is time to perform the new-claim task, it will find the HTML element that contains the new-claim attribute and the "first" value.

Once it finds the <a> tag above, it will know it needs to click that element and then wait for the new-claim="done-button" element to become visible.

This can happen in a variety of ways and Cubicle Bot doesn't care if you are using AJAX or full page loads. When the element in question becomes clickable, the bot will load all of the directives of that element and perform them next.

```
<a href="#" class="btn_done" new-claim="done-button|click|last"></a>
```

In the example above, the next element contains the following information and directives.

1. done-button: This is an arbitrary directive name that we've given to this element. This is not anything more than an identifier to put into the previous element's directives so the worker knows which "new-claim" attribute to look for next.
2. click: This is the same as the previous directive we used. Click the element.
3. last: Though there would typically be more actions to perform, we are keeping this example very simple by having the worker bot only click these two elements and be finished with its task.

**Protip:** Tasks will end automatically if they have no more processable directives. It is highly recommended that you explicitly end your tasks so that you can modify them later with less confusion.
