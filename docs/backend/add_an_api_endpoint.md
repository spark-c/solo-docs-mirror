# As a quick overview, this is what it looks like when Flas receives a web request:

1. A web request is sent from the client to some URL on the webserver (Flask app).
2. Flask routes this request to the appropriate function, denoted by the `@app.route(URL)` decorator
    - For example, a request being sent to `https://localhost:8000/debug/hello` will be received and handled by a route/function that looks like this:
    ```python
    @app.route("/debug/hello")
    def say_hello():
        return "hello"
    ```

3. That function runs, and at the end, should return some Response.
    - Flask takes care of pretty much all of the heavy lifting in creating responses! You can simply return a string or dict and Flask will handle the rest behind the scenes; alternatively for more complex objects, you can `return jsonify(my_complex_object)` (assuming that the object is JSON-Serializable).
    - For more details, see [here](https://flask.palletsprojects.com/en/2.0.x/quickstart/#about-responses) in the documentation!

Here is a very basic example of an endpoint that just returns the message “Hello, world”:

```python
@app.route("/return_hello_world", methods=["GET"])
def return_hello_world() -> str:
  return "Hello, world"
```

Then the client side receives that response, complete with appropriate status code, headers, body, etc.!

Now, let’s actually receive some data and do something with it. We’ll also return a dict, just to see the syntax.

> *Note: flask’s “request” object is a global variable that represents the client’s request. Within our function, we access all of the necessary information through this object. Below, we get the request body by accessing the request.json property (which is the json representation of the request, i.e. a Python Dict).*

```python
from flask import request

@app.route("/add_one", methods=["POST"])
def add_one() -> Dict[str, str]:
  number: int = int(request.json["body"])
  number += 1
  return {"message": str(number)}
```

Here is the flow for the above code:
1. Client sends a request with a number to increment as the payload
2. Access that payload via the request.json property
3. Increment the number
4. Return a response with the new value

> *This endpoint is present (in a slightly modified form) in our debug routes! Try it out by going to localhost:3000/#/apitest , selecting the “add_one” endpoint, and sending an integer in the body.*


# Here is a quick step-by-step guide to adding an endpoint

1. Begin with a route decorator.
    - ENDPOINT: This is a string that shows the path to this route. e.g. localhost:3000/debug/hello looks like:
        - `”/debug/hello” `
    - METHODS: This is a kwarg; list of strings that defines what HTTP methods are allowed for this route. e.g.:
        - `METHODS=[”GET”, “POST”]`
    ```python
    @app.route(ENDPOINT, METHODS)
    ```

2. Add your function declaration. The name can match the route, but this is not required.

    ```python
    @app.route(ENDPOINT, METHODS)
    def say_hello() -> str:
    ```

3. Do your work in the function; if you’re accessing any data from the request, be sure to `from flask import request`. Your data will likely be on the `request.json` property, which is a dict. e.g. `request.json[”foo”] == “bar”`.

4. Returns from this function will be turned into a request by Flask itself. You can return just a string, or a dict, or anything that is JSON serializable! For more information, see [here](https://flask.palletsprojects.com/en/2.0.x/quickstart/#about-responses).

A finished endpoint might look something like this:

```python
@app.route("/debug/my_endpoint", methods=["POST"])
def my_endpoint() -> Dict[str, str]
    if request.json is not None:
        result: str = do_lots_of_work(request.json["data"])
        return {"message": result }
    else:
        raise Exception("Request had a bad argument")
```

# How to handle endpoints that accept multiple HTTP methods

When your endpoint accepts multiple methods i.e. `@app.route(”/my_route”, methods=[”GET”, “POST”])` , you can check within your function using the flask `request` object.

Imagine that you are on a Login page. You got there by clicking a “Login” button, which made a GET request to the server, which then sent you the HTML/CSS/JavaScript and whatnot.

When you fill out the form on the Login page, the page makes a POST request to the /login endpoint with the form information. Here is how the /login endpoint might handle accepting either method:

```python
from flask import request

@app.route("/login", methods=["GET", "POST"])
def add_one():
    if request.method == "GET":
        """ User wants the login page """
        return render_template("login.html")

    elif request.method == "POST":
        """ User is submitting form info """
        form_info = request.form.to_dict()
        if credentials_are_valid(form_info):
            log_in_user(form_info)
            return redirect(url_for("user_homepage"))
        else:
            return render_template("login.html", msg="Invalid login!")
```