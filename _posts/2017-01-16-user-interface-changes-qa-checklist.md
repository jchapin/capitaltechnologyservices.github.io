---
layout: post
title: "Checklist - User Interface Changes"
title_short: "Checklist - User Interface Changes"
excerpt: >
  When a pull request changes any aspects of an application's user interface
  run through this checklist.
redirect_to: /
---

{{ page.excerpt }}

1.  It has to, "look good"

    This is a potentially frustrating goal. But if there is anything misshapen,
    distorted, inconsistent, or distracting about the changes and particularly
    if there was no specific guidance to modify the appearance of an element
    the modifications cannot pass QA. If there's a case to be made that an
    element of the user interface could benefit from modification, an issue must
    be written and specific measurements must be defined in the case.

1.  Validate against W3C standards

    1.  If the page is new it must be 100% compliant with W3C validation
        standards.

    1.  If the page exists and has existing validation issues, it must not
        increase the number of validation warnings or errors.

1.  Ensure responsive design standards

    1.  Take screen shots at the following screen widths and append them to the
        issue:
        1.  640
        1.  800
        1.  1024
        1.  1280

    1.  Additionally scale the browser from the widest possible resolution down
        to the minimum possible width. If there are any issues that show up at
        odd resolutions, note them and their potential risk on the ticket. For
        example, if the primary menu breaks to a second line at a resolution
        close to one of the break points for mobile and tablet resolutions, we
        will want to know about it.

    1.  If there is a grid system implemented on the page that allows definition
        of column widths at multiple layout sizes, check that there are explicit
        definitions of the width of each of the columns at each layout size. For
        example:

        If there is a tabular layout of data that has too much information for
        it all to be displayed on the mobile layout, see that less critical
        information is hidden at mobile resolutions. If you do not have a
        definition for what information can or should be hidden at mobile
        resolutions, seek it.

1.  Validate that all user interface controls are localized.

    1.  Make sure that all controls use localizations (i.e. ```t(:submit)```)
        instead of "Submit"

    1.  Check the English localizations file to ensure that there is a proper
        localization for the symbol.

    1.  Don't add localizations that are too specific, that can lead to a
        large and expensive to maintain collection of localizations. Instead of
        "User Updated", "Address Updated", "Phone Number Updated", it may be
        sufficient to have a notice "Record Updated", that can be used on a
        variety of models.

1.  Make sure that text is legible.

    1.  Check that there is an appropriate contrast ratio between the text color
        and the background: [http://webaim.org/resources/contrastchecker/](http://webaim.org/resources/contrastchecker/)

    1.  If you need to call attention to a particular notice, consider altering
        the space around the text instead of the color or background the text
        itself.
