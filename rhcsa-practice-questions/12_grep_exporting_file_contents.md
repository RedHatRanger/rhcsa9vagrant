***On Node1***



### QUESTION #12:
Find all strings "ich" from "/usr/share/dict/words" file and copy those strings in a /root/lines file. 

***
(scroll down for an answer)

<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>
<br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/><br/>

### ANSWER #12:
* The ```find``` command provides what is needed:
```
[root@node1 ~]# grep ich /usr/share/dict/words > /root/lines
```

SUCCESS!!
