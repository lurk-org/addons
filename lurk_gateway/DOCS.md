# Lurk Gateway

Local Home Assistant manager and the single channel to the Lurk cloud.

## Enrollment

Nothing is typed here in the normal case. The factory writes a one-time
**enrollment token** into this add-on's options. On first boot the add-on
generates a private key that never leaves the box and registers its public
key with the cloud together with the unit's hardware serial. From then on
that key is the unit's identity: it signs the proof the cloud checks on every
link connection, and it is what the Lurk app pins when it connects over the
local network.

The token is burned by the cloud on use. There is nothing to rotate, recover
or retype afterwards. The panel shows "Enrolling…" while the first boot is
working and keeps retrying if the network is not up yet.

The identity lives in `/addon_configs/lurk_gateway` (mounted as `/config`
inside the add-on): `hub.key`, `hub.crt` (the self-signed certificate the
key serves on the local network), `identity.json` (`gateway_id`, `serial`,
`link_url`) and `ha-owner.json`. Everything here survives add-on updates and
an uninstall. Only removing the add-on's configuration explicitly, or
reflashing the operating system, deletes it.

A token the cloud refuses on an already-enrolled unit is dropped at once
(tokens are one-time, so retrying could never succeed); the panel says so and
the unit keeps running on its existing identity until support issues a new
token.

The **claim code** on the welcome card is a different secret, and it decides
who OWNS the unit. It belongs in the customer's app, never on this screen.
Ownership is separate from enrollment: a gateway can be online, updatable and
supportable while belonging to nobody, and it publishes no household data
until somebody claims it.

## Owning the unit

The code from the welcome card is typed into the Lurk app. The cloud turns it
into a ticket signed for this unit and good for ten minutes, and the app
hands that ticket to the unit over the local network (`POST /claim`, the one
ownership call served without a credential, because nobody holds one for
this unit yet). The add-on relays the ticket up its link and holds the
request open until the cloud answers: being on the unit's network is the
proof, and the cloud decides. On success the cloud creates the site, records
the claimer as its owner, and sends the auth document back down the same
socket. The link restarts owned, the household starts syncing, and every
later phone joins by invitation in the app rather than by claiming again.

## The Home Assistant login

A factory-built unit is onboarded at the station with one Home Assistant
owner account, `lurk`, and a random password. Nobody at the factory keeps
it: the gateway receives it once, inside the enrollment response, kept in
`/config/ha-owner.json` (readable by root only), and **immediately sets a
new random password, deletes every other Home Assistant login and every
account other than `lurk`**, then reports the new password to the cloud on
its first connection. Whatever the station -- or anyone on the
factory floor -- knew or created is dead seconds after enrollment. Whoever
owns the unit in the Lurk app can see the current login there and sign in
to Home Assistant directly.

On release the add-on does the same as the owner: a fresh random password,
handed to the cloud, and every other login and account removed, so the
previous household keeps nothing. If Home Assistant refuses, the release
still completes and the failure is reported to the cloud like any other
leftover. A unit provisioned by hand has no such file and nothing is rotated.

**Lost welcome card.** Support issues a new claim code from the fleet page
and prints a new card; the key and the enrollment are untouched, and the
old code stops working. Nothing is typed on the box.

## Recovery (only if the identity is gone)

After a full reflash the add-on has no key. Support issues a fresh
enrollment token for this serial from the fleet page; the "Recovery" card
accepts it and the add-on enrolls again exactly as it did at the factory.
The unit keeps its owner — the key changed, the ownership record in the
cloud did not.

## When the cloud refuses the unit

If the cloud closes the link on this unit the panel says so. The add-on
cannot tell suspended from retired from reissued, and none of those is
something it can fix on its own: it keeps trying at a slow cadence and leaves
the remedy to whoever reads the fleet page. A serial mismatch (a backup
restored onto a different board) quarantines the stored identity and waits
for a token.

## Updates

You update the hub from the Lurk app, and support updates one or many from
the fleet page. The cloud sends the hub a single instruction carrying a
read-only registry login that stops working fifteen minutes later. The
add-on registers that login with the Supervisor, asks Home Assistant to
install the newer version, restarts into it, removes the login and reports
the outcome. Nothing about the login is kept on the hub: not in the logs,
not in its files. A hub that is offline, already on the newest version, or
in the middle of an update is refused before anything is sent, and the app
says which.

## Release

Releasing hands the unit to somebody else. The signal is the cloud's auth
document: one that names nobody, where the unit held a document that named
an owner, is the release. The add-on deletes the previous household's Home
Assistant data — integrations (and with them the Zigbee network and any
camera configuration), areas, floors, labels, people, automations, scenes
and scripts — forgets its own caches, and reports the outcome up the link.
The key stays: same unit, new owner. The next owner claims it with the same
welcome card.

**What a release does not do.** An add-on cannot factory-reset Home Assistant
OS. It clears everything reachable through Home Assistant's own API and
nothing below it — no disk wipe, no OS-level state, no other add-ons' data.
That covers a private resale between people who broadly trust each other. A
unit going to a stranger, or leaving a business, should be reflashed.

Two ways to run it:

- **The Lurk app, or support on the fleet page** — the site is released in
  the cloud, the link reconnects, and the document it is welcomed with names
  nobody. The add-on wipes on that document and reports what it could not
  remove back up the link.
- **The Lurk panel in Home Assistant** — reaching it takes a Home Assistant
  admin session on the machine itself, which is the closest thing to a
  physical button hold that software offers. It is deliberately unreachable
  from the LAN: a credential good enough to turn on a light must not also be
  good enough to erase the house.
