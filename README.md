# Semmle reCaptcha V2 Server

This code is to be deployed to a Heroku app. The frontend form will then send the captcha data to the Heroku app and wait for a success response before submitting the form.

## Instructions
1. Register your site/ get your reCaptcha keys [here](https://www.google.com/recaptcha)
2. Create a new [Heroku App](https://dashboard.heroku.com/apps)
3. In your new Heroku App, go to settings and create a new Config Var called `RECAPTCHA_SECRET` and enter your secret key in the value field
4. Deploy this code to your Heroku app via the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) or hook up the GitHub repository via the Deploy tab in your Heroku App
5. After you deploy your app, your app url should display 'reCaptcha verification'

## Frontend Instructions
1. Add captcha script tag `<script src="https://www.google.com/recaptcha/api.js" async defer></script>`
2. Add captcha element to form `<div class="g-recaptcha" data-sitekey="YOUR_RECAPTCHA_PUBLIC_SITE_KEY_HERE"></div>`
3. Add form event listener to send reCaptca data to Heroku server:

<pre><code>var form = document.querySelector('#contact-form');

form.addEventListener("submit", function(event) {
    event.preventDefault();
    event.stopPropagation();

    var recaptcha = document.querySelector('#g-recaptcha-response').value;

    fetch('HEROKU_APP_URL_HERE', {
        method:'POST',
        headers: {
            'Accept': 'application/json, text/plain, */*',
            'Content-type':'application/json'
        },
        body:JSON.stringify({captcha: recaptcha})
    })
    .then((res) => res.json())
    .then((data) => {
        console.log(data);
        if(data.success) {
            // Submit form if success status returned
            form.submit();
        } else {
            // Handle error messages            
        }
    });
}, false);
</code></pre>
