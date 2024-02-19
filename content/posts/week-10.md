---
title: Week 10 - Deployment
date: 2024-02-12
author: James
---

This week, there were two main tasks for our team:

1. [Work Orders](#work-orders)
2. [Deployment](#deployment)

# Work Orders

As defined by our client, a Work Order is a record that stores
information about a job involving a particular type of machine. In
particular, for our interests, it defines:

1. The machine being worked on (i.e. Machine Name, Machine Model);
2. The engineer on the job (i.e. User ID).

Our application relies on Work Orders to further contextualize the
conversation by only pulling in the documents that really matter to
the job at hand.

To this end, we had four main sub-tasks:

1. The Uploader interface needed a way to upload a document tied to a
   specific machine;
2. The Uploader interface, for the purposes of demonstration, should
   be able to create / delete work orders
3. CoreAPI needs to provide any connected interfaces access to all
   Work Orders owned by the user
4. The Web App interface needs to fetch all Work Orders

In our system, Work Orders are akin to individual conversations found
in other chatbot + LLM applications like ChatGPT. Each Work Order
defines one conversation.

We then defined the schema, using the `sqlalchemy` ORM package so that
we don't have to deal with the database directly:

``` python
class WorkOrderModel(Base):
    __tablename__ = "work_orders"
    order_id: Mapped[str] = mapped_column(
        String(36), primary_key=True, default=lambda: str(uuid4())
    )
    user_id: Mapped[str] = mapped_column(
        String(36)
    )  # Assuming user_id is string from clerk
    conversation_id: Mapped[str] = mapped_column(
        String(36), default=lambda: str(uuid4())
    )
    machine_id: Mapped[str] = mapped_column(
        String(36), ForeignKey("machines.machine_id")
    )
    task_name: Mapped[str] = mapped_column(String(255))
    task_desc: Mapped[str] = mapped_column(Text)
    created_at: Mapped[DateTime] = mapped_column(
        DateTime, default=func.now()
    )  # Automatically sets to current time

    machine = relationship("MachineModel", back_populates="work_orders")


class MachineModel(Base):
    __tablename__ = "machines"
    machine_id: Mapped[str] = mapped_column(
        String(36), primary_key=True, default=lambda: str(uuid4())
    )
    manufacturer: Mapped[str] = mapped_column(String(255))
    model: Mapped[str] = mapped_column(String(255))

    work_orders = relationship("WorkOrderModel", back_populates="machine")
```

Here, each Work Order owns a Machine, while each Machine is associated
to many Work Orders.

Here are some screenshots of the resultant handiwork:

![Uploader Interface](images/2024-02-19_01.png)

![WebApp Interface](images/2024-02-19_02.png)

# Deployment

For the past few weeks, we have been doing our best to migrate
endpoints and keys to the server-side so that we can publicly deploy
the frontends.

The CoreAPI was already deployed to begin with, given it's serverless
nature; however, the frontends had never seen the light of the day.

The URLs are not made public for obvious reasons, but we are proud to
announce that we have successfully deployed the two applications to
the public.

Right now, our deployment pipeline looks like this:

![GitHub Actions](images/2024-02-19_03.png)

On push to `main`, this GitHub Actions will trigger, which builds the
frontend and packages them. We found that the default GitHub Actions
workflow file was not sufficient for our use; hence, we modified it so
that it worked for our use case.

In particular, we first had to modify our NextJS build outputs to
`standalone`:

``` javascript
/** @type {import('next').NextConfig} */
const nextConfig = {
  reactStrictMode: true,
  output: "standalone"
};

module.exports = nextConfig;
```

Then, we had to modify the job for GitHub actions to also package our
static resources:

``` yaml
      - name: npm install and build
        env:
          NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY: ${{ secrets.NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY }}
          CLERK_SECRET_KEY: ${{ secrets.CLERK_SECRET_KEY }}
        run: |
          npm install
          npm run build
          mkdir -p .next/standalone/.next
          cp -r .next/static .next/standalone/.next/

      - name: Zip artifact for deployment
        run: |
          cd .next/standalone/
          zip ../../release.zip ./* .next -r
          cd -
```

At the same time, we also had to pass the
`NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY` secret so that Clerk could be
properly built for production deployment.

After which, we had to do a bunch of Azure changes, namely:
1. In Core API, we had to adjust our `AZP_LIST` for Clerk (the
   accepted domains that Clerk works on) to include our new deployed
   domains;
2. Create new Web Apps on Azure to support both the uploader and the
   webapp. We experimented with Static Web Apps with their [preview hybrid
   Next.JS
   support](https://learn.microsoft.com/en-us/azure/static-web-apps/deploy-nextjs-hybrid),
   but it did not properly support the authentication middleware used
   by Clerk; hence, we decided to go for Web Apps instead. In terms of
   cost, Static Web Apps would have been less expensive; hence, once
   the feature comes out of preview, we will advise the client to move
   to Static Web Apps
3. Create firewall rules on our Azure SQL Database to allow connection
   from any Azure resource, and also the IP address of the Uploader
   Web App.
