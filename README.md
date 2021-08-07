# Cybertalents Screenshot Writeup

### Let's Start With Opening `http://ec2-54-184-225-39.us-west-2.compute.amazonaws.com/screenshot/`

#### I relaised that it's amazon ec2 server

### First: Getting The Source Code

![Website Picture](https://github.com/Ahmedwaleed22/cybertalents-screenshot-writeup/blob/master/pic1.png)

<p>As we can see it's a normal page with form but It has internal servers that can only be accessed from the local server</p>

### So let's try something like `https://google.com` and leave server as `http://internalapi1.local`

![Website Picture After Request](https://github.com/Ahmedwaleed22/cybertalents-screenshot-writeup/blob/master/pic2.png)

<p>As we can see there is an image not loading and we can only see the alt text so let's try another server</p>

![Website Picture After Second Request](https://github.com/Ahmedwaleed22/cybertalents-screenshot-writeup/blob/master/pic3.png)

<p>Same thing happens in all servers still we cannot see anything except image alt text.</p>
<p>So let's try to exploit the form with php filter vulnerability exploit</p>

### let's use some codes on the "server" parameter like `php://filter/convert.base64-encode/resource=internalapi4.local/../index.php`

![Website Picture After The Exploit](https://github.com/Ahmedwaleed22/cybertalents-screenshot-writeup/blob/master/pic4.png)

<p>We still can only see unloaded image with alt text showing</p>
<p>But let's try intercepting the request using burpsuite and send the request to repeater</p>

![Request In Burpsuite](https://github.com/Ahmedwaleed22/cybertalents-screenshot-writeup/blob/master/pic5.png)

<p>We still cannot see anything strange except that image name is encrypted as md5</p>
<p>I tried to decrypt it but I couldn't so let's try to intercept the request to it</p>

#### So I changed the request to `GET /screenshot/thumbs/c191cc0fd3c6fbb063e96b12c42a4e49.jpg`

<p>And I Got This</p>

![Requesting Image File](https://github.com/Ahmedwaleed22/cybertalents-screenshot-writeup/blob/master/pic6.png)

<p>As we can see we have got a big hash it looks like a base64 hash let's send it to the decoder and choose decode as base64</p>

![Decoding The Hash](https://github.com/Ahmedwaleed22/cybertalents-screenshot-writeup/blob/master/pic7.png)

<p>We got php and html code let's take this code to any text editor (I will use sublime) text and try to analyze it</p>

![Analyzing The Code](https://github.com/Ahmedwaleed22/cybertalents-screenshot-writeup/blob/master/pic8.png)

<p>After sometime of searching I realised the "$jpeg" variable it has a value of "Server!Host@Flag", and as we can see the challenge description says that the flag is the front server hostname</p>

<p>Flag: Server!Host@Flag</p>