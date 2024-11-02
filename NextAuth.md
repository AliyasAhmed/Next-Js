# What is NextAuth.js?

NextAuth.js is an authentication library specifically designed for Next.js apps. It simplifies the process of allowing users to log in using providers like GitHub, Google, etc.

Instead of building custom authentication, NextAuth handles most of the heavy lifting, including handling sessions and connecting to external providers.

# Setting Up

npm
`npm install next-auth`

**Make Route for signup Page `api/auth/[...nextauth]/route.js` API Route Setup: The api/auth/[...nextauth]/route.js file tells NextAuth to use GitHub as a login provider.**

Inside this Route we set-up for `Github Provider`
```js
import NextAuth from "next-auth";
import GithubProvider from "next-auth/providers/github"

const handler = NextAuth({
    providers: [
        GithubProvider({
            clientId: process.env.GITHUB_ID,
            clientSecret: process.env.GITHUB_SECRET,
        }),
    ]
})

export {handler as GET, handler as POST}
```
In this providers we only have github `Authentication` we can add more like signIn with `Facebook`, `Google` etc.

### GITHUB_ID and GITHUB_SECRET are used to authenticate for signup but the `id` and `secret` are the pair of keys which are generated from github for the individual Developer in my case go to `github/settings/developerOptions/auth` There we have to register app and make secret key and paste it in the env file that we make.

# .env.local

OAuth credentials are stored in .env.local to allow GitHub login. Keys and secret will be provided from the env we created.

# Page Components
```js
"use client"
import { useSession, signIn, signOut } from "next-auth/react";

export default function Component(){
  const {data:session} = useSession()
  if(session){
    return<>
      signed in as {session.user.email}<br/>  
      <button onClick={()=> signOut()}>signOut</button>   
    </>
  }
  return <>
  Not signed in <br />
  <button onClick={()=> signIn("github")}>signIn</button>  
  </>
}
```

1. `useSession()` is a NextAuth hook that gives us access to the current session data. =>

In simple terms, `useSession()` is like a special tool provided by NextAuth that lets us check if a user is currently logged in or not. 

When you call `useSession()` in your component, it gives you information about the current user's session (or "login state"). This includes details like whether they’re logged in, their email, and other user info if they are. If no one is logged in, it just tells you, "No user is signed in."

So, it’s an easy way to see if a user is logged in and to get their info, all with just one line.

2. `data: session` holds the user’s session info if they’re logged in, or null if they’re not.


When you use `data: session`, you're getting the user's session information in a nice, organized way:

- **If the user is logged in**: `session` will hold details about them, like their email, name, and any other info NextAuth fetched from the login provider (like GitHub).
- **If the user is not logged in**: `session` will be `null`, which just means "no session info available."

So, `data: session` basically acts like a box that either has user info inside (if they're signed in) or is empty (if they're not). This lets your code easily check if there's a user logged in and respond accordingly.

4. If `session` exists, it means the user is logged in. We show their email and a Sign Out button.
4 . If `session` is `null`, the user is not signed in, so we show a Sign In button.

When the user clicks Sign In, they’re redirected to GitHub to log in. After logging in, they’re redirected back to the app, and `session` is now populated with their info.

Clicking Sign Out will clear the session, making session return to null.

# Summary of How It All Works Together
1. GitHub Setup: OAuth credentials are stored in .env.local to allow GitHub login.
2. API Route Setup: The api/auth/[...nextauth]/route.js file tells NextAuth to use GitHub as a login provider.
3. Session Provider: SessionProvider in SessionWrapper makes session data accessible throughout the app.
4. Sign-In Component: The main component checks if the user is signed in and displays either their email with a Sign Out button or a Sign In button.

With this setup, NextAuth handles the details of signing in, redirecting, and managing user sessions. You just need to add the necessary components and let NextAuth handle the authentication flow.
