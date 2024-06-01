# B9A12-Battle-For-Supremacy

## [ Private Repo Link ( Client)](https://classroom.github.com/a/BBJEe3MV)

## [ Private Repo Link (Server) ](https://classroom.github.com/a/yyMxUX7W)

## 🚩: Updates
#### 📢if we bring any changes or update any of the assignment requirements we will mention them here. Ensure that you check this file twice a day during the assignment. We will mention the variant. 
```
0: No updates yet
--------------------------------------
*01-06-2024 11:40am updates:-🕦*
1: updated category 0022: removed the query features.
2: updated some typing and grammatical errors in category 0019. please redownload
--------------------------------------

```
## What To Submit?
- Your assignment ID/Variant (The name of the pdf requirement file. Watch video for more)
- Your client-side code GitHub repository
- Your server-side code GitHub repository
- Your live website link
## Server Deployment steps

1. comment await commands outside api methods for solving gateway timeout error

```js
//comment following commands
await client.connect();
await client.db("admin").command({ ping: 1 });
```

2. create vercel.json file for configuring server

```json
{
  "version": 2,
  "builds": [
    {
      "src": "index.js",
      "use": "@vercel/node"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "index.js",
      "methods": ["GET", "POST", "PUT", "PATCH", "DELETE", "OPTIONS"]
    }
  ]
}
```

3. Add Your production domains to your cors configuration. Don't use the URL we have provided inside origin. Replace them with your own. 

```js
//Must remove "/" from your production URL
app.use(
  cors({
    origin: [
      "http://localhost:5173",
      "https://cardoctor-bd.web.app",
      "https://cardoctor-bd.firebaseapp.com",
    ]
  })
);
```

4. Deploy to Vercel

```bash
vercel
vercel --prod
```
- After completed the deployment . click on inspect link and copy the production domain
- setup your environment variables in vercel
- check your public API


<img src="code.jpg"/>

# Server Deployment Done
## If you are using a cookie, follow this extra process. We recommend using local storage to store tokens on the client side to avoid deployment issues.
1. Let's create cookie options for both the production and local server

```js
const cookieOptions = {
  httpOnly: true,
  secure: process.env.NODE_ENV === "production",
  sameSite: process.env.NODE_ENV === "production" ? "none" : "strict",
};
//localhost:5000 and localhost:5173 are treated as same site.  so sameSite value must be strict in the development server.  in production, sameSite will be none
// in development server secure will false.  in production secure will be true
```

## now we can use this object for the cookie option to modify cookies

```js
//creating Token
app.post("/jwt", logger, async (req, res) => {
  const user = req.body;
  console.log("user for token", user);
  const token = jwt.sign(user, process.env.ACCESS_TOKEN_SECRET);

  res.cookie("token", token, cookieOptions).send({ success: true });
});

//clearing Token
app.post("/logout", async (req, res) => {
  const user = req.body;
  console.log("logging out", user);
  res
    .clearCookie("token", { ...cookieOptions, maxAge: 0 })
    .send({ success: true });
});
```
