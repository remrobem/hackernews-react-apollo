# Notes

## Start the prisma server and playground (localhost:4000)

    cd server
    yarn install
    yarn prisma deploy
    yarn start

## Start the app (localhost:3000)

    cd < project folder >
    yarn start

## Libraries used

### Client

    yarn add
        apollo-boost
        react-apollo
        graphql

## Server directory structure

This app uses free AWS service from Prisma
<https://us1.prisma.sh/rob-martin/server/dev>

### prisma

- This directory holds all the files that relate to your Prisma setup.
- The Prisma client is used to access the database in your GraphQL resolvers (similar to an ORM).

### prisma.yml

- the root configuration file for your Prisma project.

### datamodel.prisma

- defines your data model in the GraphQL Schema Definition Language (SDL).
- When using Prisma, the datamodel is used to describe the database schema.

### src

- This directory holds the source files for your GraphQL server.

### schema.graphql

- Contains your application schema.
- The application schema defines the GraphQL operations you can send from the frontend.
- Used by the frontend developer to get the schema layout

### generated/prisma-client

- Contains the auto-generated Prisma client, a type-safe database access library (similar to an ORM).

### resolvers

- Contains the resolver functions for the operations defined in the application schema.

### index.js

- The entry point for your GraphQL server.

## Architectures

3 different kinds of architectures that include a GraphQL server:

- GraphQL server with a connected database
- GraphQL server that is a thin layer in front of a number of third party or legacy systems and integrates them through a single GraphQL API
- A hybrid approach of a connected database and third party or legacy systems that can all be accessed through the same GraphQL API

## Resolvers

- Server side
- The payload of a GraphQL query (or mutation) consists of a set of fields.
- In the GraphQL server implementation, each of these fields actually corresponds to exactly one function that’s called a resolver.
- The sole purpose of a resolver function is to fetch the data for its field.

## Apollo Client

- Apollo abstracts away all lower-level networking logic and provides a nice interface to the GraphQL server.
- In contrast to working with REST APIs, you don’t have to deal with constructing your own HTTP requests any more
- Instead, you can simply write queries and mutations and send them using an ApolloClient instance.
- Client defines the endpoint of your GraphQL API so it can deal with the network connections.
- See /src/index.js
- Below for simple case

    ```javascript
    import { ApolloProvider } from 'react-apollo'
    import { ApolloClient } from 'apollo-client'
    import { createHttpLink } from 'apollo-link-http'
    import { InMemoryCache } from 'apollo-cache-inmemory'


    // 2
    const httpLink = createHttpLink({
    uri: 'http://localhost:4000'
    })

    // 3
    const client = new ApolloClient({
    link: httpLink,
    cache: new InMemoryCache()
    })

    // 4
    ReactDOM.render(
    <ApolloProvider client={client}>
        <App />
    </ApolloProvider>,
    document.getElementById('root')
    )
    serviceWorker.unregister();
    ```

## Dev Steps

1. Create Components
    - example of 2 components

    ```javascript
    import React, { Component } from 'react'

    class Link extends Component {
        render() {
            return (
                <div>
                    <div>
                        {this.props.link.description} ({this.props.link.url})
                    </div>
                </div>
            )
        }
    }

    export default Link
    ```

    ```javascript
    import React, { Component } from 'react'
    import Link from './Link'

    class LinkList extends Component {
        render() {
            const linksToRender = [
            {
                id: '1',
                description: 'Prisma turns your database into a GraphQL API 😎',
                url: 'https://www.prismagraphql.com',
            },
            {
                id: '2',
                description: 'The best GraphQL client',
                url: 'https://www.apollographql.com/docs/react/',
            },
            ]

            return (
            <div>{linksToRender.map(link => <Link key={link.id} link={link} />)}</div>
            )
        }
    }

    export default LinkList
    ```

1. Basic setup in App.js to render own app

    ```javascript
    import React, { Component } from 'react'
    import LinkList from './LinkList'

    class App extends Component {
        render() {
            return <LinkList />
        }
    }

    export default App
    
```