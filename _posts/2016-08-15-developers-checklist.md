---
layout: post
title: "Developer's Checklist"
title_short: "Developer's Checklist"
excerpt: >
  This checklist guides developers through daily operations that will improve
  their personal efficiency and their communication with team members.
---

{{ page.excerpt }}

1.  If it is the 1st or the 16th of the month, compile all time records on a per
    client basis and send them in to your project management contact.
    1.  Time records must conform to the following column set or minimally
        contain the following information:
        *   Datetime Start
        *   Datetime End
        *   Duration - (Datetime Start - Datetime End) in hours to the second
            decimal place
        *   Comment - Preferrably a URL linking to the issue, but a short
            comment of a sentence or less will suffice when appropriate
        *   Task - The type of work being performed, conforming to the project
            plan's task types
        *   Project - The name of the project or product the time entry is
            associated with
1.  Check any projects for open pull requests in need of review. Make sure when
    reviewing pull requests:
    1.  Comment on specific lines if possible. Most pull requests should be
        small enough to review line by line. Particularly if the submitting
        developer has already left their own comments.
    1.  Make sure you're comments convey an understanding of what's going on in
        the pull request. More than half of the reason for conducting code
        review is increasing the knowledge and awareness of developers working
        with the submitting developer.
    1.  Bad comments are short and vague (i.e. "Looks good :+1:")
    1.  Good comments demonstrate understanding (i.e. "Replaced removing empty
        characters with the gsub method with the more efficient tr method :+1:")
1.  Post your standup message to the #developers Slack channel. Answer these
    three questions when applicable:
    1.  What were you working on your last active day?
    1.  What were you planning on working on today?
    1.  Are there any obstacles or issues that your teammates can help you with?
1.  Before starting work on an issue, check and make sure that the specification
    has enough detail.
    1.  We want to make sure we follow the DMAIC (Define, Measure, Analyze,
        Implement, Control) cycle as closely as possible. Having enough
        information is crucial to being able to properly follow the rest of the
        steps in the process.
    1.  It's also helpful to have as many of the basic "5W1H" questions in the
        answered in the specification.
        *   **Who** does the specification concern? (usernames and roles are
            helpful)
        *   **What** does the user need? (description of the feature or bug)
        *   **Where** does the functionality need to be added or fixed? (URLs)
        *   **Why** does this specification need to be addressed?
        *   **When** does the issue occur or what times are important to the
            specification (including deadlines)
        *   **How** the stakeholders would like to see the issue resolved?
    1.  Specifications must also conform to our INVEST+E guidelines. Meaning
        that issues must be:
        *   Independently addressable
        *   Negotiable
        *   Valuable
        *   Estimable
        *   Small
        *   Testable
        *   Examples included when possible
1.  Solve the problem selected
    1.  Measure
    1.  Analyze
    1.  Implement
1.  Control... by implementing a pre-push hook on all of your projects. We have
    continuous integration and deployment tools that monitor product quality.
    However, having a basic pre-push hook will prevent you from submitting a
    broken branch for approval.

        #!/bin/sh
        # Brakeman
        brakeman
        # Rubocop
        rubocop -c config/rubocop.yml
        # Tests
        rake test

1.  When your work is ready for approval:
    1.  Post any important metrics from the pre-push routine or relevant
        benchmarks to explain your work
    1.  Comment on the most important lines of your pull request
    1.  Notify the correct @team on Github
    1.  Post a link to the pull request in the Slack #developers channel
1.  When you finish an issue, go estimate more work in project backlogs.
    Propose solutions and ask for more detail when necessary.
1.  Plan what you can address in the next week as you proceed through the
    current week. You want to make sure that when the weekly developer meeting
    comes around that you have done all of the, "fishing" and that you don't
    have to obtain too many suggestions on what to work on next. You just need
    to reconcile what priorities you can address with what the project manager
    has obtained from the client, and proceed through the cycle again.
    