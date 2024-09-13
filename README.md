import requests
import csv
import json


# Set up the URL and API key
url = "https://li-data-scraper.p.rapidapi.com/company-jobs"
payload = {
    "companyIds": [5383240, 2382910],  # Modify companyIds as necessary
    "page": 1
}
headers = {
    "x-rapidapi-key": "245a0ba893mshf624aad12713292p16050bjsn9403d0ec437d",
    "x-rapidapi-host": "li-data-scraper.p.rapidapi.com",
    "Content-Type": "application/json"
}

# Make the POST request
response = requests.post(url, json=payload, headers=headers)

# Parse the response
if response.status_code == 200:
    job_data = response.json()['data']['items']
    
    # Save data to CSV
    csv_file = "job_data.csv"
    with open(csv_file, mode='w', newline='', encoding='utf-8') as file:
        writer = csv.writer(file)
        writer.writerow(["ID", "Title", "URL", "Company", "Location", "Type", "Post Date", "Benefits"])
        for job in job_data:
            writer.writerow([
                job.get('id', ''),
                job.get('title', ''),
                job.get('url', ''),
                job.get('company', {}).get('name', ''),
                job.get('location', ''),
                job.get('type', ''),
                job.get('postAt', ''),
                job.get('benefits', '')
            ])
    print(f"Data saved to {csv_file}")
    
    # Save data to JSON
    json_file = "job_data.json"
    with open(json_file, 'w', encoding='utf-8') as file:
        json.dump(job_data, file, ensure_ascii=False, indent=4)
    print(f"Data saved to {json_file}")

else:
    print(f"Failed to retrieve data: {response.status_code}, {response.text}")

Job Data Scraper
This project retrieves job data for specific companies using the li-data-scraper API and saves the results in both CSV and JSON formats. The script sends a POST request to the API, parses the response, and then saves the data into two files, job_data.csv and job_data.json.

Prerequisites
To run this script, you need:

Python 3.x installed.
The requests module for sending HTTP requests. Install it by running:
bash
Copy code
pip install requests
Setup
Clone or download the project to your local machine.

Modify the companyIds in the script to reflect the IDs of the companies you want to query. These IDs should be specified as a list.

Example:

python
Copy code
"companyIds": [5383240, 2382910]
Replace the placeholder x-rapidapi-key in the headers dictionary with your actual API key from RapidAPI:

python
Copy code
headers = {
    "x-rapidapi-key": "YOUR_RAPIDAPI_KEY_HERE",
    ...
}
(Optional) Modify other payload parameters, such as the page number, if needed.

Usage
Run the script by executing the following command:

bash
Copy code
python job_data_scraper.py
If the API call is successful, the script will save two files in the local directory:

job_data.csv - Contains job listings in CSV format.
job_data.json - Contains job listings in JSON format.
CSV File Contents
The job_data.csv file will contain the following fields:

ID: The unique ID of the job listing.
Title: The title of the job position.
URL: The URL to the job posting.
Company: The name of the company offering the job.
Location: The job location.
Type: The type of job (full-time, part-time, etc.).
Post Date: The date the job was posted.
Benefits: Any benefits offered by the job (if available).
JSON File Contents
The job_data.json file will contain the full job listing data in structured JSON format.

Error Handling
If the API request fails, the script will print an error message with the status code and the response text for debugging.

License
This project is licensed under the MIT License.

