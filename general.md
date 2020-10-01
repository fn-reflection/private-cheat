## void** in cuda
https://devtalk.nvidia.com/default/topic/418460/-void-amp-d-why-two-/  
https://www.slideshare.net/KMC_JP/c-91154309  
https://www.slideshare.net/KMC_JP/ss-45855264  
https://www.slideshare.net/KMC_JP/gpgpu-91122680  

## about cython
https://nor-isio.hateblo.jp/entry/2018/04/21/145816  
## postgres
|desc|code|
|:-|:-|
|create database |create database japanstock encoding 'UTF-8'; |
|listing databases|\l|
|listing users|\du|
|table definition|\d+ tablename|
|where is data dir|show data_directory;|
|relate to logging|show log_directory; show logging_collector;show log_destination; show log_statement;|
## sqlite
|||
|:-|:-|
|data types |{NULL,INTEGER,REAL,TEXT,BLOB} | 
|column types |{[INTEGER,int],[REAL,*{real,floa,doub}*],[TEXT,*#{char,clob,text}*],NUMERIC,[NONE,*blob*]}|
|query tables | .tables|
|query schemas | .schema {{or}} select * from sqlite_master;|
|rename table| alter table aaa rename to bbb;|
|add column|alter table aaa add column coladd;|
|delete table|drop table aaa; VACUUM;|
|constraints|{PRIMARY KEY, AUTOINCREMENT,NOT NULL,CHECK,DEFAULT}|
|keyword for default|CURRENT_TIME, CURRENT_DATE, CURRENT_TIMESTAMP|
|auto increment max rowid| select * from sqlite_sequence|
|view|create view myview as select ... ,drop view a;|
|special var|ROWID|
## powershell vs python

|description| powershell | python |
|:-|:-|:-|
|extension |.ps1 |.py|
|IDE| powershell_ise|pycharm etc.|
|help|Get-help func|func? func??|
|one-line comment|#| #|
|multi-line comment|<# {{commentlines}} #> |""" {{commentlines}} """|
|output|echo x|print (x)|
|piping|write-output > {{or}} echo >||
|Tostring|write-host||
|exec|.\{{{scriptname}}|||
|type designate|[double] [int[]] ||
|type casting|[string]$x $x as [string]|str(10)|
|type is|-is -isnot||
|get type|$x.gettype()|type(x)|
|get object properties| $obj &#124; gm| |
|control statement|foreach( in ) if(){}elseif(){}else{} for(a;b;c) while(){}|for in while if|
|.NET|$re =new-object -TypeName "System.Text.RegularExpressions.Regex" -ArgumentList "\.jpg$"||
|COM object|$excel = new-object -comobject Excel.Application||
|listing command|Get-Command ||
|call static function|[Math]::sqrt($x)|import math;math.sqrt(x)|
|compare|-lt -gt -le -ge -ne -eq -like -match -notmatch|< > <= >= != ==|
|define function|function global:a([double] x){}|def a(x):|
|natural numbers|0,1,2 1..10|[0,1,2] range(1,10)|
|create array|$arr = 0,2,4,6 {{or}} @(0,2,4,6)|[0,2,4,6]
|indexing|$arr[1] $arr[-1]|arr[1] arr[-1]|
|index slicing| $arr[9..19],$arr[0..3+7] |arr[9:20]
|bool slicing| $arr -lt 5 | arr[arr<5]
|existence of member|$x -contains 5| 5 in x|
|list push| $arr += 42 | lst.add(42)
|list append|$arr1+$arr2 | arr1+arr2|
|list delete|$arr[1]=$null |del lst[1]
|dict|@{"dog"="a";"cat"="b"}|{"dog":"a","cat":"b"}|
|shorthands|%:foreach-object ?:where-object||
|remove nil|$obj &#124; select-object|||
|regex|if ("abcdef" -match "bc(de)") {$Matches}|||
|regex naming capture|if ("abcdef" -match "bc(?<xxx>de)") {$Matches}|||
|script block|(1..3) &#124; % {if ($\_ -gt 2) {return} $_}||
|string|string -split delimiter string -join delimiter||
|eval|Invoke-Expression "Get-Date" {{or}} & "Get-Date"||
|lexical scope lambda|& { $foo++; $foo }|||
|closure|GetNewClosure()||
|apply|func @args|||

sample script  
`get-childitem C:\scripts -recurse | measure-object`
`get-childitem C:\scripts -name > filelist.txt`

---
## browser automation
[headless chrome summary](http://vaaaaaanquish.hatenablog.com/entry/2017/06/06/194546)

---
## CSS XPath
[css selectors summary](http://saucelabs.com/resources/articles/selenium-tips-css-selectors)

|description| xpath | css |
|:-|:-|:-|
| all element|//* | *|
| all `<p>` element|//p | p|
|ID|//div[@id ='pwd']| #pwd |
|class|//div[@class = 'boot'] | .boot|
| child or subchild|//div//a | div a |
| direct child|//div/a|div>a |
|first child|//p/*[0]|p > *:first-child |
|next sibling|//input[@id='username']/following-sibling::input[1]|#username + input|
|attribute value|//input[@name='username']|input[name='username']|
|and or not|//input[@name='login' and @type='submit']|input[name='login'][type='submit'] |
|fourth element of li|//li[4]|li:nth-of-type(4)|
|fourth element of * |//*[4]| *:nth-child(4)| 
|slicing|//li[position()>1]||
|match with prefix||a[id^='pre']|
|match with suffix||a[id$='post']|
|match with substring|//img[contains(@id, 'pattern')]|a[id*='pattern']|
|match without specifying attribute||a:contains('Log Out')|

`%%prun -s "cumulative"`  
`%lprun -f loadcsvpd loadcsvpd(file)`
