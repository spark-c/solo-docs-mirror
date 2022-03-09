# Outline

There are three general steps to adding a new page — to help remember, I’ll use a little analogy! Imagine we’re starting a new restaurant, and we’re going to be serving a brand new dish!

1. First we need to build the page itself using React. We keep these files in the “/src/pages” directory.
    - *This is like coming up with the recipe for our new dish for the restaurant. This is what we’ll be serving the user.*

2. Next, we need to tell the application what URL (i.e. the page’s route) should lead to our new page. We define this in “/src/routes.js”, in the routes object.
    - *Kinda like coming up with a name for the new dish. What will the customer ask for, in order to get this item?*

3. Finally, we will integrate our new page into the application! React Router will handle the URL stuff; we just need to put the page in the correct place (i.e. in the HomePage.js file, within the `<Switch></Switch>` block!)
    - *Now we’re officially putting the new dish on the menu! We’ll teach the staff about it, and advertise it to our customers. When a customer asks for the dish, our staff knows what it is and how to serve it.*

# 1. Building the Page

We use React, a JavaScript front-end framework. Learning to use React is a little beyond the scope of this docs page — but below, I’ll show an example of a page that has a heading, some text content, and imports another pre-built component.

> *Note: I haven’t yet actually tested that the “Accordion” component was used correctly here. It’s probably broken. But hopefully it demonstrates the vague structure of things.*

```javascript
import React from 'react';
import Accordion from '../components/AccordionComponent.js';

const MyNewPage: React.FC = () => {

    return (
        // the below code looks like HTML... but it's not! It is called "JSX".
        <div className="my-new-page">
            <h1>Welcome to My New Page!</h1>
            <p>Thanks for being a Solo user! This means that you are the coolest.</p>
            <p>Here are two further reasons why you are great:</p>

            <Accordion defaultActiveKey="0">
                <Accordion.Item eventKey="0">
                    <Accordion.Header>Reason #1</Accordion.Header>
                    <Accordion.Body>
                        You like giraffes, which means you're a fun person.
                    </Accordion.Body>
                </Accordion.Item>
            // ... (another item)
            </Accordion>
        </div>
    );
};

export default MyNewPage;
```

This file will go in the "/src/pages" directory.

# 2. Defining its route

If you take a look at “/src/routes.js”, it looks something like this:

```javascript
export const Routes: Route = {
    // pages
    Presentation: { path: "/" },
    DashboardOverview: { path: "/dashboard/overview" },
    Transactions: { path: "/transactions" },
    Settings: { path: "/settings" },
    ...
};
```
To add your new page, just add its configuration (its path) to this object!
```javascript
    ...
    Transactions: { path: "/transactions" },
    Settings: { path: "/settings" },
    MyNewPage: { path: "/mynewpage" },
};
```

Be sure to check that you’ve added any necessary commas before/after your new line in the object!

# 3. Plugging into React Router

Okay! Now we need to actually make it accessible to the rest of the app. If we navigate to “/src/pages/HomePage.js”, we will see where a lot of our code comes together. This is the central hub!

## a. What is React Router (big-picture)?

In fact, the user *technically* never leaves this homepage. Each “page” is just a different component, and by clicking buttons and navigating the site, we’re just changing which set of components are rendered to the screen.

If we think back to our restaurant analogy, this is the customer’s table. They log in at the front, they are taken to their table, and now they may be working through several parts of a meal. At the beginning, they may have some bread. Then this is swapped out for a salad. Dishes are cleared and brought out to the table, even as some things (like condiments, salt/pepper, napkins, etc) might stay on the table at all times. (One example of this in our app is the Sidebar). But all-in-all, the user is at the same table while the components are **switched** around.

That’s what React Router helps manage... and it even uses a component called `<Switch>`. How neat!

## b. Okay, now let's *actually* plug it in

First, import your new page into HomePage.js

```javascript
// pages
import Presentation from "./Presentation";
import Upgrade from "./Upgrade";
import Users from "./Users";
...
import MyNewPage from "./MyNewPage";
```

Now we need to place our page into the React Router `<Switch>` component. You’ll find this in the `export default` statement. It looks like this:

```javascript
export default () => (
  <Switch>
    <RouteWithLoader exact path={Routes.Presentation.path} component={Presentation} />
    <RouteWithLoader exact path={Routes.Signin.path} component={Signin} />
    <RouteWithLoader exact path={Routes.Signup.path} component={Signup} />
    ...
    {/* Pages */}
    <RouteWithSidebar exact path={Routes.Users.path} component={Users} />
    ...
  </Switch>
);
```

You’ll mainly see two types of “Routes”; `<RouteWithLoader>`  and `<RouteWithSidebar>`. The difference between the two is that the latter includes the Sidebar component, and the former does not. (Both of them have the Loader; beyond the scope of this doc). For example, We don’t want to render the Sidebar on the login page or the 404 page, so we’d use the `<RouteWithLoader>` version.

The `path` attribute is just the route that we defined earlier (i.e. “/mynewpage”). The Routes object is already imported, so you can just pass in the config you defined. The `exact` keyword denotes that we only want to load the intended page if the user types *exactly* the correct URL.

The `component` attribute should be given a reference to our page/component — that’s what we imported at the top of this file.

Your new page might be configured like so:

```javascript
export default () => (
  <Switch>
    ...
    {/* Pages */}
    <RouteWithSidebar exact path={Routes.Users.path} component={Users} />
    <RouteWithSidebar exact path={Routes.MyNewPage.path} component={MyNewPage} />
    ...
  </Switch>
);
```

Once all of your changes are saved, your new page should be ready to visit! :)