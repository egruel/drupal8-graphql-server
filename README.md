# Drupal 8 graphQL public API

https://dev-graphql-demo-blog.pantheonsite.io/graphql exposes a free public graphQL API, to test graphQL module with Drupal 8. 

This is simple fictive blog using "article" content type. Each article has a main image and tags. 

Use "explorer" to test and explore graphQL queries
https://dev-graphql-demo-blog.pantheonsite.io/graphql/explorer

You may explore the schema with the voyager : 
https://dev-graphql-demo-blog.pantheonsite.io/graphql/voyager

# Example queries :

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

## installed modules and Drupal 8 configuration

### installed modules

- composer require drupal/graphQL
- composer require drupal/pathauto

Enable all graphQL modules, except the mutations ones, unless you know what you are doing

### configuration

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
