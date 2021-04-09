# AngstromCTF 2021 - FREE FLAGS!!1!!

* **Category:** Rev
* **Points:** 50
* **Author:** [Wesley V/Retoxified](https://github.com/retoxified)

## Challenge
> Clam was browsing armstrongctf.com when suddenly a popup appeared saying "GET YOUR FREE FLAGS HERE!!!" along with [a download](free_flags). Can you fill out the survey for free flags?  
> Find it on the shell server at /problems/2021/free_flags or over netcat at nc shell.actf.co 21703.  

## Solution
>┌──(kali㉿kali)-[~]  
>└─$ nc shell.actf.co 21703  
>Congratulations! You are the 1000th CTFer!!! Fill out this short survey to get FREE FLAGS!!!  
>What number am I thinking of???  
>1000  
>Wrong >:((((  

Let's load the binary up in Ghidra to figure out what the number is that we need to enter. After hitting analyse and browsing to the main function the decompiler window shows the following code

```C
undefined4 main(void)  
{  
	int iVar1;  
	size_t sVar2;  
	long in_FS_OFFSET;  
	undefined4 local_128;  
	int local_124;  
	int local_120;  
	int local_11c;  
	char local_118 [264];  
	long local_10;  
  
	local_10 = *(long *)(in_FS_OFFSET + 0x28);  
	puts("Congratulations! You are the 1000th CTFer!!! Fill out this short survey to get FREE FLAGS!!!");  
	puts("What number am I thinking of???");  
	__isoc99_scanf("%d",&local_11c);  
	if (local_11c == 0x7a69)   
	{  
		puts("What two numbers am I thinking of???");  
		__isoc99_scanf("%d %d",&local_120,&local_124);  
		if ((local_120 + local_124 == 0x476) && (local_120 * local_124 == 0x49f59))   
		{  
			puts("What animal am I thinking of???");  
			__isoc99_scanf(" %256s",local_118);  
			sVar2 = strcspn(local_118,"\n");  
			local_118[sVar2] = '\0';  
			iVar1 = strcmp(local_118,"banana");  
			if (iVar1 == 0)  
			{  
				puts("Wow!!! Now I can sell your information to the Russian government!!!");  
				puts("Oh yeah, here\'s the FREE FLAG:");  
				print_flag();  
				local_128 = 0;  
			}  
			else   
			{  
				puts("Wrong >:((((");  
				local_128 = 1;  
			}  
		}  
		else  
		{  
			puts("Wrong >:((((");  
			local_128 = 1;  
		}  
	}  
	else   
	{  
		puts("Wrong >:((((");  
		local_128 = 1;  
	}  
	if (*(long *)(in_FS_OFFSET + 0x28) == local_10)   
	{  
		return local_128;  
	}  
	/* WARNING: Subroutine does not return */  
	__stack_chk_fail();  
}
```

Luckily for us, it seems all of the checks are happening directly here in the main method!  
Our first piece of input is stored in local_11c by a call to __isoc99_scanf, this is then compared to 0x7a69, in decimal form this is 31337  
Looking further into main we'll see that the next challenge will be to enter 2 numbers(again through __isoc99_scanf) that sum up to 0x476(1142) and multiply to 0x49f59(302937)  
We can easily look up the factor pairs of 302937 online;  
1, 302937  
3, 100979  
241, 1257  
419, 723  
Out of these factors 419 and 723 sum up to 1142, so that would be the answer to the second question.  
Now the final question is "What animal am I thinking of???", and the answer is compared to "banana"(what a weird animal that is)  
Entering these values should give us the flag!  

>┌──(kali㉿kali)-[~]  
>└─$ nc shell.actf.co 21703  
>Congratulations! You are the 1000th CTFer!!! Fill out this short survey to get FREE FLAGS!!!  
>What number am I thinking of???  
>31337  
>What two numbers am I thinking of???  
>419 723  
>What animal am I thinking of???  
>banana  
>Wow!!! Now I can sell your information to the Russian government!!!  
>Oh yeah, here's the FREE FLAG:  
>actf{what_do_you_mean_bananas_arent_animals}  

Success!

```
FLAG: actf{what_do_you_mean_bananas_arent_animals}
```
