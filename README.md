# Blockchain-learning-example-in-Python
This is a step-by-step reiteration of  the "Learn Blockchain by building one" for starters with a raspberry pi 3B+ to avoid certain difficulties that might occur along the way.
##
First, update the system and install python3.6+ (I installed python3.6 in this example)

```
$ sudo apt-get update
$ sudo apt-get install python3.6
```

For the blockchain example, install pipenv and its requirements

```
$ pip install pipenv
$ pipenv install
```

Install Flask to set up server services to finish building up the required tools

```
$ pip install Flask==0.12.2 requests==2.18.4
```

For convenience purposes, this example requires a http client, but its function can be achieved by using cURL commands. It is also because Postman is not supported on raspberry pi 3B+, neither x32 nor x64 version.

Then we use git clone command to get the example python code (or you can just copy it in vnc window)

```
$ git clone https://github.com/dvf/blockchain.git
```
##
Now,  run vnc server, then we can run 2 or 3 servers using the commands below, but the command need to be typed in .local/bin path if the server cannot run in home direction (copy the  blockchain.py file first).

```
$ vncserver
$ pipenv run python blockchain.py
$ pipenv run python blockchain.py -p 5001
$ pipenv run python blockchain.py --port 5002
```

Now, we can use cURL to do post commands, for request commands, just visit the site address in the browser for convenience.
First, let's visit localhost:5000/mine to send a mining request.

It gives you the second block following the pre-generated block, stating the proof nonce, timestamp and transaction content, here we are rewarded with a coin for mining this block ourselves.
Next, we can post our own transaction intent to the coming block to register our transaction.
Because this example dose not verify how much coin you have or the address except for the proof of work, so the transaction content is just for demonstration purpose.
We can post our transaction according to the pre-set form:

```
$ curl -X POST -H "Content-Type: application/json" -d '{
"sender": "d4ee26eee15148ee92c6cd394edd974e",
"recipient": "someone-other-address",
"amount": 5
}' "http://localhost:5000/transactions/new"
```

To mine a new block, simply refresh the /mine page
We mine 2 new blocks on port 5000, then you can see the whole chain by visiting /chain, our transaction is also recorded by visiting localhost:5000/chain.

Our "server"port acts as an individual node in the internet, to interact with other nodes,use the following cURL command:

```
$ curl -X POST -H "Content-Type: application/json" -d '{"nodes":["http://127.0.0.1:5001"]}' http://localhost:5000/nodes/register
 Then port 5001 is in the network list of port 5000 as a new node.
```

We mine 3 new blocks on port 5001,then visit 5000/nodes/resolve to find out that the original chain replaced for being the shorter one.
##
To be notice: because the program’s flaw, making the chain on port 5000 longer won’t effect the nodes/resolve result on port 5001.
By adding port5000 to 5001 list using the similar command, they can interact like between real nodes, the longer chain will take dominant position. Same goes with 3 or more nodes.

```
$ curl -X POST -H "Content-Type: application/json" -d '{"nodes":["http://127.0.0.1:5000"]}' http://localhost:5001/nodes/register
```
##
This is the end of the example verification, during which, I encountered a lot more problems than I thought I would even with a guiding website. Hopefully, following my step-by-step action, you will not be bumping into other unnecessary problems. 
It might be because I read a lot of blockchain materials and examples, I think up to this point, this example gives a well-rounded information about blockchain and a very good sense for someone who is new to the subject by making interactions meaningful and gives you a feedback for what you do, especially combined with [the document the author provides.][1]
[1]:https://medium.com/@vanflymen/learn-blockchains-by-building-one-117428612f46
