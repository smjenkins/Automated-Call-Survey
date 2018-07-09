Here's how it works at a high level
1. The end user calls or sends an SMS to the survey's phone number.
2. the app gets the call or text and makes a request to the application asking for instructions on how to respond.
3. the application Gather or Record the user's voice input, and prompts text input with Message if you are using SMS.
4. After each question, the application stores the reply in its database and go to the next question.

Tech Used: 
Php (coding language), 
Laravel (coding framework), 
Postgres (database),
ngrok (testing software for demo use),
Twilio (for demo calls and sms)


## Running locally

1. Download and enter root folder 

2. run
   $ composer install

3. Create a database (if you get "do not recognized this command" you need to download Postgres, reinstall it or start it)
  $ createdb surveys

4. run (laravel requires a .env system file)
   $ cp .env.example .env

5. Generate an `APP_KEY`(for your database)
   $ php artisan key:generate

6. Run the migrations
  $ php artisan migrate

7. Load a survey (a demo survey is in bear_survey.json)
  $ php artisan heroku:initialize bear_survey.json

8. Expose the application to the wider Internet using [ngrok](https://ngrok.com/)
   $ ngrok http 8000

   Now you have a public URL that will forward requests to your localhost. It should
   look like this:

   ```
   http://<your-ngrok-subdomain>.ngrok.io
   ```

1. Configure Twilio to call your webhooks.

   For this application you must set the voice webhook of your number.
   It will look something like this:

   ```
   http://<your-ngrok-subdomain>.ngrok.io/voice/connect
   ```

   The SMS webhook should look something like this:

   ```
   http://<your-ngrok-subdomain>.ngrok.io/sms/connect
   ```

   For this application you must set the `POST` method on the configuration for both webhooks.

   You will need to provision at least one Twilio number with SMS and voice capabilities.
   You can buy a number [right
   here](https://www.twilio.com/user/account/phone-numbers/search). Once you have
   a number you need to configure it to work with your application. Open
   [the number management page](https://www.twilio.com/user/account/phone-numbers/incoming)
   and open a number's configuration by clicking on it.


   

2. Run the application using Artisan.
   $ php artisan serve


## How to Demo

1. Give your number a call or send yourself an SMS with the "start" command.

1. Follow the instructions until you answer all the questions.

1. When you are notified that you have reached the end of the survey, visit your
   application's root to check the results at:

   http://localhost:8000


## Running the tests

The tests interact with the database so you'll first need to migrate
your test database. First, set the `DATABASE_URL_TEST` and then run:
$ createdb surveys_test
$ APP_ENV=testing php artisan migrate

Run at the top-level directory.
$ phpunit


