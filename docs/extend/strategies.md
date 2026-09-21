# Strategies as documents

A strategy expressible as data can be diffed, generated in batches and
shipped in an extension. A `grid` document searches a rule **Arvo
implements** over parameters you choose; it cannot contribute a rule, and a
document naming a rule Arvo does not have is refused when the manifest is
read, with the list of what it does have.

A `rules` document — thresholds over named signals — is read but not yet
run: Arvo has no runner for one, and says so rather than dropping it.

## A rule over a value that is not there is false

A signal may be absent: not published, or published with nothing in it. A
predicate over an absent value is **false** — never zero, never true, never
the last known value — and an empty rule set never fires either way. This
exists because the alternative is quiet: an indicator with too little
history is absent where it is computed and `0.0` by the time a rule reads
it, and `0.0` is a value a threshold matches.
