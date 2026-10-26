# Bertrande: dated, sourced AI API prices

This repository holds the published state of the [Bertrande](https://bertrande.com) registry,
committed once a day.

**The git history of this repository is the price history.** Files are sorted
by parameter id, never by name, so every commit diff reads line by line as
"this value changed on this day":

    git log --oneline values.csv
    git log -p -S "anthropic:claude-sonnet-5#price_output" values.csv

## Four files, and the difference matters

| File | Contents | What its diff tells you |
|---|---|---|
| `values.csv` | 13711 values, no provenance | **exactly the values that changed** |
| `registry.csv` | the same rows, with date, source URL, source level and SHA-256 fingerprint | what was observed again, even at an unchanged value |
| `measurements.csv` | 2885 continuous measurements: availability, latency, throughput, exchange rates | new readings, every day |
| `changes.csv` | 1153 confirmed changes, newest first | |

`values.csv` and `registry.csv` carry the same rows. The first holds state
only, so re-reading an unchanged price leaves it untouched; the second holds
the evidence, so it moves whenever an observation is written again.

**A measurement is not a published parameter**, which is why they live in a
separate file. A published parameter is what the vendor states: a price, a
context window, a rate limit. A measurement is what someone observed at an
instant: an uptime, a latency. The first changes when the vendor decides; the
second changes all the time and its movement teaches nothing. Mixed together,
measurements drown prices: on the second commit of this repository, 1,308 of
2,107 changed lines were continuous measurements.

## Provenance

Every row of `registry.csv` carries the address of its **receipt**: a permanent
page showing the value, its date, its source and the fingerprint of the page it
was read on. Anyone can recompute the receipt id from this file:

    sha256(f"{parameter_id}|{observed_at}|{content_hash}")[:12]

## The three rules of the registry

1. **No fact comes out of a language model.** Every value is read from a
   source, with its URL, its date and a fingerprint of what was read.
2. **Every published value is traceable**: where it comes from, when it was
   read, what changed since.
3. **No fabricated human identity.** Bertrande is an automated editorial
   system, and a named person answers for it.

An empty cell is honourable; an invented one would destroy the project. A
parameter whose source is too weak is not published: it is held back and
counted.

## Source levels

| Level | What it is |
|---|---|
| 1 | the vendor's API |
| 2 | the vendor's technical documentation |
| 3 | the vendor's price page |
| 4 | an identified third-party measurer |
| 5 | an aggregator, **never published on its own** |

## Licence and use

CC-BY-4.0. Free reuse with attribution: `Bertrande, https://bertrande.com`.

The read API answers the same question by date:
`https://bertrande.com/v1/services/{id}/{parameter}?on=2026-09-03`. Asked about a date
before our first reading, it returns a **416** and says since when we have been
observing, rather than a 404 that would suggest a disappearance or a `null`
that would suggest the price was zero.

Collection started on 2026-08-25. Corrections: https://bertrande.com/en/corrections/
