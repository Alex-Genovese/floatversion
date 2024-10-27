### Extracts point separated numbers, or semantic version numbers with optional suffixes

- Sorts to unique or listed entries, or to latest version in std numeric or sem. ver. and other common variations upon, from a given string or text list

From this:
```
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

To this:

```
./floatversion -c -i "$(curl -sLf  "https://cdimage.debian.org/cdimage/archive/" | grep 'src=')" | grep -E ^'11.'
11.0.0
11.1.0
11.2.0
11.3.0
11.4.0
11.5.0
11.6.0
11.7.0
11.8.0
11.9.0
11.10.0
11.11.0
```

Script outputs as true/false test, as single item, or as space or line separated list. 
If function (full or compact) is embedded, produces optional array '${fvOutputArr[*]}'

```
 floatversion --options  --input | --gt  \"quoted-string\"

  -h | --help         show help 
  -V | --version      show version
  -s | --str          show list as single string
  -c | --col          show list as column
  -r | --rev          show list in reverse order  
  -a | --all          show all extracted values, not just unique 
  -n | --num          sort by standard numbering, not versioning
  -f | --full         check for additional sem. ver. suffixes,  eg. -beta
  -i | --input        input string, text or list (for comparisons, use -g instead)
  -g | --gt           if A is greater than B, returns true, else false (from string of two, '-r' is ignored)
  -v | --verbose      for problem output, show algorithm sequences (full version only) 

  Without options, outputs the highest/latest value in list, with '-r' shows lowest/earliest
  All cases, returns false if none
```
```
./floatversion -s -i "$(curl -sL --fail "http://mirror-master.dragonflybsd.org/iso-images" | grep -E -o '"dfly-x86_64-.*_REL.iso.bz2"')"
5.0.0  5.0.1  5.0.2  5.2.0  5.2.1  5.2.2  5.4.0  5.4.1  5.4.2  5.4.3  5.6.0  5.6.1  5.6.2  5.6.3  5.8.0  5.8.1  5.8.2  5.8.3  6.0.0  6.0.1  6.2.1  6.2.2  6.4.0
```
