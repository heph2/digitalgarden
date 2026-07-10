---
{"dg-publish":true,"permalink":"/port-knocking-on-i-os/"}
---

Recently I've subscribed to Dimensione Mobile, which is one of the few mobile providers that grant IPv6 connectivity, even with some limits due to the nature of mobile connections.
This finally allows me to have full IPv6 connectivity both from home and from my mobile phone/laptop. Currently I've a setup with all my LAN machines reachable through an IPv6 subnet, only defended by the router firewall. This is quite convenient for hosting stuff, but it imposes some limits when trying to reach things if the firewall blocks every incoming connection.

The only incoming connections that I accept are those from my VPS, through an IP whitelist (only IPv6). This is totally fine for my home setup, but what if I need to reach my services from my mobile phone? Well, usually the answer is: "Use a VPN!", and yes, it's totally fine and probably the safest way to reach internal-only services. But with native IPv6 available I want to try another approach, which is Port Knocking.

Basically, for those who don't know how Port Knocking works, it's the IT equivalent of having a secret combination when knocking on a door; you can surely remember Sheldon from The Big Bang Theory knocking at Penny's door with a specific pattern.
You can do the same with a combination of IP:Port. For example, you can open a TCP connection 42 times to the router's IP on port 4242. If the firewall supports port knocking (which Mikrotik routers do), you can grant the IP that guessed the port knocking sequence access to the LAN machines, adding the current IP to a whitelist.

### Implementing Port Knocking on iOS

There are a few port knocking apps, but I would love to use the side button to trigger an automatic port knocking sequence through a Shortcut. Basically I'm using the app Scriptable, which allows you to execute generic JS scripts and also exposes some URLs that can be used by Shortcuts. I can easily trigger the shortcut from the side button or simply by talking to Siri and telling her to "Knock Knock".

```
const host="myrouter.ip";
const ports=[1234,5678];

for (const port of ports) {
	const req = new Request(`http://${host}:${port}/`);
	req.timeoutInterval = 1;
	
	try {
		await req.load();
	} catch (e) {
		console.log(`Port ${port}: error: ${e}`);
	}
	
	await new Promise(r => Timer.schedule(300, false, r));
}

console.log("Knock sequence finished");
```

Basically this script just knocks two times on the ports `1234` and `5678`, and if the firewall is configured to handle port knocking, it will add the current IP to a whitelist.
