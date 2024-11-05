# Extracts and sorts software release numbers

Point separated, multi-point separated or semantic versioned numbers and versioned suffixes

Use as standalone or embedded

- Sorts to latest version, to unique or to listed entries

```bash
curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=' | wc -l
271

floatversion -M "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=' )" 
12.6.0

floatversion --rev -M "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=' )" 
3.0

floatversion "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=')" 
3.0  3.1  4.0  5.0.0  5.0.1  5.0.2  5.0.3  5.0.4  5.0.5  5.0.6  5.0.7  5.0.8  5.0.9  5.0.10  6.0.0  6.0.1  6.0.2  6.0.2.1  6.0.3  6.0.4  6.0.5  6.0.6  6.0.7  6.0.8  6.0.9  6.0.10  7.0.0  7.1.0  7.2.0  7.3.0  7.4.0  7.5.0  7.6.0  7.7.0  7.8.0  7.9.0  7.10.0  7.11.0  8.0.0  8.1.0  8.2.0  8.3.0  8.4.0  8.5.0  8.6.0  8.7.0  8.7.1  8.8.0  8.9.0  8.10.0  8.11.0  8.11.1  9.0.0  9.0.1  9.1.0  9.2.0  9.2.1  9.3.0  9.4.0  9.5.0  9.6.0  9.7.0  9.8.0  9.9.0  9.10.0  9.11.0  9.12.0  9.13.0  10.0.0  10.1.0  10.2.0  10.3.0  10.4.0  10.5.0  10.6.0  10.7.0  10.8.0  10.9.0  10.10.0  10.11.0  10.12.0  10.13.0  11.0.0  11.1.0  11.2.0  11.3.0  11.4.0  11.5.0  11.6.0  11.7.0  11.8.0  11.9.0  11.10.0  11.11.0  12.0.0  12.1.0  12.2.0  12.3  12.3.0  12.4.0  12.5.0  12.6.0  

```

- Outputs as true/false test, as single item, or as space or line separated list.

- Filters for include, exclude, starts with, and reverse

```txt
  floatversion --options  "quoted-input-source"

  Extracts point separated numbers, or semantic version numbers with optional suffixes,
  and other common variations upon, from a given string or text-file

  -h | --help         show help 
  -V | --version      show version
  -c | --col          show list as column instead of string (unless -M)
  -r | --rev          show list in reverse order  
  -a | --all          show all extracted values, not just unique 
  -n | --num          sort by standard numbering, not versioning
  -f | --full         check for additional sem. ver. suffixes,  eg. -beta
  -F | --filter       contains given items -F  "string  string  string" 
  -S | --starts       starting with -S  "string  string  string" 
  -D | --delete       doesn't contain: -D  "string  string  string" 
  -M | --max          outputs the highest/latest value in list, only, with '-r' shows lowest/earliest
  -g | --gt           A > B, returns true/false -g "A B" (.nums or sem ver, for -lt use B A)
  -v | --verbose      for problem output, show algorithm sequences (full version only) 
       --sort-v       use system sort -V if available (& if jq methods are causing problems)
       --no-svb       no falling back to 'jq' if 'sort -V' is unavailable, show error instead

  Without options, produces a single sorted string of all unique items found
  Filters output as string, column or max. Post-output grep requires columns.
  Tests show 'jq' sort methods as more reliable than 'sort -V' esp. with alpha suffixes
  All cases, returns false if none
```

## JQ sorting

Testing shows the JQ sort formula is more reliable.

JQ:

```bash
floatversion  -f -S "1.2" "non-pad-test.txt" 
1.2.0-beta.2  1.2.3-beta.1  1.2.3-live  1.2.3-rc1  1.2.3  1.22.3-rc1  1.22.3

floatversion  -f -S "1.2" -M "non-pad-test.txt" 
1.22.3
```

Gnu `sort -V`

```bash
floatversion --sort-v  -f -S "1.2" "non-pad-test.txt" 
1.2.0-beta.2  1.2.3  1.2.3-beta.1  1.2.3-live  1.2.3-rc1  1.22.3  1.22.3-rc1  

floatversion --sort-v  -f -S "1.2" -M "non-pad-test.txt" 
1.22.3-rc1
```

## Move

- From this:
  
```bash
curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=' | grep '11.' | wc -l
82

curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=' | grep -E ^'11.' | wc -l
0

curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=' | grep -E '11.'
   <tr class="odd"><td class="indexcolicon"><a href="3.0_r0/"><img src="/icons2/folder.png" alt="[DIR]"></a></td><td class="indexcolname"><a href="3.0_r0/">3.0_r0/</a></td><td class="indexcollastmod">2011-02-16 23:03  </td><td class="indexcolsize">  - </td></tr>
   <tr class="even"><td class="indexcolicon"><a href="3.0_r1/"><img src="/icons2/folder.png" alt="[DIR]"></a></td><td class="indexcolname"><a href="3.0_r1/">3.0_r1/</a></td><td class="indexcollastmod">2011-02-16 23:05  </td><td class="indexcolsize">  - </td></tr>
   <tr class="odd"><td class="indexcolicon"><a href="3.0_r2/"><img src="/icons2/folder.png" alt="[DIR]"></a></td><td class="indexcolname"><a href="3.0_r2/">3.0_r2/</a></td><td class="indexcollastmod">2011-02-16 23:05  </td><td class="indexcolsize">  - </td></tr>
   <tr class="even"><td class="indexcolicon"><a href="3.0_r3/"><img src="/icons2/folder.png" alt="[DIR]"></a></td><td class="indexcolname"><a href="3.0_r3/">3.0_r3/</a></td><td class="indexcollastmod">2011-02-16 23:05  </td><td class="indexcolsize">  - </td></tr>
   <tr class="odd"><td class="indexcolicon"><a href="3.0_r4/"><img src="/icons2/folder.png" alt="[DIR]"></a></td><td class="indexcolname"><a href="3.0_r4/">3.0_r4/</a></td><td class="indexcollastmod">2016-11-04 23:29  </td><td class="indexcolsize">  - </td></tr>
   <tr class="even"><td class="indexcolicon"><a href="3.0_r5/"><img src="/icons2/folder.png" alt="[DIR]"></a></td><td class="indexcolname"><a href="3.0_r5/">3.0_r5/</a></td><td class="indexcollastmod">2016-11-04 23:21  </td><td class="indexcolsize">  - </td></tr>
   <tr class="odd"><td class="indexcolicon"><a href="3.0_r6/"><img src="/icons2/folder.png" alt="[DIR]"></a></td><td class="indexcolname"><a href="3.0_r6/">3.0_r6/</a></td><td class="indexcollastmod">2016-11-05 01:35  </td><td class="indexcolsize">  - </td></tr>
   <tr class="even"><td class="indexcolicon"><a href="3.1_r0/"><img src="/icons2/folder.png" alt="[DIR]"></a></td><td class="indexcolname"><a href="3.1_r0/">3.1_r0/</a></td><td class="indexcollastmod">2016-11-02 20:08  </td><td class="indexcolsize">  - </td></tr>
   <tr class="odd"><td class="indexcolicon"><a href="3.1_r0a/"><img src="/icons2/folder.png" alt="[DIR]"></a></td><td class="indexcolname"><a href="3.1_r0a/">3.1_r0a/</a></td><td class="indexcollastmod">2016-11-02 20:08  </td><td class="indexcolsize">  - </td></tr>...

   82 etc etc

```

- To this:

```bash
floatversion -r -S 11 "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=')"
11.11.0  11.10.0  11.9.0  11.8.0  11.7.0  11.6.0  11.5.0  11.4.0  11.3.0  11.2.0  11.1.0  11.0.0

floatversion -r -S 6 "$(curl -sLf "http://mirror-master.dragonflybsd.org/iso-images" | grep -Eo '"dfly-x86_64-.*_REL.iso.bz2"')"
6.4.0  6.2.2  6.2.1  6.0.1  6.0.0  
```

## With

- Multiple filters

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

- List as standard numeric or semantic

```bash
 floatversion --num "floats.txt" 
0.039  0.4  1.09  1.1  1.10  1.11  1.222  1.24  3.0  3.1114  3.141  5.111899  5.7899  16.311  16.32  34.107  34.6 

floatversion  "floats.txt" 
0.4  0.039  1.1  1.09  1.10  1.11  1.24  1.222  3.0  3.141  3.1114  5.7899  5.111899  16.32  16.311  34.6  34.107 
```

- Compare versions

```bash
Previous="$(floatversion -M "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=')" )"
Current="$(cat /etc/debian_version)"
if floatversion -g "$Current $Previous"; then echo "Up to Date"; fi
Up to Date
```

- Show verbose algorithmics

```bash
if floatversion --verbose --gt "$Current $Previous"; then echo "Up to Date"; fi

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

## Install

### Standalone

Place script in a program includes folder or copy to `$PATH` eg `sudo cp floatversion /usr/bin`

Make sure that script has execute permissions `sudo chmod +x /usr/bin/floatversion`

### Embedding in Bash scripts

In basic single scripts, the compact function is a simple copy and paste which is ready to go.  

The full version is named differently to enable easy separation within the standalone script. In larger projects, the required extra space won't notice and you have verbose mode ready built in.

When pasting the full version, it is envisaged that it be renamed to `floatversion`, the same as with the compact one. This way any later decisions to use the compact function, or any code re-use, won't impact on anything.

For testing the compact function inside scripts, it is recommended that a $PATH version be installed on the development computer. Any script calls can then be temporarily prefixed by the control word `command` which will cause routing to that instead and will allow verbose output to be enabled.

### Use in different shells

The script's Shebang will mean that direct running of the standalone is possible from the interactive terminal. 

For example:

```fish
╭─xxx@garuda in repo: floatversion on  main [$]
╰─λ echo $SHELL
/usr/bin/fish

╭─xxx@garuda in repo: floatversion on  main [$]
╰─λ ./floatversion -F 10 -M -n  non-pad-test.txt
1.10.3
```

To include calls to `floatversion` standalone script from within an actual fishshell script, the following [method](https://github.com/fish-shell/fish-shell/issues/4488) may be used:

```fish
╭─xxx@garuda in repo: floatversion on  main [$] took 0s
╰─λ if bash -c './floatversion --gt "1.4.5  2.3.6.7"'
echo yes
else
echo no
end

no

╭─xxx@garuda in repo: floatversion on  main [$] took 0s
╰─λ if bash -c './floatversion --gt "3.4.5  2.3.6.7"'
echo yes
else
echo no
end

yes
```

The use of transfer file will enable variables to  be passed:

```fish
╭─xxx@garuda in repo: floatversion on  main [$]
╰─λ set Foo 3.1.4.1

╭─xxx@garuda in repo: floatversion on  main [$]
╰─λ set Bar 42.00

╭─xxx@garuda in repo: floatversion on  main [$]
╰─λ if bash -c './floatversion --gt "$Foo  $Bar"'
echo yes
else
echo no
end

ERROR: no input found.


╭─xxx@garuda in repo: floatversion on  main [$?] took 0s
╰─λ echo "$Bar  $Foo" > fish-tx.txt

╭─xxx@garuda in repo: floatversion on  main [$?] took 0s
╰─λ if  bash -c './floatversion --gt "fish-tx.txt"'
echo yes
else
echo no
end

yes
```

### Limitations

The input must resemble [semantic versioning](http://semver.org/) to some extent. The version number must be _dot separated_. Integers are ignored.

Garbage in, Garbage out.  Some pre-filtering may be required.  
