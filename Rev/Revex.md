# angstromCTF 2021

* **Category:**  Rev
* **Points:** 75pts
* **Author:** [Movement](https://github.com/MovementGH)

## Challenge

>As an active reddit user, clam frequently browses r/ProgrammerHumor. However, the reposts about how hard regex is makes him go >:((((. So, clam decided to show them who's boss.
>
>`^(?=.*re)(?=.{21}[^_]{4}\}$)(?=.{14}b[^_]{2})(?=.{8}[C-L])(?=.{8}[B-F])(?=.{8}[^B-DF])(?=.{7}G(?<pepega>..).{7}t\k<pepega>)(?=.*u[^z].$)(?=.{11}(?<pepeega>[13])s.{2}(?!\k<pepeega>)[13]s)(?=.*_.{2}_)(?=actf\{)(?=.{21}[p-t])(?=.*1.*3)(?=.{20}(?=.*u)(?=.*y)(?=.*z)(?=.*q)(?=.*_))(?=.*Ex)`

We are given a regular expression. We will assume that only the proper flag will match this regular expression. If we look at the expression we see it is made up of a lot of non-capturing groups. This means that each set of parenthesis is a unique rule which has no effect on others. If we start with an empty string and fill in characters from what we can deduce from each rule, we will get the flag. If helpful, you can write a script to print all rules that your string does not pass:

```js

let String="                          "

let Tests=[
    /(?=.*re)/,
    /(?=.{21}[^_]{4}\}$)/,
    /(?=.{14}b[^_]{2})/,
    /(?=.{8}[C-L])/,
    /(?=.{8}[B-F])/,
    /(?=.{8}[^B-DF])/,
    /(?=.{7}G(?<pepega>..).{7}t\k<pepega>)/,
    /(?=.*u[^z].$)/,
    /(?=.{11}(?<pepeega>[13])s.{2}(?!\k<pepeega>)[13]s)/,
    /(?=.*_.{2}_)/,
    /(?=actf\{)/,
    /(?=.{21}[p-t])/,
    /(?=.*1.*3)/,
    /(?=.{20}(?=.*u)(?=.*y)(?=.*z)(?=.*q)(?=.*_))/,
    /(?=.*Ex)/,
]

for(let Test of Tests)
    if(!String.match(Test))
        console.log(Test)
```