[The 6 year old Github issue](https://github.com/microsoft/TypeScript/issues/3628) 

[The ridiculous list of projects working around this issue](#the-ridiculous-list-of-projects-working-around-this-issue)

Dear TS team,  

I love you.  You do amazing work.  You are gods among mortals.  You have brought JavaScript from the darkness, and given it the warm light of strong typing.   Look upon us, the cowering meek masses, and understand that we live in the muck and mire of a world where we are doomed to endlessly crawl on our bellies through the alleys of Github [seeking the one true npm](https://youtu.be/deDlab6vFgg?t=133).  We will forever wonder in the dark, until we have a type reflection model.  

Serialization and validation without a reflective type system just doesn't work in the real world without endless boilerplate, or _shiver_, bespoke code generation from a schema file.  The best solutions I could find are [io-ts](https://github.com/gcanti/io-ts), [zod](https://github.com/colinhacks/zod), [myzod](https://github.com/davidmdm/myzod), etc.  [See below for the ever growing list](#the-ridiculous-list-of-projects-working-around-this-issue)  The most courageous of us even resort to babel, but only to face the consequences.  We have to declare our types in a round about library-bespoke way that is foreign to the uninitiated, and the libraries aren't even able to support all the wonderful type magic you work so hard to provide.  

This is a case, where we urge you to take a step back and holistically look at what TypeScript users have to do to in the majority of modern projects, and list them out in priority order.  The decision of how to tackle serialization, and what actually has to be done to have TypeScript still work with it is at the top of the list.  Even if you still don't want to support reflection, please write a detailed doc, and put it in the official TypeScript documentation that explains how TypeScript users should tackle this problem or what library you recommend, so we don't have endlessly reinvent the wheel.

Second, and maybe even more importantly is type based dispatch.  Redux actions need a discriminator property such as ```type: 'incrementCounter'``` Without runtime type information for the string literal, we resort to having to have the interface as a generic argument, and the string literal for the discriminator explicitly declared over and over.  The moment we rename the type, the string literal discriminator needs to be renamed too, and the tools can't deal with it for us.  The official Redux Toolkit's [createAction](https://redux-toolkit.js.org/usage/usage-with-typescript#createaction) does the best it can, but then their concept of "matchers" are now everywhere in your code base.  This is because it's impossible to make a map of discriminator value to a TypeScript type to perform the downcast that you need for any message based dispatch map.  Please add a way to automatically declare a property on an interface with the discriminator set to a string literal, and a casting map to disambiguate.

There seems to be a conflict between the design goals of TypeScript that is blocking this for some reason.  Type erasure is good!  It means JavaScript project can consume TypeScript projects without any knowledge of TypeScript.  It's just emitted JavaScript.  This does not mean you can't emit the type information separately in a consumable lookup table that's  separate from the code.  The lack of this type information means we use esoteric libraries which ultimately pollute the JavaScript with all the convoluted typing working arounds... soo... type erasure has defeated the purpose of type erasure.  It's a second order effect, where the design goal defeats the design goal. :(  

Side note, please don't solve this with decorators.  A lot of us want to use interfaces.  Decorators, like C# attributes are so coupling, and we can't add them to types from other libraries.  A higher order function that's known to the compiler like: ```typescript.generateRuntimeType<T>()``` with options for discriminators etc. means it would even work with external libs.  Something like F# Type Providers, or C# Source Generators would also be welcome. 

Last thing.  If you do this, I will send you cake.  If it's in TypeScript 4.3, there will be ice-cream.  

Love, 
A humble disciple

ATTN: @ahejlsberg 

### The Ridiculous List of Projects Working Around this Issue

Please send a PR to add to this list.

- [io-ts](https://github.com/gcanti/io-ts)
- [zod](https://github.com/colinhacks/zod)
- [typescript-schema](https://github.com/christyharagan/ts-schema)
- [ts-morph](https://github.com/dsherret/ts-morph)
- [myzod](https://github.com/davidmdm/myzod)
- [ts-di-transformer](https://github.com/YePpHa/ts-di-transformer)
- [reflect-metadata](https://rbuckton.github.io/reflect-metadata/)

#### Projects that would benefit.  

- https://github.com/redux-orm/redux-orm
