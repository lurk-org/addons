# Changelog

## 0.9.1

- The Lurk integration reaches the gateway over TLS, as the LAN port serves
  it, so it sets up instead of retrying.
- An action record's `before` is the followed entity's row, the one its
  `after` shows.
- The Auto Pulse integration holds its report subscription from its setup,
  so a kit's instant report is never missed.
- The assistant's control tool takes an entity's name as its target, so a
  spoken name is acted on instead of answered as not found.

## 0.9.0

- Lurk has its own presence inside Home Assistant: the gateway installs the
  Lurk integration from the catalog, restarts Home Assistant once for it,
  stages every later install for a quiet window or a tap in the app, and
  sets it up with no person at the box.
- A connector's semantics bind meaning to entities per device, so the
  assistant answers from roles, a role's risk tightens an action's tier,
  and an action is verified by the role it changes, refused by a
  precondition or a cooldown with the reason.
- One tool table on the gateway answers the app, Home Assistant's own
  assistants and Assist; an Assist turn runs on the gateway for the person
  it came from, with the cloud only forwarding the model step.
- A vendor's own service passes the gate through its semantics, with its
  parameters checked and its report in the answer.
- A cloud integration signs in with Lurk's own OAuth client, with no secret
  on the box; Google is the first provider.
- An owner links each person to their Home Assistant user, and an action
  Assist could not finish reaches the app as a hold.

## 0.8.0

- The gateway speaks Home Assistant's own entities: the structure lists its
  floors, areas, devices, entities, config entries, subentries and labels,
  and the app draws each entity with its domain's widget.
- Every control passes one gate: each entity and each action has a tier
  (read, act, confirm, deny), an owner can tighten an entity's tier, and a
  confirm action needs a hold.
- Every action is followed to its outcome (confirmed, unconfirmed, failed
  or timed out) and written to an action record on the gateway, which keeps
  codes and passwords out of it.
- The store offers every integration Home Assistant has, and an owner adds,
  reconfigures and removes them, with their subentries and repairs, through
  Home Assistant's own setup flows.
- A discovery or an expired login waits as a card on Today until the owner
  sets it up, ignores it or signs in again.

## 0.7.5

- A version to update to: nothing changes in the add-on. It is the first
  update the 0.7.4 updater runs, and the first whose progress Lurk shows
  live, installing with its percentage, then restarting, then done.

## 0.7.4

- An update no longer fails when the store is slow to reload: the hub waits
  up to five minutes for the Supervisor, and a reload that still does not
  finish is logged and never ends the update.
- A hub whose add-on was installed since Home Assistant Core last started
  has no update entity, so its first update restarts Core, waits for the
  entity and installs through it, instead of failing and asking for a
  restart by hand.

## 0.7.3

- The hub binds Lurk's catalog to Home Assistant's registries and publishes
  things: its structure names the catalog it bound with, and the LAN and the
  cloud serve exactly the entities those things bind. The image carries its
  own catalog and the hub takes the cloud's newer signed one at `welcome`. A
  hub on 0.7.2 sends a structure with no catalog, which the app cannot draw.
- Every control, on the LAN, through the cloud and by voice, passes the
  hub's allow-list, and a held action needs a confirm id.
- The owner adds, configures and removes connectors through Home Assistant's
  own flows, and an update brings the hub's custom integrations to the
  catalog's version with one Home Assistant restart.
- Anyone on a site reads an entity's history from the hub's recorder, and
  every row carries the last state an entity held before it went away.
- The structure carries the temperature unit, the icon an owner chose for
  each entity and what the owner hid; the owner renames, moves, gives an
  icon to and hides an entity.
- No LAN bearer, frame or password reaches the hub's log.

## 0.7.2

- A control answers with the state the device reached, not a state read
  before it moved: the hub waits up to three seconds for Home Assistant to
  report the change, and acks the cloud's intent or answers the LAN call
  with that row, or with none when the device reported nothing in time. A
  device behind a cloud integration no longer flickers back on the phone.

## 0.7.1

- An update the hub cannot run through Home Assistant fails saying why: Home
  Assistant has no update entity for the add-on yet, which it creates when
  Core next starts. The hub no longer asks the Supervisor for a self-update,
  a request the Supervisor always refuses.

## 0.7.0

- The hub speaks the link's renamed instants: `sent_at` on the welcome,
  `created_at` on an intent, `updated_at`, `changed_at` and `occurred_at` on
  what it sends, every one a millisecond in UTC, and every id it mints a
  UUIDv7 from its one clock. A hub on 0.6.1 keeps its link through the
  cloud's bridge until it takes this update.
- Every member of the auth document is a UUID and its `not_before_at` is an
  instant or null.

## 0.6.1

- The hub sends Lurk its Home Assistant login at every connection until the
  cloud acks one of those reports, so a link that drops before the ack no
  longer leaves the app showing a login the hub has replaced.
- A registry login the Supervisor refuses leaves nothing registered behind it,
  and the log says what happened without quoting the login.

## 0.6.0

- The add-on takes the `update` intent: it registers the fifteen-minute
  registry login the intent carries in the Supervisor, has Home Assistant
  install the newer version, removes the login again whichever way the
  install ends, and acks once, after its own restart. Nothing of the login
  reaches the state file or the log.
- The daily pass refreshes the store and installs nothing: this add-on moves
  only when the owner or staff ask for it, the OS and Core only through the
  panel's action.
- The hub tells Lurk its Home Assistant login as soon as it is connected
  after setup, so the login the app shows is the one the hub accepts.

## 0.5.0

- The link carries the auth document, the claim and the reset and credential
  reports; gateway tokens are verified against the document's key; the
  cloud's clock stamps documents and judges intents; a 1011 keeps the
  backoff; the LAN socket dedupes by `X-Lurk-Client`; the JWKS fetch, `did`
  and the Device CA are gone.
- The cloud link carries documents, states, events and intents; MQTT,
  shadows and the config selection are gone.

## 0.4.0

- The add-on lives in the `lurk` monorepo under `gateway/`; the image is
  `registry.digitalocean.com/lurk/gateway`, built on a `gateway-vX.Y.Z` tag.
  Boxes pull it with a read-only registry token. No code change.

## 0.3.23

- The voice relay logs the events that shape a turn without logging any
  content: an `interrupted`, a `toolCallCancellation`, a `goAway`, and how
  long each tool answer took.

## 0.3.22

- `control_device` takes the service name `home_state` advertises (`turn_off`,
  not `fan.turn_off`; the dotted form still works) and builds the call from
  the entity's own domain. `home_state` answers one text line per entity
  instead of JSON rows, a third of the tokens; a control answers with that
  entity's line, so the same attributes are withheld on both. Target keys in
  `params` can no longer retarget a call.

## 0.3.21

- A mirror collection the hub has never published, such as `services` on
  first boot after an upgrade, is published on connect instead of waiting for
  the periodic pass.

## 0.3.20

- Until the service registry is known, the hub reports no `actions` at all
  rather than an empty list, so the cloud falls back instead of refusing
  everything; once the registry arrives every LAN row is re-sent. A failed
  `get_services` is logged as a warning.

## 0.3.19

- Every device row carries `actions`: the Home Assistant services this
  entity qualifies for, computed from its `supported_features` against the
  service registry (`get_services`). The structure carries `services`, the
  registry's fields and selectors for the domains present, and the cloud
  mirror gains a `services` collection. `home_state` rows carry `actions`
  too, and a service call Home Assistant refuses comes back with its own
  error text.

## 0.3.18

- `home_state` answers with every entity Home Assistant has, not a domain
  list, and `control_device` accepts any service in the entity's own domain.
  Camera access tokens and GPS coordinates are the only attributes withheld.

## 0.3.17

- `home_state` now answers with sensors and binary sensors as well as the
  controllable domains, each with its name and room, read live from Home
  Assistant. Every tool call is logged by name.
- A `/v1/state` send that loses the race with its own eviction ends the
  connection quietly instead of logging a traceback.

## 0.3.16

- The voice relay answers Lurk's tool calls on the box: `home_state` from the
  LAN cache and the structure, `control_device` through the same path as the
  LAN control API, with the member's role enforced. The phone never sees the
  call (`docs/contract-voice.md` › Relay).
- A `/v1/state` socket evicted by its own handset's reconnect no longer logs a
  traceback: a send after the close now ends that connection as a plain
  disconnect.

## 0.3.15

- `WS /v1/voice`: the hub relays a phone's Gemini Live session on the LAN.
  Edge JWT bearer, a `start` frame carrying the minted socket URL, then an
  opaque relay: the hub never reads a frame. See `docs/contract-voice.md`.

## 0.3.14

- Build: `BUILD_VERSION` has no default in the Dockerfile. It was pinned at
  0.3.2 and eleven releases stale, so a locally built image was labelled
  `io.hass.version=0.3.2`. CI passes the tag and the Supervisor passes
  config.yaml's version, so the default was only ever wrong.
- Nothing changes for a unit: the published image already carried the right
  label. This is the first release cut by `scripts/release.sh`, and proving
  that path is the reason it exists.

## 0.3.3 to 0.3.13

- Updating a hub from the app works with nothing typed on the box. The
  cloud's "update now" reaches the add-on, every pass refreshes the add-on
  store first so a version published minutes earlier is visible, the install
  is asked of Home Assistant (the Supervisor refuses an add-on that asks to
  update itself), a slow image pull has twenty minutes instead of twenty
  seconds, and a failure always carries a reason.
- An update may carry `scope: "addon"`, which moves this add-on only and
  leaves Home Assistant OS and Core alone; the restart the install causes
  resumes the same pass.
- `auto_update` is switched off for this add-on at startup: a hub moves when
  its owner asks, not on Home Assistant's own schedule.
- The version Lurk compares against the store is what the Supervisor reports
  installed, so the app never hides an update or offers one forever.
- A role change made in the app reaches the local API with the next auth
  document instead of the next token.

## 0.3.2

- Add-on: the optional `serial` no longer ships with a null default, which
  Home Assistant Supervisor rejected on real Raspberry Pi units.
- Station: Supervisor validation errors redact the one-time enrollment token.
- Station: the box check reads the whole `/health` answer; a body that arrived
  after the headers was dropped and reported as "this box says it is ?".

## 0.3.1

- Station: Supervisor calls that take longer than ten seconds (image install,
  restart) no longer time out in Home Assistant's relay.

## 0.3.0

- Image pulled from `registry.lurk.site` with a per-unit credential the hub
  registers itself at enrollment; the Supervisor updates the add-on automatically.
- The Home Assistant owner password is rotated and all sessions and extra users
  are purged the moment the unit enrolls, and again on release.
- The Ingress bind accepts only the Supervisor; a stale enrollment token is
  dropped on the first refusal.

## 0.2.0

- Factory station script: onboarding, mint, enrollment and card in one run.
- The Home Assistant owner login travels with the enrollment and is rotated on release.

## 0.1.0

- First milestone: activation, MQTT sync, fleet updates, local API, Ingress panel.
