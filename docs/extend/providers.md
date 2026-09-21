# Writing a provider

A provider is a process. Arvo starts it, reads one line from it, talks to it
over gRPC on loopback, and stops it. Any language with gRPC will do; the
protocol is the whole interface, and Arvo is not required to develop
against it.

## Starting and the handshake

Arvo runs what the manifest's `run` names, from a copy of the binary in a
cache it owns, with two variables set:

| Variable | What |
|---|---|
| `ARVO_PLUGIN_ADDR` | the address to bind, always `127.0.0.1:0` — the OS picks the port; there are no fixed ports |
| `ARVO_PLUGIN_TOKEN` | a secret this spawn must require on every call |

Bind first, then print one line on stdout and flush it:

```
address=127.0.0.1:52341
```

## What you serve

`arvo.plugin.v1.Plugin` (`GetManifest`: an id, a name, a version, and a
capability per service you serve, named exactly), plus whatever you
claimed:

| Proto | Service |
|---|---|
| `source.proto` | `arvo.source.v1.Source` — `Describe` the sources and venues you speak for, `Bars`, `Dividends`, `Search`, `Quotes`, `Connected` |
| `signal.proto` | `arvo.signal.v1.Signals` — named values that may be absent: `Describe` what you publish and whether it is causal, `Latest` what you think now |

Every call carries `arvo-token: <ARVO_PLUGIN_TOKEN>` in metadata; refuse a
call without it with `UNAUTHENTICATED`, compare in constant time, never log
it. A process started by hand with no token may serve without one — that
is how you develop it.

## Credentials

They never cross whole. A call that needs one carries a `Grant` for that
call — a `bearer`, or a `key_id` and `secret` — read from Arvo's keychain.
Use it for that call and hold nothing. A call without the grant its source
needs is a call with no session: do not fall back to credentials in your
own environment; answer unauthenticated and let the person connect the
account in Arvo.

## Errors, restarts, stopping

Errors are gRPC status codes, never a successful response describing a
failure. A provider that exits is restarted a few times with a doubling
pause, then reported unreachable with the count. Arvo stops what it started
— when the extension is disabled or removed, and when Arvo quits — by
terminating it; there is no shutdown call.

## Signals

`Describe` says what you publish: a name in the open dotted namespace, a
line about what it means, whether it is **causal** (each value computed from
data available at that instant — your claim, and it decides whether a
study may gate on it), and what computes it. `Latest` is where the
invariant lives: a value you do not have is absent, never zero; never
repeat a value once it is no longer current; a provider that is down says
nothing, and Arvo drops its names.

## Reference plugins

[arvo-plugin-yahoo](https://github.com/wjpin84/arvo-plugin-yahoo) (the
source Arvo ships with) and
[arvo-plugin-alpaca](https://github.com/wjpin84/arvo-plugin-alpaca), with
the release pipeline that produces the `assets` a manifest can name.
