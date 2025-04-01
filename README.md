# ee355_prelab_regex

Name: Diego Mendez

Email: diegomen@usc.edu

USC ID: 9333933481


ReadMe.txt
==========

This lab focused on using regular expressions and basic Unix commands to solve two problems related to text processing and IP address validation.

-------------------------------------------------------------------------------
Problem I: IP Address Validation
-------------------------------------------------------------------------------

Goal:
-----
Validate a list of IP addresses in a file named `ip.txt`, and output only the valid ones to `output_p1.txt`.

IPv4:
- Must be in the format X.X.X.X, where X is between 0 and 255
- No leading zeros allowed (e.g., 192.168.01.1 is invalid)

IPv6:
- Must be 8 groups of 1–4 hexadecimal digits separated by colons
- No leading zeros longer than 4 digits
- No use of "::" (compressed zero notation)

Commands Used:
--------------

1. First, we created the input file manually with this command:

cat > ip.txt << EOF
172.16.254.1
192.168.1.1
255.255.255.255
256.100.100.100
192.168.01.1
172.16.254.01
2001:0db8:85a3:0000:0000:8a2e:0370:7334
2001:db8:85a3:0:0:8A2E:0370:7334
2001:0db8:85a3::8A2E:0370:7334
02001:0db8:85a3:0000:0000:8a2e:0370:7334
EOF

2. Then we ran the following command to validate IPv4 addresses:

grep -E '^((25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])\.){3}(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])$' ip.txt > output_p1.txt

And this command to validate IPv6 addresses:

grep -E '^([0-9a-fA-F]{1,4}:){7}[0-9a-fA-F]{1,4}$' ip.txt >> output_p1.txt
We then checked the results:

cat output_p1.txt
Output should include:

172.16.254.1
192.168.1.1
255.255.255.255
2001:0db8:85a3:0000:0000:8a2e:0370:7334
2001:db8:85a3:0:0:8A2E:0370:7334


Troubleshooting:
Initially got no output due to overly strict regex.

Confirmed with a simpler test pattern.

Used manual file creation to ensure test inputs were correct.

Problem II: Word Frequency Analysis
Goal:
Count the frequency of each word in words.txt (case-insensitive), ignore common stopwords (the, a, an, is), and output results in lowercase sorted by word to output_p2.txt.

Test Input Created:

cat > words.txt << EOF
The day is sunny the the the Sunny is is
A quick brown fox jumps over the lazy dog
An example sentence is provided to test the word frequency
quick Quick FOX fox
EOF

Command Used:

tr '[:upper:]' '[:lower:]' < words.txt | \
tr -s ' ' '\n' | \
grep -v -w -E 'the|a|an|is' | \
sort | \
uniq -c | \
awk '{print $2, $1}' > output_p2.txt


Explanation:

tr '[:upper:]' '[:lower:]': converts all text to lowercase

tr -s ' ' '\n': splits words onto new lines

grep -v -w -E 'the|a|an|is': filters out stopwords

sort: groups identical words together

uniq -c: counts occurrences

awk '{print $2, $1}': reorders output to word count



Expected Output (output_p2.txt):

brown 1
day 1
dog 1
example 1
fox 3
frequency 1
jumps 1
lazy 1
over 1
provided 1
quick 3
sentence 1
sunny 2
test 1
to 1
word 1
