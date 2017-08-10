# Drupal 8 graphql blog public API

This site exposes a public API to test graphql with Drupal 8. This is simple blog using "article" content type. Each article has a main image and tags.

Use explorer to create some graphQL queries
http://dev-graphql-demo-blog.pantheonsite.io/graphql/explorer

Explore the schema with : 
http://dev-graphql-demo-blog.pantheonsite.io/graphql/voyager

# Example queries :

## get all articles

```
{
  nodeQuery(filter:{type:"article"}) {
    entities{
      entityLabel
      entityUrl {
        path
      }
      ...on NodeArticle {
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
    count
  }
}
```

## get one article by its "path" field :

```
{
  route(path:"/node/72") {
    alias
    routed
    path
    entity {
      entityUuid
      entityBundle
      entityId
      entityLabel
      entityType
      entityUrl {
        alias
        routed
        path
      }
    }
  }
}

