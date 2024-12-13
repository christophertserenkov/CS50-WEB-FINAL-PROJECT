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

Ensure you have Python installed and create a virtual environment. You can do this by running `python3 -m venv .venv` and activating it with `source .venv/bin/activate` on Mac and Linux or `.venv\Scripts\activate` on Windows. Alternatively, you can run `Python: Create Environment` in the VS Code Command Palette

Run `pip3 install -r requirements.txt` to install the necessary packages.

### Important step! 
In the project directory, there is a `.env` file. Open it and replace `YOUR_YELP_API_KEY` with your Yelp API key and `YOUR_GEOCODER_API_KEY` with your LocationIQ API key. If you don't provide the API keys the project will not work.

### Usage
Once you have the project installed and are in the project directory run `python3 manage.py runserver` to start the app. You can create an account and start a room.

### Deployment
This project imports Tailwind with a Play CDN script tag located in the `layout.html` file. If you wish to deploy the app you will need to install Tailwind. You can do so with the [django-tailwind](https://django-tailwind.readthedocs.io/en/latest/installation.html) library.
