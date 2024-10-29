# Extracts and sorts software release numbers

Point separated, multi-point separated or semantic versioned numbers and versioned suffixes

Use as standalone or embedded

- Sorts to latest version, to unique or to listed entries

```bash
curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=' | wc -l
271

floatversion -i "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=' )" 
12.6.0

floatversion --rev -i "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=' )" 
3.0

floatversion -s -i "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=')" 
3.0  3.1  4.0  5.0.0  5.0.1  5.0.2  5.0.3  5.0.4  5.0.5  5.0.6  5.0.7  5.0.8  5.0.9  5.0.10  6.0.0  6.0.1  6.0.2  6.0.2.1  6.0.3  6.0.4  6.0.5  6.0.6  6.0.7  6.0.8  6.0.9  6.0.10  7.0.0  7.1.0  7.2.0  7.3.0  7.4.0  7.5.0  7.6.0  7.7.0  7.8.0  7.9.0  7.10.0  7.11.0  8.0.0  8.1.0  8.2.0  8.3.0  8.4.0  8.5.0  8.6.0  8.7.0  8.7.1  8.8.0  8.9.0  8.10.0  8.11.0  8.11.1  9.0.0  9.0.1  9.1.0  9.2.0  9.2.1  9.3.0  9.4.0  9.5.0  9.6.0  9.7.0  9.8.0  9.9.0  9.10.0  9.11.0  9.12.0  9.13.0  10.0.0  10.1.0  10.2.0  10.3.0  10.4.0  10.5.0  10.6.0  10.7.0  10.8.0  10.9.0  10.10.0  10.11.0  10.12.0  10.13.0  11.0.0  11.1.0  11.2.0  11.3.0  11.4.0  11.5.0  11.6.0  11.7.0  11.8.0  11.9.0  11.10.0  11.11.0  12.0.0  12.1.0  12.2.0  12.3  12.3.0  12.4.0  12.5.0  12.6.0  

```

- Outputs as true/false test, as single item, or as space or line separated list.

- Filters for include, exclude, starts with, and reverse

```txt
 floatversion --options  --input | --gt  "quoted-string"

  -h | --help         show help 
  -V | --version      show version
  -s | --str          show list as single string
  -c | --col          show list as column
  -r | --rev          show list in reverse order  
  -a | --all          show all extracted values, not just unique 
  -n | --num          sort by standard numbering, not versioning
  -f | --full         check for additional sem. ver. suffixes,  eg. -beta
  -F | --filter       allows filtered output in single string, whereas post-output grep requires columns,
  -S | --starts       with -F as contains and -S as starting with, and -D as doesn't contain,
  -D | --delete       multiple filters allowed:  -F | -S  "string"  -D  "string  string  string"  
  -i | --input        input string, text or list (for comparisons, use -g instead)
  -g | --gt           if A is greater than B, returns true, else false (from string of two,'-r' is ignored)
  -v | --verbose      for problem output, show algorithm sequences (full version only) 

  Without options, outputs the highest/latest value in list, with '-r' shows lowest/earliest
  All cases, returns false if none
```

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
floatversion -s -r -S 11 -i "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=')"
11.11.0  11.10.0  11.9.0  11.8.0  11.7.0  11.6.0  11.5.0  11.4.0  11.3.0  11.2.0  11.1.0  11.0.0

floatversion -s -r -S 6 -i "$(curl -sLf "http://mirror-master.dragonflybsd.org/iso-images" | grep -Eo '"dfly-x86_64-.*_REL.iso.bz2"')"
6.4.0  6.2.2  6.2.1  6.0.1  6.0.0  
```

## With

- Multiple filters

```bash
floatversion -s -f -i "non-pad-test.txt" 
1.1.1  1.1.3-beta.1  1.1.3-beta.2  1.2.0-beta.2  1.2.3-beta.1  1.2.3-live  1.2.3-rc1  1.2.3  1.3.1  1.3.1.2  1.10.1  1.10.3-beta.3  1.10.3  1.22.3-rc1  1.22.3  1.30.1  12.2.3  22.3.4-beta  22.3.4-beta.1  22.3.4-rc.2  22.13.4-rc.1   

floatversion -f -s -D "beta rc" -i "non-pad-test.txt" 
1.1.1  1.2.3-live  1.2.3  1.3.1  1.3.1.2  1.10.1  1.10.3  1.22.3  1.30.1  12.2.3 

floatversion --rev -f -s -D "beta rc" -i "non-pad-test.txt" 
12.2.3  1.30.1  1.22.3  1.10.3  1.10.1  1.3.1.2  1.3.1  1.2.3  1.2.3-live  1.1.1

floatversion -f -s -F live -i "non-pad-test.txt" 
1.2.3-live
```

- List as standard numeric or semantic

```bash
 floatversion --num -s -i "floats.txt" 
0.039  0.4  1.09  1.1  1.10  1.11  1.222  1.24  3.0  3.1114  3.141  5.111899  5.7899  16.311  16.32  34.107  34.6 

floatversion  -s -i "floats.txt" 
0.4  0.039  1.1  1.09  1.10  1.11  1.24  1.222  3.0  3.141  3.1114  5.7899  5.111899  16.32  16.311  34.6  34.107 
```

- Compare versions

```bash
Previous="$(floatversion -i "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=')" )"
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

Place script in a program includes folder or copy to `$PATH` eg `sudo cp floatversion /usr/bin`

Embed: in Bash scripts, either the full or the compact function.  

For testing inside scripts, calls to the compact function may be prefixed with `command` in order to route to a path version.

### Limitations

The input must resemble [semantic versioning](http://semver.org/) to some extent. The version number must be _dot separated_. Integers are ignored.

Garbage in, Garbage out.  Some pre-filtering may be required.  
