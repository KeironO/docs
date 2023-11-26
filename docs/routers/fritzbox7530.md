# FRITZ!Box 7530

## How to flush the DNS

I recently got into a bit of a shit situation with my DNS/DHCP setup, and had to reset DNS. After much trial and tribulation, I found out that the router was the issue.

The solution is to:

1. Access your routers administrator portal (usually http://192.168.178.1) and log in.
2. On the left sidebar, click 'Home Network'.
3. On the Home Network pane, you should see a Tab entry for 'Network Settings'. Click that.
4. Scroll down and click 'Additional Settings'. You should see more entries appear now.
5. Scroll down again down to IP Addresses, and then click the 'IPv4 Settings' button.
6. Once you're in there, untick the 'Enable DHCP server' button and click Apply.
7. Wait 20 seconds.
8. Come back in and re-enable the DHCP Server.

## Turning off WPS

Annoyingly WPS is enabled by default on this router.

WPS typically uses an eight-digit PIN to authenticate devices. However, the PIN can be vulnerable to brute-force attacks, where an attacker systematically tries all possible combinations until the correct PIN is found. The eight-digit PIN provides only 10^8 possible combinations, making it susceptible to relatively quick brute-force attacks. 

It's best to disable it.

1. Log into the admin account.
2. Navigate to Wireless, and then Security in the Menu.
3. Disable the "WPS (Wi-Fi Protected Setup)" option.

## I have no idea where my router even is?

The Fritz!Box has a neat little feature where anything connected to it can ALWAYS connect to it using the following IP address.

169.254.1.1

Pretty neat!
