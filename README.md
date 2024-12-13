# CS50-Web-Final-Project

## About the project

A little over a year ago, I attended the Young Entrepreneurs Summer School organized by Startup Wise Guys. The event culminated in a hackathon, where my team, Choozy, pitched an app that lets groups of friends decide where to eat. I created an interactive prototype of the app in Figma, but at the time, I didn't know how to code. For my final project of CS50 WEB, I decided to build Choozy.

## Getting started
### Prerequisites
For this project you will need the following:
- Python (3.12.3)
- Django (5.1)
- Django REST framework (3.15)

This project uses the **Yelp Fusion API** and **LocationIQ**'s geocoding API. ***To use this project you will need to get an API key for both of them!*** No credit card is required to sign up for either of the APIs.

To get your Yelp Fusion API key:
1. Go to the [Yelp sign-up page](https://www.yelp.com/login?return_url=/developers/v3/manage_app) and create an account. You will be redirected to the Create App page.
2. Create your app by filling out the form and selecting Starter as the access tier.
3. After creating your app, you will be provided with your API key. 

To get your LocationIQ API key:
1. Go to the [LocationIQ register page](https://my.locationiq.com/register) and create an account.
2. Open the Dashboard
3. Click [Access Tokens](https://my.locationiq.com/dashboard/#accesstoken), and there you will find your API key

### Installation
Clone the GitHub repository:
```
git clone https://github.com/christophertserenkov/CS50-Web-Final-Project.git 
```

Navigate to the project directory:
```
cd CS50-Web-Final-Project
```

Ensure you have Python installed and create a virtual environment. You can do this by running `python3 -m venv .venv` and activating it with `source .venv/bin/activate` on Mac and Linux or `.venv\Scripts\activate` on Windows. Alternatively, you can run `Python: Create Environment` in the VS Code Command Palette.

Run `pip3 install -r requirements.txt` to install the necessary packages.

***Important step!*** In the project directory, there is a `.env` file. Open it and replace `YOUR_YELP_API_KEY` with your Yelp API key and `YOUR_GEOCODER_API_KEY` with your LocationIQ API key. If you don't provide the API keys the project will not work.

### Usage
Once you have the project installed and are in the project directory run `python3 manage.py runserver` to start the app. You can create an account and start a room.

### Deployment
This project imports Tailwind with a Play CDN script tag located in the `layout.html` file. If you wish to deploy the app you will need to install Tailwind. You can do so with the [django-tailwind](https://django-tailwind.readthedocs.io/en/latest/installation.html) library.

## Distinctiveness and Complexity

The web application is distinct from the other projects of the course because it utilizes features of Django not included in the course's projects, such as the Django REST Framework. The project also uses Tailwind as well as the daisyUI library to style the project, which was not covered by the course. The project is not a social network or an eCommerce website. Instead, the project is a Kahoot-like game, which can be played with friends to determine where to eat. The user can create a room, share the link to join, and see who has joined. The players who join the room are presented with a survey that lists various cuisines and types of food. The survey also asks questions about pricing, the need for parking, and whether they prefer to sit outdoors. When the host closes the room, the application collects all the responses and displays the restaurants returned by the Yelp API that match the group's preferences. The user can also see their past rooms.

The complexity of this project stems from its design. It represents rooms, players, and responses as Django database models. To authenticate the user without them needing to sign up, the user's ID is stored in the session upon joining. While creating the room, the app utilizes the browser's navigator to get the user's coordinates, which are checked to see if the user is in a country supported by the Yelp API (using the LocationIQ geocoder API). If the user is not in a supported location, the app prompts the user to enter the city and country they are in. Upon creation, the user lands on an admin page, displaying a QR code and a copyable link to share with his or her friends. In the background, an API is getting the list of players in the room, which is displayed for the admin, including their names and whether they have finished the survey or not. The front end communicates with the back end. After completing the survey, the user is redirected to a waiting page, which shows how many of the users are done using an API. The API returns the room's status and reloads the webpage with JavaScript to display the results if it has been closed. The restaurants are rendered as cards featuring their name, rating, phone number, price level, and categories. Additionally, the card displays buttons that lead to the Yelp page of the restaurant and, if available, a link to the menu.

## File descriptions

In the project directory, there is a `.env` file containing all the API keys,  the SQL database, and the `requirements.txt` file.

In the `urls.py file` in the capstone directory set the default path to reroute to the `urls.py` file in the `choozy` directory.

In the `choozy` directory we have all the template, static, and backend files. 

The `views.py` file includes these functions:
`index` - renders the home page.
`register` - creates a new user.
`login_view` - logs the user in.
`logout_view` - logs the user out.
`rooms` - renders a page displaying a "create room" button and listing all past rooms with links to their results.
`create_room` - checks if the user already has an active room, redirects them to its admin page if they do, and creates a `Room` object, checking if the location is supported by the Yelp Fusion API.
`room_admin` - renders the room admin page if the user is authenticated and the creator of said room.
`RoomViewSet` - makes an API endpoint, which the admin page calls to get the list of players (generated by CS50 Duckdebugger)
`join` - lets other players join by creating a `Player` object in the database and storing its ID in the user's session.
`room` - renders the Choozy form and stores the response of the player in the database.
`close_room` - sets the room's active status to false, gathers all the data from the `Submission` field, and makes the API call to get the restaurants, which are stored in the results field.
`players_in_room` - API used called by `waitroom.html` to get how many users in the room have completed the survey.
`results` - renders the list of restaurants returned by the Yelp API as cards.

The database fields are defined in the `models.py` file. The project includes four models: `Room`, `Player`,  `Submission`, and `Result`. The `Room` model stores the creator, coordinates, city, country, and date created. It also has a many-to-many field listing the players, who have joined the room. The `Player` field stores the player's name, status, and ID. `Submission` stores the responses of the players including selected categories, pricing level, need for parking, and if the player prefers to sit outside. The `Result` model stores data regarding the restaurants returned by the Yelp API. This includes the restaurant name, rating, pricing, categories, etc.  

`admin.py` registers the models so they can be viewed in the admin dashboard.

`serializers.py` serializes data from the models to send to the room admin page. It was generated by CS50 Duckdebugger after I asked it how I could create an API to get the players in the room.

The `templates` directory includes all the HTML files: 
`layout.html` -  includes the navbar, stylesheet tag, Tailwind CDN script tag, and the daisyUi library.
`index.html` - renders a basic homepage with a heading, image, and sign-up button, which changes into a "create room" button if the user is signed in.
`rooms.html` - displays a welcome message with the user's name, a "create room" button, and a table including all the past rooms a player has created.
`create.html` - renders a page where the user can choose to use their coordinates or select a country and city to create a room.
`room_admin.html` - displays a QR code and a link to join the room, along with a copy link button. It also shows the number of players joined, their statuses, and a list of all the players with their names.
`join.html` - allows users to join a room and enter their name.
`room.html` - renders the Choozy form with cuisines and types of food as bubbles, along with a radio button set for prices and toggles for parking and outdoor seating.
`waitroom.html` - a waiting page, which displays the amount of players in the room and how many of them are done.
`results.html` - lists all the restaurants with their data as cards.
`login.html` - login page
`register.html` - sign-up page

The `static` directory includes the `styles.css` stylesheet, and the src directory, which includes the cuisine and food type bubbles as png images. There are also some JavaScript files:
`create.js` - tries to get the user's coordinates, hides and shows the country and city selector, and uses an API to get the cities in a country and populates the city selector with them.
'room.js' - adds a ring around the selected bubbles, manages swiping between the bubbles and the additional information page, and allows the user to select between cuisine and type.
`roomAdmin.js` - handles swiping if the viewport is less than or equal to 768px and makes API calls to display the amount and list of players. 
'waiting.js` - makes API calls to get the number of players in the room and how many of them are done. Reloads the page if the room is no longer active to show results.


