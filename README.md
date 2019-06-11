
# Collection of Simple Random Sample from Population of Websites

## Simple Random Sample

Below is a program that creates a simple random sample of websites according to Alexa Analytics' Top One Million sites CSV. The function `create_sample` creates a random integer between 0 and 999,999. That number acts as the index for the site we will get from the population CSV (e.g. The site at index 0 is google.com). 

Once we have selected a site for our sample, we need to resolve its IP address (note: we will be using only IPv4 addresses as Alexa only offers those in their dataset and all websites should handle IPv4 and IPv6). If we are unable to resolve the address than we will randomly select another site to replace it.

When sampling is completed, the sample dataset (domains and IPv4 addresses) is exported to a CSV file.


```python
from random import randint
import pandas as pd
import socket

# List of top one million sites according to Alexa Analytics/Website Ranking
# https://s3.amazonaws.com/alexa-static/top-1m.csv.zip
top_sites = pd.read_csv('top-1m.csv', header=None)[1]

# n is sample size
n=1000

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

# Use Dataset Instead of Creating New Sample
Essentially importing `website_sample.csv` for our dataset so we don't have to create a new sample.


```python
n = 1000
dataset = pd.DataFrame.from_csv('website_sample.csv')
dataset
```

    /home/ari/.anaconda3/lib/python3.6/site-packages/ipykernel_launcher.py:2: FutureWarning: from_csv is deprecated. Please use read_csv(...) instead. Note that some of the default arguments are different, so please refer to the documentation for from_csv when changing your function calls
      





<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Website Domain</th>
      <th>IPv4 Address</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>ipos.vn</td>
      <td>94.237.76.49</td>
    </tr>
    <tr>
      <th>1</th>
      <td>sectorul4news.ro</td>
      <td>89.42.219.210</td>
    </tr>
    <tr>
      <th>2</th>
      <td>newprint.ca</td>
      <td>37.218.253.61</td>
    </tr>
    <tr>
      <th>3</th>
      <td>destinationhotels.com</td>
      <td>23.100.83.213</td>
    </tr>
    <tr>
      <th>4</th>
      <td>flutetoday.com</td>
      <td>97.74.55.1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>serialcrush.com</td>
      <td>62.149.142.158</td>
    </tr>
    <tr>
      <th>6</th>
      <td>mybmtc.com</td>
      <td>202.71.129.225</td>
    </tr>
    <tr>
      <th>7</th>
      <td>hmi.edu</td>
      <td>192.249.121.112</td>
    </tr>
    <tr>
      <th>8</th>
      <td>flashkit.com</td>
      <td>70.42.23.121</td>
    </tr>
    <tr>
      <th>9</th>
      <td>fucktubes.xxx</td>
      <td>104.28.23.71</td>
    </tr>
    <tr>
      <th>10</th>
      <td>thetodaypost.com</td>
      <td>104.31.66.246</td>
    </tr>
    <tr>
      <th>11</th>
      <td>avpgalaxy.net</td>
      <td>162.211.84.48</td>
    </tr>
    <tr>
      <th>12</th>
      <td>scwtenor.com</td>
      <td>74.208.236.209</td>
    </tr>
    <tr>
      <th>13</th>
      <td>ecom.ly</td>
      <td>104.18.47.2</td>
    </tr>
    <tr>
      <th>14</th>
      <td>redwolfwildernessadventures.com</td>
      <td>67.59.136.110</td>
    </tr>
    <tr>
      <th>15</th>
      <td>wesounds.com</td>
      <td>107.6.153.170</td>
    </tr>
    <tr>
      <th>16</th>
      <td>bloganten.ru</td>
      <td>92.53.114.3</td>
    </tr>
    <tr>
      <th>17</th>
      <td>karinto.in</td>
      <td>175.134.120.229</td>
    </tr>
    <tr>
      <th>18</th>
      <td>myscopeoutreach.org</td>
      <td>182.160.163.245</td>
    </tr>
    <tr>
      <th>19</th>
      <td>landsurveyor.blogfa.com</td>
      <td>149.56.201.253</td>
    </tr>
    <tr>
      <th>20</th>
      <td>finministry.com</td>
      <td>104.18.41.100</td>
    </tr>
    <tr>
      <th>21</th>
      <td>odeontravel.rs</td>
      <td>195.252.107.131</td>
    </tr>
    <tr>
      <th>22</th>
      <td>greeningtheblue.org</td>
      <td>104.27.147.128</td>
    </tr>
    <tr>
      <th>23</th>
      <td>jerusalemperspective.com</td>
      <td>104.199.115.212</td>
    </tr>
    <tr>
      <th>24</th>
      <td>bazi-oksana.ru</td>
      <td>5.101.152.32</td>
    </tr>
    <tr>
      <th>25</th>
      <td>leupold.com</td>
      <td>52.88.153.55</td>
    </tr>
    <tr>
      <th>26</th>
      <td>cloud9.gg</td>
      <td>23.227.38.32</td>
    </tr>
    <tr>
      <th>27</th>
      <td>goldsgym.com</td>
      <td>162.209.117.196</td>
    </tr>
    <tr>
      <th>28</th>
      <td>tagbox.in</td>
      <td>52.172.54.225</td>
    </tr>
    <tr>
      <th>29</th>
      <td>sexmamki.org</td>
      <td>151.80.209.25</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>970</th>
      <td>jb.com.br</td>
      <td>152.199.54.25</td>
    </tr>
    <tr>
      <th>971</th>
      <td>vietadsonline.com</td>
      <td>171.244.34.197</td>
    </tr>
    <tr>
      <th>972</th>
      <td>itftkd.ir</td>
      <td>185.128.81.85</td>
    </tr>
    <tr>
      <th>973</th>
      <td>rouxroamer.com</td>
      <td>104.31.95.99</td>
    </tr>
    <tr>
      <th>974</th>
      <td>filejo.co.kr</td>
      <td>43.255.255.83</td>
    </tr>
    <tr>
      <th>975</th>
      <td>peraichi.com</td>
      <td>54.230.89.244</td>
    </tr>
    <tr>
      <th>976</th>
      <td>hw3d.net</td>
      <td>192.99.14.211</td>
    </tr>
    <tr>
      <th>977</th>
      <td>nekoshop.ru</td>
      <td>37.140.192.198</td>
    </tr>
    <tr>
      <th>978</th>
      <td>maxbestwork.com</td>
      <td>66.199.189.51</td>
    </tr>
    <tr>
      <th>979</th>
      <td>hindihelpguru.com</td>
      <td>199.250.213.223</td>
    </tr>
    <tr>
      <th>980</th>
      <td>exertion-fitness.com</td>
      <td>23.227.38.32</td>
    </tr>
    <tr>
      <th>981</th>
      <td>aktualnacenabytu.sk</td>
      <td>212.57.38.25</td>
    </tr>
    <tr>
      <th>982</th>
      <td>geoequipos.cl</td>
      <td>192.140.57.10</td>
    </tr>
    <tr>
      <th>983</th>
      <td>removenotifications.com</td>
      <td>192.64.119.93</td>
    </tr>
    <tr>
      <th>984</th>
      <td>matbit.net</td>
      <td>5.61.47.250</td>
    </tr>
    <tr>
      <th>985</th>
      <td>homebasedonlinevehiclemarketing.com</td>
      <td>146.66.96.176</td>
    </tr>
    <tr>
      <th>986</th>
      <td>xn--v8j5erc590uusnxox.com</td>
      <td>183.90.253.8</td>
    </tr>
    <tr>
      <th>987</th>
      <td>nflsport.icu</td>
      <td>198.54.121.189</td>
    </tr>
    <tr>
      <th>988</th>
      <td>izithakazelo.blog</td>
      <td>192.0.78.191</td>
    </tr>
    <tr>
      <th>989</th>
      <td>irinabiz.ru</td>
      <td>138.201.199.38</td>
    </tr>
    <tr>
      <th>990</th>
      <td>intercity.pl</td>
      <td>46.174.180.162</td>
    </tr>
    <tr>
      <th>991</th>
      <td>bongacams3.com</td>
      <td>31.192.123.62</td>
    </tr>
    <tr>
      <th>992</th>
      <td>twinstrangers.net</td>
      <td>52.214.239.109</td>
    </tr>
    <tr>
      <th>993</th>
      <td>textgeneratorfont.com</td>
      <td>162.241.133.121</td>
    </tr>
    <tr>
      <th>994</th>
      <td>silversaints.com</td>
      <td>212.188.174.246</td>
    </tr>
    <tr>
      <th>995</th>
      <td>evassmat.com</td>
      <td>104.28.24.228</td>
    </tr>
    <tr>
      <th>996</th>
      <td>mpets.mobi</td>
      <td>136.243.25.36</td>
    </tr>
    <tr>
      <th>997</th>
      <td>londongateway.com</td>
      <td>65.52.130.1</td>
    </tr>
    <tr>
      <th>998</th>
      <td>derangler.shop</td>
      <td>85.236.56.247</td>
    </tr>
    <tr>
      <th>999</th>
      <td>tavirekini.lv</td>
      <td>94.100.11.185</td>
    </tr>
  </tbody>
</table>
<p>1000 rows × 2 columns</p>
</div>



# Determining Proportion of Websites Running AWS

## Proportion of IPv4 addresses owned by AWS

The program takes a list of all IPv4 addresses owned by AWS and compares them to the list of addresses in our sample. AWS does not give a list of IPv4 address but instead gives a subnet of their addresses, this means they've purchased addresses in bulk so they're grouped together. In order to properly compare an IPv4 address to a subnet, python offers a library called `ipaddress` that breaks up subnets and ip addresses into a data format that can easily be compared.

If an address appears in Amazon's IPv4 range (their owned addresses) than the domain associated with the IP address is appended to a list. The list of websites is then exported as a CSV file.


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




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>AWS Websites</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>leupold.com</td>
    </tr>
    <tr>
      <th>1</th>
      <td>wanasatime.com</td>
    </tr>
    <tr>
      <th>2</th>
      <td>simplesdental.com</td>
    </tr>
    <tr>
      <th>3</th>
      <td>monetixwallet.com</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10tv.in</td>
    </tr>
    <tr>
      <th>5</th>
      <td>rosedalecenter.com</td>
    </tr>
    <tr>
      <th>6</th>
      <td>margstacobistro.com</td>
    </tr>
    <tr>
      <th>7</th>
      <td>shanghainavi.com</td>
    </tr>
    <tr>
      <th>8</th>
      <td>keaweather.net</td>
    </tr>
    <tr>
      <th>9</th>
      <td>lion.co.nz</td>
    </tr>
    <tr>
      <th>10</th>
      <td>moomii.jp</td>
    </tr>
    <tr>
      <th>11</th>
      <td>figleafapp.com</td>
    </tr>
    <tr>
      <th>12</th>
      <td>maharajamultiplex.in</td>
    </tr>
    <tr>
      <th>13</th>
      <td>conchovalleyhomepage.com</td>
    </tr>
    <tr>
      <th>14</th>
      <td>honkmedia.net</td>
    </tr>
    <tr>
      <th>15</th>
      <td>willowtreeapps.com</td>
    </tr>
    <tr>
      <th>16</th>
      <td>playvod.ma</td>
    </tr>
    <tr>
      <th>17</th>
      <td>tigosports.gt</td>
    </tr>
    <tr>
      <th>18</th>
      <td>araelium.com</td>
    </tr>
    <tr>
      <th>19</th>
      <td>boostnote.io</td>
    </tr>
    <tr>
      <th>20</th>
      <td>echofoodshelf.org</td>
    </tr>
    <tr>
      <th>21</th>
      <td>obonsai.com.br</td>
    </tr>
    <tr>
      <th>22</th>
      <td>atcost.in</td>
    </tr>
    <tr>
      <th>23</th>
      <td>profsnhcadmission.in</td>
    </tr>
    <tr>
      <th>24</th>
      <td>manalonline.com</td>
    </tr>
    <tr>
      <th>25</th>
      <td>nj211.org</td>
    </tr>
    <tr>
      <th>26</th>
      <td>conta.no</td>
    </tr>
    <tr>
      <th>27</th>
      <td>cldmail.co.uk</td>
    </tr>
    <tr>
      <th>28</th>
      <td>obo.se</td>
    </tr>
    <tr>
      <th>29</th>
      <td>soft32.es</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
    </tr>
    <tr>
      <th>31</th>
      <td>juegosmesa.cl</td>
    </tr>
    <tr>
      <th>32</th>
      <td>localone.app</td>
    </tr>
    <tr>
      <th>33</th>
      <td>grjapan.jp</td>
    </tr>
    <tr>
      <th>34</th>
      <td>peachysnaps.com</td>
    </tr>
    <tr>
      <th>35</th>
      <td>knottyladyyarns.com</td>
    </tr>
    <tr>
      <th>36</th>
      <td>amormaturo.com</td>
    </tr>
    <tr>
      <th>37</th>
      <td>hltmag.co.uk</td>
    </tr>
    <tr>
      <th>38</th>
      <td>oldtownportraitgallery.com</td>
    </tr>
    <tr>
      <th>39</th>
      <td>matterport.com</td>
    </tr>
    <tr>
      <th>40</th>
      <td>parkopedia.co.uk</td>
    </tr>
    <tr>
      <th>41</th>
      <td>rehabs.com</td>
    </tr>
    <tr>
      <th>42</th>
      <td>diablomedia.com</td>
    </tr>
    <tr>
      <th>43</th>
      <td>battersea.org.uk</td>
    </tr>
    <tr>
      <th>44</th>
      <td>qmo.org.au</td>
    </tr>
    <tr>
      <th>45</th>
      <td>midlandsb.com</td>
    </tr>
    <tr>
      <th>46</th>
      <td>kuflink.co.uk</td>
    </tr>
    <tr>
      <th>47</th>
      <td>lunns.com</td>
    </tr>
    <tr>
      <th>48</th>
      <td>send2sell.com</td>
    </tr>
    <tr>
      <th>49</th>
      <td>wvi.org</td>
    </tr>
    <tr>
      <th>50</th>
      <td>letsignite.in</td>
    </tr>
    <tr>
      <th>51</th>
      <td>kolkt.com</td>
    </tr>
    <tr>
      <th>52</th>
      <td>lucasmaier.com.br</td>
    </tr>
    <tr>
      <th>53</th>
      <td>ellevatenetwork.com</td>
    </tr>
    <tr>
      <th>54</th>
      <td>completefrance.com</td>
    </tr>
    <tr>
      <th>55</th>
      <td>bellevie-group.com</td>
    </tr>
    <tr>
      <th>56</th>
      <td>via.id</td>
    </tr>
    <tr>
      <th>57</th>
      <td>tweentribune.com</td>
    </tr>
    <tr>
      <th>58</th>
      <td>filesun.net</td>
    </tr>
    <tr>
      <th>59</th>
      <td>peraichi.com</td>
    </tr>
    <tr>
      <th>60</th>
      <td>twinstrangers.net</td>
    </tr>
  </tbody>
</table>
<p>61 rows × 1 columns</p>
</div>



# 1-Proportion Z-Test for Proportion of AWS to non-AWS Websites

## Testing in Python

This is a procedure for completing a one-proportion z-test given the sample dataset and proportion of websites using AWS. First we declare `p` as the claimed market share percentage (The Forbes article claimed AWS has a 31% market share). We calculate `q` and proceed with our success failure condition. 

The `assert` lines basically test s/f - if np or qp is < 10 than the program stops and an exception (error) is raised. Because the index values when sampling were random, than we can assume our sample is random.

Next we calculate our z-score value with a one-proportion z-test:
$$
\begin{align}
z = \frac{\hat{p} - p}{\sigma} && \sigma = \sqrt{\frac{pq}{n}}
\end{align}
$$

The code for calculating z-score is `z = ((len(websites_using_aws)/n) - p)/sd`. For standard deviation, it's `sd = math.sqrt((p*q)/2)`.
Once we calculate our z-score, finally we can get our p-value. The library SciPy offers statistics which will allow us to calculate p-value similar to how a calculator would. Once complete, we can do hypothesis testing using a significance level of 5%.


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
sd = math.sqrt((p*q)/n)
z = ((len(websites_using_aws)/n) - p)/sd

p_value = st.norm.cdf(z)
  
print('P: %f\tQ: %f\nNP: %f\tNQ: %f\n\nP-Hat: %f\n\nZ-Score: %f\nP-Value: %f\n' 
      % (p, q, n*p, n*q, (len(websites_using_aws)/n), z, p_value))

# Hypothesis Testing
significants_level = 0.05

if p_value <= (significants_level): print('\033[1mReject H0')
else: print('\033[1mFail to reject H0')
```

    P: 0.310000	Q: 0.690000
    NP: 310.000000	NQ: 690.000000
    
    P-Hat: 0.061000
    
    Z-Score: -17.025268
    P-Value: 0.000000
    
    [1mReject H0


## Interpretation

We've rejected the null-hypothesis as our P-Value is approximately zero. The claim made within the Forbes article is invalid according to this observational study as we can support the alternate hypothesis that Amazon does not have a 31% market share in cloud computing/hosting.

# Confidence Interval for 1-Proportion Sample

## Confidence Interval Based on Z-Test

This is a standard method to produce a confidence interval given the z-score, sample size, claimed population proportions and sample proportions produced by the one-proportion z-test.

We use the following equations to calculate the standard deviation and standard error for our confidence interval:
$$
\begin{align}
\sigma_p = \sqrt{\frac{pq}{n}} && SE_p = \sqrt{\frac{\hat{p}\hat{q}}{n}}
\end{align}
$$

Both $\hat{p}$ and $\hat{q}$ are simply $p$ and $q$ for the sample proportion. Using the z-score we calculated during our hypothesis test as our critical value, we can multipy it by our standard error to produce our margin of error, like so:
$$
ME = Z_c\sqrt{\frac{\hat{p}\hat{q}}{n}}
$$


```python
# Sample p (statistic) and q values
aws_p = len(websites_using_aws)/n
aws_q = 1 - aws_p

# Standard error for the sample proportion
se = math.sqrt((aws_p*aws_q)/n)

# Margin of error
me = z*se

print('Interval: (%f, %f)' % (aws_p + me, aws_p - me))
```

    Interval: (-0.067852, 0.189852)


## Interpretation

I am 95% confident that the true proportion of websites within the top one-million sites population is between -6.78% and 19%. By 95% confident I mean if the above procedures are reproduced with a sample size of 1,000, the proportion of websites within the sample that use AWS as their cloud provider will be between -6.78% and 19%. Because zero lies within our interval, the results of this study can be considered insignificant.
