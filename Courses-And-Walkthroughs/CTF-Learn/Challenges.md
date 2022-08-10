# Challenges

These are the different challenges, and how I beat them.


## [Basic Injection](https://ctflearn.com/challenge/88)

Had a link to a vuln site with a SQL Injection vuln. Didn't quite remember how to do it, so did the [SQL Injection Lab](https://ctflearn.com/lab/sql-injection-part-1#). This taught me that the answer was quite simple. The query wasn't sanitized, so if the original query was `SELECT * FROM webfour.webfour where name = '$input'`, I could input `a or '1' = '1` which would make the query `SELECT * FROM webfour.webfour where name = 'a' or '1' = '1'`.

Output: 

```
Name: Luke
Data: I made this problem.
Name: Alec
Data: Steam boys.
Name: Jalen
Data: Pump that iron fool.
Name: Eric
Data: I make cars.
Name: Sam
Data: Thinks he knows SQL.
Name: fl4g__giv3r
Data: CTFlearn{th4t_is_why_you_n33d_to_sanitiz3_inputs}
Name: snoutpop
Data: jowls
Name: Chunbucket
Data: @datboiiii
```

## [Forensic 101](https://ctflearn.com/challenge/96)

Think the flag is somewhere in there. Would you help me find it? https://mega.nz/#!OHohCbTa!wbg60PARf4u6E6juuvK9-aDRe_bgEL937VO01EImM7c

So the flag is somewhere in the picture. 

Used the site [https://29a.ch/photo-forensics/#strings](https://29a.ch/photo-forensics/#strings) to extract the flag which was in a string format.

Flag: `flag{wow!_data_is_cool}`

## [Character Encoding](https://ctflearn.com/challenge/115)


In the computing industry, standards are established to facilitate information interchanges among American coders. Unfortunately, I've made communication a little bit more difficult. Can you figure this one out? 41 42 43 54 46 7B 34 35 43 31 31 5F 31 35 5F 55 35 33 46 55 4C 7D

Hext to text, used this [site](https://string-functions.com/hex-string.aspx))

Flag: `ABCTF{45C11_15_U53FUL}`


## [Taking LS](https://ctflearn.com/challenge/103)

Just take the Ls. Check out this zip file and I be the flag will remain hidden. https://mega.nz/#!mCgBjZgB!_FtmAm8s_mpsHr7KWv8GYUzhbThNn0I8cHMBi4fJQp8