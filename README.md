# Force DNS to Pi-hole on Unifi USG

1. Configure Pi-hole normally with static IP and DNS server enabled.
   Remember to set up your upstream DNS providers, eq. 1.1.1.1 for
   cloudflare.
2. Configure USG DHCP to advertise Pi-hole IP for DNS requests.
3. Download and modify [Advanced Config json file](/config.gateway.json).
   Replace ```_IP_PIHOLE_``` with the static address of pi-hole.
4. Enable the advanced config on USG.

## What does it do?

USG DHCP settings tell every device in your network that they should
use Pi-hole DNS. If any devices do not follow that advice and want to
instead use their own DNS address on port 53 UDP or port 53 TCP, those
DNS requests will be forwarded to Pi-hole anyhow. DNS Masquerade on USG
will lie to the devices that the DNS replies are exactly what they
asked for. This allows you to adblock even on annoying devices.

## Downsides?

- Masqueraded requests show as DNS requests from USG, this will mess up
  pi-hole statistics.
- Nothing is done to prevent DNS-over-HTTPS. So devices still could
  bypass your forced DNS. Consider adding firewall rules.
- This most likely will not work with IPv6.

## Any other notes?

Do not add secondary DNS address to USG DHCP configuration for redundancy
purposes. Devices will not use DNS#1 primarily and DNS#2 only after #1
fails. Devices will use both DNS randomly. For redundant pi-hole setup
check [Pihole Gravity Sync](https://github.com/vmstan/gravity-sync).