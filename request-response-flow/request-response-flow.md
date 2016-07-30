#Request/Response Flow

### What happens when we make a [request](request.md)?
1) type `www.example.com/courses` into the browser.

2) This initiates a GET request to the `/courses` endpoint on the applications [webserver](webserver.md).

3) Next, we follow the request from the webserver to the [app server](app-server.md). This is the entry point into Rails. The app server will direct the request to the router. This tells Rails where to route this request in the application. A routes file looks like:

```ruby
resources :courses, only: [:index, :show] do
end
```

Running `Rake -T` will show you what routes the resources method, above, defines. `Rake -T` will shows you a route name, a url path and the coresponding controller action for each route.

4) After hitting the router, the request will move to the controller. The example of `www.example.com/courses` is going to correspond to the `courses` [controller](controller.md), `index` action. The index action contains code for rendering a collection of records. Here is an example:

```ruby
class CoursesController < ApplicationController
  def index
    @courses = Course.all
  end
end
```

5) The first line of the `index` action calls the `Course` [model](model.md).

6) The courses model pulls the results out of the database.

7) Next the controller renders the view. It looks for a view based on a naming convention. In this example it would look for a view template called `app/view/courses/index.html.erb`.

8) The view templates defines HTML to return in the [HTTP response body](http-body.md).

## References
1. Rails Conf 2013 - How a Request Becomes a Response: https://www.youtube.com/watch?v=Cj2VDSugHM8
