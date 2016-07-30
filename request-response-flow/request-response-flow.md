#Request/Response Flow

### What happens when we make a [request](request.md)?
- type `www.example.com/courses` into the browser
- This initiates a GET request to `/courses` to our applications [webserver](webserver.md)
- Next, we follow the request from the webserver to the [app server](app-server.md)
- Now, the entry point into Rails. The app server will direct the request to the router. This tells Rails where to route this request in the application. A routes file looks like:

```ruby
resources :courses, only: [:index, :show] do
end
```

- Running `Rake -T` will show you what routes the resources method, above, defines. `Rake -T` will shows you a route name, a url path and the coresponding controller action for each route
- Back to our example. Hitting `www.example.com/courses` is going to correspond to the `courses` [controller](controller.md), `index` action
- The index action contains code for rendering a collection or records. Here is an example:

```ruby
class CoursesController < ApplicationController
  def index
    @courses = Course.all
  end
end
```

- The first line of the `index` action calls the `Course` [model](model.md)
- The courses model pulls the results out of the database
- Next the controller can render a view. It looks for a view based on a naming convention. In this example it would look for a view template called `app/view/courses/index.html.erb`
- The view templates defines HTML to return in the [HTTP response body](http-body.md)

## References
1. Rails Conf 2013 - How a Request Becomes a Response: https://www.youtube.com/watch?v=Cj2VDSugHM8