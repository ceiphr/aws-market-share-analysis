# AP Statistics: Final Project - AWS Market Share Claim Validation
## By Ari Birnbaum

This file contains all the code written to produce a simple random sample of websites, calculate the proportion that uses AWS, and run a one-proportion z-test.

---

# Collection of Simple Random Sample from Population of Websites


```python
from random import randint
import pandas as pd
import socket

# List of top one million sites according to Alexa Analytics/Website Ranking
# https://s3.amazonaws.com/alexa-static/top-1m.csv.zip
top_sites = pd.read_csv('top-1m.csv', header=None)[1]

# n is sample size
n=100

# Dictionary used for stored sample data
sample = {
    'Website Domain' : [],
    'IPv4 Address' : []
}

def create_sample(n): 
    i = 0
    while i < n:
        i += 1
        # Get random number between 0 and 999,999
        random_index = randint(0, len(top_sites) - 1)

        # If the site has not already been selected, add it to our data set
        if not top_sites[random_index] in sample['Website Domain']:
            try:
                # print("\033[0mGetting IPv4 Address for %s..." % top_sites[random_index])
                ipv4 = socket.gethostbyname(top_sites[random_index])
            # If we can't resolve the IP from the host name, replace it with a different host name
            except:
                # print("\033[1mFailed. Selecting new site for sample.")
                i -= 1
                continue
            sample['Website Domain'].append(top_sites[random_index])
            sample['IPv4 Address'].append(ipv4)

create_sample(n)

# Save sample to a CSV file
dataset = pd.DataFrame.from_dict(sample)
dataset.to_csv('website_sample.csv')

dataset
```

# Determining Proportion of Websites Running AWS


```python
import json, requests, ipaddress 

# List of IP Ranges (IPv4 and IPv6) owned by Amazon and used for AWS
# https://ip-ranges.amazonaws.com/ip-ranges.json
aws_ip_ranges = json.loads(requests.get('https://ip-ranges.amazonaws.com/ip-ranges.json').text)

# Determine if given IP address (ip_input) shows uo in AWS IPv4 Range
def check_aws(ip_input):
    # Compare given IP to all AWS IP addresses within AWS IPv4 Subnetwork
    for i in range(len(aws_ip_ranges['prefixes'])):
        # Parse IPv4 address for comparison
        site_ip = ipaddress.ip_address(ip_input)
        
        # Parse AWS IPv4 Subnet
        aws_subnet = ipaddress.ip_network(aws_ip_ranges['prefixes'][i]['ip_prefix'])
        
        # If IP is within the AWS IPv4 Range, the website is run on AWS
        if site_ip in aws_subnet:
            return True
    # If the website is not within the range, the 
    # website operates independnetly of AWS    
    return False

# List of websites using AWS
websites_using_aws = []

def get_aws_domains():

    # Check every IP within our sample against AWS IPv4 Range
    for i in range(len(dataset)):
        if check_aws(dataset['IPv4 Address'][i]):
            websites_using_aws.append(dataset['Website Domain'][i])

get_aws_domains()
            
# Save dataset of AWS websites to a CSV file
aws_df = pd.DataFrame({'AWS Websites':websites_using_aws})
aws_df.to_csv('websites_using_aws.csv')

aws_df
```

# 1-Proportion Z-Test for Proportion of AWS to non-AWS Websites


```python
import math
import scipy.stats as st

# Creating initial values from datatset/claim
claimed_marketshare = 0.31

p = claimed_marketshare
q = 1-claimed_marketshare

# Success/Failure Condition, exception raised if np or nq is less than 10
assert n*p >= 10, True
assert n*q >= 10, True

# Calculate Z-Score & P-Value
z = ((len(websites_using_aws)/n) - p)/math.sqrt((p*q)/n)
p_value = st.norm.cdf(z)
  
print('P: %f\tQ: %f\nNP: %f\tNQ: %f\n\nP-Hat: %f\n\nZ-Score: %f\nP-Value: %f\n' 
      % (p, q, n*p, n*q, (len(websites_using_aws)/n), z, p_value))

# Hypothesis Testing
confidence_level = 0.95

if p_value <= (1-confidence_level): print('\033[1mReject H0')
else: print('\033[1mFail to reject H0')
```
