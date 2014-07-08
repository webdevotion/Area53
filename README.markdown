# Area53
A python interface for AWS Route53.

 * Original Author: Marius Voila
 * Website: [http://www.mariusv.com](http://www.mariusv.com)
 * Blog Release: [An introduction to Are53](http://www.mariusv.com/python-interface-for-aws-route53/)

## Installation
Run these commands to install:

```bash
git clone git@github.com:bigmlcom/Area53.git
cd Area53
sudo python setup.py install
```

You also need to set the following environment variables:

```bash
export AWS_ACCESS_KEY_ID="<Insert your AWS Access Key>"
export AWS_SECRET_ACCESS_KEY="<Insert your AWS Secret Key>"
```

## Usage
I wrote this library to make dealing with Amazon's Route53 DNS service a lot easier.
Now you can write code like the following:

```python
>>> from area53 import route53
>>> zone = route53.create_zone('example.com')                 # Creates the zone, example.com.
>>> zone.add_a('example.com', '182.12.142.12')                # Adds A record to the zone.
>>> zone.add_cname('www.example.com', 'example.com')          # Adds CNAME record to the zone.
>>> zone.add_mx(['10 mx1.example.com', '20 mx2.example.com']) # Adds MX records to the zone.
```

Now what just happended.  On line two we create the zone.  Then we create 
an A record for the naked domain, followed by a cname for the ‘www’ subdomain 
which points back to the naked domain.  Then we also added two MX records 
for our mail exchangers.  Also, you don’t need to worry about trailing dots 
and fully qualified domain names, because that is handled automatically.

Now, we can grab a list of all zones like this: 

```python
>>>  route53.get_zones() # Get all hosted zones.
[<Zone:example.com.>, <Zone:bluepines.org.>]
```

Or we can grab our individual zone by name:

```python
>>> zone = route53.get_zone('example.com')
```

You can also look at what records you have in your zone:

```python
>>> for record in zone.get_records():
...     print record
...
<Record:A:example.com.:[u'182.12.142.12']>
<Record:CNAME:www.example.com.:['example.com.']>
<Record:MX:example.com.:[u'10 mx1.example.com.', u'20 mx2.example.com.']>
<Record:NS:example.com.:[u'ns-1249.awsdns-28.org.', u'ns-902.awsdns-48.net.']>
<Record:SOA:example.com.[u'ns-1249.awsdns-28.org. awsdns-hostmaster.amazon.com. 1 7200 900 1209600 86400']>
```

And here is how you grab just the nameservers for your zone:

```python
>>> zone.get_nameservers() # Get nameservers for the zone.
[u'ns-1249.awsdns-28.org.', u'ns-902.awsdns-48.net.']
```

You add the nameservers to your domain 
registrar to actually switch to Amazon’s DNS service.  I would recommend 
migrating a non-critical domain to start to make sure that you understand 
the process before moving your big domain with hundreds of records over to Route53. 

Now we can remove all these records and the zone itself like so:

```python
>>> zone = route53.get_zone('example.com') # Get the zone.
>>> zone.delete_a('example.com')           # Delete A record from the zone.
>>> zone.delete_cname('www.example.com')   # Delete CNAME record from the zone.
>>> zone.delete_mx()                       # Delete MX records from the zone.
>>> zone.delete()                          # Delete the zone itself.
```

If anyone has any questions, bugs or issues then feel free to ask.  
Hopefully, this will make your life with Route53 significantly easier.

## Dependencies
boto - must have at least version 2.0

