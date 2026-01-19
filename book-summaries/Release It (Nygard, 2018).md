# Release It\! (Nygard, 2018\)

This is unexpectedly good reading, and not what I expected.

It's essentially about the Production side rather than the Development side, and starts with the various gotchas and trapdoors that can catch you out when you deploy into production. It goes into the real nitty gritty, for example explaining how a TCP connection is set up and how a firewall can interfere in such a way that it quietly kills your application. (TCP connections are effectively ever-living until you close them, only a firewall can flag a TCP connection as "idle" if no traffic flows such that when you do try to send a request the firewall bins it without sending any response at all. If a thread is blocked while it waits for a response, it will stay blocked indefinitely. Once that happens on all of your threads, your application is as good as dead.) I like this level of detail and am learning a lot.

One other gotcha is the "Dogpile" whereby conditions arise that lead everything to try to do the same thing all at once, overloading the system such that it crashes. The example given was of a company that experienced a power cut and, when the power was restored, all of their hardware tried to power up at the same moment which overloaded the circuit and tripped the breaker again. (This exact thing happened at Wright Dose recently.) The proposed solution is to introduce a random delay in each device such that requests do not flood in all at once. (This is what I had to do at WDL.)

