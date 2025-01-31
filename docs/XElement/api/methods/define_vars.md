---
api_title: "define:vars"
page_title: "define:vars"
page_description: "Define your own client-sided variables from Astro's Server-side, be able to define values that would operate over the entire scope of the element"
---
# define:vars

This feature was a community requested feature for Astro. Which managed to make its way in by `v0.21`.

This powerful feature lets you pass variables from the Astro codeblock into the `<script>` by applying `<script define:vars={prop}>`.

This was extremely handy as alot of Astro works at build time generating your pages and assets. Here we tell it to at build time, to pass through a property coming from it parent and Astro will do so.

`XElement` leverages the same power to let you pass through variables of all manner, strings, numbers, arrays, objects, functions. Here we serialise the data that you are sending to the client so its safe and apply it to a special place within the overall scope of the `XElement`.

## How to use

```astro
---
    const {div:Box} = XElement
    const answer = {
        a:41,
        b:42,
        c:43
        }
    const sum = (x) => (y) => x + y
    const list = ['apple','banana','carrot']
    const dolphins = "So long, and thanks for all the fish!"
---
    <Box
        @define:vars={
            {
                answer,
                sum,
                list,
                dolphins
            }
        }
        @do={()=>{
            console.log("do ",answer.b)
        }}
        @visible={()=>{
            console.log('visible ',sum(4)(2))
        }}
        @resize={()=>{
            console.log('resized ', list)
        }}
        @click={()=>{
            console.log(`when clicked, the dolphins say: ${dolphins}`)
        }}
    ></Box>
```

With `define:vars` anything you pass through get *hoisted* to that elements module's block scope, basically it goes straight to the top, and can be called from any other method. But not outside of the element.

This lets you abstract away a lot of the code that you may write, by having it ready to Astro at build time, we can copy those variables and their values over into the module.

So you can do things like:

```astro
---
import FancyFunction from '../fancy.js'

const {div:Box}=XElement

const fancy = await FancyFunction

const msg = "So long and thanks for the fish!"

const answer = 42

const data = {
    a:2,
    b:{
        c:'3P0'
    }
}

const list = ['joy','happy','fun']
---

<Box 
    @define:vars={
        {
            fancy,
            msg,
            answer,
            data,
            list
        }
    }
    @do={()=>{
        console.log(fancy,data)
    }}
    @visible={()=>{
        console.log(msg)
    }}
    @resize={()=>{
        console.log(answer)
    }}
    @click={()=>{
        console.log(list)
    }}
>

```
