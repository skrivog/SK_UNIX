# UNIX_Assignment_Krivograd

##Data Inspection

###Attributes of `fang_et_al_genotypes`

```
here is my snippet of code used for data inspection
```
ls -lh fang_et_al_genotypes.txt
-rw-rw-r--. 1 skrivog domain users 11M Feb 17 15:28 fang_et_al_genotypes.txt

file fang_et_al_genotypes.txt
fang_et_al_genotypes.txt: ASCII text, with very long lines

wc fang_et_al_genotypes.txt
2783  2744038 11051939 fang_et_al_genotypes.txt

awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
986

By inspecting this file I learned that:

1. The file composed of ASCII text with very long lines
2. The file's word count was 2783 2744038 11051939
3. The file has 986 columns

###Attributes of `snp_position.txt`

```
here is my snippet of code used for data inspection
```
ls -lh snp_position.txt
-rw-rw-r--. 1 skrivog domain users 81K Feb 17 15:28 snp_position.txt

file snp_position.txt
snp_position.txt: ASCII text

wc snp_position.txt
984 13198 82763 snp_position.txt

awk -F "\t" '{print NF; exit}' snp_position.txt
15

By inspecting this file I learned that:

1. The file contained ASCII text
2. The word count of the file is 984 13198 82763
3. The file has 15 columns

##Data Processing

###Maize Data

```
here is my snippet of code used for data processing
```
mkdir SK_UNIX
Ls
Cp snp_position.txt transposed_genotypes.txt SK_UNIX
Cd SK_UNIX
Ls
snp_position.txt transposed_genotypes.txt

Vim snp_position.txt then set nowrap 

tail -n +3 transposed_genotypes.txt> transposed_noheader.txt

wc transposed_noheader.txt 
984 2738472 10966346 transposed_noheader.txt

tail -n +6 transposed_noheader.txt | awk -F "\t" '{print NF; exit}'
2783

sed 's/Group/SNP_ID/g' transposed_noheader.txt > nogroup.txt 

join -1 1 -2 1 -t $'\t' snp_position.txt nogroup.txt > transposed_joined.txt = transposed_joined.txt

vim transposed_joined.txt

wc transposed_joined.txt 
awk '{for(i=1;i<=NF;i++) if ($i == "ZMMIL") print "Found at column " i}' transposed_joined.txt
Found at column 2508
Found at column 2797

awk '{for(i=1;i<=NF;i++) if ($i == "ZMMMR") print "Found at column " i}' transposed_joined.txt
Found at column 2481
Found at column 2507

awk '{for(i=1;i<=NF;i++) if ($i == "ZMMLR") print "Found at column " i}' transposed_joined.txt
Found at column 1225
Found at column 2480

awk '{for(i=1;i<=NF;i++) if($i == "ZMMMR") count++}
END{print count}' transjoin.txt = see how many columns in each 
27
awk '{for(i=1;i<=NF;i++) if($i == "ZMMLR") count++}
END{print count}' transjoin.txt
1256
awk '{for(i=1;i<=NF;i++) if($i == "ZMMIL") count++}
END{print count}' transjoin.txt
290

cut -d $'\t' -f1,3,4,2508-2797,1225-2480,2481-2507 transposed_joined.txt > maize.txt = make the maize.txt
wc maize.txt
984 1550778 6216783 maize.txt

tail -n +6 maize.txt | awk -F "\t" '{print NF; exit}'
1576

sort -k3,3n maize.txt > maize_sort.txt 

wc maize_sort.txt
984 1550778 6216783 maize_sort.txt

tail -n +6 maize_sort.txt | awk -F "\t" '{print NF; exit}' = to check columns in file
1576

vim maize_sort.txt = to visualize the columns and rows to check

(head -n 1 maize.txt && tail -n +2 maize.txt | sort -k3,3n) > Maize_sorted.txt

wc Maize_sorted.txt
984 1550778 6216783 Maize_sorted.txt

tail -n +6 Maize_sorted.txt | awk -F "\t" '{print NF; exit}'
1576

grep -v "^#" Maize_sorted.txt | cut -f2 | sort | uniq -c
155 1
     53 10
    127 2
    107 3
     91 4
    122 5
     76 6
     97 7
     62 8
     60 9
      1 Chromosome
      6 multiple
     27 unknown

{ head -n 1 Maize_sorted.txt && awk '$2 == 1' Maize_sorted.txt; } > Maize_chr1.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Maize_sorted.txt && awk '$2 == 1' Maize_sorted.txt; } > Maize_chr1.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Maize_sorted.txt && awk '$2 == 2' Maize_sorted.txt; } > Maize_chr2.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Maize_sorted.txt && awk '$2 == 3' Maize_sorted.txt; } > Maize_chr3.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Maize_sorted.txt && awk '$2 == 4' Maize_sorted.txt; } > Maize_chr4.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Maize_sorted.txt && awk '$2 == 5' Maize_sorted.txt; } > Maize_chr5.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Maize_sorted.txt && awk '$2 == 6' Maize_sorted.txt; } > Maize_chr6.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Maize_sorted.txt && awk '$2 == 7' Maize_sorted.txt; } > Maize_chr7.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Maize_sorted.txt && awk '$2 == 8' Maize_sorted.txt; } > Maize_chr8.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Maize_sorted.txt && awk '$2 == 9' Maize_sorted.txt; } > Maize_chr9.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Maize_sorted.txt && awk '$2 == 10' Maize_sorted.txt; } > Maize_chr10.txt
{ head -n 1 Maize_sorted.txt && awk '$2 == "multiple" Maize_sorted.txt; } > Maize_multiple.txt
{ head -n 1 Maize_sorted.txt && awk '$2 == "unknown" Maize_sorted.txt; } > Maize_unknown.txt

wc Maize_chr1.txt Maize_chr2.txt Maize_chr3.txt Maize_chr4.txt Maize_chr5.txt Maize_chr6.txt Maize_chr7.txt Maize_chr8.txt Maize_chr
9.txt Maize_chr10.txt Maize_mulitple.txt Maize_unknown.txt
    156  245856  988210 Maize_chr1.txt
    128  201728  811380 Maize_chr2.txt
    108  170208  685104 Maize_chr3.txt
     92  144992  584066 Maize_chr4.txt
    123  193848  779831 Maize_chr5.txt
     77  121352  489390 Maize_chr6.txt
     98  154448  622008 Maize_chr7.txt
     63   99288  400970 Maize_chr8.txt
     61   96136  388329 Maize_chr9.txt
     54   85104  344187 Maize_chr10.txt
wc: Maize_mulitple.txt: No such file or directory
wc: Maize_unknown.txt: No such file or directory
    960 1512960 6093475 total

{ head -n 1 Maize_sorted.txt && awk '$2 == "multiple"' Maize_sorted.txt; } > Maize_multiple.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Maize_sorted.txt && awk '$2 == "unknown"' Maize_sorted.txt; } > Maize_unknown.txt
[skrivog@nova SK_UNIX]$ wc Maize_multiple.txt Maize_unknown.txt
     7  11026  47345 Maize_multiple.txt
    28  44128 180078 Maize_unknown.txt
    35  55154 227423 total

awk -F "\t" '{print NF; exit}' Maize_chr1.txt Maize_chr2.txt Maize_chr3.txt Maize_chr4.txt Maize_chr5.txt Maize_chr6.txt Maize_chr7.txt Maize_chr8.txt Maize_chr9.txt Maize_chr10.txt Maize_multiple.txt Maize_unknown.txt
1576

sed 's/?/-/g' Maize_sorted.txt > Maize_hyphen.txt

wc Maize_hyphen.txt
    984 1550778 6216783 Maize_hyphen.txt

awk -F "\t" '{print NF; exit}' Maize_hyphen.txt
1576

mkdir Maize_increasing.txt
mv Maize_chr1.txt Maize_chr2.txt Maize_chr3.txt Maize_chr4.txt Maize_chr5.txt Maize_chr6.txt Maize_chr7.txt Maize_chr8.txt Maize_chr9.txt Maize_chr10.txt Maize_multiple.txt Maize_unknown.txt Maize_increasing.txt/

mkdir Maize_decreasing.txt
[skrivog@nova SK_UNIX]$ ls
Maize_decreasing.txt  Maize_increasing.txt  maize_sort.txt  nogroup.txt       transposed_genotypes.txt  transposed_noheader.txt
Maize_hyphen.txt      Maize_sorted.txt      maize.txt       snp_position.txt  transposed_joined.txt     withoutgroup.txt


mv Maize_hyphen.txt Maize_decreasing.txt/
[skrivog@nova SK_UNIX]$ cd Maize_decreasing.txt/
[skrivog@nova Maize_decreasing.txt]$ ls
Maize_hyphen.txt

(head -n 1 Maize_hyphen.txt && tail -n +2 Maize_hyphen.txt | sort -k3,3nr) > Maize_decreasing10.txt
[skrivog@nova Maize_decreasing.txt]$ ls
Maize_decreasing10.txt  Maize_hyphen.txt

Maize_decreasing10.txt  Maize_hyphen.txt
[skrivog@nova Maize_decreasing.txt]$ { head -n 1 Maize_decreasing10.txt && awk '$2 == 1' Maize_decreasing.txt; } > Maize_dchr1.txt
awk: fatal: cannot open file `Maize_decreasing.txt' for reading: No such file or directory
[skrivog@nova Maize_decreasing.txt]$ { head -n 1 Maize_decreasing10.txt && awk '$2 == 1' Maize_decreasing10.txt; } > Maize_dchr1.txt
[skrivog@nova Maize_decreasing.txt]$ { head -n 1 Maize_decreasing10.txt && awk '$2 == 2' Maize_decreasing10.txt; } > Maize_dchr2.txt
[skrivog@nova Maize_decreasing.txt]$ { head -n 1 Maize_decreasing10.txt && awk '$2 == 3' Maize_decreasing10.txt; } > Maize_dchr3.txt
[skrivog@nova Maize_decreasing.txt]$ { head -n 1 Maize_decreasing10.txt && awk '$2 == 4' Maize_decreasing10.txt; } > Maize_dchr4.txt
[skrivog@nova Maize_decreasing.txt]$ { head -n 1 Maize_decreasing10.txt && awk '$2 == 5' Maize_decreasing10.txt; } > Maize_dchr5.txt
[skrivog@nova Maize_decreasing.txt]$ { head -n 1 Maize_decreasing10.txt && awk '$2 == 6' Maize_decreasing10.txt; } > Maize_dchr6.txt
[skrivog@nova Maize_decreasing.txt]$ { head -n 1 Maize_decreasing10.txt && awk '$2 == 7' Maize_decreasing10.txt; } > Maize_dchr7.txt
[skrivog@nova Maize_decreasing.txt]$ { head -n 1 Maize_decreasing10.txt && awk '$2 == 8' Maize_decreasing10.txt; } > Maize_dchr8.txt
[skrivog@nova Maize_decreasing.txt]$ { head -n 1 Maize_decreasing10.txt && awk '$2 == 9' Maize_decreasing10.txt; } > Maize_dchr9.txt
[skrivog@nova Maize_decreasing.txt]$ { head -n 1 Maize_decreasing10.txt && awk '$2 == 10' Maize_decreasing10.txt; } > Maize_dchr10.txt

wc Maize_dchr1.txt Maize_dchr2.txt Maize_dchr3.txt Maize_dchr4.txt Maize_dchr5.txt Maize_dchr6.txt Maize_dchr7.txt Maize_dchr8.txt Maize_dchr9.txt Maize_dchr10.txt
156  245856  988210 Maize_dchr1.txt
    128  201728  811380 Maize_dchr2.txt
    108  170208  685104 Maize_dchr3.txt
     92  144992  584066 Maize_dchr4.txt
    123  193848  779831 Maize_dchr5.txt
     77  121352  489390 Maize_dchr6.txt
     98  154448  622008 Maize_dchr7.txt
     63   99288  400970 Maize_dchr8.txt
61  96136 388329 Maize_dchr9.txt
    54  85104 344187 Maize_dchr10.txt
   115 181240 732516 total

awk -F "\t" '{print NF; exit}' Maize_dchr1.txt Maize_dchr2.txt Maize_dchr3.txt Maize_dchr4.txt Maize_dchr5.txt Maize_dchr6.txt Maize_dchr7.txt Maize_dchr8.txt Maize_dchr9.txt Maize_dchr10.txt
1576

Here is my brief description of what this code does:

Creates a directory to hold files for the assignment. Joins the data files together to analyze, checking word count, columns and visualizing the files to check for accuracy. Directories were made for Maize increasing order and decreasing order. Question marks were changed to hyphens in the decreasing order maize file. Separate files were made for chromosomes 1 through 10 and are both in increasing and decreasing order. Multiple and unknown chromosomes are in increasing order. 


###Teosinte Data

```
here is my snippet of code used for data processing
```
awk '{for(i=1;i<=NF;i++) if ($i == "ZMPBA") print "Found at column " i}' transposed_joined.txt
Found at column 89
Found at column 988

awk '{for(i=1;i<=NF;i++) if ($i == "ZMPIL") print "Found at column " i}' transposed_joined.txt
Found at column 1178
Found at column 1218

awk '{for(i=1;i<=NF;i++) if ($i == "ZMPJA") print "Found at column " i}' transposed_joined.txt
Found at column 989
Found at column 1022

awk '{for(i=1;i<=NF;i++) if($i == "ZMPBA") count++}
END{print count}' transposed_joined.txt
900
awk '{for(i=1;i<=NF;i++) if($i == "ZMPIL") count++}
END{print count}' transposed_joined.txt
41
awk '{for(i=1;i<=NF;i++) if($i == "ZMPJA") count++}
END{print count}' transposed_joined.txt
34

cut -d $'\t' -f1,3,4,89-988,1178-1218,989-1022 transposed_joined.txt > teo.txt 

wc teo.txt
    984  962346 3861859 teo.txt

tail -n +6 teo.txt | awk -F "\t" '{print NF; exit}'
978

sort -k3,3n maize.txt > teo_sort.txt

wc teo_sort.txt
984 1550778 6216783 teo_sort.txt

tail -n +6 teo_sort.txt | awk -F "\t" '{print NF; exit}' 
1576

vim teo_sort.txt = to visualize the columns and rows to check

(head -n 1 teo.txt && tail -n +2 teo.txt | sort -k3,3n) > Teo_sorted.txt

wc Teo_sorted.txt
984  962346 3861859 Teo_sorted.txt

tail -n +6 Teo_sorted.txt | awk -F "\t" '{print NF; exit}'
978

grep -v "^#" Teo_sorted.txt | cut -f2 | sort | uniq -c
155 1
     53 10
    127 2
    107 3
     91 4
    122 5
     76 6
     97 7
     62 8
     60 9
      1 Chromosome
      6 multiple
     27 unknown

{ head -n 1 Teo_sorted.txt && awk '$2 == 1' Teo_sorted.txt; } > Teo_chr1.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Teo_sorted.txt && awk '$2 == 2' Teo_sorted.txt; } > Teo_chr2.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Teo_sorted.txt && awk '$2 == 3' Teo_sorted.txt; } > Teo_chr3.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Teo_sorted.txt && awk '$2 == 4' Teo_sorted.txt; } > Teo_chr4.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Teo_sorted.txt && awk '$2 == 5' Teo_sorted.txt; } > Teo_chr5.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Teo_sorted.txt && awk '$2 == 6' Teo_sorted.txt; } > Teo_chr6.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Teo_sorted.txt && awk '$2 == 7' Teo_sorted.txt; } > Teo_chr7.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Teo_sorted.txt && awk '$2 == 8' Teo_sorted.txt; } > Teo_chr8.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Teo_sorted.txt && awk '$2 == 9' Teo_sorted.txt; } > Teo_chr9.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Teo_sorted.txt && awk '$2 == 10' Teo_sorted.txt; } > Teo_chr10.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Teo_sorted.txt && awk '$2 == "multiple"' Teo_sorted.txt; } > Teo_multiple.txt
[skrivog@nova SK_UNIX]$ { head -n 1 Teo_sorted.txt && awk '$2 == "unknown"' Teo_sorted.txt; } > Teo_unknown.txt

wc Teo_chr1.txt Teo_chr2.txt Teo_chr3.txt Teo_chr4.txt Teo_chr5.txt Teo_chr6.txt Teo_chr7.txt Teo_chr8.txt Teo_chr9.txt Teo_chr10.txt Teo_multiple.txt Teo_unknown.txt
    156  152568  613862 Teo_chr1.txt
    128  125184  504008 Teo_chr2.txt
    108  105624  425572 Teo_chr3.txt
     92   89976  362806 Teo_chr4.txt
    123  120294  484419 Teo_chr5.txt
     77   75306  304010 Teo_chr6.txt
     98   95844  386396 Teo_chr7.txt
     63   61614  249078 Teo_chr8.txt
     61   59658  241221 Teo_chr9.txt
     54   52812  213823 Teo_chr10.txt
      7    6840   29405 Teo_multiple.txt
     28   27384  111906 Teo_unknown.txt
    995  973104 3926506 total

awk -F "\t" '{print NF; exit}' Teo_chr1.txt Teo_chr2.txt Teo_chr3.txt Teo_chr4.txt Teo_chr5.txt Teo_chr6.txt Teo_chr7.txt Teo_chr8.txt Teo_chr9.txt Teo_chr10.txt Teo_multiple.txt Teo_unknown.txt
978

sed 's/?/-/g' Teo_sorted.txt > Teo_hyphen.txt

wc Teo_hyphen.txt
984  962346 3861859 Teo_hyphen.txt

awk -F "\t" '{print NF; exit}' Teo_hyphen.txt
978

mkdir Teo_increasing.txt

mv Teo_chr1.txt Teo_chr2.txt Teo_chr3.txt Teo_chr4.txt Teo_chr5.txt Teo_chr6.txt Teo_chr7.txt Teo_chr8.txt Teo_chr9.txt Teo_chr10.tx
t Teo_multiple.txt Teo_unknown.txt Teo_increasing.txt/

cd Teo_increasing.txt/
[skrivog@nova Teo_increasing.txt]$ ls
Teo_chr10.txt  Teo_chr2.txt  Teo_chr4.txt  Teo_chr6.txt  Teo_chr8.txt  Teo_multiple.txt
Teo_chr1.txt   Teo_chr3.txt  Teo_chr5.txt  Teo_chr7.txt  Teo_chr9.txt  Teo_unknown.txt




mv Teo_hyphen.txt Teo_decreasing.txt/
[skrivog@nova SK_UNIX]$ cd Teo_decreasing.txt/

[skrivog@nova Teo_decreasing.txt]$ ls
Teo_hyphen.txt

(head -n 1 Teo_hyphen.txt && tail -n +2 Teo_hyphen.txt | sort -k3,3nr) > Teo_decreasing10.txt

[skrivog@nova Teo_decreasing.txt]$ { head -n 1 Teo_decreasing10.txt && awk '$2 == 1' Teo_decreasing10.txt; } > Teo_dchr1.txt
[skrivog@nova Teo_decreasing.txt]$ { head -n 1 Teo_decreasing10.txt && awk '$2 == 2' Teo_decreasing10.txt; } > Teo_dchr2.txt
[skrivog@nova Teo_decreasing.txt]$ { head -n 1 Teo_decreasing10.txt && awk '$2 == 3' Teo_decreasing10.txt; } > Teo_dchr3.txt
[skrivog@nova Teo_decreasing.txt]$ { head -n 1 Teo_decreasing10.txt && awk '$2 == 4' Teo_decreasing10.txt; } > Teo_dchr4.txt
[skrivog@nova Teo_decreasing.txt]$ { head -n 1 Teo_decreasing10.txt && awk '$2 == 5' Teo_decreasing10.txt; } > Teo_dchr5.txt
[skrivog@nova Teo_decreasing.txt]$ { head -n 1 Teo_decreasing10.txt && awk '$2 == 6' Teo_decreasing10.txt; } > Teo_dchr6.txt
[skrivog@nova Teo_decreasing.txt]$ { head -n 1 Teo_decreasing10.txt && awk '$2 == 7' Teo_decreasing10.txt; } > Teo_dchr7.txt
[skrivog@nova Teo_decreasing.txt]$ { head -n 1 Teo_decreasing10.txt && awk '$2 == 8' Teo_decreasing10.txt; } > Teo_dchr8.txt
[skrivog@nova Teo_decreasing.txt]$ { head -n 1 Teo_decreasing10.txt && awk '$2 == 9' Teo_decreasing10.txt; } > Teo_dchr9.txt
[skrivog@nova Teo_decreasing.txt]$ { head -n 1 Teo_decreasing10.txt && awk '$2 == 10' Teo_decreasing10.txt; } > Teo_dchr10.txt

wc Teo_dchr1.txt Teo_dchr2.txt Teo_dchr3.txt Teo_dchr4.txt Teo_dchr5.txt Teo_dchr6.txt Teo_dchr7.txt Teo_dchr8.txt Teo_dc
hr9.txt Teo_dchr10.txt
    156  152568  613862 Teo_dchr1.txt
    128  125184  504008 Teo_dchr2.txt
    108  105624  425572 Teo_dchr3.txt
     92   89976  362806 Teo_dchr4.txt
    123  120294  484419 Teo_dchr5.txt
     77   75306  304010 Teo_dchr6.txt
     98   95844  386396 Teo_dchr7.txt
     63   61614  249078 Teo_dchr8.txt
     61   59658  241221 Teo_dchr9.txt
     54   52812  213823 Teo_dchr10.txt
    960  938880 3785195 total

awk -F "\t" '{print NF; exit}' Teo_dchr1.txt Teo_dchr2.txt Teo_dchr3.txt Teo_dchr4.txt Teo_dchr5.txt Teo_dchr6.txt Teo_dchr7.txt Teo_dchr8.txt Teo_dchr9.txt Teo_dchr10.txt
978


Here is my brief description of what this code does
Similar to the maize, teosinte directories were made for both increasing and decreasing order. Within each, there are files for chromosomes 1 through 10. Word counts, columns and files were visualized using vim to check for accuracy. 



 

 
