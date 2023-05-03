# CTF: Juicy Details

Today we'll be tackling an easy blue team CTF on THM. We'll be manually parsing logs, which is a good thing because I can certainly use the practice. * To be completely transparent, I won't be showing screenshots of all the trial and error that I went through to find the perfect command while parsing these logs. 
<br>
<br>
The first question is to find the tools that the attacker used in the order of occurrence. First, I manually opened the access log to take a quick look and to get the lay of the land. I then used the "awk" command to parse the last few columns of the log and used “unique” to make it more readable. 
<br>
<br>
<img src="https://i.imgur.com/Djsk22z.jpg" title="source: imgur.com" />
<br>
<br>
Next, they want us to find which endpoint was vulnerable to the brute force attack. So, I searched any endpoint that was being brute forced by Hydra as well as looking at the status codes. 
<br>
<br>
<img src="https://i.imgur.com/nAJItwf.jpg" title="source: imgur.com" />
<br>
<br>
The next question was an easy one. Similar to the last question, they wanted to know which endpoint was vulnerable to sql injection, so I searched for sql, the endpoint column and the status code 200.  
<br>
<br>
<img src="https://i.imgur.com/YYoJT6c.jpg" title="source: imgur.com" />
<br>
<br>
Next, they wanted to know which parameter was used for the sql injection and if you search the log focusing on all the occurrences where sqlmap was used, you can easily see "search?q=" was used repeatedly. 
<br>
<br>
<img src="https://i.imgur.com/IEwA1gW.jpg" title="source: imgur.com" />
<br>
<br>
They then asked which endpoint the attacker used to attempt to exfiltrate data. I did a preliminary search with "awk '{print $7,$9}' access.log | sort | uniq -c | sort -rn " and found that they were attempting to use ftp. I then cleaned up the script just for the purpose of documenting via a screenshot.
<br>
<br>
<img src="https://i.imgur.com/IcZRUnE.jpg" title="source: imgur.com" />
<br>
<br>
After fumbling around for a bit, I used the hint option on the website. They wanted to know which section of the website the attacker used to scrape user email addresses. The answer being the product review section.  
<br>
<br>
*This was a tricky question, because aside from seeing that the attacker was viewing the review pages, there was no way to tell that they were actually scraping user email info. 
<br>
<br>
<img src="https://i.imgur.com/QUwEXeR.jpg" title="source: imgur.com" />
<br>
<br>
Now they would like to know if the attacker was able to successfully brute force the system and what was the time stamp? 
<br>
<br>
<img src="https://i.imgur.com/gWmv9br.jpg" title="source: imgur.com" />
<br>
<br>
Yikes! This question took me longer than it should have. By adding the keyword "sql" to the awk command it wasn't giving me the information that looked necessary to answer the question. I spent far too much time cutting and pasting strings into CyberChef to make the % encoding more readable. I finally removed the "sql" keyword and it revealed the right answer. 
<br>
<br>
<img src="https://i.imgur.com/ZgNngJH.jpg" title="source: imgur.com" />
<br>
<br>
Which files did the malicious actors try to download from the vulnerable endpoint? Here we just performed another search with the focus on the keyword "ftp" to search for the files. 
<br>
<br>
<img src="https://i.imgur.com/IcZRUnE.jpg" title="source: imgur.com" />
<br>
<br>
Up next, they would like to know what service and login was used to retrieve the files? Well, we already know that FTP was used, so I analyzed the vsftpd.log file and we can see that they eventually were authenticated with an "anonymous" login. 
<br>
<br>
<img src="https://i.imgur.com/GTz2qaG.jpg" title="source: imgur.com" />
<br>
<br>
For the last question they wanted to know which service and username were used to gain a shell? This was a quick answer to retrieve, I opened the "auth.log" and did a quick scan. Thankfully it wasn't a very large log, but I used an awk script to clean it up. 
<br>
<br>
<img src="https://i.imgur.com/cn01wbZ.jpg" title="source: imgur.com" />
<br>
<br>
And there you have it. I repeated a lot of search strings and would really like to get better utilizing awk, sed, cut and grep. Practice makes perfect. 



























