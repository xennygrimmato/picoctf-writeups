# looooong

The problem statement says:

```
I heard you have some "delusions of grandeur" about your typing speed.
How fast can you go at shell2017.picoctf.com:30277?
```

On doing a netcat at `shell2017.picoctf.com` on port `30277`, we get the
following message from the server:

```
To prove your skills, you must pass this test.
Please give me the 'v' character '561' times, followed by a single '9'.
To make things interesting, you have 30 seconds.
Input:
```

It turns out that the server asks for a random character each time.
The next time I did a netcat, I got:

```
To prove your skills, you must pass this test.
Please give me the 'F' character '777' times, followed by a single '9'.
To make things interesting, you have 30 seconds.
Input:
```

## Solution

Create a socket to the given server and port.
Receive a few bytes from the server, enough to get the entire message.

Parse this message to see which character is to be sent how many times to
the server.

Create a string based on this message and send those bytes to the server.

Call the receive function to get the flag from the server.
