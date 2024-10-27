### Extracts point separated numbers, or semantic version numbers with optional suffixes

- Sorts to unique or listed entries, or to latest version in std numeric or sem. ver. and other common variations upon, from a given string or text list

```
./floatversion -s -i "$(curl -sL --fail "http://mirror-master.dragonflybsd.org/iso-images" | grep -E -o '"dfly-x86_64-.*_REL.iso.bz2"')"
5.0.0  5.0.1  5.0.2  5.2.0  5.2.1  5.2.2  5.4.0  5.4.1  5.4.2  5.4.3  5.6.0  5.6.1  5.6.2  5.6.3  5.8.0  5.8.1  5.8.2  5.8.3  6.0.0  6.0.1  6.2.1  6.2.2  6.4.0

```

Script outputs as true/false test, as single item, or as space or line separated list. 
If function (full or compact) is embedded, produces optional array '${fvOutputArr[*]}'
