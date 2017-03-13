---
layout: post
title: "Adopt the Habit of Submitting Small Pull Requests"
title_short: "Submitting Small Pull Requests"
author: "Chapin, John C."
image: '/images/posts/staircase-of-risk.jpg'
excerpt: >
  "How would you test if you were a superhero?", my wife asks me one evening.
  "Well, if I thought I could fly I would probably walk up a single stair, jump
  back to the ground and repeat the process by climbing one more stair until
  I'm proven right or wrong."... "That sounds like something Amazon Alexa would
  say."
---

{{ page.excerpt }}

<img class="img-responsive" src="{{ page.image | prepend: site.url }}">

## I Don't Like Risk

Okay, so according to my wife, I can sound like a cold and analytical robot when
answering fun hypothetical questions. But the point of my somewhat dry and
apparently uncompelling test procedure is that jumping off a flight of three
steps is a lot safer than a flight of three hundred. If, in the most likely of
scenarios, I cannot fly I would rather be proven wrong early and suffer slight
discomfort in landing compared to a broken leg or considerably worse. I've used
the same analogy before when explaining to new developers that they want to take
on as little risk as possible when adding to or enhancing the features of a
software project.

## Yeah, Yeah, Yeah...

You've heard it all before in different scenarios. "Only change one thing at a
time", "Save your game of Zelda before you go in to fight Ganon..." We're taught
from a very young age how not to get ourselves into risky or unrecoverable
situations. But sometimes we don't adopt the habits we need to proceed safely.
So how does a developer begin to build a feedback loop that encourages smaller,
lower risk changes?

#### Your goal is not to complete the entire user story in a single pull request.

Bad things happen when developers get lost in their own world working on what
they perceived to be a small feature or enhancement, that turned into an entire
project. I've seen developers frustrated by changes to the codebase that affect
the work they wanted to propose in a largish pull request. I've seen developers
miss half of the specifications for the user story out of exhaustion and loss
of focus. Large changes are bad for business. Think small.

#### Your goal is to submit small and safe pull requests on the way to completing the user story.

Every pull request should be an obvious small step on the way to completing a
user story. Your reward, if you're using Github, is to see as many purple merge
icons on your user profile. You should see far fewer closed issues. Get it.
The ratio of pull requests accepted to issues closed should be greater than 1/1.

## Get Out Your Knife and Focus on the Obvious

Say you're taking on a specification to several new fields to a user's profile.
You know that you're going to have to add the data to the user model, or an
associated model in the system. There will be fields to add to forms and
validation to perform whenever the values in those forms are submitted... Look
at how detailed this user story is! Now stop. Resolve that you aren't going to
handle implementation of all of the data model changes, record retrieval, UI
changes, validation in one pull request. Cut off the smallest foundational
changes, perhaps the addition of the user's first name as a new field on the
user model. That's it. That's all you're going to do. It's painfully obvious.
It's too easy.

## Your Full Attention

Only implementing a single aspect of an entire user story at a time is going to
force you to focus. You're going to ask if the specification has really defined
everything there is to know about a user's first name. It might raise questions
about whether you need to collect more than just a first and last name, perhaps
a salutation and a suffix should be collected too. Those might be details that
the specification omitted, but are not going to stop you from taking this small
first step... You can leave that clarification to the project manager while you
knock this first name business out of the park.

On another somewhat related note, keeping pull requests small also makes peer
review considerably easier too. No one likes combing through a large diff and
trying to pick out why someone changed a particular function... or deleted one.

<small>
  Incidentally, in regard to that last example, one should probably write the
  replacement function in one pull request and remove the old function in
  another.
</small>

## Testing Just Got Easier Too

You know you should be thinking about and writing tests for the application's
behavior before your write a line of functional code. When you commit to only
implement one aspect of a user story at a time, the test cases are easier to
call to mind as well. For instance, should a user be able to omit their first
name when completing the user profile? Should it be required? Did the user story
even mention validating the presence of a name?

## How We're Going to Troubleshoot When Things Go Wrong (git bisect)

When it happens that we have missed something in all of our reviews before
integrating a change into master, we're going to be using git bisect to go back
through all of the previous commits on a project. Say we ran into a front end
error that has appeared on the aforementioned user profile. If each of the
details of the user story was implemented in a small pull request and if the
changes between commits were small and sensible enhancements to as few files as
possible, it's going to be easy to find the problem. What is going to be
difficult to trace through is a pull request that changes dozens of lines on
dozens of files with commits that sometimes change three or four of those files
at a time.

#### Performing small commits means that at the end of a git bisect session, it will be easier to spot the cause of a problem.

I have to bring this piece to a close, it's 6:00 pm on a Friday and I'm going to
have a drink with my wife. We'll no doubt have another fun conversation that
I'll bring way too dry of a perspective to. But until the next post is written,
consider that if you take small steps in improving your software you might just
prove that you have the super power to improve code without breaking it... which
is probably the lamest super power ever.
