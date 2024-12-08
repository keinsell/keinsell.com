---
title: "Three ORMs come to Node.js"
description: "One was bloat, second one had no types, and third one is not even a ORM at all."
date: "Jan 01 2024"
---

Hey fellow Node.js resource wasters, today we will be complaining about ORMs (or rather a lack of them) and how you should think twice before choosing one.

If you are a Node.js developer, you probably have heard of ORMs, and you probably have used them. ORMs are tools that help you to interact with databases. They are not a replacement for writing SQL queries, but they can be a great help when you need to perform complex queries or when you want to abstract away the database layer.

What the case with them then? Well... Let's say you have two options in terms of ORMs: those which are based on schema-first approach and those which are based on code-first approach (which lead to severe mental illness over time). A common thing that our beautiful developers do is marketing over their libraries, and obiviously - they cannot tell you what's wrong with library before you actually use it (like... things that are not supported yet requested by the community), and that's why we are here.

I will be focusing on prisma instead code-first ORMs like TypeORM or MikroORM as you should avoid these ones as far as you are sane, I do not hate them by existance as their existance is not toxic, but it's pretty common to see later on that developers cannot resist the urge to put serialized strings into character columns, which later leads to parsing these strings without validation, and eventually some retard that will change type of data and will not remember to update the migration file and we end up in a place where whole application is fundamentally definition of undefined behavior. They also strongly rely on reflection, which do not provide any transpile-time safety, which in my opinion is unacceptable for 2024 and beyond. Additionally, I had bad experiences with migration toolkits that are provided along these pretty orms which wiped one of my databases (hopefully, wise man had a backup), a lot of these tools do not provide introspection capabilities, so you end up manually managing migrations like it would have been some ancient age. I just... do not like them and I think my reasoning for this (covered in blood, tears and alcohol) is reasonable.

When you are starting a new project, you cannot really predict what feature of ORM you will need as project grows, so let's say prisma looks good for your project, it have founding, maintainers,and community - pretty solid choice you thought...

But then you have a problem, you suddenly realize your application needs to store some geospital data with postgis extension, and you are nicely said... fucked. You are supposed to use raw SQL queries and manually type cast for whole database table for every interaction that would include column with geospatial data, while fellow TypeORM/MikroORM may have support for this, prisma does not and apperently didn't give a fuck about it for a long time.

So okay, let's say you can live with that, writing raw SQL queries for one table - it's not big deal at all, so let's say you want some polymorphic relationships... keep dreaming. Union types? No. PostgreSQL CREATE TYPE? No. Recursive types? No. Okay... so maybe just Soft Deletes? Nah, no native support yet you can do it with some extension clusterfuck. Issues like these were on a table for a long time, and it all stands still probably to this day.

However, you can enjoy launch of Prisma Accelerate. Yes, they'll happily boost your query performance, why you would need polymorphic relationships when you can have blazing fast select queries on simplest possible tables? Crazy right?

Let's get over it for a while, the only solution I have found for this like this one is to actually either use raw SQL queries with manual or no typing at all, or to introduce another library (what I'm doing with my life...) that will provide any kind of DSL that would integrate with TypeScript types and eventually will support extensions, and other complex features that prisma does not (of course it's just the matter of time, huh?). Eventually I'm happy both prisma and drizzle are fighting over introspection capabilities which allows for easy adaption of these tools (like... TypeORM require you to write models by hand, and pray to allah you actually didn't make a mistake somewhere, as you most likely find it on production or in your tests if you have any)

I have been using prisma since `1.x` versions where I could clearly say it's complete fucking garbage, but now I would just say it's just a piece of garbage and it's becoming better - it's still the only choice feels reliable for a project in TypeScript ecosystem, it's quite nice all-in-one solution that will have your back for a quite long time, and if it's not enough you can always look into adding query builder along prisma, which should help you with unsupported features or maybe just complex queries that are specific to your project.

Prisma started promising era of ORMs, the ones that are schema-first and disallow developers to store fucking garbage markup in database instead typing it, it's the good direction (beside prisma there was EdgeDB when it supported PostgreSQL but they managed to ruin potential killer in ORMs in favour of writing database from scratch). Prisma have ecosystem of tools not related to prisma itself, like Prisma Studio which is a visual tool for ones that could not afford to write SQL or get themself Datagrip license, a lot of code generators for different frameworks which makes Prisma a bit more like a Hasura or at least speeds up boring and repeative tasks of rewriting prisma models into typescript classes, and so on.

What I'm trying to say is... I have been in too many projects where TypeORM was the only considered option even through I do not know single person who would have positive opinion about it (those who were recommending it to me apperently were the ones who later been asking me how to do migration in this thing, so I not consider them even a programmers at this point). New wave of people are coming to the scene, complaing how bloated Prisma is - while one is writing application in Node.js runtime which make it bloated just by runtime, and another is complaining about how it's not a good fit for their projec while never have been using it at all.

ORMs are not a silver bullet, they are awesome or they are complete dogshit - often it's nothing in between. I'm not saying that you should not use them, but you should be aware of their limitations and be prepared to use other tools if you are not satisfied with ORMs. As far I know and been in Node.js ecosystem for four years at the time of writing this, for most of your usecases you should just be fine with Prisma, eventually you will be able to switch to something else if you need to with potentially low-cost as prisma have code generation tools that may help you from ditching it out, while I didn't seen such tools for other ORMs so far (drizzle may be first to have thing like this but I haven't seen it yet).

I hope this article will help you to make a decision about ORMs, be responsible for choice of your technology as sometimes starting project with TypeORM may be more expensive and unnecessarily painful than one may thought.

Here is a lot of things that are not supported by prisma just to highlight you of what you should be aware of (there's actually a lot of them, those are just my favorite ones, or ones that I'm aware of):

- `Prisma` do not support `CREATE_TYPE` of `postgres` (https://github.com/prisma/prisma/issues/4263)
- `Prisma` do not support `Union` Types which are useful for Polymorphic Relationships (https://github.com/prisma/prisma/issues/2505)
- `Prisma` do not support Polymorphic Relationships (https://github.com/prisma/prisma/issues/1644)
- `Prisma` do not support `groupBy` for queries (https://github.com/prisma/prisma/issues/6653)
- `Prisma` do not support recursive relationships (https://github.com/prisma/prisma/issues/3725)
- `Prisma` do not support soft deletions (https://github.com/prisma/prisma/issues/3398)
- `Prisma` do not support Row-Level Security (RLS) (https://github.com/prisma/prisma/issues/12735)

[_This rant was orignally made as joke repository to track progression of prisma issues_](https://github.com/keinsell/is-prisma-production-ready)

GL;HF
