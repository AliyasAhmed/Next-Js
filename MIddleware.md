# **Middleware**

### Next.js middleware is mainly used to run code before a request is completed. It lets you handle tasks like authentication, logging, or redirecting users **before** they reach the actual page or API route.

Here’s why it’s useful:

1. **Authentication**: You can use middleware to check if a user is logged in. If they aren’t, you can redirect them to a login page before they access a protected route.
   
2. **Logging**: Middleware can track and log user activity, such as what pages are visited or any actions taken, without placing this logic on every page.

3. **Redirection**: Middleware can automatically redirect users based on specific conditions, like sending mobile users to a mobile version of your site.

4. **Security**: It adds a layer where you can check for unwanted behaviors or protect sensitive parts of your application.

Essentially, middleware acts as a filter for incoming requests. It allows you to control how your app responds based on what the request contains. It runs **before** the actual page logic executes.s

Yes, that's exactly one of the main use cases of middleware in Next.js! Middleware allows you to **redirect users** before they access a specific page. For example, if a user tries to visit the `/home` page, and they are not logged in, you can use middleware to **redirect them to a login page**.

In short, middleware acts **before** rendering the actual page, so it can check if the user is authenticated and, if not, redirect them to the `/login` page.

So yes, middleware is used for this type of functionality!

```js
component/Navbar.js

import Link from "next/link";
export default function home2() {

  return (
    <div  >
      <nav className="flex justify-start gap-10 p-10 ">
      <Link href='/about'>about</Link>
      <Link href='/home'>Home</Link>
      <Link href='/Contact'>Contact</Link>

      </nav>
    </div>
  );
}

```

```js
app/home/page.js

const Home = () => {
  return (
    <div>
      This is home
    </div>
  )
}

export default Home

```

```js
app/Contact/page.js

const Contact = () => {
  return (
    <div>
      This is contact
    </div>
  )
}

export default Contact
```

```js
app/about/page.js
const About = () => {
  return (
    <div>
      THis is about
    </div>
  )
}

export default About
```

### Now we make a `Middleware.js` in outside of the app and we use it to redirect the user to the page we want it can be anypage. lets say user is trying to access `About` page and whenever he try to access that specific page we redirect him to anyother page.

This is the basic example of middleware Whenever we try to access any other page we will recieve middleware message.
```js
export const middleware = () =>{
    console.log("middleware") //this will show middleware whenever we try to load any of the page this will be the middle content that will show before going on to the actual page.
}
```
### now if we want user to first login then access any other page we can use middleware to show him login page first to login and then get the access of the page he was looking for.



```js
import { NextResponse } from "next/server" //next response is responsible for redirecting to the page we desire

export const middleware = (req) =>{
    return NextResponse.redirect(new URL('/about',req.url)) //this will put an error to the website by redirecting to about multiple time
}

```
#### Simple solution is to make it redirect to about page if we click on about otherwise no need to redirect it.

```js
import { NextResponse } from "next/server" 

export const middleware = (req) =>{
    if(req.nextUrl.pathname!="/about"){

        return NextResponse.redirect(new URL('/about',req.url)) 
    }
}

```
This will redirect it to the about page when we click on about page else it wont redirect it again and again.

## Matcher

```js
import { NextResponse } from "next/server" 

export const middleware = (req) =>{
    // if(req.nextUrl.pathname!="/about"){

        return NextResponse.redirect(new URL('/about',req.url)) 
    // }
}

export const config ={ // When we do this it means whenever user try to access contact he will be redirected to the about page other the contact he will be able to aceess everyother page
    matcher:'/contact/:path*'
    // matcher:['/contact/:path*, /home/:path'] // THis is for making multiple matcher
}
```
