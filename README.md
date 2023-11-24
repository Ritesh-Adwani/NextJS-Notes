# NextJS-Notes

- Next.js is a popular open-source JavaScript framework that is used for building modern web applications, particularly single-page applications (SPAs) and server-rendered applications.
- It is built on top of React, which is a widely used JavaScript library for building user interfaces, and it adds features and capabilities to simplify the process of building web applications.
- Next.js is often used in combination with popular tools and libraries like TypeScript, Redux, and GraphQL to build robust web applications.
- It is known for its developer-friendly features and its ability to handle complex use cases while simplifying common development tasks.

## Some key features and concepts associated with Next.js:

- **1. Server-side rendering (SSR):** Next.js allows you to render React components on the server-side, which can improve performance, SEO, and user experience. This means that when a user visits a page, the initial rendering is done on the server and then the application becomes interactive on the client side.

- **2. Static site generation (SSG):** Next.js supports static site generation, which pre-renders pages at build time, allowing you to serve highly optimized, fast-loading pages to your users. This is particularly useful for content-heavy websites.

- **3. Routing:** Next.js provides a simple and flexible routing system that allows you to define your application's routes and handle navigation between pages.

- **4. Automatic code splitting:** Next.js automatically splits your JavaScript code into smaller chunks, so that only the necessary code is loaded for a particular page, improving performance and reducing load times.

- **5. API routes:** You can define API routes within your Next.js application, allowing you to build backend APIs and endpoints alongside your frontend code.

- **6. CSS support:** Next.js supports various methods for handling CSS, including CSS modules, styled-components, and more, giving you flexibility in how you style your components.

- **7. Hot module replacement (HMR):** Next.js provides HMR, which enables instant code changes without requiring a full page reload during development.

- **8. Extensibility:** Next.js is highly extensible, and you can add custom webpack configurations, middleware, and plugins to tailor it to your specific needs.

---

## Routing

- Routing in Next.js is designed to be simple and intuitive.
- It provides both file-based routing and dynamic routing, allowing you to define how your application handles different routes and URLs.
- Here's how routing works in Next.js:

**1. File-Based Routing:**

- Next.js follows a convention where the structure of your project's pages directory determines the routes for your application.
- For example, if you have a file named pages/about.js, it will be automatically associated with the /about route.
- Create a new page: To create a new route, simply create a new JavaScript or TypeScript file in the pages directory. The filename corresponds to the route path.
- Example:

```
pages/
├── index.js   // Accessible at "/"
├── about.js   // Accessible at "/about"
├── blog/
│   ├── index.js   // Accessible at "/blog"
│   ├── [slug].js  // Dynamic route accessible at "/blog/[slug]"
├── products/
│   ├── [category].js  // Dynamic route accessible at "/products/[category]"

```

- With this file-based routing, you don't need to set up explicit routing configurations. Next.js automatically handles it for you.

**2. Dynamic Routing:**

- Next.js also allows you to create dynamic routes using square brackets in your file names.
- For example, if you have a file named [slug].js inside a directory, it becomes a dynamic route, and you can access it with different values for slug.
- Example:

```
pages/
├── blog/
│   ├── [slug].js  // Accessible at "/blog/[any-slug]"

```

- You can access dynamic parameters in your page components using the useRouter hook provided by Next.js. For example:

```
import { useRouter } from 'next/router';

function BlogPost() {
  const router = useRouter();
  const { slug } = router.query;

  // Use the "slug" parameter in your component
  return <div>Blog Post: {slug}</div>;
}

```

- You can create nested dynamic routes to handle more complex routing scenarios.
- Here's an example of how to set up and work with nested dynamic routes:
  Suppose you want to create a blog section with nested categories and individual blog posts:

```
pages/
├── blog/
│   ├── [category].js
│   │   ├── [slug].js
│   └── index.js
```

- **pages/blog/index.js:** This page corresponds to the root of the blog section and is accessible at /blog.

- **pages/blog/[category].js:** This page handles different blog categories and is accessible at /blog/[category]. It expects a dynamic category parameter.

- **pages/blog/[category]/[slug].js:** This page handles individual blog posts within a category and is accessible at /blog/[category]/[slug]. It expects dynamic parameters for both category and slug.

**Catch-All routes:**

- In Next.js, you can create catch-all routes using the `[[...slug]].js` file format.
- Catch-all routes allow you to capture any number of path segments and use them as parameters in your route.
- This can be useful for creating dynamic and flexible routes where the number of segments in the URL may vary.
- Here's how to create a catch-all route in Next.js:

1. Create a file with the `[[...slug]].js` format in your `pages` directory. The `[[...slug]]` part specifies that it's a catch-all route.
   For example, create a file named `pages/[...slug].js`.

2. In your catch-all route component, you can access the captured segments as an array using the `useRouter` hook.

- Here's an example:

```
// pages/[...slug].js
import { useRouter } from 'next/router';

function CatchAllRoute() {
  const router = useRouter();
  const { slug } = router.query;

  return (
    <div>
      <h1>Catch-All Route</h1>
      <p>Segments: {slug.join('/')}</p>
    </div>
  );
}

export default CatchAllRoute;
```

- In this example, any URL that matches this route pattern, such as `/page/some/extra/segments`, will be captured as an array of segments in the `slug` parameter. You can then use this array to handle and display the content according to the captured URL segments.
- You can also use this catch-all route to create dynamic and flexible routing for various purposes, such as blog post slugs, product categories, or any other scenario where you want to capture variable path segments.

**Routing Summary:**

- 1.Page based routing mechanism: Pages are associated with a route based on their file name.
- 2.Nested Routes: Nested folder structure, files will be automatically routed in the same way in the URL.
- 3.Dynamic routes: Can be created by adding square brackets to a page name.
- 4.Catch all routes: Add three dots inside square brackets to create a catch all route. Helpful when you want different URLs for the same page layout or even when you're working with pages where some of the route parameters are optional.
- 5.Link component to navigate on click of an element.
- 6.useRouter hook's router.push() method to navigate programmatically.

---

## Linking and Navigating

- There are two ways to navigate between routes in Next.js:

1. Using the `<Link>` Component
2. Using the useRouter Hook

**1. Linking with `<Link>` Component:**

- The `<Link>` component is a convenient way to create links that handle client-side navigation without a full page refresh. Here's how to use it:

  ```
  import Link from 'next/link';

  function MyComponent() {
    return (
      <div>
        <Link href="/about">
          <a>About Us</a>
        </Link>
      </div>
    );
  }
  ```

- In this example, when a user clicks on the "About Us" link, the application will navigate to the `/about` page.

**2. Navigating with Programmatic Routing:**

- If you need to perform navigation in response to user actions or programmatically, you can use the `useRouter` hook from the `next/router` module. For example:

  ```
  import { useRouter } from 'next/router';

  function MyComponent() {
    const router = useRouter();

    const handleNavigation = () => {
      router.push('/about');
    };

    return (
      <div>
        <button onClick={handleNavigation}>Go to About</button>
      </div>
    );
  }
  ```

- In this code, when the button is clicked, it triggers a programmatic navigation to the `/about` page using `router.push`.

**3. Passing Parameters in Links:**

- You can also use the `<Link>` component to navigate to dynamic routes by passing parameters as part of the URL:

  ```
  import Link from 'next/link';

  function MyComponent() {
    const postId = 1;

    return (
      <div>
        <Link href="/blog/[id]" as={`/blog/${postId}`}>
          <a>Read Post</a>
        </Link>
      </div>
    );
  }
  ```

- In this example, the `as` property is used to specify the URL with the parameter value included.

- Using both `<Link>` components for declarative navigation and programmatic routing with the `useRouter` hook, you can create a smooth and user-friendly navigation experience in your Next.js application.

---

## Pre-rendering and Static Site Generation

- Pre-rendering and static site generation (SSG) are techniques in Next.js that help improve the performance, SEO, and user experience of web applications by generating HTML pages ahead of time, as opposed to rendering them on-the-fly for each request.

**1. Pre-rendering:**

- Pre-rendering is the process of generating HTML for a web page before it's requested by a user.
- Next JS generates HTML for each page in advance instead of having it all done by client-side Javascript.

**Why pre-render?**

1.  Improves Performance:

- In a React app, you need to wait for the JS to be executed
- Perhaps fetch data from an external API and then render the UI
- There is a wait time for the user
- With a pre-rendered page, the HTML is already generated and loads faster

2.  Helps with SEO:

- If you're building a blog or an e-commerce site, SEO is a concern
- With a react app, if the search engine hits your page, it only sees a div tag with id equal to root
- If search engine hits a pre-rendered page though, all the content is present in the source code which will help index that page
- If SEO is of concern for your app, pre-rendering is what you want

- There are two primary types of pre-rendering in Next.js:

  **a. Static Generation (SG):**

  - Static Generation is used for pages that can be pre-rendered at build time.
  - During the build process, Next.js generates HTML files for all possible page variations.
  - These generated HTML files are then served to users without needing to run any server-side code, resulting in fast and efficient performance.

  **b. Server-side Rendering (SSR):**

  - Server-side Rendering is used when a page's data can only be determined at runtime, like fetching data from an external API or a database.
  - The HTML is generated on the server for each request and sent to the client.
  - Server-side rendering allows for dynamic content while still providing SEO benefits and fast loading.

**2. Static Site Generation (SSG):**

- Static Site Generation is a specific form of pre-rendering that generates pages at build time.
- It's ideal for content-heavy websites where content doesn't change frequently. Here's how it works:

  - During the build process, Next.js fetches data and generates HTML pages for all possible routes.
  - These generated pages are saved as static HTML files, which can be cached and served quickly to users.
  - This approach is highly efficient and helps reduce server load, ensuring fast loading times.

- To use pre-rendering and static generation in Next.js, you can specify how a page should be rendered by setting the `getStaticProps` and `getStaticPaths` methods for static generation and the `getServerSideProps` method for server-side rendering in your page components.
- These methods allow you to fetch data, which is then used to generate the HTML for the page at build time or runtime.

- For example, to use static generation for a page:

```
export async function getStaticProps() {
  // Fetch data at build time
  const data = await fetchData();

  return {
    props: {
      data,
    },
  };
}

function MyPage({ data }) {
  return (
    // Use the fetched data in the page
  );
}

export default MyPage;
```

---

## getStaticProps:

1.  - getStaticProps runs only on the server side
    - The function will never run client-side
    - The code you write inside getStaticProps won't even be included in the js bundle that is sent to the browser

2.  - You can write server side code directly in getStaticProps
    - Accessing the file system using the fs module or querying a database can all be done inside getStaticProps
    - You also don't have to worry about including API keys in getstaticProps as that won't make it to the browser

3.  - getStaticProps function is allowed only in a page and cannot be run from a regular component file
    - It is used only for pre-rendering and not client-side data fetching

4.  - getStaticProps should return an object and the object
      should contain a props key which is in turn an object

5.  - getStaticProps will run at build time
    - During development getStaticProps runs on every request

- For pages that need dynamic routes, you can use `getStaticPaths` to specify the possible route parameters.
- In summary, Next.js offers various methods of pre-rendering (static generation and server-side rendering) to cater to different use cases, and you can choose the appropriate approach based on your application's requirements. Static Site Generation, in particular, is a powerful technique to improve web performance and SEO by generating pages at build time.

---

## getStaticPaths

- In Next.js, the `getStaticPaths` function is used when you're working with dynamic routes and static site generation (SSG).
- It allows you to specify which paths should be pre-rendered at build time.
- This is particularly useful when you have dynamic routes where the possible values are not known in advance.

- Here's how `getStaticPaths` is typically used:

**1. Create a Page with Dynamic Route Parameters:**

- First, you need to create a page with dynamic route parameters.
- For example, if you have a blog post page where each post has a unique slug, you might create a file named `[slug].js` inside a directory like `/pages/blog/`.

**2. Define `getStaticPaths` in the Page Component:**

- In the page component file, you define a `getStaticPaths` function.
- This function returns an object with an array of `paths`.
- Each item in the `paths` array represents a set of route parameters that you want to pre-render.

- Here's an example:

```
// pages/blog/[slug].js
import { getAllBlogPostSlugs, getBlogPostBySlug } from '../../lib/blogApi';

export async function getStaticPaths() {
  const slugs = await getAllBlogPostSlugs();

  const paths = slugs.map((slug) => ({
    params: { slug },
  }));

  return { paths, fallback: false };
}
```

- In this example, `getAllBlogPostSlugs` is a function that fetches all the slugs for your blog posts.
- You then map these slugs to an array of `params` objects.

**3. Use `getStaticProps` for Data Fetching:**

- You also need to define a `getStaticProps` function in the same page component to fetch data for each specific post using the provided `slug` parameter.

```
export async function getStaticProps({ params }) {
  const post = await getBlogPostBySlug(params.slug);
  return {
    props: { post },
  };
}
```

**4. Access Parameters in the Page Component:**

- Finally, in your page component, you can access the parameters using `params` in `getStaticProps` and use the data to render the page.

```
function BlogPost({ post }) {
  // Render the blog post using the data
}

export default BlogPost;
```

- With this setup, Next.js will generate HTML pages for all the slugs you specified in `getStaticPaths`.
- When a user accesses one of these paths, they will receive a pre-rendered HTML page for that specific post.
- The `fallback: false` option is mandatory.

---

## getStaticPaths fallback

- The fallback option can have three values: false,true or blocking.

**1. fallback:false**

- The paths returned from getStaticPaths will be rendered to HTML at build time by getStaticProps.
- If fallback is set to false, then any paths not returned by getStaticPaths will result in a 404 page.
- The false value is most suitable if you have an application with a small number of paths to pre-render.
- It is also suitable when new pages are not added often.
- A blog site with few articles is a good example for fallback set to false.

**2. fallback:true**

- The paths returned from getStaticPaths will be rendered to HTML at build time by getStaticProps.
- The paths that have not been generated at build time will not result in a 404 page. Instead Next.js will serve a "fallback" version of that page on the first request to such a path.
- In the background, Next.js will statically generate the requested path HTML and JSON. This includes running getStaticProps
- When that's done, the browser receives the JSON for the generated path. This will be used to automatically render the page
  with the required props. From the user's perspective, the page will be swapped from the fallback page to the full page.
- At the same time Next.js keeps track of the new list pre-rendered pages. Subsequent requests to the same path
  will serve the generated page, just like other pages pre-rendered at build time.
- The true value is most suitable if your app has a very large number of static pages that depend on data.
- For example: A large e-commerce site
  - You want all the product pages to be pre-rendered but if you have a few thousand products, builds can take a really long time.
  - You may statically generate a small subset of products that are popular and use fallback:true for the rest.
  - When someone requests a page that's not generated yet =, the user will see the page with a loading indicator.
  - Shortly after, getStaticProps finishes and the page will be rendered with the requested data. From then onwards, everyone who requests the same page will get the statically pre-rendered page.
  - This ensures that the user always have a fast experience while preserving fast builds and the benefits of Static Generation.

**3. fallback:blocking**

- The paths returned from getStaticPaths will be rendered to HTML at build time by getStaticProps.
- The paths that have not been generated at build time will not result in a 404 page. Instead, on the first request, Next.js will render the page on the server and return the generated HTML.
- When that's done, the browser receives the HTML for the generated path. From the user's perspective, it will transition from "the browser is requesting the page" to "the full page is loaded". There is no flash of loading/fallback state.
- At the same time, Next.js keeps track of the new list of pre-rendered pages. Subsequent requests to the same path will serve the generated page, just like other pages pre-rendered at build time.
- On a UX level, sometimes, people prefer the page to be loaded without a loading indicator if the wait time is a few milliseconds. This helps avoid the layout shift.
- The main reason fallback:blocking was introduced is because some crawlers did not support Javascript. The loading page would be rendered and then the full page would be loaded which was causing a problem.

---

## Incremental Static Generation

- Incremental Static Generation (ISG) is a feature in Next.js that enhances the process of static site generation (SSG).
- There are a couple of issues with SSG:
  1. The build time is proportional to the number of pages in the application.
  2. A page, once generated, can cntain stale data till the time you rebuild the application.
- With ISG, you can update specific pages on your website without regenerating the entire site.
- It's particularly useful when you have dynamic content that changes over time, but you still want to take advantage of the performance benefits of SSG.

Here's how Incremental Static Generation works in Next.js:

**1. Initial Static Generation:**

- When you build your Next.js site for the first time, all the static pages are generated.
- These pages are pre-rendered at build time, and they include the data available at that moment.

**2. Revalidation Interval:**

- For pages that use ISG, you can specify a revalidation interval (often referred to as `revalidate`) in the `getStaticProps` function.
- This interval determines how often Next.js should check for updates and potentially re-generate the page.

  ```
  export async function getStaticProps() {
    // Fetch data here
    return {
      props: {
        // Data
      },
      revalidate: 60, // Revalidate this page every 60 seconds
    };
  }
  ```

**3. User Access:**

- When a user accesses a page that uses ISG, they receive the pre-rendered HTML from the initial build, just like regular SSG.

**4. Revalidation**:

- After the specified revalidation interval has passed, Next.js will check if there are updates for that page.
- If there are updates (e.g., new data available), it will re-run the `getStaticProps` function and generate a new version of the page.

**5. Serving the New Version**:

- The new version of the page is stored, and the next user who accesses the page gets the updated content.
- This allows you to keep your site's content fresh without rebuilding the entire site.

![ISG](./images/ISG.png)

- ISG is particularly valuable for scenarios where you have content that doesn't change very frequently, but you still want to ensure that your users see the latest data when it does change.
- By specifying a revalidation interval, you can control how often Next.js checks for updates.
- Keep in mind that ISG is a powerful feature, but you should use it wisely and consider the trade-offs.
- Setting a very short revalidation interval can lead to frequent rebuilds and potential performance issues. It's essential to strike a balance between keeping content fresh and minimizing server load.
