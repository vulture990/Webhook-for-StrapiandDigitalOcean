# Custom webhook to trigger build on DigitalOcean using Strapi
## When you  update, create or delete something in our backend using Strapi ,it has to rebuild the static website to see the Changes .

### A Custom Webhook to trigger the build comes handy for this type of tasks.

#### After Reading digitalocean's documentation on webhooks i found that the url we need to make the request call to has this format :

https://api.digitalocean.com/v2/apps/YOUR_APP_ID/deployments


## To Create Our Custom Webhook that depends on our models in Strapi  , So let's say for instance  we wanna trigger a rebuild after each post to see changes this how our webhook is going to be 
#### First we need to go to our api endpoint models (in this case i'm talking about Strapi) and i want to rebuild after creating , deleting or updating a post, so i will go to /api/posts/models/posts.js

#### We are going to use Strapi lifecycles hooks, afterCreate,afterUpdate,afterDestroy...

#### Lastly,We also need to create a Token / Key in digitalocean how: (go to your digitalocean account  ->   api  ->  Tokens/Keys)

####  put the token in a .env file as call it for example POSTS_BUILD_TOKEN

#### then install axios npm i axios

#### Our /api/posts/models/posts.js should look like this:
```js
const axios = require('axios')

const token = process.env.POSTS_BUILD_TOKEN

module.exports =  {

    lifecycles: {

    async afterCreate()  {

        axios({
            method: 'post',
            headers: {
                'Content-Type': 'json',
                'Authorization': `Bearer ${token}`
            },
            url: 'https://api.digitalocean.com/v2/apps/${token}/deployments',
           body:{
               "force_build":true
            }
          });
      },


    }
};
```
