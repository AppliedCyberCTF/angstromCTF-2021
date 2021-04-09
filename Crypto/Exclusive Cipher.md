# angstromCTF 2021

* **Category:**  Crypto
* **Points:** 40pts
* **Author:** [Movement](https://github.com/MovementGH)

## Challenge

>Clam decided to return to classic cryptography and revisit the XOR cipher! Here's some hex encoded ciphertext:
>
>`ae27eb3a148c3cf031079921ea3315cd27eb7d02882bf724169921eb3a469920e07d0b883bf63c018869a5090e8868e331078a68ec2e468c2bf13b1d9a20ea0208882de12e398c2df60211852deb021f823dda35079b2dda25099f35ab7d218227e17d0a982bee7d098368f13503cd27f135039f68e62f1f9d3cea7c`
>The key is 5 bytes long and the flag is somewhere in the message.

This challenge says that it used a 5 byte long key to xor the flag. 
Given the result `c` and input `a` of the statement `c=a^b`, we can find `b` by doing `b=c^a`. We are given the result of the XOR, and we know that somewhere in the flag there is `actf{`, which happens to be 5 bytes long. Given this information, we can write a script which XORs the input to put `actf{` at each index of the cipher, and see which gives us a legible flag:

```js
let Text='ae27eb3a148c3cf031079921ea3315cd27eb7d02882bf724169921eb3a469920e07d0b883bf63c018869a5090e8868e331078a68ec2e468c2bf13b1d9a20ea0208882de12e398c2df60211852deb021f823dda35079b2dda25099f35ab7d218227e17d0a982bee7d098368f13503cd27f135039f68e62f1f9d3cea7c';

let buffer=Buffer.from(Text,'hex');
let known=['a','c','t','f','{'].map(k=>k.charCodeAt(0));
for(let i=0;i<buffer.length-5;i++) {
    key=[];
    part=[...buffer.slice(i,i+5)];
    for(let i2=0;i2<5;i2++)
        key.push(part[i2]^known[i2]);
    let result="";
    for(let i2=0;i2<buffer.length;i2++)
        result+=(String.fromCharCode(buffer[i2]^key[i2%5]));
    console.log(result);
}
```

This script will give us a lot of random garbage, but one of the output lines will be:
`Congratulations on decrypting the message! The flag is actf{who_needs_aes_when_you_have_xor}. Good luck on the other crypto!`
This is our flag.