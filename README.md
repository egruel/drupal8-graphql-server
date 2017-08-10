# Drupal 8 graphQL blog public API

This site exposes a public API to test graphql with Drupal 8. This is simple fictive blog using "article" content type. Each article has a main image and tags. 

You can use it for free with your favorite front-end library with this endpoint :
http://dev-graphql-demo-blog.pantheonsite.io/graphql

Use explorer to create some graphQL queries
http://dev-graphql-demo-blog.pantheonsite.io/graphql/explorer

Explore the schema with : 
http://dev-graphql-demo-blog.pantheonsite.io/graphql/voyager

# Example queries :

## get 10 articles

```graphql
{
  nodeQuery(limit:10, offset:0, filter:{type:"article"}) {
    entities{
      entityLabel
      entityUrl {
        path
      }
      ...on NodeArticle {
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
    count
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

### modules
composer require drupal/graphQL
composer require drupal/pathauto

Enable all graphQL modules, except the mutations ones, unless you know what you are doing

### configuration

sites/default/services.yml :

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
