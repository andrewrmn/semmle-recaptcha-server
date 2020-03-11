# Semmle reCaptcha V2 Server

## Instructions
1. Register your site/ get your reCaptcha keys [here](https://www.google.com/recaptcha)
2. Create a new [Heroku App](https://dashboard.heroku.com/apps)
3. In your new Heroku App, go to settings and create a new Config Var called `RECAPTCHA_SECRET` and enter your secret key in the value field
4. Clone this repo
5. Deploy your app via the [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli) or hook up your GitHub repository via the Deploy tab in your Heroku App
6. After you deploy your app, your app url should display 'reCaptcha verification'

## Frontend Instructions
1. Add captcha script tag `<script src="https://www.google.com/recaptcha/api.js" async defer></script>`
2. Add captcha element to form `<div class="g-recaptcha" data-sitekey="6LcpkuAUAAAAAK8PvQ5Lz24yd_Os7vh9RCHvCAGR"></div>`
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

Form will be submitted if the captcha passes. If not, show error messages.
