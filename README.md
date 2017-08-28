# Drupal 8 graphQL public API [ WORK IN PROGRESS ]

This is a very work in progress, feel free to open PR 

https://dev-graphql-demo-blog.pantheonsite.io/graphql exposes a free public graphQL API, to test graphQL module with Drupal 8. 

This is simple fictive blog using "article" content type. Each article has a main image and tags. 

Use "explorer" to test and explore graphQL queries
https://dev-graphql-demo-blog.pantheonsite.io/graphql/explorer

You may explore the schema with the voyager : 
https://dev-graphql-demo-blog.pantheonsite.io/graphql/voyager

Table of contents
=================
  * [GraphQL example queries](#example-queries)
    * [Get ten articles](#get-10-articles)
    * [Get one article by its path field](#get-one-article-by-its-path-field-)
  * [Send graphQL queries to the server](#send-graphql-queries-to-the-server)
  * [How to create a graphql api server with Drupal 8](#how-to-create-a-graphql-api-server-with-drupal-8)

# Example queries

You can tests for real thoses queries here : https://dev-graphql-demo-blog.pantheonsite.io/graphql/explorer

## get 10 articles

```graphql
{
  nodeQuery(limit:10, offset:0, filter:{type:"article"}) {
    count
    entities{
      entityLabel
      entityUrl {
        path
      }
      ...on NodeArticle {
        body
        fieldImage {
          derivative(style:thumbnail) {
            url
          }
        }
        fieldTags {
          entityLabel
        }
      }
    }
  }
}
```

## get one article by its "path" field :

```graphql
{
  route(path: "/cogo-fere-odio-uxor") {
    alias
    routed
    path
    entity {
      entityLabel
      ... on NodeArticle {
        body
        fieldImage {
          derivative(style:large) {
            url
          }
        }
        fieldTags {
          entityLabel
        }
      }
    }
  }
}
```

## Send graphQL queries to the server

You may use an Apollo library with React, Vue.js, Nuxt.js, Angular etc .. 

You may test graphQL endpoint without any graphQL library, see this example :

https://runkit.com/nyl-auster/simple-graphql-http-example

## Example consumer :

https://drupal8-graphql-demo-blog-nuxt.now.sh/

## How to create a graphql api server with Drupal 8

### Installed modules

- composer require drupal/graphQL
- composer require drupal/pathauto ( create nice urls from our title in "alias" node field)
- composer require drupal/snail ( protect drupal native path from being accidentally overriden by our front-end aliases )

### Configuration

Enable all graphQL modules, except the mutations ones, unless you know what you are doing
Then in back-office, expose entities as graphQL api at /admin/config/graphql/content
Don't forget to attach the fields to a display mode, for example, the default one.
You can then use display mode to format the output of graphQL

Then go to */admin/people/permissions* and check *Execute arbitrary GraphQL requests*; if you want your application to request your API without authentication.

We know have to enable cors
in *sites/default/services.yml :*

```yml
  cors.config:
    enabled: true
    # Specify allowed headers, like 'x-allowed-header'.
    allowedHeaders: ['x-csrf-token', 'authorization', 'content-type', 'accept', 'origin', 'x-requested-with']
    # Specify allowed request methods, specify ['*'] to allow all possible ones.
    allowedMethods: ['*']
    # Configure requests allowed from specific origins.
    allowedOrigins: ['*']
    # Sets the Access-Control-Expose-Headers header.
    exposedHeaders: false
    # Sets the Access-Control-Max-Age header.
    maxAge: false
    # Sets the Access-Control-Allow-Credentials header.
    supportsCredentials: false
```
