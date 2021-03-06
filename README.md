## Website Performance Optimization portfolio project

Your challenge, if you wish to accept it (and we sure hope you will), is to optimize this online portfolio for speed! In particular, optimize the critical rendering path and make this page render as quickly as possible by applying the techniques you've picked up in the [Critical Rendering Path course](https://www.udacity.com/course/ud884).

To get started, check out the repository and inspect the code.

### Getting started

#### Part 1: Optimize PageSpeed Insights score for index.html

Some useful tips to help you get started:

1. Check out the repository
1. To inspect the site on your phone, you can run a local server

  ```bash
  $> cd /path/to/your-project-folder
  $> python -m SimpleHTTPServer 8080
  ```

1. Open a browser and visit localhost:8080
1. Download and install [ngrok](https://ngrok.com/) to the top-level of your project directory to make your local server accessible remotely.

  ``` bash
  $> cd /path/to/your-project-folder
  $> ./ngrok http 8080
  ```

1. Copy the public URL ngrok gives you and try running it through PageSpeed Insights! Optional: [More on integrating ngrok, Grunt and PageSpeed.](http://www.jamescryer.com/2014/06/12/grunt-pagespeed-and-ngrok-locally-testing/)

Profile, optimize, measure... and then lather, rinse, and repeat. Good luck!

#### Part 2: Optimize Frames per Second in pizza.html

To optimize views/pizza.html, you will need to modify views/js/main.js until your frames per second rate is 60 fps or higher. You will find instructive comments in main.js.

You might find the FPS Counter/HUD Display useful in Chrome developer tools described here: [Chrome Dev Tools tips-and-tricks](https://developer.chrome.com/devtools/docs/tips-and-tricks).

### Optimization Tips and Tricks
* [Optimizing Performance](https://developers.google.com/web/fundamentals/performance/ "web performance")
* [Analyzing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp.html "analyzing crp")
* [Optimizing the Critical Rendering Path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/optimizing-critical-rendering-path.html "optimize the crp!")
* [Avoiding Rendering Blocking CSS](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-blocking-css.html "render blocking css")
* [Optimizing JavaScript](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/adding-interactivity-with-javascript.html "javascript")
* [Measuring with Navigation Timing](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp.html "nav timing api"). We didn't cover the Navigation Timing API in the first two lessons but it's an incredibly useful tool for automated page profiling. I highly recommend reading.
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/eliminate-downloads.html">The fewer the downloads, the better</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/optimize-encoding-and-transfer.html">Reduce the size of text</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/image-optimization.html">Optimize images</a>
* <a href="https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching.html">HTTP caching</a>

#### Optimizations I did:
* Made all the JS scripts async
* Optimized all images and specified width and height
* Forced CSS to load later by putting it at the bottom of the body
* Deprecated determineDx and sizeSwitcher in favor of putting all computations in changePizzaSizes()
* Put ``` var pizzasDiv = document.getElementById("randomPizzas"); ``` before the ``` for ``` loop
* Made the rows of pizzas load dynamically based on page height.
* Changed     
         function updatePositions() {
            frame++;
            window.performance.mark("mark_start_frame");

            var items = document.querySelectorAll('.mover');
            for (var i = 0; i < items.length; i++) {
              var phase = Math.sin((document.body.scrollTop / 1250) + (i % 5));
              items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
            }

            // User Timing API to the rescue again. Seriously, it's worth learning.
            // Super easy to create custom metrics.
            window.performance.mark("mark_end_frame");
            window.performance.measure("measure_frame_duration", "mark_start_frame", "mark_end_frame");
            if (frame % 10 === 0) {
              var timesToUpdatePosition = window.performance.getEntriesByName("measure_frame_duration");
              logAverageFrame(timesToUpdatePosition);
          }
        }  
into
        var items;
        function updatePositions() {
          items = document.getElementsByClassName('mover');
          frame++;
          window.performance.mark("mark_start_frame");

          var scrollTop = (document.body.scrollTop / 1250);

          for (var i = 0; i < items.length; i++) {
            var phase = Math.sin(scrollTop + (i % 5));
            items[i].style.left = items[i].basicLeft + 100 * phase + 'px';
          }

          // User Timing API to the rescue again. Seriously, it's worth learning.
          // Super easy to create custom metrics.
          window.performance.mark("mark_end_frame");
          window.performance.measure("measure_frame_duration", "mark_start_frame", "mark_end_frame");
          if (frame % 10 === 0) {
            var timesToUpdatePosition = window.performance.getEntriesByName("measure_frame_duration");
            logAverageFrame(timesToUpdatePosition);
          }
        }

### Customization with Bootstrap
The portfolio was built on Twitter's <a href="http://getbootstrap.com/">Bootstrap</a> framework. All custom styles are in `dist/css/portfolio.css` in the portfolio repo.

* <a href="http://getbootstrap.com/css/">Bootstrap's CSS Classes</a>
* <a href="http://getbootstrap.com/components/">Bootstrap's Components</a>

### Running the page
* Download the repository
* Open index.html in your web browser
* You can then navigate to each page by clicking on the link
