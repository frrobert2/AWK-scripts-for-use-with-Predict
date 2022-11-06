# AWK-scripts-for-use-with-Predict

---
Title: Using Predict with Bash Scripts
Published: 2022-11-04 15:29
Author: Rev. Fr. Robert Bower
Tag: 
layout: blog
bibliography:
---

#
## Date command to take date and time in UTC to epoch

```
date -d "11/03/22 23:00" +"%s"
```

## Predict command to get ISS cordinates between two Epoch times

```
predict -f ISS 1667601636 1667864436 -o iss.txt

predict -f ISS 1667587236 1667883599 -o | awk  '$5 >=25 {print }' > iss.txt
```

## Awk Command to strip negative elevations

```
awk  '$5 !~ /^-/ {print }' iss.txt
```

## Awk command to set minimum elevations

```
awk  '$5 >=45 {print }' iss.txt
```

## Awk commands to get start and stop times for iss.txt

Examples of how to trim to start and stop times

```
awk '$1-100>prev {print prev, two, three, four, five "\n" $1, $2, $3, $4, $5}{prev=$1}{two=$2}{three=$3}{four=$4}{five=$5}' iss.txt >iss2.txt

awk '$5>=29 && $1-100>prev {print prev, two, three, four, five, six, seven, eight, nine, ten, eleven, twelve "\n" $1,$2,$3,$4,$5,$6,$7,$8,$9,$10,$11,$12}{prev=$1}{two=$2}{three=$3}{four=$4}{five=$5}{six=$6}{seven=$7}{eight=$8}{nine=$9}{ten=$10}{eleven=$11}{twelve=$12}' iss.txt > iss2.txt
```
Once iss2.txt is created you need to add the last line from iss.txt
```
awk 'END{print $1, $2, $3, $4, $5}' iss.txt >> iss2.txt
```

Examples on how to add pass time

```
awk '$1-prev<1000 && $1-prev!=0 {print $1-prev,prev, two, three, four, five, six, seven, eight, nine, ten, eleven, $1,$2,$3,$4,$5,$6,$7,$8,$9,$10,$11}{prev=$1}{two=$2}{three=$3}{four=$4}{five=$5}{six=$6}{seven=$7}{eight=$8}{nine=$9}{ten=$10}{eleven=$11}{twelve=$12}' iss2.txt | awk '{ printf " %d %3.0F\n", $2, $1/60+1}' > iss3.txt

awk '$1-prev<1000 && $1-prev!=0 {print $1-prev,prev, two, three, four, five, six, seven, eight, nine, ten, eleven, $1,$2,$3,$4,$5,$6,$7,$8,$9,$10,$11}{prev=$1}{two=$2}{three=$3}{four=$4}{five=$5}{six=$6}{seven=$7}{eight=$8}{nine=$9}{ten=$10}{eleven=$11}{twelve=$12}' iss2.txt

awk '{ print $2, strftime("%H:%M %D",$1) }' iss3.txt
```

Examples on how to set command file for issbeacon

```
awk '{ print "echo /frrobert/bin/issbeacon " $2 " | at " strftime("%H:%M %D",$1) }' iss3.txt > iss4.txt
```

Zettelkasten ID **satellitescriptnotes-2022-11-04-1529** 
