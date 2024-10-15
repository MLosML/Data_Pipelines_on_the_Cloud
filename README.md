**Automating Data Collection**


**1.1 Initial Situation:**

Gans is a dynamic startup focused on creating an E scooter sharing system. Their vision is to expand into the most popular cities globally. In each location, they will deploy hundreds of e-scooters on the streets enabling users to rent them by the minute.


**1.2 Main Target of the Project:**

automating data collection
Transforming raw data into a usable format
Secure storing the process data in the cloud


**1.3 Tools**

Web Scraping: BeautifulSoup 
Language: Python
API Interaction: requests library
Database: MySQL
Plattform: Google Cloud Platform (GCP)

![image](https://github.com/user-attachments/assets/0a4f9bcf-343f-4b36-b511-161f3b005e35)


**1.4 Project Steps**

In the following, I would like to describe the processes of the two phases, local pipeline and cloud pipeline in detail.

Gans has realized that its operational success hinges on a simple factor: ensuring its scooters are parked where users can easily access them. To address this charge it the company can take two steps:

Utilize trucks to relocate scooters
Provide economic incentives for users to pick up or drop off scooters in specific areas

Regardless guns aims to predict scooter movements as accurately as possible. While predictive modeling is planned, the first step is to gather transform and store the necessary data. The task is to collect data from external sources that can help Gans forecast e-scooter movement. Since real time daily data accessible to everyone in the company is essential the challenge was to build and automate a data pipeline in the cloud.


**2. Phase 1: Local Pipeline**

I created a data pipeline locally. This involved collecting data from external sources, transforming it and storing it in a SQL database. This set the groundwork for building a scalable and automated Pipeline in the cloud.


**2.1 Data Collection: Web Scraping**

Web scraping is the process of automatically extracting data from websites. It involved using programs or scripts to crawl websites, Internet  HTML code, and extract specific pieces of information such as prices, product descriptions, or use a reviews. It was important to use web scraping responsibly and ethically respecting copyright and privacy rights and only scraping public accessible data.


**2.1.1 Tools for Web Scraping**

BeautifulSoup is a Python library that simplifies web scraping. While other tools like Scrapy and Selenium are available BeautifulSoup offers a solid foundation for beginners and can handle most web scraping task you will come across. 

<img width="482" alt="image" src="https://github.com/user-attachments/assets/60bc4924-c82c-4df2-bc17-bf1c5d89aa0a">

This code uses the BeautifulSoup library to parse the HTML content of the response. Response.content contains the HTML of the page, and ‘html.parser’specifies the parser to use.


**2.2 Data Collection: APIs**

APIs provide  a structured and standardized means of accessing and retrieving data. They offer a more organized and efficient approach to data collection compared to Web Scraping. APIs typically require authentication, so I need to obtain the appropriate credentials before using them. Gans requested to collect weather and flight arrival information for all major cities and their airports daily starting from today and continuing for the next day.


**2.2.1 Weather API:** 

For collecting the weather data, I utilized OpenWeather an API that offers free weather forecast. Although OpenWeather provides various APIs, I specially used the 5 Day Forecast/ 3 Hours for this project. After fetching formatting the data into JSON, I used Python dictionary methods, like .keys() to access the information. Upon reviewing the data, I noticed that the dictionary format remained consistent across different days and times. This consistency allowed me to create function to extract the needed data from various cities and compile it into a data frame.

Making the API call and viewing the JSON

<img width="482" alt="image" src="https://github.com/user-attachments/assets/a3b36d3d-f175-4b24-af1d-51eefdab81e9">


**2.2.2 Flight API:**

Rapid API is an API marketplace that simplifies the process of requesting APIs by generating the necessary code for you. I utilized the AeroDataBox API to retrieve the flight arrival data required. The approach to accessing this data in Python mirrored, the previous method using dictionary methods. I created a function to extract the data into a DataFrame format. This function employs today and tomorrow variables to automatically fetch the data for the following day, as Gans needs to fly arrival times for the next day to plan accordingly.

<img width="320" alt="image" src="https://github.com/user-attachments/assets/921917b3-6c89-4780-abae-dddf28352cba">

As you can see, I am just interested in the arriving flights as these people are the potential users of the e-scooters.


**2.3 Data Storage: SQL Database**

In parallel with these data acquisition techniques, I learned how to build and maintain SQL databases.  SQL databases where the backbone of data storage providing a powerful and flexible way to organize and manage manage data locally. I created tables, inserted data, query data, and performed other essential database operations. By the end of Phase 1 I became a skilled data acquisition expert , capable of extracting, organizing and storing data from various sources.

This is how the Gans database looked like:

![image](https://github.com/user-attachments/assets/bb306843-f126-4970-a828-e15583b80b80)


**3.1 Phase 2: Cloud Pipeline**

Once the local pipeline is up and running, I migrated it to the cloud using Google Cloud Platform (GCP). GCP offers numerous advantages for data pipelines, including scalability, flexibility, automation, and maintenance.

Advantages of using cloud computing:

<img width="481" alt="image" src="https://github.com/user-attachments/assets/13afeb18-364b-4095-9aac-1a664b72dfbf">


**3.2 GCP MySQL**

GCP MySQL provides a reliable and scalable database solution that can be easily integrated with other GCP services. I set up  MySQL database in GCP and connected it to my data acquisition script. I used SQLAlchemy to store data from the code into the database this Python library simplifies the interaction between Python programs and databases.

<img width="482" alt="image" src="https://github.com/user-attachments/assets/d61ebeb2-0065-4745-a2e3-af05aa18b823">

This is the connection string for connecting to the local my SQL database using SQLAlchemy. 


**3.3 Cloud Functions**

Cloud functions allow me to run code in the cloud without managing servers. This makes them ideal for data pipelines because they can be triggered automatically. To collect data on regular basis, you have to migrate data collection scripts from Jupiter Notebooks to Cloud Functions.

**The first step is to set up the function in GCP:**

1.   In the top search bar, search for function and select Cloud Functions
2.   Select ‘create function’
3.   Enable all the required APIs – this can take a minute
4.   Set the function name as something sensible
5.   You can set the region to the same region as your sql database. This isn’t 100% necessary, but can reduce processing 
     times
6.   Check “Allow unauthenticated invocations” – as we’re not using any sensitive data, we can allow 
     this without too much worry.
7.   Click “Next”
8.   Enable any more APIs that pop up

**In the second step you are connecting your function to your instance:**

1.  Navigate to Cloud Run by clicking on the name of your function in this box Click on “Edit and deploy new revision”
2.  Scroll to the bottom of Container(s)
3.  In Cloud SQL Connections, click Add Connection
4.  Enable the Cloud SQL Admin API
5.  Select your database from the drop down. If you get a box asking to enable an API, enable that Api
    Click ‘Deploy’
6.  Once your function is deployed, click the URL again. You should now have successfully sent data to your sql database. If it was        successful, you will see the return message “Data successfully added”. If it was successful, check mysql workbench to see the 
    updates. If you see an error, time to debug


**3.4 Cloud Scheduler**

Cloud Scheduler is a scheduling service that allows to trigger Cloud Functions at specific times or events. This insures that my data pipeline runs consistently and reliably. Once I have integrated my data acquisition scripts with GCP and scheduled them to run automatically, I will have created a fully automated data pipeline in the cloud

How to schedule the function to run periodically:
	
1. Click ‘Schedule a job’
2. Give the scheduler a sensible name, here we’ll use ‘wbs-scheduler’
3. You can change the region to match the region of your database and function if you wish
4. In frequency, you need to input a Cron Expression. To better understand Cron Expressions, take a look at this website from 
   Oracle and this online generator.
5. Set the timezone to your local timezone, you can search by country
6. Click ‘continue’
7. The Target Type is HTTP
8. The URL is the URL of your function. If you unsure of what this is, you can find it on google cloud, on the Overview page of your 
   function
9. HTTP method needs to be GET, click Create


**4. Summary**

The approach used aligns with the best practice for tech projects, which typically start with a straightforward approach before venturing into more complex and innovative solutions. This is precisely why I built first the pipeline locally and only migrated it to the cloud upon completion. You should always anticipate encountering errors, discovering unforeseen requirements and generating fresh ideas along the way. Addressing these issues early on in an environment where debugging code and modifying database designs is straightforward, is key. This iterative approach ensures that you build a solid foundation for data pipeline, while allowing for flexibility and adaptability.

By the end of this project, my code is running in the cloud every day at 10 PM collecting data from the Internet and saving it to the cloud data instance. This journey has provided me with a solid grasp of the Data Engineering role, encompassing Web Scraping, API’s, local databases and automated data pipelines in the cloud. Here is a recap of the project:

- Data Collection: Gathered unnecessary data from the Internet, using Web Scraping with BeautifulSoup and various APIs
- Data Storage: and managed the database in  MySQL workbench to start collected data
- GCP - Pipelines on the cloud: Using Google Cloud Platform (GCP) to move the pipeline to the cloud
- GCP - Pipeline Automation: automating the entire data collection and storage process to run at the schedule time each day.






