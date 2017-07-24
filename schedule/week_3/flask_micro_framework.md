# Introduction to Flask
Flask [http://flask.pocoo.org/](http://flask.pocoo.org/) is a microframework for Python. There are _no batteries included_, but it has a bunch of available plugins you can use, e.g.[Flask plugins](http://flask.pocoo.org/extensions/). By using the Flask microframwork you will be able to get close to all individual steps in the publication process and glue it all together keeping the overview.

## Flask installation
It is a really simple installation. All you have to do if it is not already installed is to run:
```bash
 $ pip install flask
```
To test that it works you can run the follwing code (also available in the repo as [flask-index.py](flask-index.py)) in your terminal commandline:
```python
from flask import Flask
app = Flask(__name__)
@app.route("/")
def get_my_index():
    return "This is index for / (ROOT) in flask-index.py"
if __name__ == '__main__':
    app.run(port=5000, debug=True)
```
When running this Flask app from the commandline it will output something like this: 
```bash
 $ python flask-index.py 
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger pin code: nnn-nnn-nnn
```

And when making a request to you app through the url specified <http://localhost:5000/> it will continue to output the access information:

```
127.0.0.1 - - [23/Jul/2017 19:02:15] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [23/Jul/2017 19:02:16] "GET /favicon.ico HTTP/1.1" 404 -
127.0.0.1 - - [23/Jul/2017 19:02:17] "GET /favicon.ico HTTP/1.1" 404 -
```

So with this small amount of code you have actually brought up a simple web server (eventhough only answering requests for `/`).

## Templates
Then Jinja templating engine is available in Python. There is a convention to put the templates in a subdirectory called `templates`.
You need to include it in your python header:
```python
from flask inport render_template
```

If we put a file named index.html into the `templates` direcory with the following contents:
```html
<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
    <title>template index.html</title>
  </head>
  <body>
    <h1>index.html</h1>
    <p>This is index.html in response to a request for / (ROOT) in flask-index-template.py</p>
  </body>
</html>
```
it could be used like:
```python
from flask import Flask
from flask import render_template
app = Flask(__name__)
@app.route("/")
def get_my_index():
    return render_template("index.html")
if __name__ == '__main__':
    app.run(port=5000, debug=True)
``` 

### Dynamic data with templates 
The render_template() function can take more arguments than only the template name.
```python
 render_template("index-dyn.html", title="template index.html", header="index.html", paragraph="...") 
```

This is used in the template `index-dyn.html`:
```html
<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
    <title>{{title}}</title>
  </head>
  <body>
    <h1>{{header}}</h1>
    <p>{{paragraph}}</p>
  </body>
</html>
```
See [flask-index-template-dyn.py](flask-index-template-dyn.py) and [templates/index-dyn.html](templates/index-dyn.html) in the repo.

#### Using a Python dictionary in the template 
Instead of passing in all single variables to the `render_template` function you can use a Python dictionary. You access the keys in the template by putting the dictionary variable name and the key together with a period, e.g. `dict.title` 
See [flask-index-template-dyn-dict.py](flask-index-template-dyn.py) and [templates/index-dyn.html](templates/index-dyn-dict.html) in the repo.

## Error handling
When creating applications it is important to manage _forseen errors_ as well as _unforseen errors_ and _exceptions_.
In the previous Python sessions of the institute we have not been doing this much since we were focusing on other parts of the coding. But this week it will be needed for publishing your edition.

### Default values
To make your application more _robust_ you should use default values to avoid errors. We are also introducing _request parameters_. In this case we use the default request method _GET_ to pass our parameter `header` and its value to the server for retrieval. By convention _GET_ should only be used for retrieval and not update the state in the server application. This way you can dynamically pass parameters from the _client_ e.g. the web browser or any HTTP capable library. The other methods are _POST_, _PUT_, and _DELETE_.

```python
from flask import Flask
from flask import render_template
from flask import request

defaults = { 'title': 'dynamic request header index.html' ,
             'header': 'Default "header" is used. Give request parameter header with a value to change it.',
             'paragraph': 'This is index.html with dynamic contents in response to a request for / (ROOT) in flask-request.py'
}

app = Flask(__name__)
@app.route("/")
def get_my_index():
    header = get_request_value_with_fallback('header')
    values = {'title': defaults['title'] ,
        'header': header,
        'paragraph': defaults['paragraph'] 
}
    return render_template("index-dyn-dict.html", dict=values)

def get_request_value_with_fallback(key):
    if request.args.get(key):
        return request.args.get(key)
    return defaults[key]

if __name__ == '__main__':
    app.run(port=5000, debug=True)
```
So by passing the request parameter we can now set the header to any string value. In this case `My header`: 
```bash
 http://localhost:5000/?header=My%20header
```
 
See [flask-request.py](flask-request.py).

### Avoid dividing by zero
Python have _Exceptions_. Then can be handled sometimes _catching_ them  withing _try blocks_ might be enough, but not always. In this case  we can handle `ZeroDivisionError` to say something about if we can continue or not. If we do not handle it but let Python _throw_ the error it would break the application execution and potentially cause inconsistency or at least giving you unhappy users.

```python
(x,y) = (5,0)
try:
   z = x/y
except ZeroDivisionError:
   print "divide by zero. We need to recover. Maybe ask the user for a better value."
# Depending on if we can recover or not: do what is needed to nicely exit or take receovery actions
```