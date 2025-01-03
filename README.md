# Extracts and sorts software release numbers

Point separated, multi-point separated or [semantic](http://semver.org/) versioned and versioned suffixed

Use as standalone or embedded (~90 lines)

## Check GitHub Latest Release or Tag

```text
floatversion -V

 CurrentVersion: 1.02.02

 Checking for updates....   LatestVersion: 1.02.02  Up-to-Date 

 (c) Alex Genovese  https://github.com/TuxVinyards/floatversion
```

```bash
floatversion -r "$(curl -sf https://github.com/qemu/qemu/tags | grep 's/tag')"
9.1.1  9.1.0  9.0.3  8.2.7  7.2.14  

floatversion -M "$(curl -sf https://github.com/qemu/qemu/tags | grep 's/tag')"
9.1.1
```

- Output as true/false test, as single item, or as space or line separated list.

- Filter for include, exclude, starts with, and reverse

```txt
  floatversion --options  "quoted-input-source"

  Extracts point separated numbers, or semantic version numbers with optional suffixes,
  and other common variations upon, from a given string or text-file

  -h | --help         show help 
  -V | --version      show version
  -c | --col          show list as column instead of string
  -s | --space        use single spaced string (for alternative machine reading)
  -r | --rev          show list in reverse order  
  -a | --all          show all extracted values, not just unique 
  -n | --num          sort by standard numbering, not versioning
  -f | --full         check for additional sem. ver. suffixes,  eg. -beta
  -F | --filter       contains given items -F  "string  string  string" 
  -S | --starts       starting with -S  "string  string  string" 
  -D | --delete       doesn't contain: -D  "string  string  string" 
  -M | --max          Outputs the single highest/latest value in the list and with '-r' the lowest/earliest,
                      with integer shows top or bottom of total list eg. -M 3 is top 3 and -M 3 -r the bottom.
  -RM [int]           Outputs the single lowest/earliest value in the list. With integer, as -M but 
                      reverses only the M filter set, not the list, eg. -M 3  > 3 2 1  -RM 3 > 1 2 3
                      also accepts '-r' to reverse a lowest set filter ...
  -g | --gt           A > B, returns true/false eg. if fv -g "A B"; then .. (.nums or sem ver)  
                      Normally quiet and to be tested.  Use -v for screen output or piping.
  -v | --verbose      for problem output, show algorithm sequences (full version only) 
       --sort-v       use sort -V (if present) in preference to the default jq methods
       --no-svb       no falling back to 'jq' if 'sort -V' is unavailable, show error instead

  Without options, produces a single sorted string of all unique items found
  Filters output as string, column or max. Post-output grep requires columns.

  Tests show 'jq' sort methods as more reliable than 'sort -V' esp. with alpha suffixes
  All cases, returns false if none.  Direct '2>/dev/null' to quieten messages.
```

- Sort to latest version, to unique or to listed entries, top 3 etc

```bash
curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=' | wc -l
271

# of the 271 entries the following are unique:
floatversion "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=')" 
3.0  3.1  4.0  5.0.0  5.0.1  5.0.2  5.0.3  5.0.4  5.0.5  5.0.6  5.0.7  5.0.8  5.0.9  5.0.10  6.0.0  6.0.1  6.0.2  6.0.2.1  6.0.3  6.0.4  6.0.5  6.0.6  6.0.7  6.0.8  6.0.9  6.0.10  7.0.0  7.1.0  7.2.0  7.3.0  7.4.0  7.5.0  7.6.0  7.7.0  7.8.0  7.9.0  7.10.0  7.11.0  8.0.0  8.1.0  8.2.0  8.3.0  8.4.0  8.5.0  8.6.0  8.7.0  8.7.1  8.8.0  8.9.0  8.10.0  8.11.0  8.11.1  9.0.0  9.0.1  9.1.0  9.2.0  9.2.1  9.3.0  9.4.0  9.5.0  9.6.0  9.7.0  9.8.0  9.9.0  9.10.0  9.11.0  9.12.0  9.13.0  10.0.0  10.1.0  10.2.0  10.3.0  10.4.0  10.5.0  10.6.0  10.7.0  10.8.0  10.9.0  10.10.0  10.11.0  10.12.0  10.13.0  11.0.0  11.1.0  11.2.0  11.3.0  11.4.0  11.5.0  11.6.0  11.7.0  11.8.0  11.9.0  11.10.0  11.11.0  12.0.0  12.1.0  12.2.0  12.3  12.3.0  12.4.0  12.5.0  12.6.0  

# the latest is:
floatversion -M "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=')" 
12.6.0

# the latest three are:
floatversion -M 3 "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=')" 
12.6.0  12.5.0  12.4.0

# the oldest is:
floatversion --rev -M "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=')" 
3.0
```

- Starts with

```bash
floatversion -r --starts 11  "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=')"
11.11.0  11.10.0  11.9.0  11.8.0  11.7.0  11.6.0  11.5.0  11.4.0  11.3.0  11.2.0  11.1.0  11.0.0

floatversion -r --starts 6  "$(curl -sLf "http://mirror-master.dragonflybsd.org/iso-images" | grep -Eo '"dfly-x86_64-.*_REL.iso.bz2"')"
6.4.0  6.2.2  6.2.1  6.0.1  6.0.0  
```

- Keywords +/-

```bash
floatversion -f "non-pad-test.txt" 
1.1.1  1.1.3-beta.1  1.1.3-beta.2  1.2.0-beta.2  1.2.3-beta.1  1.2.3-live  1.2.3-rc1  1.2.3  1.3.1  1.3.1.2  1.10.1  1.10.3-beta.3  1.10.3  1.22.3-rc1  1.22.3  1.30.1  12.2.3  22.3.4-beta  22.3.4-beta.1  22.3.4-rc.2  22.13.4-rc.1   

floatversion -f -D "beta rc" "non-pad-test.txt" 
1.1.1  1.2.3-live  1.2.3  1.3.1  1.3.1.2  1.10.1  1.10.3  1.22.3  1.30.1  12.2.3 

floatversion --rev -f -D "beta rc" "non-pad-test.txt" 
12.2.3  1.30.1  1.22.3  1.10.3  1.10.1  1.3.1.2  1.3.1  1.2.3  1.2.3-live  1.1.1

floatversion -f -F live -M "non-pad-test.txt" 
1.2.3-live
```

- Standard numeric or semantic ordering

```bash
 floatversion --num "floats.txt" 
0.039  0.4  1.09  1.1  1.10  1.11  1.222  1.24  3.0  3.1114  3.141  5.111899  5.7899  16.311  16.32  34.107  34.6 

floatversion  "floats.txt" 
0.4  0.039  1.1  1.09  1.10  1.11  1.24  1.222  3.0  3.141  3.1114  5.7899  5.111899  16.32  16.311  34.6  34.107 
```

- Compare versions

```bash
LatestAchive="$(floatversion -M "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=')" )"
Current="$(cat /etc/debian_version)"
if floatversion --gt  "$Current $LatestAchive" ; then echo "Up to Date"; fi
Up to Date
```

- Reliable JQ sort formula

```bash
# JQ:
floatversion  -f -S "1.2" "non-pad-test.txt" 
1.2.0-beta.2  1.2.3-beta.1  1.2.3-live  1.2.3-rc1  1.2.3  1.22.3-rc1  1.22.3

# correct latest:
floatversion --max -f -S "1.2"  "non-pad-test.txt" 
1.22.3
```

```bash
# sort -V  incorrect:
floatversion --sort-v  -f -S "1.2" "non-pad-test.txt" 
1.2.0-beta.2  1.2.3  1.2.3-beta.1  1.2.3-live  1.2.3-rc1  1.22.3  1.22.3-rc1  

# incorrect:
floatversion --max --sort-v -f -S "1.2" "non-pad-test.txt" 
1.22.3-rc1
```

## Easy Install

FloatVersion will run in Bash or in Non-Bash shells.

Requires up-to-date versions of Bash, Grep, JQ and Curl.

- On MacOS install these via Homebrew

Curl is needed from version 1.1 for the update checker on the full version only.

### Standalone

Copy to `$PATH` eg `sudo cp floatversion /usr/bin` or `/usr/local/bin`

Make sure that it has execute permissions `sudo chmod +x /usr/bin/floatversion`

Add a short link?  

```bash
cd /usr/bin
sudo ln -s floatversion fv
>
fv -f -S "1.2" "non-pad-test.txt"  etc ...
```

### Within a program

The following simple wrapper can be used in Bash to call an includes folder full script:

```bash
ExampleProgFolder="/usr/share/my-prog/utils"
floatversion () { 
  "$ExampleProgFolder/floatversion" "$@" 
}
```

For Non-Bash, see [below](#use-in-different-shells)

### Embedding

This method is suitable only for Bash scripts.

Single scripts can add the compact function in a simple copy and paste operation which is ready to go.

In larger projects, the required extra space for the full function won't notice and you will have verbose mode ready built in.

The full version is named differently inside the standalone script to enable easy separation. When pasting the full version, it should be renamed to `floatversion`, the same as with the compact one.

Using the standard syntax `floatversion --options  "quoted-input-source"` will ensure that if a function is not present, the call will re-route to floatversion as a standalone dependency, also that any later usage changes or any code re-use won't have negative impact.

_Locale settings_ should be implemented as outlined in the `floatversion` script header.

### Output Array

The standalone script can be set to output a transfer file which is easily mappable by most shells. When embedding, the array `${fvOutputArr[*]}` is by default present as global.

### Testing

For testing the _compact_ function inside scripts, it is recommended that a $PATH version be installed on the development computer. Any Bash script calls can then be temporarily prefixed by the control word `command` which will cause the function to re-route and will allow verbose output to be enabled.

- Show _verbose_ algorithmics

```bash
if floatversion --verbose --gt "$Current $LatestAchive"; then echo "Up to Date"; fi

Str:
12.7 12.6.0

Nums: (2)
12.7
12.6.0

declare -a NumArrTemp=([0]="12.7" [1]="12.6.0")

declare -a NumArrSorted=([0]="12.6.0" [1]="12.7")

declare -a fvOutputArr=([0]="12.6.0" [1]="12.7")

true

Up to Date

```

### Updates

The script snippets, [above,](#check-github-latest-release-or-tag) show how to check `floatversion` releases and how to compare versions.

Version 1.1 now has an auto-update checker that will run on request:

```txt
floatversion -V

CurrentVersion: 1.1.0

Checking for updates....   LatestVersion: 1.1.0  Up-to-Date 

(c) Alex Genovese  https://github.com/TuxVinyards/floatversion 
```

```bash
OR ....

if floatversion -V | grep -sq 'Up-to-Date' ; then echo OK ; fi

OK
```

Requires Curl. @ 2024 Most distros will have this.

Alternatively see GitHub's dependabot docs for setting up alerts ....

### Use in different shells

The script's Shebang will mean that direct running of the standalone is possible from the interactive terminal.

For example:

```fish
╭─xxx@garuda in repo: floatversion on  main [$]
╰─λ echo $SHELL
/usr/bin/fish

╭─xxx@garuda in repo: floatversion on  main [$]
╰─λ floatversion -F 10 -M -n  "non-pad-test.txt"
1.10.3
```

To include calls to `floatversion` standalone script from within an actual fishshell script, the following [method](https://github.com/fish-shell/fish-shell/issues/4488) may be used:

```ftxt
╭─xxx@garuda in repo: floatversion on  main [$] took 0s
╰─λ if bash -c 'floatversion --gt "1.4.5  2.3.6.7"'
echo yes
else
echo no
end

no

╭─xxx@garuda in repo: floatversion on  main [$] took 0s
╰─λ if bash -c 'floatversion --gt "3.4.5  2.3.6.7"'
echo yes
else
echo no
end

yes
```

The use of a transfer file will enable variables to be passed:

```txt
╭─xxx@garuda in repo: floatversion on  main [$]
╰─λ set Foo 3.1.4.1

╭─xxx@garuda in repo: floatversion on  main [$]
╰─λ set Bar 42.00


╭─xxx@garuda in repo: floatversion on  main [$]
╰─λ if bash -c 'floatversion --gt "$Foo  $Bar"'
echo yes
else
echo no
end

ERROR: no input found.


# Output vars to file and read file instead: 

╭─xxx@garuda in repo: floatversion on  main [$?] took 0s
╰─λ echo "$Bar  $Foo" > fish-tx.txt

╭─xxx@garuda in repo: floatversion on  main [$?] took 0s
╰─λ if  bash -c 'floatversion --gt "fish-tx.txt"'
echo yes
else
echo no
end

yes
```

### Limitations

The input must resemble [semantic versioning](http://semver.org/) to _some_ extent, or decimal floating points.

Plain integers are ignored. The version number must be _dot separated_. Only _one_ single _hyphenated_ suffix is permitted.

Some pre-filtering may be required.  
