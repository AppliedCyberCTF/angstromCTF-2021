# angstromCTF 2021

* **Category:**  Misc
* **Points:** 50pts
* **Author:** [Movement](https://github.com/MovementGH)

## Challenge

>The archaeological team at ångstromCTF has uncovered an archive from over 100 years ago! Can you read the contents?
>
>Access the file at /problems/2021/archaic/archive.tar.gz on the shell server.

When we use `tar -xf archive.tar.gz` we get an error message saying that there is an unreasonable date for a file within. All we have to do to bypass this is add `--warning=no-timestamp` to our command.