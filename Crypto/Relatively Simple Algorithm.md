# angstromCTF 2021

* **Category:**  Crypto
* **Points:** 40pts
* **Author:** [Vivian](https://github.com/vivian-dai)

## Challenge

> RSA strikes strikes again again! Source

We're given `n`, `p` `q`, `e`, and `c` which is enough to solve RSA. We can calculate phi like [this](https://www.calculator.net/big-number-calculator.html?cx=11556895667671057477200219387242513875610589005594481832449286005570409920461121505578566298354611080750154513073654150580136639937876904687126793459819368&cy=9789731420840260962289569924638041579833494812169162102854947552459243338614590024836083625245719375467053459789947717068410632082598060778090631475194566&cp=20&co=multiple) using `(p - 1)(q - 1)` then run the Java code provided [here](https://crypto.stackexchange.com/questions/19915/rsa-decryption-given-n-e-and-phin) to give us the flag.
