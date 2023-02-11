# What is Intruder?                            

Intruder is Burp Suite's in-built fuzzing tool. It allows us to take a  request (usually captured in the Proxy before being passed into  Intruder) and use it as a template to send many more requests with  slightly altered values automatically. For example, by capturing a  request containing a login attempt, we could then configure Intruder to  swap out the username and password fields for values from a wordlist,  effectively allowing us to bruteforce the login form. Similarly, we  could pass in a fuzzing[1] wordlist and use Intruder to fuzz  for subdirectories, endpoints, or virtual hosts. This functionality is  very similar to that provided by command-line tools such as Wfuzz or  Ffuf.

Limitations aside, Intruder is still very useful, so it is well worth learning to use it properly.

------

Let's take a look at the Intruder interface:

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/4d8d5926216e90c046d9d8ee1025bb6f.png)

The first view we get is a relatively sparse interface that allows us to  choose our target. Assuming that we sent a request in from the Proxy (by using `Ctrl + I` or right-clicking and selecting "Send to Intruder"), this should already be populated for us.

There are four other Intruder sub-tabs:

- **Positions** allows us to select an Attack Type (we will cover these in an upcoming  task), as well as configure where in the request template we wish to  insert our payloads.
- **Payloads** allows us to select  values to insert into each of the positions we defined in the previous  sub-tab. For example, we may choose to load items in from a wordlist to  serve as payloads. How these get inserted into the template depends on  the attack type we chose in the Positions tab. There are many payload  types to choose from (anything from a simple wordlist to regexes based  on responses from the server). The Payloads sub-tab also allows us to  alter Intruder's behaviour with regards to payloads; for example, we can define pre-processing rules to apply to each payload (e.g. add a prefix or suffix, match and replace, or skip if the payload matches a defined  regex).
- **Resource Pool** is not particularly useful to  us in Burp Community. It allows us to divide our resources between  tasks. Burp Pro would allow us to run various types of automated tasks  in the background, which is where we may wish to manually allocate our  available memory and processing power between these automated tasks and  Intruder. Without access to these automated tasks, there is little point in using this, so we won't devote much time to it.
- As with most of the other Burp tools, Intruder allows us to configure attack behaviour in the **Options** sub-tab. The settings here apply primarily to how Burp handles results and how  Burp handles the attack itself. For example, we can choose to flag  requests that contain specified pieces of text or define how Burp  responds to redirect (3xx) responses.

## Positions

When we are looking to perform an attack with Intruder, the first thing we need to do is look at *positions.* Positions tell Intruder where to insert payloads (which we will look at in upcoming tasks).

Let's switch over to the Positions sub-tab:

![Screenshot of the positions sub-tab](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/c286bd89e92aab284460ab9302b3b18c.png)

Notice that Burp will attempt to determine the most likely places we may wish  to insert a payload automatically -- these are highlighted in green and  surrounded by silcrows (`§`).

On the right-hand side of the interface, we have the buttons labelled "Add §", "Clear §", and "Auto §": 

- **Add** lets us define new positions by highlighting them in the editor and clicking the button.
- **Clear** removes all defined positions, leaving us with a blank canvas to define our own.
- **Auto** attempts to select the most likely positions automatically; this is  useful if we cleared the default positions and want them back.

Here is a GIF demonstrating the process of adding, clearing, and automatically reselecting positions:

![GIF showing how to select positions](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/35caa3d4e70aae2084966b1928c3db5f.gif)

## Attack Types

- Sniper
- Battering ram
- Pitchfork
- Cluster bomb

### Sniper                            

Sniper is the first and most common attack type. 

When conducting a sniper attack, we provide *one* set of payloads. For example, this could be a single file containing a  wordlist or a range of numbers. From here on out, we will refer to a  list of items to be slotted into requests using the Burp Suite  terminology of a "Payload Set". Intruder will take each payload in a  payload set and put it into each defined position in turn.

Take a look at our example template from before:

Example Positions 

```web-idl
POST /support/login/ HTTP/1.1
Host: 10.10.253.253
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 37
Origin: http://10.10.253.253
Connection: close
Referer: http://10.10.253.253/support/login/
Upgrade-Insecure-Requests: 1

username=§pentester§&password=§Expl01ted§              
```

There are two positions defined here, targeting the `username` and `password` body parameters.

In a sniper attack, Intruder will take each position and substitute each payload into it in turn.

For example, let's assume we have a wordlist with three words in it: `burp`, `suite`, and `intruder`.

With the two positions that we have above, Intruder would use these words to make ***six\*** requests:

| **Request Number** | **Request Body**                       |
| ------------------ | -------------------------------------- |
| 1                  | `username=burp&password=Expl01ted`     |
| 2                  | `username=suite&password=Expl01ted`    |
| 3                  | `username=intruder&password=Expl01ted` |
| 4                  | `username=pentester&password=burp`     |
| 5                  | `username=pentester&password=suite`    |
| 6                  | `username=pentester&password=intruder` |

Notice how Intruder starts with the first position (`username`) and tries each of our payloads, then moves to the second position and  tries the same payloads again. We can calculate the number of requests  that Intruder Sniper will make as `requests = numberOfWords * numberOfPositions`.

This quality makes Sniper very good for single-position attacks (e.g. a password bruteforce if we know the username or *fuzzing for API endpoint*s).

### Battering Ram

Like Sniper, Battering ram takes *one* set of payloads (e.g. one wordlist). *Un*like Sniper, the Battering ram puts the same payload in *every* position rather than in each position in turn.

Let's use the same wordlist and example request as we did in the last task to illustrate this.

Example Positions 

```web-idl
POST /support/login/ HTTP/1.1
Host: 10.10.253.253
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 37
Origin: http://10.10.253.253
Connection: close
Referer: http://10.10.253.253/support/login/
Upgrade-Insecure-Requests: 1

username=§pentester§&password=§Expl01ted§              
```



If we use Battering ram to attack this, Intruder will take each payload and substitute it into every position *at once.*

With the two positions that we have above, Intruder would use the three words from before (`burp`, `suite`, and `intruder`) to make *three* requests:

| **Request Number** | **Request Body**                      |
| ------------------ | ------------------------------------- |
| 1                  | `username=burp&password=burp`         |
| 2                  | `username=suite&password=suite`       |
| 3                  | `username=intruder&password=intruder` |

As can be seen in the table, each item in our list of payloads gets put into *every* position for each request. True to the name, Battering ram just throws payloads at the target to see what sticks.

### Pitchfork

After Sniper, Pitchfork is the attack type you are most likely to  use. It may help to think of Pitchfork as being like having numerous  Snipers running simultaneously. Where Sniper uses *one* payload set (which it uses on every position simultaneously), Pitchfork uses one  payload set per position (up to a maximum of 20) and iterates through  them all at once.

This type of attack can take a little time to  get your head around, so let's use our bruteforce example from before,  but this time we need two wordlists:

- Our first wordlist will be usernames. It contains three entries: `joel`, `harriet`, `alex`.
- Let's say that Joel, Harriet, and Alex have had their passwords leaked: we know that Joel's password is `J03l`, Harriet's password is `Emma1815`, and Alex's password is `Sk1ll`.

We can use these two lists to perform a pitchfork attack on the login form from before. The process for carrying out this attack will not be  covered in this task, but you will get plenty of opportunities to  perform attacks like this later!

When using Intruder in pitchfork mode, the requests made would look something like this:

| **Request Number** | **Request Body**                     |
| ------------------ | ------------------------------------ |
| 1                  | `username=joel&password=J03l`        |
| 2                  | `username=harriet&password=Emma1815` |
| 3                  | `username=alex&password=Sk1ll`       |

See how Pitchfork takes the first item from each list and puts them into  the request, one per position? It then repeats this for the next  request: taking the second item from each list and substituting it into  the template. Intruder will keep doing this until one (or all) of the  lists run out. Ideally, our payload sets should be identical lengths  when working in Pitchfork, as Intruder will stop testing as soon as one  of the lists is complete. For example, if we have two lists, one with  100 lines and one with 90 lines, Intruder will only make 90 requests,  and the final ten items in the first list will not get tested.    

This attack type is exceptionally useful when forming things like credential stuffing attacks (we have just encountered a small-scale version of  this). We will be looking more into these later in the room.

###  Cluster Bomb

Like Pitchfork, Cluster bomb allows us to choose multiple payload  sets: one per position, up to a maximum of 20; however, whilst Pitchfork iterates through each payload set simultaneously, Cluster bomb iterates through each payload set individually, making sure that every possible  combination of payloads is tested.

Again, the best way to visualise this is with an example.

Let's use the same wordlists as before:

- Usernames: `joel`, `harriet`, `alex`.
- Passwords: `J03l`, `Emma1815`, `Sk1ll`.

But, this time, let's assume that we don't know which password belongs to  which user. We have three users and three passwords, but we don't know  how to match them up. In this case, we would use a cluster bomb attack;  this will try *every* combination of values. The request table for our username and password positions looks something like this:

| **Request Number** | **Request Body**                     |
| ------------------ | ------------------------------------ |
| 1                  | `username=joel&password=J03l`        |
| 2                  | `username=harriet&password=J03l`     |
| 3                  | `username=alex&password=J03l`        |
| 4                  | `username=joel&password=Emma1815`    |
| 5                  | `username=harriet&password=Emma1815` |
| 6                  | `username=alex&password=Emma1815`    |
| 7                  | `username=joel&password=Sk1ll`       |
| 8                  | `username=harriet&password=Sk1ll`    |
| 9                  | `username=alex&password=Sk1ll`       |

Cluster Bomb will iterate through every combination of the provided payload  sets to ensure that every possibility has been tested. This attack-type  can create a *huge* amount of traffic (*equal to the number of lines in each payload set multiplied together*), so be careful! Equally, when using Burp Community and its Intruder  rate-limiting, be aware that a Cluster Bomb attack with any moderately  sized payload set will take an incredibly long time.

That said, this is another extremely useful attack type for any kind of credential bruteforcing where a username isn't known.

## Payloads

Switch over to the "Payloads" sub-tab; this is split into four sections:

- The 

  Payload Sets

   section allows us to choose which position we want to configure a set  for as well as what type of payload we would like to use. 

  - When  we use an attack type that only allows for a single payload set (i.e.  Sniper or Battering Ram), the dropdown menu for "Payload Set" will only  have one option, regardless of how many positions we have defined.
  - If we are using one of the attack types that use multiple payload sets  (i.e. Pitchfork or Cluster Bomb), then there will be one item in the  dropdown for each position.
    ***Note:** Multiple positions should be read from top to bottom, then left to right when being assigned  numbers in the "Payload set" dropdown. For example, with two positions (*`username=§pentester§&password=§Expl01ted§`*), the first item in the payload set dropdown would refer to the username  field, and the second would refer to the password field.
    *

  The second dropdown in this section allows us to select a "payload type".  By default, this is a "Simple list" -- which, as the name suggests, lets us load in a wordlist to use. There are many other payload types  available -- some common ones include: 

  ```
  Recursive Grep
  ```

  , 

  ```
  Numbers
  ```

  , and 

  ```
  Username generator
  ```

  . It is well worth perusing this list to get a feel for the wide range of options available.

- **Payload Options** differ depending on the payload type we select for the current payload  set. For example, a "Simple List" payload type will give us a box to add and remove payloads to and from the set:
  ![Screenshot of the payload options for a simple list](https://tryhackme-images.s3.amazonaws.com/user-uploads/5d9e176315f8850e719252ed/room-content/aa7a5c278455b7f3066410b941354448.png)
  We can do this manually using the "Add" text box, paste lines in with  "Paste", or "Load..." from a file. The "Remove" button removes the  currently selected line *only*. The "Clear" button clears the entire list. Be warned: loading extremely large lists in here can cause Burp to crash!
  By contrast, the options for a `Numbers` payload type allows us to change options such as the range of numbers used and the base that we are working with.

  

- **Payload Processing** allows us to define rules to be applied to each payload in the set  before being sent to the target. For example, we could capitalise every  word or skip the payload if it matches a regex. You may not use this  section particularly regularly, but you will definitely appreciate it  when you *do* need it!

  

- Finally, we have the **Payload Encoding** section. This section allows us to override the default URL encoding  options that are applied automatically to allow for the safe  transmission of our payload. Sometimes it can be beneficial to *not* URL encode these standard "unsafe" characters,  which is where this  section comes in. We can either adjust the list of characters to be  encoded or outright uncheck the "URL-encode these characters" checkbox. 

When combined, these sections allow us to perfectly tailor our payload sets for any attack we wish to carry out.