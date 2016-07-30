#Request/Response Flow

### What happens when we make a [request](request-response-flow/request.md)?
- type `www.example.com/courses` into the browser
- This initiates a GET request to `/courses` to our applications **webserver.
- Next, we follow the request from the webserver to the ***app server
- Now, the entry point into Rails. The app server will direct the request to the router. This tells Rails where to route this request in the application. A routes file looks like:

```ruby
resources :courses, only: [:index, :show] do
end
```

- Running `Rake -T` will show you what routes the resources method, above, defines. `Rake -T` will shows you a route name, a url path and the coresponding controller action for each route.
- Back to our example. Hitting `www.example.com/courses` is going to correspond to the `courses` ****controller, `index` action.
- The index action contains code for rendering a collection or records. Here is an example:

```ruby
class CoursesController < ApplicationController
  def index
    @courses = Course.all
  end
end
```

- The first line of the `index` action calls the `Course` *****model.
- The courses model pulls the results out of the database
- Next the controller can render a view. It looks for a view based on a naming convention. In this example it would look for a view template called `app/view/courses/index.html.erb`
- The view templates defines HTML to return in the ******HTTP response body

### **What is a webserver?
- A webserver delivers webpages on the request of clients using HTTP
- Eg: Nginx, Apache, Webrick

### ***What is an appserver?
- Handles execution of your program
- Eg. Passenger, Mongrel, Thin, Unicorn
- Will spin up an instance of Rails

### ****What is a controller?
- A controller typically handles a single resource and contains one or more actions. Each action handles a different route and achieves a different business goal. For example, the index action displays all results for a given resource. the show action displays a single result based on the param provided.

### *****What is a model?
TODO

### ******HTTP response
- Headers should match up with the initial HTTP Request, in terms of HTTP content type ect.
- Cookies are returned, this is how they get back to the browser
- Returns a HTTP status code
- Returns a response body. This request body along with the content-type from the header is what allow the clients browser to render something useful.

## References
1. Rails Conf 2013 - How a Request Becomes a Response: https://www.youtube.com/watch?v=Cj2VDSugHM8
