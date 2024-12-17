# DynamicRoutes

First we Make a folder in app named as blog/page.js

```jsx
import React from 'react'

const page = () => {
  return (
    <div>
      My post
    </div>
  )
}

export default page
```
Next we make another folder which is main for this use named as blog/[slug]/page.js
```jsx
export default function page({params}){
return <div>My post : {params.slug}</div>

}
```
with this approch we can simply write `http://localhost:3000/python or anything and we will get My post : python` or anything we write

We can define the routes as well.

```jsx
export default function page({ params }) {
    // Fetch your blogs by its slug

    let languages = ['python', 'javascript', 'java', 'c', 'rust']
    if (languages.includes(params.slug)){
        return <div>My post : {params.slug}</div>
    }
    else{
        return <div>{params.slug} not found</div>
    }
    return <div>My post : {params.slug}</div>
}
```
we made blogpost/[slug]/page.js where slug is a variable and will show all the variable we call this way we dont have to make multiple pages for any post we simply make a variable like this

# Custom404page

WE need to make another page in app named as not-found.js
```jsx
import Link from "next/link";

export default function NotFound(){
    return(
        <div>
            <h2>Not Found</h2>
            <p>could not Find Request resource</p>
            <Link href="/" >Return Home </Link>
        </div>
    )
}

```
This is how we customise the 404 page we have to make not-found.js and all this down below and we can style and customise the 404 page and return to anything else.


