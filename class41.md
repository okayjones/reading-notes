# React IV

## Next.js - Deployment

[Source: Deploying Your Next.js App](https://nextjs.org/learn/basics/deploying-nextjs-app)

- `Vercel` - Deployment platform built by Next.js creators
- `DPS` - Vercel Develop, Preview, Ship" flow
- Statics are served from the Vercel Edge Network
- Server side rendering automatically isolated as serverless functions

1. Push Front End to Github
2. Import repo [https://vercel.com/new](https://vercel.com/new)
3. Give Vercel access
4. Wait for build
5. Preview URL
6. Deploy

> New PRs will show a 'preview deployment' link in git!

### Other hosting options

```json
// package.json
{
  "scripts": {
    "dev": "next",
    "build": "next build",
    "start": "next start"
  }
}
```

```bash
npm run build
npm run start
```
