# angstromCTF 2021

* **Category:**  Crypto
* **Points:** 100pts
* **Author:** [Movement](https://github.com/MovementGH)

## Challenge

>Aplet's quirky and unique so he made my own PRNG! It's not like the other PRNGs, its absolutely unbreakable!
>
> nc crypto.2021.chall.actf.co 21600

If we netcat to the challenge, it gives us two options: we can view a random number (up to 3 times), or try to guess the next random number.

We are given the source code for this challenge, and we can see that each random number it generates is used as the seed for the next random number.

There is a catch though. There are two random numbers being generated, and they are multiplied together.

```py
r1 = Generator(random.randint(10000000, 99999999))
r2 = Generator(random.randint(10000000, 99999999))
```
From this code we know that each seed will be between `10000000` and `99999999`. Using this information we can brute force the possible seeds which could create the large number. If `n1` and `n2` are the numbers, and `N` is the large number, `n1*n2=N`. If we define `n1` as the smaller of the two seeds, we know that it will be in the range `10000000<=n1<=sqrt(N)`.

We can then check each number in that range to see if N is divisible by it. If so, it is a possible seed. Once we do this, we will have a list of 5-10 seeds which may have been used. If we generate a second random number, we can go through the same process and see which of our previous seeds also work for the second number. This will reduce to one possible seed, which we can then use to predict all following numbers.

The challenge requires us to predict more than one number, so we print 3. Generator is the Generator class defined in the source we are given.

```py
r1 = Generator(random.randint(10000000, 99999999))
r2 = Generator(random.randint(10000000, 99999999))

def getPossibilities(big):
    poss=list()
    max=int(math.sqrt(big))
    for i in range(1,max):
        if(big%i==0 and big/i>max and big/i<100000000):
            poss.append((i,big/i))
    return poss

def attemptSolve(big,possibilities):
    for poss in possibilities:
        r1 = Generator(poss[0])
        r2 = Generator(poss[1])
        if r1.getNum()*r2.getNum()==big:
            return (r1,r2)

print("Enter first number:")
Big1=int(input())
Possibilities=getPossibilities(Big1)
print("Enter second number:")
Big2=int(input())
Gen=attemptSolve(Big2,Possibilities)
print("Next:")
print(Gen[0].getNum()*Gen[1].getNum())
print(Gen[0].getNum()*Gen[1].getNum())
print(Gen[0].getNum()*Gen[1].getNum())
```