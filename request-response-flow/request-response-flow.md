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

4) Next, the request will hit the web server (Nginx and Apache are two of the most common web servers). A web server will handle the incoming HTTP request and pass it on to the app server if required. For example, if the request is for something that doesn't change often (CSS, JavaScript or images) then this can be handled by the web server alone, it doesn't need to involve the app server.

Otherwise, the web server will pass the request on to the app server (Unicorn, Thin and Puma are examples of app servers). The app server is responsible for actually running the rails application, it loads your application code and keeps it in memory. When the app server gets a request from your web server, it tells your Rails app about it (you can run an app server without a web server but in production you'd usually want to use a web server).

5) What about Rack? Unicorn, Thin and Puma are all different webservers but they can all run Rails (and other Ruby applications). All of these webservers adhere to the Rack protocal.

6) Once the web server has handed the request on to the app server, this is the entry point into Rails. The app server will direct the request to the router. This tells Rails where to route this request in the application. A routes file looks like:

```ruby
resources :courses, only: [:index, :show] do
end
```

7) After hitting the router, the request will move to the controller. The example of `www.example.com/courses` is going to correspond to the `courses` [controller](controller.md), `index` action. The index action contains code for rendering a collection of records. Here is an example:

```ruby
class CoursesController < ApplicationController
  def index
    @courses = Course.all
  end
end
```

8) The first line of the `index` action calls the `Course` [model](model.md).

9) The courses model pulls the results out of the database.

10) Next the controller renders the view. It looks for a view based on a naming convention. In this example it would look for a view template called `app/view/courses/index.html.erb`.

11) The view templates defines HTML to return in the [HTTP response body](http-body.md).

## References
1. Rails Conf 2013 - How a Request Becomes a Response: https://www.youtube.com/watch?v=Cj2VDSugHM8
2. What really happens when you navigate to a URL: http://igoro.com/archive/what-really-happens-when-you-navigate-to-a-url/
3. A Web Server vs. An App Server: http://www.justinweiss.com/articles/a-web-server-vs-an-app-server/
