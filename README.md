# smart-pardot-lists
Create lists in Pardot that make use of your full Salesforce database

Sample code 

from smart_pardot_list import SPList
from pypardot.client import PardotAPI
from simple_salesforce import Salesforce

# Connect to Pardot
p = PardotAPI(
    email=YOUR_EMAIL,
    password=YOUR_PASSWORD,
    user_key=YOUR_USER_KEY)
p.authenticate()

# Connect to Salesforce
sf = Salesforce(
    username=YOUR_USERNAME,
    password=YOUR_PASSWORD,
    security_token=YOUR_SECURITY_TOKEN)

# Define variables
sObject = 'CampaignMember'
pardotListID = '7351'
# List of Salesforce campaigns to search through
campaigns = ['701610000008Aoo','701610000008Aop','701610000008Aoq','701610000005gM8']
# Format the list of campaigns in proper format for SOQL
ids = "("
for id in campaigns[:-1]:
    ids = ids + "'{id}',".format(id=id)
ids = ids + "'" + campaigns[-1] + "')"
# Put formatted list of campaigns in WHERE statement for SOQL query 
conditions = "Type__c = 'Candidate' and CampaignID in {ids}".format(ids=ids)
# Call SPList and print return value
print(SPList(p,sf,sObject,conditions,pardotListID))
