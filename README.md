# DigitalOcean Directus Hosting: How to Self-Host a Headless CMS on a Budget — Droplets vs App Platform, Pricing, Setup Steps, and Plan Comparison (With $200 Free Credit Guide)

If you've been poking around the headless CMS space lately, you've probably bumped into Directus. It's that open-source, SQL-first, API-driven content platform that developers seem to either love quietly or evangelize loudly. And if you're here, you're likely weighing one very specific question: is DigitalOcean a sensible place to host it?

The short answer is yes — and the longer answer is what this whole piece is about. We'll walk through why DigitalOcean and Directus pair well together, what it actually costs to run a Directus instance there, which deployment path makes sense for your situation, and how to stretch a fresh account's free credit as far as it'll go.

## Why People Pick Directus in the First Place

Before we talk infrastructure, let's be honest about what Directus is and isn't. It's not a hosted SaaS that holds your hand. It's a self-hostable headless CMS that wraps a real SQL database in a clean admin UI and auto-generates REST and GraphQL APIs on top of whatever schema you throw at it. That's the appeal — you keep your data in a database you actually own, and you get a polished content editing experience without writing a custom backend.

The trade-off, of course, is that "self-hostable" means you have to host it somewhere. That's where the cloud provider question comes in. Directus officially supports PostgreSQL, MySQL, SQLite, MS SQL, and CockroachDB as database backends, plus any S3-compatible storage for file uploads. It runs as a Node.js process, ideally behind a Redis cache if you're scaling beyond a single instance.

Minimum system requirements sit at 1x 0.25 vCPU with 512 MB RAM, but the Directus team's recommended minimum is 2x 1 vCPU with 2 GB RAM. Keep those numbers in mind — they'll matter when we look at plans.

## Why DigitalOcean Specifically for Directus

There are plenty of places to run a Node.js app with a Postgres database. So why does the "digitalocean directus hosting" search even exist as a thing people look up?

A few reasons, most of them practical:

- **Predictable pricing.** DigitalOcean's monthly caps and flat per-resource pricing mean you don't get surprise bills from bandwidth spikes. For a CMS backend that might sit mostly idle, that matters.
- **S3-compatible Spaces.** Directus needs external storage for file uploads when running on ephemeral infrastructure, and DigitalOcean Spaces is S3-compatible out of the box. No extra abstraction layer needed.
- **One-click Directus Droplet.** There's an official Directus listing in the DigitalOcean Marketplace, so you can spin up a preconfigured instance without writing a Dockerfile.
- **App Platform support.** Directus has a published deployment guide for the App Platform, which gives you a managed runtime while keeping your database and storage under your control.
- **Referral credit.** New accounts opened through a referral link get $200 in credit valid for 60 days — enough to run a mid-tier setup for free while you figure out your actual usage pattern.

That last point is the one that pulls a lot of people in. We'll come back to it.

## Three Ways to Run Directus on DigitalOcean

There isn't one "right" way. The choice depends on how much you want to manage yourself.

### Option 1: The One-Click Marketplace Droplet

The fastest path. From the DigitalOcean Marketplace, you can launch a Droplet with Directus preinstalled. You pick a plan, pick a region, and a few minutes later you have a running instance with an admin login screen.

This is the simplest option, but it comes with caveats. The one-click image bundles Directus with a local database (typically PostgreSQL), which is fine for a single-instance setup but makes backups and scaling harder. If you ever want to move to a managed database or add a second instance, you're doing surgery on a running system. Community threads on the DigitalOcean Q&A site are full of people asking how to extract or back up the database from a one-click install — which tells you something about its limitations.

Best for: personal projects, demos, learning the platform.

### Option 2: A Custom Droplet with Docker Compose

This is the route most self-hosters gravitate toward once they've outgrown the one-click image. You spin up a basic Droplet, install Docker, and run Directus plus a PostgreSQL container plus optionally a Redis container via a `docker-compose.yml` file.

You get full control — database version, backup strategy, environment variables, reverse proxy, SSL via Caddy or Traefik — but you also own every bit of operational responsibility. Security patches, OS updates, firewall rules, and monitoring are all on you.

Best for: people who are comfortable on a Linux command line and want maximum control at minimum cost.

### Option 3: App Platform with Managed Database and Spaces

This is the path Directus themselves document. You write a small Dockerfile (the official guide gives you the exact contents), push it to a GitHub or GitLab repo, and let the App Platform build and run it as a managed container. Your database lives in a DigitalOcean Managed Database cluster, your file uploads go to a Spaces bucket, and if you need multi-instance sync you add a Redis Droplet.

The upside is that you don't manage the runtime — DigitalOcean handles builds, deploys, autoscaling (on dedicated instances), and OS patching. The downside is cost: you're paying for the app container, the managed database, the Spaces bucket, and possibly Redis, all separately.

Best for: production deployments where you'd rather spend money than spend your Saturday debugging a Postgres upgrade.

## What It Actually Costs: DigitalOcean Plan Breakdown

Here's where the rubber meets the road. The table below covers every Droplet tier currently listed on DigitalOcean's pricing page, plus the App Platform container options. These are the resources you'd be choosing between when sizing a Directus deployment.

### Droplet Plans (All Tiers)

| Tier | Memory | vCPU | Transfer | SSD | $/hr | $/mo | Get Started |
| --- | --- | --- | --- | --- | --- | --- | --- |
| **Basic** | 512 MiB | 1 | 500 GiB | 10 GiB | $0.00595 | $4.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Basic** | 1 GiB | 1 | 1,000 GiB | 25 GiB | $0.00893 | $6.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Basic** | 2 GiB | 1 | 2,000 GiB | 50 GiB | $0.01786 | $12.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Basic** | 2 GiB | 2 | 3,000 GiB | 60 GiB | $0.02679 | $18.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Basic** | 4 GiB | 2 | 4,000 GiB | 80 GiB | $0.03571 | $24.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Basic** | 8 GiB | 4 | 5,000 GiB | 160 GiB | $0.07143 | $48.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Basic** | 16 GiB | 8 | 6,000 GiB | 320 GiB | $0.14286 | $96.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **CPU-Optimized** | 4 GiB | 2 | 4,000 GiB | 25 GiB | $0.06250 | $42.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **CPU-Optimized** | 8 GiB | 4 | 5,000 GiB | 50 GiB | $0.12500 | $84.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **CPU-Optimized** | 16 GiB | 8 | 6,000 GiB | 100 GiB | $0.25000 | $168.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **CPU-Optimized** | 32 GiB | 16 | 7,000 GiB | 200 GiB | $0.50000 | $336.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **CPU-Optimized** | 64 GiB | 32 | 9,000 GiB | 400 GiB | $1.00000 | $672.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **CPU-Optimized** | 96 GiB | 48 | 11,000 GiB | 600 GiB | $1.50000 | $1,008.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **General Purpose** | 8 GiB | 2 | 4,000 GiB | 25 GiB | $0.09375 | $63.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **General Purpose** | 16 GiB | 4 | 5,000 GiB | 50 GiB | $0.18750 | $126.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **General Purpose** | 32 GiB | 8 | 6,000 GiB | 100 GiB | $0.37500 | $252.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **General Purpose** | 64 GiB | 16 | 7,000 GiB | 200 GiB | $0.75000 | $504.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **General Purpose** | 128 GiB | 32 | 8,000 GiB | 400 GiB | $1.50000 | $1,008.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **General Purpose** | 160 GiB | 40 | 9,000 GiB | 500 GiB | $1.87500 | $1,260.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Memory-Optimized** | 16 GiB | 2 | 4,000 GiB | 50 GiB | $0.12500 | $84.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Memory-Optimized** | 32 GiB | 4 | 6,000 GiB | 100 GiB | $0.25000 | $168.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Memory-Optimized** | 64 GiB | 8 | 7,000 GiB | 200 GiB | $0.50000 | $336.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Memory-Optimized** | 128 GiB | 16 | 8,000 GiB | 400 GiB | $1.00000 | $672.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Memory-Optimized** | 192 GiB | 24 | 9,000 GiB | 600 GiB | $1.50000 | $1,008.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Memory-Optimized** | 256 GiB | 32 | 10,000 GiB | 800 GiB | $2.00000 | $1,344.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Storage-Optimized** | 16 GiB | 2 | 4,000 GiB | 300 GiB | $0.19494 | $131.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Storage-Optimized** | 32 GiB | 4 | 6,000 GiB | 600 GiB | $0.38988 | $262.00 | [Launch this Droplet](https://m.do.o/c/4aea30af3b73) |
| **Storage-Optimized** | 64 GiB | 8 | 7,000 GiB | 1,170 GiB | $0.77976 | $524.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Storage-Optimized** | 128 GiB | 16 | 8,000 GiB | 2,340 GiB | $1.55952 | $1,048.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Storage-Optimized** | 192 GiB | 24 | 9,000 GiB | 3,520 GiB | $2.33929 | $1,572.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |
| **Storage-Optimized** | 256 GiB | 32 | 10,000 GiB | 4,690 GiB | $3.11905 | $2,096.00 | [Launch this Droplet](https://bit.ly/DigitaLocean) |

### App Platform Container Plans

| CPU Type | vCPU | Memory | Transfer | Autoscaling | $/mo | Get Started |
| --- | --- | --- | --- | --- | --- | --- |
| Free tier | — | — | 1 GiB | No | $0 | [Deploy on App Platform](https://bit.ly/DigitaLocean) |
| Shared (Fixed) | 1 | 512 MiB | 50 GiB | No | $5.00 | [Deploy on App Platform](https://bit.ly/DigitaLocean) |
| Shared (Fixed) | 1 | 1 GiB | 100 GiB | No | $10.00 | [Deploy on App Platform](https://bit.ly/DigitaLocean) |
| Shared | 1 | 1 GiB | 150 GiB | No | $12.00 | [Deploy on App Platform](https://bit.ly/DigitaLocean) |
| Shared | 1 | 2 GiB | 200 GiB | No | $25.00 | [Deploy on App Platform](https://bit.ly/DigitaLocean) |
| Shared | 2 | 4 GiB | 250 GiB | No | $50.00 | [Deploy on App Platform](https://bit.ly/DigitaLocean) |

A couple of notes on the App Platform side: dedicated egress IPs run $25/month per app, a development database (512 MiB, PostgreSQL only, no backups) is $7/month, and outbound transfer over your allowance is $0.02/GiB. The free tier only covers static sites — running Directus itself requires a paid container.

## Sizing a Directus Deployment: Which Plan to Actually Pick

This is the part most pricing pages skip. Here's how the numbers above translate into real Directus decisions.

### For a personal project or a low-traffic content backend

The Basic $6/month Droplet (1 GiB RAM, 1 vCPU) is the floor for a usable single-instance Directus with a local PostgreSQL database. It's below Directus's recommended minimum, but it runs — you'll just feel it on the admin UI when you have a lot of relations loaded.

The Basic $12/month Droplet (2 GiB RAM, 1 vCPU) hits the recommended minimum and is the sweet spot for a hobby or small-business site. Pair it with the one-click Directus image and you're live in under ten minutes.

👉 [Start with the $12/month Basic Droplet](https://bit.ly/DigitaLocean)

### For a production site with moderate traffic

Step up to the Basic $24/month Droplet (4 GiB RAM, 2 vCPUs) and run Directus plus Postgres in separate Docker containers. Add a Spaces bucket for file uploads ($5/month for 250 GiB) and you've got a setup that handles real traffic without breaking a sweat.

If you'd rather not manage the runtime, the App Platform's $25/month shared container (1 vCPU, 2 GiB) plus a managed PostgreSQL database (starts around $15/month for the smallest dev-tier managed Postgres) gets you a similar footprint with less operational overhead.

👉 [Set up a production Directus deployment](https://bit.ly/DigitaLocean)

### For a multi-instance or high-traffic setup

Once you need horizontal scaling, you're looking at dedicated App Platform instances (which support autoscaling, unlike shared ones) or multiple Droplets behind a load balancer. You'll also need Redis for cross-instance sync — Directus doesn't currently support clustered Redis, so a single Redis Droplet is the way to go. A CPU-Optimized $42/month Droplet (4 GiB, 2 vCPUs) for the Directus app plus a small Basic Droplet for Redis is a reasonable starting point.

👉 [Build a scalable Directus cluster](https://bit.ly/DigitaLocean)

## The $200 Free Credit: How to Use It Well

New DigitalOcean accounts opened through a referral link get $200 in credit, valid for 60 days. That's not a marketing gimmick — it's genuinely useful for Directus hosting, because it lets you run a real production-tier setup for two months without paying anything.

Here's how to make the most of it:

1. **Sign up through the referral link.** The credit is auto-applied; no coupon code to fiddle with.
2. **Start with the App Platform path if you're new to ops.** A $25/month container plus a $15/month managed Postgres burns about $80 over 60 days — well under the credit ceiling, and you get to learn the platform without touching a firewall.
3. **Use the credit window to measure real usage.** Sixty days of real traffic data tells you far more than any sizing guide. Watch your CPU, memory, and database connection counts.
4. **Right-size before the credit runs out.** Once you know your actual numbers, drop down to the smallest plan that comfortably handles your peak load. Most Directus deployments end up much smaller than people initially assume.
5. **Set a billing alert.** DigitalOcean lets you set email alerts when spending crosses a threshold. Set one at $150 so you get a heads-up before the credit runs dry.

👉 [Claim the $200 credit and start deploying](https://bit.ly/DigitaLocean)

## A Quick Note on the Per-Second Billing Change

Worth flagging: as of January 1, 2026, DigitalOcean moved Droplets to per-second billing with a 60-second minimum. For Directus hosting this mostly matters if you're running batch jobs, automated tests, or spinning up short-lived preview instances — you're no longer paying for a full hour when you only use 90 seconds. For a long-running production Directus instance, the monthly cap still applies, so your bill doesn't change. But for development workflows where you stand up a Droplet, run migrations, and tear it down, the savings add up.

## Directus on DigitalOcean vs the Alternatives

It's fair to ask whether DigitalOcean is the right call compared to other self-hosting options. A few honest comparisons:

- **Versus a $5 VPS from a budget provider.** You can run Directus on a cheap KVM box, and many people do. What you give up is the Spaces S3 integration, the App Platform option, the managed database, and the predictable bandwidth pricing. For a hobby project that's fine. For anything where downtime costs you money, the extra few dollars a month for DigitalOcean's ecosystem pays for itself.
- **Versus Directus Cloud.** Directus offers a fully managed hosted version. It's the lowest-effort option — zero ops — but pricing scales with usage in a way that can get expensive fast for high-traffic projects. Self-hosting on DigitalOcean trades operational work for cost control and data ownership.
- **Versus AWS or GCP.** For comparable resources, DigitalOcean is generally cheaper at small-to-medium sizes, and the pricing is dramatically simpler. AWS and GCP win on raw service breadth and enterprise features. For a Directus backend serving a single site or a handful of sites, that breadth is overkill.
- **Versus other headless CMS options like Strapi or Payload.** This isn't really a hosting question — it's a platform question. Directus's SQL-first model and clean admin UI are the main differentiators. If you're already committed to Directus, the hosting calculus is what we've covered above.

## Common Pitfalls When Self-Hosting Directus on DigitalOcean

A few things that trip people up, gathered from community threads and the official deployment guide:

- **PM2 errors on the one-click image.** The Directus Docker image uses PM2, and `pidusage` (a PM2 dependency) doesn't play well with the `ps` implementation in the default image. The fix is building a custom image that adds `procps` and sets `PIDUSAGE_USE_PS=true`. The official guide includes the exact Dockerfile snippet.
- **Forgetting to set `PUBLIC_URL`.** If you don't set `PUBLIC_URL` to your app's URL (or custom domain), Directus generates incorrect asset URLs. On App Platform you can use `${APP_URL}` as the value.
- **Using the development database for production.** The $7/month development database on App Platform has no backups and is destroyed with the app. It's for development only. For real data, use a Managed Database.
- **Not configuring Spaces for file uploads.** On App Platform, the container filesystem is ephemeral. Any file uploaded to Directus without external storage configured disappears on the next deploy. Spaces is the fix.
- **Skipping Redis for multi-instance setups.** If you run more than one Directus instance, you need Redis for sync — otherwise each instance has its own cache and you get inconsistent behavior. Directus doesn't support clustered Redis yet, so a single Redis Droplet is the answer.

## A Sensible Default Setup for Most People

If you want a recommendation rather than a menu, here's a default that works for the majority of small-to-medium Directus deployments on DigitalOcean:

- **App Platform shared container, $25/month tier** (1 vCPU, 2 GiB RAM, 200 GiB transfer)
- **Managed PostgreSQL database**, smallest production size (around $15/month)
- **Spaces bucket for file storage**, $5/month for 250 GiB
- **No Redis** (single instance, so not needed)
- **Total: around $45/month**, comfortably under the $200 credit for the first 60 days

That gets you a managed runtime, a real backed-up database, durable file storage, and zero server maintenance. If your traffic grows to the point where you need autoscaling, you can move to a dedicated App Platform instance and add Redis without rearchitecting anything.

👉 [Build this default setup with $200 in credit](https://bit.ly/DigitaLocean)

## Final Thoughts

DigitalOcean isn't the only place to host Directus, but it's a particularly clean fit. The combination of predictable pricing, S3-compatible Spaces, a documented App Platform deployment path, and a one-click Marketplace image covers the full range from "I just want to try this" to "I need a scaled production backend." And the $200 referral credit gives you a real two-month runway to figure out your actual resource needs before you start paying.

If you're starting from scratch, the path of least resistance is the App Platform route with a managed database and Spaces — it's the setup Directus themselves document, and it removes the most common operational headaches. If you're comfortable on the command line and want to minimize cost, a Basic Droplet with Docker Compose gets you there for $12 to $24 a month.

Either way, the math works out: a real Directus deployment on DigitalOcean lands somewhere between $12 and $50 a month for most use cases, which is hard to beat for a headless CMS you fully own.

👉 [Open a DigitalOcean account and start hosting Directus](https://bit.ly/DigitaLocean)
