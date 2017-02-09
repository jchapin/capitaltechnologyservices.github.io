---
layout: post
title: "Fighting the Undead"
title_short: "Fighting the Undead"
excerpt: >
  A couple of times a year a developer or stakeholder will reopen a closed issue
  on a project we maintain. We discourage reopening issues. Why? Because no one
  wins when zombie issues rise from the grave.
---

{{ page.excerpt }}

Here is how the cycle is supposed to work:

The stakeholder lovingly crafts a user story, sometimes with the assistance of
one of our staff members, often on their own. The stakeholder crowns the
specification with their chosen level of priority, somewhere in the range of,
"meh, that would be nice" up to "help us! you're our only hope".

Our development team reviews the story, determines if we need any clarification
and attaches an estimate. The project manager runs to the client and presents
them with our estimate, budgets are updated, and the process is running smoothly
thus far.

Implementation starts, a solution is submitted, leading to peer review, a few
small corrections, and eventually acceptance of the pull request.

The project manager gleefully runs back to the client and directs them to an
environment where they can review the solution and discusses the release
schedule for the enhancement.

The release occurs, and the new software lives happily ever after.

Issue closed.

But every once in a while:

Issue reopened...

Why?

Because the developer made a mistake.

i.e. The developer forgot to include validation on a required field. The product
isn't receiving all of the data necessary.

OR

Because the stakeholder made a mistake.

i.e. The stakeholder was interested in the median value of a set of numbers and
not the average. So their dashboard statistics aren't exactly what they wanted.

OR

Something similar but unrelated to the specification has caused a new problem.

i.e. An earlier specification to gather information from an API and serve it up
to a user was completed last month. This month, the API vendor changed the
service without any forewarning and the data isn't present in the product
anymore.

Seems reasonable to reopen that ticket right?

No. Don't do it.

It warps the record of the development team's wins and losses.

Reopening issues is bad for the stakeholder because it makes it extremely
difficult to spot instances where the development team failed. The team simply
gets to try again, correct their mistake, and then continue to polish their
wonderful, "perfect" record of success. 

See the problem? Good. Now here's the other side of the coin...

When a stakeholder makes a mistake and omits information from a specification or
decides the issue needs to be reopened for further improvements, it steals a win
from the development team. But that's the least of the problems for both
developer and stakeholder. The estimate and priority tags on issues are very
important to a project. We directly use the estimate tag's points to project
costs and priority tag values for scheduling. Reopening an issue may change the
way that these items are reflected in the project's budget and schedule.

Finally, in the case that it's neither developer nor stakeholder fault, but some
outside condition that has inspired the resurrection of an old issue here's just
one more reason not to reopen. The communication in the ticket becomes
unnecessarily long and unfocused.

In the aforementioned example, the development team integrated a web service
from a third party and the third party changed their API, breaking the
stakeholder's product. It happens, and when that's the case you don't want to
resurrect a ticket that contains old notes, an old title, and anything else that
is irrelevant to the state of the product in the present.

Don't resurrect old issues.

Open new issues and link or associate them with the original issue.

Linking allows you to preserve the win/loss rate of the development team and
individual developers.

Linking allows for the background information on the original issue to be
associated with the new specifications without getting in the way.

Linking issues preserves the estimate and priority information that's useful and
important for budgeting and scheduling.
