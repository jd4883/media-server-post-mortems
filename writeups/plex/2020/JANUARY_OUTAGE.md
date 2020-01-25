# January 2020 - Plex Server Outage for approximately 2 weeks
### Issues Found:
* Local access stopped working (i.e. <private_ip>.<plex_port>/web) would not load when queried on my local network
* Remote access consistently dropped and would not hold
### Issue Description:
* My plex server was inaccessible to most users unless using a reverse proxy DNS address to access the server
* Local access did not work correctly because my local address was inaccessible outside of my docker environment
* My service provider started blocking 32400 and no longer honors my port forwards for it.
### Steps to Resolution:
* Default external ports were changed for plex
* External port mappings on my router needed to be adjusted to use a non-standard ephemeral port
* several issues existed in my docker plex configuration which I've ran for approximately 2 years. Although it was
 painful, I removed my server association from my plex account, recreated my libraries and settings and although I am
  still building back up my libraries metadata and watch counts, everything works in an intended way.
* I switched from the out of date binhex plex image to the linuxserver.io plex image which is very well maintained
. This image performed significantly better for me than the arch based binhex image, despite not being built on a
 smaller footprint os like Alpine, Arch, or Minideb. 
### Workarounds:
* Using my traefik reverse proxy, because I could access the local address of the container, since the docker stack
 did not have internal communication issues I was able to use my custom DNS address to access my server
* Bypassing plex and accessing media in another way is another valid approach, though this doesn't really help ones
 external user base
### Lessons Learned:
* Just because your router has a port forward doesn't mean your ISP is honoring it. In my case, my ISP still appears
 to have blocked 32400. A random ephemeral port is a non-issue
* Plex has trouble with any configuration it can construe as a double NAT. In my case I had to put my firewall in
  passthrough mode instead of having it serve IP's behind my router/modem to eliminate this issue
* port scans are worth doing as a form of validation that external connections can work. I may put together a small
 monitoring tool to check my various port forwards and report to me if one or more are not working.
### Conclusion:
Apologies to my user base for any inconveniences, my overall design is a bit less generic and now more specific to my
 needs. I do not use predictable port mappings for inbound connections to my plex server, though I am able to nicely
  mask this within docker so that I did not have to write a non-standard port mapped configuration (i.e. external
   port -> internal plex port 32400).
