---
layout: post
title: "Project Documentation"
title_short: "Project Documentation"
excerpt: >
  This article describes the types of documentation that need to accompany each
  project we're involved with and standards to which that documentation must
  conform.
---

{{ page.excerpt }}

There are four types of documentation that we need to be concerned with:

1.  User Manual - a manual for the broader user base of the application.
1.  Administration Manual - a manual for project owners on how to care for the
    product. This includes documentation for devops and system administrators
    that will be caring for the supporting infrastructure.
1.  API / Developer Manual - This manual will be produced when the product
    features the ability for third party developers to integrate our product
    into their products. This is produced in addition to documentation
    written in the source code of the product.
1.  Source Documentation - Documentation integrated into the source code of
    the product.

## User Manual Standards

The user manual is most likely just a subset of the documentation that you would
give to the administrators of the system we're building. For all of the concepts
in the system that have actions performed upon them you will want a descriptive
paragraph on how to perfom the action and a describe the consequences of that
action. Most applications will have the ability to create, read, update, and
destroy (CRUD) records. Additionally, there's usually going to be a view for a
list of all of the records of a particular type. All of these things seem self
explanatory to most developers. However, the only way to be certain that users
understand how to take advantage of a system is to document it clearly and
concisely.

Using the following Markdown template as an example, you can describe any
requirements before engaging in a process, the steps to complete a process, and
any notes relevant to the user upon process completion. For instance, one might
define **requirements** that in order to create a checking account record one
must know their routing and account number before they begin. After completing
the process steps to add the checking account, one might describe that the
checking account will now be available for use to pay invoices elsewhere in the
system.

<pre>
  <code>
# Subject

## Index

### Requirements

*
*

### Steps

1.
1.

### Notes

*
*

## Create

### Requirements

*
*

### Steps

1.
1.

### Notes

*
*

## Read

### Requirements

*
*

### Steps

1.
1.

### Notes

*
*

## Update

### Requirements

*
*

### Steps

1.
1.

### Notes

*
*

## Destroy

### Requirements

*
*

### Steps

1.
1.

### Notes

*
*

## (Any other verb...)

### Requirements

*
*

### Steps

1.
1.

### Notes

*
*
  </code>
</pre>


## Administration Manual Standards

The administration manual will contain everything in the user manual as well as
information on how to use the system from a super user's perspective. In most
cases this will cover configuration and user administration that other users do
not have access to. This may go into details on system installation, although
that particular could be a manual unto itself depending on the complexity of
the process.

## API / Developer Manual Standards

*   Description: There should always be a short statement describing the purpose
    of the web service.
*   Request
    *   Authentication: If the web service requires a form of authentication
        indicate that in the documentation so that consumers of the API know
        that their program may need an account/key/credential to consume the
        service.
    *   Headers & Parameters: Document the required and optional HTTP headers
        and parameters. Document whether the headers and parameters are in fact
        optional or not.
    *   Example Request: Include as many use cases as possible in example
        requests. If the service is a POST, PUT, or a request with a message
        associated, make sure to include an example of what that message looks
        like.
*   Response
    *   Returned Attributes: Include all of the possible attributes one might
        receive and their data type. Make sure to list whether the data is
        always returned or is optionally returned.
    *   Example Response: Include as many possible response cases as possible.

Here's a markdown template to be used when writing API documentation for RESTful
XML and JSON web services:

<pre>
  <code>
## Description



## Request

### Authentication

Authentication is not required for this service.

### Headers & Parameters

#### Headers

| Name                   | Format | Required? | Definition                                        |
| ---------------------- | ------ |:---------:| ------------------------------------------------- |
|                        |        | Y/N       |                                                   |

#### Parameters

| Name                   | Format | Required? | Definition                                        |
| ---------------------- | ------ |:---------:| ------------------------------------------------- |
|                        |        | Y/N       |                                                   |

### Example Request

**GET** URL


```json

```

```xml

```


## Response

### Returned Attributes

| Name                   | Format | Required? | Definition                                        |
| ---------------------- | ------ |:---------:| ------------------------------------------------- |
|                        |        | Y/N       |                                                   |

### Example Response

```json

```

```xml

```
  </code>
</pre>

## Source Documentation Standards

All methods and classes in our applications should be prefaced by RDoc
compatible documentation describing the intention of the class or method.

[RDoc Markup](http://docs.ruby-lang.org/en/2.3.0/RDoc/Markup.html)
