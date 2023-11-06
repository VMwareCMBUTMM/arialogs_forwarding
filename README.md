# arialogs_forwarding
Aria Logs content packs with queries that can be used to forward events to other destinations. 

These content packs each contain four queries that can be used in an Aria Logs forwarding rule to forward vCenter and ESX audit and authentication events to a 3rd party log aggregator like Splunk.

The queries only use regex and static fields to pull the relevant events, so they can be used in the log forwarding section of both Aria Logs SaaS and on-prem. I tried to collect as many key auditing and authentication events as possible in a single query, to limit the number of forwarding rules we use, so we don't hit the forwarder rule configuration limits.

Events that are pulled for forwarding:<br>

vCenter Audit Events:<br>
  User created or deleted<br>
  User permissions changed<br>
  VM reconfigured<br>
  VM created or deleted<br>
  VM console opened (ticket aquired)<br>
          
vCenter Authentication Events:<br>
  Failed login attempt<br>
  Sucessful login attempt<br>

ESX Audit Events:<br>
  Host changes (services start/stopped, host firewall change, changes to ESX image software, etc...)<br>
  Shell commands run by logged in user<br>
  ESX user created/deleted<br>

ESX Authentication Events:<br>
  Failed SSH login attempt<br>
  Successful SSH login attempt<br>
  Successful web client login/logout attempt<br>

  
         

Usage:<br>
Import the content pack for the version of Aria Logs you are using (SaaS or on-prem). For Aria Logs SaaS, you can then mark these imported queries as "favorite queries", and then pull them up in the log forwarding section, and they should autofill into the forwarding query field. Aria Logs on-prem doesn't have this feature so you will have to pull up the forwarding queries in log explorer and copy/paste them manually into a new event forwarding rule.

IMPORTANT - Some of these queries use regex, which is very CPU-costly for Aria Logs to process, but it was the only way I could grab as much info as possible in one query. If you have a larger environment and try to run a regex heavy query in log explorer (Like the ESX authentication one), it might take a long time to return any results, if it does at all. The smaller the time window the easier it is for the system to process the query. If you are trying to see what the results of one of these queries is before you hit apply to start forwarding, and nothing comes up after a while, I recommend you just click 'Save' and make sure the events are flowing into your 3rd party logging system.

These queries come with zero support and might or might not work in your environment, but they have been tested to work in a lab. This is not VMware branded or supported code, so if it causes issues in your environment, neither I or VMware are responsible. It is recommended you test these queries in a test environment first to make sure they collect the logs you are looking to forward, and they dont slow down your Aria Logs instances noticeably.
