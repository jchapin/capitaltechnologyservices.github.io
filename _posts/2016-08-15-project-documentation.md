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
    into their products. This is produced in addition to product documentation
    written in the source code of the product.
1.  Source Documentation - Documentation integrated into the source code of
    the product.

## User Manual Standards

## Administration Manual Standards

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

<pre><code>
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
</code></pre>

## Product Documentation Standards

