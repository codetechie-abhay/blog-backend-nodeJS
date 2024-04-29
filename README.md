
# 🚀 blog-backend-nodeJS

This is a backend server written in Node.js using Express for a blogging website.

## Technologies Used

- **Express**: A fast, unopinionated, minimalist web framework for Node.js.
- **Mongoose**: Elegant MongoDB object modeling for Node.js.
- **dotenv**: Zero-dependency module that loads environment variables from a .env file.
- **cookie-parser**: Parse Cookie header and populate req.cookies with an object keyed by the cookie names.

## Setup

1. Clone the repository:
   ```
   git clone https://github.com/codetechie-abhay/blog-backend-nodeJS.git
   ```

2. Install dependencies:
   ```
   npm install
   ```

3. Create a `.env` file in the root directory and add your MongoDB URI:
   ```
   MONGODB_URI=<your-mongodb-uri>
   ```

4. Start the server:
   ```
   npm start
   ```

## Usage

- **Authentication**: This server uses JWT authentication. Ensure you have the correct authentication token set in your cookies.
- **Routes**:
  - `/user`: Routes for user authentication and management.
  - `/blog`: Routes for managing blog posts.

## Quick Start

```bash
require("dotenv").config();

const path = require("path");
const express = require("express");
const mongoose = require("mongoose");
const cookieParser = require("cookie-parser");

const Blog = require("./models/blog");

const userRoute = require("./routes/user");
const blogRoute = require("./routes/blog");

const {
  checkForAuthenticationCookie,
} = require("./middlewares/authentication");

const app = express();
const PORT = process.env.PORT || 8000;

mongoose
  .connect(process.env.MONGODB_URI)
  .then(() => console.log("MongoDB Connected"))
  .catch((err) => console.error("MongoDB connection error:", err));

app.set("view engine", "ejs");
app.set("views", path.resolve("./views"));

app.use(express.urlencoded({ extended: false }));
app.use(cookieParser());
app.use(checkForAuthenticationCookie("token"));
app.use(express.static(path.resolve("./public")));

app.get("/", async (req, res) => {
  const allBlogs = await Blog.find({});
  res.render("home", {
    user: req.user,
    blogs: allBlogs,
  });
});

app.use("/user", userRoute);
app.use("/blog", blogRoute);

app.listen(PORT, () => console.log(`Server Started at http://localhost:${PORT}`));
```
