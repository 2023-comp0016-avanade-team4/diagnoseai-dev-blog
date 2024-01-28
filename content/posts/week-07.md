---
title: Week 7 - Authentication
date: 2024-01-19
author: James
---

This week, the team implemented authentication with Clerk, in an
effort to be internet-ready by early February. This will be a key
milestone to deploy the project safely.

# Background

This project was created for Avanade's client companies, hence, there
are a myriad of authentication systems that they may use.

It is hence reasoned (and confirmed with their engineers) that we can
leverage any authentication system we wanted, because it will be
modified to the individual client's needs.

Hence, there shouldn't be a focus on building a nice-looking UI,
because chances are, the software will be integrated into an existing
SSO system, like Microsoft Entra, or Keycloak.

# Choices

We were allowed (by our client) to choose between many authentication
systems, namely:

- Clerk (which we ended up with)
- NextAuth
- Supabase
- Auth0

To consider this, we had the following criteria:

1. Easy to integrate with CoreAPI
2. Guards all resources with relatively minimal effort
3. Comes with built-in UI components

## Criteria 1: Easy to integrate with CoreAPI

CoreAPI, being serverless, must be able to authenticate the user as
quickly as possible, with minimal effort. When it comes to security,
we like to reduce our attack surface by relying on a more well-funded,
and battle-tested third party.

As an additional consideration factor, CoreAPI is written in Python;
hence, the authentication method must be easily implemented in Python.

All four options fulfilled this criteria pretty easily:

1. Clerk uses JWT tokens, which can be easily verified with a public
   key
2. NextAuth is essentially an adapter over many authentication
   services, and Clerk is one of them; hence, verification is more or
   less the same as Clerk
3. Supabase provides a Python SDK to obtain the user session, which
   implies verification
4. Auth0 has a Python SDK that seems to center around Flask, but
   internally uses JWT, which hence makes verification the same as
   NextAuth

## Criteria 2: Guards all resources with relatively minimal effort

All four authentication systems have native `Next.js` components
support, which makes integrating with the two frontends relatively
simple.

With JWT support, guarding CoreAPI endpoints can be done via a
primitive method of introducing a guard clause written as a utility
function.

## Criteria 3: Built-in UI components

Clerk, NextAuth and Auth0 comes with their own UI components.

Of the three, Clerk comes built-in with the most UI components,
including:
- A default, unbranded login / signup screen (NextAuth and Auth0 has
  this as well)
- A `UserButton`, shown in a later screenshot

Based on [the
Supabase](https://supabase.com/docs/guides/auth/auth-helpers/nextjs)
docs, it appears that we would have to create the authentication UI on
our own. Furthermore, it seems that the developer would have to manage
the authentication cookies (by default, authentication data is stored
in Local Storage); while this isn't complex and a deal-breaker, it
would be nice if library did this on their own.

## Bonus Criteria: Simplicity

This means that we now have to perform a 3-way tie-breaker between
Clerk, NextAuth and Auth0.

NextAuth is the most flexible authentication framework, as it can use
many providers, _including_ Clerk. However, it does not provide the
same UI components as Clerk does (`UserButton`, in particular), and
requires almost no configuration, other than setting it up as a
provider and middleware.

Clerk is the simplest authentication framework, and is what NextAuth
calls a _Hosted Auth_. This means all user information, including
avatar URLs, usernames and passwords are stored on Clerk, washing our
hands of the responsibility.

Auth0 functions similarly to Clerk, minus the nice components we get
for free.

Hence, we chose to use Clerk.

# Result

We now have authentication for the dashboard, chatting interface, and
backend!

The following image shows the `UserButton` component on our Uploader
interface:

![Authentication User Button](images/2024-01-28_11-35.png)

And the following shows the CoreAPI utility function to verify the JWT
token:

![CoreAPI Authentication](images/2024-01-28_11-37.png)

Clerk is currently on allow-list mode, meaning no new sign-ups can
take place. This will allow us to deploy a demonstration safely onto
the internet.

# Next Steps

An important step is also _authorization_, which we have been reminded
is distinct from _authentication_. For now, this is not seen as a
priority, because the software is still relatively simple.

Our next step is to move vectors from the validation index to the
production index, so that the knowledge base has access to all
production documents during an interaction.
