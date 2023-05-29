** Background

These scripts are based around the Warframe Community Developers
(WFCD) team's repackaging of the raw HTML tables provided by DE into
usable JSON files.  They have a sort of "API" where you can fetch
sub-sets of the data, but I just grab the full table and work with
that.


** Installation

These scripts are a combination of bash and Perl.  They assume you
have `wget`, and Perl's `File::Map` (Debian package:
`libfile-map-perl`).


** Usage

`updatetables` is a simple wrapper that fetches `all.json` from the
drop data API, pretty-prints it into `wfdrops.json`, and then runs
`readloottable` on it and outputs to `full.txt`.  (This is not
including open-world bounties. Those have their own sections I need to
parse.  So "full" is a lie.

`readloottable` reads `wfdrops.json` (hard-coded, this is all a hack)
and prints it out in an indented format that's useful for reading or
using `grep` against.  I save that to `full.txt` because it takes my
workstation 4-5 seconds to parse that entire table every time.

`searchloottable` will search `wfdrops.json` for a phrase and return
something that looks like this:

$ ./searchloottable Lith K
Lith K
planet: Mercury
  location: Suisei (Spy)
   rotation: B
      Lith K10 Relic : 14.29
planet: Mercury
  location: Lares (Defense)
   rotation: A
      Lith K10 Relic : 9.09
... and so on.

Again, no bounties.  And like `readloottable`, it takes 4-5 seconds
for a search, so it is faster to grep through `full.txt`, but the
output is not as nice, with something like this:

$ key='Lith K10'; grep -a -E "(planet:|location:|rotation|$key)" full.txt
  |grep -a -B1 -E "(planet:|location:|$key)" |grep -a -v -- --
  |grep -a -B2 "$key" |grep -a -v -- --


** NOTES

https://github.com/WFCD/warframe-drop-data

# "API" to data
http://drops.warframestat.us/data/info.json
http://drops.warframestat.us/data/all.json

json_pp <all.json >wfdrops.json

# the original data from DE
https://n8k6e2y6.ssl.hwcdn.net/repos/hnfvc0o3jnfvc873njb03enrf56.html
