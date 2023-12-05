---
layout: post
title: "Quick one: Running Cloudflare workers that send emails, locally"
subtitle:
date: 2023-12-05 21:00:00+10:00
cover:
cover_artist:
cover_link:
tags:
---

I've been playing around with Cloudflare workers recently, which is a serverless compute platform with a number of handy capabilities and a pretty decent free tier.

One of the things you can do with workers is to have them execute on a schedule defined by a cron expression. Unfortunately, if you'd like to get the logs for your
worker you'll either need to do a bit of work to set up [Logpush](https://developers.cloudflare.com/logs/get-started/) or be streaming the logs via CLI or in the
console at the time your worker is running.

For my purposes, I didn't want to have to actively check logs to figure out what my worker was doing, but wanted to be occasionally notified under certain
circumstances. I figured that sending an email from the worker in those circumstances would be a better approach, and simple to set up, it's a first-class feature
within workers, just check out [the docs](https://developers.cloudflare.com/email-routing/email-workers/send-email-workers/).

When I've been developing my worker I've been running it locally using `wrangler`, as is set up when generating a project following the instruction in the
[quickstart guide](https://developers.cloudflare.com/workers/get-started/quickstarts/). I don't deploy my worker to test every change, that sounds absurd to me.

When attempting to test the email sending capability locally I ran in to the following error;

```
service core:user:my-project: Uncaught Error: No such module "cloudflare-internal:email".
  imported from "cloudflare:email"
```

You will get this error even if you're not triggering the code to send the email as the `workerd` runtime tries to start up. The fix for this wound up being pretty
simple, but I couldn't find it documented anywhere, so here it is. **Don't load that module if you don't have to**.

```typescript
// Within a function that I call to send an email
const { EmailMessage } = await import('cloudflare:email');

const emailMessage: EmailMessage = new EmailMessage(
  'from@example.com',
  'to@example.com',
  msg.asRaw()
);
```

This on its own isn't enough however, because if you run this code locally it will still fail. You'll need to work out what to do when you'd _like_ to send an
email, I just print to console locally and ensure that the `await import('cloudflare:email')` is only executed in a deployed environment.

I have the following in my `wrangler.toml`;

```
[env.local]
vars = { ENVIRONMENT = "local" }
```

I updated the `dev` script in `package.json` to start up using that environment;

```
"dev": "wrangler dev --env local"
```

Inside my `index.ts` is an `interface` that defines what will be passed to the handlers at runtime, and one of the things that will be passed in is that new
`ENVIRONMENT` variable.

```typescript
export interface Env {
  // Learn more about setting up Bindings here; https://developers.cloudflare.com/workers/configuration/bindings/#email-bindings
  NOTIFICATION_EMAIL: SendEmail;
  ENVIRONMENT: 'local' | 'staging' | 'production';
}
```

Now when I run my worker locally I can check the `env.ENVIRONMENT` property that is passed to my handler, and do something like (but more complicated than) the
following;

```typescript
if(env.ENVIRONMENT === 'local') {
  console.log('Would send email\n' + emailBody)
} else {
  // Code that would run in a deploy environment
  const { EmailMessage } = await import('cloudflare:email');
  ...
}
```
