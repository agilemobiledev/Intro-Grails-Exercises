Exercises
==========

1. *Bookmark References*
    
    Bookmark the following sites for future reference:  
        [Groovy homepage](http://groovy.codehaus.org)  
        [Groovy API](http://groovy.codehaus.org/gapi/)  
        [Groovy JDK](http://groovy.codehaus.org/groovy-jdk/)  
        [Grails homepage](http://grails.org/)  
        [Grails user guide](http://grails.org/doc/latest/)
        
2. *A new Grails application*

    a. Create a new Grails application called `hello` using the `grails create-app` command. Once it completes, change directory into the `hello` app.
    
    b. Use the `create-controller com.mycompany.Welcome` command to create a new controller called `com.mycompany.WelcomeController`. Where is the resulting file stored? Where is the test class?
    
    c. In the `WelcomeController` class, what is the default action? What URL corresponds to this action? What would happen if we started the application and accessed this action now? (If you're not sure, try it and see.)
    
3. *Render*

    a. Inside the `index` action in the `WelcomeController` class, add a line rendering the phrase `Hello, World!`.
    
        render '<h2>Hello, World!<\h2>'
    
    b. Start the application (if not already started) using the `grails run-app` command and click on the `WelcomeController` link to access the action.
    
    c. Inside the `index` action, declare a `String` variable called `n`. Assign it to the value of the `name` property in the incoming request. If `params.name` is null or empty, default to the word `World`. Change the render to use the `n` value.
    
    (Note: this is a perfect excuse to use the Elvis operator from Groovy.)
    
        String n = params.name ?: 'World'
        render "<h2>Hello, $n!</h2>"

4. *Redirect*

    a. Add a new action to the `WelcomeController` called `hello`, which takes no arguments and returns `def`. What is the URL corresponding to this action?
    
    b. Take the implementation of the `index` action and move it to the `hello` action.
    
    c. In the `index` action, redirect to the `hello` action (note the parentheses below are optional).
    
        redirect(action:'hello')
        
    d. Access the URL corresponding to the `index` action, _without_ supplying a `name` parameter. Verify that you see the string 'Hello, World!' in the display.
    
    e. Add a `name` parameter to the URL for the `index` action and access the URL. Does the parameter get passed to the `hello` action? Why not?
    
    f. Add a second argument to the `redirect` method to pass the input parameters to the next request and try again. The supplied name should now show in the response.
    
        redirect(action:'hello', params:params)
        
5. *Return*

    a. Add a new action called `greet` to the `WelcomeController` class. This time, add a `String` parameter called `name` to the method. Grails will automatically supply it from the request if it exists, and use `null` if not. As before, save it to a variable called `n`, using a default if the parameter is null or empty.
    
    b. This time, have the action return a map with one entry. The key in the entry should be `name` and the value is the `n` variable. Remember, the `return` keyword itself is optional. The complete implementation is shown below.
    
        def greet(String name) {
            String n = name ?: 'World'
            [name:n]
        }
        
    When the controller action completes, where will Grails go next?
        
    c. Create a Groovy Server Page called `greet.gsp` in the `welcome` folder under `grails-app\views`. Give it a simple HTML structure (root element of `<html>`, with a `<head>` section and a `<body>` section). In the head section, add a `meta` tag to tell Grails to use the default SiteMesh layout.
        
    In the body, add a `<div>` element with the CSS class `body`. Inside the `<div>`, add an `<h2>` element that displays "Hello, ${name}!".  The complete code is shown below.
        
        <html>
            <head>
                <meta name="layout" content="main">
                <title>Greeting</title>
            </head>
            <body>
                <div class="body">
                    <h2>Hello, ${name}!</h2>
                </div>
            </body>
        </html>
        
    d. Access the `greet` action with a URL, once without a `name` parameter and once with one.
    
6. *Testing the Controller*

    a. Open the `WelcomeControllerTests` class in the `hello\test\unit\com\mycompany\` directory.
    
    b. Add a test for the `index` action by invoking the method on the `controller` reference and checking that the re-directed URL is `/welcome/hello`.
    
        controller.index()
        assert response.redirectedUrl == '/welcome/hello'
    
    c. Add two tests for the `hello` action, one that adds a `name` parameter to the `params` map and one that doesn't. In each test, invoke the `hello` method on the `controller` and check that the `text` property of the `response` object is the expected value (the string returned by the `render` method).
    
        params.name = 'Dolly'
        controller.hello()
        assert response.text == '<h2>Hello, Dolly!</h2>'
    
    d. Add two tests for the `greet` action, again with and without a `name` parameter. This time, when you invoke the `controller.greet()` method, save the result in a variable of type `def` called `model`. In each test, assert that the `model.name` value is the proper string.
    
        def model = controller.greet()
        assert 'World' == model.name
        // and 
        def model = controller.greet('Dolly')
        assert 'Dolly' == model.name 
    
    e. Execute the `grails test-app unit:` (note the colon after the word `unit`) task and check the resulting output to make sure everything works.