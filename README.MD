# Build a Blog Using Django, GraphQL, and Vue (Explanation)

The Blog uses Django as the backend. It specifically handles the administrative interface of the blog. It also contains the models of the blog data, which are just Python classes.

The Blog uses Graphene-Django to serialize the model (authors,posts,tags) instances to JSON, which are subsequently used to send to Vue. 

GraphQL uses a schema to define the types of data and the operation that it does to the data. It runs on localhost 8000 and depending on the query output we put in we can receive the data of all the posts, tags, authors created in the administrative interface of the blog. This is because it allows nested fields in their queries. It acts as an intermediary, fetching the data from the backend (django) and then communicating it to frontend (Vue.js, specifically Vue Apollo (who then tells Vue.js)) so it can render it.

Vue.js is a front end framework and it is used to build the UI of the blog. In the blog it automatically updates the DOM with new posts. It also is responsible for the different views of posts and manages the navigation between these different views in the router folder. It also uses Vue Apollo to fetch the JSON data from Graph QL. It runs on localhost:5173 and uses npm to begin running.

## Fetching Blog Posts:
- A Vue component makes a GraphQL query using Apollo Client to fetch a list of posts.
- The GraphQL API receives the query and resolves it using the Django models.
- The data is returned to the Vue component, which renders the posts in the UI.
## Creating a New Post:
- An admin user creates a new post using the Django admin interface.
- The post is saved in the database and is now available through the GraphQL API.
- The Vue frontend can fetch and display this new post through a GraphQL query.

## Modifications 

I was having issues with having the actual fields using vue.js render. From what I understand it had to do with how the tutorial was defining useQuery. It was doing so in a manner that wasn't allowing it to reactively respond to changes in variables that made me have to move/change it to the following:

<code>  const { result, loading, error } = useQuery(ALL_POSTS_QUERY);</code>

Other than that I tried to both create a new type of post model where you could post an image and I also tried to add into it as an option in the regular post model. I was unsucessful with implementing both. I also tried to follow some tutorials to create a search bar to search the posts, tags and author. I again was unsucessful with fufilling this. My main reasons for failure was that I would encounter some sort of issue with either implmenting the image in django or its functionality would cause the rest of the blog to fall apart. I was able to modify the CSS styles of the project though, however I still am attempting to make it look more visually appealing.

