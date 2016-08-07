#Request/Response Flow

### What happens when we make a [request](request.md)?
1) type `www.example.com/courses` into the browser.

2) Your browser will make a DNS lookup to find out the IP address for the server that you are making a request to.

3) Once you're browser has an IP address is can make a HTTP request to the webserver. A HTTP request looks something like:

```
GET http://example.com/courses/ HTTP/1.1
User-Agent: Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1)
Host: www.example.com
```

4) Next, we follow the request from the webserver to the [app server](app-server.md). This is the entry point into Rails. The app server will direct the request to the router. This tells Rails where to route this request in the application. A routes file looks like:

```ruby
resources :courses, only: [:index, :show] do
end
```

Running `Rake -T` will show you what routes the resources method, above, defines. `Rake -T` will shows you a route name, a url path and the coresponding controller action for each route.

5) After hitting the router, the request will move to the controller. The example of `www.example.com/courses` is going to correspond to the `courses` [controller](controller.md), `index` action. The index action contains code for rendering a collection of records. Here is an example:

```ruby
class CoursesController < ApplicationController
  def index
    @courses = Course.all
  end
end
```

6) The first line of the `index` action calls the `Course` [model](model.md).

7) The courses model pulls the results out of the database.

8) Next the controller renders the view. It looks for a view based on a naming convention. In this example it would look for a view template called `app/view/courses/index.html.erb`.

9) The view templates defines HTML to return in the [HTTP response body](http-body.md).

## References
1. Rails Conf 2013 - How a Request Becomes a Response: https://www.youtube.com/watch?v=Cj2VDSugHM8
2. What really happens when you navigate to a URL: http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/
