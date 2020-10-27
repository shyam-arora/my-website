---
id: token-based-auth
title: The Ins and Outs of Token Based Authentication
sidebar_label: Token Based Auth
---

## The main reasons for tokens are:

- Stateless and scalable servers
- Mobile application ready
- Pass authentication to other applications
- Extra security

### How Token Based Works

Token based authentication is stateless. We are not storing any information about our user on the server or in a session.

**No session information means your application can scale and add more machines as necessary without worrying about where a user is logged in.**

Although this implementation can vary, the gist of it is as follows:

1. User Requests Access with Username / Password
2. Application validates credentials
3. Application provides a signed token to the client
4. Client stores that token and sends it along with every request
5. Server verifies token and responds with data

# What are JSON Web Tokens?

- JSON Web Tokens (JWT), pronounced "jot", are a standard since the information they carry is transmitted via JSON.
- JSON Web Tokens work across different programming languages
- **JWTs are self-contained**: They will carry all the information necessary within itself. This means that a JWT will be able to transmit basic information about itself, a payload (usually user information), and a signature.
- A JWT is easy to identify. It is three strings separated by ' . '
  aaaaaaaaaa.bbbbbbbbbbb.cccccccccccc

  Since there are 3 parts separated by a ' . ', each section is created differently. We have the 3 parts which are:

  1. header: The header carries 2 parts:

     - declaring the type, which is JWT
     - the hashing algorithm to use (HMAC SHA256 in this case)

  2. payload: The payload will carry the bulk of our JWT, also called the JWT Claims. This is where we will put the information that we want to transmit and other information about our token. (expiry etc)

  3. signature: This signature is made up of a hash of the following components: The secret is the signature held by the server. This is the way that our server will be able to verify existing tokens and sign new ones.

  ```
  var encodedString = base64UrlEncode(header) + "." + base64UrlEncode(payload);
  HMACSHA256(encodedString, 'secret');
  ```

# Authenticate a Node ES6 API with JSON Web Tokens

## Overview

We'll begin by:

1. Setting up our development environment and initializing our express server.
2. Creating our first basic route and controller.
3. Fleshing out our routes and controllers to add users and login users.
4. Creating a route and controller that will handle getting all users.

Finally, we'll

1. Add middleware to protect our get users route by requiring a user to be an admin and to have a valid token.
2. Validate that only an admin with a token can access the protected route.

## Dependencies

1. **body-parser**: This will add all the information we pass to the API to the request.body object.
2. **bcrypt**: We'll use this to hash our passwords before we save them our database.
3. **dotenv**: We'll use this to load all the environment variables we keep secret in our .env file.
4. **jsonwebtoken**: This will be used to sign and verify JSON web tokens.
5. **mongoose**: We'll use this to interface with our mongo database.

Generate JWT

```javascript
const payload = { user: user.name };
const options = { expiresIn: "2d", issuer: "https://scotch.io" };
const secret = process.env.JWT_SECRET;
const token = jwt.sign(payload, secret, options);
```

Verify JWT

```javascript

    const token = req.headers.authorization.split(' ')[1]; // Bearer <token>
      const options = {
        expiresIn: '2d',
        issuer: 'https://scotch.io'
      };
      try {
        // verify makes sure that the token hasn't expired and has been issued by us
        result = jwt.verify(token, process.env.JWT_SECRET, options);

```

An express middleware function is a function that gets triggered when a route pattern is matched in our request uri. All middleware have access to the request and response objects and can call the next()function to pass execution onto the subsequent middleware function.

## References

- [The Ins and Outs of Token Based Authentication
  ](https://scotch.io/bar-talk/the-ins-and-outs-of-token-based-authentication)
- [The Anatomy of a JSON Web Token
  ](https://scotch.io/tutorials/the-anatomy-of-a-json-web-token)
